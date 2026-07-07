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

# 8. Sorting a MultiIndex

A MultiIndex should usually be sorted before performing advanced slicing.

Suppose the DataFrame has a MultiIndex:

```text id="sort01"
Region
   City
```

Sort the index.

```python id="sort02"
df = df.sort_index()
```

Sorting improves performance and enables efficient label-based slicing.

---

## Sort a Specific Level

Sort only by **City**.

```python id="sort03"
df.sort_index(
    level="City"
)
```

This sorts records alphabetically within each region.

---

# 9. Swapping Index Levels

Sometimes analysts need to change the hierarchy.

Current hierarchy:

```text id="swap01"
Region
   City
```

Swap the levels.

```python id="swap02"
df = df.swaplevel()
```

Output:

```text id="swap03"
City
   Region
```

This operation is useful when reports need to be organized differently.

---

## Example

Before

| Region | City    |
| ------ | ------- |
| North  | Delhi   |
| South  | Chennai |

After `swaplevel()`

| City    | Region |
| ------- | ------ |
| Delhi   | North  |
| Chennai | South  |

---

# 10. Reordering Multiple Levels

For MultiIndexes with three or more levels, reorder them explicitly.

Suppose the index contains:

```text id="reorder01"
Year

Region

City
```

Reorder the hierarchy.

```python id="reorder02"
df = df.reorder_levels(
    [
        "Region",
        "City",
        "Year"
    ]
)
```

Output

```text id="reorder03"
Region

City

Year
```

This is especially useful in financial and sales reporting.

---

# 11. Cross Sections Using `xs()`

The `xs()` method extracts data from a specific level of a MultiIndex.

Suppose we want every record from the **North** region.

```python id="xs01"
df.xs(
    "North",
    level="Region"
)
```

Output

| City   | Sales |
| ------ | ----: |
| Delhi  |  5200 |
| Jaipur |  4800 |

---

## Cross Section on Another Level

Retrieve data for **Delhi**, regardless of region.

```python id="xs02"
df.xs(
    "Delhi",
    level="City"
)
```

This is much cleaner than writing complex filtering expressions.

---

# 12. Advanced Slicing with `IndexSlice`

`IndexSlice` enables powerful slicing across multiple index levels.

Import it.

```python id="slice01"
idx = pd.IndexSlice
```

Example:

```python id="slice02"
df.loc[
    idx[
        "North":"South",
        :
    ],
    :
]
```

Explanation:

* `"North":"South"` → Region range
* `:` → Every city
* Final `:` → Every column

---

## Slice Specific Cities

```python id="slice03"
df.loc[
    idx[
        :,
        [
            "Delhi",
            "Mumbai"
        ]
    ],
    :
]
```

Only the specified cities are returned.

---

# 13. MultiIndex Columns

Indexes are not limited to rows.

Columns can also have multiple levels.

Example

| Sales | Sales | Profit | Profit |
| ----- | ----- | ------ | ------ |
| 2025  | 2026  | 2025   | 2026   |

Create MultiIndex columns.

```python id="columns01"
df.columns = pd.MultiIndex.from_tuples(
    [
        ("Sales","2025"),
        ("Sales","2026"),
        ("Profit","2025"),
        ("Profit","2026")
    ]
)
```

---

## Access Multi-Level Columns

Retrieve Sales for 2026.

```python id="columns02"
df[
    (
        "Sales",
        "2026"
    )
]
```

This is common in financial statements and business dashboards.

---

# 14. Selecting Values from MultiIndex Columns

Retrieve every Sales column.

```python id="columns03"
df["Sales"]
```

Output

| 2025 | 2026 |
| ---: | ---: |
| 5200 | 6100 |

Retrieve a single value.

```python id="columns04"
df[
    (
        "Profit",
        "2025"
    )
]
```

---

# 15. Performance Benefits of Indexing

Indexes improve performance by reducing the amount of data that Pandas must scan.

Benefits include:

* Faster filtering
* Faster joins
* Efficient slicing
* Better grouping performance
* Reduced lookup time

Proper indexing becomes increasingly important as datasets grow into millions of rows.

---

# Business Example

A multinational retailer stores sales data using three index levels:

* Region
* City
* Year

Management frequently requests reports such as:

* Sales for one region across all years.
* Profit for one city.
* Revenue by year and city.
* Quarterly comparisons between regions.

Using `xs()`, `swaplevel()`, and `IndexSlice`, analysts retrieve these reports efficiently without restructuring the dataset.

---

# Best Practices

✔ Sort MultiIndexes before slicing.

✔ Use descriptive names for index levels.

✔ Prefer `xs()` for selecting a single level.

✔ Use `IndexSlice` for complex selections.

✔ Limit MultiIndex depth to what is necessary for analysis.

---

# Common Mistakes

### Slicing an Unsorted MultiIndex

Attempting advanced slicing on an unsorted MultiIndex may produce incorrect results or performance warnings.

Always sort first.

```python id="mistake01"
df = df.sort_index()
```

---

### Forgetting the Order of Levels

After using `swaplevel()` or `reorder_levels()`, review the new structure.

```python id="mistake02"
df.index.names
```

This displays the current hierarchy.

---

### Confusing Row and Column MultiIndexes

A DataFrame may have:

* A MultiIndex on rows.
* A MultiIndex on columns.
* Both simultaneously.

Always check:

```python id="mistake03"
df.index

df.columns
```

---

# Quick Recap

You have now learned how to:

* Sort MultiIndexes.
* Swap index levels.
* Reorder hierarchical indexes.
* Extract cross sections with `xs()`.
* Perform advanced slicing using `IndexSlice`.
* Work with MultiIndex columns.
* Improve performance using indexing.

> **"Well-designed indexes transform complex, multi-dimensional datasets into structures that are easy to navigate, analyze, and scale."**
