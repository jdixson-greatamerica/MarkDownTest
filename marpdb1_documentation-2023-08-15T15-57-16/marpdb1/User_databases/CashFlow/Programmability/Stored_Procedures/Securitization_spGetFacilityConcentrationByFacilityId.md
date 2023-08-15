#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.spGetFacilityConcentrationByFacilityId

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[spGetFacilityConcentrationByFacilityId]

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
| @FacilityId | int | 4 |


---

## <a name="#sqlscript"></a>SQL Script

```sql


-- =============================================
-- Author:		Logan Keenan
-- Create date: 06/03/2013
-- Description:	Gets the facility concentration test information
--Test:  [Securitization].[spGetFacilityConcentrationByFacilityId] 8
-- History:
--		Name:	John Thadison
--		Date:	02/23/2022
--		Desc:	STRY0018934  - Retire AdvanceRate logic from concentration tests.
-- =============================================
CREATE procedure [Securitization].[spGetFacilityConcentrationByFacilityId]
	@FacilityId int

	as
	begin
select ct.Name as Name, 
fc.NPV as NPV, 
Round(fc.Concentration,4) as Concentration, 
fc.ConcentrationStatus as Status, 
CASE fc.FacilityLimit_Type
	WHEN 0 THEN
		Round(flim.Percentage,4)
	WHEN 2 THEN
		flim.Amount
END as Cap,
CASE 
	WHEN fc.FacilityLimit_Type = 0  and ct.Tolerance <= 1 THEN
		[Securitization].[RoundNPV] ((coalesce(Sum(c.CurrentMonthNPV), 0) * flim.Percentage),f.Term) - fc.NPV
	WHEN fc.FacilityLimit_Type = 2  and ct.Tolerance <= 1 THEN
		flim.Amount - fc.NPV
	WHEN fc.FacilityLimit_Type = 0   THEN
		fc.NPV - [Securitization].[RoundNPV] ((coalesce(Sum(c.CurrentMonthNPV), 0) * flim.Percentage),f.Term)
	WHEN fc.FacilityLimit_Type = 2   THEN
		fc.NPV - flim.Amount
END as CapAvailability,
f.Name as FacilityName,
1 as sortorder
from Securitization.Facilities f
inner join Securitization.FacilityConcentration fc
	on fc.Facility_ID = f.ID
inner join Securitization.ConcentrationTests ct 
	on fc.ConcentrationTest_ID = ct.ID
inner join Securitization.FacilityLessor fl 
	on fl.Facility_ID = f.ID
inner join Securitization.Contracts c 
	on c.Lessor = fl.LessorID 
inner join Securitization.FacilityLimits flim
	on flim.ConcentrationTest_ID = ct.ID and flim.Facility_ID = f.ID
where 
f.ID = @FacilityId 
and ct.Type in(0,2) --0=both 1 =Facility Pool Only
group by ct.Name, fc.NPV, fc.Concentration, flim.Percentage, fc.ConcentrationStatus,fc.FacilityLimit_Type,flim.Amount,ct.Tolerance, f.Name,f.Term

UNION
select 'Total NPV' as Name, 
coalesce(Sum(c.CurrentMonthNPV), 0) as NPV, 
0 as Concentration, 
0 as Status, 
0 as Cap,
0 as CapAvailability,
f.Name as FacilityName,
4 as sortorder
from Securitization.Facilities f
inner join Securitization.FacilityLessor fl 
	on fl.Facility_ID = f.ID
inner join Securitization.Contracts c 
	on c.Lessor = fl.LessorID 
where 
f.ID = @FacilityId 
group by  f.Name


	end


GO

```


---

## <a name="#uses"></a>Uses

* [[Securitization].[RoundNPV]](../Functions/Scalar-valued_Functions/Securitization_RoundNPV.md)
* [Securitization]
* [Securitization].[ConcentrationTests]
* [Securitization].[Contracts]
* [Securitization].[Facilities]
* [Securitization].[FacilityConcentration]
* [Securitization].[FacilityLessor]
* [Securitization].[FacilityLimits]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

