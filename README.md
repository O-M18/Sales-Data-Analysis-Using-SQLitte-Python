# **Sales Data Analysis using SQLite & Python**
---
## **Project Overview**
This project utilizes **SQLite** as the database engine to store and analyze retail sales data. **Pandas** is used for handling datasets, and **Matplotlib/Seaborn** generate visual insights into revenue trends, product performance, and customer purchasing behavior.

---

## **Features**
✅ **Store & manage sales data in SQLite**  
✅ **Extract insights using SQL queries & Pandas**  
✅ **Visualize trends with Matplotlib & Seaborn**  
✅ **Optimize query execution for large datasets**  
✅ **Convert & clean date/time formats for analysis**

---

## **Technologies Used**
- **Database**: SQLite  
- **Programming Language**: Python (Pandas, SQLite3, Matplotlib, Seaborn)  
- **Visualization Tools**: Matplotlib, Seaborn  

---

## **Database Setup**
### **1️ Create & Upload Data**
Convert and load the sales dataset into SQLite before analysis:
```python
import sqlite3
import pandas as pd
try:
    df = pd.read_csv(file, encoding="utf-8")
except UnicodeDecodeError:
    df = pd.read_csv(file, encoding="latin1")
conn=sqlite3.connect('sales_data.db')
df.to_sql('sales',conn,index=False)
result=pd.read_sql_query('Select * from sales limit 10;',conn)
print(result)
conn.close()

```

---

### **2️ Querying Data**
Retrieve monthly revenue from the database:
```python
query = '''SELECT strftime('%Y-%m', InvoiceDate) AS Month, 
                  SUM(Quantity * Price) AS revenue
           FROM sales
           GROUP BY Month
           ORDER BY revenue DESC;'''

conn = sqlite3.connect("sales_data.db")
df_ = pd.read_sql_query(query, conn)
print(df_.head())  # Display the top results
```

---

## **Visualizations**
### **1 Sales Trends Over Time**
```python
import matplotlib.pyplot as plt
import seaborn as sns

plt.figure(figsize=(12,6))
sns.lineplot(data=df_, x='Month', y='revenue')
plt.xlabel("Month")
plt.ylabel("Total Revenue")
plt.title(" Sales Trends Over Time")
plt.xticks(rotation=45)
plt.show()
```
 **Insight**: Tracks revenue fluctuations to identify peak sales periods.

---

### **2️ Top-Selling Products**
```python
query = '''SELECT Description, SUM(Quantity) AS total_sold
           FROM sales
           GROUP BY Description
           ORDER BY total_sold DESC
           LIMIT 10;'''
df_top_products = pd.read_sql_query(query, conn)

plt.figure(figsize=(12,6))
sns.barplot(data=df_top_products, x='total_sold', y='Description', palette="coolwarm")
plt.xlabel("Total Sold")
plt.ylabel("Product")
plt.title(" Top-Selling Products")
plt.show()
```
 **Insight**: Helps businesses focus on high-demand products.

---

### **3️ Customer Spending Distribution**
```python
query = '''SELECT Customer_ID, SUM(Quantity * Price) AS total_spent
           FROM sale
           GROUP BY Customer_ID
           ORDER BY total_spent DESC;'''
df_customers = pd.read_sql_query(query, conn)

plt.figure(figsize=(12,6))
sns.histplot(df_customers['total_spent'], bins=30, kde=True, color="green")
plt.xlabel("Total Amount Spent")
plt.ylabel("Number of Customers")
plt.title(" Customer Spending Distribution")
plt.show()
```
 **Insight**: Identifies customer spending patterns.

---

## **Next Steps**
 **Optimize queries** for faster execution  
 **Deploy as a dashboard using Flask or Streamlit**  
 **Enhance visualization with interactive tools**  
---

### **Author**
Developed by **OM Mane**  



