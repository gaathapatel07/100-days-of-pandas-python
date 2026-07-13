
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
