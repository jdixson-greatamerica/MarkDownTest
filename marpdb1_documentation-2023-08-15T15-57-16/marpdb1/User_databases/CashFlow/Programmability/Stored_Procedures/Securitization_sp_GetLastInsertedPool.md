#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_GetLastInsertedPool

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_GetLastInsertedPool]

---

## <a name="#properties"></a>Properties

| Property | Value |
|---|---|
| ANSI Nulls On | YES |
| Quoted Identifier On | YES |


---

## <a name="#sqlscript"></a>SQL Script

```sql
CREATE PROCEDURE [Securitization].[sp_GetLastInsertedPool]
	-- Add the parameters for the stored procedure here
	
AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;

	
	SELECT 
		p.ID, p.TargetPoolSize, p.DefaultDiscountRate, p.ProductLineCode, p.StartingDate ,p.Funding, p.FundingDate, f.Name, p.StubPeriod,
		p.ModifiedOn, p.ModifiedBy
	FROM 
		Securitization.Pools p
	INNER JOIN Securitization.FacilityLessor fl on fl.ID = p.FacilityLessor_ID
	INNER JOIN Securitization.Facilities f on f.ID = fl.Facility_ID
	WHERE p.createdon = (SELECT MAX(createdon)
						FROM [CashFlow].[Securitization].[pools])
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

