#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_GetListItemsByListShortName

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_GetListItemsByListShortName]

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
| @ShortName | nvarchar(255) | 510 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
SET QUOTED_IDENTIFIER OFF
GO
-- =============================================
-- Author:		John Thadison
-- Create date: 09/01/2015
-- Description:	returns lists and list items
--Test:  [Securitization].[sp_GetListItemsByListShortName] 'state'
--		Name:	Chad Feldmann
--		Date:	5/30/2019
--		Desc:	Ticket #PRJ0040256 - Updated to sort by DisplayOrder and ItemText
-- =============================================
CREATE PROCEDURE [Securitization].[sp_GetListItemsByListShortName]
	-- Add the parameters for the stored procedure here
	@ShortName nvarchar(255)

AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;

	SELECT li.*,
	l.MODIFIABLE AS ListModifiable
	FROM Securitization.Lists l
	INNER JOIN Securitization.ListItems li 
		ON li.List_ID = l.Id
	WHERE UPPER(l.SHORTNAME) = @ShortName
	order by li.DisplayOrder, li.ItemText
	
END
GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]
* [Securitization].[ListItems]
* [Securitization].[Lists]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

