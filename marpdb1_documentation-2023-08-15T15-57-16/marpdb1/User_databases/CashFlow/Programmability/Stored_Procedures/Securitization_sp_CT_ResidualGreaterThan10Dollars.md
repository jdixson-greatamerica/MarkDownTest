#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_CT_ResidualGreaterThan10Dollars

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_CT_ResidualGreaterThan10Dollars]

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
| @PoolID | int | 4 |
| @UserID | nvarchar(255) | 510 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
SET QUOTED_IDENTIFIER OFF
GO
SET ANSI_NULLS OFF
GO
-- History:
--		Name:	John Thadison
--		Date:	02/23/2022
--		Desc:	STRY0018934  - Retire AdvanceRate logic from concentration tests.
-- =============================================
CREATE PROCEDURE [Securitization].[sp_CT_ResidualGreaterThan10Dollars]
    @PoolID [INT],
    @UserID [NVARCHAR](255)
AS
DECLARE @ConcentrationTest_ID INT;
DECLARE @FacilityLimit_ID INT;
DECLARE @FacilityLimitPcnt DECIMAL(20, 8);
DECLARE @FacilityLimitType INT;
DECLARE @FacilityLimitAmount DECIMAL(20, 8);
DECLARE @WP_ConcentrationResidualTotal DECIMAL(20, 8);
DECLARE @PFP_ConcentrationResidualTotal DECIMAL(20, 8);
DECLARE @WP_ConcentrationPercentage DECIMAL(20, 8);
DECLARE @PFP_ConcentrationPercentage DECIMAL(20, 8);
DECLARE @FacilityID INT;
DECLARE @TolerancePcnt DECIMAL(20, 8);
DECLARE @ToleranceThresholdPcnt DECIMAL(20, 8);
DECLARE @ToleranceThresholdAmount DECIMAL(20, 8);
DECLARE @WP_NPVTotal DECIMAL(20, 8);
DECLARE @PFP_NPVTotal DECIMAL(20, 8);
DECLARE @FacilityName NVARCHAR(50);

SELECT @ConcentrationTest_ID = ID
FROM Securitization.ConcentrationTests
WHERE Name = 'Residual Greater Than 10 Dollars';

SELECT @FacilityID = Facility_ID,
       @FacilityName = UPPER(f.Name)
FROM Securitization.Pools p
    INNER JOIN Securitization.FacilityLessor fl
        ON fl.ID = p.FacilityLessor_ID
    INNER JOIN Securitization.Facilities f
        ON f.ID = fl.Facility_ID
WHERE p.ID = @PoolID;
SELECT @FacilityLimit_ID = fl.ID,
       @FacilityLimitPcnt = fl.Percentage,
       @FacilityLimitType = [Type]
FROM [Securitization].[FacilityLimits] fl
WHERE ConcentrationTest_ID = @ConcentrationTest_ID
      AND Facility_ID = @FacilityID;

IF @FacilityLimit_ID > 0
BEGIN


    SELECT @TolerancePcnt = Tolerance
    FROM [Securitization].[ConcentrationTests]
    WHERE ID = @ConcentrationTest_ID;
    SELECT @FacilityID = fl.Facility_ID
    FROM Securitization.Pools p
        INNER JOIN Securitization.FacilityLessor fl
            ON fl.ID = p.FacilityLessor_ID
    WHERE p.ID = @PoolID;

    SELECT @WP_NPVTotal = SUM(COALESCE(NPV, 0))
    FROM Securitization.PoolContract pc
    WHERE Active = 1
          AND InFacilityPool = 0
          AND Pool_ID = @PoolID;

    SELECT @PFP_NPVTotal = SUM(COALESCE(NPV, 0))
    FROM Securitization.PoolContract pc
    WHERE Active = 1
          AND Pool_ID = @PoolID;

    SET @WP_NPVTotal = COALESCE(@WP_NPVTotal, 0);
    SET @PFP_NPVTotal = COALESCE(@PFP_NPVTotal, 0);

    DECLARE @RankPosition INT;
    SELECT @RankPosition = RankPosition
    FROM Securitization.ConcentrationTests
    WHERE ID = @ConcentrationTest_ID;

    SELECT @PFP_ConcentrationResidualTotal = SUM(COALESCE(c.Residual, 0))
    FROM [Securitization].[PoolContract] pc
        INNER JOIN [Securitization].[Contracts] c
            ON pc.Contract_ContractID = c.ContractID
    WHERE pc.Pool_ID = @PoolID
          AND pc.Active = 1
          AND ([Residual] > 10)
    OPTION (RECOMPILE);

    SELECT @WP_ConcentrationResidualTotal = SUM(COALESCE(c.Residual, 0))
    FROM [Securitization].[PoolContract] pc
        INNER JOIN [Securitization].[Contracts] c
            ON pc.Contract_ContractID = c.ContractID
    WHERE pc.Pool_ID = @PoolID
          AND pc.Active = 1
          AND pc.InFacilityPool = 0
          AND ([Residual] > 10)
    OPTION (RECOMPILE);

    SET @WP_ConcentrationResidualTotal = COALESCE(@WP_ConcentrationResidualTotal, 0);
    SET @PFP_ConcentrationResidualTotal = COALESCE(@PFP_ConcentrationResidualTotal, 0);
    --Calculate the current concentration percentage
    SET @WP_ConcentrationPercentage = 0;
    IF (@WP_NPVTotal <> 0)
    BEGIN
        SET @WP_ConcentrationPercentage = (@WP_ConcentrationResidualTotal / @WP_NPVTotal);
    END;

    SET @PFP_ConcentrationPercentage = 0;
    IF (@PFP_NPVTotal <> 0)
    BEGIN
        SET @PFP_ConcentrationPercentage = (@PFP_ConcentrationResidualTotal / @PFP_NPVTotal);
    END;

    IF (@FacilityLimitType = 1) --Either % or $ is greater
    BEGIN
        DECLARE @AmountPcntEquivalent DECIMAL(20, 8);
        SET @AmountPcntEquivalent = @FacilityLimitAmount / @PFP_NPVTotal;

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

    SET @ToleranceThresholdPcnt = @FacilityLimitPcnt * @TolerancePcnt;
    INSERT INTO [Securitization].[PoolConcentration]
    (
        [WorkingPoolConcentration],
        [WorkingPoolConcentrationStatus],
        [WorkingPoolNPV],
        [ProposedFacilityPoolConcentration],
        [ProposedFacilityPoolConcentrationStatus],
        [ProposedFacilityPoolNPV],
        [CreatedOn],
        [CreatedBy],
        [ModifiedOn],
        [ModifiedBy],
        [Pool_ID],
        [FacilityLimit_Type],
        [ConcentrationTest_ID]
    )
    VALUES
    (   @WP_ConcentrationPercentage, CASE
                                         WHEN @WP_ConcentrationPercentage >= @ToleranceThresholdPcnt THEN
                                             0 --Green
                                         WHEN @WP_ConcentrationPercentage < @ToleranceThresholdPcnt
                                              AND @WP_ConcentrationPercentage >= @FacilityLimitPcnt THEN
                                             1 --Yellow
                                         ELSE
                                             2 --Red
                                     END, @WP_ConcentrationResidualTotal, @PFP_ConcentrationPercentage,
        CASE
            WHEN @PFP_ConcentrationPercentage >= @ToleranceThresholdPcnt THEN
                0 --Green
            WHEN @PFP_ConcentrationPercentage < @ToleranceThresholdPcnt
                 AND @PFP_ConcentrationPercentage >= @FacilityLimitPcnt THEN
                1 --Yellow
            ELSE
                2 --Red
        END, @PFP_ConcentrationResidualTotal, GETDATE(), @UserID, GETDATE(), @UserID, @PoolID, @FacilityLimitType,
        @ConcentrationTest_ID);

END;

GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]
* [Securitization].[ConcentrationTests]
* [Securitization].[Contracts]
* [Securitization].[Facilities]
* [Securitization].[FacilityLessor]
* [Securitization].[FacilityLimits]
* [Securitization].[PoolConcentration]
* [Securitization].[PoolContract]
* [Securitization].[Pools]


---

## <a name="#usedby"></a>Used By

* [[Securitization].[SP_CT_All]](Securitization_SP_CT_All.md)


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

