
#  Day 03 — Sorting, Renaming & Transforming Data in Pandas

<div align="center">

# 100 Days of Pandas

### Day 03 · Organizing and Transforming Data Like a Data Analyst

*"Clean, well-structured data is the foundation of every meaningful analysis."*

![Difficulty](https://img.shields.io/badge/Difficulty-Beginner-success)
![Topic](https://img.shields.io/badge/Topic-Data%20Transformation-blue)
![Day](https://img.shields.io/badge/Day-03-orange)

</div>

---

#  Table of Contents

1. Introduction
2. Why Data Transformation Matters
3. Learning Objectives
4. Sorting Data
5. Sorting Multiple Columns
6. Ascending vs Descending Order
7. Renaming Columns
8. Creating New Columns
9. Updating Existing Columns
10. Dropping Columns
11. Setting & Resetting Index
12. Common Mistakes
13. Business Case Study
14. Practice Exercises
15. Interview Questions
16. Cheat Sheet
17. Mini Project
18. Summary
19. What's Next?

---

# 1. Introduction

In the previous lessons, you learned how to load datasets, inspect their structure, and retrieve specific rows and columns using powerful indexing and filtering techniques.

However, real-world datasets are rarely ready for analysis immediately. Before building reports or creating visualizations, analysts often need to reorganize, rename, and transform their data into a cleaner and more meaningful format.

Data transformation improves readability, simplifies analysis, and prepares datasets for advanced operations such as aggregation, visualization, and machine learning.

Today's lesson focuses on organizing data efficiently using sorting, renaming, creating new columns, and working with indexes.

---

# 2. Why Data Transformation Matters

Imagine you're working for an online retail company with a dataset containing thousands of customer orders.

Your manager asks you to answer questions like:

* Which products generated the highest revenue?
* Which customers placed the largest orders?
* Can you calculate the total amount after applying discounts?
* Can you rename technical column names before sharing the report with management?

These tasks require more than just selecting rows—they require transforming the dataset into a format that is easier to understand and analyze.

This is where Pandas becomes an incredibly powerful tool.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Sort datasets using one or multiple columns.
* Arrange data in ascending and descending order.
* Rename columns for better readability.
* Create new columns using existing data.
* Update existing column values.
* Remove unnecessary columns.
* Set and reset indexes effectively.
* Prepare datasets for deeper analysis.

---

# 4. Sorting Data

Sorting helps arrange records in a meaningful order, making it easier to identify trends, top performers, and outliers.

Suppose we have the following employee dataset:

| Employee | Department | Salary |
| -------- | ---------- | -----: |
| Alice    | HR         |  50000 |
| Rahul    | Sales      |  62000 |
| Emma     | IT         |  71000 |
| David    | Finance    |  68000 |
| Sophia   | IT         |  85000 |

To sort employees by salary in ascending order:

```python
df.sort_values(by="Salary")
```

Output:

| Employee | Salary |
| -------- | -----: |
| Alice    |  50000 |
| Rahul    |  62000 |
| David    |  68000 |
| Emma     |  71000 |
| Sophia   |  85000 |

By default, Pandas sorts values in **ascending order**.

---

# 5. Sorting in Descending Order

To display the highest values first, use the `ascending=False` parameter.

```python
df.sort_values(by="Salary", ascending=False)
```

Output:

| Employee | Salary |
| -------- | -----: |
| Sophia   |  85000 |
| Emma     |  71000 |
| David    |  68000 |
| Rahul    |  62000 |
| Alice    |  50000 |

Descending order is commonly used to identify:

* Top-selling products
* Highest-paid employees
* Best-performing regions
* Most profitable customers

---

# 6. Sorting by Multiple Columns

Sometimes a single column isn't enough.

Suppose you want to sort employees by:

1. Department
2. Salary (Highest First)

```python
df.sort_values(
    by=["Department", "Salary"],
    ascending=[True, False]
)
```

Pandas first groups records by department and then sorts salaries within each department.

This technique is frequently used in business reporting and dashboard preparation.

---

# Key Takeaways

* Sorting organizes data into meaningful order.
* `sort_values()` is one of the most frequently used Pandas functions.
* Multiple-column sorting enables more advanced analysis.
* Understanding sorting is essential before performing grouping, aggregation, or visualization.

> **"Well-organized data makes patterns easier to discover and decisions easier to make."**
