#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.spGetEligibilityCriteriaTotals

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[spGetEligibilityCriteriaTotals]

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
-- Create date: 11/13/2015
-- Description:	gets data tables for eligiblitycriteria totals
--EXEC [Securitization].[spGetEligibilityCriteriaTotals]
-- =============================================
create PROCEDURE [Securitization].[spGetEligibilityCriteriaTotals]
	AS
BEGIN


DECLARE @TotalCBR money,
 @TotalEligibleCBR money	
SELECT @TotalCBR = sum(cbr) FROM Securitization.Contracts c
WHERE c.Lessor=3

SELECT coalesce(@TotalCBR,0) AS TotalCBR

SELECT li.ItemText,coalesce(ect.Total,0) AS Total FROM Securitization.EligibilityCriteriaTotals ect
INNER JOIN Securitization.ListItems li ON li.Id	= ect.Id
INNER JOIN Securitization.Lists l ON l.ID = li.List_ID	
WHERE li.ShortName NOT in('HeldForExcessExposure','ManagementExclusion')
AND l.ShortName='EC_SUBCATEGORY'
ORDER BY li.DisplayOrder

SELECT @TotalEligibleCBR =  @totalCBR-sum(ect.Total)  FROM Securitization.EligibilityCriteriaTotals ect
INNER JOIN Securitization.ListItems li ON li.Id	= ect.Id
INNER JOIN Securitization.Lists l ON l.ID = li.List_ID	
WHERE li.ShortName NOT in('HeldForExcessExposure','ManagementExclusion')
AND l.ShortName='EC_SUBCATEGORY'

SELECT coalesce(@TotalEligibleCBR,0) AS TotalEligibleCBR

SELECT li.ItemText,coalesce(ect.Total,0) AS Total FROM Securitization.EligibilityCriteriaTotals ect
INNER JOIN Securitization.ListItems li ON li.Id	= ect.Id
INNER JOIN Securitization.Lists l ON l.ID = li.List_ID	
WHERE li.ShortName in('HeldForExcessExposure','ManagementExclusion')
AND l.ShortName='EC_SUBCATEGORY'
ORDER BY li.DisplayOrder

DECLARE @percentages TABLE(ItemText nvarchar(50),
ShortName nvarchar(50),
Amount money,
Percentage decimal(5,4),
DisplayOrder int
)

INSERT INTO @percentages
(
    ItemText,
    ShortName,
    Amount,
    Percentage,
	DisplayOrder
)

SELECT li.ItemText,li.ShortName, ect.Total,0,li.DisplayOrder FROM Securitization.EligibilityCriteriaTotals ect
INNER JOIN Securitization.ListItems li ON li.Id	= ect.Id
INNER JOIN Securitization.Lists l ON l.ID = li.List_ID	
WHERE l.ShortName='EC_CATEGORY'
ORDER BY li.DisplayOrder

UPDATE @percentages
SET Amount = @TotalEligibleCBR	
WHERE UPPER(ShortName) = 'GREEN'

UPDATE @percentages
SET
 Percentage =Amount	/@TotalCBR

SELECT p.ItemText,coalesce(p.Percentage,0) as Percentage FROM @percentages p
ORDER BY p.DisplayOrder

END
GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]
* [Securitization].[Contracts]
* [Securitization].[EligibilityCriteriaTotals]
* [Securitization].[ListItems]
* [Securitization].[Lists]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

