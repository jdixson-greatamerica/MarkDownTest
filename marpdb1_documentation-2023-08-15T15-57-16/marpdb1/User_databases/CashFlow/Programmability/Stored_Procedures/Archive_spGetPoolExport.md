#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Archive.spGetPoolExport

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Archive].[spGetPoolExport]

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


---

## <a name="#sqlscript"></a>SQL Script

```sql
SET QUOTED_IDENTIFIER OFF
GO

-- =============================================
-- Author:		Chad Feldmann
-- Create date: 09/17/2013
-- Description:	Returns records need to generate the export for an archived pool
--Test:  [Archive].[spGetPoolExport] 42
-- History:
--		Name:	Chad Feldmann
--		Date:	4/12/2019
--		Desc:	Ticket #PRJ0040256 - Adding VariableTreasury field.  Removing A1Term, DocRepName, BusinessSegmentID, BusinessSegmentDesc, and CashflowTest
--
--	Author:		John Thadison
--	Edit Date:	03/08/2022
--	Description: STRY0019475- Add FollowupVision, VaultName, ParentCustomerName, and ParentCustomerID to exports. 
-- =============================================
CREATE PROCEDURE [Archive].[spGetPoolExport] 
	-- Add the parameters for the stored procedure here
	@PoolID int

AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;
	
select 
ContractID,
BaseContractID,
e.VaultName,
p.FundingDate,
p.DefaultDiscountRate,
ProductLineID,
ApplicationID,
CustomerID,
LesseeName,
e.ParentCustomerID,
e.ParentCustomerName,
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
e.Term,
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
UCCSendStatus,
DocDesc,
BookingDate,
ScoreRiskClass,
BalloonPercent,
EmbeddedPrepay,
e.FollowUpVision,
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
[Securitization].[RoundNPV] (e.NPV, f.Term) NPV
FROM Archive.PoolExport e
inner join Securitization.Pools p on p.id = e.Pool_ID
inner join Securitization.FacilityLessor fl on p.FacilityLessor_ID = fl.id
inner join Securitization.Facilities f on f.ID = fl.Facility_ID
where p.id=@PoolID
and e.Active=1


END
GO

```


---

## <a name="#uses"></a>Uses

* [[Securitization].[RoundNPV]](../Functions/Scalar-valued_Functions/Securitization_RoundNPV.md)
* [Archive]
* [Archive].[PoolExport]
* [Securitization].[Facilities]
* [Securitization].[FacilityLessor]
* [Securitization].[Pools]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

