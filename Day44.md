# Day 44 — Advanced Data Validation & Quality Assurance in Pandas

## Introduction

In real-world analytics, **data cleaning alone is not enough**. Before data is used for dashboards, reporting, machine learning, or business decisions, it must be validated to ensure it is **accurate, complete, consistent, and trustworthy**.

Data Validation is the process of checking whether the data satisfies predefined business rules and quality standards. Organizations automate these checks as part of their ETL pipelines to prevent poor-quality data from reaching production systems.

---

# Topics Covered

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


The next section covers:

- Constraint validation
- Duplicate detection
- Uniqueness validation
- Referential integrity
- Regex validation
- Business rule validation
- Automated quality reports
- Enterprise validation techniques

# 8. Constraint Validation

Constraint validation ensures that relationships between columns follow business rules.

Example:

Revenue should equal Quantity × Price.

```python
invalid = df[
    df["Revenue"] !=
    df["Quantity"] * df["Price"]
]
```

Another example:

Delivery Date should never be earlier than Order Date.

```python
invalid = df[
    df["Delivery Date"] <
    df["Order Date"]
]
```

Applications:

- Banking transactions
- E-commerce orders
- Payroll systems
- Inventory management

---

# 9. Duplicate Detection

Duplicate records often cause incorrect KPIs and inaccurate reports.

Count duplicate rows.

```python
df.duplicated().sum()
```

Display duplicate records.

```python
duplicates = df[
    df.duplicated(
        keep=False
    )
]
```

Remove duplicates.

```python
df = df.drop_duplicates()
```

---

## Duplicate Based on Selected Columns

Sometimes only certain columns determine uniqueness.

```python
duplicates = df[
    df.duplicated(
        subset=[
            "Customer ID",
            "Order Date"
        ],
        keep=False
    )
]
```

---

# 10. Uniqueness Validation

Primary keys should always be unique.

Check uniqueness.

```python
df["Customer ID"].is_unique
```

Returns:

```text
True
```

or

```text
False
```

Find duplicate IDs.

```python
duplicate_ids = df[
    df["Customer ID"].duplicated(
        keep=False
    )
]
```

---

# 11. Referential Integrity

Referential integrity ensures that related tables remain consistent.

Example:

### Customers Table

| Customer ID |
|------------:|
|101|
|102|
|103|

### Orders Table

| Customer ID |
|------------:|
|101|
|102|
|104|

Customer **104** does not exist.

Detect invalid references.

```python
invalid = orders[
    ~orders["Customer ID"].isin(
        customers["Customer ID"]
    )
]
```

This identifies orphan records.

---

# 12. Regular Expression Validation

Regex validates text patterns.

## Validate Email

```python
pattern = r"^[\w\.-]+@[\w\.-]+\.\w+$"

valid = df["Email"].str.match(
    pattern
)
```

---

## Validate Phone Number

Exactly 10 digits.

```python
pattern = r"^\d{10}$"

valid = df["Phone"].str.match(
    pattern
)
```

---

## Validate PIN Code

Exactly 6 digits.

```python
pattern = r"^\d{6}$"

valid = df["PIN Code"].str.match(
    pattern
)
```

---

# 13. Business Rule Validation

Business rules vary across industries.

Examples:

### Salary Cannot Be Negative

```python
invalid = df[
    df["Salary"] < 0
]
```

---

### Attendance Percentage

```python
invalid = df[
    ~df["Attendance"].between(
        0,
        100
    )
]
```

---

### Stock Quantity

```python
invalid = df[
    df["Stock"] < 0
]
```

---

# 14. Automated Data Quality Report

Generate an overall quality report.

```python
quality_report = {

    "Rows":
    len(df),

    "Missing Values":
    df.isna().sum().sum(),

    "Duplicate Rows":
    df.duplicated().sum(),

    "Negative Revenue":
    (
        df["Revenue"] < 0
    ).sum(),

    "Duplicate Customers":
    (
        df["Customer ID"]
        .duplicated()
        .sum()
    )

}

report = pd.DataFrame(

    quality_report.items(),

    columns=[
        "Metric",
        "Value"
    ]

)

print(report)
```

Example Output

| Metric | Value |
|---------|------:|
|Rows|50000|
|Missing Values|18|
|Duplicate Rows|5|
|Negative Revenue|2|
|Duplicate Customers|3|

---

# 15. Enterprise Validation Workflow

```
Raw Dataset
      │
      ▼
Schema Validation
      │
      ▼
Data Type Validation
      │
      ▼
Missing Value Check
      │
      ▼
Range Validation
      │
      ▼
Constraint Validation
      │
      ▼
Duplicate Detection
      │
      ▼
Referential Integrity
      │
      ▼
Generate QA Report
      │
      ▼
Validated Dataset
```

---

# Business Example

A banking company validates daily transaction files.

Checks performed:

- Required columns exist.
- Account numbers are unique.
- Transaction amount is positive.
- Customer IDs exist in the master database.
- Transaction dates are valid.
- Duplicate transactions are removed.
- Invalid email and phone formats are flagged.

Only validated records are loaded into the production database.

---

# Best Practices

✔ Validate primary keys.

✔ Check referential integrity before joins.

✔ Validate business rules before reporting.

✔ Automate quality reports.

✔ Log validation failures for auditing.

---

# Common Mistakes

### Ignoring Duplicate Primary Keys

Duplicate IDs can cause incorrect joins and inaccurate reporting.

---

### Skipping Referential Integrity

Missing foreign keys create orphan records and inconsistent analyses.

---

### Hardcoding Validation Logic

Store validation rules in reusable functions or configuration files whenever possible.

---

# Quick Recap

You have now learned how to:

- Perform constraint validation.
- Detect duplicate records.
- Validate uniqueness.
- Check referential integrity.
- Validate text using regex.
- Build automated quality reports.
- Design enterprise validation workflows.

> **"Data quality is built through systematic validation. Automated validation pipelines ensure that every downstream analysis starts with reliable and trustworthy data."**

---



The final section will cover:

- Enterprise QA architecture
- Automated validation framework
- Data quality metrics
- Production best practices
- Interview questions (20+)
- Practice exercises
- Cheat sheet
- Mini project
- Executive business insights
- Complete Day 44 summary

# 16. Enterprise Data Quality Architecture

Large organizations build automated validation pipelines before data reaches production systems.

```
Data Sources
      │
      ▼
Data Ingestion
      │
      ▼
Schema Validation
      │
      ▼
Data Type Validation
      │
      ▼
Missing Value Validation
      │
      ▼
Business Rule Validation
      │
      ▼
Duplicate Detection
      │
      ▼
Referential Integrity
      │
      ▼
Quality Report
      │
      ▼
Production Database
      │
      ▼
Dashboards / Machine Learning
```

Each validation stage acts as a quality checkpoint before the data moves to the next stage.

---

# 17. Automated Validation Framework

Instead of manually validating every dataset, create reusable validation functions.

```python
def validate_dataset(df):

    report = {

        "Rows":
        len(df),

        "Missing Values":
        df.isna().sum().sum(),

        "Duplicate Rows":
        df.duplicated().sum(),

        "Negative Revenue":
        (df["Revenue"] < 0).sum(),

        "Unique Customers":
        df["Customer ID"].nunique()

    }

    return pd.DataFrame(
        report.items(),
        columns=[
            "Metric",
            "Value"
        ]
    )
```

Execute:

```python
qa_report = validate_dataset(df)

print(qa_report)
```

---

# 18. Data Quality Metrics

Organizations continuously monitor data quality.

Common metrics include:

| Metric | Description |
|---------|-------------|
|Completeness|Percentage of non-missing values|
|Accuracy|Correctness of values|
|Consistency|Uniform formatting across datasets|
|Validity|Conformance to business rules|
|Uniqueness|Duplicate-free records|
|Timeliness|Freshness of the data|

Example:

```python
completeness = (

    1 -

    df.isna().sum().sum()

    /

    df.size

) * 100
```

---

# 19. Production Best Practices

### Validate Before Loading

Always validate data before writing to production databases.

---

### Log Every Validation Failure

Example:

```python
invalid_rows.to_csv(
    "validation_errors.csv",
    index=False
)
```

This helps with debugging and auditing.

---

### Build Reusable Rules

Instead of:

```python
if age > 100:
```

Create reusable validation functions.

```python
def validate_age(age):

    return 18 <= age <= 100
```

---

### Fail Fast

If critical validations fail, stop the pipeline immediately rather than allowing incorrect data to continue downstream.

---

# 20. Enterprise Case Study

## Scenario

You are a **Senior Data Quality Engineer** at a banking organization.

Daily transaction data contains:

- Customer information
- Transactions
- Account balances
- Branch details

Before loading into the data warehouse, perform:

- Schema validation
- Duplicate detection
- Range validation
- Referential integrity
- Email validation
- Business rule validation

Example:

```python
invalid_transactions = df[
    df["Amount"] <= 0
]
```

Validate future dates.

```python
future = df[
    df["Transaction Date"] >
    pd.Timestamp.today()
]
```

---

# 21. Business Insights

After implementing automated validation:

- Duplicate customer records decreased significantly.
- Invalid transactions were detected before reporting.
- Automated quality reports reduced manual verification time.
- Referential integrity improved database consistency.
- Data quality scores increased, resulting in more reliable dashboards.

---

# 22. Practice Exercises

## Beginner

1. Validate schema.
2. Check missing values.
3. Detect duplicates.
4. Validate positive revenue.
5. Check data types.

---

## Intermediate

6. Validate email addresses.
7. Validate phone numbers.
8. Verify referential integrity.
9. Create a quality report.
10. Build reusable validation functions.

---

## Advanced

11. Design an enterprise validation framework.
12. Automate QA reporting.
13. Build configurable validation rules.
14. Log validation failures.
15. Create a production-ready validation pipeline.

---

# 23. Interview Questions

## Beginner

1. What is data validation?
2. Why is schema validation important?
3. What is range validation?
4. How do you detect duplicate records?
5. What is referential integrity?

---

## Intermediate

6. Explain constraint validation.
7. Why automate quality checks?
8. How do you validate emails?
9. What is data completeness?
10. What is data consistency?

---

## Advanced

11. Design an enterprise data quality framework.
12. Explain production validation pipelines.
13. Compare validation and data cleaning.
14. Validate datasets containing millions of rows.
15. Build reusable QA utilities.

---

# 24. Cheat Sheet

| Task | Syntax |
|------|--------|
| Missing Values | `isna()` |
| Duplicate Rows | `duplicated()` |
| Unique Values | `is_unique` |
| Range Check | `between()` |
| Membership | `isin()` |
| Regex | `str.match()` |
| Schema Check | `set(df.columns)` |
| Data Types | `dtypes` |
| Constraint Check | Boolean filtering |
| Quality Report | `DataFrame()` |

---

# 25. Mini Project

## Enterprise Data Quality Dashboard

Using any retail, banking, healthcare, HR, logistics, or telecom dataset:

Complete the following tasks:

- Validate schema.
- Validate data types.
- Check missing values.
- Detect duplicates.
- Validate numerical ranges.
- Validate business constraints.
- Verify referential integrity.
- Generate an automated QA report.
- Write **five executive-level quality insights**.
- Recommend **three improvements** to increase data quality.

### Example Business Insights

- Duplicate records caused inconsistencies in customer reporting.
- Automated validation prevented invalid revenue entries.
- Referential integrity improved database consistency.
- Data quality reports reduced manual verification effort.
- Standardized validation increased confidence in business dashboards.

---

# 26. Summary

Congratulations! 🎉

Today you mastered **Advanced Data Validation & Quality Assurance**.

You learned how to:

- Validate schemas.
- Validate data types.
- Detect missing values.
- Perform range validation.
- Apply business constraints.
- Detect duplicates.
- Verify referential integrity.
- Validate text using regular expressions.
- Generate automated quality reports.
- Build enterprise QA pipelines.

These techniques are essential for enterprise ETL pipelines, business intelligence systems, analytics engineering, and production machine learning workflows.

---

# 27. What's Next?

## 🐼 Day 45 — Advanced Feature Engineering with Pandas

Topics include:

- Creating New Features
- Mathematical Feature Engineering
- Date & Time Features
- Categorical Encoding
- One-Hot Encoding
- Label Encoding
- Feature Scaling
- Binning
- Interaction Features
- Aggregated Features
- Rolling Features
- Feature Selection
- Production Feature Pipelines

Feature Engineering is one of the most valuable skills in Machine Learning and Advanced Analytics because high-quality features often improve model performance more than choosing a more complex algorithm.

---

# 🎉 Day 44 Complete!

You have successfully completed **Advanced Data Validation & Quality Assurance in Pandas**.

You can now build automated validation pipelines, enforce business rules, generate quality reports, and ensure production datasets are reliable before analysis or machine learning.

⭐ **Next → Day 45: Advanced Feature Engineering with Pandas** 🚀🐼
