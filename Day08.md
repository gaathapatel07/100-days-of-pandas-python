# 🐼 Day 08 — Exploring Categorical Data with `value_counts()`, `unique()` & `nunique()`

<div align="center">

# 100 Days of Pandas

### Day 08 · Understanding Categories and Frequency Distributions

*"Before analyzing numbers, understand the categories they belong to."*

![Difficulty](https://img.shields.io/badge/Difficulty-Intermediate-yellow)
![Topic](https://img.shields.io/badge/Topic-Categorical%20Data-blue)
![Day](https://img.shields.io/badge/Day-08-orange)

</div>

---

# 📚 Table of Contents

1. Introduction
2. Why Categorical Analysis Matters
3. Learning Objectives
4. Understanding Categorical Data
5. Exploring Unique Values
6. Counting Unique Values
7. Frequency Distribution with `value_counts()`
8. Summary

---

# 1. Introduction

In the previous lesson, you learned how to summarize numerical data using `groupby()` and aggregation functions.

However, not every column in a dataset contains numbers. Many datasets include **categorical variables** such as regions, departments, product categories, customer segments, payment methods, and cities.

Understanding these categories is one of the first steps in Exploratory Data Analysis (EDA). By examining how often each category appears, analysts can identify dominant groups, rare occurrences, and potential data quality issues.

This chapter introduces the Pandas functions that make categorical analysis simple and efficient.

---

# 2. Why Categorical Analysis Matters

Imagine you work for an e-commerce company.

Your manager asks questions such as:

* Which customer segment places the most orders?
* Which payment method is used most often?
* How many unique products are sold?
* Which city contributes the highest number of customers?
* Are there unexpected category names caused by data entry errors?

These questions cannot be answered using averages or sums. Instead, they require counting and exploring categories.

Categorical analysis provides a quick overview of the structure and composition of a dataset.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Differentiate between numerical and categorical data.
* Find unique values in a column.
* Count the number of unique values.
* Generate frequency distributions.
* Calculate category percentages.
* Sort frequency tables for better interpretation.

---

# 4. Understanding Categorical Data

A **categorical variable** represents labels or groups rather than numerical measurements.

Examples include:

| Column           | Example Values                   |
| ---------------- | -------------------------------- |
| Region           | North, South, East, West         |
| Department       | HR, IT, Sales                    |
| Gender           | Male, Female                     |
| Payment Method   | Cash, UPI, Credit Card           |
| Customer Segment | Consumer, Corporate, Home Office |

Unlike numerical variables, categorical variables describe qualities rather than quantities.

They help analysts answer questions about composition and distribution rather than calculations like averages.

---

# 5. Finding Unique Values

The `unique()` function returns all distinct values present in a column.

### Syntax

```python id="g4rm7m"
df["Region"].unique()
```

Example Output:

```text id="m8fx2v"
array([
'North',
'South',
'East',
'West'
])
```

This quickly shows every unique category in the **Region** column.

---

## Practical Example

```python id="jlwm1m"
df["Payment Method"].unique()
```

Possible Output:

```text id="j6lt1k"
array([
'Cash',
'Credit Card',
'UPI',
'Net Banking'
])
```

This helps analysts understand all available payment methods without manually scanning thousands of rows.

---

# 6. Counting Unique Values

Knowing the categories is useful, but analysts often want to know **how many unique categories** exist.

The `nunique()` function provides this count.

```python id="jlwm2n"
df["Region"].nunique()
```

Output:

```text id="4smyr2"
4
```

Another example:

```python id="jlwm3o"
df["Product Category"].nunique()
```

Output:

```text id="v6zbw1"
12
```

This tells us that the dataset contains **12 distinct product categories**.

---

# 7. Frequency Distribution with `value_counts()`

The `value_counts()` function counts how many times each category appears.

It is one of the most frequently used functions in Pandas.

### Example

```python id="jlwm4p"
df["Region"].value_counts()
```

Example Output:

| Region | Count |
| ------ | ----: |
| West   |   245 |
| South  |   198 |
| North  |   176 |
| East   |   154 |

This table immediately shows which regions contribute the most records.

---

## Why `value_counts()` is Useful

It helps answer questions like:

* Which category is most common?
* Which category is least common?
* Are there spelling inconsistencies?
* Is the data balanced?

For example, if the output contains:

```text id="3x2vbj"
North
north
NORTH
```

it indicates inconsistent data entry that should be cleaned before analysis.

---

# Key Takeaways

After completing this section, you should understand:

* The difference between numerical and categorical variables.
* How to retrieve unique values using `unique()`.
* How to count categories with `nunique()`.
* How to generate frequency tables using `value_counts()`.
* Why categorical analysis is an essential part of exploratory data analysis.

> **"Understanding the categories within your data is often the first step toward understanding the business itself."**

