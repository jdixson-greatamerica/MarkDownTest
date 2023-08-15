#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_Facility_CT76_DeferredDeferredGAFV

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_Facility_CT76_DeferredDeferredGAFV]

---

## <a name="#properties"></a>Properties

| Property | Value |
|---|---|
| ANSI Nulls On | NO |
| Quoted Identifier On | NO |


---

## <a name="#parameters"></a>Parameters

| Name | Data Type | Max Length (Bytes) |
|---|---|---|
| @FacilityID | int | 4 |
| @UserID | nvarchar(255) | 510 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
SET QUOTED_IDENTIFIER OFF
GO
SET ANSI_NULLS OFF
GO
-- History:
--		Name:	Chad Feldmann
--		Date:	4/18/2019
--		Desc:	Ticket #PRJ0040256 - Update concentration to include BillingFrequency and ProgramTypeID
-- 
-- History:
--		Name:	John Thadison
--		Date:	02/23/2022
--		Desc:	STRY0018934  - Retire AdvanceRate logic from concentration tests.
--
--		Name:	John Thadison
--		Date:	03/08/2022
--		Desc:	STRY0017794 - New logic that outputs percentage of contracts within the concentration pool that meet business rule inputs for contract types and durations.
-- =============================================
CREATE PROCEDURE [Securitization].[sp_Facility_CT76_DeferredDeferredGAFV]
    @FacilityID [INT],
    @UserID [NVARCHAR](255)
AS
DECLARE @ConcentrationTest_ID INT;
DECLARE @FacilityLimit_ID INT;
DECLARE @FacilityLimitPcnt DECIMAL(20, 8);
DECLARE @FacilityLimitType INT;
DECLARE @FacilityLimitAmount DECIMAL(20, 8);
DECLARE @Facility_ConcentrationNPVTotal DECIMAL(20, 8);
DECLARE @Facility_ConcentrationPercentage DECIMAL(20, 8);
DECLARE @TolerancePcnt DECIMAL(20, 8);
DECLARE @ToleranceThresholdPcnt DECIMAL(20, 8);
DECLARE @ToleranceThresholdAmount DECIMAL(20, 8);
DECLARE @Facility_NPVTotal DECIMAL(20, 8);
DECLARE @FacilityName NVARCHAR(50);

SELECT @ConcentrationTest_ID = 76;

SELECT @FacilityName = f.Name
FROM Securitization.Facilities f
    INNER JOIN Securitization.FacilityLessor fl
        ON f.id = fl.Facility_ID
WHERE f.id = @FacilityID;
SELECT @FacilityLimit_ID = fl.ID,
       @FacilityLimitPcnt = fl.Percentage,
       @FacilityLimitAmount = fl.Amount,
       @FacilityLimitType = [Type]
FROM [Securitization].[FacilityLimits] fl
WHERE ConcentrationTest_ID = @ConcentrationTest_ID
      AND Facility_ID = @FacilityID;

IF @FacilityLimit_ID > 0
BEGIN


    SELECT @TolerancePcnt = Tolerance
    FROM [Securitization].[ConcentrationTests]
    WHERE ID = @ConcentrationTest_ID;
    SELECT @Facility_NPVTotal = SUM(COALESCE(CurrentMonthNPV, 0))
    FROM Securitization.Contracts c
    WHERE Lessor IN
          (
              SELECT fl.LessorID
              FROM Securitization.Facilities f
                  INNER JOIN Securitization.FacilityLessor fl
                      ON f.id = fl.Facility_ID
              WHERE f.id = @FacilityID
          );
    SET @Facility_NPVTotal = COALESCE(@Facility_NPVTotal, 0);

    DECLARE @RankPosition INT;
    SELECT @RankPosition = RankPosition
    FROM Securitization.ConcentrationTests
    WHERE id = @ConcentrationTest_ID;

    SELECT @Facility_ConcentrationNPVTotal = SUM(COALESCE(c.CurrentMonthNPV, 0))
    FROM [Securitization].[Contracts] c
    WHERE c.Lessor IN
          (
              SELECT fl.LessorID
              FROM Securitization.Facilities f
                  INNER JOIN Securitization.FacilityLessor fl
                      ON f.id = fl.Facility_ID
              WHERE f.id = @FacilityID
          )
          AND
          (
              [DaysDeferred] > 0
              AND NOT ( --Compare DaysDeferred to the more recent date
              IIF([BookingDate] > [CommenceDate],
                  DATEDIFF(DAY, [BookingDate], GETDATE()),
                  DATEDIFF(DAY, [CommenceDate], GETDATE())) > DaysDeferred
                      )
              AND NOT (RTRIM([BillingFrequency]) IN ( 'Quarterly', 'Annual', 'Semi-Annual' ))
              AND NOT ([ProgramTypeID] = 10001)
          );

    SET @Facility_ConcentrationNPVTotal = COALESCE(@Facility_ConcentrationNPVTotal, 0);
    --Calculate the current concentration percentage
    SET @Facility_ConcentrationPercentage = 0;
    IF (@Facility_NPVTotal <> 0)
    BEGIN
        SET @Facility_ConcentrationPercentage
            = CAST((@Facility_ConcentrationNPVTotal / @Facility_NPVTotal) AS DECIMAL(20, 8));
    END;

    IF (@FacilityLimitType = 1) --Either % or $ is greater
    BEGIN
        DECLARE @AmountPcntEquivalent DECIMAL(20, 8);
        IF (@Facility_NPVTotal <> 0)
        BEGIN
            SET @AmountPcntEquivalent = CAST((@FacilityLimitAmount / @Facility_NPVTotal) AS DECIMAL(20, 8));
        END;
        ELSE
        BEGIN
            SET @AmountPcntEquivalent = 0;
        END;

        IF (@AmountPcntEquivalent > @FacilityLimitPcnt)
        BEGIN
            SET @FacilityLimitType = 2; --Override to use the $ amount
        END;
        ELSE
        BEGIN
            SET @FacilityLimitType = 0; --Override to use the %
        END;
    END;

    --Calculate the Tolerance percent threshold
    IF (@FacilityLimitType = 0)
    BEGIN
        --Calculate the Tolerance percent threshold
        SET @ToleranceThresholdPcnt = @FacilityLimitPcnt * @TolerancePcnt;

        INSERT INTO [Securitization].[FacilityConcentration]
        (
            [Concentration],
            [ConcentrationStatus],
            [NPV],
            [CreatedOn],
            [CreatedBy],
            [ModifiedOn],
            [ModifiedBy],
            [Facility_ID],
            [FacilityLimit_Type],
            [ConcentrationTest_ID]
        )
        VALUES
        (   @Facility_ConcentrationPercentage,
            CASE
                WHEN @Facility_ConcentrationPercentage <= @ToleranceThresholdPcnt THEN
                    0 --Green
                WHEN @Facility_ConcentrationPercentage > @ToleranceThresholdPcnt
                     AND @Facility_ConcentrationPercentage <= @FacilityLimitPcnt THEN
                    1 --Yellow
                ELSE
                    2 --Red
            END, @Facility_ConcentrationNPVTotal, GETDATE(), @UserID, GETDATE(), @UserID, @FacilityID,
            @FacilityLimitType, @ConcentrationTest_ID);

    END;
    ELSE
    BEGIN
        --Calculate the Tolerance percent threshold
        SET @ToleranceThresholdAmount = @FacilityLimitAmount * @TolerancePcnt;

        INSERT INTO [Securitization].[FacilityConcentration]
        (
            [Concentration],
            [ConcentrationStatus],
            [NPV],
            [CreatedOn],
            [CreatedBy],
            [ModifiedOn],
            [ModifiedBy],
            [Facility_ID],
            [FacilityLimit_Type],
            [ConcentrationTest_ID]
        )
        VALUES
        (   @Facility_ConcentrationPercentage,
            CASE
                WHEN @Facility_ConcentrationNPVTotal <= @ToleranceThresholdAmount THEN
                    0 --Green
                WHEN @Facility_ConcentrationNPVTotal > @ToleranceThresholdAmount
                     AND @Facility_ConcentrationNPVTotal <= @FacilityLimitAmount THEN
                    1 --Yellow
                ELSE
                    2 --Red
            END, @Facility_ConcentrationNPVTotal, GETDATE(), @UserID, GETDATE(), @UserID, @FacilityID,
            @FacilityLimitType, @ConcentrationTest_ID);

    END;
END;


GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]
* [Securitization].[ConcentrationTests]
* [Securitization].[Contracts]
* [Securitization].[Facilities]
* [Securitization].[FacilityConcentration]
* [Securitization].[FacilityLessor]
* [Securitization].[FacilityLimits]


---

## <a name="#usedby"></a>Used By

* [[Securitization].[SP_Facility_CT_All]](Securitization_SP_Facility_CT_All.md)


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

