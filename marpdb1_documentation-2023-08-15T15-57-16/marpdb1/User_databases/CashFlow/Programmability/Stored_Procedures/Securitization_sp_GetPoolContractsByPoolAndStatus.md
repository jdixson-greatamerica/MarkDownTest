#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_GetPoolContractsByPoolAndStatus

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_GetPoolContractsByPoolAndStatus]

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
| @Status | int | 4 |


---

## <a name="#sqlscript"></a>SQL Script

```sql

-- =============================================
-- Author:		John Thadison
-- Create date: 09/01/2015
-- Description:	returns Pools including Facility Lessor by status
--Test:  [Securitization].[sp_GetPoolContractsByPoolAndStatus] 134, 'Active'
-- =============================================
Create PROCEDURE [Securitization].[sp_GetPoolContractsByPoolAndStatus]
	-- Add the parameters for the stored procedure here
	@Id int = 0,
	@Status int = -1

AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;

	IF(@Status = 1)
	Begin
	 select 
		pc.Contract_ContractID AS ContractId
		,p.ProductLineCode AS ProductLineID
		,c.Residual
		,c.TermDate
		,pc.NPV AS PoolContractNPV
		,c.LesseeName
		,c.AggregationParentName
		,c.EquipmentCost, c.BusinessUnit, c.DelinStatusDescription
	FROM 
		Securitization.PoolContract pc
		INNER JOIN Securitization.Pools p on p.ID = pc.Pool_ID
		INNER JOIN Securitization.Contracts c on c.ContractID = pc.Contract_ContractID
		WHERE pc.InFacilityPool = 0 
		AND pc.Active = 1
		AND p.ID = @Id
		ORDER BY c.ContractID DESC
	End
	ELSE IF(@Status = 0)
	Begin
		 SELECT 
			pc.Contract_ContractID AS ContractId
			,p.ProductLineCode AS ProductLineID
			,c.Residual
			,c.TermDate
			,pc.NPV AS PoolContractNPV
			,c.LesseeName
			,c.AggregationParentName
			,c.EquipmentCost, c.BusinessUnit, c.DelinStatusDescription
		FROM 
		Securitization.PoolContract pc
		inner join Pools p on p.Id = pc.Pool_id
		INNER JOIN Securitization.Contracts c on c.ContractID = pc.Contract_ContractID
		WHERE pc.InFacilityPool = 0 
		AND pc.Active = 0
		AND p.ID = @Id
		ORDER BY c.ContractID DESC
	End
	ELSE
	Begin
		 SELECT 
			pc.Contract_ContractID AS ContractId
			,p.ProductLineCode AS ProductLineID
			,c.Residual
			,c.TermDate
			,pc.NPV AS PoolContractNPV
			,c.LesseeName
			,c.AggregationParentName
			,c.EquipmentCost, c.BusinessUnit, c.DelinStatusDescription
		FROM 
		Securitization.PoolContract pc
		inner join Pools p on p.Id = pc.Pool_id
		INNER JOIN Securitization.Contracts c on c.ContractID = pc.Contract_ContractID
		WHERE pc.InFacilityPool = 0 
		AND p.ID = @Id
		ORDER BY c.ContractID DESC
	END
END
	
GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]
* [Securitization].[Contracts]
* [Securitization].[PoolContract]
* [Securitization].[Pools]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

