# Day 02 — Selecting, Filtering & Indexing Data in Pandas

<div align="center">

# 100 Days of Pandas

### Day 02 · Mastering Data Selection and Filtering

*"The ability to extract the right information is the first step toward meaningful analysis."*

![Difficulty](https://img.shields.io/badge/Difficulty-Beginner-success)
![Topic](https://img.shields.io/badge/Topic-Data%20Selection-blue)
![Day](https://img.shields.io/badge/Day-02-orange)

</div>

---

#  Table of Contents

1. Introduction
2. Why Data Selection Matters
3. Learning Objectives
4. Understanding Rows and Columns
5. Selecting Columns
6. Selecting Multiple Columns
7. Selecting Rows
8. Indexing in Pandas
9. `loc` vs `iloc`
10. Boolean Filtering
11. Filtering with Multiple Conditions
12. Common Mistakes
13. Business Scenario
14. Practice Exercises
15. Summary

---

# 1. Introduction

In Day 01, you learned how to load a dataset and inspect its structure. While understanding the data is important, real analysis begins when you start extracting only the information that matters.

Large datasets often contain hundreds of columns and thousands of rows. In most cases, you will only need a small subset of this data to answer a specific business question. Efficiently selecting rows and columns is therefore one of the most essential skills for any data analyst.

Today's lesson introduces the core techniques used to retrieve, filter, and access data in a Pandas DataFrame.

---

# 2. Why Data Selection Matters

Imagine you're working for an online retail company with a dataset containing over one million customer orders.

The dataset includes information such as:

* Order ID
* Customer Name
* Product
* Category
* Quantity
* Sales
* Discount
* Profit
* Region
* Order Date

Suppose your manager asks:

* Which customers spent more than ₹10,000?
* Which orders were placed in the West region?
* Which products generated negative profit?
* Which category produced the highest sales?

Without filtering and selecting data, answering these questions would be slow and inefficient.

Pandas provides powerful indexing and filtering tools that allow analysts to answer such questions with just a few lines of code.

---

# 3. Learning Objectives

By the end of Day 02, you will be able to:

* Select one or more columns from a DataFrame.
* Retrieve rows using labels and positions.
* Understand the difference between `loc` and `iloc`.
* Filter data using logical conditions.
* Combine multiple conditions for advanced filtering.
* Write cleaner and more readable Pandas queries.

---

# 4. Understanding Rows and Columns

Every Pandas DataFrame consists of two primary dimensions:

* **Rows** – represent individual records or observations.
* **Columns** – represent attributes or variables describing those records.

Example:

| Index | Name  | Department | Salary |
| ----: | ----- | ---------- | -----: |
|     0 | Alice | HR         |  50000 |
|     1 | Rahul | Sales      |  62000 |
|     2 | Emma  | IT         |  71000 |

In this example:

* Each row represents one employee.
* Each column stores a specific attribute.
* The leftmost numbers form the DataFrame index.

Understanding this structure is essential before performing any selection or filtering operations.

---

# 5. Selecting a Single Column

To retrieve a single column, use square bracket notation.

```python
import pandas as pd

df = pd.read_csv("employees.csv")

df["Salary"]
```

Output:

```text
0    50000
1    62000
2    71000
```

This returns a **Series**, not a DataFrame.

---

# 6. Selecting Multiple Columns

To retrieve more than one column, pass a list of column names.

```python
df[["Name", "Salary"]]
```

Output:

| Name  | Salary |
| ----- | -----: |
| Alice |  50000 |
| Rahul |  62000 |
| Emma  |  71000 |

Unlike selecting a single column, this returns a **DataFrame**.

---

# Key Takeaways

* Every DataFrame consists of rows, columns, and an index.
* Selecting a single column returns a Series.
* Selecting multiple columns returns a DataFrame.
* Understanding the structure of a DataFrame is the foundation for filtering and analysis.

---

> **"The value of data lies not in how much you have, but in how effectively you can extract the information you need."**

 In the next section, you'll learn how to retrieve rows using **`loc`** and **`iloc`**, perform conditional filtering, and answer real-world business questions using Pandas.

