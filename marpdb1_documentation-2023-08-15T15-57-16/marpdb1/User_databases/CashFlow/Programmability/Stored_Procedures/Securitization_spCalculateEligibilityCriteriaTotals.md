#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.spCalculateEligibilityCriteriaTotals

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[spCalculateEligibilityCriteriaTotals]

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
| @CalcType | varchar(20) | 20 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
SET QUOTED_IDENTIFIER OFF
GO

-- =============================================
-- Author:		Chad Feldmann
-- Create date: 11/13/2015
-- Description:	populates EligibilityCriteriaTotals
--EXEC [Securitization].[spCalculateEligibilityCriteriaTotals]
--
--		Name:	John Thadison
--		Date:	02/16/2022
--		Desc:	Add "Active" flag check to Security.EligibilityCriteria queries
-- =============================================
CREATE PROCEDURE [Securitization].[spCalculateEligibilityCriteriaTotals]
	@CalcType varchar(20) --'CAT' 'SUBCAT'
AS
BEGIN

DECLARE @StartDate datetime;
SET @StartDate = CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)--Get starting date of current month
DECLARE  @UsedContracts TABLE( ContractID char(15) ); 

DECLARE @ProductTotals TABLE
(
  SubCategoryID int, 
  Total money
)

INSERT INTO @ProductTotals
(
    SubCategoryID,
    Total
)
SELECT distinct li.id,0 FROM Securitization.EligibilityCriteria ec
INNER JOIN Securitization.ListItems li ON 
	( ec.SubCategoryID = li.ID AND UPPER(@CalcType)='SUBCAT') 
	OR
	( ec.CategoryID = li.ID AND UPPER(@CalcType)='CAT')
WHERE ec.Active = 1

DECLARE @EligibilityCriteriaID INT,
@SubCategoryID INT,
@SubTotal money	

DECLARE ECTotals_Cursor CURSOR FOR
SELECT ec.id,li.ID FROM Securitization.EligibilityCriteria ec
INNER JOIN Securitization.ListItems li ON 	
	( ec.SubCategoryID = li.ID AND UPPER(@CalcType)='SUBCAT') 
	OR
	( ec.CategoryID = li.ID AND UPPER(@CalcType)='CAT')
WHERE ec.Active = 1
ORDER BY ec.Priority

OPEN ECTotals_Cursor;
FETCH NEXT FROM ECTotals_Cursor INTO @EligibilityCriteriaID,@SubCategoryID;

WHILE @@FETCH_STATUS = 0 
   BEGIN



		SELECT @SubTotal = coalesce(sum(cbr),0) FROM Securitization.Contracts c
		INNER JOIN Securitization.ContractEligibilityCriteria cec ON cec.Contract_ContractID = c.ContractID	
		INNER JOIN Securitization.EligibilityCriteria ec ON ec.ID = cec.EligibilityCriteria_ID
		INNER JOIN 
			(SELECT ecf.EligibilityCriteria_ID FROM Securitization.EligibilityCriteriaFacilities ecf	
			INNER JOIN Securitization.Facilities f ON f.ID = ecf.Facility_ID
			INNER JOIN Securitization.FacilityLessor fl ON fl.Facility_ID = f.ID
			WHERE fl.LessorID=7
			)	f ON f.EligibilityCriteria_ID = ec.ID	
		LEFT OUTER JOIN @UsedContracts uc ON uc.ContractID = c.ContractID
		WHERE ec.ID = @EligibilityCriteriaID
		AND ec.Active = 1
		AND (cec.StartingDate IS NULL OR cec.StartingDate = @StartDate)
		AND cec.Pass = 0
		AND c.Lessor = 3
		AND uc.ContractID IS NULL	--Don't count contracts that have already been used.

		--SELECT @SubTotal,ec.Name
		--FROM Securitization.EligibilityCriteria ec
		--WHERE id =@EligibilityCriteriaID
			
		UPDATE @ProductTotals
		SET
		    Total = Total + @SubTotal -- money	
		WHERE SubCategoryID = @SubCategoryID	

		INSERT INTO @UsedContracts
		(
		    ContractID
		)

		SELECT c.ContractID FROM Securitization.Contracts c
		INNER JOIN Securitization.ContractEligibilityCriteria cec ON cec.Contract_ContractID = c.ContractID	
		INNER JOIN Securitization.EligibilityCriteria ec ON ec.ID = cec.EligibilityCriteria_ID
		INNER JOIN 
			(SELECT ecf.EligibilityCriteria_ID FROM Securitization.EligibilityCriteriaFacilities ecf	
			INNER JOIN Securitization.Facilities f ON f.ID = ecf.Facility_ID
			INNER JOIN Securitization.FacilityLessor fl ON fl.Facility_ID = f.ID
			WHERE fl.LessorID=7
			)	f ON f.EligibilityCriteria_ID = ec.ID	
		LEFT OUTER JOIN @UsedContracts uc ON uc.ContractID = c.ContractID
		WHERE ec.ID = @EligibilityCriteriaID
		AND ec.Active = 1
		AND (cec.StartingDate IS NULL OR cec.StartingDate= @StartDate)
		AND cec.Pass = 0
		AND c.Lessor = 3
		AND uc.ContractID IS NULL	--Don't count contracts that have already been used.

		FETCH NEXT FROM ECTotals_Cursor INTO @EligibilityCriteriaID,@SubCategoryID;
   END;
CLOSE ECTotals_Cursor;
DEALLOCATE ECTotals_Cursor;

INSERT INTO Securitization.EligibilityCriteriaTotals
(
    Id,
    Total
)
SELECT pt.SubCategoryID, pt.Total FROM @ProductTotals pt	
INNER JOIN Securitization.ListItems li ON pt.SubCategoryID=li.ID	



END
GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]
* [Securitization].[ContractEligibilityCriteria]
* [Securitization].[Contracts]
* [Securitization].[EligibilityCriteria]
* [Securitization].[EligibilityCriteriaFacilities]
* [Securitization].[EligibilityCriteriaTotals]
* [Securitization].[Facilities]
* [Securitization].[FacilityLessor]
* [Securitization].[ListItems]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

