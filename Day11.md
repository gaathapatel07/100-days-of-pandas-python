# Day 11 — Pivot Tables & Cross Tabulations in Pandas

<div align="center">

# 100 Days of Pandas

### Day 11 · Summarizing Data Like a Business Analyst

*"Raw data tells individual stories. Pivot tables reveal the bigger picture."*

![Difficulty](https://img.shields.io/badge/Difficulty-Intermediate-yellow)
![Topic](https://img.shields.io/badge/Topic-Pivot%20Tables-blue)
![Day](https://img.shields.io/badge/Day-11-orange)

</div>

---

# Table of Contents

1. Introduction
2. Why Pivot Tables Matter
3. Learning Objectives
4. What is a Pivot Table?
5. Understanding `pivot()`
6. Understanding `pivot_table()`
7. Aggregation in Pivot Tables
8. Summary

---

# 1. Introduction

Businesses collect millions of rows of transactional data every day.

Reading these records individually provides very little insight.

Managers usually ask summary-based questions such as:

* Which region generated the highest revenue?
* Which product category is the most profitable?
* What are the monthly sales trends?
* Which customer segment contributes the most orders?

Answering these questions efficiently requires summarizing large datasets.

One of the most powerful tools for this purpose is the **Pivot Table**.

A Pivot Table reorganizes raw data into meaningful summaries by grouping, aggregating, and restructuring information.

---

# 2. Why Pivot Tables Matter

Imagine an online retailer has recorded **500,000 orders**.

Instead of viewing every transaction, management wants a report like this:

| Region | Total Sales |
| ------ | ----------: |
| North  |  ₹12,45,000 |
| South  |  ₹15,82,000 |
| East   |  ₹10,18,000 |
| West   |  ₹17,31,000 |

Creating this summary manually would be difficult.

Using a Pivot Table, the report can be generated in a single line of code.

Pivot tables are widely used in:

* Sales Analysis
* HR Reporting
* Finance
* Inventory Management
* Marketing Analytics
* Business Intelligence

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Understand the purpose of Pivot Tables.
* Create Pivot Tables using Pandas.
* Apply aggregation functions.
* Summarize multiple dimensions simultaneously.
* Build Excel-like analytical reports.
* Perform cross-tabulation analysis.

---

# 4. What is a Pivot Table?

A Pivot Table transforms detailed records into summarized information.

Suppose we have the following sales dataset:

| Region | Category   | Sales |
| ------ | ---------- | ----: |
| North  | Furniture  |  5000 |
| South  | Furniture  |  7000 |
| North  | Technology |  6200 |
| West   | Furniture  |  8500 |
| South  | Technology |  9200 |

Instead of reading each row, management wants to know:

* Total sales by region
* Average sales by category
* Sales for each region-category combination

Pivot Tables answer these questions quickly and clearly.

---

# 5. Understanding `pivot()`

The `pivot()` function reshapes data **without performing aggregation**.

### Syntax

```python
df.pivot(
    index="Row",
    columns="Column",
    values="Value"
)
```

Example:

```python
df.pivot(
    index="Region",
    columns="Category",
    values="Sales"
)
```

Output:

| Region | Furniture | Technology |
| ------ | --------: | ---------: |
| North  |      5000 |       6200 |
| South  |      7000 |       9200 |
| West   |      8500 |        NaN |

Notice:

* Each Region becomes a row.
* Each Category becomes a column.
* Sales become the cell values.

---

## Limitation of `pivot()`

`pivot()` requires that each combination of row and column values be **unique**.

Consider this dataset:

| Region | Category  | Sales |
| ------ | --------- | ----: |
| North  | Furniture |  5000 |
| North  | Furniture |  6200 |

Running:

```python
df.pivot(
    index="Region",
    columns="Category",
    values="Sales"
)
```

produces:

```text
ValueError:
Index contains duplicate entries.
Cannot reshape.
```

When duplicate combinations exist, use **`pivot_table()`** instead.

---

# 6. Understanding `pivot_table()`

Unlike `pivot()`, `pivot_table()` can handle duplicate records because it performs aggregation.

Example:

```python
pd.pivot_table(
    df,
    values="Sales",
    index="Region",
    columns="Category",
    aggfunc="sum"
)
```

Output:

| Region | Furniture | Technology |
| ------ | --------: | ---------: |
| North  |     11200 |       6200 |
| South  |      7000 |       9200 |

Duplicate rows are automatically summarized.

---

# 7. Aggregation Functions

`pivot_table()` supports multiple aggregation functions.

### Sum

```python
aggfunc="sum"
```

Calculates total sales.

---

### Mean

```python
aggfunc="mean"
```

Calculates average sales.

---

### Count

```python
aggfunc="count"
```

Counts records.

---

### Maximum

```python
aggfunc="max"
```

Finds the highest value.

---

### Minimum

```python
aggfunc="min"
```

Finds the smallest value.

---

### Example

```python
pd.pivot_table(
    df,
    values="Profit",
    index="Region",
    aggfunc=["sum", "mean", "max"]
)
```

This produces multiple business metrics in a single report.

---

# Key Takeaways

After completing this section, you should understand:

* The purpose of Pivot Tables.
* The difference between `pivot()` and `pivot_table()`.
* Why duplicate records require `pivot_table()`.
* How aggregation functions summarize business data.
* How Pivot Tables simplify large datasets.

> **"Pivot Tables transform thousands of rows into a report that decision-makers can understand in seconds."**

# 8. Multi-Level Pivot Tables

Business reports often need to summarize data across more than one dimension.

For example:

* Sales by **Region** and **Customer Segment**
* Profit by **Category** and **Sub-Category**
* Revenue by **Year** and **Quarter**

Pandas allows multiple fields in both the **index** and **columns**.

---

## Example

Suppose the dataset contains:

| Region | Segment   | Sales |
| ------ | --------- | ----: |
| North  | Consumer  |  5200 |
| North  | Corporate |  6100 |
| South  | Consumer  |  7300 |
| South  | Corporate |  8100 |

Create a pivot table:

```python id="p81wd7"
pd.pivot_table(
    df,
    values="Sales",
    index=["Region","Segment"],
    aggfunc="sum"
)
```

### Output

| Region | Segment   | Sales |
| ------ | --------- | ----: |
| North  | Consumer  |  5200 |
| North  | Corporate |  6100 |
| South  | Consumer  |  7300 |
| South  | Corporate |  8100 |

This hierarchical structure provides more detailed business summaries.

---

# 9. Multiple Aggregation Functions

Often, managers need more than one statistic.

Instead of creating separate reports, Pandas allows multiple aggregations.

```python id="s8av6n"
pd.pivot_table(
    df,
    values="Sales",
    index="Region",
    aggfunc=[
        "sum",
        "mean",
        "max",
        "min"
    ]
)
```

### Output

| Region |   Sum | Mean |  Max |  Min |
| ------ | ----: | ---: | ---: | ---: |
| North  | 11300 | 5650 | 6100 | 5200 |
| South  | 15400 | 7700 | 8100 | 7300 |

This produces a compact summary suitable for business reporting.

---

# 10. Replacing Missing Values

Sometimes certain combinations do not exist.

Example:

| Region | Furniture | Technology |
| ------ | --------: | ---------: |
| North  |      5200 |       6100 |
| South  |      7300 |        NaN |

Instead of displaying `NaN`, replace missing values.

```python id="n6zh1g"
pd.pivot_table(
    df,
    values="Sales",
    index="Region",
    columns="Category",
    fill_value=0
)
```

Output:

| Region | Furniture | Technology |
| ------ | --------: | ---------: |
| North  |      5200 |       6100 |
| South  |      7300 |          0 |

This creates cleaner reports and avoids confusion.

---

# 11. Adding Grand Totals

Business reports often include totals.

Enable totals using:

```python id="g7jk4r"
pd.pivot_table(
    df,
    values="Sales",
    index="Region",
    aggfunc="sum",
    margins=True
)
```

Output:

| Region  | Sales |
| ------- | ----: |
| North   | 11300 |
| South   | 15400 |
| West    |  9800 |
| **All** | 36500 |

The **All** row represents the overall total.

This feature is similar to Excel Pivot Tables.

---

# 12. Cross Tabulation

While Pivot Tables summarize numerical values, **Cross Tabs** summarize the frequency of categorical variables.

Pandas provides the `crosstab()` function.

Example:

```python id="f3g7kh"
pd.crosstab(
    df["Region"],
   
```
Cross tabulation helps analysts understand the relationship between two or more categorical variables.

Unlike `pivot_table()`, which summarizes numerical values, `crosstab()` counts occurrences.

### Example

Suppose we have customer data:

| Region | Customer Segment |
| ------ | ---------------- |
| North  | Consumer         |
|        |                  |
| North | Consumer |
| North | Corporate |
| South | Consumer |
| South | Consumer |
| West | Home Office |
| West | Corporate |

Using `crosstab()`:

```python
pd.crosstab(
    df["Region"],
    df["Customer Segment"]
)
```

### Output

| Region | Consumer | Corporate | Home Office |
| ------ | -------: | --------: | ----------: |
| North  |        1 |         1 |           0 |
| South  |        2 |         0 |           0 |
| West   |        0 |         1 |           1 |

This table quickly answers questions like:

* Which customer segment dominates each region?
* Which region has the most Consumer customers?
* Are certain customer segments concentrated in specific regions?

---

# 13. Cross Tabulation with Percentages

Sometimes percentages provide more insight than raw counts.

Use the `normalize` parameter.

```python
pd.crosstab(
    df["Region"],
    df["Customer Segment"],
    normalize="index"
)
```

### Output

| Region | Consumer | Corporate | Home Office |
| ------ | -------: | --------: | ----------: |
| North  |      50% |       50% |          0% |
| South  |     100% |        0% |          0% |
| West   |       0% |       50% |         50% |

This shows the proportion of each customer segment within every region.

---

# 14. Real-World Business Case Study

## Scenario

You are working as a **Business Data Analyst** for **RetailHub**, a nationwide retail chain.

The executive team wants a quarterly performance report.

You receive a sales dataset containing:

* Order ID
* Region
* Customer Segment
* Category
* Sub-Category
* Sales
* Profit
* Quantity

Your manager asks you to prepare executive summaries without manually calculating totals.

---

## Business Questions

### Question 1

What is the total sales generated by each region?

```python
pd.pivot_table(
    df,
    values="Sales",
    index="Region",
    aggfunc="sum"
)
```

---

### Question 2

What is the average profit by product category?

```python
pd.pivot_table(
    df,
    values="Profit",
    index="Category",
    aggfunc="mean"
)
```

---

### Question 3

Show sales by Region and Customer Segment.

```python
pd.pivot_table(
    df,
    values="Sales",
    index="Region",
    columns="Customer Segment",
    aggfunc="sum"
)
```

---

### Question 4

Add overall totals.

```python
pd.pivot_table(
    df,
    values="Sales",
    index="Region",
    aggfunc="sum",
    margins=True
)
```

---

### Question 5

Count customers in each Region and Segment.

```python
pd.crosstab(
    df["Region"],
    df["Customer Segment"]
)
```

---

## Business Insights

After completing the analysis, you discover:

* The **West** region contributes the highest total sales.
* The **Technology** category generates the highest average profit.
* The **Consumer** segment dominates in every region.
* The **South** region has relatively fewer corporate customers.
* Overall company revenue exceeds ₹3.5 crore.

These summaries help management make informed decisions regarding inventory planning, marketing strategy, and regional expansion.

---

# 15. Practice Exercises

## Beginner

1. Create a Pivot Table showing total sales by region.
2. Calculate average salary by department.
3. Generate a Pivot Table using the maximum sales value.
4. Display sales by category.
5. Count records using `pivot_table()`.

---

## Intermediate

6. Create a Pivot Table with two index columns.
7. Use multiple aggregation functions.
8. Replace missing values with `fill_value`.
9. Add grand totals using `margins=True`.
10. Build a cross-tabulation between Region and Customer Segment.

---

## Advanced

11. Analyze Region, Category, and Segment simultaneously.
12. Compare Pivot Tables and GroupBy outputs.
13. Generate percentage-based Crosstabs.
14. Create a management-ready summary report.
15. Write five business recommendations based on the Pivot Table.

---

# 16. Interview Questions

## Beginner

1. What is a Pivot Table?
2. Difference between `pivot()` and `pivot_table()`?
3. Why does `pivot()` fail with duplicate values?
4. What is `crosstab()` used for?
5. What is an aggregation function?

---

## Intermediate

6. Difference between `groupby()` and `pivot_table()`?
7. Why use `fill_value`?
8. What does `margins=True` do?
9. When should you use `crosstab()` instead of `pivot_table()`?
10. How can multiple aggregation functions improve business reporting?

---

## Advanced

11. Explain a real-world use case for Pivot Tables.
12. Compare Excel Pivot Tables with Pandas Pivot Tables.
13. How would you summarize regional performance using a Pivot Table?
14. How can Pivot Tables support executive dashboards?
15. Describe a complete reporting workflow using `pivot_table()` and `crosstab()`.

---

# 17. Cheat Sheet

| Operation              | Syntax                        |
| ---------------------- | ----------------------------- |
| Simple Pivot           | `df.pivot()`                  |
| Pivot Table            | `pd.pivot_table()`            |
| Multiple Index         | `index=["Region","Category"]` |
| Multiple Aggregations  | `aggfunc=["sum","mean"]`      |
| Replace Missing Values | `fill_value=0`                |
| Grand Totals           | `margins=True`                |
| Cross Tabulation       | `pd.crosstab()`               |
| Percentage Crosstab    | `normalize="index"`           |

---

# 18. Mini Project

## Executive Sales Summary Dashboard

Using a retail or e-commerce dataset:

Perform the following tasks:

* Create a Pivot Table showing sales by region.
* Create another Pivot Table showing profit by category.
* Build a multi-level Pivot Table using Region and Customer Segment.
* Add grand totals.
* Replace missing values.
* Create a Crosstab for Region and Customer Segment.
* Calculate percentage distributions.
* Write **five executive-level business insights**.
* Export the summary tables to CSV files.

Example insights:

* The West region contributes the highest overall revenue.
* Technology products generate the strongest average profit.
* Consumer customers account for the majority of orders.
* Home Office customers are concentrated in the East region.
* Sales performance varies significantly between regions, suggesting opportunities for targeted marketing.

---

# 19. Summary

Congratulations! 🎉

Today you learned how to transform large transactional datasets into concise business reports using **Pivot Tables** and **Cross Tabulations**.

You explored:

* Reshaping data with `pivot()`.
* Summarizing data using `pivot_table()`.
* Applying multiple aggregation functions.
* Building multi-level Pivot Tables.
* Handling missing values.
* Adding grand totals.
* Performing categorical analysis with `crosstab()`.

These tools are fundamental in business intelligence and are widely used in Excel, Power BI, Tableau, SQL, and Pandas workflows.

---

# 20. What's Next?

In **Day 12**, you'll learn **Advanced Data Cleaning & String Operations**, including:

* `str.lower()`
* `str.upper()`
* `str.title()`
* `str.strip()`
* `str.replace()`
* `str.contains()`
* Regular Expressions (Regex)
* Cleaning messy real-world text data

Since text data is rarely clean in real datasets, mastering string operations is an essential skill for every data analyst.

---

<div align="center">

# Day 11 Complete!

You've mastered **Pivot Tables & Cross Tabulations**, one of the most valuable reporting techniques in data analytics.

You can now transform millions of rows of raw data into executive-ready summaries in just a few lines of Pandas code.


</div>
