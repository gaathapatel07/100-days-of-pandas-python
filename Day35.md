
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

# 8. Vectorization vs Loops

One of the biggest performance advantages of Pandas is **vectorization**.

Instead of processing one row at a time, vectorized operations process entire columns at once.

---

## ❌ Using a Loop

```python id="loop01"
for i in range(len(df)):
    df.loc[i, "Revenue"] = (
        df.loc[i, "Quantity"] *
        df.loc[i, "Price"]
    )
```

This is slow because every iteration accesses the DataFrame individually.

---

## ✅ Vectorized Approach

```python id="vector01"
df["Revenue"] = (
    df["Quantity"] *
    df["Price"]
)
```

Advantages:

* Faster execution
* Cleaner code
* Lower overhead
* Better scalability

---

# 9. `apply()` vs Vectorized Operations

`apply()` is useful for custom logic but is usually slower than built-in vectorized operations.

---

## Using `apply()`

```python id="apply01"
df["Tax"] = (
    df["Sales"]
      .apply(
          lambda x: x * 0.18
      )
)
```

---

## Preferred Vectorized Version

```python id="apply02"
df["Tax"] = (
    df["Sales"] * 0.18
)
```

Whenever possible, prefer built-in arithmetic and string methods.

---

# 10. Efficient Filtering with `query()`

Instead of:

```python id="query01"
filtered = df[
    (df["Sales"] > 5000)
    &
    (df["Region"] == "North")
]
```

Use:

```python id="query02"
filtered = df.query(
    "Sales > 5000 and Region == 'North'"
)
```

Benefits:

* Cleaner syntax
* Easier to read
* Can be faster for complex filtering

---

# 11. Faster Calculations with `eval()`

Suppose you want to calculate revenue.

Traditional approach:

```python id="eval01"
df["Revenue"] = (
    df["Quantity"] *
    df["Price"]
)
```

Using `eval()`:

```python id="eval02"
df.eval(
    "Revenue = Quantity * Price",
    inplace=True
)
```

Advantages:

* Reduced temporary objects
* Lower memory usage
* Faster on large DataFrames

---

# 12. Chunk Processing

Very large datasets may not fit into memory.

Process them in smaller chunks.

```python id="chunk01"
chunks = pd.read_csv(
    "large_sales.csv",
    chunksize=50000
)

for chunk in chunks:

    print(
        chunk.shape
    )
```

Each chunk is processed independently.

Example workflow:

```python id="chunk02"
total_sales = 0

for chunk in pd.read_csv(
    "large_sales.csv",
    chunksize=50000
):

    total_sales += (
        chunk["Sales"].sum()
    )

print(total_sales)
```

This approach allows processing datasets much larger than available RAM.

---

# 13. Sparse Data Structures

Some datasets contain many repeated or missing values.

Example:

| Customer | Coupon Used |
| -------- | ----------- |
| A        | 0           |
| B        | 0           |
| C        | 0           |
| D        | 1           |
| E        | 0           |

Convert to sparse storage.

```python id="sparse01"
df["Coupon Used"] = (
    df["Coupon Used"]
      .astype(
          "Sparse[int]"
      )
)
```

Sparse arrays reduce memory consumption when most values are identical or missing.

---

# 14. Performance Benchmarking

Measure execution time using the `time` module.

```python id="bench01"
import time

start = time.time()

df["Revenue"] = (
    df["Quantity"] *
    df["Price"]
)

end = time.time()

print(
    end - start
)
```

For Jupyter Notebook users:

```python id="bench02"
%%time

df["Revenue"] = (
    df["Quantity"] *
    df["Price"]
)
```

This displays execution time directly.

---

# 15. Comparing Common Approaches

| Task               | Slower               | Faster           |
| ------------------ | -------------------- | ---------------- |
| Row calculations   | Loops                | Vectorization    |
| Filtering          | Boolean indexing     | `query()`        |
| Column expressions | Multiple assignments | `eval()`         |
| Large files        | Read all at once     | Chunk processing |
| Repeated strings   | Object               | Category         |
| Sparse values      | Dense arrays         | Sparse arrays    |

---

# Business Example

A banking institution processes **100 million transactions** every month.

Instead of:

* Reading the entire file
* Using loops
* Storing repeated strings

The engineering team:

* Reads data in chunks.
* Uses vectorized calculations.
* Converts categorical columns.
* Uses `query()` for filtering.
* Benchmarks every optimization.

As a result, processing time is significantly reduced while memory usage remains manageable.

---

# Best Practices

✔ Prefer vectorized operations over loops.

✔ Use `query()` for complex filtering.

✔ Use `eval()` for large mathematical expressions.

✔ Process massive datasets in chunks.

✔ Benchmark performance before and after optimization.

---

# Common Mistakes

### Overusing `apply()`

`apply()` is powerful but not always the fastest option.

Always check if a vectorized alternative exists.

---

### Reading Gigantic Files Into Memory

Instead of:

```python id="mistake01"
pd.read_csv(
    "huge_file.csv"
)
```

Prefer:

```python id="mistake02"
chunksize=50000
```

---

### Benchmarking Small Datasets

Optimization benefits become noticeable mainly on medium and large datasets.

Always benchmark with representative data sizes.

---

# Quick Recap

You have now learned how to:

* Replace loops with vectorized operations.
* Compare `apply()` and vectorization.
* Filter efficiently using `query()`.
* Compute expressions using `eval()`.
* Process large datasets in chunks.
* Use sparse data structures.
* Benchmark Pandas performance.

> **"Efficient Pandas code is not just about writing fewer lines—it is about processing more data with less memory and in less time."**

# 16. Enterprise Performance Optimization Workflow

Large organizations follow a structured optimization process before deploying data pipelines.

```text id="workflow01"
Raw Dataset
      │
      ▼
Memory Profiling
      │
      ▼
Optimize Data Types
      │
      ▼
Convert Categories
      │
      ▼
Vectorize Operations
      │
      ▼
Chunk Processing
      │
      ▼
Performance Benchmarking
      │
      ▼
Production Deployment
```

Each optimization step improves speed, scalability, and resource utilization.

---

# 17. Building an Optimization Pipeline

Instead of optimizing manually every time, create a reusable function.

```python id="pipeline01"
def optimize_dataframe(df):

    # Convert repeated strings
    if "Region" in df.columns:
        df["Region"] = (
            df["Region"]
              .astype("category")
        )

    # Optimize integers
    if "Age" in df.columns:
        df["Age"] = (
            pd.to_numeric(
                df["Age"],
                downcast="integer"
            )
        )

    # Optimize floats
    if "Sales" in df.columns:
        df["Sales"] = (
            pd.to_numeric(
                df["Sales"],
                downcast="float"
            )
        )

    return df
```

Run the optimization.

```python id="pipeline02"
optimized_df = optimize_dataframe(df)
```

This creates a consistent optimization workflow for multiple datasets.

---

# 18. Memory Profiling

Measure memory before and after optimization.

Before:

```python id="memory01"
before = (
    df.memory_usage(
        deep=True
    )
    .sum()
)
```

After:

```python id="memory02"
after = (
    optimized_df
    .memory_usage(
        deep=True
    )
    .sum()
)
```

Calculate savings.

```python id="memory03"
saved = (
    before - after
)

print(
    f"Memory Saved: {saved / 1024**2:.2f} MB"
)
```

Tracking improvements helps evaluate optimization strategies.

---

# 19. Performance Tuning Checklist

Before processing a large dataset, review the following checklist:

| Check                             | Status |
| --------------------------------- | ------ |
| Measure memory usage              | ☐      |
| Optimize numeric types            | ☐      |
| Convert repeated text to category | ☐      |
| Replace loops with vectorization  | ☐      |
| Use `query()` for filtering       | ☐      |
| Use `eval()` for expressions      | ☐      |
| Process files in chunks           | ☐      |
| Benchmark execution time          | ☐      |

Following this checklist helps prevent common performance issues.

---

# 20. Enterprise Case Study

## Scenario

You are a **Senior Data Engineer** at **RetailHub**.

Every night, a pipeline processes:

* 150 million sales records
* 30 million customers
* 10 years of historical data

The original pipeline:

* Loads the full dataset into memory.
* Uses loops for calculations.
* Stores repeated text as object data.
* Takes over three hours to complete.

The engineering team optimizes the workflow.

---

## Business Questions

### Question 1

Measure memory usage.

```python id="case01"
df.memory_usage(
    deep=True
)
```

---

### Question 2

Convert regions to categories.

```python id="case02"
df["Region"] = (
    df["Region"]
      .astype("category")
)
```

---

### Question 3

Replace loops.

```python id="case03"
df["Revenue"] = (
    df["Quantity"]
    *
    df["Price"]
)
```

---

### Question 4

Read data in chunks.

```python id="case04"
chunks = pd.read_csv(
    "sales.csv",
    chunksize=100000
)
```

---

### Question 5

Benchmark execution time.

```python id="case05"
import time

start = time.time()

# Processing logic

end = time.time()

print(end - start)
```

---

# 21. Business Insights

After optimization, the analytics team observes:

* Memory consumption decreases substantially after converting repeated string columns to categorical data.
* Vectorized calculations execute much faster than equivalent row-by-row loops.
* Chunk processing enables datasets larger than available RAM to be analyzed successfully.
* Memory profiling identifies inefficient columns early in the pipeline.
* Performance benchmarking helps engineers verify the impact of each optimization.

---

# 22. Practice Exercises

## Beginner

1. Measure DataFrame memory usage.
2. Convert a numeric column to a smaller integer type.
3. Convert a float column to `float32`.
4. Convert a repeated string column to `category`.
5. Compare memory before and after optimization.

---

## Intermediate

6. Replace a loop with vectorized operations.
7. Filter using `query()`.
8. Calculate a new column using `eval()`.
9. Read a CSV file in chunks.
10. Benchmark execution time.

---

## Advanced

11. Build an optimization pipeline.
12. Optimize a large retail dataset.
13. Compare vectorization and `apply()` performance.
14. Profile a large DataFrame.
15. Create a performance tuning checklist for an ETL pipeline.

---

# 23. Interview Questions

## Beginner

1. Why is memory optimization important in Pandas?
2. What does `memory_usage()` return?
3. When should you use categorical data?
4. What is vectorization?
5. Why avoid loops in Pandas?

---

## Intermediate

6. Compare `apply()` and vectorized operations.
7. What does `query()` do?
8. When should `eval()` be used?
9. Why process files in chunks?
10. What are sparse data structures?

---

## Advanced

11. Design a scalable Pandas pipeline for 100 million rows.
12. Explain how data types affect memory usage.
13. How would you optimize an ETL pipeline?
14. Compare chunk processing and loading an entire dataset.
15. Describe your strategy for benchmarking and improving Pandas performance.

---

# 24. Cheat Sheet

| Task             | Syntax                                   |
| ---------------- | ---------------------------------------- |
| Memory Usage     | `memory_usage(deep=True)`                |
| Downcast Integer | `pd.to_numeric(..., downcast="integer")` |
| Downcast Float   | `pd.to_numeric(..., downcast="float")`   |
| Category         | `.astype("category")`                    |
| Vectorization    | `df["A"] * df["B"]`                      |
| Query            | `query()`                                |
| Eval             | `eval()`                                 |
| Chunk Processing | `chunksize=`                             |
| Sparse Type      | `.astype("Sparse[int]")`                 |
| Benchmark        | `time.time()`                            |

---

# 25. Mini Project

## High-Performance Sales Analytics Pipeline

Using any retail, banking, healthcare, logistics, or telecom dataset:

Complete the following tasks:

* Measure initial memory usage.
* Optimize numeric data types.
* Convert repeated text columns to categories.
* Replace loop-based calculations with vectorized operations.
* Use `query()` for filtering.
* Calculate derived columns using `eval()`.
* Read large files using chunk processing.
* Benchmark execution time before and after optimization.
* Write **five executive-level business insights**.
* Recommend **three performance improvements** for future scalability.

### Example Business Insights

* Converting repeated text columns to categorical data significantly reduced memory usage.
* Vectorized calculations improved processing speed compared with iterative approaches.
* Chunk processing enabled efficient handling of datasets exceeding available memory.
* Downcasting numeric types reduced storage requirements without affecting analytical results.
* Regular performance benchmarking identified optimization opportunities throughout the pipeline.

---

# 26. Summary

Congratulations! 🎉

Today you mastered **Advanced Performance Optimization & Memory Management** in Pandas.

You learned how to:

* Measure memory consumption.
* Optimize numeric data types.
* Use categorical variables effectively.
* Replace loops with vectorized operations.
* Apply `query()` and `eval()` for efficient computations.
* Process massive datasets in chunks.
* Benchmark and optimize performance.

These skills are essential for production ETL pipelines, cloud analytics, large-scale business intelligence, and enterprise data engineering.

---

# 27. What's Next?

In **Day 36**, you'll learn **Advanced Visualization with Pandas**.

Topics include:

* Built-in Pandas plotting
* Line, Bar, Area, Histogram, Box, Scatter, and Pie charts
* Subplots
* Plot customization
* Time-series visualization
* Grouped visualizations
* Business dashboards
* Statistical visualizations
* Best visualization practices

Visualization is a crucial step in communicating insights effectively and supports exploratory data analysis, executive reporting, and storytelling with data.

---

<div align="center">

