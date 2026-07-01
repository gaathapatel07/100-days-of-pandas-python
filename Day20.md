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

