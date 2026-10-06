# Superstore Sales Analysis

An exploratory data analysis (EDA) of the classic **Superstore** retail dataset (9,994 order lines, 21 columns, 2014–2017) using Python, pandas, NumPy, Matplotlib and Seaborn. The project covers data cleaning, consistency checks, descriptive statistics, business-oriented analysis (sales, regions, customers, time series, discounts), pivot tables, a dataset merge, and visualizations.

## Repository Contents

| File | Description |
|------|-------------|
| `notebooks/superstore_analysis.ipynb` | Main notebook with the full analysis |
| `data/Superstore21.csv` | Source dataset (read with `latin1` encoding) |
| `data/Region.csv` | Small auxiliary table (Region → Manager) generated in the notebook |

> **Note:** The notebook currently reads the data from an absolute Windows path. Update the `pd.read_csv(...)` and `Df.to_csv(...)` paths to match your own folder layout before running.

## Requirements

- Python 3.11+
- pandas
- numpy
- matplotlib
- seaborn
- Jupyter Notebook / JupyterLab

```bash
pip install pandas numpy matplotlib seaborn jupyter
```

## Dataset Overview

Each row is one product line within an order. Key columns:

- **Order info:** `Order ID`, `Order Date`, `Ship Date`, `Ship Mode`
- **Customer:** `Customer ID`, `Customer Name`, `Segment` (Consumer / Corporate / Home Office)
- **Geography:** `Country`, `City`, `State`, `Postal Code`, `Region` (East / West / Central / South)
- **Product:** `Product ID`, `Category`, `Sub-Category`, `Product Name`
- **Metrics:** `Sales`, `Quantity`, `Discount`, `Profit`

## Workflow

### 1. Data Loading & Inspection
Shape, dtypes, `head()` / `tail()`, and summary statistics.

### 2. Data Cleaning
- **Missing values:** Missing postal codes belonged to Burlington, Vermont, and were filled with `05401`. Postal codes were converted to zero-padded 5-character strings.
- **Duplicates:** One row (Row ID 3407) was identical to the preceding row apart from `Row ID`; it was treated as a duplicate and removed (9,993 rows remain).
- **Categorical consistency:** `Ship Mode`, `Segment`, `Country`, `State`, `Region`, and the `Category` ↔ `Sub-Category` mapping were validated.
- **Product ID/Name mismatches:** Some Product IDs had multiple names (standardized to a single name per ID), and some names mapped to multiple IDs (resolved by creating a `Unique Product Name` column in the form `Name(ID)`).
- **Numeric validity:** No zero or negative `Sales` values.
- **Dates:** `Order Date` and `Ship Date` converted to datetime; verified that no shipment precedes its order.

### 3. Statistical Analysis
- Descriptive statistics with pandas and NumPy.
- Mean vs. median comparison shows right-skewed Sales and Profit.
- Coefficient of variation: Profit (≈8.17) is more variable than Sales (≈2.71).
- IQR outlier detection: 1,167 Sales values above the upper bound.

### 4. Business Analysis
- **Sales:** totals, average per order, top category / sub-category / product, products with the worst profit-to-sales ratio.
- **Regional:** best region/state/city, average order value, profit ratio per region.
- **Customers:** top customers by sales and profit, segment performance.
- **Time series:** monthly and yearly sales/profit, strongest months and seasonality.
- **Discounts:** average discount per order, profit by discount level, discount by category, high-sales loss-making lines.
- **Pivot tables:** Region × Category, Segment × Category, Month × Category × Year.
- **Merge:** joined a Region → Manager lookup table to the data and ranked managers by sales and profit.

### 5. Visualizations
- Total sales by region (bar)
- Total profit by region (bar)
- Sales and profit by category (grouped bar)
- Monthly sales over time (line)
- Monthly profit over time (line)
- Discount vs. profit (scatter)

## Key Findings

- **Sales drivers:** Technology is the top category for both sales and profit; Phones lead in sales, Copiers lead in profit.
- **Regions:** West is first in sales and profit, with East close behind; South is weakest in sales. Central has relatively high sales but the lowest profit margin (~7.9%).
- **Segments:** Consumer is the largest segment by sales and profit; Home Office has the highest average order value.
- **Furniture** is the least profitable category by a wide margin (net loss in the Central region).
- **Time trend:** Sales fluctuate month to month but trend upward in later years, with a strong seasonal peak from September to December (November 2017 is the highest month at ~$118K).
- **Discounts:** Higher discounts are associated with lower, often negative, profit. Orders with no discount account for the bulk of total profit. This is an association, not proof of causation.
- **Profitability vs. sales:** More sales do not automatically mean more profit (e.g., 2015 had lower sales but higher profit than 2014; Furniture outsells Office Supplies in some cases yet earns less).

## Data Quality Notes

1. One duplicated order line (differing only by `Row ID`) was removed.
2. Product ID ↔ Product Name inconsistencies required standardization and a derived unique-name column.

## How to Run

1. Place `Superstore21.csv` in a local `data/` folder.
2. Edit the file paths in the notebook to point to your local copy.
3. Launch Jupyter and run all cells in order:

```bash
jupyter notebook superstore_analysis.ipynb
```
