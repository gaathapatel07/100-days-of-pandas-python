# Day 34 — Advanced Data Validation, Quality Assurance & Error Detection

<div align="center">

# 100 Days of Pandas

### Day 34 · Building Reliable & Trustworthy Data

*"High-quality decisions require high-quality data. Validation ensures your data is accurate before it becomes insight."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Data%20Validation-blue)
![Day](https://img.shields.io/badge/Day-34-orange)

</div>

---

# Table of Contents

1. Introduction
2. What is Data Validation?
3. Why Data Quality Matters
4. Learning Objectives
5. Schema Validation
6. Data Type Validation
7. Constraint Validation
8. Summary

---

# 1. Introduction

Real-world datasets frequently contain errors.

Common examples include:

* Missing values
* Duplicate records
* Invalid IDs
* Negative quantities
* Impossible dates
* Incorrect data types
* Invalid email addresses
* Inconsistent categorical values

Before analysis begins, every dataset should be validated.

Validation ensures that the data satisfies business rules and technical requirements.

---

# 2. What is Data Validation?

Data validation is the process of checking whether data conforms to predefined rules.

Example:

| Customer ID | Age | Sales |
| ----------: | --: | ----: |
|         101 |  25 |  5200 |
|         102 |  -5 |  6100 |
|         103 | 180 |  4900 |

Here:

* Age cannot be negative.
* Age should not exceed realistic human limits.

Validation identifies these invalid records before they affect analysis.

---

# 3. Why Data Quality Matters

Imagine a banking application.

The customer database contains:

| Customer | Balance |
| -------- | ------: |
| Alice    |   25000 |
| Rahul    | -500000 |
| Priya    |   32000 |

A negative balance might be valid for overdrafts—or it might indicate a data entry error.

Validation helps distinguish legitimate records from suspicious ones based on business rules.

---

# 4. Learning Objectives

By the end of this lesson, you will be able to:

* Validate schemas.
* Check data types.
* Apply business constraints.
* Detect invalid records.
* Build reliable validation pipelines.

---

# 5. Schema Validation

A schema defines the expected structure of a dataset.

Example schema:

| Column      | Expected Type |
| ----------- | ------------- |
| Customer ID | Integer       |
| Name        | String        |
| Age         | Integer       |
| Sales       | Float         |

Inspect the DataFrame structure.

```python id="schema01"
df.info()
```

Review column names.

```python id="schema02"
df.columns
```

Check the number of columns.

```python id="schema03"
len(df.columns)
```

Validate expected columns.

```python id="schema04"
expected = [
    "Customer ID",
    "Name",
    "Age",
    "Sales"
]

missing = (
    set(expected)
    -
    set(df.columns)
)

print(missing)
```

An empty set means all expected columns are present.

---

# 6. Data Type Validation

Incorrect data types can produce incorrect calculations.

Inspect data types.

```python id="dtype01"
df.dtypes
```

Example output

| Column      | Type    |
| ----------- | ------- |
| Customer ID | int64   |
| Age         | object  |
| Sales       | float64 |

Here, **Age** should be numeric.

Convert safely.

```python id="dtype02"
df["Age"] = pd.to_numeric(
    df["Age"],
    errors="coerce"
)
```

Invalid values become `NaN`, making them easier to identify and clean.

---

## Validate Date Columns

```python id="dtype03"
df["Order Date"] = pd.to_datetime(
    df["Order Date"],
    errors="coerce"
)
```

Invalid dates are converted to `NaT`.

---

# 7. Constraint Validation

Business rules define acceptable values.

---

## Positive Sales

```python id="constraint01"
invalid_sales = (
    df[
        df["Sales"] < 0
    ]
)
```

---

## Valid Age Range

```python id="constraint02"
invalid_age = (
    df[
        (df["Age"] < 18)
        |
        (df["Age"] > 100)
    ]
)
```

---

## Quantity Must Be Positive

```python id="constraint03"
invalid_quantity = (
    df[
        df["Quantity"] <= 0
    ]
)
```

---

## Revenue Validation

Check whether:

```text id="constraint04"
Revenue = Quantity × Price
```

```python id="constraint05"
invalid_revenue = (
    df[
        df["Revenue"]
        !=
        (
            df["Quantity"]
            *
            df["Price"]
        )
    ]
)
```

---

## Required Fields

Rows missing essential values.

```python id="constraint06"
required = [
    "Customer ID",
    "Sales"
]

invalid_rows = (
    df[
        df[required]
        .isna()
        .any(axis=1)
    ]
)
```

---

# Business Example

A logistics company receives shipment data from multiple warehouses.

Business rules include:

* Quantity must be greater than zero.
* Delivery Date must be after Order Date.
* Shipment ID must be unique.
* Shipping Cost cannot be negative.
* Warehouse Code must exist in the approved warehouse list.

Validation catches incorrect records before shipment reports are generated.

---

# Best Practices

✔ Define validation rules before analysis.

✔ Validate schemas immediately after importing data.

✔ Convert data types early.

✔ Keep validation logic reusable.

✔ Separate valid and invalid records for review.

---

# Common Mistakes

### Assuming Imported Data Types Are Correct

Always inspect:

```python id="mistake01"
df.dtypes
```

CSV and Excel imports may infer unexpected types.

---

### Ignoring Invalid Dates

Use:

```python id="mistake02"
errors="coerce"
```

to safely detect malformed dates.

---

### Hardcoding Business Rules

Store validation thresholds (such as minimum age or maximum quantity) in configuration variables whenever possible. This makes maintenance easier if business requirements change.

---

# Key Takeaways

After completing this section, you should understand:

* What data validation is.
* How schemas ensure structural consistency.
* How to validate data types.
* How to apply business constraints.
* Why validation improves analytical reliability.

> **"Reliable analytics begins with validated data. Every rule enforced today prevents inaccurate decisions tomorrow."**

