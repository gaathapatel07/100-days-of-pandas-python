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

> **"Efficient data analysis begins with importing the right data in the right format while minimizing unnecessary processing."**

