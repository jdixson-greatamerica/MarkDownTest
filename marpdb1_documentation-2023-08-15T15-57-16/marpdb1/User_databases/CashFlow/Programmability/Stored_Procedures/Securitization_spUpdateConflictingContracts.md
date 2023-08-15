#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.spUpdateConflictingContracts

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[spUpdateConflictingContracts]

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
| @Pool_ID | int | 4 |
| @PendingStatus | int | 4 |
| @Reason | varchar(50) | 50 |
| @ModifiedBy | nvarchar(50) | 100 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
SET QUOTED_IDENTIFIER OFF
GO

-- =============================================
-- Author:		Chad Feldmann
-- Create date: 04/29/2019
-- Description:	Ticket #PRJ0040256 - Updates the pending status flag for conflicting contracts
-- =============================================
CREATE PROCEDURE [Securitization].[spUpdateConflictingContracts]
	-- Add the parameters for the stored procedure here
	@Pool_ID int,
	@PendingStatus int,
	@Reason varchar(50),
	@ModifiedBy nvarchar(50)

AS
BEGIN

iF @PendingStatus = 0
	BEGIN
		UPDATE Securitization.PoolContract
		SET PendingStatus = @PendingStatus, 
			Reason = @Reason,
			Modifiedby = @ModifiedBy,
			ModifiedOn = GetDate()
		WHERE Securitization.PoolContract.Contract_ContractID in (select ContractID from [Securitization].[udf_GetConflictingWorkingContracts](@Pool_ID))
			AND Pool_ID = @Pool_ID
			
	END
if @PendingStatus = 1
	BEGIN
		UPDATE Securitization.PoolContract
		SET PendingStatus = @PendingStatus,
			Reason = null,
			Modifiedby = @ModifiedBy,
			ModifiedOn = GetDate()
		where Securitization.PoolContract.Contract_ContractID in (select ContractID from [Securitization].[udf_GetConflictingWorkingContracts](@Pool_ID))
		AND Pool_ID = @Pool_ID
	END

SELECT c.*, pc.Active as PoolContractActive, pc.NPV as PoolContractNPV, pc.ID as PoolContractID, Coalesce(pc.PendingStatus,pc.Active) as PoolContractPendingStatus 
FROM [Securitization].[udf_GetConflictingWorkingContracts](@Pool_ID) as rt 
INNER JOIN Securitization.Contracts as c on
	c.ContractID = rt.ContractID 
INNER JOIN Securitization.PoolContract as pc on
	pc.Contract_ContractID = c.ContractID 
INNER JOIN Securitization.Pools as p on
pc.Pool_ID = p.ID 
where p.ID = @Pool_ID;

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

