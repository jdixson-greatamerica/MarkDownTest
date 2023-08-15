#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_GetPoolSummaryInfoByPoolId

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_GetPoolSummaryInfoByPoolId]

---

## <a name="#properties"></a>Properties

| Property | Value |
|---|---|
| ANSI Nulls On | YES |
| Quoted Identifier On | YES |


---

## <a name="#parameters"></a>Parameters

| Name | Data Type | Max Length (Bytes) |
|---|---|---|
| @PoolId | int | 4 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
-- =============================================
-- Author:		John Thadison
-- Create date: 09/01/2015
-- Description:	returns Pools including Facility Lessor by status
--Test:  [Securitization].[sp_GetPoolSummaryInfoByPoolId] 159
-- History:
--		Name:	Chad Feldmann
--		Date:	06/14/2019
--		Desc:	Ticket #PRJ0040256 - Changed reason to 'Random Cut'
-- 
-- History:
--		Name:	John Thadison
--		Date:	02/23/2022
--		Desc:	STRY0018934  - Retire AdvanceRate logic from concentration tests.
-- =============================================
CREATE PROCEDURE [Securitization].[sp_GetPoolSummaryInfoByPoolId]
	-- Add the parameters for the stored procedure here
	@PoolId int = 0
AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;

	SELECT P.ID,
			P.AdvanceRate,
		  COALESCE(PassConcentrationTestCount,0) as PassConcentrationTestCount
		  ,COALESCE(WarningConcentrationTestCount,0) as WarningConcentrationTestCount
		  ,COALESCE(ErrorConcentrationTestCount,0) as ErrorConcentrationTestCount
		  ,COALESCE(WorkingPoolNPV,0) AS WorkingPoolNPV
			,COALESCE(WorkingPoolContractCount,0) AS WorkingPoolContractCount
			,COALESCE(ProposedFacilityPoolNPV,0) AS ProposedFacilityPoolNPV
			,COALESCE(RandomCutCount,0) AS RandomCutCount
	FROM Securitization.Pools p
	LEFT OUTER JOIN 
	(
		SELECT Pool_Id, COUNT(*) AS PassConcentrationTestCount
		FROM Securitization.PoolConcentration
		WHERE ProposedFacilityPoolConcentrationStatus = 0
		GROUP BY Pool_ID
	) green ON p.ID = green.Pool_ID
	LEFT OUTER JOIN 
	(
		SELECT Pool_Id, COUNT(*) AS WarningConcentrationTestCount 
		FROM Securitization.PoolConcentration
		WHERE ProposedFacilityPoolConcentrationStatus = 1
		GROUP BY Pool_ID
	) yellow ON p.ID = yellow.Pool_Id
	LEFT OUTER JOIN 
	(
		SELECT Pool_Id, COUNT(*) AS ErrorConcentrationTestCount 
		FROM Securitization.PoolConcentration
		WHERE ProposedFacilityPoolConcentrationStatus = 2
		GROUP BY Pool_ID
	) red ON p.ID = red.Pool_ID	
	LEFT OUTER JOIN 
	(
		--Working Pool Size
		SELECT Pool_Id, COALESCE(SUM(NPV), 0) AS WorkingPoolNPV 
		FROM Securitization.PoolContract pc
		WHERE 
		Active = 1
		AND InFacilityPool=0
		AND Pool_ID = @PoolId
		GROUP BY Pool_ID
	) WorkingPool ON WorkingPool.Pool_Id = p.Id
	LEFT OUTER JOIN
	(
		--GetWorkingPoolContractCount
		SELECT Pool_Id, COUNT(*) AS WorkingPoolContractCount  FROM Securitization.PoolContract 
		WHERE
		Active = 1
		AND Pool_ID = @PoolID 
		AND NPV IS NOT NULL 
		AND (InFacilityPool IS NOT NULL AND InFacilityPool=0)
		GROUP BY Pool_ID
	) WorkingPoolCount ON WorkingPoolCount.Pool_Id = p.Id
	LEFT OUTER JOIN
	(
		-- GetProposedFacilityPoolNPV
		SELECT Pool_ID, SUM(COALESCE(NPV,0)) AS ProposedFacilityPoolNPV
		FROM Securitization.PoolContract pc
		WHERE 
		Active = 1
		AND Pool_ID = @PoolId
		GROUP BY Pool_ID
	) ProposedFacilityPoolNPV ON ProposedFacilityPoolNPV.Pool_ID = p.Id
	LEFT OUTER JOIN
	(
		SELECT Pool_ID, COUNT(*) AS RandomCutCount 
		FROM Securitization.PoolContract
		WHERE Pool_ID = @PoolId
		AND active=0
		AND LOWER(Reason) in ('random','Random Cut')
		GROUP BY Pool_Id
	) RandomCount ON RandomCount.Pool_ID = p.Id

	WHERE p.ID = @PoolID
 
 END
	
GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]
* [Securitization].[PoolConcentration]
* [Securitization].[PoolContract]
* [Securitization].[Pools]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

