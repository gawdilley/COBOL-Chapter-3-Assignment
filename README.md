# RPT2000 – Year-To-Date Sales Report  

**Author:** Gabe Dilley  
**GitHub:** https://github.com/gawdilley  
**Repository:** https://github.com/gawdilley/COBOL-Chapter-1-Assignment  
**Language:** COBOL  
**Date:** February 19, 2026  

---

## Table of Contents

1. [Overview](#overview)  
2. [Program Description](#program-description)  
3. [Features](#features)  
4. [Input File Layout](#input-file-layout)  
5. [Output Report Layout](#output-report-layout)  
6. [Processing Logic](#processing-logic)  
7. [Business Rules](#business-rules)  
8. [Sample Output](#sample-output)  
9. [How to Run](#how-to-run)  

---

## Overview

`RPT2000` is a COBOL batch reporting program that reads customer master records and produces a formatted Year-To-Date (YTD) Sales Report. The report compares current year sales to previous year sales, calculates change amounts and percentages, and displays grand totals for qualifying customers.

---

## Program Description

The program:

- Reads customer records from an input sequential file (`CUSTMAST`)
- Filters customers based on a minimum YTD sales threshold
- Calculates sales change amount and percentage
- Produces a paginated, formatted report (`RPT2000`)
- Displays grand totals at the end of the report

---

## Features

- Formatted multi-line page headers  
- Automatic pagination  
- Sales comparison (Current YTD vs Last YTD)  
- Change amount calculation  
- Percentage change calculation with rounding  
- Division-by-zero protection  
- Grand total calculations  
- Financial formatting with commas and sign indicators  

---

## Input File Layout

**File Name:** `CUSTMAST`  
**Record Length:** 130 Characters  

| Field Name | PIC | Description |
|------------|------|------------|
| CM-BRANCH-NUMBER | 9(2) | Branch number |
| CM-SALESREP-NUMBER | 9(2) | Sales representative number |
| CM-CUSTOMER-NUMBER | 9(5) | Customer number |
| CM-CUSTOMER-NAME | X(20) | Customer name |
| CM-SALES-THIS-YTD | S9(5)V9(2) | Current YTD sales |
| CM-SALES-LAST-YTD | S9(5)V9(2) | Previous YTD sales |
| FILLER | X(87) | Unused space |

---

## Output Report Layout

**File Name:** `RPT2000`  
**Record Length:** 130 Characters  

The report includes:

- Report date and time
- Page number
- Column headings
- Customer detail lines
- Sales totals
- Change amount
- Change percentage
- Grand totals section

---

## Processing Logic

### Initialization
- Opens input and output files  
- Retrieves current system date and time  
- Prepares report heading  

### Record Processing
- Reads records until end-of-file  
- Includes only customers where:

```
CM-SALES-THIS-YTD >= 10000
```

### Calculations Per Customer

**Change Amount**
```
CHANGE-AMOUNT = CM-SALES-THIS-YTD - CM-SALES-LAST-YTD
```

**Change Percentage**
```
(CHANGE-AMOUNT * 100) / CM-SALES-LAST-YTD
```

If last year’s sales are zero, percent change is set to:
```
999.9
```

### Grand Totals

Totals are accumulated during processing and calculated using the same change logic as individual records.

---

## Business Rules

- Only customers with YTD sales ≥ 10,000 are printed  
- Division-by-zero is prevented  
- Percentages are rounded  
- Page breaks occur after 55 lines  
- Financial values use formatted numeric editing  

---

## Sample Output

```
DATE:  02/19/2026           YEAR-TO-DATE SALES REPORT            PAGE:    1
TIME:  14:35                                                RPT2001

BRANCH SALES CUST                    SALES            SALES        CHANGE     CHANGE
 NUM    REP NUM    CUSTOMER NAME           THIS YTD      LAST YTD   AMOUNT    PERCENT
------ ----- -----  --------------------   ----------     ---------- ---------- ------

 01    10   10001  ACME SUPPLY CO          25,450.75     20,125.50   5,325.25  26.5
 01    12   10015  METRO INDUSTRIAL        18,900.00     19,500.00    -600.00  -3.1
 02    08   20002  SOUTHERN MATERIALS      45,300.25     30,100.00  15,200.25  50.5

                                        ============= ============= ============= ======

                                        89,651.00     69,725.50     19,925.50   28.6
```

*Note: Values shown above are sample data for demonstration purposes.*

---

## How to Run

Compile:
```
cobc -x RPT2000.cbl
```

Run:
```
./RPT2000
```

Review the generated output file:
```
RPT2000
```
