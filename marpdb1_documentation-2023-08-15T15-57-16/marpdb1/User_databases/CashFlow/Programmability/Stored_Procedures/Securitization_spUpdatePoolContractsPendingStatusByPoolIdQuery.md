#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.spUpdatePoolContractsPendingStatusByPoolIdQuery

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[spUpdatePoolContractsPendingStatusByPoolIdQuery]

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
| @PendingStatus | int | 4 |
| @PoolId | int | 4 |
| @vchCriteria | varchar(8000) | 8000 |
| @Reason | varchar(50) | 50 |
| @ModifiedBy | nvarchar(50) | 100 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
SET QUOTED_IDENTIFIER OFF
GO


-- =============================================
-- Author:		Logan Keenan
-- Create date: 05/24/2011
-- Description:	Updates the pending status of pool contracts given the pool ID and option where parameters
--Test:  [Securitization].[spUpdatePoolContractsPendingStatusByPoolIdQuery] 1,60,'','Random'
-- =============================================
CREATE procedure [Securitization].[spUpdatePoolContractsPendingStatusByPoolIdQuery]

	@PendingStatus INT,
	@PoolId INT,
	@vchCriteria VARCHAR(8000) = '',
	@Reason VARCHAR(50),
	@ModifiedBy nvarchar(50)
	AS 
	BEGIN
		DECLARE @vchSQL VARCHAR(8000)

		SET @vchSQL = '';

		SET @vchSQL = @vchSQL + 

		
		'update Securitization.PoolContract 
		set PendingStatus = '+ cast(@PendingStatus as varchar(50)); 
		if(@PendingStatus=0)
		BEGIN
			SET @vchSQL = @vchSQL + ' ,Reason = '''+ cast(@Reason as varchar(50))+  '''';								  ;		
		END
		SET @vchSQL = @vchSQL + ' ,ModifiedBy = '''+ cast(@ModifiedBy as varchar(50))+  '''';
		SET @vchSQL = @vchSQL + ' ,ModifiedOn = GetDate()';
		SET @vchSQL = @vchSQL + 
		' from Securitization.PoolContract as PoolContract 
		inner join Securitization.Pools as Pool 
		on PoolContract.Pool_ID = Pool.ID
		inner join Securitization.Contracts as Contract 
		on PoolContract.Contract_ContractID = Contract.ContractID 
		where PoolContract.InFacilityPool = 0 and PoolContract.Pool_ID = ' +cast(@PoolId as varchar(50)) + 
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

