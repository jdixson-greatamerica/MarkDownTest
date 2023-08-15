#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.spCalculateNPVByPoolID

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[spCalculateNPVByPoolID]

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
| @UserID | nvarchar(50) | 100 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
SET QUOTED_IDENTIFIER OFF
GO
-- =============================================
-- Author:		Chad Feldmann
-- Create date: 03/26/2013
-- Description:	Calculates the NPV for a given pool
--Test:  [Securitization].[spCalculateNPVByPoolID] 63,'system'
-- =============================================
create PROCEDURE [Securitization].[spCalculateNPVByPoolID]
	-- Add the parameters for the stored procedure here
	@PoolID int,
	@UserID nvarchar(50)
AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;



--Calculate the NPV for contracts in the working pool
DECLARE @StartDate datetime;
DECLARE @DiscoutRate decimal(20,8);
DECLARE @StubPeriod int;

SELECT @StartDate = StartingDate,
		@DiscoutRate = DefaultDiscountRate,
		@StubPeriod = StubPeriod
FROM Securitization.Pools p
INNER JOIN Securitization.FacilityLessor fl
on p.FacilityLessor_ID = fl.ID
WHERE p.ID = @PoolID;

CREATE TABLE #Periods
(
seq varchar(5),
ContractKey bigint, 
ContractID char(15), 
DiscountRate decimal(20,8),
SpecialNPVCalculation bit,
IncludeResiduals bit
) 
insert into #Periods(seq, ContractKey, ContractID, DiscountRate,SpecialNPVCalculation,IncludeResiduals )

		--Get the rates on the working pool
		select distinct seq, 
						ct.ContractKey,
						c.ContractID, 
						@DiscoutRate as DiscountRate, 
						coalesce(fl.SpecialNPVCalculation,0),
						coalesce(fl.IncludeResiduals,0)
		from dbo.Contract_Transaction ct
		inner join Securitization.Contracts c on c.ContractKey = ct.ContractKey
		inner join Securitization.PoolContract pc on pc.Contract_ContractID = c.ContractID
		inner join Securitization.Pools p on p.ID = pc.Pool_ID
		left join Securitization.FacilityLessor fl on p.FacilityLessor_ID = fl.id
		where MONTH(@StartDate) = TransactionMonth and Year(@StartDate) = TransactionYear
		and pc.InFacilityPool = 0
		and p.id=@PoolID
		union all
		select distinct seq, 
						ct.ContractKey, 
						c.ContractID, 
						c.DiscountRate, 
						0 as SpecialNPVCalculation,--Condition below dictates this will be false.
						coalesce(fl.IncludeResiduals,0)
		from dbo.Contract_Transaction ct
		inner join Securitization.Contracts c on c.ContractKey = ct.ContractKey
		inner join Securitization.PoolContract pc on pc.Contract_ContractID = c.ContractID
		inner join Securitization.Pools p on p.ID = pc.Pool_ID
		left join Securitization.FacilityLessor fl on p.FacilityLessor_ID = fl.id
		where MONTH(@StartDate) = TransactionMonth and Year(@StartDate) = TransactionYear
		and pc.InFacilityPool = 1
		and p.id=@PoolID
	
	
begin try
		BEGIN TRANSACTION 
--Normal NPV Calculation
	UPDATE Securitization.PoolContract
	SET NPV = NetPresentValue.NPV
	FROM
	Securitization.PoolContract pc
	INNER JOIN 
	(
		select 
		p.ContractID,
		sum(coalesce(securitization.cash_flow_present_value(p.DiscountRate,t.TransactionAmount, t.Seq - p.seq + 1 ),0)) as NPV
		FROM dbo.Contract_Transaction t
		inner join #Periods p on t.ContractKey = p.ContractKey
		where 
		(t.Seq - p.seq) between 0 and 83
		and 
			(
				(t.TransactionType <> 5 and p.IncludeResiduals=0)
				or
				(p.IncludeResiduals = 1)
			)
		and p.SpecialNPVCalculation = 0
		group by t.ContractKey, p.ContractID
	) NetPresentValue 
	ON NetPresentValue.ContractID = pc.Contract_ContractID
	WHERE pc.Pool_ID = @PoolID

	
	DECLARE @AdjustedPercent  decimal(20,8)
	SET @AdjustedPercent= cast(@StubPeriod as decimal(20,8))/ cast(360 as decimal(20,8))

--Special NPV Calculation
	UPDATE Securitization.PoolContract
	SET NPV = NetPresentValue.NPV
	FROM
	Securitization.PoolContract pc
	INNER JOIN 
	(
		SELECT (Coalesce(Periods2To84.NPV,0) + Period1.NPV)/(1+ Period1.DiscountRate*(@AdjustedPercent)) NPV,
		Period1.ContractID
		FROM
		(
			select 
			t.ContractKey, 
			p.ContractID,
			p.DiscountRate,
			sum(coalesce(t.TransactionAmount,0)) as NPV
			FROM dbo.Contract_Transaction t
			inner join #Periods p on t.ContractKey = p.ContractKey 
			where 
			(t.Seq - p.seq)=0
			and 
				(
					(t.TransactionType <> 5 and p.IncludeResiduals=0)
					or
					(p.IncludeResiduals = 1)
				)
			and p.SpecialNPVCalculation = 1
			group by t.ContractKey, p.ContractID,p.DiscountRate
		) Period1
		LEFT JOIN
		(
			select 
			t.ContractKey, 
			p.ContractID,
			p.DiscountRate,
			sum(coalesce(securitization.cash_flow_present_value(p.DiscountRate,t.TransactionAmount, t.Seq - p.seq ),0)) as NPV --subtracting 1 from the seq is not necessary as we want to start with the second period
			FROM dbo.Contract_Transaction t
			inner join #Periods p on t.ContractKey = p.ContractKey
			where 
			(t.Seq - p.seq) between 1 and 83
			and 
				(
					(t.TransactionType <> 5 and p.IncludeResiduals=0)
					or
					(p.IncludeResiduals = 1)
				)
			and p.SpecialNPVCalculation = 1
			group by t.ContractKey, p.ContractID,p.DiscountRate
		) Periods2To84
		on Period1.ContractKey = Periods2To84.ContractKey
	) NetPresentValue 
	ON NetPresentValue.ContractID = pc.Contract_ContractID
	WHERE pc.Pool_ID = @PoolID
		
	UPDATE Securitization.PoolContract  
	SET NPV = 0 
	WHERE NPV is null 
		AND Pool_ID = @PoolID;

	UPDATE Securitization.PoolContract SET NPV = 
	[Securitization].[RoundNPV] (pc.NPV, f.Term)
	FROM Securitization.Contracts c
	inner join Securitization.PoolContract pc on pc.Contract_ContractID = c.ContractID
	inner join Securitization.Pools p on p.id = pc.Pool_ID
	inner join Securitization.FacilityLessor fl on p.FacilityLessor_ID = fl.id
	inner join Securitization.Facilities f on f.ID = fl.Facility_ID
	where p.id=@PoolID;


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

exec Securitization.SP_CT_All @PoolID, @UserID




END


GO

```


---

## <a name="#uses"></a>Uses

* [[Securitization].[SP_CT_All]](Securitization_SP_CT_All.md)
* [[Securitization].[cash_flow_present_value]](../Functions/Scalar-valued_Functions/Securitization_cash_flow_present_value.md)
* [[Securitization].[RoundNPV]](../Functions/Scalar-valued_Functions/Securitization_RoundNPV.md)
* [dbo].[Contract_Transaction]
* [Securitization]
* [Securitization].[Contracts]
* [Securitization].[Facilities]
* [Securitization].[FacilityLessor]
* [Securitization].[PoolContract]
* [Securitization].[Pools]


---

## <a name="#usedby"></a>Used By

* [[Archive].[spArchivePool]](Archive_spArchivePool.md)
* [[Securitization].[spInsertPool]](Securitization_spInsertPool.md)
* [[Securitization].[spRefreshPoolContract]](Securitization_spRefreshPoolContract.md)
* [[Securitization].[spUpdatePool]](Securitization_spUpdatePool.md)


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

