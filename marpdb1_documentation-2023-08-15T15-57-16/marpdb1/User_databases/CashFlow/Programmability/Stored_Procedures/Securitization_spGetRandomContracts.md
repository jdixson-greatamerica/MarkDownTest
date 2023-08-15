#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.spGetRandomContracts

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[spGetRandomContracts]

---

## <a name="#properties"></a>Properties

| Property | Value |
|---|---|
| ANSI Nulls On | YES |
| Quoted Identifier On | NO |


---

## <a name="#parameters"></a>Parameters

| Name | Data Type | Max Length (Bytes) |
|---|---|---|
| @AmountToCut | decimal(20,8) | 13 |
| @Pool_ID | int | 4 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
SET QUOTED_IDENTIFIER OFF
GO

-- =============================================
-- Author:		Chad Feldmann
-- Create date: 05/07/2013
-- Description:	returns a random list of contracts for a given pool based on the amount the user chose to cut.
--Test:  [Securitization].[spGetRandomContracts] 200000,61
-- =============================================
create PROCEDURE [Securitization].[spGetRandomContracts]
	-- Add the parameters for the stored procedure here
	@AmountToCut decimal(20,8),
	@Pool_ID int
AS
BEGIN
DECLARE @AdvanceRate decimal(6,5);

SELECT @AdvanceRate = AdvanceRate
FROM Securitization.Pools
WHERE ID = @Pool_ID

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
		from [Securitization].[udf_GetRandomContracts](@AmountToCut,@Pool_ID,@AdvanceRate) as rt 
inner join Securitization.Contracts as c on
c.ContractID = rt.ContractID 
inner join Securitization.PoolContract as pc on
pc.Contract_ContractID = c.ContractID 
inner join Securitization.Pools as p on
pc.Pool_ID = p.ID where p.ID = @Pool_ID



end
GO

```


---

## <a name="#uses"></a>Uses

* [[Securitization].[udf_GetRandomContracts]](../Functions/Table-valued_Functions/Securitization_udf_GetRandomContracts.md)
* [Securitization]
* [Securitization].[Contracts]
* [Securitization].[PoolContract]
* [Securitization].[Pools]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

