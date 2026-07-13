# 🐼 Day 42 — Advanced Pandas Method Chaining & Pipeline Design

<div align="center">

# 100 Days of Pandas

### Day 42 · Writing Clean, Readable & Production-Ready Pandas Code

*"Readable code is easier to debug, maintain, and scale. Method chaining turns complex transformations into elegant data pipelines."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Method%20Chaining-blue)
![Day](https://img.shields.io/badge/Day-42-orange)

</div>

---

# 📚 Table of Contents

1. Introduction
2. Why Method Chaining?
3. Learning Objectives
4. Basic Method Chaining
5. The `assign()` Method
6. The `query()` Method
7. Summary

---

# 1. Introduction

As data projects grow, performing transformations step by step often leads to code like this:

```python id="intro01"
df1 = df.dropna()

df2 = df1.query(
    "Sales > 5000"
)

df3 = df2.assign(
    Profit=df2["Revenue"] - df2["Cost"]
)

df4 = df3.sort_values(
    "Profit",
    ascending=False
)
```

Although correct, this approach creates unnecessary intermediate variables.

Method chaining keeps transformations together.

---

# 2. Why Method Chaining?

Instead of creating multiple temporary DataFrames, chain operations together.

Example:

```python id="intro02"
result = (

    df

    .dropna()

    .query(
        "Sales > 5000"
    )

    .assign(
        Profit=lambda x:
        x["Revenue"] - x["Cost"]
    )

    .sort_values(
        "Profit",
        ascending=False
    )

)
```

Advantages:

* More readable
* Easier to maintain
* Fewer temporary variables
* Cleaner transformation flow

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Write clean chained operations.
* Create new columns with `assign()`.
* Filter data using `query()`.
* Improve readability.
* Build reusable transformation pipelines.

---

# 4. Basic Method Chaining

Example:

```python id="chain01"
result = (

    df

    .drop_duplicates()

    .dropna()

    .sort_values(
        "Sales",
        ascending=False
    )

    .head(10)

)
```

Each method returns a new DataFrame, allowing the next method to continue the chain.

---

## Combining Multiple Operations

```python id="chain02"
top_sales = (

    df

    .query(
        "Sales > 10000"
    )

    .sort_values(
        "Sales",
        ascending=False
    )

    .reset_index(
        drop=True
    )

)
```

The flow is easier to follow than multiple assignments.

---

# 5. Using `assign()`

`assign()` creates new columns without modifying the original DataFrame.

Example:

```python id="assign01"
result = (

    df

    .assign(

        Profit=lambda x:
        x["Revenue"] - x["Cost"]

    )

)
```

---

## Multiple Columns

```python id="assign02"
result = (

    df

    .assign(

        Profit=lambda x:
        x["Revenue"] - x["Cost"],

        Margin=lambda x:
        (
            x["Profit"]
            /
            x["Revenue"]
        ) * 100

    )

)
```

Each new column can reference columns created earlier in the same `assign()` call.

---

# 6. Using `query()`

`query()` filters rows using readable expressions.

Example:

```python id="query01"
filtered = (

    df

    .query(
        "Sales > 5000"
    )

)
```

---

## Multiple Conditions

```python id="query02"
filtered = (

    df

    .query(

        "Sales > 5000 and Region == 'North'"

    )

)
```

---

## Using Variables

```python id="query03"
limit = 10000

filtered = (

    df

    .query(
        "Sales > @limit"
    )

)
```

The `@` symbol allows Python variables inside `query()`.

---

# Business Example

A retail analyst needs to prepare a sales report.

Pipeline:

```python id="business01"
report = (

    df

    .dropna()

    .query(
        "Revenue > 5000"
    )

    .assign(

        Profit=lambda x:
        x["Revenue"] - x["Cost"]

    )

    .sort_values(
        "Profit",
        ascending=False
    )

)
```

This entire transformation is completed in one readable pipeline.

---

# Best Practices

✔ Keep one method per line.

✔ Use parentheses around long chains.

✔ Use meaningful indentation.

✔ Prefer `assign()` over repeated column assignments within pipelines.

✔ Use `query()` for readable filtering.

---

# Common Mistakes

### Creating Too Many Intermediate DataFrames

Avoid:

```python id="mistake01"
df1

df2

df3

df4
```

Prefer a single transformation pipeline.

---

### Writing Long Chains on One Line

Incorrect:

```python id="mistake02"
df.dropna().sort_values().query().assign()
```

Split methods across multiple lines for readability.

---

### Forgetting Parentheses

Wrap long chains in parentheses:

```python id="mistake03"
result = (

    df

    .dropna()

)
```

---

# Key Takeaways

After completing this section, you should understand:

* Why method chaining improves readability.
* How to chain multiple transformations.
* How `assign()` creates new columns.
* How `query()` simplifies filtering.
* Why pipelines reduce unnecessary intermediate variables.

> **"Method chaining transforms scattered data manipulation into a clean, readable workflow that is easier to understand, test, and maintain."**

# 8. The `pipe()` Function

The `pipe()` method passes a DataFrame to a custom function, making pipelines modular and reusable.

Basic syntax:

```python id="pipe01"
result = (
    df
    .pipe(function_name)
)
```

---

## Example

Create a custom function.

```python id="pipe02"
def remove_negative_sales(df):

    return (
        df[
            df["Sales"] >= 0
        ]
    )
```

Use it in a pipeline.

```python id="pipe03"
result = (

    df

    .pipe(
        remove_negative_sales
    )

)
```

---

## Passing Arguments

Functions can accept additional parameters.

```python id="pipe04"
def filter_sales(df, limit):

    return (
        df[
            df["Sales"] > limit
        ]
    )
```

Use:

```python id="pipe05"
result = (

    df

    .pipe(
        filter_sales,
        limit=5000
    )

)
```

---

# 9. Creating Custom Transformation Functions

Reusable functions make pipelines cleaner.

Example:

```python id="custom01"
def add_profit(df):

    return (

        df.assign(

            Profit=
            df["Revenue"]
            -
            df["Cost"]

        )

    )
```

Another function:

```python id="custom02"
def sort_profit(df):

    return (

        df.sort_values(

            "Profit",

            ascending=False

        )

    )
```

Pipeline:

```python id="custom03"
result = (

    df

    .pipe(add_profit)

    .pipe(sort_profit)

)
```

---

# 10. Functional Programming with Pandas

Instead of modifying data step by step:

```python id="functional01"
df = df.dropna()

df = df.sort_values("Sales")

df = df.reset_index(drop=True)
```

Use:

```python id="functional02"
result = (

    df

    .dropna()

    .sort_values(
        "Sales"
    )

    .reset_index(
        drop=True
    )

)
```

Functional programming avoids unintended side effects and improves readability.

---

# 11. Debugging Pipelines

Long pipelines can be difficult to debug.

---

## Save Intermediate Results

```python id="debug01"
step1 = (

    df

    .dropna()

)

step2 = (

    step1

    .query(
        "Sales > 5000"
    )

)
```

---

## Inspect Shape

```python id="debug02"
print(
    df.shape
)
```

After filtering:

```python id="debug03"
print(
    step2.shape
)
```

---

## Use `pipe()` for Debugging

```python id="debug04"
def check_rows(df):

    print(df.shape)

    return df
```

Pipeline:

```python id="debug05"
result = (

    df

    .pipe(check_rows)

    .dropna()

    .pipe(check_rows)

)
```

This lets you inspect the DataFrame without interrupting the pipeline.

---

# 12. Reusable Data Pipelines

Example:

```python id="pipeline01"
def sales_pipeline(df):

    return (

        df

        .drop_duplicates()

        .dropna()

        .assign(

            Profit=lambda x:

            x["Revenue"]

            -

            x["Cost"]

        )

        .query(
            "Profit > 0"
        )

    )
```

Execute:

```python id="pipeline02"
clean_sales = (
    sales_pipeline(df)
)
```

Reusable pipelines improve consistency across projects.

---

# 13. Enterprise Pipeline Pattern

Professional projects often separate transformations into logical stages.

```text id="pattern01"
Load Data

↓

Clean Data

↓

Validate Data

↓

Feature Engineering

↓

Aggregation

↓

Visualization

↓

Reporting

↓

Machine Learning
```

Each stage should perform a single responsibility.

---

# 14. Pipeline Performance Tips

### Avoid Unnecessary Copies

Prefer chaining methods over creating many temporary DataFrames.

---

### Filter Early

Instead of processing the full dataset:

```python id="perf01"
result = (

    df

    .query(
        "Sales > 1000"
    )

    .groupby(
        "Region"
    )

    .sum()

)
```

Filtering first reduces later computations.

---

### Vectorized Operations

Avoid loops.

Instead:

```python id="perf02"
df["Profit"] = (

    df["Revenue"]

    -

    df["Cost"]

)
```

Vectorized operations are significantly faster.

---

# 15. Business Example

A financial analyst prepares a quarterly report.

Pipeline:

```python id="business01"
report = (

    df

    .drop_duplicates()

    .dropna()

    .assign(

        Profit=lambda x:

        x["Revenue"]

        -

        x["Cost"]

    )

    .query(
        "Profit > 1000"
    )

    .groupby(
        "Region"
    )

    .agg(

        Revenue=("Revenue","sum"),

        Profit=("Profit","sum")

    )

)
```

This pipeline performs cleaning, transformation, filtering, aggregation, and reporting in one readable workflow.

---

# Best Practices

✔ Keep functions small and focused.

✔ Use `pipe()` for reusable logic.

✔ Keep one transformation per line.

✔ Test custom functions independently.

✔ Build modular pipelines instead of long scripts.

---

# Common Mistakes

### Writing Huge Functions

Avoid one function that performs every transformation.

Break the workflow into smaller reusable functions.

---

### Mutating the Original DataFrame

Prefer returning a new transformed DataFrame instead of modifying the original in place.

---

### Ignoring Readability

A slightly longer but well-structured pipeline is usually easier to maintain than a compact but difficult-to-read implementation.

---

# Quick Recap

You have now learned how to:

* Use `pipe()`.
* Build reusable transformation functions.
* Apply functional programming concepts.
* Debug pipelines.
* Design reusable workflows.
* Optimize pipeline performance.

> **"Well-designed data pipelines are modular, reusable, and easy to understand. Clean pipeline design reduces bugs, improves collaboration, and simplifies maintenance."**

# 8. The `pipe()` Function

The `pipe()` method passes a DataFrame to a custom function, making pipelines modular and reusable.

Basic syntax:

```python id="pipe01"
result = (
    df
    .pipe(function_name)
)
```

---

## Example

Create a custom function.

```python id="pipe02"
def remove_negative_sales(df):

    return (
        df[
            df["Sales"] >= 0
        ]
    )
```

Use it in a pipeline.

```python id="pipe03"
result = (

    df

    .pipe(
        remove_negative_sales
    )

)
```

---

## Passing Arguments

Functions can accept additional parameters.

```python id="pipe04"
def filter_sales(df, limit):

    return (
        df[
            df["Sales"] > limit
        ]
    )
```

Use:

```python id="pipe05"
result = (

    df

    .pipe(
        filter_sales,
        limit=5000
    )

)
```

---

# 9. Creating Custom Transformation Functions

Reusable functions make pipelines cleaner.

Example:

```python id="custom01"
def add_profit(df):

    return (

        df.assign(

            Profit=
            df["Revenue"]
            -
            df["Cost"]

        )

    )
```

Another function:

```python id="custom02"
def sort_profit(df):

    return (

        df.sort_values(

            "Profit",

            ascending=False

        )

    )
```

Pipeline:

```python id="custom03"
result = (

    df

    .pipe(add_profit)

    .pipe(sort_profit)

)
```

---

# 10. Functional Programming with Pandas

Instead of modifying data step by step:

```python id="functional01"
df = df.dropna()

df = df.sort_values("Sales")

df = df.reset_index(drop=True)
```

Use:

```python id="functional02"
result = (

    df

    .dropna()

    .sort_values(
        "Sales"
    )

    .reset_index(
        drop=True
    )

)
```

Functional programming avoids unintended side effects and improves readability.

---

# 11. Debugging Pipelines

Long pipelines can be difficult to debug.

---

## Save Intermediate Results

```python id="debug01"
step1 = (

    df

    .dropna()

)

step2 = (

    step1

    .query(
        "Sales > 5000"
    )

)
```

---

## Inspect Shape

```python id="debug02"
print(
    df.shape
)
```

After filtering:

```python id="debug03"
print(
    step2.shape
)
```

---

## Use `pipe()` for Debugging

```python id="debug04"
def check_rows(df):

    print(df.shape)

    return df
```

Pipeline:

```python id="debug05"
result = (

    df

    .pipe(check_rows)

    .dropna()

    .pipe(check_rows)

)
```

This lets you inspect the DataFrame without interrupting the pipeline.

---

# 12. Reusable Data Pipelines

Example:

```python id="pipeline01"
def sales_pipeline(df):

    return (

        df

        .drop_duplicates()

        .dropna()

        .assign(

            Profit=lambda x:

            x["Revenue"]

            -

            x["Cost"]

        )

        .query(
            "Profit > 0"
        )

    )
```

Execute:

```python id="pipeline02"
clean_sales = (
    sales_pipeline(df)
)
```

Reusable pipelines improve consistency across projects.

---

# 13. Enterprise Pipeline Pattern

Professional projects often separate transformations into logical stages.

```text id="pattern01"
Load Data

↓

Clean Data

↓

Validate Data

↓

Feature Engineering

↓

Aggregation

↓

Visualization

↓

Reporting

↓

Machine Learning
```

Each stage should perform a single responsibility.

---

# 14. Pipeline Performance Tips

### Avoid Unnecessary Copies

Prefer chaining methods over creating many temporary DataFrames.

---

### Filter Early

Instead of processing the full dataset:

```python id="perf01"
result = (

    df

    .query(
        "Sales > 1000"
    )

    .groupby(
        "Region"
    )

    .sum()

)
```

Filtering first reduces later computations.

---

### Vectorized Operations

Avoid loops.

Instead:

```python id="perf02"
df["Profit"] = (

    df["Revenue"]

    -

    df["Cost"]

)
```

Vectorized operations are significantly faster.

---

# 15. Business Example

A financial analyst prepares a quarterly report.

Pipeline:

```python id="business01"
report = (

    df

    .drop_duplicates()

    .dropna()

    .assign(

        Profit=lambda x:

        x["Revenue"]

        -

        x["Cost"]

    )

    .query(
        "Profit > 1000"
    )

    .groupby(
        "Region"
    )

    .agg(

        Revenue=("Revenue","sum"),

        Profit=("Profit","sum")

    )

)
```

This pipeline performs cleaning, transformation, filtering, aggregation, and reporting in one readable workflow.

---

# Best Practices

✔ Keep functions small and focused.

✔ Use `pipe()` for reusable logic.

✔ Keep one transformation per line.

✔ Test custom functions independently.

✔ Build modular pipelines instead of long scripts.

---

# Common Mistakes

### Writing Huge Functions

Avoid one function that performs every transformation.

Break the workflow into smaller reusable functions.

---

### Mutating the Original DataFrame

Prefer returning a new transformed DataFrame instead of modifying the original in place.

---

### Ignoring Readability

A slightly longer but well-structured pipeline is usually easier to maintain than a compact but difficult-to-read implementation.

---

# Quick Recap

You have now learned how to:

* Use `pipe()`.
* Build reusable transformation functions.
* Apply functional programming concepts.
* Debug pipelines.
* Design reusable workflows.
* Optimize pipeline performance.

> **"Well-designed data pipelines are modular, reusable, and easy to understand. Clean pipeline design reduces bugs, improves collaboration, and simplifies maintenance."**
