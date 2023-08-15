#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.spGetFacilityExport

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[spGetFacilityExport]

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
| @FacilityID | int | 4 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
SET QUOTED_IDENTIFIER OFF
GO

-- =============================================
-- Author:		Chad Feldmann
-- Create date: 05/08/2013
-- Description:	returns contract information associated with a given facility
--Test:  [Securitization].[spGetFacilityExport] 1
-- History:
--		Name:	Chad Feldmann
--		Date:	4/12/2019
--		Desc:	Ticket #PRJ0040256 - Adding VariableTreasury field.  Removing A1Term, DocRepName, BusinessSegmentID, BusinessSegmentDesc, and CashflowTest
--		Name:		John Thadison
--		Date:	04/14/2022
--		Desc: STRY0019475- Add FollowupVision, VaultName, ParentCustomerName, and ParentCustomerID to exports.
-- =============================================
CREATE PROCEDURE [Securitization].[spGetFacilityExport]
	-- Add the parameters for the stored procedure here
	@FacilityID int

AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;
DECLARE @StartDate DateTime;
SET @StartDate = CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)--Get starting date of current month

select 
ContractID,
BaseContractID,
c.VaultName,
c.FundingDate,
c.DiscountRate as DefaultDiscountRate,
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
BusinessUnit,
c.UCCSendStatus,
c.DocDesc,
BookingDate,
ScoreRiskClass,
BalloonPercent,
EmbeddedPrepay,
FollowUpVision,
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
[Securitization].[RoundNPV] (c.CurrentMonthNPV, f.Term) as NPV
FROM Securitization.Contracts c
inner join Securitization.FacilityLessor fl 
	on c.Lessor = fl.LessorID 
inner join Securitization.Facilities f
	on f.id = fl.Facility_ID
left join (SELECT Contractkey, 
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
test.TransactionAmount,
RANK() OVER (Partition By test.contractkey ORDER BY test.seq) as seq
FROM 
	(
		SELECT	Seq, 
		ct.ContractKey,
		sum(TransactionAmount) as TransactionAmount
		from dbo.vContract_Transaction ct
		inner join Securitization.Contracts c on c.ContractKey = ct.ContractKey
		inner join Securitization.FacilityLessor fl 
			on c.Lessor = fl.LessorID 
		inner join Securitization.Facilities f
			on f.id = fl.Facility_ID
		where ct.TransactionDate >= @StartDate 
			and f.id=@FacilityID
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
) as cashflow on cashflow.ContractKey = c.ContractKey
where f.id=@FacilityID




END
GO

```


---

## <a name="#uses"></a>Uses

* [[Securitization].[RoundNPV]](../Functions/Scalar-valued_Functions/Securitization_RoundNPV.md)
* [dbo].[vContract_Transaction]
* [Securitization]
* [Securitization].[Contracts]
* [Securitization].[Facilities]
* [Securitization].[FacilityLessor]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

