#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_UpdatePoolUndoPendingChanges

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_UpdatePoolUndoPendingChanges]

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
-- Create date: 09/28/2015
-- Description:	Restores pool to previous state prior to pending changes
--Test:  [Securitization].[sp_UpdatePoolUndoPendingChanges] 
-- =============================================
create PROCEDURE [Securitization].[sp_UpdatePoolUndoPendingChanges]
	-- Add the parameters for the stored procedure here
	@PoolID int
AS
BEGIN
	--Update PoolContract. Clear the reason where the pending status is inactive.
	--Also don't clear it if it is already set to inactive.
	UPDATE Securitization.PoolContract
	SET Reason = NULL
	WHERE PendingStatus = 0
	AND Active != 0
	AND Pool_ID = @PoolID

	--Clear the pending status when the current status does not equal the pending status
	--Also check that both the current status and the pending status are set
	UPDATE Securitization.PoolContract
	SET PendingStatus = null
	WHERE Active != PendingStatus
	AND Active IS NOT NULL
	AND PendingStatus IS NOT NULL
	AND Pool_ID = @PoolID

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

