# Retail Segmentation and Sales Forecasting

This project analyzes sales and customer data for a retail company to identify:
1. Top-performing product categories and regions.
2. Customer segmentation for targeted marketing.
3. Seasonal sales trends and future revenue forecasts.

## Key Objectives:
- Clean and preprocess sales and customer data.
- Analyze revenue trends using SQL.
- Perform advanced analytics (clustering and forecasting) with Python.
- Visualize insights with an interactive Power BI dashboard.

## Workflow Overview

1. **Data Cleaning**: Clean raw sales and customer data using Python.
2. **SQL Analysis**: Extract insights on revenue trends by category, region, and seasonality.
3. **Advanced Analytics**: Perform clustering for customer segmentation and forecast sales trends using Python.
4. **Visualization**: Create an interactive Power BI dashboard to present findings.

Each step is detailed below with code examples and outputs.

### Step 1: Data Cleaning
**Objective**: Clean and preprocess raw sales and customer data to handle missing values, invalid entries, and standardize formats.

**Files Used**:
- `dirty_sales_data_updated.csv`
- `dirty_customer_data_updated.csv`

**Code**:
```python
import pandas as pd

# Load data
sales_data = pd.read_csv("data/dirty_sales_data_updated.csv")
customer_data = pd.read_csv("data/dirty_customer_data_updated.csv")

# Replace 'nan' in quantity with NaN
sales_data['quantity'] = sales_data['quantity'].replace('nan', pd.NA)
sales_data['quantity'] = pd.to_numeric(sales_data['quantity'], errors='coerce')

# Fill missing quantities with median
sales_data['quantity'].fillna(sales_data['quantity'].median(), inplace=True)

# Standardize category names
sales_data['category'] = sales_data['category'].str.title().replace({'Food': 'Food & Beverage', 'Clothes': 'Clothing'})

# Clean customer data
customer_data['payment_method'] = customer_data['payment_method'].replace('Paypal', 'Unknown')
customer_data['payment_method'] = customer_data['payment_method'].str.title()

# Save cleaned data
merged_data = pd.merge(sales_data, customer_data, on="customer_id", how="inner")
merged_data['Revenue'] = merged_data['quantity'] * merged_data['price']
merged_data.to_csv("data/cleaned_merged_data.csv", index=False)

from statsmodels.tsa.arima.model import ARIMA

# Aggregate daily revenue
time_series = merged_data.groupby('invoice_date')['Revenue'].sum()

# Fit ARIMA model
model = ARIMA(time_series, order=(5, 1, 0))
model_fit = model.fit()

# Forecast
forecast = model_fit.forecast(steps=30)
forecast.to_csv('data/forecast.csv', index=True)

