# Day 19 — Data Reshaping & Tidy Data in Pandas

<div align="center">

# 100 Days of Pandas

### Day 19 · Transforming Data into Analysis-Ready Formats

*"The way data is organized determines how easily it can be analyzed."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Data%20Reshaping-blue)
![Day](https://img.shields.io/badge/Day-19-orange)

</div>

---

# Table of Contents

1. Introduction
2. Why Data Reshaping Matters
3. Learning Objectives
4. Understanding Wide & Long Data
5. What is Tidy Data?
6. Using `melt()`
7. Customizing Melt
8. Summary

---

# 1. Introduction

Real-world datasets are rarely stored in the format required for analysis.

A dataset received from a client may contain:

* Monthly sales spread across multiple columns.
* Survey responses stored in wide tables.
* Financial reports with separate columns for every quarter.
* Healthcare records organized by visit dates.
* Excel sheets designed for readability rather than analysis.

Before building dashboards, statistical models, or machine learning pipelines, analysts often need to **reshape** the data.

Pandas provides powerful reshaping tools that convert datasets into formats better suited for analysis.

---

# 2. Why Data Reshaping Matters

Suppose monthly sales are stored like this:

| Product |  Jan |  Feb |  Mar |
| ------- | ---: | ---: | ---: |
| Laptop  | 5200 | 6100 | 5900 |
| Phone   | 4800 | 5300 | 5600 |

Although easy for humans to read, this structure is difficult to analyze.

Many analytical libraries prefer:

| Product | Month | Sales |
| ------- | ----- | ----: |
| Laptop  | Jan   |  5200 |
| Laptop  | Feb   |  6100 |
| Laptop  | Mar   |  5900 |
| Phone   | Jan   |  4800 |
| Phone   | Feb   |  5300 |
| Phone   | Mar   |  5600 |

This structure is easier to filter, group, visualize, and model.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Understand wide and long data formats.
* Convert wide data into long format.
* Apply the `melt()` function.
* Customize reshaped data.
* Prepare datasets for visualization and machine learning.

---

# 4. Understanding Wide & Long Data

## Wide Format

Each variable occupies a separate column.

Example:

| Student | Math | Science | English |
| ------- | ---: | ------: | ------: |
| Alice   |   90 |      88 |      91 |
| Rahul   |   82 |      85 |      80 |

Advantages:

* Easy to read.
* Suitable for reports.
* Common in Excel.

Disadvantages:

* Harder to aggregate.
* Difficult to visualize.
* Less flexible for statistical analysis.

---

## Long Format

Each observation occupies a separate row.

Example:

| Student | Subject | Marks |
| ------- | ------- | ----: |
| Alice   | Math    |    90 |
| Alice   | Science |    88 |
| Alice   | English |    91 |
| Rahul   | Math    |    82 |
| Rahul   | Science |    85 |
| Rahul   | English |    80 |

Advantages:

* Easier to filter.
* Better for GroupBy.
* Works naturally with visualization libraries.
* Preferred for machine learning workflows.

---

# 5. What is Tidy Data?

The concept of **Tidy Data** was introduced to make datasets easier to analyze.

A tidy dataset follows three principles:

### Rule 1

Each variable forms its own column.

### Rule 2

Each observation forms its own row.

### Rule 3

Each value occupies one cell.

Following these principles improves consistency and simplifies analysis.

---

# 6. Using `melt()`

The `melt()` function converts **wide-format** data into **long-format** data.

Suppose we have:

| Product |  Jan |  Feb |  Mar |
| ------- | ---: | ---: | ---: |
| Laptop  | 5200 | 6100 | 5900 |
| Phone   | 4800 | 5300 | 5600 |

Convert the data.

```python id="melt01"
pd.melt(
    df,
    id_vars="Product"
)
```

Output:

| Product | Variable | Value |
| ------- | -------- | ----: |
| Laptop  | Jan      |  5200 |
| Laptop  | Feb      |  6100 |
| Laptop  | Mar      |  5900 |
| Phone   | Jan      |  4800 |
| Phone   | Feb      |  5300 |
| Phone   | Mar      |  5600 |

Notice that the month names become values instead of column headers.

---

# 7. Customizing `melt()`

The default column names **Variable** and **Value** are often too generic.

Rename them using:

```python id="melt02"
pd.melt(
    df,
    id_vars="Product",
    var_name="Month",
    value_name="Sales"
)
```

Output:

| Product | Month | Sales |
| ------- | ----- | ----: |
| Laptop  | Jan   |  5200 |
| Laptop  | Feb   |  6100 |
| Laptop  | Mar   |  5900 |
| Phone   | Jan   |  4800 |
| Phone   | Feb   |  5300 |
| Phone   | Mar   |  5600 |

The resulting dataset is much more descriptive.

---

# Business Example

Imagine an international retailer receives monthly sales reports from regional offices.

Each office submits an Excel sheet where every month is stored as a separate column.

Before creating Power BI dashboards, analysts first reshape the data into a tidy format using `melt()`.

This enables:

* Monthly trend analysis.
* Sales forecasting.
* Region-wise comparisons.
* Interactive dashboards.
* Machine learning feature engineering.

---

# Best Practices

✔ Keep identifier columns in `id_vars`.

✔ Rename columns using `var_name` and `value_name`.

✔ Prefer long-format data for analytics.

✔ Validate row counts after reshaping.

✔ Preserve original datasets before restructuring.

---

# Common Mistakes

### Melting Identifier Columns

Choosing incorrect identifier columns can duplicate or distort data.

Always verify which columns uniquely identify each observation.

---

### Losing Context

Rename generic columns immediately.

Instead of:

```text
Variable
Value
```

Prefer:

```text
Month
Sales
```

This makes reports much easier to understand.

---

# Key Takeaways

After completing this section, you should understand:

* The difference between wide and long data.
* Why tidy data is important.
* How to reshape datasets using `melt()`.
* How to customize reshaped columns.
* Why long-format data is preferred for analysis.

> **"Reshaping data is often the first step toward transforming raw spreadsheets into meaningful analytical datasets."**

# 8. Understanding `pivot()`

While `melt()` converts **wide data into long data**, the `pivot()` function performs the opposite operation.

It transforms **long-format** data into a **wide-format** table.

This is especially useful for creating summary reports and dashboards.

---

## Example Dataset

| Product | Month | Sales |
| ------- | ----- | ----: |
| Laptop  | Jan   |  5200 |
| Laptop  | Feb   |  6100 |
| Laptop  | Mar   |  5900 |
| Phone   | Jan   |  4800 |
| Phone   | Feb   |  5300 |
| Phone   | Mar   |  5600 |

Convert the data into a wide table.

```python id="pivot01"
df.pivot(
    index="Product",
    columns="Month",
    values="Sales"
)
```

### Output

| Product |  Jan |  Feb |  Mar |
| ------- | ---: | ---: | ---: |
| Laptop  | 5200 | 6100 | 5900 |
| Phone   | 4800 | 5300 | 5600 |

The month values become column names.

---

# 9. When `pivot()` Fails

`pivot()` requires each combination of index and column values to be unique.

Suppose the dataset contains:

| Product | Month | Sales |
| ------- | ----- | ----: |
| Laptop  | Jan   |  5200 |
| Laptop  | Jan   |  6100 |

Running:

```python id="pivot02"
df.pivot(
    index="Product",
    columns="Month",
    values="Sales"
)
```

produces:

```text id="pivoterr01"
ValueError

Index contains duplicate entries.
Cannot reshape.
```

When duplicate values exist, use **`pivot_table()`** instead.

---

# 10. Using `pivot_table()`

Unlike `pivot()`, `pivot_table()` performs aggregation before reshaping.

Example:

```python id="pivot03"
pd.pivot_table(
    df,
    values="Sales",
    index="Product",
    columns="Month",
    aggfunc="sum"
)
```

Output:

| Product |   Jan |  Feb |  Mar |
| ------- | ----: | ---: | ---: |
| Laptop  | 11300 | 6100 | 5900 |
| Phone   |  4800 | 5300 | 5600 |

Supported aggregation functions include:

* `sum`
* `mean`
* `count`
* `max`
* `min`

---

# 11. Understanding `stack()`

The `stack()` function converts **columns into rows**.

Example:

Original DataFrame:

| Product |  Jan |  Feb |
| ------- | ---: | ---: |
| Laptop  | 5200 | 6100 |
| Phone   | 4800 | 5300 |

Apply:

```python id="stack01"
df.stack()
```

### Output

| Product | Month | Sales |
| ------- | ----- | ----: |
| Laptop  | Jan   |  5200 |
| Laptop  | Feb   |  6100 |
| Phone   | Jan   |  4800 |
| Phone   | Feb   |  5300 |

`stack()` creates a hierarchical (MultiIndex) Series.

It is commonly used when preparing data for advanced analysis.

---

# 12. Understanding `unstack()`

`unstack()` performs the opposite operation.

It converts an index level into columns.

Suppose:

| Product | Month | Sales |
| ------- | ----- | ----: |
| Laptop  | Jan   |  5200 |
| Laptop  | Feb   |  6100 |
| Phone   | Jan   |  4800 |
| Phone   | Feb   |  5300 |

After applying:

```python id="unstack01"
df.unstack()
```

The result becomes:

| Product |  Jan |  Feb |
| ------- | ---: | ---: |
| Laptop  | 5200 | 6100 |
| Phone   | 4800 | 5300 |

`stack()` and `unstack()` are especially useful when working with MultiIndex DataFrames.

---

# 13. Working with MultiIndex Reshaping

Suppose we create a MultiIndex.

```python id="multireshape01"
df = df.set_index(
    ["Region", "Category"]
)
```

Example:

| Region | Category   | Sales |
| ------ | ---------- | ----: |
| North  | Furniture  |  5200 |
| North  | Technology |  6100 |
| South  | Furniture  |  7300 |
| South  | Technology |  8100 |

Unstack the second level.

```python id="multireshape02"
df.unstack()
```

### Output

| Region | Furniture | Technology |
| ------ | --------: | ---------: |
| North  |      5200 |       6100 |
| South  |      7300 |       8100 |

This creates a clean business summary from hierarchical data.

---

# 14. Choosing the Right Function

Each reshaping function has a different purpose.

| Function        | Converts                  | Typical Use          |
| --------------- | ------------------------- | -------------------- |
| `melt()`        | Wide → Long               | Data preparation     |
| `pivot()`       | Long → Wide               | Report generation    |
| `pivot_table()` | Long → Wide + Aggregation | Business summaries   |
| `stack()`       | Columns → Rows            | MultiIndex reshaping |
| `unstack()`     | Rows → Columns            | MultiIndex reporting |

Understanding when to use each function is essential for efficient data transformation.

---

# Business Example

A multinational retailer stores daily sales in a transactional format.

Different teams require different layouts:

* Data Scientists prefer **long-format** data for modeling.
* Business Analysts create **pivot tables** for reporting.
* Executives receive **wide-format dashboards**.
* Data Engineers use **stack()** and **unstack()** when restructuring complex datasets.

Using the appropriate reshaping function ensures that the same data can support multiple business needs.

---

# Best Practices

✔ Use `melt()` for data preparation.

✔ Use `pivot()` only when each combination is unique.

✔ Use `pivot_table()` when duplicate observations exist.

✔ Apply `stack()` and `unstack()` when working with MultiIndex DataFrames.

✔ Verify row counts after reshaping to ensure data integrity.

---

# Common Mistakes

### Using `pivot()` on Duplicate Data

If duplicate index–column combinations exist, `pivot()` raises an error.

In such cases, switch to:

```python id="pivot04"
pd.pivot_table()
```

---

### Forgetting the Index Structure

`stack()` and `unstack()` rely on the DataFrame's index.

Before using them, inspect the index.

```python id="index01"
df.index
```

or

```python id="index02"
df.index.names
```

Understanding the current index structure helps avoid unexpected reshaping results.

---

# Quick Recap

You have now learned how to:

* Convert long data into wide format using `pivot()`.
* Handle duplicate values with `pivot_table()`.
* Reshape columns into rows using `stack()`.
* Convert index levels into columns using `unstack()`.
* Work with MultiIndex reshaping.
* Select the correct reshaping function for different analytical tasks.

> **"Effective data reshaping allows a single dataset to support reporting, visualization, statistical analysis, and machine learning without changing the underlying information."**

# 15. Real-World Business Case Study

## Scenario

You are working as a **Senior Data Analyst** at **RetailHub**, a multinational retail company.

Every month, regional offices submit sales reports in Excel. Unfortunately, each region follows a different reporting format.

Some files are in **wide format**, while others are already in **long format**.

Before building dashboards in Power BI, your first task is to standardize every dataset into a consistent structure.

The dataset contains:

* Region
* Product
* Jan
* Feb
* Mar
* Apr
* May
* Jun

Your objective is to reshape the data into a tidy format suitable for analysis.

---

# Business Questions

### Question 1

Convert monthly columns into rows.

```python id="case_melt01"
sales_long = pd.melt(
    df,
    id_vars=[
        "Region",
        "Product"
    ],
    var_name="Month",
    value_name="Sales"
)
```

---

### Question 2

Generate a summary report showing monthly sales by product.

```python id="case_pivot01"
monthly_summary = (
    sales_long.pivot(
        index="Product",
        columns="Month",
        values="Sales"
    )
)
```

---

### Question 3

Handle duplicate monthly entries.

```python id="case_pivot02"
monthly_summary = (
    pd.pivot_table(
        sales_long,
        index="Product",
        columns="Month",
        values="Sales",
        aggfunc="sum"
    )
)
```

---

### Question 4

Convert a MultiIndex report into a dashboard-friendly table.

```python id="case_unstack01"
report = (
    sales_long
    .groupby(
        ["Region", "Month"]
    )["Sales"]
    .sum()
    .unstack()
)
```

---

### Question 5

Return the report to long format for machine learning.

```python id="case_stack01"
report.stack()
```

---

# 16. Complete Reshaping Workflow

A typical analytics workflow follows these steps:

```text id="workflow01"
Excel File
        │
        ▼
Load into Pandas
        │
        ▼
Inspect Structure
        │
        ▼
Convert Wide → Long
        │
        ▼
Clean & Validate
        │
        ▼
Group & Aggregate
        │
        ▼
Pivot for Reporting
        │
        ▼
Create Dashboard
```

Reshaping is rarely the final step—it is usually part of a larger data preparation pipeline.

---

# 17. Performance Optimization

Large datasets require efficient reshaping techniques.

### Specify Identifier Columns Carefully

Instead of melting the entire DataFrame:

```python id="perf_melt01"
pd.melt(df)
```

Prefer:

```python id="perf_melt02"
pd.melt(
    df,
    id_vars=[
        "Region",
        "Product"
    ]
)
```

This preserves important identifiers and avoids unnecessary restructuring.

---

### Use `pivot_table()` for Large Datasets

When duplicate combinations are possible, `pivot_table()` is safer than `pivot()`.

```python id="perf_pivot01"
pd.pivot_table(
    df,
    aggfunc="sum"
)
```

---

### Reset Index After Complex Operations

```python id="perf_reset01"
summary = (
    summary
    .reset_index()
)
```

Flat tables are easier to export and visualize.

---

# 18. Business Insights

After restructuring the dataset, you observe:

* Long-format data simplifies monthly trend analysis.
* Pivot tables provide cleaner executive summaries.
* MultiIndex reshaping improves complex reporting.
* A single tidy dataset can support dashboards, forecasting, and machine learning.
* Consistent data structures reduce errors during analysis.

These improvements increase efficiency across analytics teams.

---

# 19. Practice Exercises

## Beginner

1. Convert a wide table into long format.
2. Rename melted columns.
3. Create a simple pivot table.
4. Convert long data back into wide format.
5. Compare wide and long structures.

---

## Intermediate

6. Use `pivot_table()` with multiple aggregation functions.
7. Create a MultiIndex report.
8. Apply `stack()`.
9. Apply `unstack()`.
10. Build a monthly sales summary.

---

## Advanced

11. Reshape a survey dataset.
12. Convert quarterly reports into tidy data.
13. Build a complete reshaping pipeline.
14. Compare `pivot()` and `pivot_table()` using duplicate data.
15. Write five recommendations for improving data organization.

---

# 20. Interview Questions

## Beginner

1. What is data reshaping?
2. Difference between wide and long data?
3. What does `melt()` do?
4. What is `pivot()` used for?
5. Why is tidy data important?

---

## Intermediate

6. Difference between `pivot()` and `pivot_table()`?
7. When should `stack()` be used?
8. What is `unstack()`?
9. Why are MultiIndexes useful?
10. What is the role of `id_vars` in `melt()`?

---

## Advanced

11. Explain a complete reshaping workflow.
12. How does tidy data improve machine learning?
13. Compare reshaping in Pandas with SQL transformations.
14. Why should data be reshaped before visualization?
15. Describe a real-world business scenario involving data restructuring.

---

# 21. Cheat Sheet

| Operation                 | Syntax                   |
| ------------------------- | ------------------------ |
| Wide → Long               | `pd.melt()`              |
| Long → Wide               | `pivot()`                |
| Long → Wide + Aggregation | `pivot_table()`          |
| Columns → Rows            | `stack()`                |
| Rows → Columns            | `unstack()`              |
| MultiIndex                | `set_index()`            |
| Reset Index               | `reset_index()`          |
| Rename Melt Columns       | `var_name`, `value_name` |

---

# 22. Mini Project

## Retail Sales Reshaping Pipeline

Using any retail, finance, healthcare, HR, or education dataset:

Perform the following tasks:

* Import the dataset.
* Identify whether it is in wide or long format.
* Convert wide-format data into tidy format using `melt()`.
* Rename variables appropriately.
* Generate summary reports using `pivot_table()`.
* Create a MultiIndex report.
* Apply `stack()` and `unstack()`.
* Export both the tidy dataset and the executive report.
* Write **five executive-level business insights**.
* Recommend **three improvements** to standardize future data collection.

### Example Business Insights

* Long-format data simplified monthly sales comparisons across all regions.
* Pivot tables enabled quick executive summaries by product and month.
* Tidy data reduced preprocessing time before visualization.
* MultiIndex reports improved regional performance analysis.
* A standardized reporting structure will improve consistency across departments.

---

# 23. Summary

Congratulations! 🎉

Today you mastered **Data Reshaping & Tidy Data** in Pandas.

You learned how to:

* Understand wide and long data formats.
* Create tidy datasets using `melt()`.
* Transform long data into wide reports using `pivot()`.
* Handle duplicate records with `pivot_table()`.
* Reshape MultiIndex objects using `stack()` and `unstack()`.
* Build efficient workflows for reporting and machine learning.

These techniques are fundamental in data engineering, analytics, dashboard creation, and predictive modeling.

---

# 24. What's Next?

In **Day 20**, you'll learn **Advanced File Handling & Data Input/Output in Pandas**.

Topics include:

* Reading CSV, Excel, JSON, HTML, XML, and Parquet files
* Writing data to different formats
* Reading multiple files automatically
* Chunk processing for large datasets
* Compression formats (ZIP, GZIP)
* Performance optimization
* Building automated ETL pipelines

These skills are essential for handling real-world datasets from different sources efficiently.

---

<div align="center">

# 🎉 Day 19 Complete!

You've mastered the art of reshaping data into analysis-ready formats.

From converting spreadsheets into tidy datasets to preparing executive reports and machine learning inputs, you now have the skills to organize data for virtually any analytical workflow.

⭐ **Next → Day 20: Advanced File Handling & Data Input/Output** 📂🐼

</div>
