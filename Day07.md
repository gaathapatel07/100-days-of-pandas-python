# Day 07 — Grouping & Aggregation with Pandas

<div align="center">

# 100 Days of Pandas

### Day 07 · Summarizing Data for Business Insights

*"Raw data becomes valuable only when it is summarized into meaningful information."*

![Difficulty](https://img.shields.io/badge/Difficulty-Intermediate-yellow)
![Topic](https://img.shields.io/badge/Topic-Grouping%20%26%20Aggregation-blue)
![Day](https://img.shields.io/badge/Day-07-orange)

</div>

---

# Table of Contents

1. Introduction
2. Why Grouping Matters
3. Learning Objectives
4. What is `groupby()`?
5. Understanding Groups
6. Aggregation Functions
7. Summary

---

# 1. Introduction

So far, you've learned how to load data, clean it, explore it, visualize it, and calculate statistics.

However, analysts rarely report individual rows. Instead, they summarize data to answer business questions.

For example:

* Which region generated the highest sales?
* What is the average salary by department?
* Which product category is the most profitable?
* How many customers belong to each city?

These questions require **grouping** data and applying **aggregation functions**.

Grouping transforms thousands of rows into concise summaries that support business decisions.

---

# 2. Why Grouping Matters

Imagine an e-commerce dataset with **500,000 orders**.

Looking at every individual order is impractical.

Instead, management wants reports like:

| Region | Total Sales |
| ------ | ----------: |
| North  |  ₹5,200,000 |
| South  |  ₹4,300,000 |
| East   |  ₹3,800,000 |
| West   |  ₹6,100,000 |

This report is generated using **groupby()**.

Grouping allows analysts to summarize data by categories rather than examining individual records.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Understand the purpose of `groupby()`.
* Group datasets by one or multiple columns.
* Apply aggregation functions.
* Summarize business data efficiently.
* Interpret grouped results.
* Generate management-ready reports.

---

# 4. What is `groupby()`?

The `groupby()` function divides a dataset into groups based on one or more columns.

Once grouped, each group can be analyzed independently.

General syntax:

```python
df.groupby("Column")
```

This creates a **GroupBy object**, which can then be combined with aggregation functions.

Think of it as asking:

> "Group similar records together, then calculate something about each group."

---

# 5. Understanding Groups

Consider the following employee dataset:

| Employee | Department | Salary |
| -------- | ---------- | -----: |
| Alice    | HR         |  50000 |
| Rahul    | Sales      |  62000 |
| Emma     | IT         |  71000 |
| David    | Sales      |  68000 |
| Sophia   | IT         |  85000 |

Grouping by department:

```python
df.groupby("Department")
```

Internally, Pandas creates:

```text
HR
 └── Alice

Sales
 ├── Rahul
 └── David

IT
 ├── Emma
 └── Sophia
```

Each department becomes its own group.

Nothing is calculated yet.

Grouping simply organizes the data.

---

# 6. Aggregation Functions

After grouping, analysts usually summarize each group using aggregation functions.

The most common aggregation functions are:

| Function   | Description        |
| ---------- | ------------------ |
| `sum()`    | Total              |
| `mean()`   | Average            |
| `count()`  | Number of records  |
| `max()`    | Maximum value      |
| `min()`    | Minimum value      |
| `median()` | Middle value       |
| `std()`    | Standard deviation |

Example:

Calculate the average salary for each department.

```python
df.groupby("Department")["Salary"].mean()
```

Output:

| Department | Average Salary |
| ---------- | -------------: |
| HR         |          50000 |
| IT         |          78000 |
| Sales      |          65000 |

Now answer another business question.

Which department has the highest total salary?

```python
df.groupby("Department")["Salary"].sum()
```

Output:

| Department | Total Salary |
| ---------- | -----------: |
| HR         |        50000 |
| IT         |       156000 |
| Sales      |       130000 |

With just one line of code, thousands of employee records can be transformed into a clear management report.

---

# Business Example

Suppose you're analyzing supermarket sales.

Instead of reviewing every purchase, you want to know:

* Total sales by city.
* Average order value by customer segment.
* Highest-selling product category.
* Number of orders in each region.

Each of these reports begins with **groupby()**.

---

# Best Practices

✔ Choose meaningful grouping columns.

✔ Keep aggregated reports simple.

✔ Always label grouped outputs clearly.

✔ Verify grouped totals against the original dataset.

✔ Use grouping to answer business questions—not just to summarize numbers.

---

# Key Takeaways

By now, you should understand:

* Why grouping is essential in analytics.
* How `groupby()` organizes data.
* The difference between grouping and aggregation.
* Common aggregation functions.
* How grouped summaries support decision-making.

> **"Individual records tell stories. Grouped summaries reveal business trends."**

# 9. Multiple Aggregations using `agg()`

In business analytics, calculating a single statistic is rarely enough.

Managers often ask questions like:

* What is the **total** sales for each region?
* What is the **average** order value?
* What is the **highest** sale?
* What is the **lowest** sale?
* How many orders were placed?

Instead of running multiple `groupby()` operations, Pandas allows us to calculate several statistics simultaneously using the `agg()` function.

---

## Syntax

```python
df.groupby("Column")["Value"].agg(["sum","mean","max","min"])
```

---

## Example

Suppose we have the following dataset:

| Region | Sales |
| ------ | ----: |
| North  |  5000 |
| South  |  7000 |
| North  |  6500 |
| West   |  9000 |
| South  |  8000 |

```python
df.groupby("Region")["Sales"].agg(
    ["sum","mean","max","min","count"]
)
```

### Output

| Region |   Sum | Mean |  Max |  Min | Count |
| ------ | ----: | ---: | ---: | ---: | ----: |
| North  | 11500 | 5750 | 6500 | 5000 |     2 |
| South  | 15000 | 7500 | 8000 | 7000 |     2 |
| West   |  9000 | 9000 | 9000 | 9000 |     1 |

With a single command, we've generated a concise summary that answers multiple business questions.

---

# 10. Custom Aggregation Names

Default column names such as `sum` or `mean` are acceptable, but business reports often require more descriptive labels.

Pandas allows you to rename aggregated columns.

```python
df.groupby("Region").agg(
    Total_Sales=("Sales","sum"),
    Average_Sales=("Sales","mean"),
    Highest_Sale=("Sales","max"),
    Orders=("Sales","count")
)
```

### Output

| Region | Total Sales | Average Sales | Highest Sale | Orders |
| ------ | ----------: | ------------: | -----------: | -----: |
| North  |       11500 |          5750 |         6500 |      2 |
| South  |       15000 |          7500 |         8000 |      2 |
| West   |        9000 |          9000 |         9000 |      1 |

This format is cleaner and is commonly used when preparing reports for management.

---

# 11. Grouping by Multiple Columns

Real-world datasets usually contain multiple categories.

Suppose we want to analyze sales by both **Region** and **Category**.

Dataset:

| Region | Category   | Sales |
| ------ | ---------- | ----: |
| North  | Furniture  |  5000 |
| North  | Technology |  6500 |
| South  | Furniture  |  7000 |
| South  | Technology |  8000 |
| West   | Furniture  |  9000 |

Group by two columns:

```python
df.groupby(["Region","Category"])["Sales"].sum()
```

### Output

| Region | Category   | Sales |
| ------ | ---------- | ----: |
| North  | Furniture  |  5000 |
| North  | Technology |  6500 |
| South  | Furniture  |  7000 |
| South  | Technology |  8000 |
| West   | Furniture  |  9000 |

Grouping by multiple columns allows analysts to perform much more detailed business analysis.

---

# 12. Resetting the Index

After grouping, Pandas uses the grouped columns as the index.

Sometimes you may want a regular DataFrame instead.

```python
df.groupby("Region")["Sales"].sum().reset_index()
```

Output:

| Region | Sales |
| ------ | ----: |
| North  | 11500 |
| South  | 15000 |
| West   |  9000 |

Using `reset_index()` is especially useful when exporting data or creating visualizations.

---

# 13. Sorting Grouped Results

Once data has been aggregated, it's common to sort it.

For example, display regions with the highest sales first.

```python
summary = df.groupby("Region")["Sales"].sum()

summary.sort_values(ascending=False)
```

### Output

| Region | Sales |
| ------ | ----: |
| South  | 15000 |
| North  | 11500 |
| West   |  9000 |

This immediately highlights the top-performing region.

---

# 14. Business Example

Imagine you're working as a Data Analyst for an online retailer.

Your manager asks:

* Which region generates the highest revenue?
* Which product category has the highest average profit?
* Which department receives the most orders?

Instead of manually reviewing thousands of rows, you can answer these questions using a few `groupby()` operations.

For example:

```python
df.groupby("Category")["Profit"].mean()
```

or

```python
df.groupby("Region")["Orders"].sum()
```

These summaries support faster, data-driven business decisions.

---

# Best Practices

✔ Use meaningful column names after aggregation.

✔ Apply multiple aggregations with `agg()` whenever possible.

✔ Reset the index before exporting grouped data.

✔ Sort summarized results to highlight key insights.

✔ Keep grouped outputs simple and easy to interpret.

---

# Common Mistakes

### Forgetting `reset_index()`

Grouped results often have grouped columns as the index.

```python
df.groupby("Region")["Sales"].sum()
```

If you need a standard DataFrame:

```python
df.groupby("Region")["Sales"].sum().reset_index()
```

---

### Running Multiple GroupBy Operations

Instead of writing:

```python
df.groupby("Region")["Sales"].sum()

df.groupby("Region")["Sales"].mean()

df.groupby("Region")["Sales"].count()
```

Use:

```python
df.groupby("Region")["Sales"].agg(
    ["sum","mean","count"]
)
```

This is cleaner, faster, and easier to maintain.

---

# Quick Recap

After completing this section, you should be able to:

* Apply multiple aggregation functions.
* Use the `agg()` method effectively.
* Group data by multiple columns.
* Reset indexes after grouping.
* Sort summarized business reports.

> **"The true power of `groupby()` is revealed when multiple statistics are combined to answer complex business questions in a single operation."**

# 15. Real-World Business Case Study

## Scenario

You have joined **RetailHub**, a multinational retail company, as a Junior Data Analyst.

The management team wants to analyze sales performance across different regions and product categories before planning next quarter's strategy.

You are provided with a dataset containing:

* Order ID
* Region
* Category
* Customer Segment
* Sales
* Profit
* Quantity
* Discount

Your goal is to summarize the data and provide actionable business insights.

---

## Business Questions

### Question 1

What is the total sales generated by each region?

```python
df.groupby("Region")["Sales"].sum()
```

---

### Question 2

Which region has the highest average profit?

```python
df.groupby("Region")["Profit"].mean()
```

---

### Question 3

How many orders were placed in each customer segment?

```python
df.groupby("Customer Segment")["Order ID"].count()
```

---

### Question 4

Which category generated the highest total revenue?

```python
df.groupby("Category")["Sales"].sum().sort_values(ascending=False)
```

---

### Question 5

Generate a complete regional summary.

```python
df.groupby("Region").agg(
    Total_Sales=("Sales", "sum"),
    Average_Profit=("Profit", "mean"),
    Total_Orders=("Order ID", "count"),
    Highest_Order=("Sales", "max")
)
```

---

## Business Insights

From these summaries, you might conclude:

* The **West** region generated the highest revenue.
* The **Technology** category contributed the most sales.
* The **Corporate** customer segment placed the highest number of orders.
* The **South** region recorded the highest average profit per order.
* Certain regions have fewer orders but higher average order values.

These insights help management allocate marketing budgets, optimize inventory, and improve business strategy.

---

# 16. Practice Exercises

## Beginner

1. Calculate total sales by region.
2. Find the average salary by department.
3. Count the number of employees in each department.
4. Determine the maximum sales value by category.
5. Find the minimum profit for each region.

---

## Intermediate

6. Apply `sum()`, `mean()`, and `count()` together using `agg()`.
7. Group data by **Region** and **Category**.
8. Rename aggregated columns using Named Aggregations.
9. Sort grouped results in descending order.
10. Reset the index after grouping.

---

## Advanced

11. Identify the region with the highest total sales.
12. Determine which category has the highest average profit.
13. Create a summary report containing multiple business metrics.
14. Compare grouped results before and after sorting.
15. Write five business recommendations based on the grouped data.

---

# 17. Interview Questions

## Beginner

1. What is the purpose of `groupby()` in Pandas?
2. Explain the Split–Apply–Combine strategy.
3. What is an aggregation function?
4. Difference between `count()` and `size()`?
5. What does `reset_index()` do after grouping?

---

## Intermediate

6. Why is `agg()` preferred over multiple `groupby()` calls?
7. How do you group data using multiple columns?
8. What are Named Aggregations?
9. How do you sort grouped data?
10. Why should grouped reports use meaningful column names?

---

## Advanced

11. Explain a real-world use case of `groupby()`.
12. How can grouped summaries support business decision-making?
13. What are the advantages of grouping before visualization?
14. How would you analyze regional performance using Pandas?
15. Describe how `groupby()` is similar to SQL's `GROUP BY` clause.

---

# 18. Cheat Sheet

| Operation                 | Syntax                                                      |
| ------------------------- | ----------------------------------------------------------- |
| Group by one column       | `df.groupby("Region")`                                      |
| Sum                       | `df.groupby("Region")["Sales"].sum()`                       |
| Mean                      | `df.groupby("Region")["Sales"].mean()`                      |
| Count                     | `df.groupby("Region")["Sales"].count()`                     |
| Multiple aggregations     | `df.groupby("Region")["Sales"].agg(["sum","mean","count"])` |
| Named Aggregations        | `df.groupby("Region").agg(Total=("Sales","sum"))`           |
| Group by multiple columns | `df.groupby(["Region","Category"])`                         |
| Reset index               | `.reset_index()`                                            |
| Sort grouped results      | `.sort_values()`                                            |

---

# 19. Mini Project

## Regional Sales Performance Report

Using any retail or sales dataset:

Perform the following tasks:

* Group data by Region.
* Calculate total sales, average sales, and order count.
* Group data by Region and Category.
* Create a summarized business report using `agg()`.
* Sort regions by total sales.
* Reset the index.
* Write **five business insights** based on the grouped data.
* Export the final summary as a CSV file.

---

# 20. Summary

Congratulations! 🎉

Today you learned one of the most powerful features of Pandas: **Grouping and Aggregation**.

You explored:

* The Split–Apply–Combine strategy.
* Grouping data using one or multiple columns.
* Applying aggregation functions.
* Using `agg()` for multiple summaries.
* Named Aggregations for cleaner reports.
* Sorting and resetting grouped data.
* Converting raw transactional data into meaningful business summaries.

These techniques are used extensively in reporting, business intelligence, financial analysis, and dashboard creation.

---

# 21. What's Next?

In **Day 08**, you'll learn how to analyze categorical data and understand unique values using functions such as:

* `value_counts()`
* `unique()`
* `nunique()`
* `sort_index()`
* `sort_values()`
* Frequency distributions
* Percentage distributions

These functions help analysts quickly understand the composition of a dataset and are commonly used during exploratory data analysis.

---

<div align="center">

# 🎉 Day 07 Complete!

You have mastered one of the most valuable skills in Pandas—**summarizing data with GroupBy and Aggregation**.

These concepts are used daily by data analysts to answer business questions, generate reports, and prepare dashboards.

⭐ Great job! Keep the momentum going.



</div>
