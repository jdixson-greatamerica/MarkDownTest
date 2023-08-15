#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_UpdateListItem

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_UpdateListItem]

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
| @DisplayOrder | int | 4 |
| @ItemDescription | nvarchar(256) | 512 |
| @ModifiedBy | nvarchar(50) | 100 |


---

## <a name="#sqlscript"></a>SQL Script

```sql

-- =============================================
-- Author:		Chad Feldmann
-- Create date: 09/04/2015
-- Description:	Updates a listitem
--Test:  [Securitization].[sp_UpdateListItem] 
-- =============================================
CREATE PROCEDURE [Securitization].[sp_UpdateListItem]
	-- Add the parameters for the stored procedure here
	@ID int,
	@DisplayOrder int,
	@ItemDescription nvarchar(256),
	@ModifiedBy nvarchar(50)

AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;

	UPDATE Securitization.ListItems
	SET 
		
		ModifiedBy = @ModifiedBy,
		ModifiedOn = GETDATE(),
		DisplayOrder = @DisplayOrder,
		ItemDescription = @ItemDescription
	
	WHERE ID = @ID;

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

