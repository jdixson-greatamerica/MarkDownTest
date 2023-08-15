#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.SP_Facility_CT_All

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[SP_Facility_CT_All]

---

## <a name="#properties"></a>Properties

| Property | Value |
|---|---|
| ANSI Nulls On | NO |
| Quoted Identifier On | NO |


---

## <a name="#parameters"></a>Parameters

| Name | Data Type | Max Length (Bytes) |
|---|---|---|
| @FacilityID | int | 4 |
| @UserID | nvarchar(255) | 510 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
SET QUOTED_IDENTIFIER OFF
GO
SET ANSI_NULLS OFF
GO
-- History:
--		Name:	Chad Feldmann
--		Date:	4/18/2019
--		Desc:	Ticket #PRJ0040256 - Remove ConcentrationTest (15,17,67,68,73,82)
-- =============================================
CREATE PROCEDURE [Securitization].[SP_Facility_CT_All]
	@FacilityID [int],
	@UserID [nvarchar](255)
AS
SET NOCOUNT ON;
select 1 as success--Needed to be called by ssis
DELETE FROM [Securitization].[FacilityConcentration] WHERE Facility_ID=@FacilityID;

exec [Securitization].[sp_Facility_CT1_EquipLocUSVI] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT2_EquipLocPR] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT3_EquipLocBVI] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT4_WaiverDA] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT5_TotalLeveragedThirdPartyCollectL] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT6_TotalLeveragedThirdPartyBilled] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT7_ThirdPartyBilledTotal] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT8_OfficeEquip] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT9_RemTerm72but84] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT10_QuarterlyQ] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT11_SkipPmtSkips] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT12_AnnualA] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT13_SemiAnnualsSA] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT14_VariableV] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT16_DeferredDeferred] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT18_TotalnonmonthlyQSkipsASAVDeferredGAFI] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT19_Municipals] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT20_TotalThirdParty] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT21_PO] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT22_Software] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT23_VendorLeasingLikeEquipment] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT24_InventoryFinance] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT25_VendorPrepayVP] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT26_DocRecourseTotalDR] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT27_XeroxVendorGroup] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT28_BundledLeaseContracts] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT29_XeroxVendorGroupBundled] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT30_RiskofLoss] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT31_LateFileNonperfectedSecurityInterest] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT32_PartnersinLeasingTotalPIL] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT33_FixtureOnlyAgreements] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT34_StateCA] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT35_StateFL] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT36_StateTX] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT37_TotalLDRVPPIL] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT_Residual] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT39_Balloons] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT40_LargeBalloons] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT41_ConditionalSales] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT42_ServicesIndustriesSICCode] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT43_BulkPurchase] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT44_ThirdPartyCollect] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT45_PrevMonthDelin31] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT46_EquipLocCanada] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT47_EmbeddedPrepayment] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT48_LargestCustomer] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT49_LargestSingleStateexCAFLTX] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT50_StateCAFLTX] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT51_SingleLargestLeveragedThirdPartyCollect] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT52_SingleLargestLeveragedThirdPartyBilled] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT53_ThirdPartyBilledIndividual] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT54_LargestThirdParty] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT55_Top5ThirdParty] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT56_Top15Customers] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT57_Top7Customers] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT58_DocRecourseIndividualVendor3rdParty] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT59_LargestVendor] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT60_LargestVendorExcludingXerox] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT61_MaxToptwovendorsbundled] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT62_MaxThirdthrough10thbundled] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT63_MaxAllothervendorsbundled] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT64_PartnersinLeasingIndividual] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT65_Top10Customers] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT66_LargestVendor] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT69_EquipLocGuam] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT70_LargestSICCodeFourdigit] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT71_Top5Customers] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT72_Top5SICCodesFourdigit] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT_ResidualGreaterThan10Dollars] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT75_TotalForeignLocation] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT76_DeferredDeferredGAFV] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT77_TotalnonmonthlyQSkipsASAVDeferredGAFV] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT78_JuniorLienNote] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT80_NonPerfectedSecurityInterest] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT81_SecuredNotes_InventoryFinance] @FacilityID, @UserID;
exec [Securitization].[sp_Facility_CT86_VaultName] @FacilityID, @UserID;

GO

```


---

## <a name="#uses"></a>Uses

* [[Securitization].[sp_Facility_CT_Residual]](Securitization_sp_Facility_CT_Residual.md)
* [[Securitization].[sp_Facility_CT_ResidualGreaterThan10Dollars]](Securitization_sp_Facility_CT_ResidualGreaterThan10Dollars.md)
* [[Securitization].[sp_Facility_CT1_EquipLocUSVI]](Securitization_sp_Facility_CT1_EquipLocUSVI.md)
* [[Securitization].[sp_Facility_CT10_QuarterlyQ]](Securitization_sp_Facility_CT10_QuarterlyQ.md)
* [[Securitization].[sp_Facility_CT11_SkipPmtSkips]](Securitization_sp_Facility_CT11_SkipPmtSkips.md)
* [[Securitization].[sp_Facility_CT12_AnnualA]](Securitization_sp_Facility_CT12_AnnualA.md)
* [[Securitization].[sp_Facility_CT13_SemiAnnualsSA]](Securitization_sp_Facility_CT13_SemiAnnualsSA.md)
* [[Securitization].[sp_Facility_CT14_VariableV]](Securitization_sp_Facility_CT14_VariableV.md)
* [[Securitization].[sp_Facility_CT16_DeferredDeferred]](Securitization_sp_Facility_CT16_DeferredDeferred.md)
* [[Securitization].[sp_Facility_CT18_TotalnonmonthlyQSkipsASAVDeferredGAFI]](Securitization_sp_Facility_CT18_TotalnonmonthlyQSkipsASAVDeferredGAFI.md)
* [[Securitization].[sp_Facility_CT19_Municipals]](Securitization_sp_Facility_CT19_Municipals.md)
* [[Securitization].[sp_Facility_CT2_EquipLocPR]](Securitization_sp_Facility_CT2_EquipLocPR.md)
* [[Securitization].[sp_Facility_CT20_TotalThirdParty]](Securitization_sp_Facility_CT20_TotalThirdParty.md)
* [[Securitization].[sp_Facility_CT21_PO]](Securitization_sp_Facility_CT21_PO.md)
* [[Securitization].[sp_Facility_CT22_Software]](Securitization_sp_Facility_CT22_Software.md)
* [[Securitization].[sp_Facility_CT23_VendorLeasingLikeEquipment]](Securitization_sp_Facility_CT23_VendorLeasingLikeEquipment.md)
* [[Securitization].[sp_Facility_CT24_InventoryFinance]](Securitization_sp_Facility_CT24_InventoryFinance.md)
* [[Securitization].[sp_Facility_CT25_VendorPrepayVP]](Securitization_sp_Facility_CT25_VendorPrepayVP.md)
* [[Securitization].[sp_Facility_CT26_DocRecourseTotalDR]](Securitization_sp_Facility_CT26_DocRecourseTotalDR.md)
* [[Securitization].[sp_Facility_CT27_XeroxVendorGroup]](Securitization_sp_Facility_CT27_XeroxVendorGroup.md)
* [[Securitization].[sp_Facility_CT28_BundledLeaseContracts]](Securitization_sp_Facility_CT28_BundledLeaseContracts.md)
* [[Securitization].[sp_Facility_CT29_XeroxVendorGroupBundled]](Securitization_sp_Facility_CT29_XeroxVendorGroupBundled.md)
* [[Securitization].[sp_Facility_CT3_EquipLocBVI]](Securitization_sp_Facility_CT3_EquipLocBVI.md)
* [[Securitization].[sp_Facility_CT30_RiskofLoss]](Securitization_sp_Facility_CT30_RiskofLoss.md)
* [[Securitization].[sp_Facility_CT31_LateFileNonperfectedSecurityInterest]](Securitization_sp_Facility_CT31_LateFileNonperfectedSecurityInterest.md)
* [[Securitization].[sp_Facility_CT32_PartnersinLeasingTotalPIL]](Securitization_sp_Facility_CT32_PartnersinLeasingTotalPIL.md)
* [[Securitization].[sp_Facility_CT33_FixtureOnlyAgreements]](Securitization_sp_Facility_CT33_FixtureOnlyAgreements.md)
* [[Securitization].[sp_Facility_CT34_StateCA]](Securitization_sp_Facility_CT34_StateCA.md)
* [[Securitization].[sp_Facility_CT35_StateFL]](Securitization_sp_Facility_CT35_StateFL.md)
* [[Securitization].[sp_Facility_CT36_StateTX]](Securitization_sp_Facility_CT36_StateTX.md)
* [[Securitization].[sp_Facility_CT37_TotalLDRVPPIL]](Securitization_sp_Facility_CT37_TotalLDRVPPIL.md)
* [[Securitization].[sp_Facility_CT39_Balloons]](Securitization_sp_Facility_CT39_Balloons.md)
* [[Securitization].[sp_Facility_CT4_WaiverDA]](Securitization_sp_Facility_CT4_WaiverDA.md)
* [[Securitization].[sp_Facility_CT40_LargeBalloons]](Securitization_sp_Facility_CT40_LargeBalloons.md)
* [[Securitization].[sp_Facility_CT41_ConditionalSales]](Securitization_sp_Facility_CT41_ConditionalSales.md)
* [[Securitization].[sp_Facility_CT42_ServicesIndustriesSICCode]](Securitization_sp_Facility_CT42_ServicesIndustriesSICCode.md)
* [[Securitization].[sp_Facility_CT43_BulkPurchase]](Securitization_sp_Facility_CT43_BulkPurchase.md)
* [[Securitization].[sp_Facility_CT44_ThirdPartyCollect]](Securitization_sp_Facility_CT44_ThirdPartyCollect.md)
* [[Securitization].[sp_Facility_CT45_PrevMonthDelin31]](Securitization_sp_Facility_CT45_PrevMonthDelin31.md)
* [[Securitization].[sp_Facility_CT46_EquipLocCanada]](Securitization_sp_Facility_CT46_EquipLocCanada.md)
* [[Securitization].[sp_Facility_CT47_EmbeddedPrepayment]](Securitization_sp_Facility_CT47_EmbeddedPrepayment.md)
* [[Securitization].[sp_Facility_CT48_LargestCustomer]](Securitization_sp_Facility_CT48_LargestCustomer.md)
* [[Securitization].[sp_Facility_CT49_LargestSingleStateexCAFLTX]](Securitization_sp_Facility_CT49_LargestSingleStateexCAFLTX.md)
* [[Securitization].[sp_Facility_CT5_TotalLeveragedThirdPartyCollectL]](Securitization_sp_Facility_CT5_TotalLeveragedThirdPartyCollectL.md)
* [[Securitization].[sp_Facility_CT50_StateCAFLTX]](Securitization_sp_Facility_CT50_StateCAFLTX.md)
* [[Securitization].[sp_Facility_CT51_SingleLargestLeveragedThirdPartyCollect]](Securitization_sp_Facility_CT51_SingleLargestLeveragedThirdPartyCollect.md)
* [[Securitization].[sp_Facility_CT52_SingleLargestLeveragedThirdPartyBilled]](Securitization_sp_Facility_CT52_SingleLargestLeveragedThirdPartyBilled.md)
* [[Securitization].[sp_Facility_CT53_ThirdPartyBilledIndividual]](Securitization_sp_Facility_CT53_ThirdPartyBilledIndividual.md)
* [[Securitization].[sp_Facility_CT54_LargestThirdParty]](Securitization_sp_Facility_CT54_LargestThirdParty.md)
* [[Securitization].[sp_Facility_CT55_Top5ThirdParty]](Securitization_sp_Facility_CT55_Top5ThirdParty.md)
* [[Securitization].[sp_Facility_CT56_Top15Customers]](Securitization_sp_Facility_CT56_Top15Customers.md)
* [[Securitization].[sp_Facility_CT57_Top7Customers]](Securitization_sp_Facility_CT57_Top7Customers.md)
* [[Securitization].[sp_Facility_CT58_DocRecourseIndividualVendor3rdParty]](Securitization_sp_Facility_CT58_DocRecourseIndividualVendor3rdParty.md)
* [[Securitization].[sp_Facility_CT59_LargestVendor]](Securitization_sp_Facility_CT59_LargestVendor.md)
* [[Securitization].[sp_Facility_CT6_TotalLeveragedThirdPartyBilled]](Securitization_sp_Facility_CT6_TotalLeveragedThirdPartyBilled.md)
* [[Securitization].[sp_Facility_CT60_LargestVendorExcludingXerox]](Securitization_sp_Facility_CT60_LargestVendorExcludingXerox.md)
* [[Securitization].[sp_Facility_CT61_MaxToptwovendorsbundled]](Securitization_sp_Facility_CT61_MaxToptwovendorsbundled.md)
* [[Securitization].[sp_Facility_CT62_MaxThirdthrough10thbundled]](Securitization_sp_Facility_CT62_MaxThirdthrough10thbundled.md)
* [[Securitization].[sp_Facility_CT63_MaxAllothervendorsbundled]](Securitization_sp_Facility_CT63_MaxAllothervendorsbundled.md)
* [[Securitization].[sp_Facility_CT64_PartnersinLeasingIndividual]](Securitization_sp_Facility_CT64_PartnersinLeasingIndividual.md)
* [[Securitization].[sp_Facility_CT65_Top10Customers]](Securitization_sp_Facility_CT65_Top10Customers.md)
* [[Securitization].[sp_Facility_CT66_LargestVendor]](Securitization_sp_Facility_CT66_LargestVendor.md)
* [[Securitization].[sp_Facility_CT69_EquipLocGuam]](Securitization_sp_Facility_CT69_EquipLocGuam.md)
* [[Securitization].[sp_Facility_CT7_ThirdPartyBilledTotal]](Securitization_sp_Facility_CT7_ThirdPartyBilledTotal.md)
* [[Securitization].[sp_Facility_CT70_LargestSICCodeFourdigit]](Securitization_sp_Facility_CT70_LargestSICCodeFourdigit.md)
* [[Securitization].[sp_Facility_CT71_Top5Customers]](Securitization_sp_Facility_CT71_Top5Customers.md)
* [[Securitization].[sp_Facility_CT72_Top5SICCodesFourdigit]](Securitization_sp_Facility_CT72_Top5SICCodesFourdigit.md)
* [[Securitization].[sp_Facility_CT75_TotalForeignLocation]](Securitization_sp_Facility_CT75_TotalForeignLocation.md)
* [[Securitization].[sp_Facility_CT76_DeferredDeferredGAFV]](Securitization_sp_Facility_CT76_DeferredDeferredGAFV.md)
* [[Securitization].[sp_Facility_CT77_TotalnonmonthlyQSkipsASAVDeferredGAFV]](Securitization_sp_Facility_CT77_TotalnonmonthlyQSkipsASAVDeferredGAFV.md)
* [[Securitization].[sp_Facility_CT78_JuniorLienNote]](Securitization_sp_Facility_CT78_JuniorLienNote.md)
* [[Securitization].[sp_Facility_CT8_OfficeEquip]](Securitization_sp_Facility_CT8_OfficeEquip.md)
* [[Securitization].[sp_Facility_CT80_NonPerfectedSecurityInterest]](Securitization_sp_Facility_CT80_NonPerfectedSecurityInterest.md)
* [[Securitization].[sp_Facility_CT81_SecuredNotes_InventoryFinance]](Securitization_sp_Facility_CT81_SecuredNotes_InventoryFinance.md)
* [[Securitization].[sp_Facility_CT86_VaultName]](Securitization_sp_Facility_CT86_VaultName.md)
* [[Securitization].[sp_Facility_CT9_RemTerm72but84]](Securitization_sp_Facility_CT9_RemTerm72but84.md)
* [Securitization]
* [Securitization].[FacilityConcentration]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

