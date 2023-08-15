#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.spUpdateRandomAddContracts

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[spUpdateRandomAddContracts]

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
-- Create date: 09/25/2015
-- Description:	Updates the pending status flag for the top x contracts that meed the amount to add criteria
--Test:  [Securitization].[spUpdateRandomAddContracts] 2000000,60,0
-- =============================================
CREATE PROCEDURE [Securitization].[spUpdateRandomAddContracts]
	-- Add the parameters for the stored procedure here
	@AmountToAdd decimal(20,8),
	@Pool_ID int,
	@PendingStatus int,
	@Reason varchar(50),
	@ModifiedBy nvarchar(50)

AS
BEGIN

DECLARE @AdvanceRate decimal(6,5);

SELECT @AdvanceRate = AdvanceRate
FROM Securitization.Pools
WHERE ID = @Pool_ID

IF @PendingStatus = 0
	BEGIN
		UPDATE Securitization.PoolContract
		SET PendingStatus = @PendingStatus, 
			Reason = @Reason,
			Modifiedby = @ModifiedBy,
			ModifiedOn = GetDate()
		WHERE Securitization.PoolContract.Contract_ContractID IN 
			(SELECT rt.ContractID FROM [Securitization].[udf_GetRandomAddContracts](@AmountToAdd,@Pool_ID,@AdvanceRate) as rt)
			AND Pool_ID = @Pool_ID
	END
IF @PendingStatus = 1
	BEGIN
		UPDATE Securitization.PoolContract
		SET PendingStatus = @PendingStatus,
			Modifiedby = @ModifiedBy,
			ModifiedOn = GetDate()
		WHERE Securitization.PoolContract.Contract_ContractID in 
			(SELECT rt.ContractID FROM [Securitization].[udf_GetRandomAddContracts](@AmountToAdd,@Pool_ID,@AdvanceRate) as rt)
			AND Pool_ID = @Pool_ID
	END

	SELECT 
		c.*, 
		pc.Active as PoolContractActive, 
		pc.NPV as PoolContractNPV, 
		pc.ID as PoolContractID, 
		Coalesce(pc.PendingStatus,pc.Active) as PoolContractPendingStatus 
	FROM [Securitization].[udf_GetRandomAddContracts](@AmountToAdd,@Pool_ID,@AdvanceRate) as rt 
	INNER JOIN Securitization.Contracts as c on
		c.ContractID = rt.ContractID 
	INNER JOIN Securitization.PoolContract as pc on
		pc.Contract_ContractID = c.ContractID 
	INNER JOIN Securitization.Pools as p on
		pc.Pool_ID = p.ID 
	WHERE p.ID = @Pool_ID;

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

