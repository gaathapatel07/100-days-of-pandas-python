# Day 24 — Categorical Data, Encoding & Memory Optimization

<div align="center">

# 100 Days of Pandas

### Day 24 · Working Efficiently with Categorical Variables

*"Not all data is numerical. Understanding categorical data is essential for building efficient analytical and machine learning workflows."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Categorical%20Data-blue)
![Day](https://img.shields.io/badge/Day-24-orange)

</div>

---

# Table of Contents

1. Introduction
2. Why Categorical Data Matters
3. Learning Objectives
4. Understanding Categorical Data
5. Category Data Type
6. Memory Optimization
7. Category Operations
8. Summary

---

# 1. Introduction

Real-world datasets contain a mixture of numerical and categorical variables.

Examples of categorical data include:

* Gender
* Department
* City
* Product Category
* Payment Method
* Customer Segment
* Education Level
* Blood Group
* Marital Status

Unlike numerical values, categorical variables represent labels or groups rather than measurable quantities.

Pandas provides a dedicated **Category** data type that stores repeated values efficiently and improves analytical performance.

---

# 2. Why Categorical Data Matters

Imagine an e-commerce company with **10 million customer records**.

One column stores customer gender.

Instead of storing:

```text id="cat01"
Male

Female

Male

Female

Male
```

millions of times as strings, Pandas stores each unique category only once and replaces repeated values with compact integer codes.

Benefits include:

* Lower memory consumption
* Faster grouping
* Improved sorting
* Better performance
* Easier machine learning preprocessing

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Understand categorical variables.
* Convert object columns into categories.
* Reduce memory usage.
* Inspect category metadata.
* Perform category-specific operations.

---

# 4. Understanding Categorical Data

Categorical variables describe qualities instead of quantities.

### Examples

| Customer | Gender |
| -------- | ------ |
| Alice    | Female |
| Rahul    | Male   |
| Priya    | Female |

Unlike numerical variables, categories cannot usually be added or averaged.

Categories are divided into two major types.

---

## Nominal Categories

No natural ordering exists.

Examples:

* Gender
* Country
* City
* Department
* Blood Group

Example:

| City   |
| ------ |
| Delhi  |
| Mumbai |
| Pune   |

Changing the order does not change their meaning.

---

## Ordinal Categories

Categories have a meaningful order.

Examples:

* Education Level
* Shirt Size
* Customer Satisfaction
* Performance Rating

Example:

| Satisfaction |
| ------------ |
| Poor         |
| Average      |
| Good         |
| Excellent    |

Order matters even though the values are textual.

---

# 5. Category Data Type

Initially, text columns usually appear as:

```python id="dtype01"
df.dtypes
```

Output:

```text id="dtype02"
Gender    object
```

Convert the column.

```python id="dtype03"
df["Gender"] = (
    df["Gender"]
      .astype("category")
)
```

Now check the type.

```python id="dtype04"
df.dtypes
```

Output:

```text id="dtype05"
Gender    category
```

The column now occupies significantly less memory.

---

# Viewing Categories

Display all unique categories.

```python id="cat02"
df["Gender"].cat.categories
```

Output:

```text id="cat03"
Index(
[
'Female',
'Male'
]
)
```

---

# Category Codes

Internally, Pandas stores integer codes.

```python id="cat04"
df["Gender"].cat.codes
```

Output:

| Gender | Code |
| ------ | ---: |
| Female |    0 |
| Male   |    1 |
| Female |    0 |

The displayed labels remain unchanged, but storage becomes much more efficient.

---

# 6. Memory Optimization

Measure memory usage.

```python id="memory01"
df.memory_usage(
    deep=True
)
```

After converting repeated text columns into categories, memory consumption often decreases dramatically.

Example:

| Data Type | Memory Usage |
| --------- | -----------: |
| Object    |        18 MB |
| Category  |         2 MB |

Large datasets benefit the most from category conversion.

---

## Checking Total Memory

```python id="memory02"
df.memory_usage(
    deep=True
).sum()
```

This returns the total memory consumed by the DataFrame.

---

# 7. Category Operations

Count occurrences.

```python id="cat05"
df["Gender"].value_counts()
```

Output:

| Gender | Count |
| ------ | ----: |
| Female |  5400 |
| Male   |  4600 |

---

Sort categories.

```python id="cat06"
df.sort_values(
    "Gender"
)
```

Filtering works exactly like normal text columns.

```python id="cat07"
df[
    df["Gender"] ==
    "Female"
]
```

Categories integrate seamlessly with other Pandas operations.

---

# Business Example

An HR department maintains employee records.

Columns include:

* Department
* Gender
* Job Title
* Employment Type
* Office Location

These columns contain relatively few unique values but millions of repeated entries.

Converting them into categorical variables reduces memory usage, speeds up reporting, and improves dashboard performance.

---

# Best Practices

✔ Convert repeated text columns into categories.

✔ Leave high-cardinality columns (e.g., email addresses) as strings.

✔ Measure memory before and after optimization.

✔ Document category definitions for future reference.

✔ Use categories before building machine learning pipelines.

---

# Common Mistakes

### Converting High-Cardinality Columns

Avoid converting columns where almost every value is unique.

Examples:

* Email Address
* Customer ID
* Transaction ID

These columns gain little benefit from the category data type.

---

### Assuming Categories Are Numeric

Category codes are internal representations and should not be treated as meaningful numerical values.

---

# Key Takeaways 

After completing this section, you should understand:

* What categorical data represents.
* The difference between nominal and ordinal categories.
* How to convert object columns into categories.
* How categories improve memory efficiency.
* How to inspect and work with category metadata.

> **"Categorical data allows analysts to represent repeated information efficiently, reducing memory usage while improving performance across analytical workflows."**

