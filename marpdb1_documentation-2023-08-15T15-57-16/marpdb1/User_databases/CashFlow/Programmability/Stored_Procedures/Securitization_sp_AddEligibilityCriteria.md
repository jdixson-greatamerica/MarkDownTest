#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_AddEligibilityCriteria

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_AddEligibilityCriteria]

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
| @NewCriteria | varchar(128) | 128 |
| @Description | varchar(128) | 128 |
| @ExistingCriteriaName | varchar(128) | 128 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
-- =============================================
-- Author:		John Thadison
-- Create date: 03/12/2022
-- Description:	Add new eligibility criteria, based on existing and add associations
-- =============================================
--
CREATE PROCEDURE [Securitization].[sp_AddEligibilityCriteria]
    @NewCriteria VARCHAR(128),
    @Description VARCHAR(128),
	@ExistingCriteriaName VARCHAR(128) 
AS
BEGIN
BEGIN TRANSACTION;
    -- SET NOCOUNT ON added to prevent extra result sets from
    -- interfering with SELECT statements.
    SET NOCOUNT ON;

    DECLARE @NewEligibilityCriteriaId INT,
            @ExistingEligibilityCriteriaId INT;

	SET @NewCriteria = LTRIM(RTRIM(@NewCriteria));
    

        SELECT @ExistingEligibilityCriteriaId = ID
        FROM Securitization.EligibilityCriteria
        WHERE Name = @ExistingCriteriaName;

        IF (@ExistingEligibilityCriteriaId IS NULL)
        BEGIN
            SELECT 'The existing eligibility criteria could not be found: ' + @ExistingCriteriaName;
        END;

        SELECT @NewEligibilityCriteriaId = ID
        FROM Securitization.EligibilityCriteria
        WHERE Name = @NewCriteria;

        IF (@NewEligibilityCriteriaId IS NOT NULL)
        BEGIN
            SELECT 'The new eligibility criteria:' + @NewCriteria + ', already exists in the database: ';
        END;

		
        IF (@ExistingEligibilityCriteriaId IS NOT NULL AND @NewEligibilityCriteriaId IS NULL)
        BEGIN
            INSERT INTO Securitization.EligibilityCriteria
            (
                Name,
                LogicDescription,
                Active,
                CreatedOn,
                CreatedBy,
                ModifiedOn,
                ModifiedBy,
                ProgramValue,
                CategoryID,
                SubCategoryID,
                Priority
            )
           SELECT @NewCriteria, @Description, Active, GETDATE(), 'SYSTEM',GETDATE(), 'SYSTEM', @NewCriteria, CategoryID, SubCategoryID, Priority
		   FROM Securitization.EligibilityCriteria
            WHERE ID = @ExistingEligibilityCriteriaId;

            --Get Id of new eligibility criteria
            SELECT @NewEligibilityCriteriaId = SCOPE_IDENTITY();

            --Insert new records into EligibilityCriteriaFacilities
            

            INSERT INTO Securitization.EligibilityCriteriaFacilities
            (
                Facility_ID,
                EligibilityCriteria_ID
            )
            SELECT Facility_ID,
                   @NewEligibilityCriteriaId
            FROM Securitization.EligibilityCriteriaFacilities
            WHERE EligibilityCriteria_ID = @ExistingEligibilityCriteriaId;
        END;
	COMMIT TRANSACTION;
END;
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

