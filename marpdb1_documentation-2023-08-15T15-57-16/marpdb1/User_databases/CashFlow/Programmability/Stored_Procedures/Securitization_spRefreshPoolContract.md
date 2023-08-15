#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.spRefreshPoolContract

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[spRefreshPoolContract]

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
| @Pool_ID | int | 4 |
| @UserID | nvarchar(50) | 100 |
| @IsContractAddDisabled | bit | 1 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
-- =============================================
-- Author:		Chad Feldmann
-- Create date: 05/02/2013
-- Description:	Updated PoolContract Table for a given pool
--Test:  [Securitization].[spRefreshPoolContract] 30, 'system'
-- =============================================
create PROCEDURE [Securitization].[spRefreshPoolContract]
	-- Add the parameters for the stored procedure here
	@Pool_ID int,
	@UserID nvarchar(50),
	@IsContractAddDisabled bit
AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;

	begin try
		BEGIN TRANSACTION 
			--Needed for data import
	select 1 as success;
	DECLARE @StartingDate DATETIME;
	DECLARE @FacilityID INT;
	DECLARE @FacilityLessor INT;

	--SET the Starting Date and Facility ID based on Pool ID
	SELECT @StartingDate = StartingDate, @FacilityID = fl.Facility_ID, @FacilityLessor=fl.LessorID
	FROM Securitization.Pools p 
	INNER JOIN Securitization.FacilityLessor fl on p.FacilityLessor_ID = fl.ID
	WHERE p.ID = @Pool_ID;

	--Insert Working pool contracts that are now eligible
	-- Contracts that are now eligible are contracts that are not currently listed in the PoolContract table
	IF @IsContractAddDisabled = 0 
	BEGIN
		INSERT INTO Securitization.PoolContract(
		[Active],
		[CreatedOn],
		[CreatedBy],
		[ModifiedOn],
		[ModifiedBy],
		[Contract_ContractID],
		[Pool_ID],
		[InFacilityPool]
		)
		SELECT 1,GETDATE(),'SystemImport',GETDATE(),'SystemImport',vc.ContractID,@Pool_ID,0
		FROM  [Securitization].[GetValidContractsByDateAndFacility](@StartingDate,@FacilityID) vc --Gets a list of all valid contracts for a facility
		INNER JOIN Securitization.Contracts c on c.ContractID = vc.ContractID
		INNER JOIN Securitization.Pools p on p.id = @Pool_ID
		INNER JOIN Securitization.Lessors l on l.Pool_ID = p.ID  and c.Lessor= l.LessorID
		LEFT OUTER JOIN (select contract_contractID,Pool_ID FROM Securitization.PoolContract where Pool_ID = @Pool_ID) pc on pc.Contract_ContractID=vc.ContractID 
		WHERE pc.Contract_ContractID is null 
	END
	--Update working pool contracts that are no longer eligible
	--Contracts that are no longer eligible are those contained in the PoolContract table but are not contained in the Valid Contract table.
	UPDATE Securitization.PoolContract
	SET Active = 0,
	Reason='NotEligible'
	FROM Securitization.PoolContract pc
	LEFT OUTER JOIN [Securitization].[GetValidContractsByDateAndFacility](@StartingDate,@FacilityID) vc on pc.Contract_ContractID = vc.ContractID
	WHERE vc.ContractID is null and pc.Pool_ID=@Pool_ID and InFacilityPool=0; --If the contact is not in the list of valid contracts then it is no longer valid.


	--Contracts that were valid at one point, then went to invalid, now are valid again.
	--Do not make contracts active that were specifically omitted by the user.
	IF @IsContractAddDisabled = 0 
	BEGIN
		UPDATE Securitization.PoolContract
		SET Active = 1,
		Reason=''
		FROM Securitization.PoolContract pc
		INNER JOIN [Securitization].[GetValidContractsByDateAndFacility](@StartingDate,@FacilityID) vc on pc.Contract_ContractID = vc.ContractID
		WHERE pc.Active = 0 and pc.Pool_ID=@Pool_ID  and pc.Reason ='NotEligible';
	END

	--FACILTIY POOL SCENARIOS
	--1. Delete contracts that are no longer in the facility pool
	--2. Add contracts that are now in the facility pool

	--Need to delete contracts in the facility pool that are no longer in that facility
	DELETE
	FROM Securitization.PoolContract 
	WHERE 
		Contract_ContractID not in
		(
			select ContractID From Securitization.Contracts c 
			WHERE c.Lessor = @FacilityLessor
		)
		and Pool_ID = @Pool_ID
		and InFacilityPool = 1

	--Insert contracts that are now in the facility pool
	    IF @IsContractAddDisabled = 0 
        BEGIN
	INSERT INTO Securitization.PoolContract(
		[Active],[CreatedOn],[CreatedBy],[ModifiedOn],[ModifiedBy],[Contract_ContractID],[Pool_ID],[InFacilityPool])
		SELECT 1,GETDATE(),'SystemImport',GETDATE(),'SystemImport',c.ContractID,@Pool_ID,1
		FROM Securitization.Contracts c
		LEFT OUTER JOIN 
			(
				SELECT contract_contractID,Pool_ID 
				FROM Securitization.PoolContract WHERE
				Pool_ID = @Pool_ID
			) pc on pc.Contract_ContractID=c.ContractID 

		WHERE pc.Contract_ContractID is null and c.Lessor = @FacilityLessor
	END
	--Refresh NPV for the contracts in the pool
	EXEC Securitization.spCalculateNPVByPoolID @Pool_ID,@UserID;

	EXEC [Securitization].[SP_CT_All] @Pool_ID,@UserID;

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

* [[Securitization].[SP_CT_All]](Securitization_SP_CT_All.md)
* [[Securitization].[spCalculateNPVByPoolID]](Securitization_spCalculateNPVByPoolID.md)
* [[Securitization].[GetValidContractsByDateAndFacility]](../Functions/Table-valued_Functions/Securitization_GetValidContractsByDateAndFacility.md)
* [Securitization]
* [Securitization].[Contracts]
* [Securitization].[FacilityLessor]
* [Securitization].[Lessors]
* [Securitization].[PoolContract]
* [Securitization].[Pools]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

