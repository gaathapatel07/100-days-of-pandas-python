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

> **"Cleaning data is not about making every dataset perfect—it is about making it reliable enough to support confident decisions."**

