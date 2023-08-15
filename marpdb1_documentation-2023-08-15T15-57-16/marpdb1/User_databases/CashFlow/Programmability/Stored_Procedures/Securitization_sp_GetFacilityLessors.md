#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_GetFacilityLessors

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_GetFacilityLessors]

---

## <a name="#properties"></a>Properties

| Property | Value |
|---|---|
| ANSI Nulls On | YES |
| Quoted Identifier On | NO |


---

## <a name="#sqlscript"></a>SQL Script

```sql
SET QUOTED_IDENTIFIER OFF
GO
CREATE PROCEDURE [Securitization].[sp_GetFacilityLessors]
	-- Add the parameters for the stored procedure here
	

AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;

	SELECT f.Name
		  ,fl.LessorID
		  ,fl.Facility_ID
	FROM Securitization.FacilityLessor fl
	INNER JOIN Securitization.Facilities f 
		ON f.ID = fl.Facility_ID
	ORDER BY fl.LessorID
END
GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]
* [Securitization].[Facilities]
* [Securitization].[FacilityLessor]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

