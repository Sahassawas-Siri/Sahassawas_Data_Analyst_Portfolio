# Retail Segmentation and Sales Forecasting
This project analyzes sales and customer data for a retail company operating in shopping malls. The objective is to:
1. Identify top-performing product categories and regions.
2. Optimize customer segmentation for targeted marketing.
3. Forecast future sales trends to improve resource planning.
---
## Workflow Overview
The project follows these steps:

1. **Data Cleaning**: Clean raw sales and customer data to fix invalid and missing values.
2. **SQL Analysis**: Use SQL queries to analyze revenue trends by category, region, and seasonality.
3. **Advanced Analytics**: Perform customer segmentation and sales forecasting with Python.
4. **Visualization**: Create an interactive Power BI dashboard for stakeholders.
---
# Data Cleaning 

# Step 1: Load Data
import pandas as pd

sales_data = pd.read_csv("data/dirty_sales_data_updated.csv")
customer_data = pd.read_csv("data/dirty_customer_data_updated.csv")

print(sales_data.info())
print(customer_data.info())

# Step 2: Data Cleaning - Replace 'nan' in quantity with NaN
sales_data['quantity'] = sales_data['quantity'].replace('nan', pd.NA)
sales_data['quantity'] = pd.to_numeric(sales_data['quantity'], errors='coerce')

# Fill missing quantities with median
sales_data['quantity'].fillna(sales_data['quantity'].median(), inplace=True)

# Standardize category names
sales_data['category'] = sales_data['category'].str.title().replace({'Food': 'Food & Beverage', 'Clothes': 'Clothing'})
