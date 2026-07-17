# 🐼 Day 50 — Advanced Pandas Performance Optimization

## 📖 Introduction

Pandas is one of the most powerful data analysis libraries, but as datasets grow from thousands to millions of rows, inefficient code can become slow and memory-intensive.

Performance optimization is the process of writing Pandas code that executes faster, consumes less memory, and scales efficiently for large datasets.

Data Analysts, Data Scientists, and Machine Learning Engineers frequently optimize Pandas workflows before deploying them in production.

---

# 📚 Topics Covered

- Why Performance Optimization Matters
- Measuring Performance
- Memory Optimization
- Efficient Data Types
- Vectorization
- Avoiding Loops
- Apply vs Vectorization
- Query Optimization
- Efficient Filtering
- Performance Benchmarking

---

# 1. Why Performance Optimization Matters

Suppose you have:

- 1,000 rows → Almost any solution works.
- 100,000 rows → Poor code becomes noticeable.
- 10 million rows → Inefficient code may take several minutes or even hours.

Optimized code provides:

- Faster execution
- Lower memory usage
- Better scalability
- Lower cloud computing costs

---

# 2. Learning Objectives

By the end of this lesson, you will be able to:

- Measure execution time.
- Reduce memory usage.
- Use efficient data types.
- Write vectorized code.
- Optimize filtering operations.
- Benchmark different approaches.

---

# 3. Measuring Performance

Python's `time` module measures execution time.

```python
import time

start = time.time()

df["Total"] = df["Price"] * df["Quantity"]

end = time.time()

print(end - start)
```

For Jupyter Notebook:

```python
%%time

df["Total"] = df["Price"] * df["Quantity"]
```

Average execution time.

```python
%timeit df["Price"] * df["Quantity"]
```

---

# 4. Memory Usage

Check memory consumption.

```python
df.memory_usage()
```

Deep memory usage.

```python
df.memory_usage(deep=True)
```

Total memory.

```python
df.memory_usage(deep=True).sum()
```

Convert to MB.

```python
df.memory_usage(deep=True).sum() / 1024**2
```

Example Output

| Column | Memory |
|---------|--------:|
|Customer|4.5 MB|
|Region|2.1 MB|
|Sales|0.8 MB|

---

# 5. Efficient Data Types

Choosing appropriate data types reduces memory usage.

Current data types.

```python
df.dtypes
```

Convert integer type.

```python
df["Age"] = df["Age"].astype("int16")
```

Convert float type.

```python
df["Sales"] = df["Sales"].astype("float32")
```

Convert category.

```python
df["Region"] = df["Region"].astype("category")
```

Memory comparison.

```python
df.info(memory_usage="deep")
```

---

# 6. Why Category Data Type?

Repeated text values consume unnecessary memory.

Before

```
North
South
East
West
North
South
```

Instead of storing each string repeatedly, Pandas stores an integer code.

```python
df["Region"] = df["Region"].astype("category")
```

Benefits:

- Smaller memory footprint
- Faster grouping
- Faster sorting

---

# 7. Vectorization

Vectorization performs operations on entire columns instead of individual rows.

Vectorized approach.

```python
df["Revenue"] = df["Price"] * df["Quantity"]
```

Loop approach (Not Recommended).

```python
for i in range(len(df)):
    df.loc[i, "Revenue"] = (
        df.loc[i, "Price"] *
        df.loc[i, "Quantity"]
    )
```

Vectorized operations are significantly faster because they are implemented in optimized C code.

---

# 8. Avoiding Loops

Instead of:

```python
result = []

for value in df["Sales"]:

    result.append(value * 1.18)

df["GST"] = result
```

Use:

```python
df["GST"] = df["Sales"] * 1.18
```

Benefits:

- Cleaner code
- Faster execution
- Better scalability

---

# 9. Apply vs Vectorization

Using `apply()`.

```python
df["Square"] = df["Sales"].apply(
    lambda x: x**2
)
```

Vectorized alternative.

```python
df["Square"] = df["Sales"] ** 2
```

Comparison

| Method | Speed |
|---------|------:|
|Loop|Slow|
|apply()|Medium|
|Vectorization|Fastest|

Use vectorized operations whenever possible.

---

# 10. Query Optimization

Instead of:

```python
df[
    (df["Sales"] > 5000) &
    (df["Region"] == "West")
]
```

Use:

```python
df.query(
    'Sales > 5000 and Region == "West"'
)
```

Advantages:

- Cleaner syntax
- Easier to read
- Often faster for complex filters

---

# 11. Efficient Filtering

Use Boolean indexing.

```python
high_sales = df[df["Sales"] > 5000]
```

Multiple conditions.

```python
filtered = df[
    (df["Sales"] > 5000) &
    (df["Profit"] > 1000)
]
```

Use `.isin()` for multiple values.

```python
df[
    df["Region"].isin(
        ["North", "West"]
    )
]
```

---

# 12. Performance Benchmarking

Compare execution time of different approaches.

Example:

```python
%timeit df["Sales"] * 2
```

```python
%timeit df["Sales"].apply(
    lambda x: x * 2
)
```

Typical Results

| Method | Time |
|---------|------:|
|Loop|520 ms|
|apply()|85 ms|
|Vectorized|5 ms|

The vectorized approach can be **10–100× faster** depending on the dataset.

---

# Business Example

A retail company processes 25 million transaction records every night.

After optimization:

- Converted object columns to `category`
- Replaced loops with vectorized operations
- Used `query()` for filtering
- Reduced memory usage by 60%
- Reduced processing time from 40 minutes to under 8 minutes

These improvements allowed reports to be generated before the start of the business day.

---

# Best Practices

✔ Use vectorized operations whenever possible.

✔ Choose the smallest appropriate data type.

✔ Convert repeated text columns to `category`.

✔ Measure execution time before optimizing.

✔ Optimize only after identifying bottlenecks.

---

# Common Mistakes

### Using Loops for Column Operations

Loops are much slower than vectorized operations.

---

### Ignoring Memory Usage

Large object columns can consume significant memory unnecessarily.

---

### Premature Optimization

Always profile your code before spending time optimizing it.

---

# Key Takeaways

After completing this section, you should understand:

- Measuring execution time
- Memory optimization
- Efficient data types
- Vectorization
- Avoiding loops
- Apply vs vectorization
- Query optimization
- Efficient filtering
- Performance benchmarking

> **"Efficient Pandas code is not just about speed—it enables scalable analytics, reduces resource consumption, and supports production-ready data pipelines."**

---

## Next (Day 50 – Part 2)

The next section covers:

- Chunk Processing
- Efficient GroupBy Operations
- Optimizing Merge & Join
- Sorting Performance
- Index Optimization
- Copy vs View
- Parallel Processing Concepts
- Large Dataset Strategies
- Enterprise Performance Optimization
