#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.spGetRandomAddContracts

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[spGetRandomAddContracts]

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
| @AmountToAdd | decimal(20,8) | 13 |
| @Pool_ID | int | 4 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
SET QUOTED_IDENTIFIER OFF
GO

-- =============================================
-- Author:		Chad Feldmann
-- Create date: 09/25/2015
-- Description:	returns a random list of contracts for a given pool based on the amount the user chose to add.
--Test:  [Securitization].[spGetRandomAddContracts] 200000,61
-- =============================================
CREATE PROCEDURE [Securitization].[spGetRandomAddContracts]
	-- Add the parameters for the stored procedure here
	@AmountToAdd decimal(20,8),
	@Pool_ID int
AS
BEGIN

DECLARE @AdvanceRate decimal(6,5);

SELECT @AdvanceRate = AdvanceRate
FROM Securitization.Pools
WHERE ID = @Pool_ID

SELECT 
	c.*,  
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
FROM [Securitization].[udf_GetRandomAddContracts](@AmountToAdd,@Pool_ID,@AdvanceRate) as rt 
	INNER JOIN Securitization.Contracts as c on
	c.ContractID = rt.ContractID 
	INNER JOIN Securitization.PoolContract as pc on
	pc.Contract_ContractID = c.ContractID 
WHERE pc.Pool_ID = @Pool_ID



END
GO

```


---

## <a name="#uses"></a>Uses

* [[Securitization].[udf_GetRandomAddContracts]](../Functions/Table-valued_Functions/Securitization_udf_GetRandomAddContracts.md)
* [Securitization]
* [Securitization].[Contracts]
* [Securitization].[PoolContract]
* [Securitization].[Pools]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

