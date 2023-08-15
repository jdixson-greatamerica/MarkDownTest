#### 

[Project](../../../../../../index.md) > [marpdb1](../../../../../index.md) > [User databases](../../../../index.md) > [CashFlow](../../../index.md) > Programmability > Functions > [Table-valued Functions](Table-valued_Functions.md) > etl.ftAdjustments_ByNbrPayments

# ![Table-valued Functions](../../../../../../Images/Function_Table32.png) [etl].[ftAdjustments_ByNbrPayments]

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
| @nbrPayments | tinyint | 1 |
| @allowAdjToZero | bit | 1 |


---

## <a name="#sqlscript"></a>SQL Script

```sql

-- =============================================
-- Author:		Ed Leighton-Dick
-- Create date: Apr 2013
-- Description:	Calculates transactions for adjustments to the cash flow, such as advances
--				Removes specified number of payments from the end of the cash flow
-- Dependencies:Contract_Transaction
-- History:
--		Name: Ed Leighton-Dick
--		Date: 6/3/2013
--		Desc: Allow adjustments when the amount is not zero
-- =============================================

CREATE FUNCTION [etl].[ftAdjustments_ByNbrPayments] (@ContractKey bigint, @nbrPayments tinyint, @allowAdjToZero bit)
	RETURNS @adjTransactions TABLE ([year] int, [month] tinyint, seq int, amount money)
AS BEGIN
	DECLARE @curMonth tinyint, @curYear int, @curSeq int, @curAmt money, @hasAdjustments tinyint;
	DECLARE @rem tinyint = @nbrPayments;
	
	IF @nbrPayments > 0
	BEGIN
		DECLARE cMonths CURSOR STATIC READ_ONLY FOR
			SELECT x.TransactionYear, x.TransactionMonth, x.Seq, SUM(x.TransactionAmount), SUM(CASE WHEN x.TransactionType IN (3, 4) THEN 1 ELSE 0 END) AS hasAdjustments
				FROM etl.Contract_Transaction x
					INNER JOIN dbo.TransactionType t ON t.Code = x.TransactionType AND t.IncludeInAdjustmentCalculation = 1
				WHERE x.ContractKey = @ContractKey
				GROUP BY x.TransactionYear, x.TransactionMonth, x.Seq
				ORDER BY TransactionYear DESC, TransactionMonth DESC;
				
		OPEN cMonths;
		FETCH NEXT FROM cMonths INTO @curYear, @curMonth, @curSeq, @curAmt, @hasAdjustments;
		
		WHILE @@FETCH_STATUS = 0 AND @rem > 0
		BEGIN
			-- Allow adjustments when the amount is not zero or when there are no existing adjustments to that transaction
			IF ((@curAmt > 0.00) OR 
				(@allowAdjToZero = 1 AND @hasAdjustments = 0))
			BEGIN
				INSERT INTO @adjTransactions ([year], [month], seq, amount)
					VALUES (@curYear, @curMonth, @curSeq, @curAmt);
				
				SET @rem = @rem - 1;
			END;

			FETCH NEXT FROM cMonths INTO @curYear, @curMonth, @curSeq, @curAmt, @hasAdjustments;
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

