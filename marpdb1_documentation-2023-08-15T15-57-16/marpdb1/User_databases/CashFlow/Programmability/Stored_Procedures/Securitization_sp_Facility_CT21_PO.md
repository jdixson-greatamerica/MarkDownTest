#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_Facility_CT21_PO

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_Facility_CT21_PO]

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
-- =============================================
-- History:
--		Name:	John Thadison
--		Date:	02/23/2022
--		Desc:	STRY0018934  - Retire AdvanceRate logic from concentration tests.
-- =============================================
CREATE PROCEDURE [Securitization].[sp_Facility_CT21_PO]
	@FacilityID [int],
	@UserID [nvarchar](255)
AS
DECLARE @ConcentrationTest_ID int
DECLARE @FacilityLimit_ID int
DECLARE @FacilityLimitPcnt decimal(20,8)
DECLARE @FacilityLimitType int 
DECLARE @FacilityLimitAmount decimal(20,8) 
DECLARE @Facility_ConcentrationNPVTotal decimal(20,8)
DECLARE @Facility_ConcentrationPercentage decimal(20,8)
DECLARE @TolerancePcnt decimal(20, 8)
DECLARE @ToleranceThresholdPcnt decimal(20, 8)
DECLARE @ToleranceThresholdAmount decimal(20, 8)
DECLARE @Facility_NPVTotal decimal(20, 8)
DECLARE @FacilityName nvarchar(50)

SELECT @ConcentrationTest_ID = 21

SELECT @FacilityName = f.Name from Securitization.Facilities f 
INNER JOIN Securitization.FacilityLessor fl on f.id = fl.Facility_ID
where f.id = @FacilityID
SELECT @FacilityLimit_ID = fl.ID, @FacilityLimitPcnt = fl.Percentage,@FacilityLimitAmount = fl.Amount, @FacilityLimitType = [Type] FROM [Securitization].[FacilityLimits] fl
WHERE ConcentrationTest_ID = @ConcentrationTest_ID
AND Facility_ID = @FacilityID

IF @FacilityLimit_ID > 0
BEGIN


SELECT @TolerancePcnt = Tolerance FROM [Securitization].[ConcentrationTests] WHERE ID = @ConcentrationTest_ID;
SELECT @Facility_NPVTotal = SUM(COALESCE(CurrentMonthNPV,0)) from Securitization.Contracts c WHERE Lessor in(SELECT fl.LessorID from Securitization.Facilities f INNER JOIN Securitization.FacilityLessor fl on f.id = fl.Facility_ID where f.id = @FacilityID);
SET @Facility_NPVTotal = COALESCE(@Facility_NPVTotal,0)

DECLARE @RankPosition INT; 
SELECT @RankPosition = RankPosition 
FROM Securitization.ConcentrationTests 
WHERE id = @ConcentrationTest_ID; 

SELECT @Facility_ConcentrationNPVTotal = SUM(COALESCE(c.CurrentMonthNPV,0)) FROM [Securitization].[Contracts] c
WHERE c.Lessor in (SELECT fl.LessorID from Securitization.Facilities f INNER JOIN Securitization.FacilityLessor fl on f.id = fl.Facility_ID where f.id = @FacilityID)
	AND (	      (
[PurchaseOrder] = '1'	      )
) 

SET @Facility_ConcentrationNPVTotal = COALESCE(@Facility_ConcentrationNPVTotal,0)
--Calculate the current concentration percentage
SET @Facility_ConcentrationPercentage = 0
IF(@Facility_NPVTotal <> 0)
BEGIN
SET @Facility_ConcentrationPercentage = CAST((@Facility_ConcentrationNPVTotal/@Facility_NPVTotal) as decimal(20,8))
END

IF(@FacilityLimitType = 1)--Either % or $ is greater
BEGIN
	DECLARE @AmountPcntEquivalent decimal(20,8)
	IF ( @Facility_NPVTotal <> 0 ) 
BEGIN
	SET @AmountPcntEquivalent = CAST((@FacilityLimitAmount/@Facility_NPVTotal) as decimal(20,8))
END
ELSE
BEGIN
SET @AmountPcntEquivalent = 0
END

	IF(@AmountPcntEquivalent > @FacilityLimitPcnt)
	BEGIN
		SET @FacilityLimitType = 2 --Override to use the $ amount
	END
	ELSE
	BEGIN
		SET @FacilityLimitType = 0 --Override to use the %
	END
END

--Calculate the Tolerance percent threshold
IF(@FacilityLimitType = 0)
BEGIN
	--Calculate the Tolerance percent threshold
	SET @ToleranceThresholdPcnt = @FacilityLimitPcnt * @TolerancePcnt

	INSERT INTO [Securitization].[FacilityConcentration](
	[Concentration],
	[ConcentrationStatus],
	[NPV],
	[CreatedOn],
	[CreatedBy],
	[ModifiedOn],
	[ModifiedBy],
	[Facility_ID],
	[FacilityLimit_Type],
	[ConcentrationTest_ID])
	VALUES (
	@Facility_ConcentrationPercentage,
	CASE 
	WHEN @Facility_ConcentrationPercentage <=@ToleranceThresholdPcnt THEN
	0--Green
	WHEN @Facility_ConcentrationPercentage > @ToleranceThresholdPcnt and @Facility_ConcentrationPercentage <= @FacilityLimitPcnt THEN
	1--Yellow
	ELSE
	2--Red
	END ,
	@Facility_ConcentrationNPVTotal,
	GETDATE(),
	@UserID,
	GETDATE(),
	@UserID,
	@FacilityID,
	@FacilityLimitType,
	@ConcentrationTest_ID)

END
ELSE
BEGIN
	--Calculate the Tolerance percent threshold
	SET @ToleranceThresholdAmount= @FacilityLimitAmount * @TolerancePcnt

	INSERT INTO [Securitization].[FacilityConcentration](
	[Concentration],
	[ConcentrationStatus],
	[NPV],
	[CreatedOn],
	[CreatedBy],
	[ModifiedOn],
	[ModifiedBy],
	[Facility_ID],
	[FacilityLimit_Type],
	[ConcentrationTest_ID])
	VALUES (
	@Facility_ConcentrationPercentage,
	CASE 
	WHEN @Facility_ConcentrationNPVTotal <=@ToleranceThresholdAmount THEN
	0--Green
	WHEN @Facility_ConcentrationNPVTotal > @ToleranceThresholdAmount and @Facility_ConcentrationNPVTotal <= @FacilityLimitAmount THEN
	1--Yellow
	ELSE
	2--Red
	END ,
	@Facility_ConcentrationNPVTotal,
	GETDATE(),
	@UserID,
	GETDATE(),
	@UserID,
	@FacilityID,
	@FacilityLimitType,
	@ConcentrationTest_ID)

END
END


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

