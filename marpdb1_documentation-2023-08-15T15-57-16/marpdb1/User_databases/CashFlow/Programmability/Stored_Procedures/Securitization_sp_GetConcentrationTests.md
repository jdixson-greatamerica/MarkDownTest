#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_GetConcentrationTests

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_GetConcentrationTests]

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
| @FacilityId | int | 4 |


---

## <a name="#sqlscript"></a>SQL Script

```sql

-- =============================================
-- Author:		Chad Feldmann
-- Create date: 09/04/2015
-- Description:	Gets all ConcentrationTests
--Test:  [Securitization].[sp_GetConcentrationTests] 
-- =============================================
CREATE PROCEDURE [Securitization].[sp_GetConcentrationTests]
@FacilityId INT
AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
SET NOCOUNT ON;
SELECT 
	ct.ID,
	ct.Name, 
	ct.LogicDescription, 
	fl.ID AS FacilityLimitID,
	fl.Type, 
	fl.Percentage, 
	fl.Amount, 
	fl.ModifiedBy, 
	fl.ModifiedOn
FROM
	Securitization.ConcentrationTests ct
	INNER JOIN Securitization.FacilityConcentration fc ON fc.ConcentrationTest_ID = ct.ID
	INNER JOIN Securitization.FacilityLimits fl ON fl.ConcentrationTest_ID = ct.ID	
WHERE fc.Facility_ID = @FacilityId
AND fl.Facility_ID	= @FacilityId
ORDER BY Name

END

GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]
* [Securitization].[ConcentrationTests]
* [Securitization].[FacilityConcentration]
* [Securitization].[FacilityLimits]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

