#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.spDeletePool

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[spDeletePool]

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
| @DeletedBy | nvarchar(50) | 100 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
SET QUOTED_IDENTIFIER OFF
GO
-- =============================================
-- Author:		Chad Feldmann
-- Create date: 05/08/2013
-- Description:	deletes a pool
--Test:  [Securitization].[spUpdatePool] 
-- =============================================
create PROCEDURE [Securitization].[spDeletePool]
	-- Add the parameters for the stored procedure here
	@PoolID int,
	@DeletedBy nvarchar(50)
AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
SET NOCOUNT ON;
begin try
		BEGIN TRANSACTION 
			--Needed for trigger
			DECLARE @name VARBINARY(128) 
			SET @name = CAST(@DeletedBy	AS VARBINARY(128));
			SET CONTEXT_INFO @name /*Set it*/

			delete from Securitization.Lessors where Pool_ID = @PoolID;
			delete from Securitization.PoolConcentration where Pool_ID = @PoolID;
			delete from Securitization.PoolContract where Pool_ID = @PoolID;
			delete from Securitization.Pools where id =@PoolID;
			
			COMMIT TRANSACTION 

end try

begin catch
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

* [Securitization]
* [Securitization].[Lessors]
* [Securitization].[PoolConcentration]
* [Securitization].[PoolContract]
* [Securitization].[Pools]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

