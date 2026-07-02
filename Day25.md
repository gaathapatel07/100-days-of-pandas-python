# Day 25 — Advanced `map()`, `apply()`, `pipe()` & Vectorization

<div align="center">

# 100 Days of Pandas

### Day 25 · Writing Efficient & Pythonic Pandas Code

*"The best Pandas code is not only correct—it is readable, reusable, and optimized for performance."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Data%20Transformation-blue)
![Day](https://img.shields.io/badge/Day-25-orange)

</div>

---


# Table of Contents

1. Introduction
2. Why Data Transformation Matters
3. Learning Objectives
4. Understanding `map()`
5. Using Dictionaries with `map()`
6. Using Functions with `map()`
7. Understanding `replace()`
8. Summary

---

# 1. Introduction

Raw datasets rarely arrive in the exact format required for analysis.

Data Analysts frequently need to:

* Convert codes into descriptive labels.
* Standardize categorical values.
* Create new calculated columns.
* Apply business rules.
* Transform existing data into meaningful features.

Pandas provides several powerful transformation functions that make these tasks concise, efficient, and highly readable.

Among the most commonly used are:

* `map()`
* `replace()`
* `apply()`
* `pipe()`
* Vectorized operations

Understanding when to use each method is essential for writing professional Pandas code.

---

# 2. Why Data Transformation Matters

Imagine a retail company stores payment methods as numeric codes.

| Payment Code |
| -----------: |
|            1 |
|            2 |
|            3 |

These values are difficult for managers to interpret.

Transforming them into descriptive labels makes reports easier to understand.

| Payment Method |
| -------------- |
| Cash           |
| Card           |
| UPI            |

This improves dashboard readability without changing the underlying information.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Transform values using `map()`.
* Replace values efficiently.
* Apply custom functions.
* Write cleaner transformation pipelines.
* Improve code readability and performance.

---

# 4. Understanding `map()`

The `map()` function transforms every value in a **Series**.

It is commonly used for:

* Code translation
* Label mapping
* Value standardization
* Category conversion

---

## Basic Example

```python id="map01"
df["Gender"] = (
    df["Gender"]
      .map({
          "M":"Male",
          "F":"Female"
      })
)
```

### Output

| Original | Result |
| -------- | ------ |
| M        | Male   |
| F        | Female |
| M        | Male   |

Each value is replaced according to the mapping dictionary. 

---

# 5. Using Dictionaries with `map()`

Suppose product categories are stored numerically.

| Code |
| ---: |
|  101 |
|  102 |
|  103 |

Convert them into meaningful labels.

```python id="map02"
category_map = {
    101:"Electronics",
    102:"Furniture",
    103:"Clothing"
}

df["Category"] = (
    df["Code"]
      .map(category_map)
)
```

Output:

| Code | Category    |
| ---: | ----------- |
|  101 | Electronics |
|  102 | Furniture   |
|  103 | Clothing    |

Dictionary-based mapping is fast and easy to maintain.

---

# 6. Using Functions with `map()`

Instead of a dictionary, `map()` can apply a function.

Example:

```python id="map03"
df["Customer"] = (
    df["Customer"]
      .map(str.upper)
)
```

Output:

| Original | Result |
| -------- | ------ |
| Alice    | ALICE  |
| Rahul    | RAHUL  |

Custom lambda functions also work.

```python id="map04"
df["Sales"] = (
    df["Sales"]
      .map(
          lambda x:
          x * 1.10
      )
)
```

Output:

| Sales | Updated Sales |
| ----: | ------------: |
|  1000 |          1100 |
|  2500 |          2750 |

This example increases every sales value by 10%.

---

# 7. Understanding `replace()`

While `map()` works on a Series, `replace()` is more flexible and can replace values in a Series or an entire DataFrame.

---

## Replace a Single Value

```python id="replace01"
df["City"] = (
    df["City"]
      .replace(
          "Bombay",
          "Mumbai"
      )
)
```

---

## Replace Multiple Values

```python id="replace02"
df["City"] = (
    df["City"]
      .replace({
          "Bombay":"Mumbai",
          "Madras":"Chennai",
          "Calcutta":"Kolkata"
      })
)
```

Output:

| Original | Updated |
| -------- | ------- |
| Bombay   | Mumbai  |
| Madras   | Chennai |
| Calcutta | Kolkata |

---

## Replace Across the Entire DataFrame

```python id="replace03"
df.replace(
    {
        "N/A":None,
        "-":None,
        "Unknown":None
    }
)
```

This standardizes missing-value representations across every column.

---

# Business Example

A multinational retailer receives transaction data from several regional offices.

Different offices use different codes:

| Region |
| ------ |
| N      |
| S      |
| E      |
| W      |

Using `map()`, analysts convert these abbreviations into descriptive region names.

Similarly, outdated city names and inconsistent labels are standardized using `replace()` before reports are generated.

---

# Best Practices

✔ Use `map()` for one-to-one value transformations.

✔ Use dictionaries instead of long `if` statements.

✔ Use `replace()` for multiple replacements across a DataFrame.

✔ Keep mapping dictionaries separate from transformation logic.

✔ Validate transformed values after mapping.

---

# Common Mistakes

### Missing Dictionary Keys

Suppose the mapping dictionary is:

```python id="mistake01"
{
    "M":"Male",
    "F":"Female"
}
```

If the dataset contains:

```text id="mistake02"
X
```

The output becomes:

```text id="mistake03"
NaN
```

Always check for unmapped values after using `map()`.

---

### Using `map()` on an Entire DataFrame

Incorrect:

```python id="mistake04"
df.map(...)
```

`map()` is designed for **Series**, not DataFrames.

For DataFrames, consider `replace()` or `apply()` depending on the use case.

---

# Key Takeaways

After completing this section, you should understand:

* How `map()` transforms values.
* The difference between dictionary mapping and function mapping.
* How `replace()` differs from `map()`.
* Why mapping improves readability.
* How standardized values improve business reporting.

> **"Data transformation bridges the gap between raw information and meaningful insights by converting complex codes into understandable business language."**

# 8. Understanding `apply()`

The `apply()` function applies a custom function along a Series or DataFrame.

It is one of the most flexible functions in Pandas.

Typical use cases include:

* Feature engineering
* Data transformation
* Business rule implementation
* Statistical calculations
* Custom aggregations

---

## Applying a Function to a Series

Suppose we want to classify sales.

```python id="apply01"
def sales_category(sales):

    if sales >= 10000:
        return "High"

    elif sales >= 5000:
        return "Medium"

    else:
        return "Low"

df["Sales Category"] = (
    df["Sales"]
      .apply(sales_category)
)
```

### Output

| Sales | Sales Category |
| ----: | -------------- |
| 12000 | High           |
|  6500 | Medium         |
|  3000 | Low            |

---

## Using Lambda Functions

Small transformations are often written using lambda functions.

```python id="apply02"
df["Discounted Price"] = (
    df["Price"]
      .apply(
          lambda x:
          x * 0.9
      )
)
```

Output:

| Price | Discounted |
| ----: | ---------: |
|  1000 |        900 |
|  2500 |       2250 |

Lambda functions make short transformations concise.

---

# 9. Applying Functions Across Rows

`apply()` can also work row-wise.

Example:

Calculate total revenue.

```python id="apply03"
df["Revenue"] = (
    df.apply(
        lambda row:
        row["Quantity"] * row["Price"],
        axis=1
    )
)
```

### Example

| Quantity | Price | Revenue |
| -------: | ----: | ------: |
|        5 |   100 |     500 |
|        3 |   250 |     750 |

Here:

* `axis=1` → Row-wise
* `axis=0` → Column-wise (default)

---

# 10. Applying Functions Across Columns

Calculate the range of each numerical column.

```python id="apply04"
df[
    ["Sales", "Profit"]
].apply(
    lambda column:
    column.max() - column.min()
)
```

Output:

| Column | Range |
| ------ | ----: |
| Sales  |  9500 |
| Profit |  2200 |

---

# 11. `applymap()` and Modern `DataFrame.map()`

Historically, `applymap()` was used to apply a function to **every element** of a DataFrame.

```python id="applymap01"
df = df.applymap(
    lambda value:
    str(value).upper()
)
```

In **modern versions of Pandas**, the recommended method is:

```python id="applymap02"
df = df.map(
    lambda value:
    str(value).upper()
)
```

Both perform element-wise transformations, but `DataFrame.map()` is the modern replacement for `applymap()`.

---

## Example

Original DataFrame:

| City   | State       |
| ------ | ----------- |
| Delhi  | Delhi       |
| Mumbai | Maharashtra |

After mapping:

| City   | State       |
| ------ | ----------- |
| DELHI  | DELHI       |
| MUMBAI | MAHARASHTRA |

---

# 12. Understanding `pipe()`

Complex transformation pipelines become difficult to read.

The `pipe()` function allows transformations to be broken into reusable steps.

---

## Create a Custom Function

```python id="pipe01"
def add_tax(dataframe):

    dataframe["Price"] = (
        dataframe["Price"] * 1.18
    )

    return dataframe
```

Apply it.

```python id="pipe02"
df = (
    df.pipe(add_tax)
)
```

---

## Chaining Multiple Pipes

```python id="pipe03"
df = (
    df
    .pipe(add_tax)
    .pipe(clean_names)
    .pipe(remove_duplicates)
)
```

Each function performs one logical task.

This improves readability and maintainability.

---

# 13. Method Chaining

Instead of creating many intermediate DataFrames:

```python id="chain01"
df = df.dropna()

df = df.sort_values("Sales")

df = df.reset_index(drop=True)
```

Use method chaining.

```python id="chain02"
df = (
    df
    .dropna()
    .sort_values("Sales")
    .reset_index(drop=True)
)
```

Benefits include:

* Cleaner code
* Easier debugging
* Better readability
* Fewer temporary variables

---

# 14. Writing Reusable Functions

Instead of repeating the same cleaning logic:

```python id="reuse01"
def clean_city(city):

    return (
        city
        .strip()
        .title()
    )

df["City"] = (
    df["City"]
      .apply(clean_city)
)
```

Reusable functions reduce duplication and improve maintainability.

---

# 15. Choosing the Right Transformation Method

| Function          | Works On           | Best Use                    |
| ----------------- | ------------------ | --------------------------- |
| `map()`           | Series             | One-to-one mapping          |
| `replace()`       | Series / DataFrame | Replace existing values     |
| `apply()`         | Series / DataFrame | Custom transformations      |
| `DataFrame.map()` | DataFrame          | Element-wise transformation |
| `pipe()`          | DataFrame          | Reusable workflows          |

Choosing the correct function improves both readability and performance.

---

# Business Example

A multinational retailer prepares customer data before generating reports.

Tasks include:

* Mapping payment codes.
* Calculating revenue.
* Standardizing city names.
* Applying tax calculations.
* Removing duplicates.
* Exporting the cleaned dataset.

Using `pipe()` and method chaining, the entire workflow becomes modular and easier to maintain.

---

# Best Practices

✔ Prefer vectorized operations whenever possible.

✔ Use `map()` for simple value replacements.

✔ Use `apply()` for custom business logic.

✔ Write reusable helper functions.

✔ Keep each `pipe()` function focused on a single task.

✔ Chain transformations for better readability.

---

# Common Mistakes

### Overusing `apply()`

Many tasks can be performed using built-in vectorized methods.

Instead of:

```python id="mistake01"
df["Sales"].apply(
    lambda x:
    x * 2
)
```

Prefer:

```python id="mistake02"
df["Sales"] * 2
```

Vectorized operations are usually much faster.

---

### Creating Very Long Lambda Functions

Avoid placing complex business logic inside lambda expressions.

Instead:

```python id="mistake03"
def classify_sales(value):

    if value >= 10000:
        return "High"

    elif value >= 5000:
        return "Medium"

    return "Low"
```

This improves readability and makes testing easier.

---

### Mixing Too Many Responsibilities in One Function

Each custom function should perform one clear task.

For example:

* Clean names
* Calculate tax
* Remove duplicates

rather than combining all operations into one large function.

---

# Quick Recap

You have now learned how to:

* Apply custom functions using `apply()`.
* Use lambda functions for concise transformations.
* Apply row-wise and column-wise operations.
* Perform element-wise transformations with `DataFrame.map()`.
* Build reusable workflows using `pipe()`.
* Write clean method-chained Pandas code.

> **"Well-designed transformation pipelines are easier to understand, easier to maintain, and easier to scale as datasets and business requirements grow."**

# 16. Vectorization

One of the biggest advantages of Pandas is **vectorization**.

Instead of processing one row at a time using loops, vectorized operations apply calculations to an entire column simultaneously.

Vectorization is:

* Faster
* Cleaner
* Easier to read
* Optimized internally using NumPy

---

## Example

Instead of:

```python id="vector01"
result = []

for value in df["Sales"]:
    result.append(value * 1.10)

df["Updated Sales"] = result
```

Use:

```python id="vector02"
df["Updated Sales"] = (
    df["Sales"] * 1.10
)
```

Both produce the same result, but the vectorized version is significantly faster.

---

# 17. Vectorization vs `apply()`

Many beginners use `apply()` for every task.

However, built-in vectorized operations should be preferred whenever possible.

Example:

Using `apply()`:

```python id="vector03"
df["Tax"] = (
    df["Price"]
      .apply(
          lambda x:
          x * 0.18
      )
)
```

Using vectorization:

```python id="vector04"
df["Tax"] = (
    df["Price"] * 0.18
)
```

The second approach is:

* Faster
* More readable
* Better optimized

---

# 18. Performance Comparison

Suppose a DataFrame contains **1 million rows**.

| Method               | Approximate Time |
| -------------------- | ---------------: |
| Python Loop          |      8.5 seconds |
| `apply()`            |      2.4 seconds |
| Vectorized Operation |     0.08 seconds |

The exact timings depend on hardware and the specific operation, but vectorized operations are generally much faster than loops and often faster than `apply()`.

---

# 19. Building a Complete Transformation Pipeline

Professional workflows often combine several operations.

```python id="pipeline01"
def clean_data(dataframe):

    return (
        dataframe
        .drop_duplicates()
        .dropna()
        .assign(
            Revenue=lambda x:
            x["Quantity"] * x["Price"]
        )
        .sort_values("Revenue")
        .reset_index(drop=True)
    )
```

Execute:

```python id="pipeline02"
clean_df = clean_data(df)
```

The transformation remains concise, readable, and reusable.

---

# 20. Enterprise ETL Workflow

Large organizations often process data through a structured pipeline.

```text id="etl01"
Raw Data
    │
    ▼
Read Dataset
    │
    ▼
Clean Missing Values
    │
    ▼
Transform Columns
    │
    ▼
Apply Business Rules
    │
    ▼
Create New Features
    │
    ▼
Validate Data
    │
    ▼
Export Clean Dataset
```

Breaking transformations into small reusable functions makes enterprise code easier to maintain.

---

# 21. Real-World Business Case Study

## Scenario

You are working as a **Senior Data Analyst** for **RetailHub**.

The daily transaction dataset contains:

* Duplicate records
* Missing values
* Payment codes
* Product prices
* Quantities
* Customer cities

Your goal is to prepare the data before creating executive dashboards.

---

## Business Questions

### Question 1

Map payment codes.

```python id="case_apply01"
payment_map = {
    1: "Cash",
    2: "Card",
    3: "UPI"
}

df["Payment"] = (
    df["Payment Code"]
      .map(payment_map)
)
```

---

### Question 2

Calculate revenue.

```python id="case_apply02"
df["Revenue"] = (
    df["Price"] *
    df["Quantity"]
)
```

---

### Question 3

Standardize city names.

```python id="case_apply03"
df["City"] = (
    df["City"]
      .str.strip()
      .str.title()
)
```

---

### Question 4

Remove duplicate records.

```python id="case_apply04"
df = (
    df
      .drop_duplicates()
)
```

---

### Question 5

Generate a reusable pipeline.

```python id="case_apply05"
def prepare_sales(dataframe):

    return (
        dataframe
        .drop_duplicates()
        .dropna()
        .assign(
            Revenue=lambda x:
            x["Price"] * x["Quantity"]
        )
    )

clean_df = prepare_sales(df)
```

---

# 22. Business Insights

After transforming the sales dataset, analysts discover:

* Standardized payment methods improve reporting consistency.
* Vectorized calculations reduce processing time significantly.
* Reusable transformation pipelines simplify maintenance.
* Revenue calculations become consistent across departments.
* Clean, modular code improves collaboration within analytics teams.

---

# 23. Practice Exercises

## Beginner

1. Use `map()` to convert country codes into names.
2. Replace outdated city names.
3. Apply a discount using a lambda function.
4. Create a new revenue column.
5. Standardize customer names.

---

## Intermediate

6. Write a reusable cleaning function.
7. Apply row-wise calculations.
8. Build a method-chaining pipeline.
9. Compare `apply()` with vectorized operations.
10. Replace multiple values using a dictionary.

---

## Advanced

11. Create a complete ETL transformation pipeline.
12. Optimize a slow transformation using vectorization.
13. Build reusable `pipe()` functions.
14. Benchmark transformation performance.
15. Write five recommendations for improving code efficiency.

---

# 24. Interview Questions

## Beginner

1. What is `map()`?
2. What is `replace()`?
3. What is `apply()`?
4. What is a lambda function?
5. What is vectorization?

---

## Intermediate

6. Difference between `map()` and `apply()`?
7. Difference between `apply()` and vectorized operations?
8. What is `pipe()`?
9. Why use method chaining?
10. When should `replace()` be preferred over `map()`?

---

## Advanced

11. Explain an enterprise ETL transformation workflow.
12. Compare loops, `apply()`, and vectorization.
13. Design a reusable transformation pipeline.
14. How would you optimize transformations on a 100-million-row dataset?
15. What coding practices improve maintainability in Pandas projects?

---

# 25. Cheat Sheet

| Task                            | Syntax                   |
| ------------------------------- | ------------------------ |
| Map Values                      | `Series.map()`           |
| Replace Values                  | `replace()`              |
| Apply Function                  | `apply()`                |
| Element-wise DataFrame Function | `DataFrame.map()`        |
| Method Chaining                 | `df.method1().method2()` |
| Pipe Function                   | `pipe()`                 |
| Create Column                   | `assign()`               |
| Vectorized Calculation          | `df["A"] * df["B"]`      |
| Row-wise Apply                  | `apply(axis=1)`          |
| Column-wise Apply               | `apply(axis=0)`          |

---

# 26. Mini Project

## Automated Sales Data Transformation Pipeline

Using any retail, banking, HR, healthcare, or logistics dataset:

Complete the following tasks:

* Standardize categorical values using `map()`.
* Replace inconsistent values.
* Remove duplicates.
* Handle missing values.
* Create calculated columns using vectorized operations.
* Write reusable transformation functions.
* Build a complete method-chaining pipeline.
* Export the processed dataset.
* Write **five executive-level business insights**.
* Recommend **three performance improvements** for the pipeline.

### Example Business Insights

* Vectorized calculations reduced processing time compared with row-wise transformations.
* Standardized payment methods improved reporting consistency.
* Modular transformation functions simplified future maintenance.
* Revenue calculations became consistent across all reports.
* Automated preprocessing reduced manual effort and improved data quality.

---

# 27. Summary

Congratulations! 🎉

Today you mastered **Advanced Data Transformation** in Pandas.

You learned how to:

* Transform values using `map()` and `replace()`.
* Apply custom functions with `apply()`.
* Build reusable workflows using `pipe()`.
* Write clean method-chained code.
* Optimize transformations using vectorization.
* Create production-ready ETL pipelines.

These techniques are fundamental for data engineering, business intelligence, feature engineering, and scalable analytics.

---

# 28. What's Next?

In **Day 26**, you'll learn **Advanced GroupBy, Aggregation & Pivot Analysis**.

Topics include:

* Multi-Level GroupBy
* Multiple Aggregations
* Named Aggregations
* `agg()`
* `transform()`
* `filter()`
* Crosstab
* Pivot Analysis
* Business KPI Reporting
* Advanced Analytical Workflows

These concepts are widely used for executive reporting, business intelligence dashboards, financial analysis, and operational analytics.

---

<div align="center">

# 🎉 Day 25 Complete!

You've learned how to transform, clean, and optimize data using modern Pandas techniques.

By combining vectorization, reusable functions, method chaining, and efficient transformation strategies, you can write clean, scalable, and production-ready code for real-world analytics projects.

⭐ **Next → Day 26: Advanced GroupBy, Aggregation & Pivot Analysis** 📊🐼

</div>
