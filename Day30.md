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

> **"Efficient data analysis begins with importing the right data in the right format while minimizing unnecessary processing."**

