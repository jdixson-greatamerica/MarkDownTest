#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_UpdatePoolContract

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_UpdatePoolContract]

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
| @PendingStatus | int | 4 |
| @Reason | varchar(50) | 50 |
| @ModifiedBy | nvarchar(50) | 100 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
SET QUOTED_IDENTIFIER OFF
GO

-- =============================================
-- Author:		Chad Feldmann
-- Create date: 09/16/2015
-- Description:	Updates PoolContract record
--Test:  [Securitization].[sp_UpdatePoolContract] 133
-- =============================================
CREATE PROCEDURE [Securitization].[sp_UpdatePoolContract]
	-- Add the parameters for the stored procedure here
	@Id int,
	@PendingStatus int,
	@Reason varchar(50),
	@ModifiedBy nvarchar(50)
AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;
	UPDATE Securitization.PoolContract 
	SET PendingStatus = @PendingStatus,
	Reason = @Reason,
	Modifiedby = @ModifiedBy,
	ModifiedOn = GetDate()

	WHERE Id = @Id
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

