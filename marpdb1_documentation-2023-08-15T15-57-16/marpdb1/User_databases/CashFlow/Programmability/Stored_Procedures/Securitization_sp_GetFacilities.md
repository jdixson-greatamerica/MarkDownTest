#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_GetFacilities

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_GetFacilities]

---

## <a name="#properties"></a>Properties

| Property | Value |
|---|---|
| ANSI Nulls On | YES |
| Quoted Identifier On | YES |


---

## <a name="#sqlscript"></a>SQL Script

```sql

-- =============================================
-- Author:		Chad Feldmann
-- Create date: 09/04/2015
-- Description:	Gets all Facilities
--Test:  [Securitization].[sp_GetFacilities] 
-- =============================================
CREATE PROCEDURE [Securitization].[sp_GetFacilities]

AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
SET NOCOUNT ON;


SELECT 
	f.*,
	fl.LessorID,
	[Securitization].[CalculateFacilitySpread] ( 
    ProgramFeePct,
	UnusedFeePct,
	ServicerFeePct,
	BackupServicerFeePct ,
	OtherPct ,
	SpreadCalculation 
)  AS Spread
FROM
Securitization.Facilities f
INNER JOIN Securitization.FacilityLessor fl ON fl.Facility_ID = f.ID	
ORDER BY fl.LessorID

END

GO

```


---

## <a name="#uses"></a>Uses

* [[Securitization].[CalculateFacilitySpread]](../Functions/Scalar-valued_Functions/Securitization_CalculateFacilitySpread.md)
* [Securitization]
* [Securitization].[Facilities]
* [Securitization].[FacilityLessor]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

