#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Archive.spFinalizePool

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Archive].[spFinalizePool]

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
| @PoolID | int | 4 |
| @DiscountRate | decimal(8,7) | 5 |
| @AdvanceRate | decimal(6,5) | 5 |
| @UserID | nvarchar(50) | 100 |
| @Comments | nvarchar(max) | max |
| @Comments2 | nvarchar(max) | max |


---

## <a name="#sqlscript"></a>SQL Script

```sql

-- =============================================
-- Author:		Chad Feldmann
-- Create date: 09/17/2013
-- Description:	Finalizes a pool 
--1.	Updates the pool status
--2.	Calculates NPV based on the archived pool data
--3.	Deletes all records from the PoolContract table.
--Test:  [Archive].[spFinalizePool]
-- =============================================
create PROCEDURE [Archive].[spFinalizePool]
	-- Add the parameters for the stored procedure here
	@PoolID int,
	@DiscountRate decimal(8,7),
	@AdvanceRate decimal(6,5),
	@UserID nvarchar(50),
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
SET DefaultDiscountRate = @DiscountRate,
	AdvanceRate= @AdvanceRate,
	ModifiedBy = @UserID,
	ModifiedOn = GETDATE(),
	Comments = @Comments,
	Comments2 = @Comments2,
	[Status] = 3
WHERE ID = @PoolID;

--Calculate the NPV for contracts in the working pool
DECLARE @StartDate datetime;
DECLARE @StubPeriod int;
DECLARE @IncludeResiduals int;
DECLARE @SpecialNPVCalculation int;

SELECT @StartDate = StartingDate,
		@StubPeriod = StubPeriod,
		@IncludeResiduals = fl.IncludeResiduals,
		@SpecialNPVCalculation = fl.SpecialNPVCalculation
FROM Securitization.Pools p
INNER JOIN Securitization.FacilityLessor fl
on p.FacilityLessor_ID = fl.ID
WHERE p.ID = @PoolID;



CREATE TABLE #CashflowArchive(
	[PoolID] INT,
	[ContractID] char(15),
	seq int NULL,
	transactionAmount money NULL)

--Build the Cashflow transaction temporary table
INSERT INTO #CashflowArchive(PoolID, ContractID, seq, transactionAmount)
select
Pool_ID,
ContractID,  
CAST(REPLACE([Month],'Month','') as INT)-1 as Seq, 
Amount

FROM
(Select 
 Pool_ID, 
 ContractID,
Month1,
Month2,
Month3,
Month4,
Month5,
Month6,
Month7,
Month8,
Month9,
Month10,
Month11,
Month12,
Month13,
Month14,
Month15,
Month16,
Month17,
Month18,
Month19,
Month20,
Month21,
Month22,
Month23,
Month24,
Month25,
Month26,
Month27,
Month28,
Month29,
Month30,
Month31,
Month32,
Month33,
Month34,
Month35,
Month36,
Month37,
Month38,
Month39,
Month40,
Month41,
Month42,
Month43,
Month44,
Month45,
Month46,
Month47,
Month48,
Month49,
Month50,
Month51,
Month52,
Month53,
Month54,
Month55,
Month56,
Month57,
Month58,
Month59,
Month60,
Month61,
Month62,
Month63,
Month64,
Month65,
Month66,
Month67,
Month68,
Month69,
Month70,
Month71,
Month72,
Month73,
Month74,
Month75,
Month76,
Month77,
Month78,
Month79,
Month80,
Month81,
Month82,
Month83,
Month84
From archive.PoolExport
where Pool_ID=@PoolID) cashflow
UNPIVOT (Amount for [Month] IN(
Month1,
Month2,
Month3,
Month4,
Month5,
Month6,
Month7,
Month8,
Month9,
Month10,
Month11,
Month12,
Month13,
Month14,
Month15,
Month16,
Month17,
Month18,
Month19,
Month20,
Month21,
Month22,
Month23,
Month24,
Month25,
Month26,
Month27,
Month28,
Month29,
Month30,
Month31,
Month32,
Month33,
Month34,
Month35,
Month36,
Month37,
Month38,
Month39,
Month40,
Month41,
Month42,
Month43,
Month44,
Month45,
Month46,
Month47,
Month48,
Month49,
Month50,
Month51,
Month52,
Month53,
Month54,
Month55,
Month56,
Month57,
Month58,
Month59,
Month60,
Month61,
Month62,
Month63,
Month64,
Month65,
Month66,
Month67,
Month68,
Month69,
Month70,
Month71,
Month72,
Month73,
Month74,
Month75,
Month76,
Month77,
Month78,
Month79,
Month80,
Month81,
Month82,
Month83,
Month84)) as unpvt
order by contractid,CAST(REPLACE([Month],'Month','') as INT)



--Normal NPV Calculation
	UPDATE Archive.PoolExport
	SET NPV = NetPresentValue.NPV
	--select *
	FROM
	Archive.PoolExport e
	INNER JOIN 
	(
		select 
		ContractID,
		sum(coalesce(securitization.cash_flow_present_value(@DiscountRate,transactionAmount, seq + 1 ),0)) as NPV
		FROM #CashflowArchive 
		where 
		seq between 0 and 83
		and @SpecialNPVCalculation = 0
		group by ContractID
	) NetPresentValue 
	ON NetPresentValue.ContractID = e.ContractID
	WHERE e.Pool_ID = @PoolID

	
	DECLARE @AdjustedPercent  decimal(20,8)
	SET @AdjustedPercent= cast(@StubPeriod as decimal(20,8))/ cast(360 as decimal(20,8))

--Special NPV Calculation
	UPDATE archive.PoolExport
	SET NPV = NetPresentValue.NPV
	--SELECT *
	FROM
	archive.PoolExport export
	INNER JOIN 
	(
		SELECT (Coalesce(Periods2To84.NPV,0) + Period1.NPV)/(1+ @DiscountRate*(@AdjustedPercent)) NPV,
		Period1.ContractID
		FROM
		(
			select 
			ContractID,
			sum(coalesce(transactionAmount,0)) as NPV
			FROM #CashflowArchive
			where 
			seq=0 
			and @SpecialNPVCalculation = 1
			group by ContractID
		) Period1
		LEFT JOIN
		(
			select 
			ContractID,
			sum(coalesce(securitization.cash_flow_present_value(@DiscountRate,transactionAmount, seq ),0)) as NPV --subtracting 1 from the seq is not necessary as we want to start with the second period
			FROM #CashflowArchive
			where 
			seq between 1 and 83
			and @SpecialNPVCalculation = 1
			group by ContractID
		) Periods2To84
		on Period1.ContractID = Periods2To84.ContractID
	) NetPresentValue 
	ON NetPresentValue.ContractID = export.ContractID
	WHERE export.Pool_ID = @PoolID
		
	UPDATE Archive.PoolExport 
	SET NPV = 0 
	WHERE NPV is null 
		AND Pool_ID = @PoolID;

	UPDATE Archive.PoolExport SET NPV = 
	[Securitization].[RoundNPV] (e.NPV, f.Term)
	FROM Archive.PoolExport e
	inner join Securitization.Pools p on p.id = e.Pool_ID
	inner join Securitization.FacilityLessor fl on p.FacilityLessor_ID = fl.id
	inner join Securitization.Facilities f on f.ID = fl.Facility_ID
	where p.id=@PoolID;

	DELETE FROM Securitization.PoolContract 
	WHERE Pool_ID = @PoolID;

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
* [Archive]
* [Archive].[PoolExport]
* [Securitization].[Facilities]
* [Securitization].[FacilityLessor]
* [Securitization].[PoolContract]
* [Securitization].[Pools]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

