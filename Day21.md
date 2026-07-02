# Day 21 — Missing Data Handling & Advanced Data Cleaning

<div align="center">

# 100 Days of Pandas

### Day 21 · Cleaning Data for Reliable Analysis

*"Clean data is the foundation of every accurate insight, dashboard, and machine learning model."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Data%20Cleaning-blue)
![Day](https://img.shields.io/badge/Day-21-orange)

</div>

---


# Table of Contents

1. Introduction
2. Why Data Cleaning Matters
3. Learning Objectives
4. Understanding Missing Data
5. Detecting Missing Values
6. Removing Missing Values
7. Filling Missing Values
8. Summary

---

# 1. Introduction

In the real world, datasets are rarely perfect.

Missing values, duplicate records, incorrect formats, inconsistent spellings, and invalid entries are common challenges that analysts face daily.

Examples include:

* Customers who forgot to enter their phone numbers.
* Products with missing prices.
* Survey respondents skipping questions.
* Sensors temporarily failing to record readings.
* Employees with incomplete personal information.

Before performing analysis or building machine learning models, these issues must be identified and corrected.

Pandas provides a comprehensive set of tools for cleaning data efficiently.

---

# 2. Why Data Cleaning Matters

Imagine you're working for an e-commerce company.

The sales dataset contains:

| Order ID | Customer | City   | Sales |
| -------- | -------- | ------ | ----: |
| 1001     | Alice    | Delhi  |  5200 |
| 1002     | Rahul    | NaN    |  6100 |
| 1003     | Priya    | Mumbai |   NaN |
| 1004     | Arjun    | Delhi  |  7200 |

If missing values are ignored:

* Sales reports become inaccurate.
* Dashboards display incorrect KPIs.
* Machine learning models produce unreliable predictions.
* Business decisions become less trustworthy.

Cleaning the data ensures higher accuracy and consistency.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Detect missing values.
* Remove incomplete records.
* Fill missing values using different strategies.
* Understand when each method should be used.
* Build cleaner datasets for analysis.

---

# 4. Understanding Missing Data

Missing values represent unknown or unavailable information.

In Pandas, missing values are typically represented as:

```python
NaN
```

which stands for **Not a Number**.

Although originally designed for numerical data, `NaN` is commonly used to represent missing values in many data types.

Example:

| Name  | Age | Salary |
| ----- | --: | -----: |
| Alice |  25 |  52000 |
| Rahul | NaN |  61000 |
| Priya |  28 |    NaN |

Missing values require careful handling because many calculations ignore or propagate them.

---

# 5. Detecting Missing Values

## Using `isnull()`

Check whether values are missing.

```python id="null01"
df.isnull()
```

Output:

| Name  | Age   | Salary |
| ----- | ----- | ------ |
| False | False | False  |
| False | True  | False  |
| False | False | True   |

`True` indicates a missing value.

---

## Using `isna()`

`isna()` is identical to `isnull()`.

```python id="null02"
df.isna()
```

Both functions can be used interchangeably.

---

## Counting Missing Values

Calculate the number of missing values in each column.

```python id="null03"
df.isnull().sum()
```

Example Output:

| Column | Missing Values |
| ------ | -------------: |
| Age    |              1 |
| Salary |              1 |

This is one of the most frequently used commands during exploratory data analysis.

---

## Total Missing Values

Calculate the total number of missing values in the entire dataset.

```python id="null04"
df.isnull().sum().sum()
```

This provides an overall measure of data completeness.

---

## Percentage of Missing Values

Understanding percentages is often more useful than raw counts.

```python id="null05"
(
    df.isnull().sum()
    /
    len(df)
) * 100
```

Example Output:

| Column | Missing (%) |
| ------ | ----------: |
| Age    |        10.0 |
| Salary |         5.0 |

This helps prioritize which columns require attention.

---

# 6. Removing Missing Values

Sometimes incomplete records should be removed.

Use `dropna()`.

```python id="drop01"
df.dropna()
```

This removes every row containing at least one missing value.

---

## Removing Columns

Delete columns containing missing values.

```python id="drop02"
df.dropna(
    axis=1
)
```

Here:

* `axis=0` → Rows (default)
* `axis=1` → Columns

---

## Removing Rows with All Missing Values

```python id="drop03"
df.dropna(
    how="all"
)
```

Only rows where **every value** is missing are removed.

---

## Removing Rows Based on a Threshold

Keep rows that contain at least three non-missing values.

```python id="drop04"
df.dropna(
    thresh=3
)
```

Threshold-based filtering is useful when datasets contain partial information.

---

# 7. Filling Missing Values

Instead of deleting information, analysts often replace missing values.

## Fill with a Constant

```python id="fill01"
df.fillna(0)
```

Useful for:

* Missing quantities.
* Missing counts.
* Default numeric values.

---

## Fill Individual Columns

```python id="fill02"
df["City"] = (
    df["City"]
      .fillna("Unknown")
)
```

Categorical variables are often filled using descriptive labels.

---

## Fill with the Mean

```python id="fill03"
df["Salary"] = (
    df["Salary"]
      .fillna(
          df["Salary"].mean()
      )
)
```

Suitable for approximately symmetric numerical distributions.

---

## Fill with the Median

```python id="fill04"
df["Age"] = (
    df["Age"]
      .fillna(
          df["Age"].median()
      )
)
```

Median is preferred when outliers exist.

---

## Fill with the Mode

```python id="fill05"
df["City"] = (
    df["City"]
      .fillna(
          df["City"].mode()[0]
      )
)
```

Useful for categorical variables.

---

# Business Example

A healthcare company maintains patient records.

Several entries are incomplete because patients skipped optional questions.

Instead of removing these records entirely, analysts:

* Replace missing ages with the median age.
* Fill missing cities using the most common city.
* Replace missing test results only after consulting domain experts.

This preserves valuable information while improving data quality.

---

# Best Practices

✔ Investigate why data is missing before filling it.

✔ Use the median when numerical data contains outliers.

✔ Use the mode for categorical variables.

✔ Avoid deleting large amounts of valuable information.

✔ Always validate the cleaned dataset after modifications.

---

# Common Mistakes

### Filling Every Column with Zero

Incorrect:

```python id="mistake01"
df.fillna(0)
```

This may introduce unrealistic values for names, cities, or dates.

Instead, choose filling strategies appropriate for each column.

---

### Removing Too Much Data

Deleting rows indiscriminately can reduce the dataset and introduce bias.

Always evaluate the impact before using `dropna()`.

---


# Key Takeaways

After completing this section, you should understand:

* How to detect missing values.
* Different methods for removing incomplete records.
* Appropriate strategies for filling missing data.
* When to use the mean, median, or mode.
* Why careful data cleaning improves analytical accuracy.

> **"Effective data cleaning is not about removing imperfections—it is about making informed decisions that preserve the integrity and usefulness of the data."**

