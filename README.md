# Amazon Sales Dashboard
This project showcases an Amazon Sales Dashboard built to practice and improve skills in data visualization and data analytics. The dashboard provides a visual representation of sales performance across different regions, product categories, and order statuses. Note that all data in this project is fake and was generated using Python's Faker library. The dataset used here is purely for learning and demonstration purposes, and it does not reflect any real Amazon sales data.

#Features
Total Orders and Sales (2020-2024): Provides a quick summary of total orders, total sales, and overall profit.
Sales and Profit by Region: Visualizes sales and profit distribution across four regions (North, South, East, West).
Top 5 Selling Products: Highlights the best-selling products based on quantity.
Regional Sales Performance: Displays sales data on a map to provide geographic insights.
Sales vs Profit Comparison: Compares sales and profit for each product category.
Order Status: Breaks down order statuses, including Shipped, Delivered, and Pending percentages.
Additionally, there’s a navigation icon labeled “Dataset” in the dashboard. Clicking this icon redirects users to a new dashboard view that presents a detailed dataset view. This feature provides easy navigation between the main dashboard and dataset details.

#Data Generation
As mentioned, this is not real data. All data points shown in the dashboard were generated using the Faker library in Python. This library allowed us to create a realistic-looking dataset by generating fake names, addresses, product categories, and numerical data.

Why Faker?
Using Faker is an excellent approach for data practitioners who want to create realistic data without violating privacy or using confidential information. It's particularly useful for generating sample datasets for visualization, testing, and other analytical purposes.

#Dashboard Navigation
The dashboard includes several interactive components, allowing users to filter and view data by:

Product Category: Filter by categories such as Clothing, Electronics, Furniture, and Office Supplies.
Year Range: Adjust the slider to view data from 2020 to 2024.
Technologies Used
Python: For data generation and preprocessing
Pandas: For data manipulation
Plotly/Dash or Power BI/Tableau: (Specify the tool) for dashboard creation and visualization
Faker: For generating the fake dataset
How to Use the Dashboard
Explore Sales and Profit: Analyze overall orders, sales, and profit across different metrics.
Region-Based Analysis: Use the “Sales by Region” and “Profit by Region” charts to understand how each region contributes to the business.
Product Insights: View top-selling products and their corresponding quantities.
Order Status Breakdown: Check the shipment status to understand the order flow.
Navigate to Dataset: Click on the “Dataset” icon to open a new dashboard view where the full dataset is displayed in tabular form.

#Code for Dataset Generating 
import pandas as pd
import numpy as np
import random
from faker import Faker

# Initialize Faker
fake = Faker()

# Define number of rows
num_rows = 10000

# Define order IDs
order_ids = [f"ORD-{i:04}" for i in range(1, num_rows + 1)]

# Define subcategories with average sales and profit margins
subcategories = {
    'Electronics': {
        'Cell Phones': {'avg_sales': 600, 'avg_profit': 150},
        'Laptops': {'avg_sales': 800, 'avg_profit': 200},
        'WiFi': {'avg_sales': 150, 'avg_profit': 30}
    },
    'Furniture': {
        'Tables': {'avg_sales': 300, 'avg_profit': 80},
        'Chairs': {'avg_sales': 150, 'avg_profit': 40},
        'Doors': {'avg_sales': 200, 'avg_profit': 50}
    },
    'Clothing': {
        'Shirts': {'avg_sales': 30, 'avg_profit': 10},
        'Pants': {'avg_sales': 40, 'avg_profit': 15},
        'Tops': {'avg_sales': 25, 'avg_profit': 8},
        'Jeans': {'avg_sales': 50, 'avg_profit': 20},
        'T-Shirts': {'avg_sales': 20, 'avg_profit': 5}
    },
    'Office Supplies': {
        'Papers': {'avg_sales': 10, 'avg_profit': 2},
        'Printers': {'avg_sales': 200, 'avg_profit': 50},
        'Desktops': {'avg_sales': 500, 'avg_profit': 100}
    }
}

# Create a list to ensure all categories and subcategories are included
data = []

# Prepopulate the dataset with varied data
for _ in range(num_rows):
    category = random.choices(list(subcategories.keys()), weights=[0.4, 0.2, 0.3, 0.1])[0]
    subcategory = random.choice(list(subcategories[category].keys()))

    # Varying quantity based on category
    quantity = random.randint(1, 5) if category != 'Electronics' else random.randint(1, 3)

    # Varying discounts
    discount = round(random.uniform(0, 0.25), 2) if random.random() > 0.1 else 0.0  # 10% chance of no discount

    # Random order date in the past decade
    order_date = fake.date_this_decade()

    # Randomly assign regions based on probabilities
    region = random.choices(['North', 'South', 'East', 'West'], weights=[0.25, 0.25, 0.25, 0.25])[0]

    # Add entry to data
    data.append({
        "OrderID": f"ORD-{len(data)+1:04}",
        "OrderDate": order_date,
        "CustomerID": f"CUST-{random.randint(1000, 9999)}",
        "CustomerName": fake.name(),
        "ProductID": f"PROD-{random.randint(100, 999)}",
        "ProductCategory": category,
        "ProductSubCategory": subcategory,
        "Quantity": quantity,
        "Discount": discount,
        "Region": region,
        "State": fake.state(),
        "City": fake.city(),
        "CustomerSegment": random.choice(['Consumer', 'Corporate', 'Home Office']),
        "ReturnStatus": random.choices(['Yes', 'No'], weights=[0.1, 0.9])[0],  # 10% return rate
        "PaymentMode": random.choice(['Credit Card', 'Cash', 'Digital Wallet', 'Bank Transfer']),
        "EmployeeID": f"EMP-{random.randint(100, 999)}",
        "EmployeeName": fake.name(),
        "Rating": random.randint(1, 5),
        "OrderTotal": 0,  # Placeholder
        "OrderStatus": random.choices(['Shipped', 'Pending', 'Delivered'], weights=[0.6, 0.2, 0.2])[0],
        "CustomerAge": random.randint(18, 65),
        "ProductRating": round(random.uniform(1, 5), 1),
        "PaymentMethodFee": round(random.uniform(0, 5), 2),
        "ShippingCost": round(random.uniform(5, 20), 2),
        "PromotionCode": random.choice(['PROMO10', 'PROMO20', 'NONE']),
        "SalesChannel": random.choice(['Online', 'In-Store', 'Mobile']),
        "ProductSize": random.choice(['S', 'M', 'L', 'XL']),
        "ReturnReason": random.choice(['Defective', 'Wrong Size', 'No Reason', None])
    })

# Create the DataFrame
df = pd.DataFrame(data)

# Function to calculate Sales and Profit
def calculate_sales_profit(row):
    category = row['ProductCategory']
    subcategory = row['ProductSubCategory']

    if category in subcategories and subcategory in subcategories[category]:
        avg_sales = subcategories[category][subcategory]['avg_sales']
        avg_profit = subcategories[category][subcategory]['avg_profit']

        # Calculate sales and profit based on quantity and discounts
        sales = avg_sales * row['Quantity'] * (1 - row['Discount'])
        profit = avg_profit * row['Quantity']

        return sales, profit
    return 0, 0

# Apply the function to calculate Sales and Profit
df['Sales'], df['Profit'] = zip(*df.apply(calculate_sales_profit, axis=1))

# Save updated dataset to Excel
df.to_excel("Flipcart_sales_data_realistic.xlsx", index=False)

#Acknowledgments
Faker Library for enabling data generation.
Tableau for powerful data visualization tools.
Pandas profiling for Data Understanding.

