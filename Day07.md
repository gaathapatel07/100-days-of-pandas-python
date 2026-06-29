# 🐼 Day 07 — Grouping & Aggregation with Pandas

<div align="center">

# 100 Days of Pandas

### Day 07 · Summarizing Data for Business Insights

*"Raw data becomes valuable only when it is summarized into meaningful information."*

![Difficulty](https://img.shields.io/badge/Difficulty-Intermediate-yellow)
![Topic](https://img.shields.io/badge/Topic-Grouping%20%26%20Aggregation-blue)
![Day](https://img.shields.io/badge/Day-07-orange)

</div>

---

# 📚 Table of Contents

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

