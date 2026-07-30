# Generating Visualizations

This document explains how to generate charts for all SQL query results.

## Prerequisites

```bash
pip install pandas matplotlib seaborn
```

## Running the Script

```bash
python generate_visualizations.py
```

## Output

The script creates a `/visualizations/` folder with 10 PNG charts:

1. **01_clv_ranking.png** - Top customers by lifetime value
<img width="1489" height="590" alt="image" src="https://github.com/user-attachments/assets/9cb08b09-932e-42b2-9075-54386daeeb94" />

2. **02_customer_tenure.png** - Customer distribution by years active
3. **03_gap_analysis.png** - Days between purchases distribution
4. **04_churn_risk.png** - Customers by risk level
5. **05_monthly_revenue.png** - Monthly sales trend
6. **06_mom_growth.png** - Month-over-month growth percentage
7. **07_yoy_growth.png** - Year-over-year sales and growth rate
8. **08_branch_ranking.png** - Branch sales comparison
9. **09_best_branch_monthly.png** - Top branch each month
10. **10_customer_preference.png** - Customer branch preferences

## What Each Chart Shows

### CLV Ranking
- Bar chart of top 10 customers by total spending
- Scatter plot showing relationship between rank and spending amount

### Customer Tenure
- Bar chart showing how many customers have 0, 1, 2, 3+ years tenure
- Pie chart showing percentage breakdown

### Gap Analysis
- Histogram showing frequency of purchase gaps (7 days, 30 days, 90 days, etc.)
- Line chart tracking how purchase frequency changes

### Churn Risk
- Horizontal bar chart ranking customers by risk level
- Pie chart showing percentage of customers in each risk category

### Monthly Revenue
- Line chart tracking sales trend throughout the year
- Area chart showing cumulative revenue by month

### MoM Growth
- Bar chart showing monthly growth percentage (green for positive, red for negative)

### YoY Growth
- Line chart showing annual sales for 2020-2026
- Bar chart showing year-over-year growth rate percentage

### Branch Ranking
- Horizontal bar chart comparing total sales across 4 branches
- Pie chart showing each branch's share of total revenue

### Best Branch Monthly
- Bar chart showing which branch performed best each month
- Color-coded by branch for easy identification

### Customer Preference
- Bar chart showing count of favorite vs least-favorite branch preferences
- Pie chart showing distribution percentages

## Customization

You can modify the script to:
- Change colors: Edit the `colors` variable
- Adjust figure size: Modify `figsize` parameter
- Add your actual data: Replace sample data with CSV imports

## Using with Real Data

To generate charts from actual query results:

```python
import pandas as pd

# Load CSV exported from SQL queries
df = pd.read_csv('your_query_results.csv')

# Generate chart (example)
plt.figure(figsize=(12, 6))
plt.bar(df['column1'], df['column2'])
plt.title('Your Chart Title')
plt.savefig('visualizations/your_chart.png')
plt.close()
```

## Notes

- Charts are saved at 300 DPI for high quality
- All charts include gridlines and clear labels
- Color scheme is consistent across all visualizations
- Dimensions are optimized for GitHub README display

---

For more information about queries, see **README.md**
