#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.spSetIsContractAddDisabled

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[spSetIsContractAddDisabled]

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
| @IsContractAddDisabled | bit | 1 |
| @ID | int | 4 |


---

## <a name="#sqlscript"></a>SQL Script

```sql

-- =============================================
-- Author:		Scott Monroe
-- Create date: 10/25/2013
-- Description:	
-- History:
--
--		Name:			
--		Date:			
--		Description:	
-- =============================================

CREATE PROCEDURE [Securitization].[spSetIsContractAddDisabled] 
	@IsContractAddDisabled BIT,
	@ID INT
AS
BEGIN

	UPDATE Securitization.Pools
	SET IsContractAddDisabled = @IsContractAddDisabled
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

