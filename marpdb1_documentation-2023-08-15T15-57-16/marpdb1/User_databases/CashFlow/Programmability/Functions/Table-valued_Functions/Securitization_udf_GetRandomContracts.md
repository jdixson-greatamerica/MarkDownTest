#### 

[Project](../../../../../../index.md) > [marpdb1](../../../../../index.md) > [User databases](../../../../index.md) > [CashFlow](../../../index.md) > Programmability > Functions > [Table-valued Functions](Table-valued_Functions.md) > Securitization.udf_GetRandomContracts

# ![Table-valued Functions](../../../../../../Images/Function_Table32.png) [Securitization].[udf_GetRandomContracts]

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
| @AmountToCut | decimal(20,8) | 13 |
| @Pool_ID | int | 4 |
| @AdvanceRate | decimal(6,5) | 5 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
CREATE FUNCTION [Securitization].[udf_GetRandomContracts]
(
	@AmountToCut decimal(20,8),
	@Pool_ID int,
	@AdvanceRate decimal(6,5)
)
returns
@RandomTotals TABLE
(
  ContractID char(15),
  Guid uniqueidentifier,
  NPV money,
  SubTotal money
)

AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	--SET NOCOUNT ON;

DECLARE @TotalAmountToCut money;

IF @AmountToCut = -1 
BEGIN
	SELECT @TotalAmountToCut = sum(NPV*@AdvanceRate) 
	FROM Securitization.vActiveWorkingPoolContracts pc
	WHERE pc.Pool_ID = @Pool_ID;

	SELECT @TotalAmountToCut = @TotalAmountToCut - TargetPoolSize
	FROM Securitization.Pools p 
	WHERE p.ID = @Pool_ID;

END
ELSE
BEGIN
	 SET @TotalAmountToCut = @AmountToCut;
END

DECLARE @ContractID char(15);
DECLARE @Guid uniqueidentifier;
DECLARE @FundedAmount money;
DECLARE @SubTotal money;

DECLARE RandomCut_Cursor CURSOR FOR
SELECT Contract_ContractID, guid, coalesce(npv,0) * @AdvanceRate
from Securitization.vActiveWorkingPoolContracts
WHERE Pool_ID=@Pool_ID
order by Guid
OPEN RandomCut_Cursor;
FETCH NEXT FROM RandomCut_Cursor INTO @ContractID,@guid, @FundedAmount;
SET @SubTotal=0;

WHILE @@FETCH_STATUS = 0 
   BEGIN
		SET @SubTotal = @SubTotal + @FundedAmount;
		IF(@SubTotal <= @TotalAmountToCut)
		BEGIN
			INSERT INTO  @RandomTotals(ContractID, Guid, NPV, SubTotal)
			VALUES(@ContractID, @Guid, @FundedAmount, @SubTotal);
			FETCH NEXT FROM RandomCut_Cursor INTO @ContractID,@guid, @FundedAmount;
		END
		ELSE
		BEGIN
			BREAK;
		END
   END;
CLOSE RandomCut_Cursor;
DEALLOCATE RandomCut_Cursor;
return 
end
GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]
* [Securitization].[Pools]
* [Securitization].[vActiveWorkingPoolContracts]


---

## <a name="#usedby"></a>Used By

* [[Securitization].[spGetRandomContracts]](../../Stored_Procedures/Securitization_spGetRandomContracts.md)
* [[Securitization].[spUpdateRandomContracts]](../../Stored_Procedures/Securitization_spUpdateRandomContracts.md)


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

