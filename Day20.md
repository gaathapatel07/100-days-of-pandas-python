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

# 17. Real-World Business Case Study

## Scenario

You are working as a **Senior Data Analyst** at **RetailHub**, a global e-commerce company.

Every day, the analytics team receives data from multiple departments:

* Sales transactions in CSV format
* Inventory reports in Excel
* Customer information from a REST API (JSON)
* Product catalog in XML
* Historical archives stored as Parquet

Before creating dashboards in Power BI and running predictive models, all these files must be imported, validated, cleaned, merged, and stored in a consistent format.

Your task is to design an automated ETL (Extract, Transform, Load) workflow using Pandas.

---

# Business Questions

### Question 1

Load sales data from CSV.

```python id="case_io01"
sales = pd.read_csv(
    "sales.csv"
)
```

---

### Question 2

Load inventory data from Excel.

```python id="case_io02"
inventory = pd.read_excel(
    "inventory.xlsx"
)
```

---

### Question 3

Import customer data from JSON.

```python id="case_io03"
customers = pd.read_json(
    "customers.json"
)
```

---

### Question 4

Combine monthly sales reports.

```python id="case_io04"
from glob import glob

files = glob(
    "monthly_reports/*.csv"
)

monthly_data = pd.concat(
    [pd.read_csv(file) for file in files],
    ignore_index=True
)
```

---

### Question 5

Export the cleaned dataset as a Parquet file.

```python id="case_io05"
monthly_data.to_parquet(
    "clean_sales.parquet"
)
```

---

# 18. Understanding the ETL Process

ETL stands for:

* **Extract** – Collect data from multiple sources.
* **Transform** – Clean, validate, reshape, and enrich the data.
* **Load** – Store the processed data for reporting or analysis.

A typical ETL pipeline looks like this:

```text
             External Sources
                    │
 ┌──────────┬─────────────┬────────────┬───────────┐
 │          │             │            │
 CSV      Excel         JSON         Parquet
 │          │             │            │
 └──────────┴─────────────┴────────────┘
                    │
                    ▼
           Extract into Pandas
                    │
                    ▼
      Clean, Validate & Transform
                    │
                    ▼
          Merge & Enrich Data
                    │
                    ▼
       Export to CSV / Parquet
                    │
                    ▼
   Power BI • Tableau • ML Models • SQL
```

ETL pipelines are the foundation of modern analytics systems.

---

# 19. Comparing File Formats

Choosing the right file format improves performance, storage efficiency, and interoperability.

| Format  | Advantages                      | Limitations                     | Best Use Cases               |
| ------- | ------------------------------- | ------------------------------- | ---------------------------- |
| CSV     | Simple, widely supported        | Larger file size, no formatting | Data exchange, quick imports |
| Excel   | Multiple sheets, formatting     | Slower for large datasets       | Business reports             |
| JSON    | Nested structures, API-friendly | Larger than Parquet             | Web applications, REST APIs  |
| XML     | Structured and standardized     | Verbose and slower              | Enterprise systems           |
| Parquet | Highly compressed, fast reads   | Less human-readable             | Big data, cloud analytics    |

---

# 20. Performance Tips

When working with large datasets:

### Read Only Necessary Columns

```python id="perf_io01"
df = pd.read_csv(
    "sales.csv",
    usecols=[
        "Product",
        "Sales",
        "Profit"
    ]
)
```

---

### Specify Data Types

```python id="perf_io02"
df = pd.read_csv(
    "sales.csv",
    dtype={
        "Product": "string",
        "Quantity": "int32",
        "Sales": "float32"
    }
)
```

This reduces memory usage and speeds up processing.

---

### Process Large Files in Chunks

```python id="perf_io03"
for chunk in pd.read_csv(
    "sales.csv",
    chunksize=100000
):
    process(chunk)
```

Chunking prevents memory overflow and supports scalable workflows.

---

# 21. Business Insights

After automating the ETL workflow, you discover:

* Import time is significantly reduced by loading only required columns.
* Parquet files occupy much less storage than CSV files.
* Automating monthly imports eliminates repetitive manual work.
* Standardized file structures reduce reporting errors.
* A centralized ETL pipeline improves consistency across departments.

These improvements increase productivity and support reliable business reporting.

---

# 22. Practice Exercises

## Beginner

1. Read a CSV file.
2. Import an Excel worksheet.
3. Load a JSON file.
4. Export a DataFrame to CSV.
5. Save a DataFrame as an Excel file.

---

## Intermediate

6. Read an XML file.
7. Import a Parquet dataset.
8. Process a large CSV using chunks.
9. Read multiple CSV files from a folder.
10. Export a cleaned dataset to Parquet.

---

## Advanced

11. Build an ETL workflow combining multiple file formats.
12. Compare CSV and Parquet file sizes.
13. Automate monthly report generation.
14. Optimize memory usage during import.
15. Write five recommendations to improve enterprise data management.

---

# 23. Interview Questions

## Beginner

1. What is ETL?
2. What is the purpose of `read_csv()`?
3. Why use `index=False` when exporting?
4. What is the difference between CSV and Excel?
5. Why is JSON commonly used with APIs?

---

## Intermediate

6. Why is Parquet preferred for analytical workloads?
7. What are the advantages of chunk processing?
8. How do you read multiple files automatically?
9. Why should data types be specified during import?
10. What are the benefits of compressed files?

---

## Advanced

11. Design an ETL pipeline using Pandas.
12. Compare CSV, JSON, Excel, and Parquet.
13. How would you process a 200 GB dataset?
14. How can ETL workflows improve business reporting?
15. What file-handling strategies improve analytics performance?

---

# 24. Cheat Sheet

| Task                | Syntax                |
| ------------------- | --------------------- |
| Read CSV            | `pd.read_csv()`       |
| Write CSV           | `df.to_csv()`         |
| Read Excel          | `pd.read_excel()`     |
| Write Excel         | `df.to_excel()`       |
| Read JSON           | `pd.read_json()`      |
| Write JSON          | `df.to_json()`        |
| Read XML            | `pd.read_xml()`       |
| Read Parquet        | `pd.read_parquet()`   |
| Write Parquet       | `df.to_parquet()`     |
| Read ZIP            | `compression="zip"`   |
| Read GZIP           | `compression="gzip"`  |
| Chunk Processing    | `chunksize=`          |
| Read Multiple Files | `glob()` + `concat()` |

---

# 25. Mini Project

## Automated ETL Pipeline for Retail Analytics

Using any retail, finance, healthcare, HR, or logistics dataset:

Complete the following tasks:

* Import CSV, Excel, and JSON datasets.
* Validate missing values and data types.
* Merge datasets into a unified DataFrame.
* Process large files using chunking where appropriate.
* Export the cleaned data to both CSV and Parquet.
* Organize files into `raw`, `processed`, and `exports` folders.
* Generate a summary report containing record counts, missing values, and key statistics.
* Write **five executive-level business insights**.
* Recommend **three improvements** to streamline the ETL process.

### Example Business Insights

* Automating file imports reduced manual processing time by over 70%.
* Parquet storage significantly reduced disk usage while improving read performance.
* Consistent folder organization simplified collaboration across analytics teams.
* Chunk processing enabled efficient handling of large datasets without memory issues.
* Standardized ETL workflows improved the accuracy and reliability of executive reports.

---

# 26. Summary

Congratulations! 🎉

Today you mastered **Advanced File Handling & Data Input/Output** in Pandas.

You learned how to:

* Import data from CSV, Excel, JSON, XML, HTML, and Parquet files.
* Export datasets to multiple formats.
* Read compressed files.
* Process large datasets using chunking.
* Automate file imports.
* Build efficient ETL workflows.

These skills are essential for modern data analytics, business intelligence, data engineering, and machine learning pipelines.

---

# 27. What's Next?

In **Day 21**, you'll learn **Missing Data Handling & Advanced Data Cleaning**.

Topics include:

* Detecting missing values
* `fillna()`
* `dropna()`
* Forward fill (`ffill`)
* Backward fill (`bfill`)
* Interpolation
* Removing duplicates
* Outlier detection
* Data validation techniques

These techniques are fundamental for producing clean, reliable datasets that support accurate analysis and predictive modeling.

---

<div align="center">

# Day 20 Complete!

You've mastered importing, exporting, and managing data across multiple file formats while building scalable ETL workflows.

These skills form the backbone of professional data analytics, enabling you to transform raw data into reliable, analysis-ready datasets.

 **Next → Day 21: Missing Data Handling & Advanced Data Cleaning** 

</div>
