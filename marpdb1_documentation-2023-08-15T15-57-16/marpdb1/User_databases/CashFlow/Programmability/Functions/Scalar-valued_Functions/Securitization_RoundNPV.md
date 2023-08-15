#### 

[Project](../../../../../../index.md) > [marpdb1](../../../../../index.md) > [User databases](../../../../index.md) > [CashFlow](../../../index.md) > Programmability > Functions > [Scalar-valued Functions](Scalar-valued_Functions.md) > Securitization.RoundNPV

# ![Scalar-valued Functions](../../../../../../Images/Function_Scalar32.png) [Securitization].[RoundNPV]

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
| @Value | decimal(20,8) | 13 |
| @Term | bit | 1 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
create FUNCTION [Securitization].[RoundNPV] ( 
     @Value decimal(20,8) 
    ,@Term bit
) RETURNS decimal(20,8)  
AS BEGIN 

    RETURN 
		Case @Term 
			WHEN 1 THEN
				@Value 
			ELSE
				ROUND(@Value,2)
		END 
         
END 
GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]


---

## <a name="#usedby"></a>Used By

* [[Archive].[spArchivePool]](../../Stored_Procedures/Archive_spArchivePool.md)
* [[Archive].[spFinalizePool]](../../Stored_Procedures/Archive_spFinalizePool.md)
* [[Archive].[spGetPoolExport]](../../Stored_Procedures/Archive_spGetPoolExport.md)
* [[Securitization].[spCalculateNPVByPoolID]](../../Stored_Procedures/Securitization_spCalculateNPVByPoolID.md)
* [[Securitization].[spCalculateNPVForAllContracts]](../../Stored_Procedures/Securitization_spCalculateNPVForAllContracts.md)
* [[Securitization].[spGetFacilityConcentrationByFacilityId]](../../Stored_Procedures/Securitization_spGetFacilityConcentrationByFacilityId.md)
* [[Securitization].[spGetFacilityExport]](../../Stored_Procedures/Securitization_spGetFacilityExport.md)
* [[Securitization].[spGetPoolConcentrationTestsByPoolId]](../../Stored_Procedures/Securitization_spGetPoolConcentrationTestsByPoolId.md)
* [[Securitization].[spGetPoolExport]](../../Stored_Procedures/Securitization_spGetPoolExport.md)
* [[Securitization].[spGetWorkingPoolConcentrationTestsByPoolId]](../../Stored_Procedures/Securitization_spGetWorkingPoolConcentrationTestsByPoolId.md)


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

