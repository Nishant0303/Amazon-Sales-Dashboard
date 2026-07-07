# Power BI Build Reference

Paste these directly into Power BI Desktop against `Dataset.xlsx`. Column names match the dataset exactly, so no renaming is required.

## 1. Load
Home → Get Data → Excel → select `Dataset.xlsx` → Load (or Transform Data first, recommended).

## 2. Power Query cleaning steps (Transform Data)
Apply in this order:
1. `Remove Duplicates` — select all columns → Remove Rows → Remove Duplicates
2. `Remove Nulls` — filter `Discount` column → uncheck `(null)`, or Remove Rows → Remove Blank Rows
3. Change Type:
   - `Order Date` → Date
   - `Sales`, `Profit`, `Discount` → Decimal Number
   - `Quantity` → Whole Number
4. Home → Close & Apply

## 3. Date table (New Table, in Report/Data view)
```dax
DateTable = CALENDAR(MIN(Sales[Order Date]), MAX(Sales[Order Date]))
```
Add calculated columns on `DateTable`:
```dax
Year = YEAR(DateTable[Date])
Month = FORMAT(DateTable[Date], "MMMM")
MonthNum = MONTH(DateTable[Date])
Quarter = "Q" & FORMAT(DateTable[Date], "Q")
```
> Add `MonthNum` so you can sort the `Month` column by it (Column tool → Sort by Column) — this keeps your line chart in Jan→Dec order instead of alphabetical.

## 4. Relationship (Model view)
`Sales[Order Date]` (many) → `DateTable[Date]` (one), single direction, active.

## 5. Core measures
```dax
Total Sales = SUM(Sales[Sales])

Total Profit = SUM(Sales[Profit])

Total Orders = DISTINCTCOUNT(Sales[Order ID])

Average Order Value = DIVIDE([Total Sales], [Total Orders])

Profit Margin = DIVIDE([Total Profit], [Total Sales])
```

## 6. Extra measures used on the Customer & Product pages
```dax
Total Customers = DISTINCTCOUNT(Sales[Customer])

Repeat Customers =
VAR OrdersPerCustomer =
    SUMMARIZE(Sales, Sales[Customer], "Orders", DISTINCTCOUNT(Sales[Order ID]))
RETURN
    COUNTROWS(FILTER(OrdersPerCustomer, [Orders] > 1))

Repeat Customer % = DIVIDE([Repeat Customers], [Total Customers])

Best Selling Product =
CALCULATE(
    SELECTCOLUMNS(TOPN(1, VALUES(Sales[Product]), [Total Sales], DESC), "Product", Sales[Product])
)

YoY Sales Growth =
VAR CurrentSales = [Total Sales]
VAR PriorSales = CALCULATE([Total Sales], SAMEPERIODLASTYEAR(DateTable[Date]))
RETURN DIVIDE(CurrentSales - PriorSales, PriorSales)
```

## 7. Visual mapping (what to drop where)
| Visual | Fields |
|---|---|
| KPI cards | `[Total Sales]`, `[Total Profit]`, `[Total Orders]`, `[Profit Margin]` |
| Line chart | Axis: `DateTable[Month]` (sorted by `MonthNum`) · Values: `[Total Sales]` |
| Bar chart (Top 10 products) | Axis: `Sales[Product]` (Top N filter = 10 by `[Total Sales]`) · Values: `[Total Sales]` |
| Pie chart | Legend: `Sales[Category]` · Values: `[Total Sales]` |
| Map | Location: `Sales[State]` · Bubble/color size: `[Total Sales]` |
| Tree map | Group: `Sales[Category]` · Values: `[Total Profit]` |
| Table | `Sales[Product]`, `[Total Sales]`, `[Total Profit]`, `SUM(Sales[Quantity])` |
| Slicers | `DateTable[Year]`, `Sales[Region]`, `Sales[Category]`, `Sales[State]`, `Sales[Segment]` |

## 8. Drill-through (nice interview talking point)
Create a **Product Detail** drill-through page. Add `Sales[Product]` to the Drill-through fields well. On the bar chart, right-click any product → Drill through, to demonstrate this in a demo/interview.
