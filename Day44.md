# 🐼 Day 44 — Advanced Data Validation & Quality Assurance in Pandas

## 📖 Introduction

In real-world analytics, **data cleaning alone is not enough**. Before data is used for dashboards, reporting, machine learning, or business decisions, it must be validated to ensure it is **accurate, complete, consistent, and trustworthy**.

Data Validation is the process of checking whether the data satisfies predefined business rules and quality standards. Organizations automate these checks as part of their ETL pipelines to prevent poor-quality data from reaching production systems.

---

# 📚 Topics Covered

- Data Validation Fundamentals
- Schema Validation
- Data Type Validation
- Missing Value Validation
- Range Validation
- Constraint Validation
- Duplicate Detection
- Uniqueness Validation
- Referential Integrity
- Regular Expression Validation
- Data Quality Reports
- Enterprise QA Pipelines

---

# 1. What is Data Validation?

Data validation is the process of verifying that a dataset follows predefined business rules.

Examples:

- Customer ID must be unique.
- Revenue cannot be negative.
- Age should be between 18 and 100.
- Order Date should not be after Delivery Date.
- Email addresses should follow a valid format.

Without validation:

- Reports become inaccurate.
- Machine learning models perform poorly.
- Business decisions become unreliable.

---

# 2. Why Data Validation Matters

Imagine an e-commerce company.

Incorrect data:

| Customer ID | Revenue |
|-------------|--------:|
|101|-500|
|102|3500|
|102|4200|

Problems:

- Negative revenue
- Duplicate Customer IDs

Without validation, business KPIs will be incorrect.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

- Validate dataset structure.
- Check data types.
- Detect invalid values.
- Validate business rules.
- Generate automated quality reports.

---

# 4. Schema Validation

A schema defines:

- Column names
- Data types
- Expected structure

Example:

```python
expected_columns = [

    "Customer ID",

    "Revenue",

    "Region",

    "Order Date"

]

missing = (

    set(expected_columns)

    -

    set(df.columns)

)

print(missing)
```

Output

```text
set()
```

means every required column exists.

---

## Extra Columns

```python
extra = (

    set(df.columns)

    -

    set(expected_columns)

)
```

Useful for detecting unexpected fields.

---

# 5. Data Type Validation

Check data types.

```python
df.dtypes
```

Example

| Column | Type |
|----------|-----------|
|Revenue|float64|
|Quantity|int64|
|Region|object|
|Order Date|datetime64|

---

Validate one column.

```python
assert (

    df["Revenue"].dtype

    ==

    "float64"

)
```

---

# 6. Missing Value Validation

Count missing values.

```python
df.isna().sum()
```

Percentage.

```python
(

    df.isna().mean()

    *

    100

).round(2)
```

Output

| Column | Missing % |
|---------|----------:|
|Age|2.4|
|Revenue|0|
|Region|0.5|

---

# 7. Range Validation

Age validation.

```python
invalid = (

    df[

        ~df["Age"].between(

            18,

            100

        )

    ]

)
```

Revenue validation.

```python
invalid = (

    df[

        df["Revenue"] < 0

    ]

)
```

Discount validation.

```python
invalid = (

    df[

        ~df["Discount"].between(

            0,

            100

        )

    ]

)
```

---

# Business Example

A retail company receives daily customer transactions.

Validation checks include:

- Revenue cannot be negative.
- Customer IDs must be unique.
- Discount should be between 0 and 100.
- Order dates must be valid.
- Required columns must exist.

Only validated records are loaded into the reporting database.

---

# Best Practices

✔ Validate schemas before processing.

✔ Verify data types immediately after import.

✔ Check missing values before analysis.

✔ Validate numerical ranges.

✔ Document validation failures.

---

# Common Mistakes

### Ignoring Invalid Values

Incorrect records should never be silently accepted.

---

### Hardcoding Business Rules

Store validation rules in configuration files whenever possible.

---

### Validating Only Once

Validation should occur throughout the ETL pipeline.

---

# Key Takeaways

After completing this section, you should understand:

- What data validation is.
- How schema validation works.
- How to validate data types.
- How to detect missing values.
- How to perform range validation.

> **"Data validation ensures that business decisions are based on accurate, reliable, and trustworthy information."**

---

## Next (Day 44 – Part 2)

The next section covers:

- Constraint validation
- Duplicate detection
- Uniqueness validation
- Referential integrity
- Regex validation
- Business rule validation
- Automated quality reports
- Enterprise validation techniques
