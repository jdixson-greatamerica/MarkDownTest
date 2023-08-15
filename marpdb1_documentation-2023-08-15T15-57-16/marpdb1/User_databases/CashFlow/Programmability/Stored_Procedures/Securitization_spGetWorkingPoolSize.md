#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.spGetWorkingPoolSize

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[spGetWorkingPoolSize]

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
-- Author:		Logan Keenan
-- Create date: 06/03/2013
-- Description:	gets the size of a given working pool
--Test:  [Securitization].[spGetWorkingPoolSize]
-- =============================================

create procedure [Securitization].[spGetWorkingPoolSize]
	@PoolId int

	as
	begin
	select coalesce( SUM(pc.NPV), 0) from Securitization.PoolContract as pc
where pc.Active = '1' and pc.InFacilityPool='0' and pc.Pool_ID = @PoolId
	end
GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]
* [Securitization].[PoolContract]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

