#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.spGetPoolConcentrationTestsByPoolId

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[spGetPoolConcentrationTestsByPoolId]

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
-- Author:		Logan Keenan
-- Create date: 06/03/2013
-- Description:	Gets the pool concentration test information
--Test:  [Securitization].[spGetPoolConcentrationTestsByPoolId] 
-- 
-- History:
--		Name:	John Thadison
--		Date:	02/23/2022
--		Desc:	STRY0018934  - Retire AdvanceRate logic from concentration tests.
-- =============================================
CREATE procedure [Securitization].[spGetPoolConcentrationTestsByPoolId]
	@PoolId int

	as
	begin

DECLARE @tempPool_ID INT
set @tempPool_ID = @PoolId

select  ct.Name as Name, 
pc.ProposedFacilityPoolNPV as Amount, 
pc.ProposedFacilityPoolConcentration as [Percent], 
CASE pc.FacilityLimit_Type
	WHEN 0 THEN
		fl.Percentage
	WHEN 2 THEN
		fl.Amount
END as Cap,
pc.ProposedFacilityPoolConcentrationStatus as Status, 
CASE 
	WHEN pc.FacilityLimit_Type = 0 and ct.Tolerance <= 1  THEN
		[Securitization].[RoundNPV] (sum(PoolContract.NPV) * fl.Percentage,f.Term) - pc.ProposedFacilityPoolNPV
	WHEN pc.FacilityLimit_Type = 2  and ct.Tolerance <= 1 THEN
		fl.Amount - pc.ProposedFacilityPoolNPV
	WHEN pc.FacilityLimit_Type = 0 THEN
		pc.ProposedFacilityPoolNPV - [Securitization].[RoundNPV] (sum(PoolContract.NPV) * fl.Percentage,f.Term)
	WHEN pc.FacilityLimit_Type = 2 THEN
		pc.ProposedFacilityPoolNPV - fl.Amount
END as CapAvailability,
pc.FacilityLimit_Type
from Securitization.Pools p 
inner join Securitization.PoolContract PoolContract 
	on p.id = PoolContract.Pool_ID
inner join Securitization.PoolConcentration pc on pc.Pool_ID = p.ID
inner join Securitization.ConcentrationTests ct on ct.ID = pc.ConcentrationTest_ID
inner join Securitization.FacilityLessor as FacilityLessor on FacilityLessor.ID = p.FacilityLessor_ID
inner join Securitization.FacilityLimits fl on fl.ConcentrationTest_ID = ct.ID and fl.Facility_ID = FacilityLessor.Facility_ID
inner join Securitization.Facilities f on f.ID = fl.Facility_ID
where p.id = @tempPool_ID and PoolContract.Active = 1
and ct.Type in(0,2) --0=both 1 =Facility Pool Only
group by ct.Name, pc.ProposedFacilityPoolNPV, pc.ProposedFacilityPoolConcentration, fl.Percentage, pc.ProposedFacilityPoolConcentrationStatus, pc.ProposedFacilityPoolNPV, pc.FacilityLimit_Type,fl.Amount, ct.Tolerance, f.Name,f.Term
order by ct.Name
OPTION (OPTIMIZE FOR UNKNOWN)
	end

GO

```


---

## <a name="#uses"></a>Uses

* [[Securitization].[RoundNPV]](../Functions/Scalar-valued_Functions/Securitization_RoundNPV.md)
* [Securitization]
* [Securitization].[ConcentrationTests]
* [Securitization].[Facilities]
* [Securitization].[FacilityLessor]
* [Securitization].[FacilityLimits]
* [Securitization].[PoolConcentration]
* [Securitization].[PoolContract]
* [Securitization].[Pools]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

