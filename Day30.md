# Day 30 — Advanced Input & Output (I/O), File Formats & Data Serialization

<div align="center">

# 100 Days of Pandas

### Day 30 · Reading, Writing & Managing Data Efficiently

*"Data analysis begins with importing data and ends with delivering clean, actionable information in the right format."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Input%20%26%20Output-blue)
![Day](https://img.shields.io/badge/Day-30-orange)

</div>

---

# Table of Contents

1. Introduction
2. Why I/O Matters
3. Learning Objectives
4. Reading CSV Files
5. Reading Excel Files
6. Reading JSON Files
7. Reading HTML Tables
8. Summary

---

# 1. Introduction

Every analytics project starts with importing data from one or more sources.

Common data sources include:

* CSV files
* Excel workbooks
* JSON APIs
* SQL databases
* HTML tables
* XML files
* Parquet files
* Cloud storage

Pandas provides built-in functions for efficiently reading and writing these formats.

---

# 2. Why I/O Matters

Imagine a retail company.

Customer information is stored in:

* Excel

Sales transactions in:

* CSV

Product information in:

* SQL

Marketing campaign data from:

* JSON APIs

To build a complete dashboard, analysts must import and combine data from all these sources.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Read common file formats.
* Export processed datasets.
* Handle large files efficiently.
* Understand serialization formats.
* Build reusable import pipelines.

---

# 4. Reading CSV Files

CSV (Comma-Separated Values) is the most common file format in analytics.

Read a CSV file.

```python id="csv01"
import pandas as pd

df = pd.read_csv(
    "sales.csv"
)
```

Display the first few rows.

```python id="csv02"
df.head()
```

---

## Select Specific Columns

```python id="csv03"
df = pd.read_csv(
    "sales.csv",
    usecols=[
        "Customer",
        "Sales"
    ]
)
```

This reduces memory usage when working with large datasets.

---

## Specify a Different Delimiter

Some CSV files use semicolons.

```python id="csv04"
df = pd.read_csv(
    "sales.csv",
    sep=";"
)
```

---

## Handle Missing Values While Reading

```python id="csv05"
df = pd.read_csv(
    "sales.csv",
    na_values=[
        "NA",
        "N/A",
        "-"
    ]
)
```

Specified values are automatically interpreted as missing.

---

# 5. Reading Excel Files

Many businesses store reports in Excel.

Read an Excel workbook.

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
    sheet_name="January"
)
```

---

## Read Multiple Sheets

```python id="excel03"
all_sheets = pd.read_excel(
    "sales.xlsx",
    sheet_name=None
)
```

Returns a dictionary where:

* Keys → Sheet names
* Values → DataFrames

---

# 6. Reading JSON Files

JSON is widely used for APIs.

Example JSON

```json id="json01"
{
  "Customer": "Alice",
  "Sales": 5200
}
```

Read JSON.

```python id="json02"
df = pd.read_json(
    "sales.json"
)
```

JSON data from REST APIs can often be passed directly into a DataFrame after parsing.

---

# 7. Reading HTML Tables

Pandas can extract tables directly from web pages.

```python id="html01"
tables = pd.read_html(
    "https://example.com/report.html"
)
```

The result is a list of DataFrames.

Retrieve the first table.

```python id="html02"
df = tables[0]
```

This is useful for:

* Financial reports
* Government statistics
* Sports tables
* Public datasets

---

# Business Example

A finance team receives:

* Daily transactions as CSV.
* Monthly reports in Excel.
* Exchange rates from a JSON API.
* Benchmark tables from a financial website.

Using Pandas, analysts import all datasets into a single reporting workflow before performing analysis.

---

# Best Practices

✔ Read only the columns you need.

✔ Specify data types when possible.

✔ Handle missing values during import.

✔ Verify imported data using `head()` and `info()`.

✔ Keep raw files unchanged and work on copies.

---

# Common Mistakes

### Assuming Every CSV Uses Commas

Many European datasets use:

```text id="mistake01"
;
```

instead of

```text id="mistake02"
,
```

Always inspect the delimiter before importing.

---

### Ignoring Data Types

Large numeric columns imported as strings may slow down analysis.

Check:

```python id="mistake03"
df.dtypes
```

immediately after reading a file.

---

### Reading Entire Files Unnecessarily

If only a few columns are needed:

```python id="mistake04"
usecols=[...]
```

can significantly improve performance.

---

# Key Takeaways

After completing this section, you should understand:

* How to read CSV, Excel, JSON, and HTML data.
* How to import only required columns.
* How to handle missing values during import.
* Why validating imported data is important.
* How efficient imports improve analytical workflows.

# 8. Writing CSV Files

After cleaning and analyzing data, exporting results is a common task.

Save a DataFrame as a CSV file.

```python id="write01"
df.to_csv(
    "clean_sales.csv",
    index=False
)
```

### Why `index=False`?

By default, Pandas writes the DataFrame index to the file.

Output without `index=False`:

|    | Customer | Sales |
| -: | -------- | ----: |
|  0 | Alice    |  5200 |
|  1 | Rahul    |  6100 |

Output with `index=False`:

| Customer | Sales |
| -------- | ----: |
| Alice    |  5200 |
| Rahul    |  6100 |

---

# 9. Writing Excel Files

Export data to Excel.

```python id="excel04"
df.to_excel(
    "sales_report.xlsx",
    index=False
)
```

---

## Writing Multiple Sheets

```python id="excel05"
with pd.ExcelWriter(
    "business_report.xlsx"
) as writer:

    sales_df.to_excel(
        writer,
        sheet_name="Sales",
        index=False
    )

    customer_df.to_excel(
        writer,
        sheet_name="Customers",
        index=False
    )
```

Output workbook:

```
business_report.xlsx

├── Sales

└── Customers
```

---

# 10. Writing JSON Files

Export structured data.

```python id="json03"
df.to_json(
    "sales.json",
    orient="records",
    indent=4
)
```

Example Output

```json id="json04"
[
    {
        "Customer":"Alice",
        "Sales":5200
    },
    {
        "Customer":"Rahul",
        "Sales":6100
    }
]
```

Useful for APIs and web applications.

---

# 11. Working with Parquet

Parquet is a modern columnar storage format widely used in:

* Apache Spark
* Hadoop
* AWS Athena
* Databricks
* Snowflake

Write Parquet.

```python id="parquet01"
df.to_parquet(
    "sales.parquet"
)
```

Read Parquet.

```python id="parquet02"
pd.read_parquet(
    "sales.parquet"
)
```

Advantages:

* Smaller files
* Faster reading
* Better compression
* Efficient analytical queries

---

# 12. Reading Large Files Using Chunks

Large datasets may not fit into memory.

Read them in smaller chunks.

```python id="chunk01"
chunks = pd.read_csv(
    "sales.csv",
    chunksize=10000
)

for chunk in chunks:

    print(
        chunk.shape
    )
```

Each chunk contains **10,000 rows**.

Useful for:

* Big data preprocessing
* Incremental ETL
* Memory-efficient analysis

---

# 13. Compression

Compress output files while exporting.

```python id="compress01"
df.to_csv(
    "sales.csv.gz",
    compression="gzip",
    index=False
)
```

Supported compression formats:

| Format | Compression |
| ------ | ----------- |
| `gzip` | `.gz`       |
| `zip`  | `.zip`      |
| `bz2`  | `.bz2`      |
| `xz`   | `.xz`       |

Compressed files save storage and transfer faster.

---

# 14. Clipboard Operations

Copy a DataFrame directly to the system clipboard.

```python id="clip01"
df.to_clipboard(
    index=False
)
```

Paste directly into:

* Excel
* Google Sheets
* Word
* Email

Read data from the clipboard.

```python id="clip02"
pd.read_clipboard()
```

Useful for quick exploratory analysis.

---

# 15. Pickle Serialization

Pickle preserves Python objects.

Save:

```python id="pickle01"
df.to_pickle(
    "sales.pkl"
)
```

Load:

```python id="pickle02"
pd.read_pickle(
    "sales.pkl"
)
```

Advantages:

* Fast
* Preserves data types
* Suitable for Python workflows

Limitations:

* Python-specific
* Not recommended for sharing with non-Python applications

---

# 16. Working with SQL Databases

Pandas integrates seamlessly with relational databases.

Read a SQL table.

```python id="sql01"
import sqlite3

connection = sqlite3.connect(
    "sales.db"
)

df = pd.read_sql(
    "SELECT * FROM sales",
    connection
)
```

---

## Write Data to SQL

```python id="sql02"
df.to_sql(
    "sales_summary",
    connection,
    if_exists="replace",
    index=False
)
```

Options for `if_exists`:

| Option    | Meaning                            |
| --------- | ---------------------------------- |
| `fail`    | Raise an error if the table exists |
| `replace` | Drop and recreate the table        |
| `append`  | Add new rows to the existing table |

---

# Business Example

A multinational retailer processes data from several systems:

* CSV transaction files from stores.
* Excel reports from finance.
* JSON responses from an online API.
* SQL databases containing customer records.

The cleaned data is exported as:

* Excel reports for managers.
* Parquet files for the data lake.
* SQL tables for dashboards.
* Compressed CSV files for archival storage.

---

# Best Practices

✔ Export only cleaned and validated data.

✔ Use Parquet for analytical workloads.

✔ Use chunking for very large datasets.

✔ Compress archival files.

✔ Choose serialization formats appropriate for downstream systems.

---

# Common Mistakes

### Exporting the Index Unintentionally

Unless the index has business meaning:

```python id="mistake01"
index=False
```

should usually be specified.

---

### Reading Huge Files at Once

Instead of:

```python id="mistake02"
pd.read_csv(
    "large.csv"
)
```

Prefer:

```python id="mistake03"
chunksize=10000
```

for memory efficiency.

---

### Using Pickle for Cross-Platform Data Sharing

Pickle is excellent within Python ecosystems but CSV, JSON, or Parquet are generally better choices for sharing data across different technologies.

---

# Quick Recap

You have now learned how to:

* Export CSV, Excel, JSON, and Parquet files.
* Handle very large datasets using chunking.
* Compress exported files.
* Copy data to and from the clipboard.
* Serialize DataFrames with Pickle.
* Read from and write to SQL databases.

> **"Choosing the right file format improves performance, portability, storage efficiency, and collaboration across modern data ecosystems."**

# 17. Enterprise ETL Pipeline

ETL stands for:

* **Extract** → Read data from multiple sources.
* **Transform** → Clean, validate, and process data.
* **Load** → Store the processed data for reporting or analysis.

A typical ETL workflow looks like this:

```text id="etl01"
CSV Files
Excel Reports
JSON APIs
SQL Databases
Cloud Storage
      │
      ▼
Extract
      │
      ▼
Validate Data
      │
      ▼
Clean Missing Values
      │
      ▼
Standardize Formats
      │
      ▼
Merge Multiple Sources
      │
      ▼
Feature Engineering
      │
      ▼
Generate KPIs
      │
      ▼
Export Reports
```

ETL pipelines form the backbone of business intelligence systems.

---

# 18. Building an End-to-End Data Pipeline

Instead of manually cleaning every dataset, create a reusable workflow.

```python id="pipeline01"
def prepare_sales_data(file_path):

    df = pd.read_csv(file_path)

    df = (
        df
        .drop_duplicates()
        .dropna(subset=["Sales"])
        .assign(
            City=lambda x:
            x["City"]
              .str.strip()
              .str.title()
        )
    )

    return df
```

Export the cleaned dataset.

```python id="pipeline02"
clean_df = prepare_sales_data(
    "sales.csv"
)

clean_df.to_csv(
    "clean_sales.csv",
    index=False
)
```

This makes the process repeatable and easier to maintain.

---

# 19. Automating Multi-File Processing

Suppose a company receives monthly sales files.

```text id="files01"
January.csv

February.csv

March.csv

April.csv
```

Process all files automatically.

```python id="files02"
from pathlib import Path

folder = Path("monthly_sales")

all_files = folder.glob("*.csv")

dataframes = []

for file in all_files:

    dataframes.append(
        pd.read_csv(file)
    )

sales_df = pd.concat(
    dataframes,
    ignore_index=True
)
```

This approach scales well when dozens or hundreds of files must be processed.

---

# 20. Performance Optimization

Large datasets require efficient file handling.

### Read Only Required Columns

```python id="perf01"
df = pd.read_csv(
    "sales.csv",
    usecols=[
        "Customer",
        "Sales",
        "City"
    ]
)
```

---

### Specify Data Types

```python id="perf02"
df = pd.read_csv(
    "sales.csv",
    dtype={
        "Customer ID": "int32",
        "Region": "category"
    }
)
```

Explicit data types reduce memory usage and improve loading speed.

---

### Parse Dates During Import

Instead of converting dates later:

```python id="perf03"
df = pd.read_csv(
    "sales.csv",
    parse_dates=[
        "Order Date"
    ]
)
```

This avoids an additional transformation step.

---

### Memory Usage Check

```python id="perf04"
df.memory_usage(
    deep=True
)
```

Monitor memory regularly when working with large datasets.

---

# 21. Enterprise Case Study

## Scenario

You are working as a **Senior Data Engineer** at **RetailHub**.

Every morning, the company receives:

* Sales data in CSV format.
* Customer information from SQL.
* Marketing data from JSON APIs.
* Inventory reports in Excel.

Your responsibility is to create a single analytics-ready dataset for Power BI dashboards.

---

## Business Questions

### Question 1

Read sales data.

```python id="case01"
sales = pd.read_csv(
    "sales.csv"
)
```

---

### Question 2

Read customer data.

```python id="case02"
customers = pd.read_excel(
    "customers.xlsx"
)
```

---

### Question 3

Merge datasets.

```python id="case03"
final_df = pd.merge(
    sales,
    customers,
    on="Customer ID",
    how="left"
)
```

---

### Question 4

Export the processed data.

```python id="case04"
final_df.to_parquet(
    "dashboard.parquet"
)
```

---

### Question 5

Store the results in SQL.

```python id="case05"
final_df.to_sql(
    "sales_dashboard",
    connection,
    if_exists="replace",
    index=False
)
```

---

# 22. Business Insights

After automating the ETL process, the organization observes:

* Manual report preparation time is significantly reduced.
* Consistent import pipelines minimize human errors.
* Parquet storage reduces disk usage and improves query speed.
* SQL integration enables real-time dashboard updates.
* Standardized exports improve collaboration across departments.

---

# 23. Practice Exercises

## Beginner

1. Read a CSV file.
2. Read an Excel workbook.
3. Read a JSON file.
4. Export a DataFrame as CSV.
5. Export a DataFrame as Excel.

---

## Intermediate

6. Read only selected columns.
7. Import large files using chunks.
8. Write multiple Excel sheets.
9. Export compressed CSV files.
10. Read and write SQL tables.

---

## Advanced

11. Build an automated ETL pipeline.
12. Process multiple monthly files.
13. Compare CSV and Parquet performance.
14. Create a reusable import/export workflow.
15. Design a scalable enterprise data ingestion process.

---

# 24. Interview Questions

## Beginner

1. What is ETL?
2. Difference between CSV and Excel?
3. How do you read JSON using Pandas?
4. What does `to_csv()` do?
5. Why use `index=False`?

---

## Intermediate

6. Why use Parquet instead of CSV?
7. How do you read large files efficiently?
8. What is Pickle serialization?
9. How do you write data to SQL?
10. What is `chunksize`?

---

## Advanced

11. Design an enterprise ETL pipeline using Pandas.
12. Compare CSV, JSON, Excel, and Parquet.
13. How would you process a dataset larger than memory?
14. How do you optimize file I/O for millions of records?
15. Explain a complete data ingestion workflow from raw files to dashboards.

---

# 25. Cheat Sheet

| Task           | Syntax                          |
| -------------- | ------------------------------- |
| Read CSV       | `pd.read_csv()`                 |
| Write CSV      | `to_csv()`                      |
| Read Excel     | `pd.read_excel()`               |
| Write Excel    | `to_excel()`                    |
| Read JSON      | `pd.read_json()`                |
| Write JSON     | `to_json()`                     |
| Read HTML      | `pd.read_html()`                |
| Read Parquet   | `pd.read_parquet()`             |
| Write Parquet  | `to_parquet()`                  |
| Read SQL       | `pd.read_sql()`                 |
| Write SQL      | `to_sql()`                      |
| Compression    | `compression="gzip"`            |
| Read in Chunks | `chunksize=`                    |
| Pickle         | `to_pickle()` / `read_pickle()` |

---

# 26. Mini Project

## Enterprise Sales ETL System

Using any retail, banking, healthcare, logistics, or HR dataset:

Complete the following tasks:

* Import data from CSV, Excel, and JSON.
* Merge datasets into a unified DataFrame.
* Clean missing values and duplicates.
* Standardize text and date formats.
* Generate business KPIs.
* Export results as CSV, Excel, Parquet, and SQL.
* Compress archival files.
* Write **five executive-level business insights**.
* Recommend **three improvements** for building scalable ETL pipelines.

### Example Business Insights

* Automated ETL pipelines reduced manual processing time.
* Reading only required columns lowered memory consumption.
* Parquet storage improved data retrieval speed for analytical workloads.
* SQL exports enabled seamless integration with dashboarding tools.
* Standardized data formats improved consistency across business units.

---

# 27. Summary

Congratulations! 🎉

Today you mastered **Advanced Input & Output (I/O), File Formats & Data Serialization** in Pandas.

You learned how to:

* Read and write CSV, Excel, JSON, HTML, and Parquet files.
* Work with SQL databases.
* Handle very large datasets using chunking.
* Compress exported data.
* Build reusable ETL pipelines.
* Optimize file loading and exporting.

These skills are fundamental for modern data engineering, business intelligence, cloud analytics, and enterprise reporting.

---

# 28. 🎉 Milestone Achieved — 30 Days of Pandas

You have successfully completed **30 Days** of your **100 Days of Pandas** journey.

So far, you've mastered:

* ✅ Pandas Fundamentals
* ✅ DataFrames & Series
* ✅ Data Selection & Filtering
* ✅ Cleaning & Missing Data
* ✅ String Operations & Regex
* ✅ Date & Time Analysis
* ✅ Categorical Data
* ✅ Apply, Map & Vectorization
* ✅ GroupBy & Pivot Tables
* ✅ Merge, Join & Concat
* ✅ MultiIndex & Hierarchical Data
* ✅ Professional ETL & File Handling

You now have a strong foundation that mirrors the day-to-day work of Data Analysts and Data Engineers.

---

# 29. What's Next?

In **Day 31**, you'll begin **Advanced Time Series Analysis in Pandas**.

Topics include:

* Time Series Fundamentals
* DateTime Index
* Resampling
* Frequency Conversion
* Rolling & Expanding Windows
* Lag & Lead Features
* Moving Averages
* Time-Based Shifting
* Trend & Seasonality Analysis
* Preparing Time Series for Forecasting

These skills are essential for finance, forecasting, demand planning, IoT analytics, and operational reporting.

---

<div align="center">

# Day 30 Complete!

You've built a strong professional foundation in Pandas—from importing raw data to creating clean, analytics-ready datasets and scalable ETL workflows.

The next phase will focus on **time-series analytics**, one of the most valuable skills in data science and business forecasting.


</div>

> **"Efficient data analysis begins with importing the right data in the right format while minimizing unnecessary processing."**

