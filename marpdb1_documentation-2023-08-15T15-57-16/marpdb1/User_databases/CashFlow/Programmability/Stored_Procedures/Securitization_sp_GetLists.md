#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_GetLists

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_GetLists]

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
-- Author:		Chad Feldmann
-- Create date: 09/04/2015
-- Description:	Gets all Lists
--Test:  [Securitization].[sp_GetLists] 
-- =============================================
CREATE PROCEDURE [Securitization].[sp_GetLists]

AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
SET NOCOUNT ON;

SELECT * FROM
Securitization.Lists
ORDER BY Name

END

GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]
* [Securitization].[Lists]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

