#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.SP_CT_All

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[SP_CT_All]

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
| @PoolID | int | 4 |
| @UserID | nvarchar(255) | 510 |


---

## <a name="#sqlscript"></a>SQL Script

```sql
SET QUOTED_IDENTIFIER OFF
GO
SET ANSI_NULLS OFF
GO


-- History:
--		Name: JDixson
--		Date: 5/16/2017
--		Desc: PRJ0016920 Including ConcentrationTest 80 and 81
--		Name:	Chad Feldmann
--		Date:	4/18/2019
--		Desc:	Ticket #PRJ0040256 - Remove ConcentrationTest (15,17,67,68,73,82)
-- =============================================

CREATE PROCEDURE [Securitization].[SP_CT_All]
	@PoolID [int],
	@UserID [nvarchar](255)
AS
DELETE FROM [Securitization].[PoolConcentration] WHERE Pool_ID=@PoolID;

exec [Securitization].[sp_CT1_EquipLocUSVI] @PoolID, @UserID;
exec [Securitization].[sp_CT2_EquipLocPR] @PoolID, @UserID;
exec [Securitization].[sp_CT3_EquipLocBVI] @PoolID, @UserID;
exec [Securitization].[sp_CT4_WaiverDA] @PoolID, @UserID;
exec [Securitization].[sp_CT5_TotalLeveragedThirdPartyCollectL] @PoolID, @UserID;
exec [Securitization].[sp_CT6_TotalLeveragedThirdPartyBilled] @PoolID, @UserID;
exec [Securitization].[sp_CT7_ThirdPartyBilledTotal] @PoolID, @UserID;
exec [Securitization].[sp_CT8_OfficeEquip] @PoolID, @UserID;
exec [Securitization].[sp_CT9_RemTerm72but84] @PoolID, @UserID;
exec [Securitization].[sp_CT10_QuarterlyQ] @PoolID, @UserID;
exec [Securitization].[sp_CT11_SkipPmtSkips] @PoolID, @UserID;
exec [Securitization].[sp_CT12_AnnualA] @PoolID, @UserID;
exec [Securitization].[sp_CT13_SemiAnnualsSA] @PoolID, @UserID;
exec [Securitization].[sp_CT14_VariableV] @PoolID, @UserID;
exec [Securitization].[sp_CT16_DeferredDeferred] @PoolID, @UserID;
exec [Securitization].[sp_CT18_TotalnonmonthlyQSkipsASAVDeferredGAFI] @PoolID, @UserID;
exec [Securitization].[sp_CT19_Municipals] @PoolID, @UserID;
exec [Securitization].[sp_CT20_TotalThirdParty] @PoolID, @UserID;
exec [Securitization].[sp_CT21_PO] @PoolID, @UserID;
exec [Securitization].[sp_CT22_Software] @PoolID, @UserID;
exec [Securitization].[sp_CT23_VendorLeasingLikeEquipment] @PoolID, @UserID;
exec [Securitization].[sp_CT24_InventoryFinance] @PoolID, @UserID;
exec [Securitization].[sp_CT25_VendorPrepayVP] @PoolID, @UserID;
exec [Securitization].[sp_CT26_DocRecourseTotalDR] @PoolID, @UserID;
exec [Securitization].[sp_CT27_XeroxVendorGroup] @PoolID, @UserID;
exec [Securitization].[sp_CT28_BundledLeaseContracts] @PoolID, @UserID;
exec [Securitization].[sp_CT29_XeroxVendorGroupBundled] @PoolID, @UserID;
exec [Securitization].[sp_CT30_RiskofLoss] @PoolID, @UserID;
exec [Securitization].[sp_CT31_LateFileNonperfectedSecurityInterest] @PoolID, @UserID;
exec [Securitization].[sp_CT32_PartnersinLeasingTotalPIL] @PoolID, @UserID;
exec [Securitization].[sp_CT33_FixtureOnlyAgreements] @PoolID, @UserID;
exec [Securitization].[sp_CT34_StateCA] @PoolID, @UserID;
exec [Securitization].[sp_CT35_StateFL] @PoolID, @UserID;
exec [Securitization].[sp_CT36_StateTX] @PoolID, @UserID;
exec [Securitization].[sp_CT37_TotalLDRVPPIL] @PoolID, @UserID;
exec [Securitization].[sp_CT39_Balloons] @PoolID, @UserID;
exec [Securitization].[sp_CT40_LargeBalloons] @PoolID, @UserID;
exec [Securitization].[sp_CT41_ConditionalSales] @PoolID, @UserID;
exec [Securitization].[sp_CT42_ServicesIndustriesSICCode] @PoolID, @UserID;
exec [Securitization].[sp_CT43_BulkPurchase] @PoolID, @UserID;
exec [Securitization].[sp_CT44_ThirdPartyCollect] @PoolID, @UserID;
exec [Securitization].[sp_CT45_PrevMonthDelin31] @PoolID, @UserID;
exec [Securitization].[sp_CT46_EquipLocCanada] @PoolID, @UserID;
exec [Securitization].[sp_CT47_EmbeddedPrepayment] @PoolID, @UserID;
exec [Securitization].[sp_CT48_LargestCustomer] @PoolID, @UserID;
exec [Securitization].[sp_CT49_LargestSingleStateexCAFLTX] @PoolID, @UserID;
exec [Securitization].[sp_CT50_StateCAFLTX] @PoolID, @UserID;
exec [Securitization].[sp_CT51_SingleLargestLeveragedThirdPartyCollect] @PoolID, @UserID;
exec [Securitization].[sp_CT52_SingleLargestLeveragedThirdPartyBilled] @PoolID, @UserID;
exec [Securitization].[sp_CT53_ThirdPartyBilledIndividual] @PoolID, @UserID;
exec [Securitization].[sp_CT54_LargestThirdParty] @PoolID, @UserID;
exec [Securitization].[sp_CT55_Top5ThirdParty] @PoolID, @UserID;
exec [Securitization].[sp_CT56_Top15Customers] @PoolID, @UserID;
exec [Securitization].[sp_CT57_Top7Customers] @PoolID, @UserID;
exec [Securitization].[sp_CT58_DocRecourseIndividualVendor3rdParty] @PoolID, @UserID;
exec [Securitization].[sp_CT59_LargestVendor] @PoolID, @UserID;
exec [Securitization].[sp_CT60_LargestVendorExcludingXerox] @PoolID, @UserID;
exec [Securitization].[sp_CT61_MaxToptwovendorsbundled] @PoolID, @UserID;
exec [Securitization].[sp_CT62_MaxThirdthrough10thbundled] @PoolID, @UserID;
exec [Securitization].[sp_CT63_MaxAllothervendorsbundled] @PoolID, @UserID;
exec [Securitization].[sp_CT64_PartnersinLeasingIndividual] @PoolID, @UserID;
exec [Securitization].[sp_CT65_Top10Customers] @PoolID, @UserID;
exec [Securitization].[sp_CT66_LargestVendor] @PoolID, @UserID;
exec [Securitization].[sp_CT69_EquipLocGuam] @PoolID, @UserID;
exec [Securitization].[sp_CT70_LargestSICCodeFourdigit] @PoolID, @UserID;
exec [Securitization].[sp_CT71_Top5Customers] @PoolID, @UserID;
exec [Securitization].[sp_CT72_Top5SICCodesFourdigit] @PoolID, @UserID;
exec [Securitization].[sp_CT_ResidualGreaterThan10Dollars] @PoolID, @UserID;
exec [Securitization].[sp_CT75_TotalForeignLocation] @PoolID, @UserID;
exec [Securitization].[sp_CT76_DeferredDeferredGAFV] @PoolID, @UserID;
exec [Securitization].[sp_CT77_TotalnonmonthlyQSkipsASAVDeferredGAFV] @PoolID, @UserID;
exec [Securitization].[sp_CT78_JuniorLienNote] @PoolID, @UserID;
EXEC [Securitization].[sp_CT80_NonPerfectedSecurityInterest] @PoolID, @UserID
EXEC [Securitization].[sp_CT81_SecuredNotes_InventoryFinance] @PoolID, @UserID
EXEC [Securitization].[sp_CT86_VaultName] @PoolID, @UserID
GO

```


---

## <a name="#uses"></a>Uses

* [[Securitization].[sp_CT_ResidualGreaterThan10Dollars]](Securitization_sp_CT_ResidualGreaterThan10Dollars.md)
* [[Securitization].[sp_CT1_EquipLocUSVI]](Securitization_sp_CT1_EquipLocUSVI.md)
* [[Securitization].[sp_CT10_QuarterlyQ]](Securitization_sp_CT10_QuarterlyQ.md)
* [[Securitization].[sp_CT11_SkipPmtSkips]](Securitization_sp_CT11_SkipPmtSkips.md)
* [[Securitization].[sp_CT12_AnnualA]](Securitization_sp_CT12_AnnualA.md)
* [[Securitization].[sp_CT13_SemiAnnualsSA]](Securitization_sp_CT13_SemiAnnualsSA.md)
* [[Securitization].[sp_CT14_VariableV]](Securitization_sp_CT14_VariableV.md)
* [[Securitization].[sp_CT16_DeferredDeferred]](Securitization_sp_CT16_DeferredDeferred.md)
* [[Securitization].[sp_CT18_TotalnonmonthlyQSkipsASAVDeferredGAFI]](Securitization_sp_CT18_TotalnonmonthlyQSkipsASAVDeferredGAFI.md)
* [[Securitization].[sp_CT19_Municipals]](Securitization_sp_CT19_Municipals.md)
* [[Securitization].[sp_CT2_EquipLocPR]](Securitization_sp_CT2_EquipLocPR.md)
* [[Securitization].[sp_CT20_TotalThirdParty]](Securitization_sp_CT20_TotalThirdParty.md)
* [[Securitization].[sp_CT21_PO]](Securitization_sp_CT21_PO.md)
* [[Securitization].[sp_CT22_Software]](Securitization_sp_CT22_Software.md)
* [[Securitization].[sp_CT23_VendorLeasingLikeEquipment]](Securitization_sp_CT23_VendorLeasingLikeEquipment.md)
* [[Securitization].[sp_CT24_InventoryFinance]](Securitization_sp_CT24_InventoryFinance.md)
* [[Securitization].[sp_CT25_VendorPrepayVP]](Securitization_sp_CT25_VendorPrepayVP.md)
* [[Securitization].[sp_CT26_DocRecourseTotalDR]](Securitization_sp_CT26_DocRecourseTotalDR.md)
* [[Securitization].[sp_CT27_XeroxVendorGroup]](Securitization_sp_CT27_XeroxVendorGroup.md)
* [[Securitization].[sp_CT28_BundledLeaseContracts]](Securitization_sp_CT28_BundledLeaseContracts.md)
* [[Securitization].[sp_CT29_XeroxVendorGroupBundled]](Securitization_sp_CT29_XeroxVendorGroupBundled.md)
* [[Securitization].[sp_CT3_EquipLocBVI]](Securitization_sp_CT3_EquipLocBVI.md)
* [[Securitization].[sp_CT30_RiskofLoss]](Securitization_sp_CT30_RiskofLoss.md)
* [[Securitization].[sp_CT31_LateFileNonperfectedSecurityInterest]](Securitization_sp_CT31_LateFileNonperfectedSecurityInterest.md)
* [[Securitization].[sp_CT32_PartnersinLeasingTotalPIL]](Securitization_sp_CT32_PartnersinLeasingTotalPIL.md)
* [[Securitization].[sp_CT33_FixtureOnlyAgreements]](Securitization_sp_CT33_FixtureOnlyAgreements.md)
* [[Securitization].[sp_CT34_StateCA]](Securitization_sp_CT34_StateCA.md)
* [[Securitization].[sp_CT35_StateFL]](Securitization_sp_CT35_StateFL.md)
* [[Securitization].[sp_CT36_StateTX]](Securitization_sp_CT36_StateTX.md)
* [[Securitization].[sp_CT37_TotalLDRVPPIL]](Securitization_sp_CT37_TotalLDRVPPIL.md)
* [[Securitization].[sp_CT39_Balloons]](Securitization_sp_CT39_Balloons.md)
* [[Securitization].[sp_CT4_WaiverDA]](Securitization_sp_CT4_WaiverDA.md)
* [[Securitization].[sp_CT40_LargeBalloons]](Securitization_sp_CT40_LargeBalloons.md)
* [[Securitization].[sp_CT41_ConditionalSales]](Securitization_sp_CT41_ConditionalSales.md)
* [[Securitization].[sp_CT42_ServicesIndustriesSICCode]](Securitization_sp_CT42_ServicesIndustriesSICCode.md)
* [[Securitization].[sp_CT43_BulkPurchase]](Securitization_sp_CT43_BulkPurchase.md)
* [[Securitization].[sp_CT44_ThirdPartyCollect]](Securitization_sp_CT44_ThirdPartyCollect.md)
* [[Securitization].[sp_CT45_PrevMonthDelin31]](Securitization_sp_CT45_PrevMonthDelin31.md)
* [[Securitization].[sp_CT46_EquipLocCanada]](Securitization_sp_CT46_EquipLocCanada.md)
* [[Securitization].[sp_CT47_EmbeddedPrepayment]](Securitization_sp_CT47_EmbeddedPrepayment.md)
* [[Securitization].[sp_CT48_LargestCustomer]](Securitization_sp_CT48_LargestCustomer.md)
* [[Securitization].[sp_CT49_LargestSingleStateexCAFLTX]](Securitization_sp_CT49_LargestSingleStateexCAFLTX.md)
* [[Securitization].[sp_CT5_TotalLeveragedThirdPartyCollectL]](Securitization_sp_CT5_TotalLeveragedThirdPartyCollectL.md)
* [[Securitization].[sp_CT50_StateCAFLTX]](Securitization_sp_CT50_StateCAFLTX.md)
* [[Securitization].[sp_CT51_SingleLargestLeveragedThirdPartyCollect]](Securitization_sp_CT51_SingleLargestLeveragedThirdPartyCollect.md)
* [[Securitization].[sp_CT52_SingleLargestLeveragedThirdPartyBilled]](Securitization_sp_CT52_SingleLargestLeveragedThirdPartyBilled.md)
* [[Securitization].[sp_CT53_ThirdPartyBilledIndividual]](Securitization_sp_CT53_ThirdPartyBilledIndividual.md)
* [[Securitization].[sp_CT54_LargestThirdParty]](Securitization_sp_CT54_LargestThirdParty.md)
* [[Securitization].[sp_CT55_Top5ThirdParty]](Securitization_sp_CT55_Top5ThirdParty.md)
* [[Securitization].[sp_CT56_Top15Customers]](Securitization_sp_CT56_Top15Customers.md)
* [[Securitization].[sp_CT57_Top7Customers]](Securitization_sp_CT57_Top7Customers.md)
* [[Securitization].[sp_CT58_DocRecourseIndividualVendor3rdParty]](Securitization_sp_CT58_DocRecourseIndividualVendor3rdParty.md)
* [[Securitization].[sp_CT59_LargestVendor]](Securitization_sp_CT59_LargestVendor.md)
* [[Securitization].[sp_CT6_TotalLeveragedThirdPartyBilled]](Securitization_sp_CT6_TotalLeveragedThirdPartyBilled.md)
* [[Securitization].[sp_CT60_LargestVendorExcludingXerox]](Securitization_sp_CT60_LargestVendorExcludingXerox.md)
* [[Securitization].[sp_CT61_MaxToptwovendorsbundled]](Securitization_sp_CT61_MaxToptwovendorsbundled.md)
* [[Securitization].[sp_CT62_MaxThirdthrough10thbundled]](Securitization_sp_CT62_MaxThirdthrough10thbundled.md)
* [[Securitization].[sp_CT63_MaxAllothervendorsbundled]](Securitization_sp_CT63_MaxAllothervendorsbundled.md)
* [[Securitization].[sp_CT64_PartnersinLeasingIndividual]](Securitization_sp_CT64_PartnersinLeasingIndividual.md)
* [[Securitization].[sp_CT65_Top10Customers]](Securitization_sp_CT65_Top10Customers.md)
* [[Securitization].[sp_CT66_LargestVendor]](Securitization_sp_CT66_LargestVendor.md)
* [[Securitization].[sp_CT69_EquipLocGuam]](Securitization_sp_CT69_EquipLocGuam.md)
* [[Securitization].[sp_CT7_ThirdPartyBilledTotal]](Securitization_sp_CT7_ThirdPartyBilledTotal.md)
* [[Securitization].[sp_CT70_LargestSICCodeFourdigit]](Securitization_sp_CT70_LargestSICCodeFourdigit.md)
* [[Securitization].[sp_CT71_Top5Customers]](Securitization_sp_CT71_Top5Customers.md)
* [[Securitization].[sp_CT72_Top5SICCodesFourdigit]](Securitization_sp_CT72_Top5SICCodesFourdigit.md)
* [[Securitization].[sp_CT75_TotalForeignLocation]](Securitization_sp_CT75_TotalForeignLocation.md)
* [[Securitization].[sp_CT76_DeferredDeferredGAFV]](Securitization_sp_CT76_DeferredDeferredGAFV.md)
* [[Securitization].[sp_CT77_TotalnonmonthlyQSkipsASAVDeferredGAFV]](Securitization_sp_CT77_TotalnonmonthlyQSkipsASAVDeferredGAFV.md)
* [[Securitization].[sp_CT78_JuniorLienNote]](Securitization_sp_CT78_JuniorLienNote.md)
* [[Securitization].[sp_CT8_OfficeEquip]](Securitization_sp_CT8_OfficeEquip.md)
* [[Securitization].[sp_CT80_NonPerfectedSecurityInterest]](Securitization_sp_CT80_NonPerfectedSecurityInterest.md)
* [[Securitization].[sp_CT81_SecuredNotes_InventoryFinance]](Securitization_sp_CT81_SecuredNotes_InventoryFinance.md)
* [[Securitization].[sp_CT86_VaultName]](Securitization_sp_CT86_VaultName.md)
* [[Securitization].[sp_CT9_RemTerm72but84]](Securitization_sp_CT9_RemTerm72but84.md)
* [Securitization]
* [Securitization].[PoolConcentration]


---

## <a name="#usedby"></a>Used By

* [[Securitization].[spCalculateNPVByPoolID]](Securitization_spCalculateNPVByPoolID.md)
* [[Securitization].[spRefreshPoolContract]](Securitization_spRefreshPoolContract.md)


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

