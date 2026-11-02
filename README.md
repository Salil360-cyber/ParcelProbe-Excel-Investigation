# ParcelProbe: Amazon.in Seller Order Issue Investigation (Excel)

Excel-only investigation of why Amazon.in seller orders get cancelled or returned, and which orders need action now.

## Data

[E-Commerce Sales Dataset](https://www.kaggle.com/datasets/thedevastator/unlock-profits-with-e-commerce-sales-data) (Kaggle, The Devastator), file `Amazon Sale Report.csv`: a clothing seller's Amazon.in order report with 128,975 order lines (120,378 orders), 31 Mar – 29 Jun 2022. Not included here; download it from Kaggle. No simulated data is used.

## Workbook

| Sheet | What it does |
|---|---|
| Dashboard | KPI cards, 5 PivotCharts, Category and Month slicers |
| Findings | Results and recommendations |
| Orders | Power Query output + 6 formula columns (XLOOKUP, IFS, TEXT) |
| P1–P8 | One PivotTable per investigation question |
| Case_Tracker | 420 cases with priority, SLA status, owner, status dropdown |
| StatusMap, CaseMap | Lookup tables (also in `ParcelProbe_Lookups.xlsx`) |

## Key findings

- 14.2% of order lines were cancelled (18,329 of 128,969).
- Merchant-fulfilled lines cancelled at 17.5% vs 12.8% for Amazon-fulfilled, and the gap holds in every category. So category mix does not explain it; fulfilment type is the strongest lead (seller or SKU effects would need more data).
- 77,801 Amazon-fulfilled lines show only "Shipped": the report has no delivery or return status for them, so their delivery and return rates cannot be measured from it.
- Merchant return-to-seller rate averaged 6.5%, but 14.3% in Assam, 11.2% in Bihar and 10.8% in Odisha. Kerala has the highest cancel rate (17.8%) but one of the lowest return rates (4.5%): its problem is before dispatch.
- Cancel rate falls steadily with size: XS 16.7% to 6XL 9.8% (a fit-related hypothesis; the data has no cancel reasons to confirm it).
- 420 cases need action, 354 past SLA. All 218 status mismatches (order "Shipped", courier "Cancelled" or "Unshipped") are Amazon-fulfilled and 151 are over 30 days old.
- Promotion vs cancel looks like 0.4% vs 36.7%, but only 295 of 18,329 cancelled lines carry a promotion ID, so this is treated as a recording artefact, not a cause.

## Case rules

| Case type | Rule | Priority | SLA days |
|---|---|---|---|
| Lost / Damaged | Status is Lost in Transit or Damaged | P1 | 2 |
| Status mismatch | Order Shipped but courier Cancelled/Unshipped, or order Pending but courier Shipped | P2 | 3 |
| Stuck before dispatch | Pending for more than 3 days | P2 | 3 |
| Return in progress | Returning to Seller or Rejected by Buyer | P3 | 7 |

Not flagged: 5,837 lines with order status "Cancelled" but courier "Shipped". These look like cancellations after dispatch that follow the return flow; they would be added to the mismatch rule only if the process says they are errors.

## Data cleaning (Power Query)

Removed 6 duplicate rows; standardised 69 state spellings to 37; dropped an unlabelled column (`Unnamed: 22`, only "False" or blank, no documented meaning) and two columns with a single value apart from blanks (currency INR, ship-country IN); fixed a header with a trailing space (`Sales Channel `); parsed month-day-year dates with the US locale; stored pincodes as text; flagged 7,792 blank amounts (7,563 of them cancelled) and the single day of March data (171 lines).

## Assumptions

SLA days and priorities are project assumptions, not Amazon policy. Case age is measured from the order date to the last date in the data (29 Jun 2022).

## Files

| File | Contents |
|---|---|
| `ParcelProbe_Lookups.xlsx` | StatusMap (13 statuses → 6 groups), CaseMap (priority, SLA, action, owner), StateFix (10 state corrections) |

## Skills

Power Query, XLOOKUP, IFS, COUNTIFS, AVERAGEIFS, named ranges, Excel Tables, PivotTables with value filters, PivotCharts, slicers with report connections, data validation, conditional formatting.

---
Author: **Salil Gokhale** · [LinkedIn](https://www.linkedin.com/in/salil-gokhale-analytics)
