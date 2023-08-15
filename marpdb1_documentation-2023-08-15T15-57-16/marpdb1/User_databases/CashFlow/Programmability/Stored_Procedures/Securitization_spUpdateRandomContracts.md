#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.spUpdateRandomContracts

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[spUpdateRandomContracts]

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
-- Edited By:	Logan Keenan
-- Create date: 05/07/2013
-- Description:	Updates the pending status flag for the top x contracts that meed the amount to cut criteria
--Test:  [Securitization].[spUpdateRandomContracts] 2000000,60,0
-- =============================================
CREATE PROCEDURE [Securitization].[spUpdateRandomContracts]
	-- Add the parameters for the stored procedure here
	@AmountToCut decimal(20,8),
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

if @PendingStatus = 0
	begin
		Update Securitization.PoolContract
		SET PendingStatus = @PendingStatus, 
			Reason = @Reason,
			Modifiedby = @ModifiedBy,
			ModifiedOn = GetDate()
		where Securitization.PoolContract.Contract_ContractID in (select rt.ContractID from [Securitization].[udf_GetRandomContracts](@AmountToCut,@Pool_ID,@AdvanceRate) as rt)
		AND Pool_ID = @Pool_ID
	end
if @PendingStatus = 1
	begin
		Update Securitization.PoolContract
		SET PendingStatus = @PendingStatus, 
			Reason = null,
			Modifiedby = @ModifiedBy,
			ModifiedOn = GetDate()
		where Securitization.PoolContract.Contract_ContractID in (select rt.ContractID from [Securitization].[udf_GetRandomContracts](@AmountToCut,@Pool_ID,@AdvanceRate) as rt)
		AND Pool_ID = @Pool_ID
	end

select c.*, pc.Active as PoolContractActive, pc.NPV as PoolContractNPV, pc.ID as PoolContractID, Coalesce(pc.PendingStatus,pc.Active) as PoolContractPendingStatus from [Securitization].[udf_GetRandomContracts](@AmountToCut,@Pool_ID,@AdvanceRate) as rt 
inner join Securitization.Contracts as c on
c.ContractID = rt.ContractID 
inner join Securitization.PoolContract as pc on
pc.Contract_ContractID = c.ContractID 
inner join Securitization.Pools as p on
pc.Pool_ID = p.ID 
where p.ID = @Pool_ID;



END



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

