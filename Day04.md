# 🐼 Day 04 — Data Cleaning: Handling Missing Values & Duplicate Data

<div align="center">

# 100 Days of Pandas

### Day 04 · Cleaning Data for Accurate Analysis

*"Garbage in, garbage out. Clean data is the foundation of trustworthy insights."*

![Difficulty](https://img.shields.io/badge/Difficulty-Beginner-success)
![Topic](https://img.shields.io/badge/Topic-Data%20Cleaning-blue)
![Day](https://img.shields.io/badge/Day-04-orange)

</div>

---

# 📚 Table of Contents

1. Introduction
2. Why Data Cleaning Matters
3. Learning Objectives
4. Understanding Missing Values
5. Types of Missing Data
6. Detecting Missing Values
7. Counting Missing Values
8. Visualizing Missing Data
9. Summary

---

# 1. Introduction

In the previous lessons, you learned how to load datasets, explore their structure, filter records, sort information, and transform data into a more meaningful format.

However, real-world datasets are rarely perfect. They often contain missing values, duplicate records, inconsistent formats, incorrect entries, and invalid data types. Before any meaningful analysis can begin, these issues must be identified and resolved.

This process is known as **data cleaning**, and it is one of the most important responsibilities of a data analyst.

In this lesson, you'll learn how to identify and handle missing values using Pandas.

---

# 2. Why Data Cleaning Matters

Imagine you're analyzing customer information for an online shopping platform.

A small sample of the dataset might look like this:

| Customer | Age | City   | Sales |
| -------- | --: | ------ | ----: |
| Alice    |  24 | Mumbai |  2500 |
| Rahul    |   — | Delhi  |  1800 |
| Emma     |  29 | —      |  3200 |
| David    |  31 | Pune   |     — |

At first glance, the dataset appears normal. However, several values are missing.

If these missing values are ignored:

* Average age may be inaccurate.
* Sales reports may become misleading.
* Machine learning models may fail to train.
* Business decisions may be based on incomplete information.

Cleaning the dataset before analysis ensures that results are reliable and trustworthy.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Understand what missing values are.
* Identify missing values in a dataset.
* Count missing values in rows and columns.
* Understand different types of missing data.
* Decide when to remove or retain incomplete records.
* Build cleaner datasets for further analysis.

---

# 4. Understanding Missing Values

A **missing value** represents data that is unavailable, unknown, or not recorded.

In Pandas, missing values are commonly represented as:

* `NaN` (Not a Number)
* `None`
* Empty cells (depending on the data source)

Example:

| Employee | Salary | Department |
| -------- | -----: | ---------- |
| Alice    |  50000 | HR         |
| Rahul    |    NaN | Sales      |
| Emma     |  72000 | IT         |
| David    |  68000 | NaN        |

Missing values can occur for many reasons:

* Human error during data entry
* Survey participants skipping questions
* Sensor or hardware failures
* Data corruption
* Errors during data collection

Recognizing missing values is the first step toward cleaning a dataset.

---

# 5. Types of Missing Data

Not all missing values are the same. Understanding why data is missing helps determine the best cleaning strategy.

## Missing Completely at Random (MCAR)

The missing values have no relationship with any other variable.

Example:

A random internet interruption prevents a few customer records from being uploaded.

---

## Missing at Random (MAR)

The missing values depend on another variable.

Example:

Older customers are less likely to provide their email addresses.

---

## Missing Not at Random (MNAR)

The missing values depend on the missing value itself.

Example:

Customers with very high salaries intentionally choose not to disclose their income.

MNAR is the most difficult type of missing data to handle because the missingness itself contains information.

---

# 6. Detecting Missing Values

Suppose we load an employee dataset.

```python
import pandas as pd

df = pd.read_csv("employees.csv")
```

To identify missing values:

```python
df.isnull()
```

Example Output:

| Employee | Salary | Department |
| -------- | ------ | ---------- |
| False    | False  | False      |
| False    | True   | False      |
| False    | False  | False      |
| False    | False  | True       |

Each **True** value indicates a missing entry.

---

# 7. Counting Missing Values

Instead of viewing every missing value individually, analysts usually count how many missing values exist in each column.

```python
df.isnull().sum()
```

Example Output:

```text
Employee      0
Salary        1
Department    1
dtype: int64
```

This tells us:

* Employee → No missing values
* Salary → 1 missing value
* Department → 1 missing value

This quick summary helps identify which columns require attention before analysis.

---

# Key Takeaways

By this point, you should understand:

* What missing values are.
* Why missing values occur.
* The three major categories of missing data.
* How to detect missing values using `isnull()`.
* How to count missing values using `isnull().sum()`.

These skills are the first step in building clean, reliable datasets.

> **"A dataset is only as valuable as its quality. Before searching for insights, ensure your data can be trusted."**

