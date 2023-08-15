#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.spAssociateFacilityConcentration

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[spAssociateFacilityConcentration]

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
| @FacilityID | int | 4 |
| @ConcentrationID | int | 4 |
| @Type | int | 4 |
| @Percentage | decimal(6,5) | 5 |
| @Amount | decimal(20,8) | 13 |


---

## <a name="#sqlscript"></a>SQL Script

```sql

-- =============================================
-- Author:		Scott Monroe
-- Create date: 12/5/2013
-- Description:	Adds a facility limit for a concentration test
-- History:
--
--		Name:			
--		Date:			
--		Description:	
-- =============================================

CREATE PROCEDURE [Securitization].[spAssociateFacilityConcentration] 
	@FacilityID int, --
	@ConcentrationID INT, --
	@Type INT,
	@Percentage DECIMAL(6,5),
	@Amount DECIMAL(20,8)
AS
BEGIN

    DECLARE @Count INT = ( SELECT   COUNT(facility_id)
                           FROM     Securitization.FacilityLimits
                           WHERE    Facility_ID = @FacilityID
                                    AND ConcentrationTest_ID = @ConcentrationID
                         )

    IF ( @Count < 1 ) 
        BEGIN
            INSERT  INTO Securitization.FacilityLimits
                    ( Type ,
                      Percentage ,
                      Amount ,
                      CreatedOn ,
                      CreatedBy ,
                      ModifiedOn ,
                      ModifiedBy ,
                      Facility_ID ,
                      ConcentrationTest_ID
                    )
            VALUES  ( @Type , -- Type - int
                      @Percentage , -- Percentage - decimal
                      @Amount , -- Amount - decimal
                      '2013-12-05 16:15:16' , -- CreatedOn - datetime
                      N'SYSTEM' , -- CreatedBy - nvarchar(50)
                      '2013-12-05 16:15:16' , -- ModifiedOn - datetime
                      N'SYSTEM' , -- ModifiedBy - nvarchar(50)
                      @FacilityID , -- Facility_ID - int
                      @ConcentrationID  -- ConcentrationTest_ID - int
                    ) 
        END 

END

GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]
* [Securitization].[FacilityLimits]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

