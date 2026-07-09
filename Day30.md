# Day 29 — Advanced Missing Data Handling & Data Cleaning

<div align="center">

# 100 Days of Pandas

### Day 29 · Cleaning Data for Accurate Analysis

*"The quality of your analysis depends directly on the quality of your data."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Data%20Cleaning-blue)
![Day](https://img.shields.io/badge/Day-29-orange)

</div>

---

# Table of Contents

1. Introduction
2. Why Data Cleaning Matters
3. Learning Objectives
4. Understanding Missing Data
5. Detecting Missing Values
6. Counting Missing Values
7. Removing Missing Values
8. Summary

---

# 1. Introduction

Real-world datasets are rarely perfect.

Common problems include:

* Missing values
* Duplicate records
* Incorrect spellings
* Invalid dates
* Wrong data types
* Extra spaces
* Mixed formatting
* Outliers

Before performing analysis or building machine learning models, datasets must be cleaned.

Pandas provides a rich collection of tools for identifying, correcting, and removing data quality issues.

---

# 2. Why Data Cleaning Matters

Imagine an online retail company.

The sales dataset contains:

| Customer | Sales |
| -------- | ----: |
| Alice    |  5200 |
| Rahul    |   NaN |
| Priya    |  4800 |

If the missing value is ignored:

* Revenue calculations become inaccurate.
* Average sales decrease incorrectly.
* Dashboards become unreliable.
* Machine learning models may fail.

Cleaning the data ensures trustworthy business insights.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Detect missing values.
* Remove missing records.
* Fill missing values intelligently.
* Identify duplicate records.
* Build reliable data-cleaning pipelines.

---

# 4. Understanding Missing Data

Missing values are represented as:

```text id="missing01"
NaN
```

or

```text id="missing02"
None
```

or

```text id="missing03"
NaT
```

depending on the data type.

Example

| Customer | Sales |
| -------- | ----: |
| Alice    |  5200 |
| Rahul    |   NaN |
| Priya    |  4800 |

The missing sales value should be handled before analysis.

---

# 5. Detecting Missing Values

Use `isna()`.

```python id="missing04"
df.isna()
```

Output

| Customer | Sales |
| -------- | ----: |
| False    | False |
| False    |  True |
| False    | False |

`True` indicates a missing value.

---

## Detect Missing Values in One Column

```python id="missing05"
df["Sales"].isna()
```

---

## Detect Non-Missing Values

```python id="missing06"
df.notna()
```

Returns `True` wherever values are present.

---

# 6. Counting Missing Values

Count missing values in each column.

```python id="missing07"
df.isna().sum()
```

Output

| Column   | Missing |
| -------- | ------: |
| Customer |       0 |
| Sales    |       1 |
| Profit   |       3 |

---

## Total Missing Values

```python id="missing08"
df.isna().sum().sum()
```

Returns the total number of missing values in the DataFrame.

---

## Percentage of Missing Values

```python id="missing09"
(
    df.isna()
      .mean()
      * 100
)
```

Output

| Column   | Missing % |
| -------- | --------: |
| Customer |        0% |
| Sales    |        5% |
| Profit   |       12% |

This helps prioritize cleaning efforts.

---

# 7. Removing Missing Values

## Remove Rows with Missing Values

```python id="drop01"
clean_df = (
    df.dropna()
)
```

Every row containing at least one missing value is removed.

---

## Remove Columns with Missing Values

```python id="drop02"
df.dropna(
    axis=1
)
```

Columns containing missing values are removed.

---

## Remove Rows Only When Every Value Is Missing

```python id="drop03"
df.dropna(
    how="all"
)
```

---

## Remove Rows Based on Specific Columns

```python id="drop04"
df.dropna(
    subset=[
        "Sales",
        "Profit"
    ]
)
```

Rows are removed only if the specified columns contain missing values.

---

## Minimum Non-Missing Values Required

Keep rows with at least three valid values.

```python id="drop05"
df.dropna(
    thresh=3
)
```

---

# Business Example

A hospital maintains patient records.

Some rows contain:

* Missing patient age.
* Missing diagnosis.
* Missing admission dates.

Analysts:

* Identify missing values.
* Remove incomplete records where necessary.
* Preserve valuable records with only minor missing information.

This improves reporting quality while minimizing unnecessary data loss.

---

# Best Practices

✔ Measure missing values before cleaning.

✔ Understand why data is missing.

✔ Remove rows only when appropriate.

✔ Preserve important business records whenever possible.

✔ Document all cleaning decisions.

---

# Common Mistakes

### Removing Too Much Data

Using:

```python id="mistake01"
df.dropna()
```

may remove thousands of valuable records.

Always inspect the percentage of missing values first.

---

### Ignoring Missing Values

Many Pandas functions skip missing values automatically, but not every algorithm does.

Always assess missing data before analysis.

---

# Key Takeaways

After completing this section, you should understand:

* What missing values are.
* How to detect missing data.
* How to count missing values.
* How to remove missing values safely.
* Why thoughtful data cleaning is critical for reliable analysis.

> **"Cleaning data is not about removing imperfections—it is about ensuring that every business decision is based on trustworthy information."**

# 8. Filling Missing Values with `fillna()`

Removing missing values is not always the best option.

Instead, replace them with meaningful values.

Suppose the dataset contains:

| Customer | Sales |
| -------- | ----: |
| Alice    |  5200 |
| Rahul    |   NaN |
| Priya    |  4800 |

Fill missing values with zero.

```python id="fill01"
df["Sales"] = (
    df["Sales"]
      .fillna(0)
)
```

Output

| Customer | Sales |
| -------- | ----: |
| Alice    |  5200 |
| Rahul    |     0 |
| Priya    |  4800 |

---

## Fill Multiple Columns

```python id="fill02"
df.fillna(
    {
        "Sales": 0,
        "Profit": 0,
        "City": "Unknown"
    }
)
```

Each column receives its own replacement value.

---

# 9. Filling Using Statistics

Replacing missing values with statistical measures often preserves the overall distribution.

---

## Fill with Mean

```python id="fill03"
df["Salary"] = (
    df["Salary"]
      .fillna(
          df["Salary"].mean()
      )
)
```

Best suited for approximately normally distributed numerical data.

---

## Fill with Median

```python id="fill04"
df["Income"] = (
    df["Income"]
      .fillna(
          df["Income"].median()
      )
)
```

Median is more robust when outliers are present.

---

## Fill with Mode

```python id="fill05"
df["City"] = (
    df["City"]
      .fillna(
          df["City"].mode()[0]
      )
)
```

Mode is commonly used for categorical variables.

---

# 10. Forward Fill (`ffill`)

Forward Fill copies the previous valid value.

```python id="ffill01"
df["Sales"] = (
    df["Sales"]
      .ffill()
)
```

Example

| Day | Sales |
| --: | ----: |
|   1 |  5200 |
|   2 |   NaN |
|   3 |  6100 |

Output

| Day | Sales |
| --: | ----: |
|   1 |  5200 |
|   2 |  5200 |
|   3 |  6100 |

Useful for time-series data where the previous observation remains valid.

---

# 11. Backward Fill (`bfill`)

Backward Fill copies the next valid value.

```python id="bfill01"
df["Sales"] = (
    df["Sales"]
      .bfill()
)
```

Example

| Day | Sales |
| --: | ----: |
|   1 |  5200 |
|   2 |   NaN |
|   3 |  6100 |

Output

| Day | Sales |
| --: | ----: |
|   1 |  5200 |
|   2 |  6100 |
|   3 |  6100 |

---

# 12. Interpolation

Interpolation estimates missing values based on neighboring values.

```python id="interp01"
df["Temperature"] = (
    df["Temperature"]
      .interpolate()
)
```

Example

| Day | Temperature |
| --: | ----------: |
|   1 |          20 |
|   2 |         NaN |
|   3 |          24 |

Output

| Day | Temperature |
| --: | ----------: |
|   1 |          20 |
|   2 |          22 |
|   3 |          24 |

Interpolation is commonly used in:

* Weather data
* Sensor readings
* Stock prices
* Scientific measurements

---

# 13. Detecting Duplicate Records

Duplicate rows can distort business metrics.

Identify duplicates.

```python id="dup01"
df.duplicated()
```

Output

| Row | Duplicate |
| --: | --------- |
|   0 | False     |
|   1 | False     |
|   2 | True      |

---

## Count Duplicate Rows

```python id="dup02"
df.duplicated().sum()
```

---

# 14. Removing Duplicates

Remove duplicate rows.

```python id="dup03"
df = (
    df.drop_duplicates()
)
```

---

## Remove Duplicates Based on Specific Columns

```python id="dup04"
df.drop_duplicates(
    subset=[
        "Customer ID"
    ]
)
```

---

## Keep the Last Occurrence

```python id="dup05"
df.drop_duplicates(
    subset="Customer ID",
    keep="last"
)
```

Options:

| Option  | Meaning                         |
| ------- | ------------------------------- |
| `first` | Keep first occurrence (default) |
| `last`  | Keep last occurrence            |
| `False` | Remove all duplicates           |

---

# 15. Standardizing Data

Real-world data often contains inconsistent formatting.

Example:

| City   |
| ------ |
| mumbai |
| Mumbai |
| MUMBAI |

Standardize the values.

```python id="clean01"
df["City"] = (
    df["City"]
      .str.strip()
      .str.title()
)
```

Output

| City   |
| ------ |
| Mumbai |
| Mumbai |
| Mumbai |

---

## Standardizing Text

```python id="clean02"
df["Department"] = (
    df["Department"]
      .str.upper()
)
```

or

```python id="clean03"
df["Department"] = (
    df["Department"]
      .str.lower()
)
```

Consistency improves grouping and reporting accuracy.

---

# 16. Validating Data

After cleaning, validate the dataset.

Missing values.

```python id="valid01"
df.isna().sum()
```

Duplicates.

```python id="valid02"
df.duplicated().sum()
```

Data types.

```python id="valid03"
df.dtypes
```

Summary statistics.

```python id="valid04"
df.describe()
```

Validation ensures that cleaning operations produced the intended result.

---

# Business Example

A banking institution receives customer records from multiple branches.

Problems include:

* Missing customer ages.
* Duplicate customer IDs.
* Inconsistent city names.
* Missing account balances.

Analysts:

* Fill missing balances using appropriate strategies.
* Remove duplicate customer IDs.
* Standardize city names.
* Validate the cleaned dataset before generating regulatory reports.

---

# Best Practices

✔ Choose an imputation strategy based on the data type and business context.

✔ Use the median for skewed numerical data.

✔ Standardize text before grouping.

✔ Validate the dataset after every major cleaning step.

✔ Keep a copy of the raw dataset before making changes.

---

# Common Mistakes

### Filling Every Missing Value with Zero

Zero may not always be a meaningful replacement.

Choose values that make sense in the business context.

---

### Removing Legitimate Duplicate Transactions

Not every repeated row is an error.

For example, two customers may legitimately purchase the same product at the same time.

Always verify duplicates before deleting them.

---

### Ignoring Text Standardization

Values such as:

```text id="mistake01"
Delhi

delhi

 DELHI
```

represent the same city but will be treated as different categories unless standardized.

---

# Quick Recap

You have now learned how to:

* Fill missing values using `fillna()`.
* Use mean, median, and mode for imputation.
* Apply Forward Fill and Backward Fill.
* Perform interpolation.
* Detect and remove duplicates.
* Standardize text values.
* Validate cleaned datasets.

> **"Effective data cleaning preserves valuable information while eliminating inconsistencies that could lead to misleading conclusions."**

# 17. Enterprise Data Cleaning Workflow

Real-world organizations follow a structured data-cleaning process before performing analytics or training machine learning models.

Typical workflow:

```text id="workflow01"
Raw Dataset
      │
      ▼
Detect Missing Values
      │
      ▼
Handle Missing Values
      │
      ▼
Remove Duplicate Records
      │
      ▼
Standardize Text & Formats
      │
      ▼
Validate Data Types
      │
      ▼
Check Business Rules
      │
      ▼
Export Clean Dataset
```

Each stage improves the quality and reliability of the dataset.

---

# 18. Building a Complete Cleaning Pipeline

Instead of cleaning step by step, combine operations into a reusable pipeline.

```python id="pipeline01"
def clean_dataset(df):

    return (
        df
        .drop_duplicates()
        .assign(
            City=lambda x:
            x["City"]
              .str.strip()
              .str.title()
        )
        .assign(
            Sales=lambda x:
            x["Sales"]
              .fillna(
                  x["Sales"].median()
              )
        )
        .reset_index(drop=True)
    )
```

Run the pipeline.

```python id="pipeline02"
clean_df = clean_dataset(df)
```

Reusable pipelines reduce repetitive code and improve maintainability.

---

# 19. Data Validation Checklist

Cleaning is incomplete without validation.

### Check Missing Values

```python id="valid05"
df.isna().sum()
```

---

### Check Duplicate Rows

```python id="valid06"
df.duplicated().sum()
```

---

### Verify Data Types

```python id="valid07"
df.dtypes
```

---

### Review Summary Statistics

```python id="valid08"
df.describe(
    include="all"
)
```

---

### Verify Unique Categories

```python id="valid09"
df["City"].unique()
```

Validation helps detect remaining inconsistencies before reporting.

---

# 20. Performance Optimization

Large datasets require efficient cleaning strategies.

---

## Vectorized Operations

Instead of loops:

```python id="perf01"
for city in df["City"]:
    city = city.title()
```

Use:

```python id="perf02"
df["City"] = (
    df["City"]
      .str.title()
)
```

Vectorized methods are much faster.

---

## Convert Data Types

```python id="perf03"
df["Region"] = (
    df["Region"]
      .astype("category")
)
```

This reduces memory usage for repeated values.

---

## Clean Only Necessary Columns

Avoid transforming every column unnecessarily.

Example:

```python id="perf04"
df["Customer"] = (
    df["Customer"]
      .str.strip()
)
```

Target only columns requiring cleanup.

---

# 21. Enterprise Case Study

## Scenario

You are a **Senior Data Analyst** at **RetailHub**.

The sales dataset contains:

* Missing sales values.
* Duplicate orders.
* Inconsistent city names.
* Invalid customer ages.
* Extra spaces in customer names.

Management requires a clean dataset before generating executive dashboards.

---

## Business Questions

### Question 1

Count missing values.

```python id="case01"
df.isna().sum()
```

---

### Question 2

Fill missing sales with the median.

```python id="case02"
df["Sales"] = (
    df["Sales"]
      .fillna(
          df["Sales"].median()
      )
)
```

---

### Question 3

Remove duplicate records.

```python id="case03"
df = (
    df.drop_duplicates()
)
```

---

### Question 4

Standardize city names.

```python id="case04"
df["City"] = (
    df["City"]
      .str.strip()
      .str.title()
)
```

---

### Question 5

Create a reusable cleaning function.

```python id="case05"
def prepare_data(dataframe):

    dataframe = (
        dataframe
        .drop_duplicates()
    )

    dataframe["City"] = (
        dataframe["City"]
          .str.strip()
          .str.title()
    )

    dataframe["Sales"] = (
        dataframe["Sales"]
          .fillna(
              dataframe["Sales"].median()
          )
    )

    return dataframe
```

---

# 22. Business Insights

After cleaning the dataset, analysts discover:

* Missing values no longer distort sales calculations.
* Duplicate orders are removed, preventing revenue overestimation.
* Standardized city names improve regional reporting.
* Consistent data types simplify downstream analysis.
* Reusable cleaning pipelines reduce manual effort and improve reproducibility.

---

# 23. Practice Exercises

## Beginner

1. Detect missing values.
2. Count missing values.
3. Remove rows with missing values.
4. Fill missing values with zero.
5. Remove duplicate rows.

---

## Intermediate

6. Fill missing values using the mean.
7. Fill missing values using the median.
8. Standardize city names.
9. Remove duplicates using specific columns.
10. Validate data types after cleaning.

---

## Advanced

11. Build a reusable cleaning pipeline.
12. Compare different imputation strategies.
13. Optimize a large dataset using categories.
14. Create a complete validation report.
15. Write five recommendations for improving data quality.

---

# 24. Interview Questions

## Beginner

1. What is a missing value?
2. Difference between `NaN`, `None`, and `NaT`?
3. What does `isna()` return?
4. What is `fillna()`?
5. What is `dropna()`?

---

## Intermediate

6. Difference between Forward Fill and Backward Fill?
7. When should interpolation be used?
8. How do you detect duplicates?
9. Why standardize text values?
10. How do you validate a cleaned dataset?

---

## Advanced

11. Design a complete data-cleaning workflow.
12. Compare mean, median, and mode imputation.
13. How would you clean a dataset with 100 million rows?
14. How do data quality issues affect machine learning models?
15. What practices ensure reproducible data-cleaning pipelines?

---

# 25. Cheat Sheet

| Task                | Syntax                     |
| ------------------- | -------------------------- |
| Detect Missing      | `isna()`                   |
| Detect Non-Missing  | `notna()`                  |
| Count Missing       | `isna().sum()`             |
| Remove Missing      | `dropna()`                 |
| Fill Missing        | `fillna()`                 |
| Forward Fill        | `ffill()`                  |
| Backward Fill       | `bfill()`                  |
| Interpolate         | `interpolate()`            |
| Detect Duplicates   | `duplicated()`             |
| Remove Duplicates   | `drop_duplicates()`        |
| Standardize Text    | `.str.strip().str.title()` |
| Validate Data Types | `dtypes`                   |

---

# 26. Mini Project

## Customer Data Quality Improvement Pipeline

Using any retail, banking, healthcare, HR, or telecom dataset:

Complete the following tasks:

* Detect missing values.
* Measure the percentage of missing data.
* Apply appropriate imputation techniques.
* Detect and remove duplicate records.
* Standardize categorical text columns.
* Validate data types.
* Build a reusable cleaning pipeline.
* Export the cleaned dataset.
* Write **five executive-level business insights**.
* Recommend **three improvements** for future data collection and quality assurance.

### Example Business Insights

* Median imputation preserved the distribution of sales values.
* Removing duplicate transactions prevented inflated revenue calculations.
* Standardized city names improved regional reporting accuracy.
* Automated cleaning reduced manual preprocessing time.
* A reusable pipeline ensured consistent data preparation across reporting cycles.

---

# 27. Summary

Congratulations! 🎉

Today you mastered **Advanced Missing Data Handling & Data Cleaning** in Pandas.

You learned how to:

* Detect and quantify missing values.
* Remove or impute missing data appropriately.
* Apply Forward Fill, Backward Fill, and interpolation.
* Detect and remove duplicate records.
* Standardize text values.
* Validate cleaned datasets.
* Build reusable data-cleaning pipelines.

These skills are fundamental for business intelligence, data engineering, machine learning, and every real-world analytics project.

---

# 28. What's Next?

In **Day 30**, you'll learn **Advanced Input & Output (I/O), File Formats & Data Serialization**.

Topics include:

* Reading CSV, Excel, JSON, HTML, XML, and Parquet files
* Writing data to multiple formats
* Compression (`gzip`, `zip`, `bz2`)
* Reading large files in chunks
* Working with SQL databases
* Clipboard operations
* Pickle serialization
* Efficient file handling and performance optimization

These concepts are essential for building end-to-end data pipelines and integrating Pandas with databases, cloud storage, and enterprise systems.

---

<div align="center">

#  Day 29 Complete!

You've mastered one of the most critical phases of any analytics workflow—**data cleaning**.

By learning to detect, clean, validate, and standardize datasets, you now have the skills to prepare reliable, high-quality data for reporting, dashboards, statistical analysis, and machine learning.

</div>
