#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_GetPoolById

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_GetPoolById]

---

## <a name="#properties"></a>Properties

| Property | Value |
|---|---|
| ANSI Nulls On | YES |
| Quoted Identifier On | NO |


---

## <a name="#parameters"></a>Parameters

| Name | Data Type | Max Length (Bytes) |
|---|---|---|
| @Id | int | 4 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
SET QUOTED_IDENTIFIER OFF
GO

-- =============================================
-- Author:		John Thadison
-- Create date: 09/01/2015
-- Description:	returns pool by Id
--Test:  [Securitization].[sp_GetPoolById] 133
-- =============================================
CREATE PROCEDURE [Securitization].[sp_GetPoolById]
	-- Add the parameters for the stored procedure here
	@Id int
AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;

	SELECT 
		p.ID, 
		p.TargetPoolSize, 
		p.DefaultDiscountRate, 
		p.AdvanceRate, 
		p.ProductLineCode, 
		p.StartingDate ,
		p.Funding, 
		p.FundingDate, 
		f.Name as FacilityName, 
		p.StubPeriod,
		p.Comments,
		p.Comments2,
		p.ModifiedOn, 
		p.ModifiedBy,
		p.[Status]
	FROM 
		Securitization.Pools p
	INNER JOIN Securitization.FacilityLessor fl ON fl.ID = p.FacilityLessor_ID
	INNER JOIN Securitization.Facilities f ON f.ID = fl.Facility_ID
	WHERE p.Id = @Id
END
GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]
* [Securitization].[Facilities]
* [Securitization].[FacilityLessor]
* [Securitization].[Pools]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

