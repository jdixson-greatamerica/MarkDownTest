#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Archive.spGetConflictingContractsByPoolId

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Archive].[spGetConflictingContractsByPoolId]

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
| @PoolId | int | 4 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
-- =============================================
-- Author:		Chad Feldmann
-- Create date: 09/17/2013
-- Description:	Gets a list of contracts in active funding pools that are in conflict(duplicates) of contracts in the current pool.
--Test:  [Securitization].[spGetConflictingContractsByPoolId] 60
-- History:
--		Name:	Chad Feldmann
--		Date:	4/12/2019
--		Desc:	Ticket #PRJ0040256 - Adding VariableTreasury field.  Removing A1Term, DocRepName, BusinessSegmentID, BusinessSegmentDesc, and CashflowTest
-- =============================================
CREATE PROCEDURE [Archive].[spGetConflictingContractsByPoolId]
	@PoolId int
AS
BEGIN

with PotentialConflicts (ContractID) AS
(
select distinct Contract_ContractID from Securitization.PoolContract pc
inner join Securitization.Pools p on p.id = pc.Pool_ID
where p.Status in(1,2)
and p.id <> @PoolId --Contracts not in this pool
and pc.InFacilityPool=0 --Only check contracts in working pool
and Coalesce(pc.PendingStatus,pc.Active) = 1 --Only check active contracts
and p.Funding = 1 --Only check on funding pools
)



select e.ContractID,
e.ContractKey,
e.ACHCode,
e.AggregationParentID,
e.AggregationParentName,
e.ApplicationID,
e.BalloonPercent,
e.BaseContractID,
e.BillingFrequency,
e.BillToState,
e.BillToZip,
e.BookingDate,
e.BusinessUnit,
e.CBR,
e.CollateralCode,
e.CollateralCodeDesc,
e.CommenceDate,
e.Concentrations,
e.VariableTreasury,
e.ContractStatusDesc,
e.ContractStatusID,
e.CustomerID,
e.DaysDeferred,
e.DealerID,
e.DealerName,
e.Delin31,
e.Delin61,
e.Delin91,
e.DelinStatus_PrevMonth,
e.DelinStatusCode,
e.DelinStatusDescription,
e.DiscountRate,
e.EmbeddedPrepay,
e.EndingPymtsAdv,
e.EquipCodeDesc,
e.EquipCounty,
e.EquipmentCost,
e.EquipState,
e.EquipZip,
e.FinalPymtDate,
e.FirstPymtDate,
e.FixtureOnly,
e.GAFI,
e.GAFIAmount,
e.GrossContract,
e.IndirectBillCollectVendorID,
e.IndirectBillCollectVendorName,
e.IndirectBilling,
e.InsStatusCode,
e.InsStatusCodeDesc,
e.LeaseType,
e.LegalStatusCode,
e.LegalStatusDesc,
e.LesseeAddress1,
e.LesseeAddress2,
e.LesseeCity,
e.LesseeName,
e.LesseePhone,
e.LesseeState,
e.LesseeZip,
e.Lessor,
e.LeveragedIndirectBillCollect,
e.MunicipalLease,
e.NextDueDate,
e.OutstandingOriginals,
e.AggregationParentID_WithExceptions,
e.AggregationParentName_WithExceptions, 
e.PortfolioPurchCode,
e.PortfolioPurchDesc,
e.PrefundCode,
e.PrivateLabel,
e.ProductLineID,
e.ProductLineDesc,
e.ProgramTypeDesc,
e.ProgramTypeID,
e.ProgramVariation,
e.ProgramVariationDesc,
e.Promotion,
e.PromotionDesc,
e.PurchaseOptionDesc,
e.PurchaseOptionID,
e.PurchaseOrder,
e.PymtsArrears,
e.PymtsMade,
e.PymtsUpFront,
e.RecourseVendorID,
e.RecourseVendorName,
e.RemainingTerm,
e.RentalPymt,
e.Residual,
e.ScoreRiskClass,
e.SecurityDeposit,
e.SICCode,
e.SICDivisionDesc,
e.Term,
e.TermDate,
e.UnAmortizedResidual,
e.VariablePayment,
e.CreatedOn,
e.CreatedBy,
e.ModifiedOn,
e.ModifiedBy,
NEWID() as Guid,
0 as PoolContractID, 
e.Active as PoolContractActive, 
e.Reason as PoolContractReason, 
cast(1 as bit) as PoolContractInFacilityPool,
e.NPV as PoolContractNPV, 
e.CreatedOn as PoolContractCreatedOn, 
e.CreatedBy as PoolContractCreatedBy, 
e.ModifiedOn as PoolContractModifiedOn, 
e.ModifiedBy as PoolContractModifiedBy, 
e.Active as PoolContractPendingStatus, 
e.Pool_ID
	 
from Archive.PoolExport as e 
inner join Securitization.Pools as Pool on e.Pool_ID = Pool.ID 
inner join PotentialConflicts conf on conf.ContractID=e.ContractID
where 
e.active = 1
and e.Pool_ID = @PoolId 
and conf.ContractID is not null

END



GO

```


---

## <a name="#uses"></a>Uses

* [Archive]
* [Archive].[PoolExport]
* [Securitization].[PoolContract]
* [Securitization].[Pools]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

