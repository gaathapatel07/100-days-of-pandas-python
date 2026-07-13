
# 🐼 Day 41 — Advanced File Handling & Data Integration in Pandas

<div align="center">

# 100 Days of Pandas

### Day 41 · Importing, Exporting & Integrating Data from Multiple Sources

*"Data becomes valuable only after it is collected, integrated, and prepared for analysis."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-File%20Handling%20%26%20Data%20Integration-blue)
![Day](https://img.shields.io/badge/Day-41-orange)

</div>

---

# 📚 Table of Contents

1. Introduction
2. Why File Handling Matters
3. Learning Objectives
4. Reading CSV Files
5. Reading Excel Files
6. Reading JSON Files
7. Writing Data to Files
8. Summary

---

# 1. Introduction

Businesses collect data from many different systems.

Common sources include:

* CSV files
* Excel spreadsheets
* JSON APIs
* SQL databases
* Cloud storage
* ERP systems
* CRM platforms
* IoT devices

Pandas provides powerful functions to import, export, and integrate data from these sources.

---

# 2. Why File Handling Matters

Imagine an e-commerce company.

Sales data comes from:

* Website orders (CSV)
* Inventory system (Excel)
* Customer profiles (SQL Database)
* Payment gateway (JSON API)

Before analysis, these datasets must be imported and combined into a single DataFrame.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Read data from multiple file formats.
* Export processed datasets.
* Work with Excel workbooks.
* Read JSON files.
* Build data integration workflows.

---

# 4. Reading CSV Files

CSV (Comma-Separated Values) is the most common data format.

Basic import:

```python id="csv01"
import pandas as pd

df = pd.read_csv(
    "sales.csv"
)
```

View the data.

```python id="csv02"
df.head()
```

---

## Read Specific Columns

```python id="csv03"
df = pd.read_csv(
    "sales.csv",
    usecols=[
        "Customer ID",
        "Sales",
        "Region"
    ]
)
```

---

## Read Limited Rows

```python id="csv04"
df = pd.read_csv(
    "sales.csv",
    nrows=1000
)
```

Useful for exploring very large datasets.

---

## Parse Dates During Import

```python id="csv05"
df = pd.read_csv(
    "sales.csv",
    parse_dates=[
        "Order Date"
    ]
)
```

This automatically converts the specified column to `datetime64`.

---

# 5. Reading Excel Files

Read the first worksheet.

```python id="excel01"
df = pd.read_excel(
    "sales.xlsx"
)
```

---

## Read a Specific Sheet

```python id="excel02"
df = pd.read_excel(
    "sales.xlsx",
    sheet_name="Orders"
)
```

---

## Read Multiple Sheets

```python id="excel03"
sheets = pd.read_excel(
    "sales.xlsx",
    sheet_name=None
)
```

Output

```text id="excel04"
{
    "Orders": DataFrame,
    "Customers": DataFrame,
    "Products": DataFrame
}
```

Access a sheet.

```python id="excel05"
orders = sheets["Orders"]
```

---

# 6. Reading JSON Files

JSON is widely used by APIs and web applications.

Read a JSON file.

```python id="json01"
df = pd.read_json(
    "customers.json"
)
```

---

## Normalize Nested JSON

Example JSON:

```json
{
  "customer": {
    "id": 101,
    "name": "Alice"
  }
}
```

Flatten nested data.

```python id="json02"
from pandas import json_normalize

flat = json_normalize(data)
```

---

# 7. Writing Data to Files

Export to CSV.

```python id="write01"
df.to_csv(
    "clean_sales.csv",
    index=False
)
```

---

## Export to Excel

```python id="write02"
df.to_excel(
    "report.xlsx",
    index=False
)
```

---

## Export to JSON

```python id="write03"
df.to_json(
    "output.json",
    orient="records",
    indent=4
)
```

---

## Export Selected Columns

```python id="write04"
df[
    [
        "Customer ID",
        "Sales"
    ]
].to_csv(
    "summary.csv",
    index=False
)
```

---

# Business Example

A logistics company receives:

* Orders in CSV format.
* Driver schedules in Excel.
* GPS tracking information as JSON.

The analytics team:

* Imports all three sources.
* Standardizes column names.
* Converts dates.
* Combines datasets for reporting.

This integrated dataset powers operational dashboards.

---

# Best Practices

✔ Parse dates during import.

✔ Import only required columns.

✔ Explore large datasets using `nrows`.

✔ Export clean datasets without indexes.

✔ Keep consistent file naming conventions.

---

# Common Mistakes

### Importing Entire Files Unnecessarily

Instead of:

```python id="mistake01"
pd.read_csv(
    "huge.csv"
)
```

Use:

```python id="mistake02"
usecols=
```

or

```python id="mistake03"
nrows=
```

to reduce memory usage.

---

### Forgetting `index=False`

Without:

```python id="mistake04"
index=False
```

an extra index column is written to CSV or Excel files.

---

### Ignoring Date Parsing

Importing dates as strings complicates later analysis.

Whenever possible, use:

```python id="mistake05"
parse_dates=
```

during import.

---

# Key Takeaways

After completing this section, you should understand:

* How to import CSV, Excel, and JSON files.
* How to export cleaned datasets.
* How to read multiple Excel sheets.
* Why parsing dates during import is beneficial.
* How file handling supports data integration workflows.

> **"Effective data analysis begins with efficient data ingestion. The ability to import, integrate, and export information from multiple sources is a fundamental skill in modern analytics."**
# 8. Reading Data from SQL Databases

Most enterprise data is stored in relational databases.

Common databases include:

* MySQL
* PostgreSQL
* SQL Server
* Oracle
* SQLite

Pandas can read SQL tables directly.

---

## Read an Entire Table

```python id="sql01"
import sqlite3

conn = sqlite3.connect(
    "sales.db"
)

df = pd.read_sql(
    "SELECT * FROM Orders",
    conn
)
```

---

## Read Filtered Data

```python id="sql02"
query = """
SELECT
    CustomerID,
    Sales,
    Region
FROM Orders
WHERE Sales > 5000
"""

df = pd.read_sql(
    query,
    conn
)
```

Reading only required records improves performance.

---

## Write Data to SQL

```python id="sql03"
df.to_sql(
    "CleanOrders",
    conn,
    if_exists="replace",
    index=False
)
```

`if_exists` options:

| Option    | Meaning                            |
| --------- | ---------------------------------- |
| `fail`    | Raise an error if the table exists |
| `replace` | Drop and recreate the table        |
| `append`  | Add new rows                       |

---

# 9. Reading Data from APIs

Many companies expose data through REST APIs.

Examples:

* Weather services
* Stock market APIs
* Payment gateways
* CRM platforms
* Social media APIs

---

## Request Data

```python id="api01"
import requests

response = requests.get(
    "https://api.example.com/orders"
)

data = response.json()
```

Convert to DataFrame.

```python id="api02"
df = pd.DataFrame(data)
```

---

## Flatten Nested API Responses

```python id="api03"
from pandas import json_normalize

df = json_normalize(data)
```

---

# 10. Reading Parquet Files

Parquet is a highly efficient columnar storage format.

Advantages:

* Smaller file size
* Faster reading
* Better compression
* Optimized for analytics

Read Parquet.

```python id="parquet01"
df = pd.read_parquet(
    "sales.parquet"
)
```

Write Parquet.

```python id="parquet02"
df.to_parquet(
    "clean_sales.parquet"
)
```

Parquet is widely used in Spark, Hadoop, Snowflake, and cloud data lakes.

---

# 11. Working with Compressed Files

Pandas can read compressed files directly.

Read ZIP.

```python id="zip01"
df = pd.read_csv(
    "sales.zip",
    compression="zip"
)
```

---

Read GZIP.

```python id="zip02"
df = pd.read_csv(
    "sales.csv.gz",
    compression="gzip"
)
```

Compression reduces storage requirements and transfer times.

---

# 12. Batch Processing Multiple Files

Businesses often receive many files every day.

Example directory:

```text id="batch01"
January.csv

February.csv

March.csv
```

Read all CSV files.

```python id="batch02"
import glob

files = glob.glob(
    "data/*.csv"
)

dfs = []

for file in files:

    temp = pd.read_csv(file)

    dfs.append(temp)
```

Combine them.

```python id="batch03"
sales = pd.concat(
    dfs,
    ignore_index=True
)
```

---

# 13. Combining Data from Multiple Sources

Suppose:

* Orders → CSV
* Customers → SQL
* Inventory → Excel

Read each source.

```python id="combine01"
orders = pd.read_csv(
    "orders.csv"
)

customers = pd.read_sql(
    "SELECT * FROM Customers",
    conn
)

inventory = pd.read_excel(
    "inventory.xlsx"
)
```

Merge datasets.

```python id="combine02"
combined = (
    orders
      .merge(
          customers,
          on="Customer ID"
      )
      .merge(
          inventory,
          on="Product ID"
      )
)
```

Integrated datasets provide a complete business view.

---

# 14. Enterprise ETL Pipeline

ETL stands for:

* **Extract**
* **Transform**
* **Load**

Workflow:

```text id="etl01"
CSV

↓

Excel

↓

API

↓

SQL Database

↓

Cleaning

↓

Transformation

↓

Validation

↓

Aggregation

↓

Reporting Database

↓

Dashboard
```

ETL pipelines automate data integration.

---

# 15. File Handling Performance Tips

### Read Only Needed Columns

```python id="perf01"
df = pd.read_csv(
    "sales.csv",
    usecols=[
        "Sales",
        "Region"
    ]
)
```

---

### Specify Data Types

```python id="perf02"
df = pd.read_csv(

    "sales.csv",

    dtype={
        "Region":"category",
        "Quantity":"int16"
    }
)
```

This reduces memory usage during import.

---

### Read Large Files in Chunks

```python id="perf03"
chunks = pd.read_csv(

    "large.csv",

    chunksize=50000
)

for chunk in chunks:

    print(chunk.shape)
```

---

# Business Example

A multinational retailer receives:

* Daily sales from CSV files.
* Customer information from PostgreSQL.
* Inventory reports from Excel.
* Product metadata from an API.
* Historical archives in Parquet format.

Engineers:

* Import each dataset.
* Clean and validate records.
* Merge all sources.
* Store the integrated data in a warehouse.
* Refresh executive dashboards automatically.

---

# Best Practices

✔ Use Parquet for large analytical datasets.

✔ Read only required columns.

✔ Process large datasets in chunks.

✔ Standardize schemas before merging.

✔ Validate imported data before analysis.

---

# Common Mistakes

### Reading Entire Databases

Avoid:

```python id="mistake01"
SELECT *
```

on extremely large production tables unless necessary.

Retrieve only the required columns and rows.

---

### Mixing Data Types

Ensure key columns (such as Customer ID) have matching data types across all sources before merging.

---

### Ignoring File Compression

Compressed formats reduce storage costs and improve transfer efficiency.

---

# Quick Recap

You have now learned how to:

* Read SQL databases.
* Import data from APIs.
* Work with Parquet files.
* Read compressed files.
* Process multiple files automatically.
* Integrate data from multiple sources.
* Understand ETL workflows.
* Optimize file handling performance.

> **"Modern analytics depends on integrating data from diverse systems. Efficient file handling and ETL pipelines transform disconnected sources into unified, analysis-ready datasets."**

# 16. Enterprise Data Integration Workflow

Organizations follow a structured workflow to integrate data from multiple systems.

```text id="workflow01"
CSV Files
      │
      ▼
Excel Reports
      │
      ▼
SQL Database
      │
      ▼
REST APIs
      │
      ▼
Parquet Files
      │
      ▼
Data Cleaning
      │
      ▼
Schema Validation
      │
      ▼
Data Integration
      │
      ▼
Data Warehouse
      │
      ▼
Dashboards / Machine Learning
```

A structured workflow improves data quality, consistency, and scalability.

---

# 17. Building an Automated ETL Pipeline

Create a reusable ETL function.

```python id="pipeline01"
import pandas as pd

def etl_pipeline():

    # Extract
    orders = pd.read_csv(
        "orders.csv"
    )

    customers = pd.read_excel(
        "customers.xlsx"
    )

    # Transform
    orders = (
        orders
        .drop_duplicates()
    )

    orders["Order Date"] = (
        pd.to_datetime(
            orders["Order Date"]
        )
    )

    # Load
    final = (
        orders.merge(
            customers,
            on="Customer ID"
        )
    )

    return final
```

Execute:

```python id="pipeline02"
final_df = etl_pipeline()
```

This reusable pipeline reduces manual work and ensures consistent processing.

---

# 18. File Management Best Practices

When working with multiple files:

### Recommended Folder Structure

```text id="folder01"
project/

│

├── data/

│     ├── raw/

│     ├── cleaned/

│     ├── processed/

│

├── notebooks/

│

├── scripts/

│

├── reports/

│

└── output/
```

Advantages:

* Easier maintenance
* Better collaboration
* Clear separation of raw and processed data
* Reproducible workflows

---

## Naming Conventions

Prefer:

```text id="folder02"
sales_2026_01.csv

sales_2026_02.csv

sales_2026_03.csv
```

Avoid:

```text id="folder03"
file1.csv

newfile.csv

finalfinal.csv
```

Descriptive names improve organization.

---

# 19. Performance Optimization

Large-scale data integration requires efficient processing.

---

## Read Large CSV Files in Chunks

```python id="perf01"
chunks = pd.read_csv(
    "sales.csv",
    chunksize=100000
)

for chunk in chunks:

    # Process chunk
    pass
```

---

## Use Parquet for Analytics

```python id="perf02"
df.to_parquet(
    "sales.parquet"
)
```

Parquet generally provides:

* Faster reads
* Better compression
* Lower storage requirements

---

## Import Only Necessary Data

```python id="perf03"
df = pd.read_csv(

    "sales.csv",

    usecols=[
        "Customer ID",
        "Revenue",
        "Region"
    ]
)
```

Reducing unnecessary columns improves import speed.

---

# 20. Enterprise Case Study

## Scenario

You are a **Senior Data Engineer** at **RetailHub**.

Daily data arrives from:

* Website (CSV)
* ERP System (SQL)
* CRM (Excel)
* Inventory API
* Historical Data Lake (Parquet)

The company wants one integrated dataset every morning before business reports refresh.

---

## Business Questions

### Question 1

Import orders.

```python id="case01"
orders = pd.read_csv(
    "orders.csv"
)
```

---

### Question 2

Import customer information.

```python id="case02"
customers = pd.read_excel(
    "customers.xlsx"
)
```

---

### Question 3

Merge both datasets.

```python id="case03"
combined = (
    orders.merge(
        customers,
        on="Customer ID"
    )
)
```

---

### Question 4

Export cleaned data.

```python id="case04"
combined.to_parquet(
    "integrated.parquet"
)
```

---

### Question 5

Generate the final reporting dataset.

```python id="case05"
report = (
    combined.groupby(
        "Region"
    )["Revenue"]
     .sum()
)
```

---

# 21. Business Insights

After implementing the ETL workflow, the organization observes:

* Automated integration reduces manual effort and processing errors.
* Standardized schemas improve data consistency across systems.
* Parquet storage reduces storage requirements and accelerates analytical queries.
* Batch processing enables daily ingestion of large volumes of operational data.
* Centralized datasets improve reporting accuracy and support enterprise-wide analytics.

---

# 22. Practice Exercises

## Beginner

1. Read a CSV file.
2. Read an Excel worksheet.
3. Read a JSON file.
4. Export a DataFrame to CSV.
5. Export a DataFrame to Excel.

---

## Intermediate

6. Import a SQL table.
7. Read a Parquet file.
8. Process multiple CSV files.
9. Merge datasets from different sources.
10. Export processed data.

---

## Advanced

11. Build a reusable ETL pipeline.
12. Integrate CSV, Excel, SQL, and API data.
13. Optimize imports for large datasets.
14. Design a scalable file management system.
15. Prepare an enterprise reporting dataset.

---

# 23. Interview Questions

## Beginner

1. What is ETL?
2. What is the difference between CSV and Excel?
3. Why is Parquet preferred for analytics?
4. How do you read JSON data in Pandas?
5. Why use `parse_dates`?

---

## Intermediate

6. Explain how to connect Pandas to a SQL database.
7. How do you process multiple files automatically?
8. Why are compressed files useful?
9. How would you integrate API and database data?
10. Explain schema standardization.

---

## Advanced

11. Design a production ETL pipeline.
12. Compare CSV and Parquet for large-scale analytics.
13. How would you process terabytes of daily data?
14. Explain best practices for enterprise data integration.
15. How do you optimize data ingestion for cloud analytics?

---

# 24. Cheat Sheet

| Task              | Syntax             |
| ----------------- | ------------------ |
| Read CSV          | `pd.read_csv()`    |
| Read Excel        | `pd.read_excel()`  |
| Read JSON         | `pd.read_json()`   |
| Read SQL          | `pd.read_sql()`    |
| Write CSV         | `to_csv()`         |
| Write Excel       | `to_excel()`       |
| Write Parquet     | `to_parquet()`     |
| Merge Data        | `merge()`          |
| Concatenate Files | `pd.concat()`      |
| Normalize JSON    | `json_normalize()` |

---

# 25. Mini Project

## Enterprise Multi-Source Data Integration

Using data from multiple sources:

* CSV
* Excel
* SQL
* JSON
* Parquet

Complete the following tasks:

* Import all datasets.
* Standardize column names.
* Validate schemas.
* Clean missing values.
* Merge datasets.
* Export the integrated dataset.
* Generate executive KPIs.
* Build an automated ETL pipeline.
* Write **five executive-level business insights**.
* Recommend **three improvements** for the data integration process.

### Example Business Insights

* Integrating multiple data sources provides a unified view of customer activity.
* Standardized schemas reduce inconsistencies across reporting systems.
* Parquet storage improves query performance for large analytical datasets.
* Automated ETL pipelines reduce manual intervention and improve reliability.
* Centralized reporting enables faster executive decision-making.

---

# 26. Summary

Congratulations! 🎉

Today you mastered **Advanced File Handling & Data Integration**.

You learned how to:

* Read and write CSV, Excel, JSON, SQL, and Parquet files.
* Import data from APIs.
* Process compressed and batch files.
* Build ETL pipelines.
* Integrate multiple data sources.
* Optimize large-scale file handling.

These skills are essential for Data Engineering, Business Intelligence, Analytics Engineering, and enterprise reporting.

---

# 27. What's Next?

In **Day 42**, you'll learn **Advanced Pandas Method Chaining & Pipeline Design**.

Topics include:

* Method chaining fundamentals
* `pipe()` function
* Writing clean and readable pipelines
* Custom transformation functions
* Functional programming with Pandas
* Reusable data workflows
* Debugging chained operations
* Production-ready pipeline architecture
* Enterprise data transformation patterns

Mastering method chaining will help you write cleaner, more maintainable, and production-ready Pandas code.

---

<div align="center">

# 🎉 Day 41 Complete!

You've mastered **Advanced File Handling & Data Integration**, enabling you to build efficient ETL pipelines and integrate data from multiple enterprise systems.

By combining imports, exports, SQL, APIs, Parquet, and automated workflows, you're now equipped to handle real-world data engineering tasks with confidence.

⭐ **Next → Day 42: Advanced Pandas Method Chaining & Pipeline Design** 🔗🐼

</div>
