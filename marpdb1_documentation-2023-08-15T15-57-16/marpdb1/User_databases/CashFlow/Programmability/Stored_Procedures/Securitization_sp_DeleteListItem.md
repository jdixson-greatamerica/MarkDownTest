#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_DeleteListItem

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_DeleteListItem]

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
| @DeletedBy | nvarchar(50) | 100 |


---

## <a name="#sqlscript"></a>SQL Script

```sql

-- =============================================
-- Author:		Chad Feldmann
-- Create date: 09/04/2015
-- Description:	delete a listitem
--Test:  [Securitization].[sp_DeleteListItem] 
-- =============================================
CREATE PROCEDURE [Securitization].[sp_DeleteListItem]
	-- Add the parameters for the stored procedure here
	@ID int,
	@DeletedBy nvarchar(50)
AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;
	--Needed by delete trigger
	DECLARE @name VARBINARY(128) 
	SET @name = CAST(@DeletedBy	AS VARBINARY(128));
	SET CONTEXT_INFO @name /*Set it*/

	DELETE Securitization.ListItems
	WHERE Id = @ID;

END


GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]
* [Securitization].[ListItems]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

