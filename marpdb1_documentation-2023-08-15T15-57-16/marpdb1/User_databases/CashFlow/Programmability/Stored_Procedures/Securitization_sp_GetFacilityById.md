#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_GetFacilityById

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_GetFacilityById]

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
| @Id | int | 4 |


---

## <a name="#sqlscript"></a>SQL Script

```sql

-- =============================================
-- Author:		Chad Feldmann
-- Create date: 09/04/2015
-- Description:	Gets all Facilities
--Test:  [Securitization].[sp_GetFacilityById] 
-- =============================================
CREATE PROCEDURE [Securitization].[sp_GetFacilityById]
	@Id int = 0
AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
SET NOCOUNT ON;

SELECT * FROM
Securitization.Facilities
WHERE Id = @Id

END

GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]
* [Securitization].[Facilities]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

