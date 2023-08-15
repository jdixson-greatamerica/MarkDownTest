#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.spGetIsContractAddDisabled

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[spGetIsContractAddDisabled]

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
| @ID | int | 4 |


---

## <a name="#sqlscript"></a>SQL Script

```sql

-- =============================================
-- Author:		Scott Monroe
-- Create date: 10/28/2013
-- Description:	
-- History:
--
--		Name:			
--		Date:			
--		Description:	
-- =============================================

CREATE PROCEDURE [Securitization].[spGetIsContractAddDisabled] 
	@ID int
AS
BEGIN

	SELECT IsContractAddDisabled
	FROM Securitization.Pools
	WHERE ID = @ID

END

GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]
* [Securitization].[Pools]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

