#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_CloneFacility

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_CloneFacility]

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
| @NewFacilityName | varchar(128) | 128 |
| @ExistingFacilityName | varchar(128) | 128 |
| @LessorId | int | 4 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
-- =============================================
-- Author:		John Thadison
-- Create date: 03/03/2022
-- Description:	Clone a facility and create assign related eligibility criteria
-- =============================================
--
CREATE PROCEDURE [Securitization].[sp_CloneFacility]
    @NewFacilityName VARCHAR(128),
    @ExistingFacilityName VARCHAR(128),
    @LessorId INT
AS
BEGIN
    BEGIN TRANSACTION;
    -- SET NOCOUNT ON added to prevent extra result sets from
    -- interfering with SELECT statements.
    SET NOCOUNT ON;

    DECLARE @NewFacilityID INT,
            @ExistingFacilityID INT;



    SELECT @ExistingFacilityID = ID
    FROM Securitization.Facilities
    WHERE Name = @ExistingFacilityName;

    IF (@ExistingFacilityID IS NULL)
    BEGIN
        SELECT 'The existing facility could not be found: ' + @ExistingFacilityName;
    END;

    SELECT @NewFacilityID = ID
    FROM Securitization.Facilities
    WHERE Name = @NewFacilityName;

    IF (@NewFacilityID IS NOT NULL)
    BEGIN
        SELECT 'The new facility:' + @NewFacilityName + ', already exists in the database: ';
    END;

    IF (ISNUMERIC(@LessorId) = 0)
    BEGIN
        SELECT 'The LessorID must be a numeric value';
    END;
    --clone existing facility to the new facility
    IF (
           @ExistingFacilityID IS NOT NULL
           AND @NewFacilityID IS NULL
           AND ISNUMERIC(@LessorId) = 1
       )
    BEGIN
        INSERT INTO Securitization.Facilities
        (
            Name,
            Active,
            IncludeResiduals,
            SpecialNPVCalculation,
            CreatedOn,
            CreatedBy,
            ModifiedOn,
            ModifiedBy,
            Capacity,
            ProgramFeePct,
            UnusedFeePct,
            ServicerFeePct,
            BackupServicerFeePct,
            OtherPct,
            Term,
            SpreadCalculation
        )
        SELECT @NewFacilityName,
               Active,
               IncludeResiduals,
               SpecialNPVCalculation,
               GETDATE(),
               'SYSTEM',
               GETDATE(),
               'SYSTEM',
               Capacity,
               ProgramFeePct,
               UnusedFeePct,
               ServicerFeePct,
               BackupServicerFeePct,
               OtherPct,
               Term,
               SpreadCalculation
        FROM Securitization.Facilities
        WHERE ID = @ExistingFacilityID;

        --Get Id of new facility
        SELECT @NewFacilityID = SCOPE_IDENTITY();

        SELECT *
        FROM Securitization.Facilities
        WHERE ID = @NewFacilityID;

        PRINT ('New facilityID: ' + CAST(@NewFacilityID AS VARCHAR(10)));
        PRINT ('Existing facilityId: ' + CAST(@ExistingFacilityID AS VARCHAR(10)));

        DECLARE @EC_ID INT;

        --Insert new records into EligibilityCriteriaFacilities


        INSERT INTO Securitization.EligibilityCriteriaFacilities
        (
            Facility_ID,
            EligibilityCriteria_ID
        )
        SELECT @NewFacilityID,
               EligibilityCriteria_ID
        FROM Securitization.EligibilityCriteriaFacilities
        WHERE Facility_ID = @ExistingFacilityID;

        SELECT *
        FROM Securitization.EligibilityCriteriaFacilities
        WHERE Facility_ID = @NewFacilityID;

        --Insert new records into FacilityConcentration

        INSERT INTO Securitization.FacilityConcentration
        (
            Concentration,
            ConcentrationStatus,
            NPV,
            FacilityLimit_Type,
            CreatedOn,
            CreatedBy,
            ModifiedOn,
            ModifiedBy,
            Facility_ID,
            ConcentrationTest_ID
        )
        SELECT Concentration,
               ConcentrationStatus,
               NPV,
               FacilityLimit_Type,
               GETDATE(),
               'system',
               GETDATE(),
               'system',
               @NewFacilityID,
               ConcentrationTest_ID
        FROM Securitization.FacilityConcentration
        WHERE Facility_ID = @ExistingFacilityID;

        SELECT *
        FROM Securitization.FacilityConcentration
        WHERE Facility_ID = @NewFacilityID;

        --Insert new records into FacilityLessor

        INSERT INTO Securitization.FacilityLessor
        (
            LessorID,
            IncludeResiduals,
            SpecialNPVCalculation,
            CreatedOn,
            CreatedBy,
            ModifiedOn,
            ModifiedBy,
            Facility_ID
        )
        SELECT @LessorId,
               IncludeResiduals,
               SpecialNPVCalculation,
               GETDATE(),
               'SYSTEM',
               GETDATE(),
               'SYSTEM',
               @NewFacilityID
        FROM Securitization.FacilityLessor
        WHERE Facility_ID = @ExistingFacilityID;

        SELECT *
        FROM Securitization.FacilityLessor
        WHERE Facility_ID = @NewFacilityID;

        --Insert new records into FacilityLimits

        INSERT INTO Securitization.FacilityLimits
        (
            [Type],
            [Percentage],
            Amount,
            CreatedOn,
            CreatedBy,
            ModifiedOn,
            ModifiedBy,
            Facility_ID,
            ConcentrationTest_ID
        )
        SELECT [Type],
               [Percentage],
               Amount,
               GETDATE(),
               'SYSTEM',
               GETDATE(),
               'SYSTEM',
               @NewFacilityID,
               ConcentrationTest_ID
        FROM Securitization.FacilityLimits
        WHERE Facility_ID = @ExistingFacilityID;

        SELECT *
        FROM Securitization.FacilityLimits
        WHERE Facility_ID = @NewFacilityID;

    END;
END;
COMMIT TRANSACTION;

GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]
* [Securitization].[EligibilityCriteriaFacilities]
* [Securitization].[Facilities]
* [Securitization].[FacilityConcentration]
* [Securitization].[FacilityLessor]
* [Securitization].[FacilityLimits]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

