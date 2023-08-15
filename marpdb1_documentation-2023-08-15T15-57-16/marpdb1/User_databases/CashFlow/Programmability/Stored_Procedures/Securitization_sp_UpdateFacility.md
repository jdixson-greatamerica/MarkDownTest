#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_UpdateFacility

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_UpdateFacility]

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
| @Capacity | decimal(20,2) | 13 |
| @ProgramFee | decimal(6,5) | 5 |
| @UnusedFee | decimal(6,5) | 5 |
| @ServicerFee | decimal(6,5) | 5 |
| @BackupServicerFee | decimal(6,5) | 5 |
| @Other | decimal(6,5) | 5 |
| @SpreadCalculation | nvarchar(250) | 500 |
| @Active | bit | 1 |
| @ModifiedBy | nvarchar(50) | 100 |


---

## <a name="#sqlscript"></a>SQL Script

```sql

-- =============================================
-- Author:		Chad Feldmann
-- Create date:10/26/2015
-- Description:	Updates a facility
--Test:  [Securitization].[sp_UpdateFacility] 
-- =============================================
CREATE PROCEDURE [Securitization].[sp_UpdateFacility]
	-- Add the parameters for the stored procedure here
	@FacilityID int,
	@Capacity decimal(20,2),
	@ProgramFee decimal(6,5),
	@UnusedFee decimal(6,5),
	@ServicerFee decimal(6,5),
	@BackupServicerFee decimal(6,5),
	@Other decimal(6,5),
	@SpreadCalculation nvarchar(250),
	@Active bit,
	@ModifiedBy nvarchar(50)
AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;

	UPDATE Securitization.Facilities
	SET
		Capacity = @Capacity, 
		ProgramFeePct	 = @ProgramFee,
		UnusedFeePct = @UnusedFee,
		ServicerFeePct = @ServicerFee,
		BackupServicerFeePct = @BackupServicerFee,
		OtherPct = @Other,
		Active = @Active,	
		SpreadCalculation = @SpreadCalculation,
		ModifiedOn = GETDATE(),
		ModifiedBy = @ModifiedBy	
	WHERE ID = @FacilityID
END

GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]
* [Securitization].[Facilities]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

