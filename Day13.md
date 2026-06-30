# Day 13 — Advanced Missing Data Handling & Data Quality

<div align="center">

# 100 Days of Pandas

### Day 13 · Building Reliable Datasets for Accurate Analysis

*"The quality of every analysis depends on the quality of the data behind it."*

![Difficulty](https://img.shields.io/badge/Difficulty-Intermediate-yellow)
![Topic](https://img.shields.io/badge/Topic-Advanced%20Data%20Cleaning-blue)
![Day](https://img.shields.io/badge/Day-13-orange)

</div>

---

# Table of Contents

1. Introduction
2. Why Data Quality Matters
3. Learning Objectives
4. Understanding Missing Data
5. Types of Missing Data
6. Detecting Missing Values
7. Measuring Data Quality
8. Summary

---

# 1. Introduction

Real-world datasets are rarely perfect.

Whether data comes from customer forms, IoT sensors, mobile applications, banking systems, hospital databases, or online marketplaces, it often contains missing values, incomplete records, inconsistent entries, duplicate observations, and invalid values.

Before any meaningful analysis can begin, analysts must assess and improve data quality.

Data cleaning is not simply about removing errors—it is about ensuring that business decisions are based on trustworthy information.

This lesson focuses on advanced techniques for identifying and evaluating missing data before deciding how to handle it.

---

# 2. Why Data Quality Matters

Imagine an e-commerce company preparing a quarterly sales report.

The dataset contains:

| Order ID | Customer | Sales | Region |
| -------- | -------- | ----: | ------ |
| 1001     | Alice    |  2500 | West   |
| 1002     | Rahul    |   NaN | South  |
| 1003     | Emma     |  4200 | NaN    |
| 1004     | David    |  1800 | West   |

If missing values are ignored:

* Revenue calculations become inaccurate.
* Regional reports become incomplete.
* Forecasting models may fail.
* Dashboard KPIs may mislead decision-makers.

Improving data quality ensures that reports, dashboards, and predictive models are based on reliable information.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Evaluate dataset quality.
* Measure missing data percentages.
* Differentiate between different types of missingness.
* Decide when to remove or preserve incomplete records.
* Build a structured data-cleaning workflow.

---

# 4. Understanding Missing Data

Missing data represents information that is unavailable or was not recorded.

In Pandas, missing values commonly appear as:

* `NaN`
* `None`
* Empty strings (after import)
* Missing timestamps (`NaT`)

Example:

| Customer | Age | City   |
| -------- | --: | ------ |
| Alice    |  24 | Mumbai |
| Rahul    | NaN | Delhi  |
| Emma     |  29 | NaN    |
| David    |  31 | Pune   |

Although the dataset appears complete at first glance, two important values are missing.

Understanding the pattern of missingness is more valuable than immediately replacing values.

---

# 5. Types of Missing Data

Statisticians classify missing data into three categories.

---

## Missing Completely at Random (MCAR)

Missing values occur randomly and are unrelated to any other variable.

Example:

A temporary internet interruption prevents several customer records from being uploaded.

MCAR generally has the least impact on analysis.

---

## Missing at Random (MAR)

Missing values depend on another observed variable.

Example:

Customers in one region are less likely to provide their phone numbers.

The missingness is systematic but explainable.

---

## Missing Not at Random (MNAR)

The missing value itself is related to why it is missing.

Example:

High-income customers intentionally choose not to disclose their salary.

MNAR requires careful investigation because deleting or replacing these values may introduce bias.

---

# 6. Detecting Missing Values

The first step is identifying where missing values occur.

### Detect Missing Values

```python id="n3v5qw"
df.isna()
```

or

```python id="y8p2tm"
df.isnull()
```

Both functions produce identical results.

---

### Count Missing Values

```python id="f7k9ac"
df.isna().sum()
```

Example Output:

| Column   | Missing Values |
| -------- | -------------: |
| Customer |              0 |
| Sales    |              1 |
| Region   |              1 |

This provides a quick overview of which columns require attention.

---

### Count Missing Values by Row

```python id="u1c6jf"
df.isna().sum(axis=1)
```

Output:

| Row | Missing Count |
| --: | ------------: |
|   0 |             0 |
|   1 |             1 |
|   2 |             1 |
|   3 |             0 |

This helps identify records that may need further investigation.

---

# 7. Measuring Data Quality

Counting missing values is useful, but percentages provide a clearer picture.

Calculate the percentage of missing values in each column.

```python id="m4z8ld"
missing_percentage = (
    df.isna()
      .mean()
      * 100
)

missing_percentage
```

Example Output:

| Column   | Missing (%) |
| -------- | ----------: |
| Customer |         0.0 |
| Sales    |        25.0 |
| Region   |        25.0 |

Percentage-based analysis helps prioritize cleaning efforts.

For example:

| Missing Percentage | Suggested Action                                          |
| -----------------: | --------------------------------------------------------- |
|               0–5% | Fill or investigate                                       |
|              5–20% | Analyze carefully before filling                          |
|             20–50% | Consider whether the column remains useful                |
|          Above 50% | Evaluate dropping the column if it provides limited value |

These are general guidelines—the appropriate action depends on the business context.

---

# Key Takeaways

After completing this section, you should understand:

* Why data quality assessment comes before data cleaning.
* The different types of missing data.
* How to detect missing values.
* How to measure missing-value percentages.
* Why business context should guide cleaning decisions.

# 8. Forward Fill (`ffill`)

Not every missing value should be replaced with the mean or median.

In many real-world datasets, the previous value remains valid until a new one is recorded.

This technique is called **Forward Fill**.

---

## Example Dataset

| Date       | Stock Price |
| ---------- | ----------: |
| 2025-01-01 |         102 |
| 2025-01-02 |         NaN |
| 2025-01-03 |         NaN |
| 2025-01-04 |         108 |

Instead of replacing the missing values with the average stock price, we carry forward the most recent known value.

```python id="ffill01"
df["Stock Price"] = (
    df["Stock Price"]
    .ffill()
)
```

### Output

| Date       | Stock Price |
| ---------- | ----------: |
| 2025-01-01 |         102 |
| 2025-01-02 |         102 |
| 2025-01-03 |         102 |
| 2025-01-04 |         108 |

---

## When to Use Forward Fill

Forward Fill works well for:

* Stock market prices
* IoT sensor readings
* Daily inventory levels
* Machine status logs
* Slowly changing business data

It should only be used when carrying the previous value forward is logically valid.

---

# 9. Backward Fill (`bfill`)

Backward Fill works in the opposite direction.

Instead of using the previous value, it fills missing entries with the next available observation.

Example:

| Date       | Temperature |
| ---------- | ----------: |
| 2025-01-01 |         NaN |
| 2025-01-02 |         NaN |
| 2025-01-03 |          24 |
| 2025-01-04 |          26 |

```python id="bfill01"
df["Temperature"] = (
    df["Temperature"]
    .bfill()
)
```

### Output

| Date       | Temperature |
| ---------- | ----------: |
| 2025-01-01 |          24 |
| 2025-01-02 |          24 |
| 2025-01-03 |          24 |
| 2025-01-04 |          26 |

---

## When to Use Backward Fill

Typical use cases include:

* Survey datasets
* Forecast values
* Reference tables
* Configuration data

As with Forward Fill, only use Backward Fill when it makes sense for the problem being solved.

---

# 10. Interpolation

Sometimes neither the previous nor the next value is appropriate.

Interpolation estimates missing values using surrounding observations.

Example:

| Day | Sales |
| --: | ----: |
|   1 |   100 |
|   2 |   NaN |
|   3 |   140 |

Instead of copying 100 or 140, interpolation estimates a value between them.

```python id="interp01"
df["Sales"] = (
    df["Sales"]
    .interpolate()
)
```

### Output

| Day | Sales |
| --: | ----: |
|   1 |   100 |
|   2 |   120 |
|   3 |   140 |

Interpolation is particularly useful for continuous numerical data.

---

## Common Applications

Interpolation is widely used in:

* Weather analysis
* Financial time series
* Sensor monitoring
* Scientific research
* Manufacturing systems

---

# 11. Replacing Invalid Values

Missing values are not the only data quality issue.

Datasets often contain impossible or invalid values.

Example:

| Employee | Age |
| -------- | --: |
| Alice    |  25 |
| Rahul    |  -5 |
| Emma     |  31 |
| David    | 999 |

Here:

* `-5` is impossible.
* `999` is clearly unrealistic.

First, replace invalid values with `NaN`.

```python id="replace01"
import numpy as np

df["Age"] = (
    df["Age"]
    .replace(
        [-5, 999],
        np.nan
    )
)
```

Then apply an appropriate filling strategy.

This approach keeps the cleaning process consistent.

---

# 12. Detecting Out-of-Range Values

Business rules help identify suspicious records.

Example:

Customer age should be between **18** and **100**.

Find invalid ages:

```python id="range01"
invalid_age = df[
    (df["Age"] < 18) |
    (df["Age"] > 100)
]
```

Similarly:

* Discount > 100%
* Quantity < 0
* Negative prices
* Future order dates

These are all examples of values that should be validated before analysis.

---

# 13. Creating a Data Quality Report

Rather than cleaning data immediately, many analysts first prepare a summary report.

Example:

```python id="report01"
quality_report = (
    pd.DataFrame({
        "Missing Values": df.isna().sum(),
        "Missing (%)": (
            df.isna().mean() * 100
        ),
        "Data Type": df.dtypes
    })
)

quality_report
```

### Example Output

| Column     | Missing Values | Missing (%) | Data Type      |
| ---------- | -------------: | ----------: | -------------- |
| Sales      |              5 |         2.1 | float64        |
| Region     |              2 |         0.8 | object         |
| Order Date |              0 |         0.0 | datetime64[ns] |

This report provides a quick overview of dataset health before analysis begins.

---

# Best Practices

✔ Investigate why values are missing before replacing them.

✔ Choose a filling strategy based on business logic.

✔ Validate numerical ranges using business rules.

✔ Replace impossible values before performing statistical analysis.

✔ Document every cleaning step to ensure reproducibility.

---

# Common Mistakes

### Using the Same Strategy for Every Column

Avoid replacing every missing value with the mean.

Different columns require different strategies.

For example:

* Salary → Median
* City → Mode
* Sensor Data → Forward Fill
* Temperature → Interpolation

---

### Ignoring Invalid Values

A dataset may contain no missing values but still be inaccurate due to impossible values.

Always validate ranges and business rules in addition to checking for `NaN`.

---

# Quick Recap

You have now learned how to:

* Apply Forward Fill.
* Apply Backward Fill.
* Estimate missing values using Interpolation.
* Detect impossible values.
* Validate data using business rules.
* Create a reusable data quality report.

> **"High-quality datasets are not created by chance—they are built through careful validation, thoughtful cleaning strategies, and a clear understanding of the business context."**

> **"Cleaning data is not about making every dataset perfect—it is about making it reliable enough to support confident decisions."**

# 14. Real-World Business Case Study

## Scenario

You have joined **RetailHub**, a multinational e-commerce company, as a Data Quality Analyst.

Every night, transactional data from multiple systems is consolidated into a central data warehouse. Before analysts can build dashboards or generate reports, the incoming data must pass a series of quality checks.

The daily dataset contains:

* Order ID
* Customer ID
* Order Date
* Sales
* Discount
* Quantity
* Region
* Delivery Days

During inspection, you discover several issues:

* Missing sales values
* Missing regions
* Negative quantities
* Discounts greater than 100%
* Future order dates
* Duplicate orders

Your responsibility is to prepare the dataset for business reporting.

---

# Business Questions

### Question 1

Calculate missing-value percentages for every column.

```python id="dq01"
(
    df.isna().mean() * 100
).round(2)
```

---

### Question 2

Fill missing regions using the most frequent value.

```python id="dq02"
df["Region"] = (
    df["Region"]
    .fillna(
        df["Region"].mode()[0]
    )
)
```

---

### Question 3

Replace negative quantities with missing values.

```python id="dq03"
import numpy as np

df.loc[
    df["Quantity"] < 0,
    "Quantity"
] = np.nan
```

---

### Question 4

Identify discounts greater than 100%.

```python id="dq04"
invalid_discount = df[
    df["Discount"] > 100
]
```

---

### Question 5

Create a complete data quality report.

```python id="dq05"
quality_report = pd.DataFrame({
    "Missing Values": df.isna().sum(),
    "Missing (%)": (
        df.isna().mean() * 100
    ).round(2),
    "Data Type": df.dtypes,
    "Unique Values": df.nunique()
})

quality_report
```

---

# Business Insights

After completing the quality assessment, you discover:

* Less than **2%** of sales records contain missing values.
* Most missing regions can be recovered using historical customer information.
* A small number of records contain impossible discount values, indicating data entry issues.
* Duplicate order IDs suggest synchronization problems between systems.
* Overall data quality is high after applying validation and cleaning rules.

These insights help improve reporting accuracy and reduce errors in downstream analytics.

---

# 15. Practice Exercises

## Beginner

1. Count missing values in every column.
2. Calculate missing-value percentages.
3. Replace missing values using the mode.
4. Detect duplicate records.
5. Remove duplicate rows.

---

## Intermediate

6. Apply Forward Fill to a time-series column.
7. Apply Backward Fill.
8. Interpolate missing numerical values.
9. Detect invalid ages and negative quantities.
10. Build a data quality summary table.

---

## Advanced

11. Validate discounts between **0% and 100%**.
12. Detect future dates in the dataset.
13. Create reusable validation rules for numerical columns.
14. Compare the dataset before and after cleaning.
15. Write five recommendations to improve future data collection.

---

# 16. Interview Questions

## Beginner

1. What is data quality?
2. What is the difference between `NaN` and `None`?
3. When should you use `fillna()`?
4. What does `ffill()` do?
5. What is interpolation?

---

## Intermediate

6. Difference between Forward Fill and Backward Fill?
7. When is interpolation preferable to filling with the mean?
8. Why should invalid values be replaced before analysis?
9. What is a data quality report?
10. How do business rules improve data validation?

---

## Advanced

11. Describe a complete data-cleaning workflow.
12. How would you validate an e-commerce dataset before reporting?
13. Explain how poor-quality data affects machine learning models.
14. Why should every cleaning step be documented?
15. How would you automate recurring data quality checks?

---

# 17. Cheat Sheet

| Task                  | Syntax                   |
| --------------------- | ------------------------ |
| Count missing values  | `df.isna().sum()`        |
| Missing percentage    | `df.isna().mean() * 100` |
| Forward Fill          | `df.ffill()`             |
| Backward Fill         | `df.bfill()`             |
| Interpolate           | `df.interpolate()`       |
| Replace values        | `df.replace()`           |
| Remove duplicates     | `df.drop_duplicates()`   |
| Count unique values   | `df.nunique()`           |
| Detect invalid values | Boolean conditions       |
| Create quality report | `pd.DataFrame({...})`    |

---

# 18. Mini Project

## Data Quality Assessment Report

Choose any public dataset (Retail, Healthcare, Banking, HR, or Education).

Complete the following tasks:

* Load the dataset.
* Generate a data quality report.
* Calculate missing-value percentages.
* Apply appropriate filling strategies for different columns.
* Detect duplicate records.
* Validate numerical ranges using business rules.
* Detect invalid dates or values.
* Export the cleaned dataset.
* Create a summary report describing every cleaning operation.
* Write **five business insights** and **five recommendations** to improve future data collection.

### Example Business Insights

* Missing values are concentrated in customer demographic fields.
* Sales data is nearly complete, making revenue analysis reliable.
* Duplicate records account for less than 1% of observations.
* Most invalid values occur in manually entered fields.
* Date validation revealed a small number of future transactions caused by input errors.

---

# 19. Summary

Congratulations! 🎉

Today you learned how to evaluate and improve **data quality** using advanced cleaning techniques.

You explored:

* Measuring missing-value percentages.
* Forward Fill and Backward Fill.
* Interpolation for numerical data.
* Detecting invalid values.
* Applying business validation rules.
* Creating reusable data quality reports.

These skills are essential for analysts because high-quality data forms the foundation of trustworthy reports, dashboards, forecasting models, and business decisions.

---

# 20. What's Next?

In **Day 14**, you'll learn **Advanced Indexing & MultiIndex Operations**.

Topics include:

* `set_index()`
* `reset_index()`
* Hierarchical (MultiIndex) DataFrames
* Selecting data with `.loc[]` and `.iloc[]`
* Working with multiple index levels
* Sorting and slicing MultiIndex data
* Building hierarchical reports

MultiIndex is a powerful feature that allows analysts to organize and query complex datasets more efficiently.

---

<div align="center">

# 🎉 Day 13 Complete!

You've learned how to assess, validate, and improve data quality using techniques that are widely applied in professional analytics workflows.

From missing-value analysis to validation rules and quality reports, you're now equipped to prepare reliable datasets for deeper analysis.

⭐ **Next → Day 14: Advanced Indexing & MultiIndex Operations** 🗂️🐼

</div>
