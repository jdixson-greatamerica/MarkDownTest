#### 

[Project](../../../../../index.md) > [marpdb1](../../../../index.md) > [User databases](../../../index.md) > [CashFlow](../../index.md) > Programmability > [Stored Procedures](Stored_Procedures.md) > Securitization.sp_PassFail_AllCriteria

# ![Stored Procedures](../../../../../Images/StoredProcedure32.png) [Securitization].[sp_PassFail_AllCriteria]

---

## <a name="#properties"></a>Properties

| Property | Value |
|---|---|
| ANSI Nulls On | NO |
| Quoted Identifier On | NO |


---

## <a name="#sqlscript"></a>SQL Script

```sql
SET QUOTED_IDENTIFIER OFF
GO
SET ANSI_NULLS OFF
GO




-- History:
--		Name: JDixson
--		Date: 04/24/2017
--		Desc: PRJ0016920 adding the following criteria:
--								Dealer ID 028571-0000
--								Originated from EquipLocCanada <= 100k
--		Name:	Chad Feldmann
--		Date:	4/17/2019
--		Desc:	Ticket #PRJ0040256 - Updating Legal Status and Contract Status eligibility criteria to use lists
--
--		Name:	John Thadison
--		Date:	02/16/2022
--		Desc:	Add "Active" flag check to Security.EligibilityCriteria queries
--
--		Name:	JTDixson
--		Date:	4/24/2023
--		Desc:	Made correction to logic when looking up to ListItems.  If the ItemText was being converted to an int or tiny int, removed that and added convert to varchar to the value from securitization.contract
--				INC0225127



CREATE PROCEDURE [Securitization].[sp_PassFail_AllCriteria]
AS
DECLARE @Pass BIT
DECLARE @EligibilityCriteriaID INT
DECLARE @StartingDate DATETIME

--Product Line ID 1
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Product Line ID 0001' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN RTRIM([ProductLineID] ) = '0001' THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE() FROM [Securitization].[Contracts]; 
END

--Product Line ID 2
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Product Line ID 0002' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN RTrim([ProductLineID] )  = '0002' THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate() FROM [Securitization].[Contracts]; 
END

--Product Line ID 4
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Product Line ID 0004' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN RTrim([ProductLineID] ) = '0004' THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate() FROM [Securitization].[Contracts]; 
END

--Product Line ID 5
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Product Line ID 0005' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN RTrim([ProductLineID] ) = '0005' THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate() FROM [Securitization].[Contracts]; 
END

--Product Line ID 6
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Product Line ID 0006' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN RTrim([ProductLineID] ) = '0006' THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate() FROM [Securitization].[Contracts]; 
END

--Product Line ID 7
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Product Line ID 0007' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN RTrim([ProductLineID] ) = '0007' THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate() FROM [Securitization].[Contracts]; 
END

--Delinquent Status Code 1
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Delinquent Status Code 1' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN [DelinStatusCode] > 1 THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate() FROM [Securitization].[Contracts]; 
END

--Delinquent Status Code 31
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Delinquent Status Code 31' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN [DelinStatusCode] > 31 THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate() FROM [Securitization].[Contracts]; 
END

--Delinquent 91
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Delinquent 91' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([Delin91] IS NOT NULL AND [Delin91] <> '') THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate() FROM [Securitization].[Contracts]; 
END

--Delinquent Status Code Prev Month 1
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Delinquent Status Code Prev Month 1' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN [DelinStatus_PrevMonth] > 1 THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate() FROM [Securitization].[Contracts]; 
END

--Delinquent Status Code Prev Month 31
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Delinquent Status Code Prev Month 31' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN [DelinStatus_PrevMonth] > 31 THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate() FROM [Securitization].[Contracts]; 
END

--OutstandingOriginals
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'OutstandingOriginals' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN [OutstandingOriginals] = 'Y' THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate() FROM [Securitization].[Contracts]; 
END

--Billing Frequency Payments Arrears
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Billing Frequency Payments Arrears' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN (RTrim([BillingFrequency] ) IN ('Annual','Semi-Annual')) AND ([PymtsArrears] = 'Arrears') THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate() FROM [Securitization].[Contracts]; 
END

--Eligible Bill To, Equipment, Lessee State
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Eligible Bill To, Equipment, Lessee State' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN (RTrim([BillToState] ) NOT IN (SELECT [ItemText] FROM [Securitization].[ListItems] WHERE [List_ID] = 1)) OR (RTrim([EquipState] ) NOT IN (SELECT [ItemText] FROM [Securitization].[ListItems] WHERE [List_ID] = 1)) OR (RTrim([LesseeState] ) NOT IN (SELECT [ItemText] FROM [Securitization].[ListItems] WHERE [List_ID] = 1)) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate() FROM [Securitization].[Contracts]; 
END

--Eligible Bill To, Equipment, Lessee State GAPRI
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Eligible Bill To, Equipment, Lessee State GAPRI' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN (RTrim([BillToState] ) NOT IN (SELECT [ItemText] FROM [Securitization].[ListItems] WHERE [List_ID] = 2)) OR (RTrim([EquipState] ) NOT IN (SELECT [ItemText] FROM [Securitization].[ListItems] WHERE [List_ID] = 2)) OR (RTrim([LesseeState] ) NOT IN (SELECT [ItemText] FROM [Securitization].[ListItems] WHERE [List_ID] = 2)) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate() FROM [Securitization].[Contracts]; 
END

--Eligible Bill To, Equipment, Lessee State GAPRII
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Eligible Bill To, Equipment, Lessee State GAPRII' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN (RTrim([BillToState] ) NOT IN (SELECT [ItemText] FROM [Securitization].[ListItems] WHERE [List_ID] = 3)) OR (RTrim([EquipState] ) NOT IN (SELECT [ItemText] FROM [Securitization].[ListItems] WHERE [List_ID] = 3)) OR (RTrim([LesseeState] ) NOT IN (SELECT [ItemText] FROM [Securitization].[ListItems] WHERE [List_ID] = 3)) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate() FROM [Securitization].[Contracts]; 
END

--Contract Status ID
DECLARE @ContractStatusListID INT;
SELECT @ContractStatusListID = ID FROM Securitization.Lists WHERE ShortName = 'ContractStatusID'

SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Contract Status ID' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN CONVERT(VARCHAR,[ContractStatusID]) IN (SELECT ItemText FROM Securitization.ListItems WHERE List_ID = @ContractStatusListID) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate() FROM [Securitization].[Contracts]; 
END

--Contract Status ID Blank 37
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Contract Status ID Blank 37' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN [ContractStatusID] <> 37 THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate() FROM [Securitization].[Contracts]; 
END

--Legal Status Code
DECLARE @LegalStatusIDListID INT;
SELECT @LegalStatusIDListID = ID FROM Securitization.Lists WHERE ShortName = 'LegalStatusID'

SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Legal Status Code' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN CONVERT(VARCHAR,[LegalStatusCode]) IN (SELECT ItemText FROM Securitization.ListItems WHERE List_ID = @LegalStatusIDListID) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate() FROM [Securitization].[Contracts]; 
END

--Legal Status Code Blank 37
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Legal Status Code Blank 37' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN [LegalStatusCode] <> 37 THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate() FROM [Securitization].[Contracts]; 
END

--Workflow Status
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'WorkflowStatus' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN coalesce(WorkflowStatus ,'') = 'GEC' OR ProgramTypeID >= 10001 THEN 1 ELSE 0 END, 'system', getdate(), 'system', getdate() FROM [Securitization].[Contracts]; 
END

--Dealer ID 016089-0000
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Dealer ID 016089-0000' AND Active = 1

IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([AggregationParentID] = '016089-0000' OR [DealerID] = '016089-0000') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 61 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 61) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date) FROM [Securitization].[Contracts]; 


INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([AggregationParentID] = '016089-0000' OR [DealerID] = '016089-0000') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 61 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 61) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([AggregationParentID] = '016089-0000' OR [DealerID] = '016089-0000') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 61 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 61) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([AggregationParentID] = '016089-0000' OR [DealerID] = '016089-0000') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 61 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 61) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date) FROM [Securitization].[Contracts]; 
END

--Dealer ID 016003-0000
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Dealer ID 016003-0000' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([AggregationParentID] = '016003-0000' OR [DealerID] = '016003-0000') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 91 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 91) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([AggregationParentID] = '016003-0000' OR [DealerID] = '016003-0000') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 91 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 91) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([AggregationParentID] = '016003-0000' OR [DealerID] = '016003-0000') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 91 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 91) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([AggregationParentID] = '016003-0000' OR [DealerID] = '016003-0000') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 91 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 91) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date) FROM [Securitization].[Contracts]; 
END

--Program Variation 1007
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Program Variation 1007' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1007') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 61 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 61) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1007') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 61 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 61) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1007') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 61 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 61) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1007') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 61 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 61) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date) FROM [Securitization].[Contracts]; 
END

--Program Variation 1008
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Program Variation 1008' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1008') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 91 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 91) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1008') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 91 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 91) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1008') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 91 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 91) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1008') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 91 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 91) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date) FROM [Securitization].[Contracts]; 
END

--Program Variation 1001
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Program Variation 1001' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1001') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 121 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 121) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1001') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 121 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 121) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1001') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 121 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 121) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1001') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 121 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 121) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date) FROM [Securitization].[Contracts]; 
END

--Program Variation 1016
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Program Variation 1016' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1016') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 121 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 121) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1016') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 121 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 121) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1016') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 121 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 121) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1016') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 121 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 121) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date) FROM [Securitization].[Contracts]; 
END

--Program Variation 1017
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Program Variation 1017' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1017') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 151 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 151) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1017') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 151 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 151) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1017') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 151 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 151) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1017') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 151 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 151) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date) FROM [Securitization].[Contracts]; 
END

--Program Variation 1018
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Program Variation 1018' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1018') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 181 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 181) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1018') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 181 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 181) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1018') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 181 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 181) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1018') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 181 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 181) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date) FROM [Securitization].[Contracts]; 
END

--Program Variation 1019
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Program Variation 1019' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1019') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 211 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 211) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1019') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 211 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 211) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1019') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 211 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 211) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1019') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 211 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 211) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date) FROM [Securitization].[Contracts]; 
END

--Program Variation 1002
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Program Variation 1002' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1002') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 211 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 211) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1002') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 211 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 211) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1002') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 211 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 211) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1002') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 211 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 211) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date) FROM [Securitization].[Contracts]; 
END

--Program Variation 1020
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Program Variation 1020' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1020') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 241 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 241) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1020') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 241 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 241) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1020') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 241 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 241) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1020') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 241 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 241) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date) FROM [Securitization].[Contracts]; 
END

--Program Variation 1021
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Program Variation 1021' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1021') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 301 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 301) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1021') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 301 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 301) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1021') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 301 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 301) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1021') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 301 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 301) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date) FROM [Securitization].[Contracts]; 
END

--Program Variation 1003
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Program Variation 1003' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1003') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 391 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 391) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1003') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 391 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 391) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1003') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 391 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 391) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1003') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 391 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 391) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date) FROM [Securitization].[Contracts]; 
END

--Originated from Filtered Customers
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Originated from Filtered Customers' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN RTrim([customerID] ) IN (SELECT [ItemText] FROM [Securitization].[ListItems] WHERE [List_ID] = 5) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate() FROM [Securitization].[Contracts]; 
END

--Originated from Filtered Dealers
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Originated from Filtered Dealers' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN RTrim([DealerID] ) IN (SELECT [ItemText] FROM [Securitization].[ListItems] WHERE [List_ID] = 4) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate() FROM [Securitization].[Contracts]; 
END

--Balloon Percent Greater Than 15
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Balloon Percent Greater Than 15' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN [BalloonPercent] > 15 THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN [BalloonPercent] > 15 THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN [BalloonPercent] > 15 THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN [BalloonPercent] > 15 THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date) FROM [Securitization].[Contracts]; 
END

--Balloon Percent Greater Than 20
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Balloon Percent Greater Than 20' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN [BalloonPercent] > 20 THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN [BalloonPercent] > 20 THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN [BalloonPercent] > 20 THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN [BalloonPercent] > 20 THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date) FROM [Securitization].[Contracts]; 
END

--Balloon Percent Greater Than 10
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Balloon Percent Greater Than 10' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN [BalloonPercent] > 10 THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN [BalloonPercent] > 10 THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN [BalloonPercent] > 10 THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN [BalloonPercent] > 10 THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date) FROM [Securitization].[Contracts]; 
END

--Eligible Bill To, Equipment, Lessee State GAFV
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Eligible Bill To, Equipment, Lessee State GAFV' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN (RTrim([BillToState] ) NOT IN (SELECT [ItemText] FROM [Securitization].[ListItems] WHERE [List_ID] = 42)) OR (RTrim([EquipState] ) NOT IN (SELECT [ItemText] FROM [Securitization].[ListItems] WHERE [List_ID] = 42)) OR (RTrim([LesseeState] ) NOT IN (SELECT [ItemText] FROM [Securitization].[ListItems] WHERE [List_ID] = 42)) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate() FROM [Securitization].[Contracts]; 
END

--Program Variation 1028
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Program Variation 1028' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1028') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 396 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 396) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1028') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 396 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 396) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1028') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 396 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 396) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1028') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 396 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 396) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date) FROM [Securitization].[Contracts]; 
END

--Billing Freq Pymt Arrears From First Pymt Date
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Billing Freq Pymt Arrears From First Pymt Date' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN (RTrim([BillingFrequency] ) IN ('Annual','Semi-Annual')) AND ([PymtsArrears] = 'Arrears') AND (DATEDIFF(day, [BookingDate], [FirstPymtDate])> 90 OR DATEDIFF(day, [CommenceDate], [FirstPymtDate])> 90) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate() FROM [Securitization].[Contracts]; 
END

--Program Variation 1024
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Program Variation 1024' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1024') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 271 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 271) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1024') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 271 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date)) < 271) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,getdate()))-1),DATEADD(mm,1,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1024') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 271 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date)) < 271) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,getdate()))-1),DATEADD(mm,2,getdate())) AS date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1024') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 271 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date)) < 271) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,getdate()))-1),DATEADD(mm,3,getdate())) AS date) FROM [Securitization].[Contracts]; 
END

--Dealer ID 000256-0000
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Dealer ID 000256-0000' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([AggregationParentID] = '000256-0000' OR [DealerID] = '000256-0000') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 9 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 9) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([AggregationParentID] = '000256-0000' OR [DealerID] = '000256-0000') AND (DATEDIFF(DAY, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,GETDATE()))-1),DATEADD(mm,1,GETDATE())) AS DATE)) < 9 OR DATEDIFF(DAY, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,GETDATE()))-1),DATEADD(mm,1,GETDATE())) AS DATE)) < 9) THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,GETDATE()))-1),DATEADD(mm,1,GETDATE())) AS DATE) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([AggregationParentID] = '000256-0000' OR [DealerID] = '000256-0000') AND (DATEDIFF(DAY, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,GETDATE()))-1),DATEADD(mm,2,GETDATE())) AS DATE)) < 9 OR DATEDIFF(DAY, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,GETDATE()))-1),DATEADD(mm,2,GETDATE())) AS DATE)) < 9) THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,GETDATE()))-1),DATEADD(mm,2,GETDATE())) AS DATE) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([AggregationParentID] = '000256-0000' OR [DealerID] = '000256-0000') AND (DATEDIFF(DAY, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,GETDATE()))-1),DATEADD(mm,3,GETDATE())) AS DATE)) < 9 OR DATEDIFF(DAY, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,GETDATE()))-1),DATEADD(mm,3,GETDATE())) AS DATE)) < 9) THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,GETDATE()))-1),DATEADD(mm,3,GETDATE())) AS DATE) FROM [Securitization].[Contracts]; 
END

--Dealer ID 013811-0000
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Dealer ID 013811-0000' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([AggregationParentID] = '013811-0000' OR [DealerID] = '013811-0000') AND (DATEDIFF(DAY, [BookingDate], CAST(DATEADD(dd,-(DAY(GETDATE())-1),GETDATE()) AS DATE)) < 91 OR DATEDIFF(DAY, [CommenceDate], CAST(DATEADD(dd,-(DAY(GETDATE())-1),GETDATE()) AS DATE)) < 91) THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE(), CAST(DATEADD(dd,-(DAY(GETDATE())-1),GETDATE()) AS DATE) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([AggregationParentID] = '013811-0000' OR [DealerID] = '013811-0000' ) AND (DATEDIFF(DAY, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,GETDATE()))-1),DATEADD(mm,1,GETDATE())) AS DATE)) < 91 OR DATEDIFF(DAY, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,GETDATE()))-1),DATEADD(mm,1,GETDATE())) AS DATE)) < 91) THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,GETDATE()))-1),DATEADD(mm,1,GETDATE())) AS DATE) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([AggregationParentID] = '013811-0000' OR [DealerID] = '013811-0000') AND (DATEDIFF(DAY, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,GETDATE()))-1),DATEADD(mm,2,GETDATE())) AS DATE)) < 91 OR DATEDIFF(DAY, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,GETDATE()))-1),DATEADD(mm,2,GETDATE())) AS DATE)) < 91) THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,GETDATE()))-1),DATEADD(mm,2,GETDATE())) AS DATE) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([AggregationParentID] = '013811-0000' OR [DealerID] = '013811-0000') AND (DATEDIFF(DAY, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,GETDATE()))-1),DATEADD(mm,3,GETDATE())) AS DATE)) < 91 OR DATEDIFF(DAY, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,GETDATE()))-1),DATEADD(mm,3,GETDATE())) AS DATE)) < 91) THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,GETDATE()))-1),DATEADD(mm,3,GETDATE())) AS DATE) FROM [Securitization].[Contracts]; 
END

--Dealer ID 034185-0000
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Dealer ID 034185-0000' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([AggregationParentID] = '034185-0000' OR [DealerID] = '034185-0000') AND (DATEDIFF(DAY, [BookingDate], CAST(DATEADD(dd,-(DAY(GETDATE())-1),GETDATE()) AS DATE)) < 31 OR DATEDIFF(DAY, [CommenceDate], CAST(DATEADD(dd,-(DAY(GETDATE())-1),GETDATE()) AS DATE)) < 31) THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE(), CAST(DATEADD(dd,-(DAY(GETDATE())-1),GETDATE()) AS DATE) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([AggregationParentID] = '034185-0000' OR [DealerID] = '034185-0000' ) AND (DATEDIFF(DAY, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,GETDATE()))-1),DATEADD(mm,1,GETDATE())) AS DATE)) < 31 OR DATEDIFF(DAY, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,GETDATE()))-1),DATEADD(mm,1,GETDATE())) AS DATE)) < 31) THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,GETDATE()))-1),DATEADD(mm,1,GETDATE())) AS DATE) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([AggregationParentID] = '034185-0000' OR [DealerID] = '034185-0000') AND (DATEDIFF(DAY, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,GETDATE()))-1),DATEADD(mm,2,GETDATE())) AS DATE)) < 31 OR DATEDIFF(DAY, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,GETDATE()))-1),DATEADD(mm,2,GETDATE())) AS DATE)) < 31) THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,GETDATE()))-1),DATEADD(mm,2,GETDATE())) AS DATE) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([AggregationParentID] = '034185-0000' OR [DealerID] = '034185-0000') AND (DATEDIFF(DAY, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,GETDATE()))-1),DATEADD(mm,3,GETDATE())) AS DATE)) < 31 OR DATEDIFF(DAY, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,GETDATE()))-1),DATEADD(mm,3,GETDATE())) AS DATE)) < 31) THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,GETDATE()))-1),DATEADD(mm,3,GETDATE())) AS DATE) FROM [Securitization].[Contracts]; 
END


--Term Date 84
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Term Date 84' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn,StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN DATEDIFF(MONTH,CAST(DATEADD(dd,-(DAY(GETDATE())-1),GETDATE()) AS DATE),TermDate) >84 THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE(),CAST(DATEADD(dd,-(DAY(GETDATE())-1),GETDATE()) AS DATE) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn,StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN DATEDIFF(MONTH,CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,GETDATE()))-1),DATEADD(mm,1,GETDATE())) AS DATE),TermDate) >84 THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE(),CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,GETDATE()))-1),DATEADD(mm,1,GETDATE())) AS DATE) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn,StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN DATEDIFF(MONTH,CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,GETDATE()))-1),DATEADD(mm,2,GETDATE())) AS DATE),TermDate) >84 THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE(),CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,GETDATE()))-1),DATEADD(mm,2,GETDATE())) AS DATE) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn,StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN DATEDIFF(MONTH,CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,GETDATE()))-1),DATEADD(mm,3,GETDATE())) AS DATE),TermDate) >84 THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE(),CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,GETDATE()))-1),DATEADD(mm,3,GETDATE())) AS DATE) FROM [Securitization].[Contracts]; 
END


--Eligible Bill To, Equipment, Lessee State Term
DECLARE @EligibleBillToTermListID INT
SELECT @EligibleBillToTermListID = ID FROM Securitization.Lists l	WHERE l.ShortName ='EligibleStatesTerm'

SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Eligible Bill To, Equipment, Lessee State Term' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN (RTRIM([BillToState] ) NOT IN (SELECT [ItemText] FROM [Securitization].[ListItems] WHERE [List_ID] = @EligibleBillToTermListID	)) OR (RTRIM([EquipState] ) NOT IN (SELECT [ItemText] FROM [Securitization].[ListItems] WHERE [List_ID] = @EligibleBillToTermListID)) OR (RTRIM([LesseeState] ) NOT IN (SELECT [ItemText] FROM [Securitization].[ListItems] WHERE [List_ID] = @EligibleBillToTermListID)) THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE() FROM [Securitization].[Contracts]; 
END

--Program Variation 1043
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Program Variation 1043' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1043') AND (DATEDIFF(day, [BookingDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 331 OR DATEDIFF(day, [CommenceDate], CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date)) < 331) THEN 0 ELSE 1 END , 'system', getdate(), 'system', getdate(), CAST(DATEADD(dd,-(DAY(getdate())-1),getdate()) as date) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1043') AND (DATEDIFF(DAY, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,GETDATE()))-1),DATEADD(mm,1,GETDATE())) AS DATE)) < 331 OR DATEDIFF(DAY, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,GETDATE()))-1),DATEADD(mm,1,GETDATE())) AS DATE)) < 331) THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,GETDATE()))-1),DATEADD(mm,1,GETDATE())) AS DATE) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1043') AND (DATEDIFF(DAY, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,GETDATE()))-1),DATEADD(mm,2,GETDATE())) AS DATE)) < 331 OR DATEDIFF(DAY, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,GETDATE()))-1),DATEADD(mm,2,GETDATE())) AS DATE)) < 331) THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,GETDATE()))-1),DATEADD(mm,2,GETDATE())) AS DATE) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1043') AND (DATEDIFF(DAY, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,GETDATE()))-1),DATEADD(mm,3,GETDATE())) AS DATE)) < 331 OR DATEDIFF(DAY, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,GETDATE()))-1),DATEADD(mm,3,GETDATE())) AS DATE)) < 331) THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,GETDATE()))-1),DATEADD(mm,3,GETDATE())) AS DATE) FROM [Securitization].[Contracts]; 
END

--Program Variation 1023
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Program Variation 1023' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1023') AND (DATEDIFF(DAY, [BookingDate], CAST(DATEADD(dd,-(DAY(GETDATE())-1),GETDATE()) AS DATE)) < 151 OR DATEDIFF(DAY, [CommenceDate], CAST(DATEADD(dd,-(DAY(GETDATE())-1),GETDATE()) AS DATE)) < 151) THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE(), CAST(DATEADD(dd,-(DAY(GETDATE())-1),GETDATE()) AS DATE) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1023') AND (DATEDIFF(DAY, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,GETDATE()))-1),DATEADD(mm,1,GETDATE())) AS DATE)) < 151 OR DATEDIFF(DAY, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,GETDATE()))-1),DATEADD(mm,1,GETDATE())) AS DATE)) < 151) THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,GETDATE()))-1),DATEADD(mm,1,GETDATE())) AS DATE) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1023') AND (DATEDIFF(DAY, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,GETDATE()))-1),DATEADD(mm,2,GETDATE())) AS DATE)) < 151 OR DATEDIFF(DAY, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,GETDATE()))-1),DATEADD(mm,2,GETDATE())) AS DATE)) < 151) THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,GETDATE()))-1),DATEADD(mm,2,GETDATE())) AS DATE) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([ProgramVariation] = '1023') AND (DATEDIFF(DAY, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,GETDATE()))-1),DATEADD(mm,3,GETDATE())) AS DATE)) < 151 OR DATEDIFF(DAY, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,GETDATE()))-1),DATEADD(mm,3,GETDATE())) AS DATE)) < 151) THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,GETDATE()))-1),DATEADD(mm,3,GETDATE())) AS DATE) FROM [Securitization].[Contracts]; 
END

--Originated from EquipLocCanada >= 100k
SELECT  @EligibilityCriteriaID = ID FROM    Securitization.EligibilityCriteria WHERE   [ProgramValue] = 'EquipLocCanada-Individual Contract' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT  INTO [Securitization].[ContractEligibilityCriteria] ( EligibilityCriteria_ID , Contract_ContractID , Pass , CreatedBy , CreatedOn , ModifiedBy , ModifiedOn)
        SELECT @EligibilityCriteriaID , ContractID , CASE WHEN RTRIM(EquipmentCost) >= CONVERT(MONEY, 100000) AND (RTRIM([EquipState] ) IN ('CAN','AB','BC','MB','NB','NF','NT','NS','NU','ON','PE','QC','SK','YT'))  THEN 0 ELSE 1 END  ,'system' , GETDATE() ,'system' , GETDATE() FROM    [Securitization].[Contracts]
END


--Dealer ID 028571-0000
SELECT @EligibilityCriteriaID = ID FROM Securitization.EligibilityCriteria WHERE [ProgramValue] = 'Dealer ID 028571-0000' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([AggregationParentID] = '028571-0000' OR [DealerID] = '028571-0000') AND (DATEDIFF(DAY, [BookingDate], CAST(DATEADD(dd,-(DAY(GETDATE())-1),GETDATE()) AS DATE)) < 121 OR DATEDIFF(DAY, [CommenceDate], CAST(DATEADD(dd,-(DAY(GETDATE())-1),GETDATE()) AS DATE)) < 121) THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE(), CAST(DATEADD(dd,-(DAY(GETDATE())-1),GETDATE()) AS DATE) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([AggregationParentID] = '028571-0000' OR [DealerID] = '028571-0000' ) AND (DATEDIFF(DAY, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,GETDATE()))-1),DATEADD(mm,1,GETDATE())) AS DATE)) < 121 OR DATEDIFF(DAY, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,GETDATE()))-1),DATEADD(mm,1,GETDATE())) AS DATE)) < 121) THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,1,GETDATE()))-1),DATEADD(mm,1,GETDATE())) AS DATE) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([AggregationParentID] = '028571-0000' OR [DealerID] = '028571-0000') AND (DATEDIFF(DAY, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,GETDATE()))-1),DATEADD(mm,2,GETDATE())) AS DATE)) < 121 OR DATEDIFF(DAY, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,GETDATE()))-1),DATEADD(mm,2,GETDATE())) AS DATE)) < 121) THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,2,GETDATE()))-1),DATEADD(mm,2,GETDATE())) AS DATE) FROM [Securitization].[Contracts]; 

INSERT INTO [Securitization].[ContractEligibilityCriteria] (EligibilityCriteria_ID, Contract_ContractID, Pass, CreatedBy, CreatedOn, ModifiedBy, ModifiedOn, StartingDate)
SELECT @EligibilityCriteriaID, ContractID, CASE WHEN ([AggregationParentID] = '028571-0000' OR [DealerID] = '028571-0000') AND (DATEDIFF(DAY, [BookingDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,GETDATE()))-1),DATEADD(mm,3,GETDATE())) AS DATE)) < 121 OR DATEDIFF(DAY, [CommenceDate], CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,GETDATE()))-1),DATEADD(mm,3,GETDATE())) AS DATE)) < 121) THEN 0 ELSE 1 END , 'system', GETDATE(), 'system', GETDATE(), CAST(DATEADD(dd,-(DAY(DATEADD(mm,3,GETDATE()))-1),DATEADD(mm,3,GETDATE())) AS DATE) FROM [Securitization].[Contracts]; 
END

DECLARE @FollowupVisionListID INT
SELECT @FollowupVisionListID = ID FROM Securitization.Lists l	WHERE l.ShortName ='FollowupVision'
--Vision Followup - W9_Serial
SELECT  @EligibilityCriteriaID = ID FROM  Securitization.EligibilityCriteria WHERE [ProgramValue] = 'FollowUpVision' AND Active = 1
IF @EligibilityCriteriaID IS NOT NULL
BEGIN
INSERT  INTO [Securitization].[ContractEligibilityCriteria] ( EligibilityCriteria_ID , Contract_ContractID , Pass , CreatedBy , CreatedOn , ModifiedBy , ModifiedOn)
    --all items listed in FollowupVision column must exist in the FollowupVision list
	--example: if a and b are in the list then:
	--a is valid but a,c is not valid
	--b is valid but c,b is not valid
	--a,b is valid c,a,b is not valid
	--b,a is valid c,b,a is not valid
	SELECT @EligibilityCriteriaID , ContractID,  MIN(tes.Pass) ,'system' , GETDATE() ,'system' , GETDATE() FROM 
	(
	SELECT c.ContractID, item, c.FollowUpVision,
	CASE WHEN item IS NOT null AND ITEM NOT IN  (SELECT ItemText FROM [Securitization].[ListItems] WHERE [List_ID] = @FollowupVisionListID)  THEN 0 ELSE 1 END AS Pass
	FROM Securitization.Contracts c
	outer APPLY [Securitization].[udf_List2Table](c.FollowUpVision,',')  

	) tes 
	GROUP BY tes.ContractID,tes.FollowUpVision
END

GO

```


---

## <a name="#uses"></a>Uses

* [[Securitization].[udf_List2Table]](../Functions/Table-valued_Functions/Securitization_udf_List2Table.md)
* [Securitization]
* [Securitization].[ContractEligibilityCriteria]
* [Securitization].[Contracts]
* [Securitization].[EligibilityCriteria]
* [Securitization].[ListItems]
* [Securitization].[Lists]


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

