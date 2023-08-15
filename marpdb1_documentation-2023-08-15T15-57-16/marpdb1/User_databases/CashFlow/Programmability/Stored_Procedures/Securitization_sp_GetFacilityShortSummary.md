#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_GetFacilityShortSummary

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_GetFacilityShortSummary]

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

-- =============================================
-- Author:		Chad Feldmann
-- Create date: 10/21/2015
-- Description:	Gets summary data for each non-term facility.  
--Test:  [Securitization].[sp_GetFacilityShortSummary]
-- =============================================
CREATE PROCEDURE [Securitization].[sp_GetFacilityShortSummary] 
AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;

	SELECT f.Name,f.Capacity,sum(CurrentMonthNPV * AdvanceRate) AS DebtOutstanding
	FROM Securitization.Facilities f	
	INNER JOIN Securitization.FacilityLessor fl ON fl.Facility_ID = f.ID
	INNER JOIN	Securitization.Contracts c ON c.Lessor	= fl.LessorID	
	WHERE f.Term = 0
	GROUP BY 
			f.Name,
			f.Capacity
	ORDER BY f.Name
END
GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]
* [Securitization].[Contracts]
* [Securitization].[Facilities]
* [Securitization].[FacilityLessor]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

