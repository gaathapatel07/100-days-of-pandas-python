# Day 43 — Advanced Performance Optimization & Memory Management in Pandas

## Introduction

As datasets grow larger, inefficient Pandas code can become slow and memory-intensive. Performance optimization techniques help process millions of rows efficiently while reducing memory consumption.

---

# Topics Covered

- Memory Profiling
- Optimizing Data Types
- Categorical Data
- Efficient Indexing
- Vectorization vs Loops
- Chunk Processing
- Performance Benchmarking
- Memory Optimization
- Enterprise Optimization Workflow
- Large Dataset Strategy

---

# 1. Memory Profiling

Check memory usage of the DataFrame.

```python
df.info(memory_usage="deep")
```

Column-wise memory usage.

```python
df.memory_usage(deep=True)
```

Total memory usage.

```python
df.memory_usage(deep=True).sum() / 1024**2
```

Example Output

```
284.52 MB
```

---

# 2. Optimizing Data Types

Convert integer columns to smaller data types.

```python
df["Quantity"] = df["Quantity"].astype("int16")
```

Downcast float columns.

```python
df["Sales"] = pd.to_numeric(
    df["Sales"],
    downcast="float"
)
```

Automatically downcast integers.

```python
df["Age"] = pd.to_numeric(
    df["Age"],
    downcast="integer"
)
```

Benefits:

- Lower memory usage
- Faster processing
- Better scalability

---

# 3. Using Categorical Data

Repeated text values consume significant memory.

Convert object columns to category.

```python
df["Region"] = df["Region"].astype("category")
```

Example Memory Usage

| Data Type | Memory |
|-----------|---------|
| object | 48 MB |
| category | 3 MB |

Suitable columns:

- Country
- Region
- Department
- Gender
- Product Category

---

# 4. Efficient Indexing

Create an index.

```python
df = df.set_index("Customer ID")
```

Access rows efficiently.

```python
df.loc[10025]
```

Reset index.

```python
df.reset_index()
```

---

# 5. Vectorization vs Loops

Slow approach:

```python
profits = []

for i in range(len(df)):
    profits.append(
        df.loc[i, "Revenue"] -
        df.loc[i, "Cost"]
    )
```

Fast vectorized approach:

```python
df["Profit"] = df["Revenue"] - df["Cost"]
```

Vectorized operations are significantly faster.

---

# 6. Chunk Processing

Process very large CSV files in chunks.

```python
chunks = pd.read_csv(
    "sales.csv",
    chunksize=100000
)

for chunk in chunks:
    process(chunk)
```

Useful for:

- ETL pipelines
- Large CSV files
- Limited RAM environments

---

# 7. Performance Benchmarking

Measure execution time.

```python
import time

start = time.time()

df.groupby("Region")["Sales"].sum()

end = time.time()

print(end - start)
```

In Jupyter Notebook:

```python
%%time
```

Average execution time:

```python
%timeit df["Sales"].mean()
```

---

# 8. Memory Optimization Techniques

Read only required columns.

```python
pd.read_csv(
    "sales.csv",
    usecols=[
        "Sales",
        "Region"
    ]
)
```

Specify data types during import.

```python
pd.read_csv(
    "sales.csv",
    dtype={
        "Region": "category",
        "Quantity": "int16"
    }
)
```

Parse dates during import.

```python
pd.read_csv(
    "sales.csv",
    parse_dates=["Order Date"]
)
```

---

# 9. Enterprise Optimization Workflow

```
Import Data
      │
      ▼
Profile Memory
      │
      ▼
Optimize Data Types
      │
      ▼
Convert Categories
      │
      ▼
Index Frequently Used Columns
      │
      ▼
Vectorize Calculations
      │
      ▼
Chunk Large Files
      │
      ▼
Benchmark Performance
      │
      ▼
Production Pipeline
```

---

# 10. Large Dataset Strategy

For datasets containing millions of rows:

- Read only required columns.
- Filter early.
- Convert object columns to category.
- Downcast numeric columns.
- Avoid loops.
- Use vectorized operations.
- Process files in chunks.
- Benchmark code regularly.

---

# 11. Enterprise Case Study

## Scenario

A telecommunications company processes:

- 120 million call records
- 40 GB daily log files

### Optimization

```python
df = pd.read_csv(
    "calls.csv",
    usecols=[
        "Customer",
        "Duration",
        "Region"
    ],
    dtype={
        "Region": "category",
        "Duration": "int16"
    }
)
```

Vectorized calculation.

```python
df["Minutes"] = df["Duration"] / 60
```

Aggregation.

```python
summary = (
    df.groupby("Region")
      .agg(
          Total_Minutes=("Minutes", "sum"),
          Calls=("Minutes", "count")
      )
)
```

Results:

- Memory reduced by 65%
- Runtime reduced from 42 minutes to 8 minutes
- Faster dashboard refresh

---

# Best Practices

- Profile memory before optimizing.
- Downcast numeric columns.
- Convert repeated strings to category.
- Read only required columns.
- Filter early.
- Use vectorized operations.
- Benchmark critical operations.
- Process large files in chunks.

---

# Common Mistakes

### Using Loops

Prefer vectorized operations.

### Leaving Everything as object

Convert repeated strings to category.

### Loading Entire Files

Read only necessary rows and columns.

### Ignoring Memory Usage

Large datasets may fail because of insufficient RAM.

---

# Practice Exercises

## Beginner

1. Measure DataFrame memory usage.
2. Convert a column to category.
3. Downcast integer columns.
4. Create an index.
5. Compare memory before and after optimization.

## Intermediate

6. Replace loops with vectorized operations.
7. Import selected columns using usecols.
8. Specify dtypes while importing CSV.
9. Benchmark a groupby operation.
10. Process CSV files in chunks.

## Advanced

11. Optimize a 5 GB dataset.
12. Build a memory profiling function.
13. Benchmark multiple optimization methods.
14. Design a scalable ETL pipeline.
15. Reduce memory usage by at least 50%.

---

# Interview Questions

### Beginner

1. Why is performance optimization important?
2. What is memory profiling?
3. Why use category dtype?
4. What is downcasting?
5. What is vectorization?

### Intermediate

6. Explain chunk processing.
7. Compare loops with vectorized operations.
8. How do indexes improve performance?
9. How do you benchmark Pandas code?
10. Why specify dtype during import?

### Advanced

11. Optimize a dataset with 100 million rows.
12. Design a scalable ETL pipeline.
13. Explain memory optimization techniques.
14. Compare CSV and Parquet performance.
15. Reduce dashboard refresh time for enterprise reports.

---

# Cheat Sheet

| Task | Syntax |
|------|--------|
| Memory Usage | `memory_usage()` |
| Deep Memory | `info(memory_usage="deep")` |
| Downcast | `pd.to_numeric(..., downcast=)` |
| Category | `.astype("category")` |
| Read Columns | `usecols=` |
| Chunk Processing | `chunksize=` |
| Vectorization | `df["A"] + df["B"]` |
| Benchmark | `%timeit` |
| Set Index | `set_index()` |
| Reset Index | `reset_index()` |

---

# Mini Project

## High-Performance Retail Analytics

Tasks:

- Profile memory.
- Downcast numeric columns.
- Convert repeated text columns to category.
- Read selected columns.
- Benchmark processing time.
- Compare loop vs vectorized implementation.
- Process large files in chunks.
- Generate KPIs.
- Write five optimization insights.
- Recommend three scalability improvements.

Example Insights:

- Category dtype reduced memory significantly.
- Downcasting improved efficiency.
- Vectorized operations outperformed loops.
- Chunk processing enabled analysis of datasets larger than RAM.
- Early filtering reduced computation time.

---

# Summary

Today you learned:

- Memory profiling
- Downcasting
- Category dtype
- Efficient indexing
- Vectorization
- Chunk processing
- Benchmarking
- Performance optimization
- Large dataset handling

These techniques are essential for production-scale analytics, ETL pipelines, business intelligence, and data engineering.

---

# Next Topic

Topics:

- Schema Validation
- Constraint Checking
- Range Validation
- Referential Integrity
- Automated Quality Reports
- Data Quality Metrics
- Enterprise QA Pipelines
