#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_UpdateFacilityLimit

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_UpdateFacilityLimit]

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
| @FacilityLimitID | int | 4 |
| @Percentage | decimal(6,5) | 5 |
| @Amount | decimal(20,2) | 13 |
| @Type | int | 4 |
| @ModifiedBy | nvarchar(50) | 100 |


---

## <a name="#sqlscript"></a>SQL Script

```sql

-- =============================================
-- Author:		Chad Feldmann
-- Create date:10/06/2015
-- Description:	Updates a concentration test limit
--Test:  [Securitization].[sp_UpdateFacilityLimit] 
-- =============================================
CREATE PROCEDURE [Securitization].[sp_UpdateFacilityLimit]
	-- Add the parameters for the stored procedure here
	@FacilityLimitID int,
	@Percentage decimal(6,5),
	@Amount decimal(20,2),
	@Type int,
	@ModifiedBy nvarchar(50)
AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;

		IF(@Type = 0)--Percentage
		BEGIN
			UPDATE Securitization.FacilityLimits
			SET
			   [Type] = @Type,
			    ModifiedOn = GETDATE(),
			    ModifiedBy = @ModifiedBy,
				Percentage = @Percentage,
				Amount = 0
			WHERE ID = @FacilityLimitID
		END
		ELSE IF (@Type = 1)--BOTH
		BEGIN
		UPDATE Securitization.FacilityLimits
			SET
			   [Type] = @Type, 
			    ModifiedOn = GETDATE(),
			    ModifiedBy = @ModifiedBy,
				Percentage = @Percentage,
				Amount = @Amount	
			WHERE ID = @FacilityLimitID
		END
		ELSE IF (@Type = 2)--Amount
		BEGIN
			UPDATE Securitization.FacilityLimits
			SET
			   [Type] = @Type, 
			    ModifiedOn = GETDATE(),
			    ModifiedBy = @ModifiedBy,
				Percentage = 0,
				Amount = @Amount	
			WHERE ID = @FacilityLimitID
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

