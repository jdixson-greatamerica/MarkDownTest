#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > etl.spContract_UpdateCashFlowTest

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [etl].[spContract_UpdateCashFlowTest]

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
| @RunDate | date | 3 |


---

## <a name="#sqlscript"></a>SQL Script

```sql

-- =============================================
-- Author:		 Ed Leighton-Dick
-- Create date:  May 2013
-- Description:	 Calculates the CashFlowTest field in the Contract table
-- Dependencies: dbo.Contract, raw.Contract, dbo.Contract_Transaction, successful load of Contract_Transaction
-- History:
--		Name: Chuckles
--		Date: 01/30/2014
--		Desc: Added second CTE to do the calculation so we can put min/max values on the calc result
--		Name: 
--		Date: 
--		Desc: 
--		TESTING SCRIPT
--		DECLARE @currentDate datetime=GETDATE()
--
--							exec etl.spContract_UpdateCashFlowTest @currentDate
-- =============================================

CREATE PROCEDURE [etl].[spContract_UpdateCashFlowTest] (@RunDate date) AS
BEGIN
	  IF @RunDate = '1/1/1900'
                                SET @RunDate = GETDATE();
 
                WITH CashFlow AS
                                (SELECT c.ContractKey, SUM(ct.TransactionAmount) AS TotalCashFlows
                                                FROM dbo.Contract_Transaction ct
                                                     INNER JOIN dbo.TransactionType tt ON tt.Code = ct.TransactionType
													 INNER JOIN Securitization.Contracts c ON c.ContractKey = ct.ContractKey	
                                                WHERE tt.IncludeInAdjustmentCalculation = 1
                                                  AND (ct.TransactionYear > YEAR(@RunDate) OR (ct.TransactionYear = YEAR(@RunDate) AND ct.TransactionMonth >= MONTH(@RunDate)))
                                                GROUP BY c.ContractKey),
                                CashFlowCalculation AS
                                (
                                SELECT COALESCE(((cf.TotalCashFlows - c.CBR + (ROUND(COALESCE(r.SecurityDeposit,0) / r.Payment, 0) * r.Payment)) / r.Payment),0) AS CashFlowTest, c.ContractKey AS ContractKey
                                                                FROM Securitization.Contracts c
                                                LEFT OUTER JOIN raw.Contract r ON r.ContractKey = c.ContractKey	 AND r.Payment <> 0.00 
                                                LEFT OUTER JOIN CashFlow cf ON cf.ContractKey	= r.ContractKey		
                                               
                                )

								UPDATE Securitization.Contracts
								SET
								    CashflowTest = cc.CashFlowTest	
								FROM Securitization.Contracts c 
								inner join CashFlowCalculation cc ON cc.ContractKey	= c.ContractKey		
END;

GO

```


---

## <a name="#uses"></a>Uses

* [dbo].[Contract_Transaction]
* [dbo].[TransactionType]
* [etl]
* [raw].[Contract]
* [Securitization].[Contracts]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

