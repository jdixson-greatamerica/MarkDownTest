#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.spCalculateNPVForAllContracts

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[spCalculateNPVForAllContracts]

---

## <a name="#properties"></a>Properties

| Property | Value |
|---|---|
| ANSI Nulls On | YES |
| Quoted Identifier On | NO |


---

## <a name="#sqlscript"></a>SQL Script

```sql
SET QUOTED_IDENTIFIER OFF
GO
-- =============================================
-- Author:		Chad Feldmann
-- Create date: 03/26/2013
-- Description:	Sets the CurrentMonthNPV in the Securitization.Contracts table.
--Test:  [Securitization].[spCalculateNPVByPoolID] 63,'system'
-- =============================================
create PROCEDURE [Securitization].[spCalculateNPVForAllContracts]
	-- Add the parameters for the stored procedure here
AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;


begin try
		BEGIN TRANSACTION 

--Calculate the NPV for contracts in the working pool
DECLARE @StartDate datetime;
DECLARE @DiscoutRate decimal(20,8);
DECLARE @StubPeriod float;

SET @StartDate = CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)--Get starting date of current month
--SELECT @StartDate = StartingDate,
--		@DiscoutRate = DefaultDiscountRate,
--		@StubPeriod = StubPeriod
--FROM Securitization.Pools p
--INNER JOIN Securitization.FacilityLessor fl
--on p.FacilityLessor_ID = fl.ID
--WHERE p.ID = @PoolID;

DECLARE @Periods TABLE
(
seq varchar(5),
ContractKey bigint, 
ContractID char(15), 
DiscountRate decimal(20,8),
SpecialNPVCalculation bit,
IncludeResiduals bit
) 
insert into @Periods(seq, ContractKey, ContractID, DiscountRate,SpecialNPVCalculation,IncludeResiduals )

		select distinct seq, 
						ct.ContractKey, 
						c.ContractID, 
						c.DiscountRate, 
						0 as SpecialNPVCalculation,--Condition below dictates this will be false.
						coalesce(fl.IncludeResiduals,0)
		from dbo.Contract_Transaction ct
		inner join Securitization.Contracts c on c.ContractKey = ct.ContractKey
		inner join Securitization.FacilityLessor fl on c.lessor = fl.LessorID
		inner join Securitization.Facilities f on fl.Facility_ID = f.ID
		
		where MONTH(@StartDate) = TransactionMonth 
		and Year(@StartDate) = TransactionYear
		--and Upper(f.Name) <> 'TERM'
		
		
--Normal NPV Calculation
	UPDATE Securitization.Contracts
	SET CurrentMonthNPV = NetPresentValue.NPV
	FROM
	Securitization.Contracts c
	INNER JOIN 
	(
		select 
		p.ContractID,
		sum(coalesce(securitization.cash_flow_present_value(p.DiscountRate,t.TransactionAmount, t.Seq - p.seq + 1 ),0)) as NPV
		FROM dbo.Contract_Transaction t
		inner join @Periods p on t.ContractKey = p.ContractKey
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
	ON NetPresentValue.ContractID = c.ContractID
	
	UPDATE Securitization.Contracts
	SET  CurrentMonthNPV = 0
	WHERE CurrentMonthNPV is null;

	UPDATE Securitization.Contracts 
	SET CurrentMonthNPV = 
		[Securitization].[RoundNPV] (CurrentMonthNPV, f.Term)
		
	FROM Securitization.Contracts c
	inner join Securitization.FacilityLessor fl on fl.LessorID = c.Lessor
	inner join Securitization.Facilities f on f.ID = fl.Facility_ID
	
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

* [[Securitization].[cash_flow_present_value]](../Functions/Scalar-valued_Functions/Securitization_cash_flow_present_value.md)
* [[Securitization].[RoundNPV]](../Functions/Scalar-valued_Functions/Securitization_RoundNPV.md)
* [dbo].[Contract_Transaction]
* [Securitization]
* [Securitization].[Contracts]
* [Securitization].[Facilities]
* [Securitization].[FacilityLessor]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

