#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization. sp_GetPoolPFPConcentrationTestCountsByPoolId

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[ sp_GetPoolPFPConcentrationTestCountsByPoolId]

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
| @Id | int | 4 |


---

## <a name="#sqlscript"></a>SQL Script

```sql

-- =============================================
-- Author:		John Thadison
-- Create date: 09/01/2015
-- Description:	returns Pools including Facility Lessor by status
--Test:  [Securitization].[ sp_GetPoolPFPConcentrationTestCountsByPoolId] 134
-- =============================================
Create PROCEDURE [Securitization].[ sp_GetPoolPFPConcentrationTestCountsByPoolId]
	-- Add the parameters for the stored procedure here
	@Id int = 0

AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;

	
		 SELECT 
			*
		FROM Securitization.PoolConcentration pc
		INNER JOIN Securitization.ConcentrationTests ct ON ct.ID = pc.ConcentrationTest_ID
		INNER JOIN Securitization.Pools p ON p.ID = pc.Pool_ID
		WHERE p.ID = @Id AND pc.ProposedFacilityPoolConcentrationStatus = 0
	
END
	
GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]
* [Securitization].[ConcentrationTests]
* [Securitization].[PoolConcentration]
* [Securitization].[Pools]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

