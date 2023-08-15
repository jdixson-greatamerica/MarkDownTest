#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_GetListById

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_GetListById]

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
| @Id | int | 4 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
SET QUOTED_IDENTIFIER OFF
GO
-- =============================================
-- Author:		John Thadison
-- Create date: 09/01/2015
-- Description:	returns lists 
--Test:  [Securitization].[sp_GetListById] 5
-- =============================================
Create PROCEDURE [Securitization].[sp_GetListById]
	-- Add the parameters for the stored procedure here
	@Id int = 0

AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;
	
	SELECT *
	FROM Securitization.Lists
	WHERE ID = @Id
	
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

