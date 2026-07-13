
# Day 35 — Advanced Performance Optimization & Memory Management in Pandas

<div align="center">

# 100 Days of Pandas

### Day 35 · Writing Faster & More Memory-Efficient Pandas Code

*"Efficient code isn't just about speed—it enables scalable analytics, lower memory usage, and production-ready data pipelines."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Performance%20Optimization-blue)
![Day](https://img.shields.io/badge/Day-35-orange)

</div>

---

# Table of Contents

1. Introduction
2. Why Performance Matters
3. Learning Objectives
4. Measuring Memory Usage
5. Optimizing Data Types
6. Using Categorical Data
7. Summary

---

# 1. Introduction

Pandas performs exceptionally well for most datasets.

However, as datasets grow to millions of rows, inefficient code can lead to:

* High memory consumption
* Slow execution
* System crashes
* Long ETL runtimes
* Increased cloud computing costs

Performance optimization ensures that data processing remains fast and scalable.

---

# 2. Why Performance Matters

Imagine an e-commerce company processing:

* 50 million transactions
* 10 million customers
* 5 years of sales history

Without optimization:

* Reports may take hours.
* Memory usage may exceed available RAM.
* Dashboards refresh slowly.

Optimized Pandas workflows reduce processing time and resource usage.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Measure DataFrame memory usage.
* Optimize data types.
* Reduce memory consumption.
* Use categorical variables efficiently.
* Build scalable Pandas workflows.

---

# 4. Measuring Memory Usage

Check overall memory usage.

```python id="memory01"
df.memory_usage() 
```

Example Output

| Column      | Memory (Bytes) |
| ----------- | -------------: |
| Index       |            128 |
| Customer ID |           8000 |
| Sales       |           8000 |
| City        |          32000 |

---

## Deep Memory Usage

For object columns, use:

```python id="memory02"
df.memory_usage(
    deep=True
)
```

This includes the memory consumed by string objects.

---

## Total Memory Usage

```python id="memory03"
total_memory = (
    df.memory_usage(
        deep=True
    )
    .sum()
    /
    1024**2
)

print(
    f"{total_memory:.2f} MB"
)
```

Output

```text id="memory04"
24.75 MB
```


Knowing memory usage is the first step toward optimization.

---

# 5. Optimizing Data Types

Many datasets use larger data types than necessary.

Example:

| Column | Current Type |
| ------ | ------------ |
| Age    | int64        |
| Rating | float64      |

Smaller types often consume much less memory.

---

## Integer Optimization

```python id="dtype01"
df["Age"] = (
    df["Age"]
      .astype("int8")
)
```

Possible integer types:

| Type    | Range             |
| ------- | ----------------- |
| `int8`  | -128 to 127       |
| `int16` | -32,768 to 32,767 |
| `int32` | ≈ ±2 billion      |
| `int64` | Very large        |

Choose the smallest type that safely stores your data.

---

## Float Optimization

```python id="dtype02"
df["Rating"] = (
    df["Rating"]
      .astype("float32")
)
```

`float32` typically requires half the memory of `float64`.

---

# 6. Using Categorical Data

Columns containing repeated text values consume significant memory.

Example

| Region |
| ------ |
| North  |
| North  |
| South  |
| East   |
| North  |

Instead of storing each string repeatedly:

```python id="category01"
df["Region"] = (
    df["Region"]
      .astype("category")
)
```

Categories store each unique value once and reference it internally.

---

## Compare Memory Usage

Before conversion.

```python id="category02"
df["Region"].memory_usage(
    deep=True
)
```

After conversion.

```python id="category03"
df["Region"] = (
    df["Region"]
      .astype("category")
)

df["Region"].memory_usage(
    deep=True
)
```

Memory usage often drops dramatically for columns with many repeated values.

---

## When to Use Categories

Suitable for:

* Country
* State
* Region
* Department
* Product Category
* Gender
* Payment Method

Avoid categories for columns with mostly unique values, such as:

* Customer Name
* Email
* Order ID

---

# Business Example

A telecom company stores 20 million customer records.

Columns include:

* Customer ID
* State
* Plan Type
* Gender
* Monthly Charges

By converting **State**, **Plan Type**, and **Gender** to the `category` data type, the analytics team significantly reduces memory usage, allowing larger datasets to fit into available RAM.

---

# Best Practices

✔ Measure memory before optimizing.

✔ Use the smallest safe numeric type.

✔ Convert repeated text columns to `category`.

✔ Review data types after importing files.

✔ Re-measure memory after optimization.

---

# Common Mistakes

### Converting Numbers to Very Small Types

Example:

```python id="mistake01"
df["Age"].astype("int8")
```

This is safe only if values fit within the `int8` range.

Always verify the minimum and maximum values first.

---

### Converting Unique Identifiers to Categories

Columns like:

* Customer ID
* Invoice Number
* Transaction ID

usually have many unique values and typically do not benefit from categorical conversion.

---

### Ignoring Object Columns

Object columns are often the largest contributors to memory usage.

Inspect them carefully using:

```python id="mistake02"
df.memory_usage(
    deep=True
)
```

---

# Key Takeaways

After completing this section, you should understand:

* How to measure DataFrame memory usage.
* How to optimize integer and float data types.
* When categorical variables reduce memory.
* Why memory optimization improves scalability.
* How efficient data types enable faster analytics.

> **"Performance optimization begins with understanding how data is stored. Efficient data types reduce memory usage, improve execution speed, and make large-scale analytics practical."**
