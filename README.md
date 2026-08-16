# DataCo Supply Chain Performance Analysis

An end-to-end exploratory analysis of the DataCo Supply Chain dataset (~180K order records, 2015–2018), focused primarily on **shipping and delivery performance**, with supporting analysis of sales, profitability, regional distribution, and product category performance.

## Problem Statement

Late deliveries are a core operational risk in supply chain management — they damage customer trust and can erode profitability. This project examines:
- Where and how late deliveries happen (by shipping mode, region, and market)
- Whether operational performance (on-time delivery) is linked to business outcomes (profit)
- How sales and profitability are distributed across product categories and regions

## Dataset

- **Source:** DataCo Supply Chain Dataset
- **Size:** ~180,500 rows × 53 original columns (columns reduced during cleaning)
- **Period covered:** January 2015 – January 2018
- **Grain:** One row per order line item

## Methodology

### 1. Data Cleaning
- Removed PII columns with no analytical value: `Customer Email`, `Customer Password`, `Customer Fname`, `Customer Lname`, `Customer Street`
- Removed empty/near-useless columns: `Product Description` (100% missing), `Product Image` (URL only), `Order Zipcode` (~86% missing), `Product Status` (~99% missing)
- Removed redundant duplicate ID columns: `Order Customer Id`, `Product Category Id`, `Order Item Cardprod Id`
- Converted `Order Date` and `Shipping Date` to proper datetime types
- Filled missing `Customer Zipcode` values with a placeholder

### 2. Shipping & Delivery Performance (primary focus)
- Late delivery rate by shipping mode, order region, and market
- Shipping gap (`actual shipping days − scheduled shipping days`) by the same groupings
- Cross-check between late delivery rate and shipping gap to confirm consistency
- [Add: profit vs. delivery-status comparison, if completed]

### 3. Sales & Profitability
- Total sales and profit by product category
- Profit margin by category, to surface high-revenue/low-margin and low-revenue/high-margin outliers

### 4. Regional / Market Breakdown
- Order volume, total sales, and total profit by order region and order country

### 5. Product / Category Performance
- Top and bottom categories by sales
- Top and bottom categories by profit margin
- Comparison of the two rankings to highlight divergence

### 6. Visualization
- Monthly sales trend (time series, Jan 2015 – Jan 2018)
- Bar charts: sales/profit by category and by region
- Heatmap: late delivery rate by region × shipping mode

## Key Findings

**Overall late delivery rate: 54.8%** of all order lines (98,977 of 180,519) were delivered late. This is the headline number for the whole analysis — more than half of all orders miss their scheduled delivery window.

### Late delivery rate by shipping mode

| Shipping Mode | Late Delivery Rate |
|---|---|
| First Class | 95.3% |
| Second Class | 76.6% |
| Same Day | 45.7% |
| Standard Class | 38.1% |

Counterintuitively, the *faster/premium* shipping modes have the worst on-time performance. First Class is late 95% of the time, while Standard Class — the slowest scheduled mode — is on time most often. This is because premium modes have tighter scheduled windows, leaving little slack to absorb any delay.

### Average shipping gap (actual − scheduled days) by shipping mode

| Shipping Mode | Avg. Gap (days) |
|---|---|
| Second Class | +1.99 |
| First Class | +1.00 |
| Same Day | +0.48 |
| Standard Class | −0.004 |

This confirms the late-rate pattern: Second Class and First Class consistently run over their scheduled window, while Standard Class is essentially on target on average.

### Late delivery rate by region (top 5 worst / best)

| Worst 5 Regions | Rate | Best 5 Regions | Rate |
|---|---|---|---|
| Central Africa | 58.0% | Canada | 48.8% |
| South Asia | 56.3% | West Africa | 52.8% |
| East Africa | 55.9% | Caribbean | 53.1% |
| Western Europe | 55.9% | Southern Africa | 53.3% |
| South of USA | 55.8% | West of USA | 54.0% |

Regional variation is much smaller than the shipping-mode effect — most regions cluster around 53–58%. Canada stands out as the one clear outlier with meaningfully better performance.

### Profit vs. delivery status

| Delivery Status | Avg. Profit/Order | Avg. Profit Ratio |
|---|---|---|
| On-time (0) | $22.40 | 0.122 |
| Late (1) | $21.62 | 0.120 |

Profit is nearly identical between on-time and late orders — a ~3.5% difference. **Late delivery does not appear to meaningfully erode per-order profitability** in this dataset, which is itself a notable finding: the cost of lateness here shows up as a customer-experience/operational risk, not a direct margin hit.

### Sales & profitability by category

**Top 5 by total sales:**

| Category | Total Sales | Total Profit | Profit Margin |
|---|---|---|---|
| Fishing | $6.93M | $756.2K | 10.9% |
| Cleats | $4.43M | $494.6K | 11.2% |
| Camping & Hiking | $4.12M | $427.5K | 10.4% |
| Cardio Equipment | $3.69M | $383.0K | 10.4% |
| Women's Apparel | $3.15M | $350.4K | 11.1% |

**Top 5 by profit margin (diverges from sales ranking):**

| Category | Total Sales | Profit Margin |
|---|---|---|
| Golf Bags & Carts | $10.4K | 17.5% |
| Fitness Accessories | $35.6K | 14.8% |
| Toys | $6.1K | 14.8% |
| Soccer | $26.5K | 14.7% |
| Baseball & Softball | $94.1K | 13.6% |

**Bottom 5 by profit margin:**

| Category | Total Sales | Profit Margin |
|---|---|---|
| Strength Training | $54.9K | 0.6% |
| As Seen on TV! | $20.6K | 3.5% |
| Men's Clothing | $43.9K | 4.6% |
| Basketball | $27.1K | 6.8% |
| Books | $12.6K | 7.0% |

This is the clearest divergence in the dataset: none of the top-selling categories (Fishing, Cleats, Camping & Hiking) appear in the top-margin list. High-volume categories run steady ~10–11% margins, while the best margins come from low-volume niche categories. **Strength Training** is a specific red flag — decent sales volume ($54.9K) but almost no profit (0.6% margin).

### Regional breakdown

**Top 5 regions by total profit:**

| Region | Orders | Total Sales | Total Profit |
|---|---|---|---|
| Western Europe | 10,010 | $5.89M | $625.4K |
| Central America | 9,396 | $5.67M | $616.3K |
| South America | 4,979 | $2.96M | $335.2K |
| Northern Europe | 3,716 | $2.16M | $233.5K |
| Southern Europe | 3,543 | $2.05M | $230.8K |

**Bottom 5 regions by total profit:**

| Region | Orders | Total Sales | Total Profit |
|---|---|---|---|
| Central Asia | 184 | $109.8K | $13.0K |
| Canada | 309 | $186.9K | $23.9K |
| Southern Africa | 398 | $228.3K | $30.8K |
| Central Africa | 556 | $327.3K | $33.4K |
| East Africa | 613 | $376.2K | $43.2K |

Profit by region tracks order volume closely — no region here shows high volume with disproportionately low profit, unlike the category-level margin story above.

### Overall totals

| Metric | Value |
|---|---|
| Total orders analyzed | 180,519 order lines |
| Total sales | $36.78M |
| Total profit | $3.97M |
| Avg. profit per order line | $21.97 |
| Date range | Jan 2015 – Jan 2018 |

## Tools Used
- Python: pandas, NumPy
- Visualization: matplotlib, seaborn
- Jupyter Notebook

## Project Structure
```
supply-chain-analysis/
├── data/
│   └── DataCoSupplyChainDataset.csv
├── notebooks/
│   ├── 1_Loading_Dataset.ipynb
│   ├── 2_Dataset_Cleanup.ipynb
│   ├── 3_Shipping_delivery_performance.ipynb
│   ├── 4_Sales_profitability.ipynb
│   ├── 5_Regional_market_breakdown.ipynb
│   ├── 6_Product_category_performance.ipynb
│   └── 7_Visualization.ipynb
├── outputs/
│   └── figures/
└── README.md
```

## Author
Muhammad Hashir — BBA (Supply Chain Management specialization), NUML Islamabad
[LinkedIn](https://www.linkedin.com/in/muhammad-hashir-data) · [GitHub](https://github.com/MuhammadHashir109)
