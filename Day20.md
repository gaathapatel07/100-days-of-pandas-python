# Day 20 — Advanced File Handling & Data Input/Output in Pandas

<div align="center">

# 100 Days of Pandas

### Day 20 · Reading, Writing & Managing Data Efficiently

*"Before data can be analyzed, it must first be collected, imported, validated, and stored efficiently."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-File%20Handling-blue)
![Day](https://img.shields.io/badge/Day-20-orange)

</div>

---

# Table of Contents

1. Introduction
2. Why File Handling Matters
3. Learning Objectives
4. Reading CSV Files
5. Reading Excel Files
6. Reading JSON Files
7. Reading HTML Tables
8. Summary

---

# 1. Introduction

Modern organizations collect enormous volumes of data from multiple sources every day.

A Data Analyst may receive:

* CSV files from sales systems
* Excel reports from finance teams
* JSON responses from APIs
* HTML tables from websites
* XML files from enterprise software
* Parquet datasets from cloud storage
* SQL query results from databases

Before any cleaning, visualization, or machine learning can begin, the data must first be imported correctly.

Pandas provides a comprehensive collection of functions for reading and writing data across many popular formats.

---

# 2. Why File Handling Matters

Imagine you're working as a Data Analyst for a multinational retailer.

Every morning you receive:

* Daily sales in CSV format
* Inventory reports in Excel
* Customer feedback from an API (JSON)
* Currency exchange rates from a website
* Historical transactions stored as Parquet files

Your first responsibility is to consolidate these datasets into a unified DataFrame for analysis.

Efficient file handling reduces manual effort and improves the reliability of analytical workflows.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Import data from multiple file formats.
* Export processed datasets.
* Read compressed files.
* Process large datasets efficiently.
* Build reusable ETL workflows.
* Optimize file input/output performance.

---

# 4. Reading CSV Files

CSV (Comma-Separated Values) is one of the most common file formats in data analysis.

Example:

```python id="csv01"
import pandas as pd

df = pd.read_csv(
    "sales.csv"
)
```

Display the first five records.

```python id="csv02"
df.head()
```

---

## Reading CSV with a Custom Separator

Some files use semicolons instead of commas.

```python id="csv03"
df = pd.read_csv(
    "sales.csv",
    sep=";"
)
```

---

## Reading Selected Columns

Instead of loading the entire dataset:

```python id="csv04"
df = pd.read_csv(
    "sales.csv",
    usecols=[
        "Product",
        "Sales",
        "Profit"
    ]
)
```

This reduces memory usage and speeds up loading.

---

## Limiting Rows

Read only the first 100 rows.

```python id="csv05"
df = pd.read_csv(
    "sales.csv",
    nrows=100
)
```

Useful for exploring very large datasets.

---

## Handling Missing Values During Import

Treat specific values as missing.

```python id="csv06"
df = pd.read_csv(
    "sales.csv",
    na_values=[
        "NA",
        "Unknown",
        "-"
    ]
)
```

---

# 5. Reading Excel Files

Many business reports are shared as Excel workbooks.

Read the first worksheet.

```python id="excel01"
df = pd.read_excel(
    "sales.xlsx"
)
```

---

## Reading a Specific Sheet

```python id="excel02"
df = pd.read_excel(
    "sales.xlsx",
    sheet_name="January"
)
```

---

## Reading Multiple Sheets

```python id="excel03"
sheets = pd.read_excel(
    "sales.xlsx",
    sheet_name=None
)
```

The result is a dictionary where:

* Key → Sheet name
* Value → DataFrame

---

# 6. Reading JSON Files

Many web APIs return JSON data.

Example:

```python id="json01"
df = pd.read_json(
    "customers.json"
)
```

JSON is commonly used for:

* REST APIs
* Mobile applications
* Cloud services
* Configuration files

---

# 7. Reading HTML Tables

Pandas can extract tables directly from web pages.

```python id="html01"
tables = pd.read_html(
    "https://example.com"
)
```

The result is a list of DataFrames.

Retrieve the first table.

```python id="html02"
df = tables[0]
```

This technique is widely used in web scraping and automated reporting.

---

# Business Example

A financial analyst prepares a daily market report.

The workflow includes:

* Reading stock prices from CSV files.
* Importing financial statements from Excel.
* Collecting exchange rates through JSON APIs.
* Extracting economic indicators from HTML tables.

All of these datasets are combined before generating business reports and dashboards.

---

# Best Practices

✔ Keep raw data separate from processed data.

✔ Load only the required columns.

✔ Specify data types whenever possible.

✔ Validate imported datasets before analysis.

✔ Organize datasets using clear folder structures.

Example:

```text
Project/
│
├── data/
│   ├── raw/
│   ├── processed/
│   └── exports/
│
├── notebooks/
│
├── scripts/
│
└── reports/
```

---

# Common Mistakes

### Loading Entire Large Files

Avoid reading unnecessary columns.

Instead of:

```python id="mistake01"
pd.read_csv("large_file.csv")
```

Prefer:

```python id="mistake02"
pd.read_csv(
    "large_file.csv",
    usecols=[
        "Sales",
        "Profit"
    ]
)
```

---

### Assuming Every CSV Uses Commas

Always inspect the file.

Some datasets use:

* Semicolons (`;`)
* Tabs (`\t`)
* Pipes (`|`)

Specify the correct separator when importing.

---

# Key Takeaways

After completing this section, you should understand:

* How to import CSV, Excel, JSON, and HTML data.
* How to optimize file loading.
* Why selecting columns improves performance.
* How business analysts consolidate multiple data sources.
* Why proper file organization improves project maintainability.

> **"Successful data analysis begins long before visualization—it starts with importing the right data, in the right format, as efficiently as possible."**

# 8. Writing CSV Files

After cleaning and analyzing data, the next step is often exporting the results.

Save a DataFrame as a CSV file.

```python id="writecsv01"
df.to_csv(
    "processed_sales.csv",
    index=False
)
```

### Why `index=False`?

By default, Pandas writes the DataFrame index as an additional column.

Using `index=False` prevents unnecessary index columns from appearing in the exported file.

---

## Exporting Selected Columns

```python id="writecsv02"
df.to_csv(
    "sales_summary.csv",
    columns=[
        "Product",
        "Sales",
        "Profit"
    ],
    index=False
)
```

Only the specified columns are exported.

---

# 9. Writing Excel Files

Business reports are commonly shared as Excel workbooks.

```python id="excelwrite01"
df.to_excel(
    "sales_report.xlsx",
    index=False
)
```

---

## Writing Multiple Sheets

```python id="excelwrite02"
with pd.ExcelWriter(
    "business_report.xlsx"
) as writer:

    sales_df.to_excel(
        writer,
        sheet_name="Sales",
        index=False
    )

    profit_df.to_excel(
        writer,
        sheet_name="Profit",
        index=False
    )

    customer_df.to_excel(
        writer,
        sheet_name="Customers",
        index=False
    )
```

Multiple worksheets improve report organization.

---

# 10. Writing JSON Files

Export structured data for APIs or web applications.

```python id="jsonwrite01"
df.to_json(
    "customers.json",
    orient="records",
    indent=4
)
```

The `records` orientation creates one JSON object per row, which is widely used in REST APIs.

---

# 11. Reading XML Files

Many enterprise systems exchange information using XML.

Read an XML file.

```python id="xml01"
df = pd.read_xml(
    "employees.xml"
)
```

Typical XML use cases include:

* ERP systems
* Banking software
* Healthcare records
* Government data

---

# 12. Reading Parquet Files

Parquet is a columnar storage format designed for analytics.

```python id="parquet01"
df = pd.read_parquet(
    "sales.parquet"
)
```

Advantages:

* Faster loading.
* Better compression.
* Reduced storage requirements.
* Excellent performance on large datasets.

Parquet is widely used with:

* Apache Spark
* Hadoop
* Snowflake
* Databricks
* AWS Athena

---

# 13. Writing Parquet Files

```python id="parquet02"
df.to_parquet(
    "processed_sales.parquet"
)
```

Parquet is often preferred over CSV for large analytical datasets.

---

# 14. Reading Compressed Files

Pandas can directly read compressed datasets.

ZIP example:

```python id="zip01"
df = pd.read_csv(
    "sales.zip",
    compression="zip"
)
```

GZIP example:

```python id="gzip01"
df = pd.read_csv(
    "sales.csv.gz",
    compression="gzip"
)
```

Compressed files reduce storage space and network transfer time.

---

# 15. Processing Large Files Using Chunks

Large datasets may exceed available memory.

Instead of loading everything at once, process the data in smaller chunks.

```python id="chunk01"
chunks = pd.read_csv(
    "large_sales.csv",
    chunksize=50000
)
```

Each iteration returns a DataFrame containing 50,000 rows.

```python id="chunk02"
for chunk in chunks:
    print(chunk.shape)
```

Chunk processing enables efficient analysis of very large files.

---

# 16. Reading Multiple Files Automatically

Suppose a folder contains monthly sales reports.

```
sales/
├── jan.csv
├── feb.csv
├── mar.csv
├── apr.csv
```

Load every CSV file.

```python id="multi01"
from glob import glob

files = glob("sales/*.csv")

dfs = [
    pd.read_csv(file)
    for file in files
]

combined = pd.concat(
    dfs,
    ignore_index=True
)
```

This creates one consolidated DataFrame.

---

# Business Example

An international retailer receives monthly sales files from every regional office.

Instead of importing each file manually, analysts automate the process:

* Read every CSV file.
* Merge them into one dataset.
* Remove duplicates.
* Validate missing values.
* Export a cleaned Parquet file for Power BI.

This automation saves hours of manual work every month.

---

# Best Practices

✔ Export reports without indexes.

✔ Prefer Parquet for large analytical datasets.

✔ Process large files using chunks.

✔ Store raw and processed data separately.

✔ Automate repetitive imports whenever possible.

✔ Use meaningful file names with dates or versions.

Example:

```
exports/
│
├── sales_report_2026_07.csv
├── customer_summary.xlsx
├── inventory_snapshot.parquet
└── profit_dashboard.json
```

---

# Common Mistakes

### Exporting the DataFrame Index

Incorrect:

```python id="mistake03"
df.to_csv("sales.csv")
```

Output contains an unnecessary index column.

Correct:

```python id="mistake04"
df.to_csv(
    "sales.csv",
    index=False
)
```

---

### Loading Large Files into Memory

Avoid:

```python id="mistake05"
pd.read_csv(
    "100GB_dataset.csv"
)
```

Instead:

```python id="mistake06"
pd.read_csv(
    "100GB_dataset.csv",
    chunksize=100000
)
```

This significantly reduces memory consumption.

---

# Quick Recap

You have now learned how to:

* Export CSV, Excel, JSON, and Parquet files.
* Read XML and compressed datasets.
* Process large datasets in chunks.
* Combine multiple files automatically.
* Build scalable file-handling workflows.

> **"Efficient file handling transforms raw files into reliable analytical datasets while reducing manual effort, improving performance, and supporting scalable data pipelines."**
