#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Archive.spArchivePool

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Archive].[spArchivePool]

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
| @UserID | nvarchar(50) | 100 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
-- =============================================
-- Author:		Chad Feldmann
-- Create date: 09/17/2013
-- Description:	Archive a pool 
--1.	Updates the pool status
--2.	Calculates NPV and concentration tests
--3.	Moves all contract and cashflow data related to the pool to the Archive.PoolExport table.
--Test:  [Securitization].[spArchivePool]
-- History:
--		Name:	Chad Feldmann
--		Date:	4/12/2019
--		Desc:	Ticket #PRJ0040256 - Adding VariableTreasury field.  Removing A1Term and DocRepName
--		Name:	John Thadison
--		Date:	04/14/2022
--		Desc: STRY0019475- Add FollowupVision, VaultName, ParentCustomerName, and ParentCustomerID to exports. 
-- =============================================
CREATE PROCEDURE [Archive].[spArchivePool]
	-- Add the parameters for the stored procedure here
	@PoolID int,
	@UserID nvarchar(50)
AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
SET NOCOUNT ON;
begin try
		BEGIN TRANSACTION 
	
	--Step 1:  Update the pool
	UPDATE Securitization.Pools
	SET Status = 2, 
	DefaultDiscountRate = 0,
	ModifiedBy = @UserID,
	ModifiedOn = getdate()
	WHERE ID=@PoolID;

	--Step 2:  Calc NPV and Concentration tests.
	exec Securitization.spCalculateNPVByPoolID @PoolID,@UserID;

	--Step 3:  Archive the pool
CREATE TABLE #Cashflow(
	[Contractkey] bigint ,
	Month1 money NULL,
	Month2 money NULL,
	Month3 money NULL,
	Month4 money NULL,
	Month5 money NULL,
	Month6 money NULL,
	Month7 money NULL,
	Month8 money NULL,
	Month9 money NULL,
	Month10 money NULL,
	Month11 money NULL,
	Month12 money NULL,
	Month13 money NULL,
	Month14 money NULL,
	Month15 money NULL,
	Month16 money NULL,
	Month17 money NULL,
	Month18 money NULL,
	Month19 money NULL,
	Month20 money NULL,
	Month21 money NULL,
	Month22 money NULL,
	Month23 money NULL,
	Month24 money NULL,
	Month25 money NULL,
	Month26 money NULL,
	Month27 money NULL,
	Month28 money NULL,
	Month29 money NULL,
	Month30 money NULL,
	Month31 money NULL,
	Month32 money NULL,
	Month33 money NULL,
	Month34 money NULL,
	Month35 money NULL,
	Month36 money NULL,
	Month37 money NULL,
	Month38 money NULL,
	Month39 money NULL,
	Month40 money NULL,
	Month41 money NULL,
	Month42 money NULL,
	Month43 money NULL,
	Month44 money NULL,
	Month45 money NULL,
	Month46 money NULL,
	Month47 money NULL,
	Month48 money NULL,
	Month49 money NULL,
	Month50 money NULL,
	Month51 money NULL,
	Month52 money NULL,
	Month53 money NULL,
	Month54 money NULL,
	Month55 money NULL,
	Month56 money NULL,
	Month57 money NULL,
	Month58 money NULL,
	Month59 money NULL,
	Month60 money NULL,
	Month61 money NULL,
	Month62 money NULL,
	Month63 money NULL,
	Month64 money NULL,
	Month65 money NULL,
	Month66 money NULL,
	Month67 money NULL,
	Month68 money NULL,
	Month69 money NULL,
	Month70 money NULL,
	Month71 money NULL,
	Month72 money NULL,
	Month73 money NULL,
	Month74 money NULL,
	Month75 money NULL,
	Month76 money NULL,
	Month77 money NULL,
	Month78 money NULL,
	Month79 money NULL,
	Month80 money NULL,
	Month81 money NULL,
	Month82 money NULL,
	Month83 money NULL,
	Month84 money NULL
	)

insert into #Cashflow
SELECT Contractkey, 
[1] AS Month1,
[2] AS Month2,
[3] AS Month3,
[4] AS Month4,
[5] AS Month5,
[6] AS Month6,
[7] AS Month7,
[8] AS Month8,
[9] AS Month9,
[10] AS Month10,
[11] AS Month11,
[12] AS Month12,
[13] AS Month13,
[14] AS Month14,
[15] AS Month15,
[16] AS Month16,
[17] AS Month17,
[18] AS Month18,
[19] AS Month19,
[20] AS Month20,
[21] AS Month21,
[22] AS Month22,
[23] AS Month23,
[24] AS Month24,
[25] AS Month25,
[26] AS Month26,
[27] AS Month27,
[28] AS Month28,
[29] AS Month29,
[30] AS Month30,
[31] AS Month31,
[32] AS Month32,
[33] AS Month33,
[34] AS Month34,
[35] AS Month35,
[36] AS Month36,
[37] AS Month37,
[38] AS Month38,
[39] AS Month39,
[40] AS Month40,
[41] AS Month41,
[42] AS Month42,
[43] AS Month43,
[44] AS Month44,
[45] AS Month45,
[46] AS Month46,
[47] AS Month47,
[48] AS Month48,
[49] AS Month49,
[50] AS Month50,
[51] AS Month51,
[52] AS Month52,
[53] AS Month53,
[54] AS Month54,
[55] AS Month55,
[56] AS Month56,
[57] AS Month57,
[58] AS Month58,
[59] AS Month59,
[60] AS Month60,
[61] AS Month61,
[62] AS Month62,
[63] AS Month63,
[64] AS Month64,
[65] AS Month65,
[66] AS Month66,
[67] AS Month67,
[68] AS Month68,
[69] AS Month69,
[70] AS Month70,
[71] AS Month71,
[72] AS Month72,
[73] AS Month73,
[74] AS Month74,
[75] AS Month75,
[76] AS Month76,
[77] AS Month77,
[78] AS Month78,
[79] AS Month79,
[80] AS Month80,
[81] AS Month81,
[82] AS Month82,
[83] AS Month83,
[84] AS Month84
FROM 
(SELECT 
test.ContractKey, 
--test.Seq, 
test.TransactionAmount,
RANK() OVER (Partition By test.contractkey ORDER BY test.seq) as seq
FROM 
	(
		SELECT	Seq, 
		ct.ContractKey,
		sum(TransactionAmount) as TransactionAmount
		from dbo.vContract_Transaction ct
		inner join Securitization.Contracts c on c.ContractKey = ct.ContractKey
		inner join Securitization.PoolContract pc on pc.Contract_ContractID = c.ContractID
		inner join Securitization.Pools p on p.id = pc.Pool_ID
		inner join Securitization.FacilityLessor fl on p.FacilityLessor_ID = fl.ID
		where 
			ct.TransactionDate >= p.StartingDate 
			and p.id=@PoolID
			and (fl.IncludeResiduals = 1 or (fl.IncludeResiduals = 0 and ct.TransactionType <> 5))
		group by seq, ct.ContractKey
	)test) p
PIVOT
(
sum (TransactionAmount)
FOR seq IN
( [1],
[2],
[3],
[4],
[5],
[6],
[7],
[8],
[9],
[10],
[11],
[12],
[13],
[14],
[15],
[16],
[17],
[18],
[19],
[20],
[21],
[22],
[23],
[24],
[25],
[26],
[27],
[28],
[29],
[30],
[31],
[32],
[33],
[34],
[35],
[36],
[37],
[38],
[39],
[40],
[41],
[42],
[43],
[44],
[45],
[46],
[47],
[48],
[49],
[50],
[51],
[52],
[53],
[54],
[55],
[56],
[57],
[58],
[59],
[60],
[61],
[62],
[63],
[64],
[65],
[66],
[67],
[68],
[69],
[70],
[71],
[72],
[73],
[74],
[75],
[76],
[77],
[78],
[79],
[80],
[81],
[82],
[83],
[84] )
) AS pvt


CREATE CLUSTERED INDEX IDX_C_Cashflow_ContractKey ON #Cashflow(ContractKey)

--Only one file shoudl be archived in this pool.
DELETE FROM archive.PoolExport
WHERE Pool_ID = @PoolID;

INSERT INTO archive.PoolExport(
Pool_ID, 
ContractID,
ContractKey,
BaseContractID,
VaultName,
FundingDate,
DiscountRate,
ProductLineID,
ApplicationID,
CustomerID,
LesseeName,
ParentCustomerID,
ParentCustomerName,
LesseeAddress1,
LesseeAddress2,
LesseeCity,
LesseeState,
LesseeZip,
LesseePhone,
SicCode,
SICDivisionDesc,
MunicipalLease,
EquipCodeDesc,
DealerID,
DealerName,
AggregationParentID,
AggregationParentName,
AggregationParentID_WithExceptions,
AggregationParentName_WithExceptions,
EquipmentCost,
GrossContract,
CBR,
Residual,
UnAmortizedResidual,
RentalPymt,
BillingFrequency,
PymtsArrears,
Term,
VariablePayment,
SecurityDeposit,
PymtsUpfront,
EndingPymtsAdv,
PymtsMade,
CommenceDate,
FirstPymtDate,
TermDate,
NextDueDate,
FinalPymtDate,
DelinStatusCode,
DelinStatusDescription,
Delin31,
Delin61,
Delin91,
DelinStatus_PrevMonth,
EquipState,
EquipCounty,
EquipZip,
BillToState,
BillToZip,
InsStatusCode,
InsStatusCodeDesc,
ProgramTypeID,
ProgramTypeDesc,
ProgramVariation,
ProgramVariationDesc,
Promotion,
PromotionDesc,
PurchaseOptionID,
PurchaseOptionDesc,
PrivateLabel,
RemainingTerm,
LeaseType,
DaysDeferred,
PurchaseOrder, 
GAFI,
GAFIAmount,
PrefundCode,
CollateralCode,
CollateralCodeDesc,
RecourseVendorID,
RecourseVendorName,
IndirectBilling,
IndirectBillCollectVendorID,
IndirectBillCollectVendorName,
LeveragedIndirectBillCollect,
FixtureOnly,
Concentrations,
VariableTreasury,
PortfolioPurchCode,
PortfolioPurchDesc,
ACHCode,
BusinessSegmentID,
BusinessSegmentDesc,
BusinessUnit,
UCCSendStatus,
DocDesc,
WorkflowStatus,
WorkflowStatusDesc,
BookingDate,
ScoreRiskClass,
BalloonPercent,
EmbeddedPrepay,
CashflowTest,
FollowUpVision,
AdvanceRate,
Lessor,
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
Month84,
NPV, 
Active,
CreatedOn,
CreatedBy,
ModifiedOn,
ModifiedBy,
Reason
)
SELECT
@PoolID, 
ContractID,
c.ContractKey,
BaseContractID,
c.VaultName,
p.FundingDate,
p.DefaultDiscountRate,
ProductLineID,
ApplicationID,
CustomerID,
LesseeName,
c.ParentCustomerID,
c.ParentCustomerName,
LesseeAddress1,
LesseeAddress2,
LesseeCity,
LesseeState,
LesseeZip,
LesseePhone,
SicCode,
SICDivisionDesc,
MunicipalLease,
EquipCodeDesc,
DealerID,
DealerName,
AggregationParentID,
AggregationParentName,
AggregationParentID_WithExceptions,
AggregationParentName_WithExceptions,
EquipmentCost,
GrossContract,
CBR,
Residual,
UnAmortizedResidual,
RentalPymt,
BillingFrequency,
PymtsArrears,
c.Term,
VariablePayment,
SecurityDeposit,
PymtsUpfront,
EndingPymtsAdv,
PymtsMade,
CommenceDate,
FirstPymtDate,
TermDate,
NextDueDate,
FinalPymtDate,
DelinStatusCode,
DelinStatusDescription,
Delin31,
Delin61,
Delin91,
DelinStatus_PrevMonth,
EquipState,
EquipCounty,
EquipZip,
BillToState,
BillToZip,
InsStatusCode,
InsStatusCodeDesc,
ProgramTypeID,
ProgramTypeDesc,
ProgramVariation,
ProgramVariationDesc,
Promotion,
PromotionDesc,
PurchaseOptionID,
PurchaseOptionDesc,
PrivateLabel,
RemainingTerm,
LeaseType,
DaysDeferred,
PurchaseOrder, 
GAFI as Bundled,
GAFIAmount as MaintenanceCharges,
PrefundCode as DAWaiver,
CollateralCode,
CollateralCodeDesc,
RecourseVendorID,
RecourseVendorName,
IndirectBilling,
IndirectBillCollectVendorID,
IndirectBillCollectVendorName,
LeveragedIndirectBillCollect,
FixtureOnly,
Concentrations,
VariableTreasury,
PortfolioPurchCode,
PortfolioPurchDesc,
ACHCode,
BusinessSegmentID,
BusinessSegmentDesc,
BusinessUnit,
UCCSendStatus,
DocDesc,
WorkflowStatus,
WorkflowStatusDesc,
BookingDate,
ScoreRiskClass,
BalloonPercent,
EmbeddedPrepay,
CashflowTest,
FollowUpVision,
c.AdvanceRate,
c.Lessor,
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
Month84,
[Securitization].[RoundNPV] (pc.NPV, f.Term) as NPV,
1,
pc.CreatedOn,
pc.CreatedBy,
pc.ModifiedOn,
pc.ModifiedBy,
pc.Reason
FROM Securitization.Contracts c
INNER JOIN Securitization.PoolContract pc on pc.Contract_ContractID = c.ContractID
INNER JOIN Securitization.Pools p on p.id = pc.Pool_ID
INNER JOIN Securitization.FacilityLessor fl on p.FacilityLessor_ID = fl.id
INNER JOIN Securitization.Facilities f on f.ID = fl.Facility_ID
LEFT JOIN #Cashflow t on t.Contractkey = c.ContractKey
where p.id=@PoolID
and pc.InFacilityPool=0
and pc.Active=1



		
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

* [[Securitization].[spCalculateNPVByPoolID]](Securitization_spCalculateNPVByPoolID.md)
* [[Securitization].[RoundNPV]](../Functions/Scalar-valued_Functions/Securitization_RoundNPV.md)
* [Archive]
* [Archive].[PoolExport]
* [dbo].[vContract_Transaction]
* [Securitization].[Contracts]
* [Securitization].[Facilities]
* [Securitization].[FacilityLessor]
* [Securitization].[PoolContract]
* [Securitization].[Pools]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

