# Day 14 — Advanced Indexing & MultiIndex Operations

<div align="center">

# 100 Days of Pandas

### Day 14 · Mastering Data Selection and Hierarchical Indexing

*"Efficient data analysis begins with knowing how to locate exactly the information you need."*

![Difficulty](https://img.shields.io/badge/Difficulty-Intermediate-yellow)
![Topic](https://img.shields.io/badge/Topic-Advanced%20Indexing-blue)
![Day](https://img.shields.io/badge/Day-14-orange)

</div>

---

# Table of Contents

1. Introduction
2. Why Indexing Matters
3. Learning Objectives
4. Understanding DataFrame Indexes
5. Setting and Resetting Indexes
6. Label-Based Selection with `loc`
7. Position-Based Selection with `iloc`
8. Summary

---


# 1. Introduction

Every DataFrame in Pandas has an **index**.

Although many beginners think of the index as just row numbers, it plays a much more significant role. An index acts as a unique identifier that allows Pandas to locate, filter, align, and retrieve data efficiently.

As datasets grow larger, relying only on row numbers becomes impractical. Proper indexing improves readability, simplifies filtering, and often improves performance.

In this lesson, you'll learn how to use indexes effectively and discover how **MultiIndex** enables hierarchical data organization.

---

# 2. Why Indexing Matters

Imagine an online retailer with a dataset containing over one million customer orders.

Without indexing, retrieving all orders for a particular customer would require scanning every row.

Instead, analysts often use **Customer ID** or **Order ID** as the index.

Example:

| Customer ID | Name  | Region |
| ----------: | ----- | ------ |
|         101 | Alice | West   |
|         102 | Rahul | South  |
|         103 | Emma  | North  |

After setting **Customer ID** as the index, retrieving a specific customer becomes simple and intuitive.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Understand the purpose of DataFrame indexes.
* Set and reset indexes.
* Retrieve records using labels.
* Select records by position.
* Differentiate between `loc` and `iloc`.
* Prepare data for hierarchical indexing.

---

# 4. Understanding DataFrame Indexes

When a DataFrame is created, Pandas automatically assigns a default integer index.

Example:

| Index | Name  | Sales |
| ----: | ----- | ----: |
|     0 | Alice |  2500 |
|     1 | Rahul |  4200 |
|     2 | Emma  |  3900 |

These default indexes are useful, but business datasets often have more meaningful identifiers such as:

* Customer ID
* Employee ID
* Order ID
* Product Code
* Invoice Number

Using meaningful indexes makes data retrieval easier and more intuitive.

---

# 5. Setting and Resetting Indexes

### Setting an Index

Convert a column into the DataFrame index.

```python id="idx01"
df = df.set_index("Customer ID")
```

Example Output:

| Customer ID | Name  | Sales |
| ----------: | ----- | ----: |
|         101 | Alice |  2500 |
|         102 | Rahul |  4200 |
|         103 | Emma  |  3900 |

Notice that **Customer ID** is no longer a regular column—it becomes the row index.

---

### Why Set an Index?

Setting an index helps:

* Retrieve rows quickly.
* Improve readability.
* Align datasets during joins.
* Build hierarchical structures.

---

### Resetting the Index

Return the index to a regular column.

```python id="idx02"
df = df.reset_index()
```

Output:

| Index | Customer ID | Name  | Sales |
| ----: | ----------: | ----- | ----: |
|     0 |         101 | Alice |  2500 |
|     1 |         102 | Rahul |  4200 |
|     2 |         103 | Emma  |  3900 |

This is useful before exporting data or performing operations that require a standard DataFrame.

---

# 6. Label-Based Selection with `loc`

The `loc[]` accessor selects data using **row and column labels**.

Suppose **Customer ID** is the index.

Retrieve Customer **102**:

```python id="loc01"
df.loc[102]
```

Output:

| Name  | Sales |
| ----- | ----: |
| Rahul |  4200 |

---

### Select Multiple Labels

```python id="loc02"
df.loc[
    [101, 103]
]
```

---

### Select Specific Columns

```python id="loc03"
df.loc[
    [101, 103],
    ["Name", "Sales"]
]
```

Output:

| Customer ID | Name  | Sales |
| ----------: | ----- | ----: |
|         101 | Alice |  2500 |
|         103 | Emma  |  3900 |

`loc[]` is ideal when working with meaningful identifiers instead of numeric positions.

---

# 7. Position-Based Selection with `iloc`

Unlike `loc[]`, the `iloc[]` accessor uses **integer positions**.

Retrieve the first row:

```python id="iloc01"
df.iloc[0]
```

Retrieve the first three rows:

```python id="iloc02"
df.iloc[0:3]
```

Retrieve rows 2 to 5 and the first two columns:

```python id="iloc03"
df.iloc[
    2:6,
    0:2
]
```

Unlike `loc[]`, `iloc[]` ignores row labels completely and always uses positional indexing.

---

# `loc[]` vs `iloc[]`

| Feature               | `loc[]`     | `iloc[]`          |
| --------------------- | ----------- | ----------------- |
| Selection Type        | Labels      | Integer Positions |
| Row Selection         | Customer ID | Row Number        |
| Column Selection      | Column Name | Column Position   |
| Uses Index Labels     | ✅           | ❌                 |
| Uses Integer Position | ❌           | ✅                 |

Choosing the correct accessor depends on how you want to identify the data.

---

# Key Takeaways

After completing this section, you should understand:

* The importance of DataFrame indexes.
* How to set and reset indexes.
* The difference between label-based and position-based selection.
* When to use `loc[]` and `iloc[]`.
* Why proper indexing improves data organization and retrieval.

> **"Indexes are more than row numbers—they are the foundation of efficient data selection, organization, and analysis in Pandas."**

# 8. Understanding MultiIndex

So far, we've worked with a single index column.

However, many real-world datasets require multiple levels of identification.

For example, consider a retail company operating across different regions.

A single **Product** may exist in multiple **Regions**.

Using only one index is no longer sufficient.

This is where **MultiIndex**, also known as **Hierarchical Indexing**, becomes valuable.

A MultiIndex allows multiple columns to function together as the DataFrame's index.

---

## Example Dataset

| Region | Category   | Sales |
| ------ | ---------- | ----: |
| North  | Furniture  |  5200 |
| North  | Technology |  6100 |
| South  | Furniture  |  7300 |
| South  | Technology |  8100 |

Create a MultiIndex:

```python id="multi01"
df = df.set_index(
    ["Region", "Category"]
)
```

### Output

| Region | Category   | Sales |
| ------ | ---------- | ----: |
| North  | Furniture  |  5200 |
|        | Technology |  6100 |
| South  | Furniture  |  7300 |
|        | Technology |  8100 |

Now every row is uniquely identified by both **Region** and **Category**.

---

# 9. Accessing MultiIndex Data

Once a MultiIndex has been created, you can retrieve records using multiple labels.

Retrieve Furniture sales in the North region:

```python id="multi02"
df.loc[
    ("North", "Furniture")
]
```

Output:

| Sales |
| ----: |
|  5200 |

---

Retrieve all categories for the South region:

```python id="multi03"
df.loc["South"]
```

Output:

| Category   | Sales |
| ---------- | ----: |
| Furniture  |  7300 |
| Technology |  8100 |

---

Retrieve multiple regions:

```python id="multi04"
df.loc[
    ["North", "South"]
]
```

This flexibility makes MultiIndex ideal for complex reporting.

---

# 10. Sorting MultiIndex Data

Many MultiIndex operations require sorted indexes.

Sort the index:

```python id="multi05"
df = df.sort_index()
```

Sorting ensures predictable results when slicing or selecting hierarchical data.

---

# 11. Slicing MultiIndex Data

One of the biggest advantages of MultiIndex is hierarchical slicing.

Suppose you want only the Furniture category across all regions.

```python id="multi06"
df.xs(
    "Furniture",
    level="Category"
)
```

### Output

| Region | Sales |
| ------ | ----: |
| North  |  5200 |
| South  |  7300 |

The `xs()` method (cross-section) extracts data from a specified index level.

---

## Slice by Region

```python id="multi07"
df.loc["North"]
```

---

## Slice by Multiple Levels

```python id="multi08"
df.loc[
    ("North", "Technology")
]
```

Hierarchical slicing is much cleaner than applying multiple filtering conditions.

---

# 12. Swapping Index Levels

Sometimes a different hierarchy is needed.

Suppose you want **Category** as the first level instead of **Region**.

```python id="multi09"
df.swaplevel()
```

Output:

| Category   | Region | Sales |
| ---------- | ------ | ----: |
| Furniture  | North  |  5200 |
| Technology | North  |  6100 |
| Furniture  | South  |  7300 |
| Technology | South  |  8100 |

This is useful when analyzing data from different perspectives.

---

# 13. Removing Index Levels

Convert the MultiIndex back into regular columns.

```python id="multi10"
df.reset_index()
```

This restores a standard DataFrame, making it easier to export or visualize the data.

---

# Business Example

Imagine you're analyzing retail performance.

Each transaction is uniquely identified by:

* Region
* Store
* Product Category

Instead of repeatedly filtering three separate columns, a MultiIndex allows fast and intuitive access.

For example:

```python id="multi11"
df.loc[
    ("West", "Store A")
]
```

or

```python id="multi12"
df.xs(
    "Electronics",
    level="Category"
)
```

This approach simplifies reporting and improves code readability.

---

# Best Practices

✔ Use MultiIndex when one column cannot uniquely identify a record.

✔ Sort the index before slicing.

✔ Choose meaningful index levels.

✔ Reset the index before exporting data to CSV or Excel.

✔ Use `xs()` for cleaner hierarchical selections.

---

# Common Mistakes

### Forgetting to Sort the Index

Some slicing operations may fail or produce unexpected results if the MultiIndex is not sorted.

Always sort first:

```python id="multi13"
df = df.sort_index()
```

---

### Using `iloc[]` Instead of `loc[]`

MultiIndex is label-based.

Use:

```python id="multi14"
df.loc[
    ("North", "Furniture")
]
```

rather than relying on row positions.

---

# Quick Recap

You have now learned how to:

* Create a MultiIndex.
* Retrieve hierarchical data.
* Slice across index levels.
* Sort hierarchical indexes.
* Swap index levels.
* Convert MultiIndexes back into regular columns.

> **"MultiIndex transforms flat tables into structured hierarchies, making complex datasets easier to organize, navigate, and analyze."**
