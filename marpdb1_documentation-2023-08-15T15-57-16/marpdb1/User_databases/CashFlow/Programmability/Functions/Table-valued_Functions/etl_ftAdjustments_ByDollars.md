#### 

[Project](../../../../../../index.md) > [marpdb1](../../../../../index.md) > [User databases](../../../../index.md) > [CashFlow](../../../index.md) > Programmability > Functions > [Table-valued Functions](Table-valued_Functions.md) > etl.ftAdjustments_ByDollars

# ![Table-valued Functions](../../../../../../Images/Function_Table32.png) [etl].[ftAdjustments_ByDollars]

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
| @ContractKey | bigint | 8 |
| @Amount | money | 8 |


---

## <a name="#sqlscript"></a>SQL Script

```sql

-- =============================================
-- Author:		Ed Leighton-Dick
-- Create date: Apr 2013
-- Description:	Calculates transactions for adjustments to the cash flow, such as security deposits
--				Removes specified dollar amount from ending payments until dollar amount exhausted
-- Dependencies:Contract_Transaction
-- History:
--		Name:
--		Date:
--		Desc:
-- =============================================

CREATE FUNCTION [etl].[ftAdjustments_ByDollars] (@ContractKey bigint, @Amount money) 
	RETURNS @adjTransactions TABLE ([year] int, [month] tinyint, seq int, amount money)
AS BEGIN
	DECLARE @curMonth tinyint, @curYear int, @curSeq int, @curAmt money;
	DECLARE @rem money = @Amount;
	
	IF @Amount > 0.00
	BEGIN
		DECLARE cMonths CURSOR STATIC READ_ONLY FOR
			SELECT x.TransactionYear, x.TransactionMonth, x.Seq, SUM(x.TransactionAmount)
				FROM etl.Contract_Transaction x
					INNER JOIN dbo.TransactionType t ON t.Code = x.TransactionType AND t.IncludeInAdjustmentCalculation = 1
				WHERE x.ContractKey = @ContractKey
				GROUP BY x.TransactionYear, x.TransactionMonth, x.Seq
				ORDER BY TransactionYear DESC, TransactionMonth DESC;
				
		OPEN cMonths;
		FETCH NEXT FROM cMonths INTO @curYear, @curMonth, @curSeq, @curAmt;
		
		WHILE @@FETCH_STATUS = 0 AND @rem > 0.00
		BEGIN
			IF @curAmt > 0.00
			BEGIN
				INSERT INTO @adjTransactions ([year], [month], seq, amount)
					VALUES (@curYear, @curMonth, @curSeq, (CASE WHEN @curAmt <= @rem THEN @curAmt ELSE @rem END));
				
				SET @rem = @rem - @curAmt;
			END;

			FETCH NEXT FROM cMonths INTO @curYear, @curMonth, @curSeq, @curAmt;
		END;
	END;
			
	RETURN;
END;
GO

```


---

## <a name="#uses"></a>Uses

* [dbo].[TransactionType]
* [etl]
* [etl].[Contract_Transaction]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

