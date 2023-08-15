#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.spAssociateFacilityEligibility

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[spAssociateFacilityEligibility]

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
| @EligibilityID | int | 4 |
| @FacilityID | int | 4 |


---

## <a name="#sqlscript"></a>SQL Script

```sql

-- =============================================
-- Author:		Scott Monroe
-- Create date: 12/4/2013
-- Description:	Associates a Facility with an Eligibility Criteria
-- History:
--
--		Name:			
--		Date:			
--		Description:	
-- =============================================

CREATE PROCEDURE [Securitization].[spAssociateFacilityEligibility] 
	@EligibilityID int, --
	@FacilityID int --
AS
BEGIN


                              
    DECLARE @Count INT = ( SELECT   COUNT(Facility_ID)
                           FROM     Securitization.EligibilityCriteriaFacilities
                           WHERE    Facility_ID = @FacilityID
                                    AND EligibilityCriteria_ID = @EligibilityID
                         )
	--Checks to see if the association already exists
    IF ( @Count = 0 ) 
        BEGIN
            INSERT  INTO [Securitization].[EligibilityCriteriaFacilities]
                    ( Facility_ID ,
                      EligibilityCriteria_ID
                    )
            VALUES  ( @FacilityID ,
                      @EligibilityID
                    )
        END
   

    

END

GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]
* [Securitization].[EligibilityCriteriaFacilities]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

