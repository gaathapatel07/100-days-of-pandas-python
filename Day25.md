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

