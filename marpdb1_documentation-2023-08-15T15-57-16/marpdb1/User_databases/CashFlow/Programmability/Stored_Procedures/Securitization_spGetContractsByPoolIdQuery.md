#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.spGetContractsByPoolIdQuery

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[spGetContractsByPoolIdQuery]

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
| @PoolId | int | 4 |
| @vchCriteria | varchar(8000) | 8000 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
SET QUOTED_IDENTIFIER OFF
GO
-- =============================================
-- Author:		Chad Feldmann
-- Create date: 03/26/2013
-- Description:	Gets contracts for a specific pool based on the vchCriteria parameter
--Test:  [Securitization].[spGetContractsByPoolIdTest] 60,'PoolContract.Active = 1'
--
-- Author:		John Thadison
-- Create date: 02/28/2022
-- Description:	Select specific contract fields from Contracts table vs selecting everything
--
-- =============================================

CREATE PROCEDURE [Securitization].[spGetContractsByPoolIdQuery]
	@PoolId int,
	@vchCriteria varchar(8000) = ''
AS
BEGIN
DECLARE @vchSQL varchar(8000)

	SET @vchSQL = ''

	SET @vchSQL = @vchSQL + '
	SELECT [Contract].ContractID,
    [Contract].ProductLineID,
    [Contract].Residual,
    [Contract].TermDate,
    [Contract].CurrentMonthNPV,
    [Contract].LesseeName,
    [Contract].AggregationParentName,
    [Contract].EquipmentCost,
    [Contract].BusinessUnit,
    [Contract].DelinStatusDescription,
    PoolContract.ID AS PoolContractID,
    PoolContract.Active AS PoolContractActive,
    PoolContract.Reason AS PoolContractReason,
    PoolContract.InFacilityPool AS PoolContractInFacilityPool,
    PoolContract.NPV AS PoolContractNPV,
    PoolContract.CreatedOn AS PoolContractCreatedOn,
    PoolContract.CreatedBy AS PoolContractCreatedBy,
    PoolContract.ModifiedOn AS PoolContractModifiedOn,
    PoolContract.ModifiedBy AS PoolContractModifiedBy,
    COALESCE(PoolContract.PendingStatus, PoolContract.Active) AS PoolContractPendingStatus,
    PoolContract.Pool_ID
FROM Securitization.Contracts AS [Contract]
    INNER JOIN Securitization.PoolContract AS PoolContract
        ON Contract.ContractID = PoolContract.Contract_ContractID
    INNER JOIN Securitization.Pools AS Pool
        ON PoolContract.Pool_ID = Pool.ID
WHERE PoolContract.InFacilityPool = 0
      AND PoolContract.Pool_ID = ' + cast(@PoolId as varchar(50)) +
CASE WHEN @vchCriteria <> ''
THEN
  ' and ' + @vchCriteria
  ELSE
  ''
END
EXECUTE (@vchSQL)
END




GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

