#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_GetPendingRemovedContractCountByPoolId

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_GetPendingRemovedContractCountByPoolId]

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
| @PoolID | int | 4 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
SET QUOTED_IDENTIFIER OFF
GO
-- =============================================
-- Author:		Chad Feldmann
-- Create date: 09/16/2015
-- Description:	Returns a count of the total number of contracts that are pending removal from the pool
--Test:  [Securitization].[sp_GetPendingRemovedContractCountByPoolId] 
-- =============================================
create PROCEDURE [Securitization].[sp_GetPendingRemovedContractCountByPoolId]
	-- Add the parameters for the stored procedure here
	@PoolID int

AS
BEGIN

  SELECT COUNT(*)
  FROM Securitization.PoolContract
  WHERE Pool_ID=@PoolId
  AND (InFacilityPool IS NOT NULL AND InFacilityPool = 0)
  AND (PendingStatus IS NOT NULL AND PendingStatus <> Active)

END

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

