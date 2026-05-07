# RPT2000 – Year-To-Date Sales Report

**Course:** COBOL Programming – Chapter 3 Assignment
**Author:** [Gabe Dilley](https://github.com/gawdilley)
**Date:** February 19, 2026
**GitHub:** [COBOL-Chapter-3-Assignment](https://github.com/gawdilley/COBOL-Chapter-3-Assignment)

---

## Description

RPT2000 is a COBOL batch reporting program that reads a sequential customer master file (`CUSTMAST`) and produces a formatted Year-To-Date (YTD) Sales Report (`RPT2000`). The report filters customers based on a minimum sales threshold, compares current and prior year sales, and displays grand totals at the end of the report.

---

## What the Program Does

### Input
The program reads fixed-length customer master records (130 characters each), each containing:
- Branch number
- Sales rep number
- Customer number
- Customer name
- Sales amount for the current year-to-date
- Sales amount for the prior year-to-date

### Processing
The program performs the following steps:

1. **Opens** the input customer master file and the output report file.
2. **Retrieves** the current system date and time using `FUNCTION CURRENT-DATE` and formats them into the report heading.
3. **Reads** each customer record sequentially until end-of-file.
4. **Filters** records — only customers with current YTD sales of `$10,000.00` or more are included in the report.
5. **Calculates** for each qualifying customer:
   - **Change Amount** — the difference between this year's and last year's YTD sales.
   - **Change Percent** — the percentage change relative to last year's sales. If last year's sales are zero, the percentage is capped at `999.9%` to prevent a divide-by-zero error.
6. **Accumulates** grand totals for this YTD, last YTD, and change amount across all printed records.
7. **Manages pagination** — when a page fills up (55 lines), a new page is started automatically with fresh heading lines.
8. **Prints** the grand total section at the end of the report.

### Output
The printed report (`RPT2000`) includes:

- **Report headings** on each page with the current date, time, report title, report ID, and page number.
- **Column headers** describing Branch #, Sales Rep #, Customer #, Customer Name, Sales This YTD, Sales Last YTD, Change Amount, and Change Percent.
- **One detail line per qualifying customer** showing all of the above fields.
- **A grand total line** at the end of the report showing totals across all printed customers.

---

## Example Output

```
DATE:  02/19/2026           YEAR-TO-DATE SALES REPORT            PAGE:    1
TIME:  14:35                                                      RPT2001

BRANCH SALES  CUST   CUSTOMER NAME         SALES          SALES        CHANGE    CHANGE
 NUM    REP   NUM                         THIS YTD       LAST YTD      AMOUNT   PERCENT
------  -----  -----  --------------------  ----------  ----------  ----------  ------

 01      10   10001  ACME SUPPLY CO          25,450.75   20,125.50    5,325.25    26.5
 01      12   10015  METRO INDUSTRIAL        18,900.00   19,500.00      600.00-    3.1-
 02      08   20002  SOUTHERN MATERIALS      45,300.25   30,100.00   15,200.25    50.5

                                           ===========  ===========  ===========  ======
                              GRAND TOTAL   89,651.00    69,725.50   19,925.50    28.6
```

---

## New Concepts Used

- **Record filtering** — using a conditional `IF` statement to skip records that do not meet a minimum sales threshold (`CM-SALES-THIS-YTD >= 10,000`) before printing or accumulating
- **`FUNCTION CURRENT-DATE`** — using the built-in COBOL intrinsic function to retrieve and display the system date and time in the report heading
- **Divide-by-zero protection** — using an `IF` condition to check for a zero denominator before computing the change percentage, substituting a sentinel value (`999.9`) when needed
- **Grand total accumulation** — adding each qualifying customer's sales values to running total fields and printing them at the end of the report after all records have been processed
- **Automatic pagination** — tracking lines printed per page with `LINE-COUNT` and `LINES-ON-PAGE`, and triggering a heading routine when the threshold is reached
- **Signed numeric fields (`S`)** — using `S9` PIC clauses and negative-sign edit characters (`-`) in output fields to properly handle and display negative sales change values
- **`ROUNDED` option on `COMPUTE`** — applying rounding to the percentage calculation to improve accuracy of displayed results
- **Report-style output formatting** — constructing heading lines, detail lines, and a total line using `FILLER`, fixed-width `PIC` clauses, and edit characters like `ZZ,ZZ9.99-`

---

## Authors

| Name | Profile |
|------|---------|
| Gabe Dilley | [GitHub](https://github.com/gawdilley) |
