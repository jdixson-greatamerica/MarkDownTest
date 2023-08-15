#### 

[Project](../../../../../../index.md) > [marpdb1](../../../../../index.md) > [User databases](../../../../index.md) > [CashFlow](../../../index.md) > Programmability > Functions > [Scalar-valued Functions](Scalar-valued_Functions.md) > dbo.fnBillingFrequency

# ![Scalar-valued Functions](../../../../../../Images/Function_Scalar32.png) [dbo].[fnBillingFrequency]

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
| @PaymentType | varchar(20) | 20 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
CREATE FUNCTION [dbo].[fnBillingFrequency] (@PaymentType varchar(20))
	RETURNS tinyint
AS BEGIN
	RETURN CASE @PaymentType
				WHEN 'Monthly' THEN 1
				WHEN 'Quarterly' THEN 3
				WHEN 'Semi-Annual' THEN 6
				WHEN 'Annual' THEN 12
				ELSE 1
			END;
END;
GO

```


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

