#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.spUpdatePoolNoRefresh

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[spUpdatePoolNoRefresh]

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
| @ModifiedBy | nvarchar(50) | 100 |
| @Comments | nvarchar(max) | max |
| @Comments2 | nvarchar(max) | max |
| @IsContractAddDisabled | bit | 1 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
SET QUOTED_IDENTIFIER OFF
GO
-- =============================================
-- Author:		Chad Feldmann
-- Create date: 05/08/2013
-- Description:	Updates a pool
--Test:  [Securitization].[spUpdatePoolNoRefresh] 
-- =============================================
create PROCEDURE [Securitization].[spUpdatePoolNoRefresh]
	-- Add the parameters for the stored procedure here
	@PoolID int,
	@ModifiedBy nvarchar(50),
	@Comments nvarchar(max)=null,
	@Comments2 nvarchar(max)=null,
	@IsContractAddDisabled bit = null
AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;

	DECLARE @CurrentComments nvarchar(max);
	DECLARE @CurrentComments2 nvarchar(max);
	DECLARE @CurrentIsContractAddDisabled bit;

	SELECT @CurrentComments = Comments,
	@CurrentComments2 = Comments2
	FROM Securitization.Pools
	WHERE Id = @PoolID

	UPDATE Securitization.Pools
	SET 
		Comments = COALESCE(@Comments,@CurrentComments),
		Comments2 = COALESCE(@Comments2,@CurrentComments2),
		IsContractAddDisabled = COALESCE(@IsContractAddDisabled,@CurrentIsContractAddDisabled),
		ModifiedBy = @ModifiedBy,
		ModifiedOn = GETDATE()
	WHERE ID=@PoolID;

END

GO

```


---

## <a name="#uses"></a>Uses

* [Securitization]
* [Securitization].[Pools]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

