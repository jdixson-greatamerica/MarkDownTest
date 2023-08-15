#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.spUpdatePool

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[spUpdatePool]

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
| @TargetPoolSize | decimal(20,8) | 13 |
| @DefaultDiscountRate | decimal(20,8) | 13 |
| @AdvanceRate | decimal(20,8) | 13 |
| @StubPeriod | smallint | 2 |
| @ModifiedBy | nvarchar(50) | 100 |
| @Funding | bit | 1 |
| @Comments | nvarchar(max) | max |
| @Comments2 | nvarchar(max) | max |


---

## <a name="#sqlscript"></a>SQL Script

```sql
SET QUOTED_IDENTIFIER OFF
GO
-- =============================================
-- Author:		Chad Feldmann
-- Create date: 05/08/2013
-- Description:	Updates a pool
--Test:  [Securitization].[spUpdatePool] 
-- =============================================
CREATE PROCEDURE [Securitization].[spUpdatePool]
	-- Add the parameters for the stored procedure here
	@PoolID int,
	@TargetPoolSize decimal(20,8),
	@DefaultDiscountRate decimal(20,8),
	@AdvanceRate decimal(20,8),
	@StubPeriod smallint,
	@ModifiedBy nvarchar(50),
	@Funding bit,
	@Comments nvarchar(max)='',
	@Comments2 nvarchar(max)=''

AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;

begin try 
	BEGIN TRANSACTION 
	
	UPDATE Securitization.Pools
	SET 
		TargetPoolSize = @TargetPoolSize,
		DefaultDiscountRate= @DefaultDiscountRate,
		AdvanceRate = @AdvanceRate,
		StubPeriod=@StubPeriod,
		ModifiedBy = @ModifiedBy,
		ModifiedOn = GETDATE(),
		Funding = @Funding,
		Comments = @Comments,
		Comments2 = @Comments2
	
	WHERE ID=@PoolID;
	
	--Set current status = to the pending status
	UPDATE Securitization.PoolContract 
	SET
		Active = PendingStatus
	WHERE Pool_ID = @PoolID AND PendingStatus IS NOT NULL;

	--Reset the pending status
	UPDATE Securitization.PoolContract
	SET
		PendingStatus = NULL
	WHERE Pool_ID = @PoolID;

	--Clear out the reason if the contract is active in the pool
	UPDATE Securitization.PoolContract
	SET
		Reason = NULL
	WHERE Pool_ID = @PoolID
	AND Active=1;

	--Calculate the NPV on contracts in the working and destination pool
	EXEC Securitization.spCalculateNPVByPoolID @PoolID,@ModifiedBy 

COMMIT TRANSACTION 

END TRY

BEGIN CATCH
	IF @@TRANCOUNT > 0		
		ROLLBACK TRANSACTION
	declare	@ErrorMessage	varchar(MAX),
				@ErrorProcedure varchar(30),
				@ErrorSeverity	int

	select @ErrorMessage = ERROR_MESSAGE(), @ErrorProcedure = ERROR_PROCEDURE(), @ErrorSeverity = ERROR_SEVERITY()
		
	--execute dbo.spInsertError @ErrorMessage, @ErrorProcedure
	raiserror(@ErrorMessage, @ErrorSeverity, 1)
end catch


END

GO

```


---

## <a name="#uses"></a>Uses

* [[Securitization].[spCalculateNPVByPoolID]](Securitization_spCalculateNPVByPoolID.md)
* [Securitization]
* [Securitization].[PoolContract]
* [Securitization].[Pools]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

