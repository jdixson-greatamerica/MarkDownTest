#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.spInsertPool

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[spInsertPool]

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
| @TargetPoolSize | decimal(20,8) | 13 |
| @DefaultDiscountRate | decimal(20,8) | 13 |
| @AdvanceRate | decimal(20,8) | 13 |
| @ProductLineCode | char(10) | 10 |
| @Status | int | 4 |
| @StartingDate | datetime | 8 |
| @FundingDate | datetime | 8 |
| @CreatedBy | nvarchar(50) | 100 |
| @LessorList | nvarchar(256) | 512 |
| @FacilityLessorNumber | int | 4 |
| @StubPeriod | smallint | 2 |
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
-- Create date: 03/26/2013
-- Description:	Creates a new pool
--Test:  [Securitization].[spInsertPool] 3000000,.03,'9999',1,'05/01/2013','05/01/2013','cfeldman','1,3', 8,20,1
-- =============================================
create PROCEDURE [Securitization].[spInsertPool]
	-- Add the parameters for the stored procedure here
	@TargetPoolSize decimal(20,8),
	@DefaultDiscountRate decimal(20,8),
	@AdvanceRate decimal(20,8),
	@ProductLineCode char(10),
	@Status int, 
	@StartingDate datetime,
	@FundingDate datetime,
	@CreatedBy nvarchar(50),
	@LessorList nvarchar(256),
	@FacilityLessorNumber int,
	@StubPeriod smallint,
	@Funding bit,
	@Comments nvarchar(max),
	@Comments2 nvarchar(max)
AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;


begin try
		BEGIN TRANSACTION 

DECLARE @FacilityLessorID int;
DECLARE @FacilityID int;
SELECT @FacilityLessorID = ID,
@FacilityID = Facility_ID
FROM Securitization.FacilityLessor
WHERE LessorID = @FacilityLessorNumber

declare @Pool table(
    ID int);

INSERT INTO Securitization.Pools(	
[TargetPoolSize],
[DefaultDiscountRate],
[AdvanceRate],
[ProductLineCode],
[Status],
[StartingDate],
[FundingDate],
[CreatedOn],
[CreatedBy],
[ModifiedOn],
[ModifiedBy],
[FacilityLessor_ID],
[StubPeriod],
[Funding],
[Comments],
[Comments2])
OUTPUT inserted.Id
INTO @Pool
Values
(
@TargetPoolSize,
@DefaultDiscountRate,
@AdvanceRate,
@ProductLineCode,
@Status,
@StartingDate,
@FundingDate,
GETDATE(),
@CreatedBy,
GETDATE(),
@CreatedBy,
@FacilityLessorID,
@StubPeriod,
@Funding,
@Comments,
@Comments2)
;

DECLARE @PoolID INT
select @PoolID= id from @Pool

--Insert Lessors that make up the workign pool
DECLARE @Lessors TABLE
(
item varchar(5)
) 
insert into @Lessors(item)
select item from  [Securitization].[udf_List2Table](@LessorList,',')


insert into Securitization.Lessors
(
	[LessorID],
	[CreatedOn],
	[CreatedBy],
	[ModifiedOn],
	[ModifiedBy],
	[Pool_ID]
)
select item, 
	GETDATE(),
	@CreatedBy,
	GETDATE(),
	@CreatedBy,
	@PoolID
From 
@Lessors;

--Insert Working pool contracts
insert into Securitization.PoolContract(
	[Active],
	[CreatedOn],
	[CreatedBy],
	[ModifiedOn],
	[ModifiedBy],
	[Contract_ContractID],
	[Pool_ID],
	[InFacilityPool]
)
select
1,
GETDATE(),
@CreatedBy,
GETDATE(),
@CreatedBy,
c.ContractID,
@PoolID,
0
FROM [Securitization].[GetValidContractsByDateAndFacility](@StartingDate,@FacilityID) vc
inner join Securitization.Contracts c on c.ContractID = vc.ContractID
inner join Securitization.Lessors l on l.Pool_ID = @PoolID and l.LessorID = c.Lessor


--Insert contracts in the facility/destination pool
insert into Securitization.PoolContract(
	[Active],
	[CreatedOn],
	[CreatedBy],
	[ModifiedOn],
	[ModifiedBy],
	[Contract_ContractID],
	[Pool_ID],
	[InFacilityPool]
)
select
1,
GETDATE(),
@CreatedBy,
GETDATE(),
@CreatedBy,
ContractID,
@PoolID,
1
FROM Securitization.Contracts 
WHERE Lessor = @FacilityLessorNumber;

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

--Calculate the NPV on contracts in the working and destination pool
exec Securitization.spCalculateNPVByPoolID @PoolID, @CreatedBy

SELECT @PoolID;



END

GO

```


---

## <a name="#uses"></a>Uses

* [[Securitization].[spCalculateNPVByPoolID]](Securitization_spCalculateNPVByPoolID.md)
* [[Securitization].[GetValidContractsByDateAndFacility]](../Functions/Table-valued_Functions/Securitization_GetValidContractsByDateAndFacility.md)
* [[Securitization].[udf_List2Table]](../Functions/Table-valued_Functions/Securitization_udf_List2Table.md)
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

