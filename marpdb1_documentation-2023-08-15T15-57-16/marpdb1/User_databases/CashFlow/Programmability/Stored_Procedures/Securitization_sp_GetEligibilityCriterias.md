#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_GetEligibilityCriterias

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_GetEligibilityCriterias]

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
-- Description:	Gets all EligibilityCriterias
--Test:  [Securitization].[sp_GetEligibilityCriterias] 26
--
--
-- Author:		John Thadison
-- Change date: 02/15/2022
-- Description:	check for "Active" eligibility criteria
-- =============================================
CREATE PROCEDURE [Securitization].[sp_GetEligibilityCriterias]
@FacilityId INT
AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
SET NOCOUNT ON;


SELECT * FROM
Securitization.EligibilityCriteria ec
INNER JOIN Securitization.EligibilityCriteriaFacilities ecf ON ecf.EligibilityCriteria_ID = ec.ID
WHERE ecf.Facility_ID=@FacilityId
AND ec.Active = 1
ORDER BY Name

END

GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]
* [Securitization].[EligibilityCriteria]
* [Securitization].[EligibilityCriteriaFacilities]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

