#### 

[Project](../../../../../../index.md) > [marpdb1](../../../../../index.md) > [User databases](../../../../index.md) > [CashFlow](../../../index.md) > Programmability > Functions > [Scalar-valued Functions](Scalar-valued_Functions.md) > Securitization.cash_flow_present_value

# ![Scalar-valued Functions](../../../../../../Images/Function_Scalar32.png) [Securitization].[cash_flow_present_value]

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
| @yearly_rate | decimal(20,8) | 13 |
| @cash_flow_amount | decimal(20,8) | 13 |
| @periods | int | 4 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
create FUNCTION [Securitization].[cash_flow_present_value] ( 
     @yearly_rate decimal(20,8) 
    ,@cash_flow_amount decimal(20,8) 
	,@periods int
     
) RETURNS decimal(20,8)  
AS BEGIN 


    RETURN 
		@cash_flow_amount 
        / POWER (1.0 + (@yearly_rate/12), @periods) 
         
END 
GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]


---

## <a name="#usedby"></a>Used By

* [[Archive].[spFinalizePool]](../../Stored_Procedures/Archive_spFinalizePool.md)
* [[Securitization].[spCalculateNPVByPoolID]](../../Stored_Procedures/Securitization_spCalculateNPVByPoolID.md)
* [[Securitization].[spCalculateNPVForAllContracts]](../../Stored_Procedures/Securitization_spCalculateNPVForAllContracts.md)


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

