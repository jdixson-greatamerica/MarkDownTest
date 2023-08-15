#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_CT78_JuniorLienNote

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_CT78_JuniorLienNote]

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
-- =============================================
-- History:
--		Name:	John Thadison
--		Date:	02/23/2022
--		Desc:	STRY0018934  - Retire AdvanceRate logic from concentration tests.
-- =============================================
CREATE PROCEDURE [Securitization].[sp_CT78_JuniorLienNote]
	@PoolID [int],
	@UserID [nvarchar](255)
AS
DECLARE @ConcentrationTest_ID int
DECLARE @FacilityLimit_ID int
DECLARE @FacilityLimitPcnt decimal(20,8)
DECLARE @FacilityLimitType int
DECLARE @FacilityLimitAmount decimal(20,8)
DECLARE @WP_ConcentrationNPVTotal decimal(20,8)
DECLARE @PFP_ConcentrationNPVTotal decimal(20,8)
DECLARE @WP_ConcentrationPercentage decimal(20,8)
DECLARE @PFP_ConcentrationPercentage decimal(20,8)
DECLARE @FacilityID int
DECLARE @TolerancePcnt decimal(20, 8)
DECLARE @ToleranceThresholdPcnt decimal(20, 8)
DECLARE @ToleranceThresholdAmount decimal(20, 8)
DECLARE @WP_NPVTotal decimal(20, 8)
DECLARE @PFP_NPVTotal decimal(20, 8)
DECLARE @FacilityName nvarchar(50)

SELECT @ConcentrationTest_ID = 78

SELECT @FacilityID = Facility_ID, @FacilityName = UPPER(f.Name)  FROM Securitization.Pools p
INNER JOIN Securitization.FacilityLessor fl on fl.id = p.FacilityLessor_ID
inner join Securitization.Facilities f on f.id = fl.Facility_ID
WHERE p.ID = @PoolID;
SELECT @FacilityLimit_ID = fl.ID, @FacilityLimitPcnt = fl.Percentage,@FacilityLimitAmount = fl.Amount, @FacilityLimitType = [Type] FROM [Securitization].[FacilityLimits] fl
WHERE ConcentrationTest_ID = @ConcentrationTest_ID
AND Facility_ID = @FacilityID

IF @FacilityLimit_ID > 0
BEGIN


SELECT @TolerancePcnt = Tolerance FROM [Securitization].[ConcentrationTests] WHERE ID = @ConcentrationTest_ID;
SELECT @FacilityID = fl.Facility_ID FROM Securitization.Pools p INNER JOIN Securitization.FacilityLessor fl on fl.id = p.FacilityLessor_ID WHERE p.ID = @PoolID;
SELECT @WP_NPVTotal = SUM(COALESCE(NPV,0)) 
from Securitization.PoolContract pc
WHERE Active=1 and InFacilityPool=0 and Pool_ID = @PoolID;

SELECT @PFP_NPVTotal = SUM(COALESCE(NPV,0)) 
from Securitization.PoolContract pc
WHERE Active=1 and Pool_ID = @PoolID;

SET @WP_NPVTotal = COALESCE(@WP_NPVTotal,0)
SET @PFP_NPVTotal = COALESCE(@PFP_NPVTotal,0)

DECLARE @RankPosition INT; 
SELECT @RankPosition = RankPosition 
FROM Securitization.ConcentrationTests 
WHERE id = @ConcentrationTest_ID; 

SELECT @PFP_ConcentrationNPVTotal = SUM(COALESCE(pc.NPV,0)) FROM [Securitization].[PoolContract] pc
INNER JOIN [Securitization].[Contracts] c on pc.Contract_ContractID = c.ContractID
WHERE pc.Pool_ID = @PoolID
AND pc.Active = 1
	AND (
	      (
[ProgramTypeID] = 0121	      )
) OPTION (RECOMPILE);

SELECT @WP_ConcentrationNPVTotal = SUM(COALESCE(pc.NPV,0)) FROM [Securitization].[PoolContract] pc
INNER JOIN [Securitization].[Contracts] c on pc.Contract_ContractID = c.ContractID
WHERE pc.Pool_ID = @PoolID
AND pc.Active = 1
AND pc.InFacilityPool=0
	AND (	      (
[ProgramTypeID] = 0121	      )
) OPTION (RECOMPILE);

SET @WP_ConcentrationNPVTotal = COALESCE(@WP_ConcentrationNPVTotal,0)
SET @PFP_ConcentrationNPVTotal = COALESCE(@PFP_ConcentrationNPVTotal,0)
--Calculate the current concentration percentage
SET @WP_ConcentrationPercentage = 0
IF(@WP_NPVTotal <> 0)
BEGIN
SET @WP_ConcentrationPercentage = CAST((@WP_ConcentrationNPVTotal/@WP_NPVTotal) as decimal(20,8))
END

SET @PFP_ConcentrationPercentage = 0
IF(@PFP_NPVTotal <> 0)
BEGIN
SET @PFP_ConcentrationPercentage = CAST((@PFP_ConcentrationNPVTotal/@PFP_NPVTotal) as decimal(20,8))
END

IF(@FacilityLimitType = 1)--Either % or $ is greater
BEGIN
	DECLARE @AmountPcntEquivalent decimal(20,8)
	SET @AmountPcntEquivalent = CAST((@FacilityLimitAmount/@PFP_NPVTotal) as decimal(20,8))

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
	SET @ToleranceThresholdPcnt = @FacilityLimitPcnt * @TolerancePcnt
	INSERT INTO [Securitization].[PoolConcentration](
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
	[ConcentrationTest_ID])
	VALUES (@WP_ConcentrationPercentage,
	CASE 
	WHEN @WP_ConcentrationPercentage <=@ToleranceThresholdPcnt THEN
	0--Green
	WHEN @WP_ConcentrationPercentage > @ToleranceThresholdPcnt and @WP_ConcentrationPercentage <= @FacilityLimitPcnt THEN
	1--Yellow
	ELSE
	2--Red
	END ,
	@WP_ConcentrationNPVTotal,
	@PFP_ConcentrationPercentage,
	CASE 
	WHEN @PFP_ConcentrationPercentage <=@ToleranceThresholdPcnt THEN
	0--Green
	WHEN @PFP_ConcentrationPercentage > @ToleranceThresholdPcnt and @PFP_ConcentrationPercentage <= @FacilityLimitPcnt THEN
	1--Yellow
	ELSE
	2--Red
	END ,
	@PFP_ConcentrationNPVTotal,
	GETDATE(),
	@UserID,
	GETDATE(),
	@UserID,
	@PoolID,
	@FacilityLimitType,
	@ConcentrationTest_ID)
END
ELSE
BEGIN
SET @ToleranceThresholdAmount = @FacilityLimitAmount * @TolerancePcnt
	INSERT INTO [Securitization].[PoolConcentration](
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
	[ConcentrationTest_ID])
	VALUES (@WP_ConcentrationPercentage,
	CASE 
	WHEN @WP_ConcentrationNPVTotal <=@ToleranceThresholdAmount THEN
	0--Green
	WHEN @WP_ConcentrationNPVTotal > @ToleranceThresholdAmount and @WP_ConcentrationNPVTotal <= @FacilityLimitAmount THEN
 1--Yellow
	ELSE
	2--Red
	END ,
	@WP_ConcentrationNPVTotal,
	@PFP_ConcentrationPercentage,
	CASE 
	WHEN @PFP_ConcentrationNPVTotal <=@ToleranceThresholdAmount THEN
	0--Green
	WHEN @PFP_ConcentrationNPVTotal > @ToleranceThresholdAmount and @PFP_ConcentrationNPVTotal <= @FacilityLimitAmount THEN
	1--Yellow
	ELSE
	2--Red
	END ,
	@PFP_ConcentrationNPVTotal,
	GETDATE(),
	@UserID,
	GETDATE(),
	@UserID,
	@PoolID,
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

