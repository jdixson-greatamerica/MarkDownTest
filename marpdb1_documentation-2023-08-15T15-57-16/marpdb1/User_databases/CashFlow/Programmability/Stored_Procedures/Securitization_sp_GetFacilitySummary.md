#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_GetFacilitySummary

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_GetFacilitySummary]

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
-- Create date: 10/15/2015
-- Description:	Gets summary data for each non-term facility.  
--Test:  [Securitization].[sp_GetFacilitySummary]
-- History:
--		Name:	Chad Feldmann
--		Date:	06/18/2019
--		Desc:	Ticket #PRJ0040256 - Update so GAFII returns DebtOutstanding even if there is no data in contracts table. 
-- =============================================
CREATE PROCEDURE [Securitization].[sp_GetFacilitySummary] 
AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;


DECLARE @facilitySummary TABLE(
	ID int,
	Name nvarchar(50),
	Capacity decimal(20,8),
	DebtOutstanding decimal(20,8),
	Last12Months decimal(20,8),
	AmountAvailable decimal(20,8),
	CurrentUtilization decimal(6,5),
	WeightedDiscountRate decimal(7,6),
	--WeightedFixedRate decimal(7,6),
	Spread decimal(6,5))

	INSERT INTO @facilitySummary
	(
		ID,
		Name,
		Capacity,
		DebtOutstanding,
		Spread
	)
	SELECT f.ID, f.Name,f.Capacity,COALESCE(sum(CurrentMonthNPV * AdvanceRate),0) AS DebtOutstanding,
	[Securitization].[CalculateFacilitySpread] ( 
		ProgramFeePct,
		UnusedFeePct,
		ServicerFeePct,
		BackupServicerFeePct ,
		OtherPct ,
		SpreadCalculation 
	)  AS Spread  FROM Securitization.Facilities f	
	INNER JOIN Securitization.FacilityLessor fl ON fl.Facility_ID = f.ID
	LEFT OUTER JOIN Securitization.Contracts c ON c.Lessor	= fl.LessorID	
	WHERE f.Term = 0
	GROUP BY f.ID,
			f.Name,
			f.Capacity,
			f.ProgramFeePct,
			f.UnusedFeePct,
			f.ServicerFeePct,
			f.BackupServicerFeePct ,
			f.OtherPct ,
			f.SpreadCalculation 
	ORDER BY f.Name

	UPDATE @facilitySummary
	SET
	   AmountAvailable = Capacity - DebtOutstanding,
	   CurrentUtilization = DebtOutstanding / Capacity	

---**************************************************************************************
--Calculate Weighted Fixed and Weighted Discounter Rates
DECLARE @ProductLineNPV TABLE(
ProductLineID char(10),
Lessor smallint	,
Name nvarchar(50),
DiscountRate decimal(9,8),
AdvanceRate decimal(9,8), 
NPVTotal decimal(20,8),
FixedRate decimal(9,8)
)

INSERT INTO @ProductLineNPV
(
    ProductLineID,
    Lessor,
    Name,
    DiscountRate,
    AdvanceRate,
    NPVTotal,
	FixedRate
)

SELECT 
	c.ProductLineID,
	c.Lessor,
	f.Name,
	c.DiscountRate,
	c.AdvanceRate, 
	sum(c.CurrentMonthNPV) AS NPV,
	c.DiscountRate - [Securitization].[CalculateFacilitySpread] ( 
		ProgramFeePct,
		UnusedFeePct,
		ServicerFeePct,
		BackupServicerFeePct ,
		OtherPct ,
		SpreadCalculation 
	)  AS FixedRate
FROM  Securitization.Facilities f 
LEFT OUTER JOIN Securitization.FacilityLessor	fl ON f.Id = fl.Facility_ID	
LEFT OUTER JOIN Securitization.Contracts c	ON fl.LessorID = c.Lessor
WHERE 
f.Term=0
GROUP BY c.ProductLineID,c.Lessor,f.Name,c.DiscountRate,c.AdvanceRate,ProgramFeePct,
		UnusedFeePct,
		ServicerFeePct,
		BackupServicerFeePct ,
		OtherPct ,
		SpreadCalculation;
		
WITH WeightedAverages(AverageDiscount, AverageFixed, FacilityName)
AS
(
SELECT 
CASE 
WHEN sum(NPVTotal) <>0 THEN
 sum(pln.DiscountRate * NPVTotal)/ sum(NPVTotal)
ELSE
0
END,
CASE 
WHEN sum(NPVTotal) <>0 THEN
 sum(pln.FixedRate	 * NPVTotal)/ sum(NPVTotal)
ELSE
0
END
  ,pln.Name FROM @ProductLineNPV pln	
GROUP BY pln.Name
)

UPDATE @facilitySummary
SET
    WeightedDiscountRate = w.AverageDiscount--, -- decimal
   -- WeightedFixedRate = w.AverageFixed -- decimal
FROM @facilitySummary fs	
INNER JOIN WeightedAverages w ON w.FacilityName = fs.Name	


UPDATE @facilitySummary
SET
Last12Months = tot.Last12Months
FROM @facilitySummary fs
INNER JOIN(
	SELECT Name, sum(Last12Months) AS Last12Months FROM
	(
		SELECT f.Name,sum(coalesce(pe.NPV,0)*coalesce(p.AdvanceRate,0)) AS Last12Months FROM Securitization.Pools p
		INNER JOIN Securitization.FacilityLessor fl ON p.FacilityLessor_ID = fl.id
		INNER JOIN Securitization.Facilities f ON f.Id = fl.Facility_ID	
		INNER JOIN Archive.PoolExport pe ON pe.Pool_ID = p.ID	
		WHERE p.FundingDate >= DATEADD(d,1,DATEADD(YYYY,-1, CONVERT(date,GETDATE())))
		AND p.Funding=1
		AND p.Status = 3
		AND f.Term=0
		GROUP BY  f.Name,pe.AdvanceRate
	) tmp
	GROUP BY Name
) tot ON tot.Name = fs.Name 



---**************************************************************************************

--Pivot data and return
	SELECT  *
	FROM
	(
	SELECT 1 AS ord,'Capacity' AS col, COALESCE(fs.Capacity,0) AS val ,name FROM @facilitySummary fs UNION ALL	
	SELECT 2 AS ord,'Debt Outstanding' AS col, COALESCE(fs.DebtOutstanding,0) AS val,name FROM @facilitySummary fs UNION ALL	
	SELECT 3 AS ord,'Amount Available' AS col, COALESCE(fs.AmountAvailable,0) AS val,name FROM @facilitySummary fs  UNION ALL	
	SELECT 4 AS ord,'Total Funded - Prior 12 Months' AS col, COALESCE(fs.Last12Months,0) AS val,name FROM @facilitySummary fs  UNION ALL	
	SELECT 5 AS ord,'Current Utilization' AS col, COALESCE(fs.CurrentUtilization,0) AS val,name FROM @facilitySummary fs  UNION ALL	
	SELECT 6 AS ord,'Weighted Discount Rate' AS col, COALESCE(fs.WeightedDiscountRate,0) AS val,name FROM @facilitySummary fs  UNION ALL	
	--SELECT 7 AS ord,'Weighted Fixed Rate' AS col, COALESCE(fs.WeightedFixedRate,0) AS val,name FROM @facilitySummary fs  UNION ALL	
	SELECT 8 AS ord,'Spread' AS col, COALESCE(fs.Spread,0) AS val,name FROM @facilitySummary fs 
	) AS SourceTable

	PIVOT
	(
	 max(val)
	FOR Name IN ([GAFI],[GAFIII],[GAFIV],[GAFV],[GAPRI],[GAPRII])
	) AS PivotTable
	ORDER BY ord;

END
GO

```


---

## <a name="#uses"></a>Uses

* [[Securitization].[CalculateFacilitySpread]](../Functions/Scalar-valued_Functions/Securitization_CalculateFacilitySpread.md)
* [Archive].[PoolExport]
* [Securitization]
* [Securitization].[Contracts]
* [Securitization].[Facilities]
* [Securitization].[FacilityLessor]
* [Securitization].[Pools]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

