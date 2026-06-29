# Day 08 — Exploring Categorical Data with `value_counts()`, `unique()` & `nunique()`

<div align="center">

# 100 Days of Pandas

### Day 08 · Understanding Categories and Frequency Distributions

*"Before analyzing numbers, understand the categories they belong to."*

![Difficulty](https://img.shields.io/badge/Difficulty-Intermediate-yellow)
![Topic](https://img.shields.io/badge/Topic-Categorical%20Data-blue)
![Day](https://img.shields.io/badge/Day-08-orange)

</div>

---

# Table of Contents

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

# 8. Understanding Frequency Distribution

A **frequency distribution** shows how often each unique value appears in a dataset.

Instead of reading thousands of rows individually, analysts summarize the occurrence of each category.

Consider the following customer dataset:

| Customer ID | Region |
| ----------: | ------ |
|         101 | North  |
|         102 | South  |
|         103 | North  |
|         104 | West   |
|         105 | East   |
|         106 | North  |

Using:

```python id="a7t2vr"
df["Region"].value_counts()
```

Output:

| Region | Frequency |
| ------ | --------: |
| North  |         3 |
| South  |         1 |
| West   |         1 |
| East   |         1 |

This immediately tells us that **North** has the highest number of customers.

---

# 9. Calculating Percentage Distribution

Raw counts are useful, but percentages often provide more meaningful business insights.

Use the `normalize=True` parameter.

```python id="q3mf6k"
df["Region"].value_counts(normalize=True)
```

Output:

| Region | Percentage |
| ------ | ---------: |
| North  |      50.0% |
| South  |      16.7% |
| West   |      16.7% |
| East   |      16.7% |

Percentages make comparisons easier, especially when datasets differ in size.

---

# 10. Sorting Frequency Tables

By default, `value_counts()` sorts values in descending order.

Example:

```python id="r4je8m"
df["Department"].value_counts()
```

If you want alphabetical order instead, use:

```python id="j1vn0x"
df["Department"].value_counts().sort_index()
```

Output:

| Department | Count |
| ---------- | ----: |
| Finance    |    18 |
| HR         |    25 |
| IT         |    42 |
| Sales      |    31 |

Sorting alphabetically is useful when preparing reports or comparing categories.

---

# 11. Including Missing Values

By default, `value_counts()` ignores missing values.

Suppose the dataset contains:

| Department |
| ---------- |
| HR         |
| IT         |
| NaN        |
| Sales      |
| HR         |

Using:

```python id="u7op2c"
df["Department"].value_counts()
```

The missing value is excluded.

To include missing values:

```python id="a8yt4p"
df["Department"].value_counts(dropna=False)
```

Output:

| Department | Count |
| ---------- | ----: |
| HR         |     2 |
| IT         |     1 |
| Sales      |     1 |
| NaN        |     1 |

This helps analysts understand the completeness of categorical data.

---

# 12. Combining `value_counts()` with Other Functions

You can combine `value_counts()` with other Pandas methods.

### Display the Top Three Categories

```python id="p5x1mr"
df["Category"].value_counts().head(3)
```

---

### Display the Least Frequent Categories

```python id="b0lm8s"
df["Category"].value_counts().tail(3)
```

---

### Convert Frequency Table into a DataFrame

```python id="c9wa7f"
df["Region"].value_counts().reset_index()
```

Output:

| Region | Count |
| ------ | ----: |
| West   |   245 |
| South  |   198 |
| North  |   176 |
| East   |   154 |

This format is useful for exporting reports or creating charts.

---

# 13. Business Examples

## Example 1 – Customer Segments

```python id="d6ke9h"
df["Customer Segment"].value_counts()
```

Business Question:

**Which customer segment contributes the most orders?**

---

## Example 2 – Payment Methods

```python id="e3tr8n"
df["Payment Method"].value_counts(normalize=True)
```

Business Question:

**Which payment method is most preferred by customers?**

---

## Example 3 – Product Categories

```python id="f4yu0q"
df["Category"].nunique()
```

Business Question:

**How many product categories does the company offer?**

---

## Example 4 – Sales Regions

```python id="g2nm5w"
df["Region"].unique()
```

Business Question:

**Which geographical regions does the business operate in?**

---

# Best Practices

✔ Use `value_counts()` during the early stages of EDA.

✔ Check for inconsistent category names such as:

```text id="q1pf9v"
North
north
NORTH
```

✔ Use percentages when presenting findings to stakeholders.

✔ Include missing values during data quality assessment.

✔ Convert frequency tables into DataFrames before exporting or visualizing.

---

# Common Mistakes

### Assuming Similar Names Are Different Categories

For example:

```text id="l8xt5y"
HR
hr
Hr
```

These represent the same department but will be counted separately.

Always clean categorical values before analysis.

---

### Forgetting Missing Values

```python id="s6pw1r"
df["Department"].value_counts()
```

This excludes missing entries.

If data completeness is important:

```python id="y7ev2u"
df["Department"].value_counts(dropna=False)
```

---

# Quick Recap

You have now learned how to:

* Generate frequency distributions.
* Calculate percentage distributions.
* Sort category counts.
* Include missing values.
* Combine `value_counts()` with other Pandas functions.
* Interpret categorical summaries for business analysis.

> **"Frequency distributions reveal the structure of a dataset, helping analysts understand what is common, what is rare, and what deserves further investigation."**

> **"Understanding the categories within your data is often the first step toward understanding the business itself."**

# 14. Real-World Business Case Study

## Scenario

You have recently joined **RetailHub**, a fast-growing e-commerce company, as a Junior Data Analyst.

The marketing team wants to better understand its customer base before launching a nationwide promotional campaign.

You receive a dataset containing:

* Customer ID
* Region
* Customer Segment
* Product Category
* Payment Method
* Order Priority
* Sales

Your task is to analyze the categorical columns and prepare a summary report.

---

## Business Questions

### Question 1

How many unique customer segments are present?

```python id="k0b7xj"
df["Customer Segment"].nunique()
```

---

### Question 2

List all available payment methods.

```python id="i9m4fd"
df["Payment Method"].unique()
```

---

### Question 3

Which payment method is used most frequently?

```python id="m4qe2r"
df["Payment Method"].value_counts()
```

---

### Question 4

Calculate the percentage distribution of customer segments.

```python id="f1vy5m"
df["Customer Segment"].value_counts(normalize=True) * 100
```

---

### Question 5

Include missing values while counting regions.

```python id="a6sw9z"
df["Region"].value_counts(dropna=False)
```

---

## Business Insights

After performing the analysis, you discover:

* The **Consumer** segment contributes nearly half of all orders.
* **UPI** is the most frequently used payment method.
* Most customers belong to the **West** region.
* A few records contain missing region information that should be cleaned.
* Certain category names have inconsistent capitalization, indicating data quality issues.

These findings help the marketing and operations teams improve customer targeting, reporting accuracy, and data quality.

---

# 15. Practice Exercises

## Beginner

1. Display all unique departments.
2. Count the number of unique cities.
3. Generate a frequency table for the Region column.
4. Display the top three product categories.
5. Calculate the percentage distribution of payment methods.

---

## Intermediate

6. Sort category counts alphabetically.
7. Include missing values in the frequency table.
8. Identify the least common customer segment.
9. Convert a frequency table into a DataFrame.
10. Detect inconsistent category names.

---

## Advanced

11. Analyze three categorical columns and summarize their distributions.
12. Compare category percentages across different datasets.
13. Clean inconsistent category labels and regenerate the frequency table.
14. Create a report highlighting the top five categories in multiple columns.
15. Write five business recommendations based on the frequency analysis.

---

# 16. Interview Questions

## Beginner

1. What is categorical data?
2. What does `unique()` return?
3. What is the purpose of `nunique()`?
4. How does `value_counts()` work?
5. Why is categorical analysis important?

---

## Intermediate

6. Why does `value_counts()` ignore missing values by default?
7. What does `normalize=True` do?
8. Difference between `unique()` and `nunique()`?
9. How can `sort_index()` improve reports?
10. When should percentages be preferred over raw counts?

---

## Advanced

11. How can inconsistent category names affect business analysis?
12. Describe a real-world use case of `value_counts()`.
13. How would you analyze customer segments for a marketing campaign?
14. Explain why data cleaning is important before frequency analysis.
15. How can categorical analysis support strategic business decisions?

---

# 17. Cheat Sheet

| Function                       | Purpose                            |
| ------------------------------ | ---------------------------------- |
| `unique()`                     | Display all unique values          |
| `nunique()`                    | Count unique values                |
| `value_counts()`               | Count occurrences of each category |
| `value_counts(normalize=True)` | Calculate percentage distribution  |
| `sort_index()`                 | Sort categories alphabetically     |
| `head()`                       | Display the top records            |
| `tail()`                       | Display the last records           |
| `reset_index()`                | Convert results into a DataFrame   |
| `dropna=False`                 | Include missing values in counts   |

---

# 18. Mini Project

## Customer Segmentation Report

Using any customer or retail dataset:

Perform the following tasks:

* Identify all categorical columns.
* Count unique values in each categorical column.
* Generate frequency tables.
* Calculate percentage distributions.
* Include missing values where appropriate.
* Identify inconsistent category names.
* Create a clean summary report.
* Write **five business insights** based on your findings.

Example insights:

* The Consumer segment contributes the highest percentage of sales.
* Credit Card and UPI account for over 80% of transactions.
* The West region has the largest customer base.
* Certain categories require standardization due to inconsistent naming.
* Missing values are concentrated in the Region column.

---

# 19. Summary

Congratulations! 🎉

Today you learned how to analyze categorical data using Pandas.

You explored:

* Understanding categorical variables.
* Finding unique values with `unique()`.
* Counting categories with `nunique()`.
* Generating frequency distributions using `value_counts()`.
* Calculating percentage distributions.
* Including missing values in frequency tables.
* Identifying inconsistencies in categorical data.

These techniques are essential for exploratory data analysis and help analysts quickly understand the composition and quality of a dataset.

---

# 20. What's Next?

In **Day 09**, you'll learn how to work with **dates and time** in Pandas.

Topics include:

* `to_datetime()`
* Extracting Year, Month, Day
* Working with Hours and Minutes
* Filtering by Date
* Date Arithmetic
* Time Series Basics
* Business Calendar Analysis

Dates appear in almost every business dataset, making datetime manipulation one of the most valuable skills for any data analyst.

---

<div align="center">

# 🎉 Day 08 Complete!

You now know how to explore, summarize, and interpret categorical data using Pandas.

These techniques are used extensively in customer analytics, HR reporting, sales analysis, finance, healthcare, and business intelligence.

⭐ Keep the momentum going!



</div>
