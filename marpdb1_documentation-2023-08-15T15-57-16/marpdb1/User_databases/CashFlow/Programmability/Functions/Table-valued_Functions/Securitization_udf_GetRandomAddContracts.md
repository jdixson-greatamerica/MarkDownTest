#### 

[Project](../../../../../../index.md) > [marpdb1](../../../../../index.md) > [User databases](../../../../index.md) > [CashFlow](../../../index.md) > Programmability > Functions > [Table-valued Functions](Table-valued_Functions.md) > Securitization.udf_GetRandomAddContracts

# ![Table-valued Functions](../../../../../../Images/Function_Table32.png) [Securitization].[udf_GetRandomAddContracts]

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
| @AmountToAdd | decimal(20,8) | 13 |
| @Pool_ID | int | 4 |
| @AdvanceRate | decimal(6,5) | 5 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
-- =============================================
-- Author:		cfeldman
-- Create date: 09/25/2015
-- Description:	Return table of contracts to be randomly added.
-- History:
--		Name:	Chad Feldmann
--		Date:	06/14/2019
--		Desc:	Ticket #PRJ0040256 - Changed reason to 'Random Cut'
-- =============================================
CREATE FUNCTION [Securitization].[udf_GetRandomAddContracts]
(
	@AmountToAdd decimal(20,8),
	@Pool_ID int,
	@AdvanceRate decimal(6,5)
)
returns
@RandomTotals TABLE
(
  ContractID char(15),
  [Guid] uniqueidentifier,
  NPV money,
  SubTotal money
)

AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	--SET NOCOUNT ON;

DECLARE @TotalAmountToAdd money;

IF @AmountToAdd = -1 
BEGIN
	SELECT @TotalAmountToAdd = sum(NPV*@AdvanceRate) 
	FROM Securitization.vActiveWorkingPoolContracts pc
	WHERE pc.Pool_ID = @Pool_ID;

	SELECT @TotalAmountToAdd = @TotalAmountToAdd - TargetPoolSize
	FROM Securitization.Pools p 
	WHERE p.ID = @Pool_ID;

END
ELSE
BEGIN
	 SET @TotalAmountToAdd = @AmountToAdd;
END

DECLARE @ContractID char(15);
DECLARE @Guid uniqueidentifier;
DECLARE @FundedAmount money;
DECLARE @SubTotal money;
--Select any contracts that have been removed.
DECLARE RandomAdd_Cursor CURSOR FOR
SELECT Contract_ContractID, [guid], coalesce(npv,0) * @AdvanceRate
from Securitization.PoolContract pc
inner join Securitization.Contracts c on pc.Contract_ContractID = c.ContractID
WHERE Pool_ID=@Pool_ID
AND Active=0
AND LOWER(Reason) in ('random','Random Cut')
order by [Guid]
OPEN RandomAdd_Cursor;
FETCH NEXT FROM RandomAdd_Cursor INTO @ContractID,@guid, @FundedAmount;
SET @SubTotal=0;

WHILE @@FETCH_STATUS = 0 
   BEGIN
		SET @SubTotal = @SubTotal + @FundedAmount;
		IF(@SubTotal <= @TotalAmountToAdd)
		BEGIN
			INSERT INTO  @RandomTotals(ContractID, Guid, NPV, SubTotal)
			VALUES(@ContractID, @Guid, @FundedAmount, @SubTotal);
			FETCH NEXT FROM RandomAdd_Cursor INTO @ContractID,@guid, @FundedAmount;
		END
		ELSE
		BEGIN
			BREAK;
		END
   END;
CLOSE RandomAdd_Cursor;
DEALLOCATE RandomAdd_Cursor;
return 
end
GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]
* [Securitization].[Contracts]
* [Securitization].[PoolContract]
* [Securitization].[Pools]
* [Securitization].[vActiveWorkingPoolContracts]


---

## <a name="#usedby"></a>Used By

* [[Securitization].[spGetRandomAddContracts]](../../Stored_Procedures/Securitization_spGetRandomAddContracts.md)
* [[Securitization].[spUpdateRandomAddContracts]](../../Stored_Procedures/Securitization_spUpdateRandomAddContracts.md)


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

