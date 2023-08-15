#### 

[Project](../../../../../../index.md) > [marpdb1](../../../../../index.md) > [User databases](../../../../index.md) > [CashFlow](../../../index.md) > Programmability > Functions > [Scalar-valued Functions](Scalar-valued_Functions.md) > Securitization.CalculateFacilitySpread

# ![Scalar-valued Functions](../../../../../../Images/Function_Scalar32.png) [Securitization].[CalculateFacilitySpread]

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
| @ProgramFeePct | decimal(6,5) | 5 |
| @UnusedFeePct | decimal(6,5) | 5 |
| @ServicerFeePct | decimal(6,5) | 5 |
| @BackupServicerFeePct | decimal(6,5) | 5 |
| @OtherPct | decimal(6,5) | 5 |
| @SpreadCalculation | nvarchar(250) | 500 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
-- =============================================
-- Author:		cfeldman
-- Create date: 10/01/2015
-- Description:	Calculates the spread based on parameters and formula
-- SELECT [Securitization].[CalculateFacilitySpread] ( .02,.03,.04,.05,.06,'ProgramFeePct+UnusedFeePct+ServicerFeePct+BackupServicerFeePct+OtherPct')
-- =============================================
create FUNCTION [Securitization].[CalculateFacilitySpread] ( 
    @ProgramFeePct decimal(6, 5),
	@UnusedFeePct decimal(6, 5) ,
	@ServicerFeePct decimal(6, 5),
	@BackupServicerFeePct decimal(6, 5),
	@OtherPct decimal(6, 5),
	@SpreadCalculation nvarchar(250)
) RETURNS decimal(20,8)  
AS BEGIN 

DECLARE @valueList varchar(8000)
DECLARE @pos INT
DECLARE @len INT
DECLARE @value varchar(8000)

SET @valueList = @SpreadCalculation
SET @valueList	= @valueList + '+'
set @pos = 0
set @len = 0

DECLARE @Spread decimal(6,5) = 0

WHILE CHARINDEX('+', @valueList, @pos+1)>0
BEGIN
    set @len = CHARINDEX('+', @valueList, @pos+1) - @pos
    set @value = SUBSTRING(@valueList, @pos, @len)
	
	IF(LOWER(@value) = 'programfeepct')
	BEGIN
		SET @Spread = @Spread + COALESCE( @ProgramFeePct,0);
	END
	ELSE IF(LOWER(@value) = 'unusedfeepct')
	BEGIN
		SET @Spread = @Spread +COALESCE(  @UnusedFeePct,0);
	END
	ELSE IF(LOWER(@value) = 'servicerfeepct')
	BEGIN
		SET @Spread = @Spread + COALESCE( @ServicerFeePct,0);
	END
	ELSE IF(LOWER(@value) = 'backupservicerfeepct')
	BEGIN
		SET @Spread = @Spread + COALESCE( @BackupServicerFeePct,0);
	END
	ELSE IF(LOWER(@value) = 'otherpct')
	BEGIN
		SET @Spread = @Spread + COALESCE( @OtherPct, 0);
	END

    set @pos = CHARINDEX('+', @valueList, @pos+@len) +1
END
   
   RETURN @Spread      
END 


GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]


---

## <a name="#usedby"></a>Used By

* [[Securitization].[sp_GetFacilities]](../../Stored_Procedures/Securitization_sp_GetFacilities.md)
* [[Securitization].[sp_GetFacilitySummary]](../../Stored_Procedures/Securitization_sp_GetFacilitySummary.md)


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

