# Day 28 — MultiIndex, Hierarchical Indexing & Advanced Index Operations

<div align="center">

# 100 Days of Pandas

### Day 28 · Mastering Multi-Dimensional Data Analysis

*"Indexes are more than row labels—they organize data, improve performance, and unlock powerful analytical capabilities."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-MultiIndex%20%26%20Indexing-blue)
![Day](https://img.shields.io/badge/Day-28-orange)

</div>

---

# Table of Contents 

1. Introduction
2. Why MultiIndex Matters
3. Learning Objectives
4. Understanding Indexes
5. Creating a MultiIndex
6. Working with Hierarchical Rows
7. Setting & Resetting Indexes
8. Summary

---

# 1. Introduction

Every Pandas DataFrame has an index.

By default, it looks like this:

| Index | Customer | Sales |
| ----: | -------- | ----: |
|     0 | Alice    |  5200 |
|     1 | Rahul    |  6100 |
|     2 | Priya    |  4800 |

However, real-world datasets often require **multiple levels of indexing**.

For example:

| Region | City    | Sales |
| ------ | ------- | ----: |
| North  | Delhi   |  5200 |
| North  | Jaipur  |  4800 |
| South  | Chennai |  6100 |

Using **Region** and **City** together creates a hierarchical index.

This enables efficient slicing, grouping, and reporting.

---

# 2. Why MultiIndex Matters

Imagine a multinational retailer.

Management wants reports by:

* Region
* City
* Product Category
* Month

Using only one index makes analysis difficult.

A MultiIndex organizes the data naturally.

Example:

```text id="multi01"
North
   Delhi
   Jaipur

South
   Chennai
   Kochi
```

This hierarchy closely resembles business reporting structures.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Understand hierarchical indexes.
* Create MultiIndexes.
* Set and reset indexes.
* Navigate multiple index levels.
* Improve analytical workflows.

---

# 4. Understanding Indexes

Check the current index.

```python id="index01"
df.index
```

Output

```text id="index02"
RangeIndex(
start=0,
stop=100,
step=1
)
```

Change the index.

```python id="index03"
df = df.set_index(
    "Customer ID"
)
```

Output

| Customer ID | Name  | Sales |
| ----------: | ----- | ----: |
|         101 | Alice |  5200 |
|         102 | Rahul |  6100 |

Now **Customer ID** becomes the row index.

---

## Reset the Index

Return to the default numeric index.

```python id="index04"
df = df.reset_index()
```

---

# 5. Creating a MultiIndex

Suppose the dataset contains:

| Region | City    | Sales |
| ------ | ------- | ----: |
| North  | Delhi   |  5200 |
| North  | Jaipur  |  4800 |
| South  | Chennai |  6100 |
| South  | Kochi   |  4500 |

Create a MultiIndex.

```python id="multi02"
df = df.set_index(
    [
        "Region",
        "City"
    ]
)
```

Output

```text id="multi03"
Region
   City
```

Now two hierarchical levels exist.

---

## Display the Index

```python id="multi04"
df.index
```

Output

```text id="multi05"
MultiIndex(...)
```

---

# 6. Working with Hierarchical Rows

Select one region.

```python id="multi06"
df.loc["North"]
```

Output

| City   | Sales |
| ------ | ----: |
| Delhi  |  5200 |
| Jaipur |  4800 |

---

Select one city inside a region.

```python id="multi07"
df.loc[
    (
        "North",
        "Delhi"
    )
]
```

Output

| Sales |
| ----: |
|  5200 |

---

Select multiple cities.

```python id="multi08"
df.loc[
    (
        "South",
        [
            "Chennai",
            "Kochi"
        ]
    )
]
```

Hierarchical indexing allows precise access to nested data.

---

# 7. Setting & Resetting Multiple Levels

Reset every level.

```python id="multi09"
df.reset_index()
```

---

Reset only one level.

```python id="multi10"
df.reset_index(
    level="City"
)
```

This keeps **Region** as the index while converting **City** back into a column.

---

# Business Example

A global retailer stores sales data using:

* Region
* Country
* City

Analysts frequently generate reports such as:

* Revenue by region
* Sales by city
* Country-wise performance

Using MultiIndex makes these reports easier to generate and navigate.

---

# Best Practices

✔ Use MultiIndex for naturally hierarchical data.

✔ Keep indexes meaningful and stable.

✔ Reset indexes before exporting to CSV.

✔ Avoid unnecessary MultiIndexes in very simple datasets.

✔ Name index levels clearly.

---

# Common Mistakes

### Forgetting the Current Index

After calling:

```python id="mistake01"
df.set_index("Customer ID")
```

The column is no longer available as a regular column.

Attempting:

```python id="mistake02"
df["Customer ID"]
```

may produce a `KeyError`.

Use:

```python id="mistake03"
df.reset_index()
```

when the column is needed again.

---

### Using MultiIndex Unnecessarily

Simple datasets often work better with a regular index.

Choose MultiIndex only when the hierarchy provides analytical value.

---

# Key Takeaways

After completing this section, you should understand:

* What indexes represent.
* How to create a MultiIndex.
* How hierarchical indexes organize data.
* How to access MultiIndex rows.
* How to set and reset indexes.

> **"Hierarchical indexing transforms flat tables into structured, multi-dimensional datasets that mirror real-world business relationships."**

