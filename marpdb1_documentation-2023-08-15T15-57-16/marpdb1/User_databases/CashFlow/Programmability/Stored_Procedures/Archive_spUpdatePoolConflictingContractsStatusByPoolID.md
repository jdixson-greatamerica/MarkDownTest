#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Archive.spUpdatePoolConflictingContractsStatusByPoolID

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Archive].[spUpdatePoolConflictingContractsStatusByPoolID]

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
| @PoolId | int | 4 |
| @UserID | nvarchar(50) | 100 |


---

## <a name="#sqlscript"></a>SQL Script

```sql




-- =============================================
-- Author:		Chad Feldmann
-- Create date: 09/17/2013
-- Description:	Used to remove conflicting contracts from an archived pool.
--Test:  [Archive].[spUpdatePoolContractsPendingStatusByPoolIdQuery] 1,60,''
-- =============================================
create procedure [Archive].[spUpdatePoolConflictingContractsStatusByPoolID]

	@Status int,
	@PoolId int,
	@UserID nvarchar(50)
	as 
BEGIN

	with PotentialConflicts (ContractID) AS
	(
	SELECT distinct Contract_ContractID from Securitization.PoolContract pc
	inner join Securitization.Pools p on p.id = pc.Pool_ID
	WHERE p.Status in(1,2)
	and p.id <> @PoolId --Contracts not in this pool
	and pc.InFacilityPool=0 --Only check contracts in working pool
	and Coalesce(pc.PendingStatus,pc.Active) = 1 --Only check active contracts
	and p.Funding = 1 --Only check on funding pools
	)

	UPDATE Archive.PoolExport 
	SET Active = @Status,
	ModifiedBy = @UserID,
	ModifiedOn = GETDATE(),
	Reason = 'Conflicting Contract'

	FROM Archive.PoolExport e
	INNER JOIN PotentialConflicts conf on conf.ContractID=e.ContractID
	WHERE 
	e.Pool_ID = @PoolId 
	and conf.ContractID is not null

END





GO

```


---

## <a name="#uses"></a>Uses

* [Archive]
* [Archive].[PoolExport]
* [Securitization].[PoolContract]
* [Securitization].[Pools]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

