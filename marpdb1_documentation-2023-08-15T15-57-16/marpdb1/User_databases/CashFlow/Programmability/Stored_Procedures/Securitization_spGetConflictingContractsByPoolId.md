#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.spGetConflictingContractsByPoolId

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[spGetConflictingContractsByPoolId]

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
| @Pool_ID | int | 4 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
-- =============================================
-- Author:		Chad Feldmann
-- Create date: 04/29/2019
-- Description:	Ticket #PRJ0040256 - Get a listing of contracts that conflict with contracts in @Pool_ID
-- =============================================

CREATE PROCEDURE [Securitization].[spGetConflictingContractsByPoolId]
	@Pool_ID int
AS
BEGIN


select c.*,  
	pc.ID as PoolContractID, 
	pc.Active as PoolContractActive, 
	pc.Reason as PoolContractReason, 
	pc.InFacilityPool as PoolContractInFacilityPool,
	 pc.NPV as PoolContractNPV, 
	 pc.CreatedOn as PoolContractCreatedOn, 
	 pc.CreatedBy as PoolContractCreatedBy, 
	 pc.ModifiedOn as PoolContractModifiedOn, 
	 pc.ModifiedBy as PoolContractModifiedBy,  
	 Coalesce(pc.PendingStatus,
	 pc.Active) as PoolContractPendingStatus , 
	 pc.Pool_ID
	 
from Securitization.Pools p
INNER JOIN Securitization.PoolContract pc on p.ID = pc.Pool_ID
INNER JOIN Securitization.Contracts c on c.ContractID = pc.Contract_ContractID
inner join Securitization.udf_GetConflictingWorkingContracts(@Pool_ID) conf on conf.ContractID=pc.Contract_ContractID
WHERE p.id = @Pool_ID

END
GO

```


---

## <a name="#uses"></a>Uses

* [[Securitization].[udf_GetConflictingWorkingContracts]](../Functions/Table-valued_Functions/Securitization_udf_GetConflictingWorkingContracts.md)
* [Securitization]
* [Securitization].[Contracts]
* [Securitization].[PoolContract]
* [Securitization].[Pools]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

