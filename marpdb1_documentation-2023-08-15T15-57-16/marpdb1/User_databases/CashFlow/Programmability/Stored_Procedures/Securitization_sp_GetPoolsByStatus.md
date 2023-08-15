#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_GetPoolsByStatus

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_GetPoolsByStatus]

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
| @Status | int | 4 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
-- =============================================
-- History:
--		Name:	John Thadison
--		Date:	02/23/2022
--		Desc:	STRY0018934  - Retire AdvanceRate logic from concentration tests.
-- =============================================
CREATE PROCEDURE [Securitization].[sp_GetPoolsByStatus]
	-- Add the parameters for the stored procedure here
	@Status int = 0

AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;

	IF(@Status = 1)
	BEGIN
		SELECT 
			p.ID, 
			p.TargetPoolSize, 
			p.DefaultDiscountRate,
			p.AdvanceRate,  
			p.ProductLineCode, 
			p.StartingDate ,
			p.Funding, 
			p.FundingDate, 
			f.Name as FacilityName, 
			p.StubPeriod,
			p.ModifiedOn, 
			p.ModifiedBy,
			STUFF(( SELECT ',' + Convert(varchar,l.LessorID)
                FROM Securitization.Lessors l
				WHERE l.Pool_ID = p.ID
              FOR
                XML PATH('')
              ), 1, 1, '') AS Lessors,
			  Comments,
			  Comments2,
			  [Status],
			  COALESCE(WorkingPoolNPV,0) as WorkingPoolNPV
		FROM 
			Securitization.Pools p
			INNER JOIN Securitization.FacilityLessor fl ON fl.ID = p.FacilityLessor_ID		
			INNER JOIN Securitization.Facilities f ON f.ID = fl.Facility_ID
			LEFT OUTER JOIN 
			(
				--Working Pool Size
				SELECT Pool_Id, COALESCE(SUM(NPV), 0) AS WorkingPoolNPV 
				FROM Securitization.PoolContract pc
				WHERE 
				Active = 1
				AND InFacilityPool=0
				GROUP BY Pool_ID
			) WorkingPool ON WorkingPool.Pool_Id = p.Id
		WHERE p.Status = 1
		ORDER BY p.ID DESC
	END
	ELSE IF(@Status > 1)--Archived and or  pools
	BEGIN
		SELECT 
			p.ID, 
			p.TargetPoolSize, 
			p.DefaultDiscountRate, 
			p.AdvanceRate, 
			p.ProductLineCode, 
			p.StartingDate ,
			p.Funding, 
			p.FundingDate, 
			f.Name as FacilityName, 
			p.StubPeriod,
			p.ModifiedOn, 
			p.ModifiedBy,
			STUFF(( SELECT ',' + Convert(varchar,l.LessorID)
                FROM Securitization.Lessors l
				WHERE l.Pool_ID = p.ID
              FOR
                XML PATH('')
              ), 1, 1, '') AS Lessors,
			  Comments,
			  Comments2,
			  [Status],
			  COALESCE(WorkingPoolNPV,0) as WorkingPoolNPV
		FROM 
			Securitization.Pools p
			INNER JOIN Securitization.FacilityLessor fl ON fl.ID = p.FacilityLessor_ID	
			INNER JOIN Securitization.Facilities f ON f.ID = fl.Facility_ID	
			LEFT OUTER JOIN 
			(
				--Working Pool Size
				SELECT Pool_Id, COALESCE(SUM(NPV), 0) AS WorkingPoolNPV 
				FROM Archive.PoolExport
				where Active = 1 
				GROUP BY Pool_Id
							) WorkingPool ON WorkingPool.Pool_Id = p.Id
		WHERE p.Status > 1
		ORDER BY p.ID DESC
	END

END
GO

```


---

## <a name="#uses"></a>Uses

* [Archive].[PoolExport]
* [Securitization]
* [Securitization].[Facilities]
* [Securitization].[FacilityLessor]
* [Securitization].[Lessors]
* [Securitization].[PoolContract]
* [Securitization].[Pools]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

