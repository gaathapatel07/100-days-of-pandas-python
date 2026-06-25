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

# 7. Understanding Indexing in Pandas

Before learning how to filter data efficiently, it's important to understand **indexing**.

An **index** is a unique label assigned to each row in a DataFrame. By default, Pandas automatically creates a numerical index starting from `0`.

Example:

| Index | Name  | Department | Salary |
| ----: | ----- | ---------- | -----: |
|     0 | Alice | HR         |  50000 |
|     1 | Rahul | Sales      |  62000 |
|     2 | Emma  | IT         |  71000 |
|     3 | David | Finance    |  68000 |

The numbers in the first column (`0, 1, 2, 3`) are called the **index**.

You can think of the index as the "address" of each row, allowing Pandas to quickly locate and retrieve records.

---

# 8. Selecting Rows Using `loc`

The `loc` accessor is used to retrieve rows and columns **based on labels**.

### Syntax

```python
df.loc[row_label, column_label]
```

### Example

```python
import pandas as pd

df = pd.read_csv("employees.csv")

df.loc[2]
```

Output:

| Name | Department | Salary |
| ---- | ---------- | -----: |
| Emma | IT         |  71000 |

Since the index label is `2`, Pandas returns the third row.

---

### Selecting Specific Columns with `loc`

```python
df.loc[:, ["Name", "Salary"]]
```

Output:

| Name  | Salary |
| ----- | -----: |
| Alice |  50000 |
| Rahul |  62000 |
| Emma  |  71000 |
| David |  68000 |

Here:

* `:` means **all rows**
* `["Name", "Salary"]` selects only those columns

---

### Selecting Specific Rows and Columns

```python
df.loc[1:3, ["Name", "Department"]]
```

Output:

| Name  | Department |
| ----- | ---------- |
| Rahul | Sales      |
| Emma  | IT         |
| David | Finance    |

**Note:** Unlike normal Python slicing, `loc` includes the ending label.

---

# 9. Selecting Rows Using `iloc`

The `iloc` accessor retrieves data using **integer positions** rather than labels.

### Syntax

```python
df.iloc[row_position, column_position]
```

### Example

```python
df.iloc[2]
```

Output:

| Name | Department | Salary |
| ---- | ---------- | -----: |
| Emma | IT         |  71000 |

Although the output looks the same as `loc`, `iloc` identifies rows based on **position**, not labels.

---

### Selecting Multiple Rows

```python
df.iloc[0:3]
```

Output:

| Name  | Department | Salary |
| ----- | ---------- | -----: |
| Alice | HR         |  50000 |
| Rahul | Sales      |  62000 |
| Emma  | IT         |  71000 |

Unlike `loc`, the ending position is **excluded**, just like standard Python slicing.

---

### Selecting Rows and Columns Together

```python
df.iloc[0:3, 0:2]
```

Output:

| Name  | Department |
| ----- | ---------- |
| Alice | HR         |
| Rahul | Sales      |
| Emma  | IT         |

---

# 10. `loc` vs `iloc`

Understanding the difference between these two accessors is essential.

| Feature                    | `loc`  | `iloc`            |
| -------------------------- | ------ | ----------------- |
| Uses                       | Labels | Integer Positions |
| Includes End Value         | ✅ Yes  | ❌ No              |
| Select Columns by Name     | ✅ Yes  | ❌ No              |
| Select Columns by Position | ❌ No   | ✅ Yes             |

### Example

```python
df.loc[1:3]
```

Returns rows **1, 2, and 3**.

```python
df.iloc[1:3]
```

Returns rows **1 and 2**.

This difference is one of the most common interview questions about Pandas.

---

# 11. Best Practices

✔ Use **`loc`** when your dataset has meaningful row labels or when selecting columns by name.

✔ Use **`iloc`** when working with row or column positions.

✔ Avoid hardcoding column positions in large projects, as datasets may change over time.

✔ Prefer descriptive column names instead of relying on numerical indexes.

---

# 12. Common Mistakes

### Mistake 1 — Mixing Labels and Positions

❌ Incorrect

```python
df.loc[:, 0]
```

`loc` expects **column labels**, not column positions.

---

✅ Correct

```python
df.iloc[:, 0]
```

or

```python
df.loc[:, "Name"]
```

---

### Mistake 2 — Forgetting That `iloc` Excludes the End Position

```python
df.iloc[0:3]
```

Returns:

Rows `0`, `1`, and `2`

**Not** row `3`.

---

### Mistake 3 — Using Column Positions Instead of Names

This works:

```python
df.iloc[:, 2]
```

But in production code, it's often better to use:

```python
df["Salary"]
```

Using column names makes code easier to read and less likely to break if the dataset structure changes.

---

# Quick Recap

By now, you've learned how to:

* Understand the purpose of indexes.
* Retrieve rows using labels with `loc`.
* Retrieve rows using positions with `iloc`.
* Select specific rows and columns.
* Distinguish between label-based and position-based indexing.
* Avoid common indexing mistakes.

> **"Choosing the right data is just as important as analyzing it. Mastering `loc` and `iloc` gives you precise control over every row and column in your dataset."**

---
---

# 13. Filtering Data with Conditions

In real-world data analysis, you rarely need to work with an entire dataset. Instead, you focus on records that satisfy specific conditions.

For example:

* Customers who spent more than ₹10,000
* Employees earning above ₹75,000
* Orders placed in the South region
* Products with negative profit
* Students scoring above 90%

Filtering allows you to retrieve only the rows that meet your criteria.

---

## Basic Filtering

Suppose we have the following dataset:

| Employee | Department | Salary | Experience |
| -------- | ---------- | -----: | ---------: |
| Alice    | HR         |  50000 |          3 |
| Rahul    | Sales      |  62000 |          5 |
| Emma     | IT         |  71000 |          4 |
| David    | Finance    |  68000 |          6 |
| Sophia   | IT         |  85000 |          8 |

Let's filter employees earning more than ₹65,000.

```python
df[df["Salary"] > 65000]
```

Output:

| Employee | Department | Salary | Experience |
| -------- | ---------- | -----: | ---------: |
| Emma     | IT         |  71000 |          4 |
| David    | Finance    |  68000 |          6 |
| Sophia   | IT         |  85000 |          8 |

The expression:

```python
df["Salary"] > 65000
```

returns a Boolean Series.

Example:

```text
0    False
1    False
2     True
3     True
4     True
```

Pandas then keeps only the rows where the condition evaluates to **True**.

---

# Comparison Operators

The following operators are commonly used while filtering:

| Operator | Meaning                  | Example              |
| -------- | ------------------------ | -------------------- |
| `>`      | Greater than             | `Salary > 50000`     |
| `<`      | Less than                | `Age < 25`           |
| `>=`     | Greater than or equal to | `Marks >= 90`        |
| `<=`     | Less than or equal to    | `Profit <= 0`        |
| `==`     | Equal to                 | `Department == "IT"` |
| `!=`     | Not equal to             | `Region != "East"`   |

---

# Filtering Text Values

You can also filter categorical columns.

Example:

```python
df[df["Department"] == "IT"]
```

Output:

| Employee | Department | Salary |
| -------- | ---------- | -----: |
| Emma     | IT         |  71000 |
| Sophia   | IT         |  85000 |

This returns only employees from the IT department.

---

# Multiple Conditions

Often, business problems involve more than one condition.

Example:

Find employees working in IT **and** earning more than ₹70,000.

```python
df[(df["Department"] == "IT") & (df["Salary"] > 70000)]
```

Output:

| Employee | Department | Salary |
| -------- | ---------- | -----: |
| Sophia   | IT         |  85000 |

Notice that each condition is enclosed within parentheses.

---

# Logical Operators

Pandas supports three logical operators.

## AND (`&`)

Both conditions must be True.

```python
df[(df["Department"] == "IT") & (df["Salary"] > 70000)]
```

---

## OR (`|`)

At least one condition must be True.

```python
df[(df["Department"] == "HR") | (df["Department"] == "Finance")]
```

---

## NOT (`~`)

Reverses the condition.

Example:

Return everyone except the IT department.

```python
df[~(df["Department"] == "IT")]
```

---

# Using `isin()`

Instead of writing multiple OR conditions, use `isin()`.

Without `isin()`:

```python
df[(df["Department"] == "HR") | (df["Department"] == "Finance")]
```

With `isin()`:

```python
df[df["Department"].isin(["HR", "Finance"])]
```

Cleaner and easier to read.

---

# Filtering Missing Values

Return rows where Salary is missing.

```python
df[df["Salary"].isnull()]
```

Return rows where Salary is available.

```python
df[df["Salary"].notnull()]
```

---

# Business Scenario

Imagine you work as a Data Analyst for an e-commerce company.

Your manager asks you to identify:

* Customers who spent more than ₹15,000.
* Orders placed in the West region.
* Products generating negative profit.
* Customers eligible for premium membership.

Each of these questions can be answered by applying filters to the dataset instead of manually searching through thousands of records.

---

# Best Practices

✔ Keep filtering expressions readable.

✔ Use descriptive column names.

✔ Prefer `isin()` when checking multiple values.

✔ Break long filtering conditions into multiple lines for better readability.

✔ Verify your filters by checking the number of returned rows.

---

# Common Mistakes

❌ Missing parentheses

```python
df["Salary"] > 50000 & df["Department"] == "IT"
```

This produces unexpected results.

---

✅ Correct

```python
df[(df["Salary"] > 50000) & (df["Department"] == "IT")]
```

---

❌ Using `and` instead of `&`

```python
df[(df["Salary"] > 50000) and (df["Department"] == "IT")]
```

This raises an error because `and` works with single Boolean values, not Pandas Series.

---

✅ Correct

```python
df[(df["Salary"] > 50000) & (df["Department"] == "IT")]
```

---

# Key Takeaways

After this section, you should be able to:

* Filter rows using comparison operators.
* Apply multiple conditions using `&` and `|`.
* Exclude data using `~`.
* Use `isin()` for cleaner code.
* Filter missing and non-missing values.
* Solve common business queries using Boolean indexing.

> **"Filtering transforms raw data into meaningful subsets, allowing analysts to answer focused business questions quickly and efficiently."**

# Key Takeaways

* Every DataFrame consists of rows, columns, and an index.
* Selecting a single column returns a Series.
* Selecting multiple columns returns a DataFrame.
* Understanding the structure of a DataFrame is the foundation for filtering and analysis.

---

> **"The value of data lies not in how much you have, but in how effectively you can extract the information you need."**

 In the next section, you'll learn how to retrieve rows using **`loc`** and **`iloc`**, perform conditional filtering, and answer real-world business questions using Pandas.

