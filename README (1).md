# 📊 Amazon Sales Analytics Dashboard

An interactive **Power BI** dashboard that analyzes Amazon sales, profit, customer behavior, and product performance — built with Power Query, DAX, and data modeling best practices.

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-005A9C?style=for-the-badge)
![Status](https://img.shields.io/badge/status-completed-brightgreen?style=for-the-badge)

---

## 🔗 Live Demo
- **Interactive demo (this repo):** open [`dashboard.html`](dashboard.html) directly in a browser, or enable **GitHub Pages** on this repo (Settings → Pages → Deploy from branch → `main` / root) to get a public link.
- **Power BI Report (published):** [Add your Power BI Service link here once published](#)

---

## 🖼️ Dashboard Preview

| Sales Overview | Customer Analysis | Product Analysis |
|---|---|---|
| ![Dashboard](images/dashboard.png) | ![Customer](images/customer.png) | ![Product](images/product.png) |

---

## 📌 Project Overview

This project simulates a real-world business intelligence task: turning raw Amazon transactional sales data into an executive-ready, interactive dashboard that supports data-driven decisions on revenue, profitability, customers, and inventory.

**Key questions the dashboard answers:**
- What are total sales, profit, and order volume — and how are they trending?
- Which products, categories, and regions drive the most revenue and profit?
- Who are the top and repeat customers, and how do segments compare?
- Where is profit margin being eroded (e.g. by discounting)?

---

## 🗂️ Dataset

- **Source:** [Kaggle – Amazon Sales Dataset](https://www.kaggle.com/) *(replace with the exact dataset link you used)*
- **Size:** ~20,000–100,000 rows
- **Columns:** `Order ID`, `Order Date`, `Customer`, `Region`, `State`, `Category`, `Sub-Category`, `Product`, `Sales`, `Profit`, `Quantity`, `Discount`

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| **Power BI Desktop** | Dashboard design & publishing |
| **Power Query (M)** | Data cleaning & transformation |
| **DAX** | Calculated columns & measures |
| **Excel/CSV** | Raw data source |

---

## 🧹 Data Cleaning (Power Query)

- Removed duplicate rows and null values
- Renamed columns for consistency
- Converted `Order Date` to Date type
- Converted `Sales` and `Profit` to Decimal Number
- Built a dedicated **Date Table** for time intelligence:

```dax
DateTable = CALENDAR(MIN(Sales[Order Date]), MAX(Sales[Order Date]))

Year = YEAR(DateTable[Date])
Month = FORMAT(DateTable[Date], "MMMM")
Quarter = "Q" & FORMAT(DateTable[Date], "Q")
```

Relationship: `Sales[Order Date]` → `DateTable[Date]` (one-to-many, single direction).

---

## 📐 Data Model

A simple **star schema**: one fact table (`Sales`) connected to the `DateTable` dimension, keeping the model fast and interview-friendly to explain.

---

## 🧮 Key DAX Measures

```dax
Total Sales = SUM(Sales[Sales])

Total Profit = SUM(Sales[Profit])

Total Orders = DISTINCTCOUNT(Sales[Order ID])

Average Order Value = DIVIDE([Total Sales], [Total Orders])

Profit Margin = DIVIDE([Total Profit], [Total Sales])
```

---

## 📑 Report Pages

### 1. Sales Overview
- KPI cards: Total Sales, Total Profit, Total Orders, Profit Margin
- Monthly sales trend (line chart)
- Top 10 products (bar chart)
- Sales by category (pie chart)
- Sales by state (map)
- Category profit (tree map)
- Detailed product table

### 2. Customer Analysis
- Top customers
- Repeat customers
- Sales by customer segment
- Customer-wise sales table

### 3. Product Analysis
- Best- and least-selling products
- Product-level profit
- Quantity sold
- Inventory trends *(if available in source data)*

**Interactivity:** Slicers for Year, Region, Category, State, and Customer Segment across all pages.

---

## 🎨 Design

- Palette: Dark Blue / Orange / White
- Consistent fonts, rounded corners, and titled visuals for a clean, professional look

---

## 🚀 How to Use

1. Clone this repo
   ```bash
   git clone https://github.com/<your-username>/Amazon-Sales-Dashboard.git
   ```
2. Open `Dashboard.pbix` in Power BI Desktop
3. If prompted, update the data source path to point to `Dataset.xlsx`
4. Refresh data (Home → Refresh)

---

## 📁 Repository Structure

```
Amazon-Sales-Dashboard/
│
├── Dashboard.pbix
├── Dataset.xlsx
├── dashboard.html                       ← interactive live demo (open in any browser)
├── DAX_and_PowerQuery_Reference.md       ← every M/DAX snippet used, ready to paste
├── README.md
├── .gitignore
└── images/
    ├── dashboard.png
    ├── customer.png
    └── product.png
```

---

## 💼 About This Project

Built as a portfolio project to demonstrate Power BI, DAX, and Power Query skills for business intelligence and data analyst roles.

**Author:** *Your Name*
**LinkedIn:** *Add link*
**Portfolio/CV:** *Add link*
