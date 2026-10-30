# UrbanCart: Order Returns & Refund Leakage Analysis

**An end-to-end Excel + Power Query ETL project** that cleans and reconciles four messy datasets, tests every refund against six business rules, and measures the refund value that needs investigation.

> **Note:** UrbanCart is a fictional company and all data is synthetic, created for learning and portfolio purposes.

![Dashboard](03_screenshots/01_dashboard.png)

---

## Business Problem
UrbanCart refunds customers when they return items. Order, payment, return and refund data sit in four separate systems with inconsistent formats, so Finance cannot easily confirm that each refund was valid.

**Goal:** Clean and combine the four datasets, test every refund against business rules, and quantify the refund value at risk, without assuming any savings before the analysis.

## Tools Used
- Microsoft Excel
- Power Query (Extract and Transform)
- Excel formulas (rule tests and reconciliation checks)
- PivotTables, PivotCharts and Slicers (reporting)

## Data
| Table | Source system | Raw rows | Clean rows |
|---|---|---|---|
| Orders | Order system | 410 | 400 |
| Payments | Payment gateway | 378 | 374 |
| Returns | Warehouse | 88 | 86 |
| Refunds | Finance | 79 | 79 |

The data deliberately includes real-world problems: duplicate rows, text dates, inconsistent spellings, negative and text amounts, missing values, and records that do not match across tables.

## ETL Workflow
| Stage | What was done |
|---|---|
| **Extract** | Loaded 4 raw tables into Power Query as untouched connections |
| **Transform** | Profiled each table, defined business rules first, then standardised text, converted text dates (day-first), mapped payment modes, converted text and negative amounts, recalculated invalid order amounts, and removed 16 exact duplicates |
| **Reconcile** | Joined Refunds to Returns, Orders and Payments (left joins) and proved row counts and money totals tie out from raw to clean |
| **Test** | Applied six business-rule tests in Excel formulas and measured value at risk without double counting |
| **Load / Report** | Built PivotTables, PivotCharts, slicers, an investigation list and a one-page dashboard |

**Layers:** Raw → Staging → Clean → Reconciliation → Dashboard

## Business Rules
A refund is valid only if all of these hold:

| Rule | A refund fails if... |
|---|---|
| R1 | It has no matching return record |
| R2 | The return status is not Received |
| R3 | The order has no successful payment |
| R4 | The refund is more than the amount paid |
| R5 | It is a second or later refund for the same return |
| R6 | The return was requested more than 30 days after the order |

Each refund is counted once, using the first rule it fails (priority R1, R3, R2, R5, R4, R6). For R4 only the excess over the amount paid is counted.

## Key Findings
- **23 of 79 refunds (29.1%)** broke at least one rule.
- **Value at risk is ₹39,978**, which is 19.2% of the ₹208,389 refunded.
- Duplicate refunds (₹15,091) and refunds with no return record (₹14,694) make up **74.5%** of value at risk.
- No refund was paid on a Rejected or Pending return (R2 had zero failures).
- **Bengaluru** has the highest value at risk (₹13,594). **Home & Kitchen** has the highest value at risk (₹16,093), and **Beauty** has the highest exception rate (53.8%).
- Flagged refunds appear under all four approvers, which points to a process or control gap rather than one person.

**"Value at risk" means refund money that needs investigation. It is not proven loss and not a savings estimate.**

## Data Quality and Reconciliation
- Raw rows − duplicates = clean rows for all four tables (difference 0).
- Valid refunds + exception refunds = total refunds issued (₹208,389, difference 0).
- Orphan records and odd dates were **flagged, not deleted**, so no evidence was hidden.

![DQ Checks](03_screenshots/02_dq_checks.png)

![Leakage Summary](03_screenshots/03_leakage_summary.png)

## Assumptions and Limitations
- The data is synthetic, and the 30-day return window and other rules are assumptions to be confirmed with the business.
- Negative refund amounts were treated as sign errors and flagged.
- Invalid order amounts were recalculated as Quantity × Unit Price.
- City, category and approver samples are small (3 to 25 refunds), so differences are patterns to investigate, not proven trends.
- The month view is partial for the latest month.

## Repository Structure
```
01_raw_data/        Original untouched dataset
02_excel_project/   Finished workbook (queries, tables, pivots, dashboard)
03_screenshots/     Dashboard, checks and Power Query steps
04_documentation/   Business rules, issues log and Power Query code
```

## How to Use
1. Download `02_excel_project/UrbanCart_ETL_Project.xlsx`.
2. Open it in Microsoft Excel (Power Query needs the desktop version).
3. Start at the **Dashboard** sheet, then see **DQ_Checks** and **Leakage_Summary**.
4. See **Data → Queries & Connections** for the Power Query steps.

## What I Learned
Cleaning data with documented rules, joining tables without losing or duplicating records, reconciling totals, and describing results honestly without overclaiming.

## Contact
**[Balaji V]** | [www.linkedin.com/in/balajivenkatesan26] | [balajivenkatesan78@gmail.com]
