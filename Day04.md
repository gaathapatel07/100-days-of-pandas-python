# Day 04 — Data Cleaning: Handling Missing Values & Duplicate Data

<div align="center">

# 100 Days of Pandas

### Day 04 · Cleaning Data for Accurate Analysis

*"Garbage in, garbage out. Clean data is the foundation of trustworthy insights."*

![Difficulty](https://img.shields.io/badge/Difficulty-Beginner-success)
![Topic](https://img.shields.io/badge/Topic-Data%20Cleaning-blue)
![Day](https://img.shields.io/badge/Day-04-orange)

</div>

---

# Table of Contents

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

---

# 8. Understanding `notnull()`

While `isnull()` helps identify missing values, there are many situations where you only want to work with rows that contain valid data.

The `notnull()` function returns **True** for existing values and **False** for missing values.

Consider the following dataset:

| Employee | Salary | Department |
| -------- | -----: | ---------- |
| Alice    |  50000 | HR         |
| Rahul    |    NaN | Sales      |
| Emma     |  72000 | IT         |
| David    |  68000 | NaN        |

Using `notnull()`:

```python
df.notnull()
```

Output:

| Employee | Salary | Department |
| -------- | ------ | ---------- |
| True     | True   | True       |
| True     | False  | True       |
| True     | True   | True       |
| True     | True   | False      |

This function is particularly useful when filtering only complete records.

Example:

```python
df[df["Salary"].notnull()]
```

This returns employees whose salary information is available.

---

# 9. Removing Missing Values using `dropna()`

Sometimes a dataset contains only a few missing values, making it easier to remove incomplete records instead of filling them.

Pandas provides the `dropna()` function for this purpose.

## Removing Rows with Missing Values

```python
df.dropna()
```

Example Output:

| Employee | Salary | Department |
| -------- | -----: | ---------- |
| Alice    |  50000 | HR         |
| Emma     |  72000 | IT         |

Rows containing at least one missing value are removed.

---

## Removing Columns with Missing Values

```python
df.dropna(axis=1)
```

Here:

* `axis=0` → Remove rows *(default)*
* `axis=1` → Remove columns

Be cautious when dropping columns, as valuable information may be lost.

---

## Removing Rows Only If All Values Are Missing

```python
df.dropna(how="all")
```

Only rows where **every column** is missing are removed.

---

## Removing Rows with a Threshold

Suppose you want to keep rows that contain at least **three non-missing values**.

```python
df.dropna(thresh=3)
```

This is useful for partially complete datasets where some missing values are acceptable.

---

# 10. Filling Missing Values using `fillna()`

Removing data is not always the best solution.

In many situations, analysts prefer to **replace missing values** instead of deleting records.

The `fillna()` function allows you to substitute missing values with meaningful alternatives.

---

## Filling with a Constant Value

Replace missing salaries with **0**.

```python
df["Salary"] = df["Salary"].fillna(0)
```

Example Output:

| Employee | Salary |
| -------- | -----: |
| Alice    |  50000 |
| Rahul    |      0 |
| Emma     |  72000 |

---

## Filling with Text

Replace missing departments.

```python
df["Department"] = df["Department"].fillna("Unknown")
```

Output:

| Employee | Department |
| -------- | ---------- |
| Alice    | HR         |
| Rahul    | Sales      |
| Emma     | IT         |
| David    | Unknown    |

Using descriptive labels often makes reports easier to interpret.

---

# 11. Filling with Mean

For numerical data, replacing missing values with the **mean** is a common approach.

```python
mean_salary = df["Salary"].mean()

df["Salary"] = df["Salary"].fillna(mean_salary)
```

Suppose the average salary is **₹63,500**.

The missing salary will be replaced with this value.

This method works well when the data is normally distributed and does not contain significant outliers.

---

# 12. Filling with Median

The **median** is often preferred when the dataset contains extreme values.

```python
median_salary = df["Salary"].median()

df["Salary"] = df["Salary"].fillna(median_salary)
```

Median is more resistant to unusually high or low values than the mean.

---

# 13. Filling with Mode

For categorical data, the **mode** (most frequent value) is commonly used.

```python
mode_department = df["Department"].mode()[0]

df["Department"] = df["Department"].fillna(mode_department)
```

Example:

If most employees belong to the **Sales** department, missing department values will be replaced with **Sales**.

---

# Which Method Should You Choose?

| Situation                             | Recommended Method |
| ------------------------------------- | ------------------ |
| Numerical data with no major outliers | Mean               |
| Numerical data with outliers          | Median             |
| Categorical data                      | Mode               |
| Unknown category                      | Constant value     |
| Very few missing records              | Drop rows          |
| Mostly empty column                   | Drop column        |

Choosing the right strategy depends on both the data and the business problem.

---

# Best Practices

✔ Always investigate why data is missing before filling or deleting it.

✔ Avoid replacing every missing value with **0**, as it may introduce misleading results.

✔ Use the **median** when dealing with skewed numerical data.

✔ Keep a copy of the original dataset before making changes.

✔ Document every cleaning step for reproducibility.

---

# Common Mistakes

### Filling All Columns with the Same Value

```python
df.fillna(0)
```

While this works, replacing text columns with `0` often produces unrealistic data.

It is usually better to fill each column according to its data type.

---

### Dropping Too Many Rows

```python
df.dropna()
```

This may remove a large portion of your dataset.

Always check how many records will be lost before using `dropna()`.

---

# Quick Recap

You now know how to:

* Detect valid values using `notnull()`.
* Remove incomplete rows or columns using `dropna()`.
* Replace missing values using `fillna()`.
* Fill numerical data with the mean or median.
* Fill categorical data with the mode.
* Select the most appropriate cleaning strategy for different situations.

> **"Cleaning data is not about removing imperfections—it is about making thoughtful decisions that preserve the integrity of your analysis."**
