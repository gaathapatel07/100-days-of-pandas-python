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
