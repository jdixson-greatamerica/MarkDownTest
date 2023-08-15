#### 

[Project](../../../../../../index.md) > [marpdb1](../../../../../index.md) > [User databases](../../../../index.md) > [CashFlow](../../../index.md) > Programmability > Functions > [Table-valued Functions](Table-valued_Functions.md) > Securitization.udf_GetConflictingWorkingContracts

# ![Table-valued Functions](../../../../../../Images/Function_Table32.png) [Securitization].[udf_GetConflictingWorkingContracts]

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

CREATE FUNCTION [Securitization].[udf_GetConflictingWorkingContracts]
(
	@Pool_ID int
)
returns
@ConflictingContracts TABLE
(
  ContractID char(15)
)

AS
BEGIN
--Get a listing of active contracts in other active pools
	WITH PotentialConflictsInActivePools (ContractID) AS
	(
	SELECT DISTINCT Contract_ContractID FROM Securitization.PoolContract pc
	INNER JOIN Securitization.Pools p on p.id = pc.Pool_ID
	where p.Status = 1 --1 is active and 2 is archived
	AND p.id <> @Pool_ID --Contracts not in this pool
	AND Coalesce(pc.PendingStatus,pc.Active) = 1 --Only check active contracts
	AND p.Funding = 1 --Only check on funding pools
	)

--Add conflicting contracts
	INSERT INTO @ConflictingContracts(ContractID)
	SELECT conf.ContractID
	FROM Securitization.Pools p 
	INNER JOIN Securitization.PoolContract pc on p.ID = pc.Pool_ID
	INNER JOIN PotentialConflictsInActivePools conf on pc.Contract_ContractID=conf.ContractID
	WHERE 
	p.id = @Pool_ID
	AND Coalesce(pc.Active,0) = 1; --Only check active contracts


--Get a listing of active contracts in other archived pools
	WITH PotentialConflictsInArchivedPools (ContractID) AS
	(
	SELECT DISTINCT pc.ContractID FROM Archive.PoolExport pc
	INNER JOIN Securitization.Pools p on p.id = pc.Pool_ID
	where p.Status = 2 --1 is active and 2 is archived
	AND p.id <> @Pool_ID --Contracts not in this pool
	AND pc.Active = 1 --Only check active contracts
	AND p.Funding = 1 --Only check on funding pools
	)

--Add conflicting contracts
	INSERT INTO @ConflictingContracts(ContractID)
	SELECT conf.ContractID
	FROM Securitization.Pools p 
	INNER JOIN Securitization.PoolContract pc on p.ID = pc.Pool_ID
	INNER JOIN PotentialConflictsInArchivedPools conf on pc.Contract_ContractID=conf.ContractID
	WHERE 
	p.id = @Pool_ID
	AND Coalesce(pc.PendingStatus,pc.Active) = 1 --Only check active contracts


return 
end
GO

```


---

## <a name="#uses"></a>Uses

* [Archive].[PoolExport]
* [Securitization]
* [Securitization].[PoolContract]
* [Securitization].[Pools]


---

## <a name="#usedby"></a>Used By

* [[Securitization].[spGetConflictingContractsByPoolId]](../../Stored_Procedures/Securitization_spGetConflictingContractsByPoolId.md)
* [[Securitization].[spUpdateConflictingContracts]](../../Stored_Procedures/Securitization_spUpdateConflictingContracts.md)


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

