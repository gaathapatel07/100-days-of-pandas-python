
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

