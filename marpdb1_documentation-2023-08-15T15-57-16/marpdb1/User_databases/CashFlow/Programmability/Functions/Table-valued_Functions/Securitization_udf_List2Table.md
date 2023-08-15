#### 

[Project](../../../../../../index.md) > [marpdb1](../../../../../index.md) > [User databases](../../../../index.md) > [CashFlow](../../../index.md) > Programmability > Functions > [Table-valued Functions](Table-valued_Functions.md) > Securitization.udf_List2Table

# ![Table-valued Functions](../../../../../../Images/Function_Table32.png) [Securitization].[udf_List2Table]

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
| @List | varchar(max) | max |
| @Delim | char | 1 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
create FUNCTION [Securitization].[udf_List2Table]
(
@List VARCHAR(MAX),
@Delim CHAR
)
RETURNS
@ParsedList TABLE
(
item VARCHAR(MAX)
)
AS
BEGIN
DECLARE @item VARCHAR(MAX), @Pos INT
SET @List = LTRIM(RTRIM(@List))+ @Delim
SET @Pos = CHARINDEX(@Delim, @List, 1)
WHILE @Pos > 0
BEGIN
SET @item = LTRIM(RTRIM(LEFT(@List, @Pos - 1)))
IF @item <> ''
BEGIN
INSERT INTO @ParsedList (item)
VALUES (CAST(@item AS VARCHAR(MAX)))
END
SET @List = RIGHT(@List, LEN(@List) - @Pos)
SET @Pos = CHARINDEX(@Delim, @List, 1)
END
RETURN
END
GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]


---

## <a name="#usedby"></a>Used By

* [[Securitization].[sp_PassFail_AllCriteria]](../../Stored_Procedures/Securitization_sp_PassFail_AllCriteria.md)
* [[Securitization].[spInsertPool]](../../Stored_Procedures/Securitization_spInsertPool.md)


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

