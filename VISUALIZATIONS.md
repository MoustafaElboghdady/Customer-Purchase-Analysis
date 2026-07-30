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
<img width="1396" height="590" alt="image" src="https://github.com/user-attachments/assets/57079825-8059-49c9-9704-dcc002c9e9c3" />

3. **03_gap_analysis.png** - Days between purchases distribution
<img width="1490" height="590" alt="image" src="https://github.com/user-attachments/assets/27940aad-c09f-48fe-b403-813c4c29362d" />

4. **04_churn_risk.png** - Customers by risk level
<img width="1445" height="590" alt="image" src="https://github.com/user-attachments/assets/e98bca3b-876e-4515-a264-365438f7f26a" />

5. **05_monthly_revenue.png** - Monthly sales trend in 2020
<img width="1489" height="990" alt="image" src="https://github.com/user-attachments/assets/df5663f1-41b3-4a8d-966b-52b108f69fe7" />

6. **06_mom_growth.png** - Month-over-month growth percentage for 2020
<img width="1489" height="590" alt="image" src="https://github.com/user-attachments/assets/8189c556-657d-4a50-bb36-82ca2e3a3cdc" />

7. **07_yoy_growth.png** - Year-over-year sales and growth rate
<img width="1589" height="590" alt="image" src="https://github.com/user-attachments/assets/d26845bd-0b5a-45b8-8cbe-be86783bf981" />

8. **08_branch_ranking.png** - Branch sales comparison
<img width="1589" height="590" alt="image" src="https://github.com/user-attachments/assets/b5cfb913-44d0-4f9e-91f5-a95d75033c68" />

9. **09_best_branch_monthly.png** - Top branch each month 2020
<img width="1489" height="590" alt="image" src="https://github.com/user-attachments/assets/1fb49b93-f217-4899-8012-ddd086d46fdc" />



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
