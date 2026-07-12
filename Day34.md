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

# 8. Duplicate Validation

Duplicate records can inflate revenue, customer counts, and business KPIs.

Detect duplicate rows.

```python id="dup01"
duplicates = (
    df[df.duplicated()]
)
```

Count duplicates.

```python id="dup02"
df.duplicated().sum()
```

---

## Check Duplicates Using Specific Columns

Customer IDs should be unique.

```python id="dup03"
duplicate_ids = (
    df[
        df.duplicated(
            subset=["Customer ID"],
            keep=False
        )
    ]
)
```

Output

| Customer ID | Name  |
| ----------: | ----- |
|         101 | Alice |
|         101 | Alice |

---

# 9. Range Validation

Many business fields have acceptable ranges.

Example:

| Column     | Valid Range |
| ---------- | ----------- |
| Age        | 18–100      |
| Discount % | 0–100       |
| Sales      | ≥ 0         |
| Rating     | 1–5         |

---

## Discount Validation

```python id="range01"
invalid_discount = (
    df[
        (df["Discount"] < 0)
        |
        (df["Discount"] > 100)
    ]
)
```

---

## Rating Validation

```python id="range02"
invalid_rating = (
    df[
        ~df["Rating"].between(1,5)
    ]
)
```

---

# 10. Regular Expression (Regex) Validation

Regex helps validate structured text.

Examples:

* Email
* Phone Number
* PIN Code
* Product Code
* Invoice Number

---

## Email Validation

```python id="regex01"
email_pattern = (
    r"^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$"
)

invalid_email = (
    df[
        ~df["Email"]
        .str.match(
            email_pattern,
            na=False
        )
    ]
)
```

---

## Indian PIN Code Validation

```python id="regex02"
pin_pattern = r"^[1-9][0-9]{5}$"

invalid_pin = (
    df[
        ~df["PIN Code"]
        .astype(str)
        .str.match(
            pin_pattern,
            na=False
        )
    ]
)
```

---

## Phone Number Validation

```python id="regex03"
phone_pattern = r"^[6-9][0-9]{9}$"

invalid_phone = (
    df[
        ~df["Phone"]
        .astype(str)
        .str.match(
            phone_pattern,
            na=False
        )
    ]
)
```

---

# 11. Uniqueness Constraints

Certain business fields must always be unique.

Examples:

* Customer ID
* Employee ID
* Invoice Number
* Order ID

Check uniqueness.

```python id="unique01"
df["Order ID"].is_unique
```

Output

```text id="unique02"
True
```

Find duplicate order IDs.

```python id="unique03"
duplicate_orders = (
    df[
        df["Order ID"]
        .duplicated(
            keep=False
        )
    ]
)
```

---

# 12. Cross-Field Validation

Sometimes one field depends on another.

Example:

Revenue should equal:

```text id="cross01"
Quantity × Price
```

Validate:

```python id="cross02"
invalid_rows = (
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

## Date Validation

Delivery Date should be after Order Date.

```python id="cross03"
invalid_dates = (
    df[
        df["Delivery Date"]
        <
        df["Order Date"]
    ]
)
```

---

## Profit Validation

Profit should not exceed Revenue.

```python id="cross04"
invalid_profit = (
    df[
        df["Profit"]
        >
        df["Revenue"]
    ]
)
```

---

# 13. Outlier Detection

Outliers are unusually high or low values.

---

## Using the IQR Method

Calculate quartiles.

```python id="outlier01"
Q1 = df["Sales"].quantile(0.25)

Q3 = df["Sales"].quantile(0.75)

IQR = Q3 - Q1
```

Determine limits.

```python id="outlier02"
lower = Q1 - 1.5 * IQR

upper = Q3 + 1.5 * IQR
```

Detect outliers.

```python id="outlier03"
outliers = (
    df[
        (df["Sales"] < lower)
        |
        (df["Sales"] > upper)
    ]
)
```

---

# 14. Data Quality Score

Measure overall dataset quality.

Example:

```python id="score01"
total_cells = (
    df.shape[0]
    *
    df.shape[1]
)

missing = (
    df.isna()
      .sum()
      .sum()
)

quality_score = (
    1 -
    missing / total_cells
) * 100
```

Output

```text id="score02"
97.6%
```

Organizations often track this score over time.

---

# 15. Validation Report

Summarize validation results.

```python id="report01"
report = {
    "Missing Values":
        df.isna().sum().sum(),

    "Duplicate Rows":
        df.duplicated().sum(),

    "Invalid Ages":
        len(invalid_age),

    "Invalid Emails":
        len(invalid_email),

    "Outliers":
        len(outliers)
}
```

Convert into a DataFrame.

```python id="report02"
validation_report = (
    pd.DataFrame(
        report.items(),
        columns=[
            "Check",
            "Count"
        ]
    )
)
```

Example

| Check          | Count |
| -------------- | ----: |
| Missing Values |    12 |
| Duplicate Rows |     3 |
| Invalid Emails |     4 |
| Outliers       |     7 |

---

# Business Example

A healthcare organization validates patient records before generating reports.

Validation includes:

* Unique Patient ID.
* Valid age range.
* Correct email format.
* Admission date before discharge date.
* No duplicate medical record numbers.
* Detect unusual billing amounts.

Only validated records are used for operational dashboards and regulatory reporting.

---

# Best Practices

✔ Validate unique identifiers before joins.

✔ Use regex for structured text fields.

✔ Apply cross-field validation for business logic.

✔ Investigate outliers before removing them.

✔ Generate validation reports for every ETL pipeline.

---

# Common Mistakes

### Treating Every Outlier as an Error

Some outliers represent legitimate business events, such as unusually large purchases or seasonal spikes.

Investigate before removing them.

---

### Validating Only Individual Columns

Many errors appear only when comparing multiple fields, such as Delivery Date occurring before Order Date.

Always include cross-field validation.

---

### Ignoring Validation Reports

Running validation without documenting the results makes it difficult to monitor data quality over time.

---

# Quick Recap

You have now learned how to:

* Detect duplicate records.
* Validate ranges and constraints.
* Validate structured text using regex.
* Enforce uniqueness.
* Perform cross-field validation.
* Detect outliers using the IQR method.
* Generate data quality reports.

> **"Data validation is more than finding errors—it is the process of building confidence that every analysis is based on accurate, consistent, and trustworthy information."**
