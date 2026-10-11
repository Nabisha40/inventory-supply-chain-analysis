# Inventory and Supply Chain Analysis

An end-to-end data analytics project using **Microsoft Excel, Power Query, Power BI, and DAX** to review inventory performance, warehouse utilization, transportation spending, sales volume, supplier lead times, and order status. The Power BI dashboard consolidates operational indicators into one interactive view for business users.

## 1. Business Problem

Inventory and supply chain teams need to balance product availability, storage capacity, transportation spending, and timely order fulfillment. When these measures are spread across tables or reviewed separately, it can be difficult to spot inefficiencies, compare regions and product categories, or decide where to investigate first.

This project addresses the need for a centralized, filterable view of supply chain performance. It is an **operational analysis**, not a predictive model, and the findings should be used to guide further investigation rather than treated as proof of a specific cause.

## 2. Objectives and How They Were Addressed

| Objective | How it was addressed | Business value |
|---|---|---|
| Review inventory and warehouse usage | Displayed warehouse utilization and inventory levels by category and region | Supports capacity planning and inventory allocation |
| Understand inventory efficiency | Presented Days Sales of Inventory (DSI) and inventory turnover ratio as KPI cards | Helps assess how inventory is moving through the business |
| Compare transportation spending | Built a clustered column chart of transportation cost by region and category | Highlights combinations that may warrant logistics cost review |
| Track sales volume over time | Created a yearly units-sold trend chart | Gives context for demand and inventory planning |
| Compare supplier/product lead times | Compared average lead time across product categories | Helps identify categories for supplier and replenishment review |
| Monitor order outcomes | Visualized order-status counts for fulfilled, pending, and canceled orders | Makes fulfillment workload and cancellation volume visible |
| Enable focused analysis | Added Region and Category slicers | Lets users compare a selected region/category without rebuilding reports |

## 3. Dataset

**File:** `Inventory Supply Chain Dataset.csv`

The dataset contains **1,200 records and 15 columns**. The fields cover date, region, category, supplier, warehouse, order status, units sold, inventory level, transportation cost, order accuracy, lead time, backorder flag, cost of goods sold (COGS), average inventory, and warehouse capacity.

### Data preparation

The dataset was reviewed and used as the source for the Power BI report. The analytical workflow is:

1. Inspect column names, data types, categorical values, and numeric fields.
2. Check missing values and consistency before building visuals.
3. Convert the date field to a usable date type and confirm numeric fields are numeric.
4. Load and transform the data in Power BI / Power Query as needed.
5. Create KPI measures and aggregate fields for the charts.
6. Validate the visual summaries against the source data and use slicers to test category- and region-level views.

A check of the supplied CSV found **no blank/null values in the 15 columns**. This does not by itself guarantee that every value is valid; unusual values and business rules should still be reviewed when extending the analysis.

## 4. Tools and Technologies

- **Microsoft Excel:** initial dataset review and preparation.
- **Power BI Desktop:** data model, report design, KPI cards, charts, and slicers.
- **Power Query:** data import and transformation.
- **DAX:** KPI and aggregation calculations.
- **CSV:** source data format.

## 5. Dashboard and Charts

Dashboard preview:
<img width="640" height="361" alt="dashboard Photo" src="https://github.com/user-attachments/assets/ca713583-2621-420f-b5c4-a3672f1769ab" />


The dashboard includes the following visuals:

### KPI cards

- **Warehouse Utilization: 34.08**
- **Days Sales of Inventory: 15.56**
- **Inventory Turnover Ratio: 23.47**

These values are reproduced from the supplied dashboard. The exact interpretation of each KPI depends on the DAX measure used in the Power BI file; review the measure definitions in the model before comparing the values with another system or reporting period.

### Warehouse Utilization gauge

A gauge shows warehouse utilization against a 0–100 scale, with the current dashboard value at **34.08**. It provides a quick capacity-use indicator. Confirm the measure's formula and unit in the PBIX file before using it as a formal utilization percentage.

### Transportation Cost by Region and Category

A clustered column chart compares transportation cost across **North, South, East, and West**, split by **Accessories, Clothing, Electronics, and Furniture**. It allows the user to compare both regional spending and category mix.

### Units Sold by Year

A line/area chart displays total units sold by year. In the supplied data, annual totals are:

| Year | Units sold |
|---:|---:|
| 2020 | 54,930 |
| 2021 | 761 |
| 2022 | 191,763 |
| 2023 | 194,775 |
| 2024 | 198,442 |


The series has a sharp change between 2021 and 2022. Because the available record counts differ substantially by year, compare coverage and date completeness before interpreting this as a real-world demand change. The supplied dataset contains only one record for 2021.

### Average Lead Time by Category

A doughnut chart compares the average number of lead-time days:

| Category | Average lead time (days) |
|---|---:|
| Accessories | 16.60 |
| Electronics | 15.68 |
| Furniture | 15.50 |
| Clothing | 15.29 |

### Backorder / Order Status Counts

The dashboard visual is titled “Count of Backorder by Order Status” and shows the number of records in each order status. The displayed counts are:

| Order status | Records | Share of all records |
|---|---:|---:|
| Fulfilled | 838 | 69.8% |
| Pending | 248 | 20.7% |
| Canceled | 114 | 9.5% |

**Important:** These bars match the `Order Status` record counts. They should not be interpreted as the count of rows where the Boolean `Backorder` flag is true. In the CSV, **116 records** have `Backorder = TRUE`.

### Inventory Level by Category and Region

A horizontal clustered bar chart compares the summed inventory level for each category across the four regions. This helps reveal where inventory is concentrated and supports stock allocation discussions.

### Interactive slicers

- **Region:** filter the dashboard to North, South, East, or West.
- **Category:** filter to Accessories, Clothing, Electronics, or Furniture.

## 6. Key Findings and Insights

The following findings were calculated from the supplied CSV unless specifically identified as a dashboard KPI.

1. **Order outcomes:** {int(status.get('Fulfilled',0)):,} of {total:,} records are fulfilled ({status.get('Fulfilled',0)/total:.1%}); {int(status.get('Pending',0)):,} are pending ({status.get('Pending',0)/total:.1%}); and {int(status.get('Canceled',0)):,} are canceled ({status.get('Canceled',0)/total:.1%}). Pending and canceled orders are useful starting points for an order-fulfillment review.
2. **Transportation spending differs by region:** total recorded transportation cost is highest in **{reg.index[0]}** ({reg.iloc[0]:,.2f}) and lowest in **{reg.index[-1]}** ({reg.iloc[-1]:,.2f}). Currency units are not specified in the dataset, so these figures are shown without a currency symbol.
3. **Category cost mix:** recorded transportation cost is highest for **{cat.index[0]}** ({cat.iloc[0]:,.2f}) and lowest for **{cat.index[-1]}** ({cat.iloc[-1]:,.2f}). This can help prioritize a deeper cost-per-unit or route-level analysis.
4. **Lead times are relatively close across categories:** Accessories has the highest average lead time at **{lead['Accessories']:.2f} days**, while Clothing has the lowest at **{lead['Clothing']:.2f} days**. The difference is worth exploring with supplier-level and order-level data before changing replenishment policy.
5. **Backorder flag:** {back:,} records ({back/total:.1%}) have `Backorder = TRUE`; {total-back:,} do not. Compare these rows with order status, category, region, and lead time to identify patterns.
6. **Order accuracy:** {accurate:,} records ({accurate/total:.1%}) are marked accurate and {total-accurate:,} ({(total-accurate)/total:.1%}) are marked inaccurate. Investigating inaccurate orders may reveal fulfillment or data-quality issues.
7. **Yearly units sold require context:** the yearly total rises from {int(units_year.loc[2022]):,} in 2022 to {int(units_year.loc[2024]):,} in 2024, but 2021 has only one record and 2020 has fewer records than later years. Confirm date coverage before drawing a trend conclusion.
8. **Dashboard KPIs:** the report displays warehouse utilization **34.08**, DSI **15.56**, and inventory turnover ratio **23.47**. These are useful summary indicators, but the underlying DAX definitions should be checked before making external comparisons.

## 7. Business Recommendations

- **Reduce fulfillment friction:** review pending orders by age, supplier, region, and category; track whether they are delayed, awaiting stock, or awaiting processing.
- **Investigate cancellations:** segment canceled records by region, category, supplier, and lead time to identify recurring patterns.
- **Review transportation costs:** start with the highest-cost regions and categories, then compare cost per unit, shipment distance, and carrier/service level if those fields become available.
- **Improve replenishment planning:** combine sales trends, inventory on hand, lead time, and backorder history to set category-specific reorder points and safety stock.
- **Review warehouse capacity:** validate the warehouse-utilization measure and examine warehouse-level inventory against capacity before redistributing stock.
- **Engage suppliers on lead time:** investigate categories and suppliers with consistently long or variable lead times; average lead time alone may hide outliers.
- **Improve order accuracy:** inspect inaccurate orders to identify picking, packing, data-entry, or system issues.
- **Validate time-series coverage:** investigate the sparse 2021 data and confirm that the dates and records represent comparable reporting periods.
- **Set a regular KPI review:** monitor DSI, inventory turnover, warehouse utilization, order status, and transportation cost consistently over time.

## 8. Methodology

1. **Business understanding:** define the operational questions around inventory, warehouse usage, logistics costs, sales volume, lead time, and order outcomes.
2. **Data inspection:** review the CSV schema, data types, categorical fields, and null counts.
3. **Preparation and transformation:** make sure dates and numeric fields are suitable for aggregation and reporting.
4. **Data modeling:** load the dataset into Power BI and organize fields for reporting.
5. **KPI development:** use DAX measures for dashboard-level metrics such as warehouse utilization, DSI, and inventory turnover.
6. **Visualization:** select KPI cards, a gauge, a clustered column chart, a yearly trend chart, a doughnut chart, a status-count column chart, and a regional/category inventory chart.
7. **Interactive validation:** use Region and Category slicers to explore subsets and check whether visual patterns persist.
8. **Insight generation:** translate the observed patterns into questions and practical operational recommendations.

## 9. Results and Deliverables

- An interactive Power BI dashboard consolidating inventory and supply-chain indicators.
- KPI cards for warehouse utilization, Days Sales of Inventory, and inventory turnover ratio.
- Charts for transportation cost, yearly units sold, lead time, order status, and inventory distribution.
- Region and Category slicers for focused analysis.
- A set of data-backed findings and recommendations for follow-up analysis.

The project provides visibility and decision support. It does **not** prove that costs or delays were reduced, because no before-and-after intervention or measured business outcome is included in the supplied files.

## 10. Project Files

```text
inventory-supply-chain-analysis-main/
├── Inventory Supply Chain Dataset.csv   # Source dataset
├── Inventory Supply Chain.pbix          # Power BI report/model
├── dashboard Photo.png                  # Dashboard screenshot
└── README.md                            # Project documentation
```

## 11. How to Run the Project

1. Download or clone the project repository.
2. Open `Inventory Supply Chain.pbix` in **Power BI Desktop**.
3. If Power BI asks for the source file, point it to `Inventory Supply Chain Dataset.csv` in the project folder.
4. Refresh the dataset if needed.
5. Open the report page and use the Region and Category slicers to explore the charts.
6. To validate the KPIs, inspect the DAX measures in the model and confirm their formulas and units.

## 12. Limitations and Next Steps

- The dataset does not define a currency unit for transportation cost or COGS.
- The 2021 data contains only one record, so year-over-year comparisons require caution.
- KPI values are taken from the supplied dashboard; their exact formulas should be validated in the PBIX file.
- The dashboard is descriptive. It does not forecast demand, recommend optimal reorder quantities, or prove cost savings.
- Useful future enhancements include supplier scorecards, cost per unit sold, order aging, lead-time variability, safety-stock calculations, and demand forecasting after validating data coverage.

## Skills Demonstrated

Power BI, Power Query, DAX, Excel, data preparation, data quality checks, KPI development, data visualization, exploratory analysis, business insight generation, and operational recommendations.
