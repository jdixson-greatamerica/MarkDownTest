#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_InsertListItem

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_InsertListItem]

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
| @ListID | int | 4 |
| @DisplayOrder | int | 4 |
| @ItemDescription | nvarchar(256) | 512 |
| @ItemText | nvarchar(50) | 100 |
| @ModifiedBy | nvarchar(50) | 100 |


---

## <a name="#sqlscript"></a>SQL Script

```sql

-- =============================================
-- Author:		Chad Feldmann
-- Create date: 09/04/2015
-- Description:	Insert a listitem
--Test:  [Securitization].[sp_InsertListItem] 
-- =============================================
CREATE PROCEDURE [Securitization].[sp_InsertListItem]
	-- Add the parameters for the stored procedure here
	@ListID int,
	@DisplayOrder int,
	@ItemDescription nvarchar(256),
	@ItemText nvarchar(50),
	@ModifiedBy nvarchar(50)

AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;

	Insert Securitization.ListItems(
	CreatedBy, 
	CreatedOn, 
	ModifiedBy, 
	ModifiedOn,
	ItemText, 
	ItemDescription, 
	DisplayOrder,
	List_Id)
	VALUES(@ModifiedBy,
		GETDATE(),
		@ModifiedBy,
		GETDATE(),
		@ItemText,
		@ItemDescription,
		@DisplayOrder,
		@ListId)

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

