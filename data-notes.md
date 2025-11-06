# Data Notes

## Tables (Star Schema)
**Facts:** `Transaction_Data` (orders), `Return_Data` (returns)  
**Dims:** `Calendar`, `Products`, `Customers`, `Stores`, `Regions`

## Relationships
- `Calendar[date]` → `Transaction_Data[transaction_date]` (1:*)
- `Products[product_id]` → `Transaction_Data[product_id]` (1:*)
- `Customers[customer_id]` → `Transaction_Data[customer_id]` (1:*)
- `Stores[store_id]` → `Transaction_Data[store_id]` (1:*)
- `Products[product_id]` → `Return_Data[product_id]` (1:*)

## Modeling & Cleaning
- Standardized numeric types (prices/qty), fixed category/brand casing.
- Calendar with Year/Quarter/Month/Weekday; marked as Date table.
- Single-direction filters from dims → facts; keys on product/customer/store/date.

## KPI Definitions
- **Active Customers (90d):** distinct customers whose **Last Purchase Date ≤ 90 days** from the **Anchor Date** (max date in data or selected date).
- **Repeat Rate:** **Repeat Customers ÷ Customer Count**, where **Repeat Customers = customers with ≥ 2 orders** under current filters.
- **Segments (Customer Segmentation):**
  - **Returning:** ≥ 2 orders & recent
  - **New:** first order recent
  - **At Risk / Inactive:** > 90 days since last purchase
  - **Other:** everyone else
- **Return Rate:** **Quantity Returned ÷ Quantity Sold**.


## Report UX
- **Drill-down hierarchies** on geography (Country → Region/State) 
- **Edited interactions** so slicers filter visuals without cross-highlight noise.
- **Notes page** documents KPI/segment definitions used across the report.


---
**Appendix:** See [`measures.md`](./measures.md) for full DAX.
