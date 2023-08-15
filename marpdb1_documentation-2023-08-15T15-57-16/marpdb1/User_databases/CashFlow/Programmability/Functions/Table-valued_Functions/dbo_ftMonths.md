#### 

[Project](../../../../../../index.md) > [marpdb1](../../../../../index.md) > [User databases](../../../../index.md) > [CashFlow](../../../index.md) > Programmability > Functions > [Table-valued Functions](Table-valued_Functions.md) > dbo.ftMonths

# ![Table-valued Functions](../../../../../../Images/Function_Table32.png) [dbo].[ftMonths]

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
| @MinDate | date | 3 |
| @MaxDate | date | 3 |
| @BillingCycle | char(12) | 12 |
| @firstPaymentDate | date | 3 |


---

## <a name="#sqlscript"></a>SQL Script

```sql


CREATE FUNCTION [dbo].[ftMonths] (@MinDate date, @MaxDate date, @BillingCycle char(12), @firstPaymentDate date = NULL)
	RETURNS @ftMonths TABLE ([year] int, [month] tinyint, seq int, skipMonth bit)
AS BEGIN
	DECLARE @wkDates TABLE ([curDate] date, [year] int, [month] tinyint, skipMonth bit);
	DECLARE @curDate date = @MinDate;
	DECLARE @fpDate date = @firstPaymentDate;

	-- The day doesn't matter for what we're doing here, so reset it to the first of the month.
	-- Errors happen later if the day is too close to the end of the month or if the start date's day is earlier in the month than the end date's day.
	SET @curDate = DATEADD(day, (DAY(@curDate) - 1) * -1, @curDate);
	SET @fpDate = DATEADD(day, (DAY(@fpDate) - 1) * -1, @fpDate);
	
	WHILE @curDate <= @MaxDate
	BEGIN
		INSERT INTO @wkDates ([curDate], [year], [month], skipMonth)
			VALUES (@curDate, YEAR(@curDate), MONTH(@curDate), 0);
			
		SET @curDate = DATEADD(month, 1, @curDate);
	END;

	-- Skip all payments before the specified first payment date
	IF @firstPaymentDate IS NOT NULL
		UPDATE @wkDates
			SET skipMonth = 1
			WHERE [curDate] < @fpDate;

	-- Skip all payments for which the billing cycle is "0"
	UPDATE @wkDates
		SET skipMonth = 1
		WHERE SUBSTRING(@BillingCycle, [month], 1) = '0';
		
	-- Return all rows
	INSERT INTO @ftMonths ([year], [month], seq, skipMonth)
		SELECT [year], [month], ROW_NUMBER() OVER (ORDER BY [year], [month]) AS seq, skipMonth
			FROM @wkDates;
			
	RETURN;
END;


GO

```


---

###### Author:  Josh Dixson

###### Copyright 2023 - All Rights Reserved

###### Created: Tuesday, August 15, 2023 3:57:16 PM

