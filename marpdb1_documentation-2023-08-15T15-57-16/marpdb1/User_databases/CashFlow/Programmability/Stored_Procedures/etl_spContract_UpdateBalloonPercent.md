#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > etl.spContract_UpdateBalloonPercent

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [etl].[spContract_UpdateBalloonPercent]

---

## <a name="#properties"></a>Properties

| Property | Value |
|---|---|
| ANSI Nulls On | YES |
| Quoted Identifier On | YES |


---

## <a name="#sqlscript"></a>SQL Script

```sql
-- =============================================
-- Author:		 Ed Leighton-Dick
-- Create date:  June 2013
-- Description:	 Updates BalloonPercent field in Contract table
-- Dependencies: Cash Flow process
-- History:
--		Name: 
--		Date: 
--		Desc: 
-- =============================================

CREATE PROCEDURE [etl].[spContract_UpdateBalloonPercent] AS
BEGIN
	WITH allNonZeroCFX AS
		(SELECT ct.ContractKey, ct.Seq, SUM(TransactionAmount) AS TransactionAmount, ROW_NUMBER() OVER (PARTITION BY ct.ContractKey ORDER BY ct.ContractKey ASC, ct.Seq DESC) AS rowNbr
			FROM dbo.Contract_Transaction ct
				INNER JOIN raw.Contract c ON c.ContractKey = ct.ContractKey AND c.IsVariable = 1
				INNER JOIN dbo.TransactionType tt ON tt.Code = ct.TransactionType AND tt.IncludeInAdjustmentCalculation = 1
			GROUP BY ct.ContractKey, ct.Seq, ct.TransactionYear, ct.TransactionMonth
			HAVING SUM(TransactionAmount) <> 0.00),
		lastNonZeroCFX AS
		(SELECT ContractKey, Seq, TransactionAmount
			FROM allNonZeroCFX
			WHERE rowNbr = 1),
		balloonPercents AS
		(SELECT c.ContractID, l.ContractKey, l.TransactionAmount AS LastTransactionAmount, mt.TransactionAmount AS MaxTransactionAmount, c.EquipmentCost, CAST((l.TransactionAmount / c.EquipmentCost) * 100 AS decimal(4,1)) AS BalloonPercent
			FROM lastNonZeroCFX l
				CROSS APPLY (SELECT MAX(TransactionAmount) AS TransactionAmount FROM allNonZeroCFX WHERE ContractKey = l.ContractKey AND Seq <> l.Seq) mt
				INNER JOIN raw.Contract c ON c.ContractKey = l.ContractKey
			WHERE l.TransactionAmount > mt.TransactionAmount)
	UPDATE dbo.Contract
		SET BalloonPercent = b.BalloonPercent
		FROM dbo.Contract c
			LEFT OUTER JOIN balloonPercents b ON b.ContractKey = c.ContractKey
		WHERE NOT EXISTS (SELECT 'x' FROM etl.ErrorLog WHERE ContractKey = c.ContractKey);
END;

GO

```


---

## <a name="#uses"></a>Uses

* [dbo].[Contract]
* [dbo].[Contract_Transaction]
* [dbo].[TransactionType]
* [etl]
* [etl].[ErrorLog]
* [raw].[Contract]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

