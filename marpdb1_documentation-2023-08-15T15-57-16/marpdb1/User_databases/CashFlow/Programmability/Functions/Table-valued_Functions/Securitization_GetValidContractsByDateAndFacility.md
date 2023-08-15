#### 

[Project](../../../../../../index.md) > [marpdb1](../../../../../index.md) > [User databases](../../../../index.md) > [CashFlow](../../../index.md) > Programmability > Functions > [Table-valued Functions](Table-valued_Functions.md) > Securitization.GetValidContractsByDateAndFacility

# ![Table-valued Functions](../../../../../../Images/Function_Table32.png) [Securitization].[GetValidContractsByDateAndFacility]

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
| @StartingDate | datetime | 8 |
| @FacilityID | int | 4 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
-- =============================================
-- Author:		<Author,,Name>
-- Create date: <Create Date,,>
-- Description:	<Description,,>
-- =============================================
create FUNCTION [Securitization].[GetValidContractsByDateAndFacility]
(
	-- Add the parameters for the function here
	@StartingDate datetime,
	@FacilityID int
)
RETURNS 
@ValidContracts TABLE 
(
	-- Add the column definitions for the TABLE variable here
	ContractID char(15),
	Pass bit

)
AS
BEGIN

DECLARE @FailedEligibilityTests TABLE
(
  Contract_ContractID char(15),
  EligibilityCriteria_ID int,
  ContractEligibilityCriteriaID int,
  Pass bit
)

IF( EXISTS(SELECT distinct StartingDate FROM Securitization.ContractEligibilityCriteria where StartingDate=@StartingDate))
BEGIN

--Get list of failed eligiblity criteria tests
INSERT INTO @FailedEligibilityTests (Contract_ContractID, EligibilityCriteria_ID,ContractEligibilityCriteriaID, Pass)
  select cec.Contract_ContractID, cec.EligibilityCriteria_ID, cec.id, cec.Pass from Securitization.ContractEligibilityCriteria cec 
where pass=0  and coalesce(StartingDate,@StartingDate)=@StartingDate

insert into @ValidContracts(ContractID, Pass)
(
select c.ContractID, 1 as Pass from Securitization.Contracts c
left join(
select distinct Contract_ContractID from @FailedEligibilityTests cec
inner join Securitization.EligibilityCriteriaFacilities ecf on ecf.EligibilityCriteria_ID=cec.EligibilityCriteria_ID
where Facility_ID=@FacilityID) FailedByFacility on FailedByFacility.Contract_ContractID = c.ContractID
where FailedByFacility.Contract_ContractID is null
)

END

RETURN

END

GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]
* [Securitization].[ContractEligibilityCriteria]
* [Securitization].[Contracts]
* [Securitization].[EligibilityCriteriaFacilities]


---

## <a name="#usedby"></a>Used By

* [[Securitization].[spInsertPool]](../../Stored_Procedures/Securitization_spInsertPool.md)
* [[Securitization].[spRefreshPoolContract]](../../Stored_Procedures/Securitization_spRefreshPoolContract.md)


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

