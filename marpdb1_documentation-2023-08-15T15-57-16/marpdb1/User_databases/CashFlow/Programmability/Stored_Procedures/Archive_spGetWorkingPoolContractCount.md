#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Archive.spGetWorkingPoolContractCount

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Archive].[spGetWorkingPoolContractCount]

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
| @PoolId | int | 4 |


---

## <a name="#sqlscript"></a>SQL Script

```sql

-- =============================================
-- Author:		Chad Feldmann
-- Create date: 09/17/2013
-- Description:	Gets the pool size for an archived working pool
--Test:  [Archive].[spGetWorkingPoolContractCount] 42
-- =============================================
create procedure [Archive].[spGetWorkingPoolContractCount]
	@PoolId int

	as
	begin
	select COUNT(*) from Archive.PoolExport
where Active = 1 and Pool_ID = @PoolId
	end
GO

```


---

## <a name="#uses"></a>Uses

* [Archive]
* [Archive].[PoolExport]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

