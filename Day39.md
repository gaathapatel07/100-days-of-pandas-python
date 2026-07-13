# Day 39 — Advanced Missing Data Handling & Data Cleaning Strategies

<div align="center">

# 100 Days of Pandas

### Day 39 · Preparing High-Quality Data for Analytics & Machine Learning

*"Clean data is the foundation of trustworthy analytics. Every successful model begins with reliable, consistent, and complete data."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Data%20Cleaning-blue)
![Day](https://img.shields.io/badge/Day-39-orange)

</div>

---

# Table of Contents

1. Introduction
2. Understanding Missing Data
3. Why Missing Data Matters
4. Learning Objectives
5. Missing Data Mechanisms
6. Detecting Missing Values
7. Basic Missing Value Handling
8. Summary

---

# 1. Introduction

Real-world datasets are rarely perfect.

Common issues include:

* Missing values
* Duplicate records
* Typographical errors
* Inconsistent formatting
* Invalid dates
* Extra whitespace
* Mixed letter cases
* Incorrect categories

Before performing analysis, these issues must be addressed.

---

# 2. Understanding Missing Data

Missing values appear as:

* `NaN`
* `None`
* `NaT` (for datetime)
* Empty strings (`""`)
* Placeholder values such as `"Unknown"` or `"N/A"`

Example:

| Customer | Age | City   |
| -------- | --: | ------ |
| Alice    |  25 | Delhi  |
| Rahul    | NaN | Mumbai |
| Priya    |  30 | NaN    |

Missing data can affect calculations, visualizations, and machine learning models.

---

# 3. Why Missing Data Matters

Suppose a hospital dataset contains missing patient ages.

If the missing values are ignored:

* Average age may be misleading.
* Risk analysis may become inaccurate.
* Machine learning models may fail.
* Business decisions may be based on incomplete information.

Handling missing values appropriately improves analytical reliability.

---

# 4. Learning Objectives

By the end of this lesson, you will be able to:

* Understand different missing data mechanisms.
* Detect missing values.
* Handle missing data appropriately.
* Apply imputation techniques.
* Build cleaner datasets.

---

# 5. Missing Data Mechanisms

Understanding **why** data is missing helps determine the best cleaning strategy.

### MCAR (Missing Completely At Random)

Missing values occur randomly.

Example:

A sensor occasionally fails due to a temporary power interruption.

No systematic relationship exists between the missing values and other variables.

---

### MAR (Missing At Random)

Missing values depend on other observed variables.

Example:

Older customers are less likely to provide their email address.

The missingness is related to **Age**, which is observed.

---

### MNAR (Missing Not At Random)

Missing values depend on the missing value itself.

Example:

Customers with very high salaries choose not to disclose their income.

The missingness is directly related to the hidden value.

MNAR is the most challenging type to handle.

---

# 6. Detecting Missing Values

Identify missing values.

```python id="missing01"
df.isna()
```

Count missing values.

```python id="missing02"
df.isna().sum()
```

Total missing values.

```python id="missing03"
df.isna().sum().sum()
```

Missing value percentage.

```python id="missing04"
(
    df.isna().mean()
    * 100
).round(2)
```

Example Output

| Column | Missing % |
| ------ | --------: |
| Age    |      2.50 |
| City   |      0.80 |
| Sales  |      0.00 |

---

# 7. Basic Missing Value Handling

## Remove Rows with Missing Values

```python id="drop01"
clean_df = (
    df.dropna()
)
```

Removes rows containing at least one missing value.

---

## Remove Columns with Missing Values

```python id="drop02"
clean_df = (
    df.dropna(
        axis=1
    )
)
```

---

## Remove Rows Only When Specific Columns Are Missing

```python id="drop03"
clean_df = (
    df.dropna(
        subset=[
            "Sales",
            "Customer ID"
        ]
    )
)
```

---

## Fill Missing Values with a Constant

```python id="fill01"
df["City"] = (
    df["City"]
      .fillna(
          "Unknown"
      )
)
```

---

## Fill Numerical Values

```python id="fill02"
df["Age"] = (
    df["Age"]
      .fillna(0)
)
```

---

## Fill Multiple Columns

```python id="fill03"
df = df.fillna({

    "City":"Unknown",

    "Age":0,

    "Sales":0
})
```

---

# Business Example

A retail company imports customer information.

Some records contain:

* Missing cities.
* Missing ages.
* Missing purchase amounts.

The analytics team:

* Fills missing cities with `"Unknown"`.
* Removes rows missing Customer IDs.
* Replaces missing sales with zero only if zero represents "no recorded sale" according to business rules.
* Documents every cleaning decision.

This creates a reliable dataset for reporting.

---

# Best Practices

✔ Understand why values are missing before replacing them.

✔ Calculate missing percentages before cleaning.

✔ Remove rows only when necessary.

✔ Document all imputation decisions.

✔ Validate cleaned datasets before analysis.

---

# Common Mistakes

### Filling Every Missing Value with Zero

Zero may have a business meaning.

For example:

* Zero sales
* Zero quantity
* Zero balance

Replacing missing values with zero without considering context can introduce bias.

---

### Removing Too Many Rows

If a large proportion of the dataset contains missing values, dropping all such rows may result in substantial information loss.

Evaluate the impact before removing data.

---

### Ignoring Missing Data Patterns

Missing values may reveal important business processes or system issues.

Investigate patterns instead of immediately replacing values.

---

# Key Takeaways

After completing this section, you should understand:

* Why missing data occurs.
* The difference between MCAR, MAR, and MNAR.
* How to detect missing values.
* How to remove or fill missing data.
* Why business context is essential when handling missing values.

> **"Handling missing data is not simply about filling blanks—it is about making informed decisions that preserve the integrity and usefulness of the dataset."**

# 8. Mean, Median & Mode Imputation

Instead of removing missing values, we often replace them with statistically meaningful values.

## Mean Imputation

Best for normally distributed numerical data.

```python id="impute01"
df["Age"] = (
    df["Age"]
      .fillna(
          df["Age"].mean()
      )
)
```

Example

| Original Age |
| ------------ |
| 25           |
| 30           |
| NaN          |
| 40           |

Mean = 31.67

After imputation:

|   Age |
| ----: |
|    25 |
|    30 |
| 31.67 |
|    40 |

---

## Median Imputation

Preferred when outliers exist.

```python id="impute02"
df["Salary"] = (
    df["Salary"]
      .fillna(
          df["Salary"].median()
      )
)
```

Median is more robust than the mean.

---

## Mode Imputation

Useful for categorical variables.

```python id="impute03"
df["City"] = (
    df["City"]
      .fillna(
          df["City"].mode()[0]
      )
)
```

Example

| City   |
| ------ |
| Delhi  |
| Mumbai |
| Delhi  |
| NaN    |

Mode = Delhi

---

# 9. Forward Fill (`ffill`)

Forward fill copies the previous valid value.

```python id="ffill01"
df = df.ffill()
```

Example

| Date | Sales |
| ---- | ----: |
| Jan  |   500 |
| Feb  |   NaN |
| Mar  |   620 |

After:

| Date | Sales |
| ---- | ----: |
| Jan  |   500 |
| Feb  |   500 |
| Mar  |   620 |

Useful for time-series datasets.

---

# 10. Backward Fill (`bfill`)

Backward fill copies the next valid value.

```python id="bfill01"
df = df.bfill()
```

Example

| Date | Sales |
| ---- | ----: |
| Jan  |   500 |
| Feb  |   NaN |
| Mar  |   620 |

After:

| Date | Sales |
| ---- | ----: |
| Jan  |   500 |
| Feb  |   620 |
| Mar  |   620 |

---

# 11. Interpolation

Interpolation estimates missing numerical values based on surrounding observations.

```python id="interp01"
df["Temperature"] = (
    df["Temperature"]
      .interpolate()
)
```

Example

| Day | Temp |
| --- | ---: |
| 1   |   20 |
| 2   |  NaN |
| 3   |   24 |

Result

| Day | Temp |
| --- | ---: |
| 1   |   20 |
| 2   |   22 |
| 3   |   24 |

Useful for:

* Weather data
* Stock prices
* Sensor readings
* Time-series analysis

---

# 12. Conditional Imputation

Replace missing values differently depending on business rules.

Example:

Fill missing salary based on department median.

```python id="cond01"
df["Salary"] = (
    df.groupby("Department")["Salary"]
      .transform(
          lambda x: x.fillna(
              x.median()
          )
      )
)
```

This preserves department-specific characteristics.

---

# 13. Advanced Duplicate Handling

Check duplicates based on selected columns.

```python id="dup01"
duplicates = (
    df[
        df.duplicated(
            subset=[
                "Customer ID",
                "Order Date"
            ],
            keep=False
        )
    ]
)
```

---

## Keep Latest Record

```python id="dup02"
df = (
    df.sort_values(
        "Order Date"
    )
    .drop_duplicates(
        subset="Customer ID",
        keep="last"
    )
)
```

Useful for customer master data.

---

# 14. Text Standardization

Real-world text data is often inconsistent.

Example:

| Original |
| -------- |
| Delhi    |
| DELHI    |
| delhi    |
| Delhi    |

Standardize:

```python id="text01"
df["City"] = (
    df["City"]
      .str.strip()
      .str.title()
)
```

Output

| City  |
| ----- |
| Delhi |
| Delhi |
| Delhi |
| Delhi |

---

## Lowercase

```python id="text02"
df["Email"] = (
    df["Email"]
      .str.lower()
)
```

---

## Remove Extra Spaces

```python id="text03"
df["Name"] = (
    df["Name"]
      .str.strip()
)
```

---

# 15. Data Consistency Checks

Ensure related fields follow business rules.

---

## Revenue Check

```python id="check01"
invalid = (
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

## Age Validation

```python id="check02"
invalid = (
    df[
        ~df["Age"].between(
            18,
            100
        )
    ]
)
```

---

## Discount Validation

```python id="check03"
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

A healthcare organization receives patient data from multiple hospitals.

Cleaning steps:

* Fill missing ages using department median.
* Standardize hospital names.
* Remove duplicate patient IDs.
* Validate admission and discharge dates.
* Check billing totals.

Only validated records are loaded into the reporting system.

---

# Best Practices

✔ Choose imputation methods based on data distribution.

✔ Use forward/backward fill only for sequential or time-series data.

✔ Standardize text before grouping or joining datasets.

✔ Validate business rules after cleaning.

✔ Document every transformation.

---

# Common Mistakes

### Using Mean with Highly Skewed Data

For skewed distributions (e.g., salaries), the median is often a better choice because it is less affected by extreme values.

---

### Forward Filling Unrelated Records

Only use `ffill()` when adjacent records are logically connected (such as time-series observations).

---

### Ignoring Text Inconsistencies

Values like `"Delhi"`, `"delhi"`, and `" DELHI "` represent the same city but will be treated as different categories unless standardized.

---

# Quick Recap

You have now learned how to:

* Apply mean, median, and mode imputation.
* Use forward fill and backward fill.
* Interpolate missing values.
* Perform conditional imputation.
* Handle duplicates intelligently.
* Standardize text.
* Validate business consistency rules.

> **"Effective data cleaning combines statistical methods with business understanding to create datasets that are accurate, consistent, and ready for reliable analysis."**

# 16. Enterprise Data Cleaning Workflow

Professional organizations follow a structured cleaning workflow before any analysis.

```text id="workflow01"
Raw Dataset
      │
      ▼
Schema Validation
      │
      ▼
Data Type Correction
      │
      ▼
Missing Value Analysis
      │
      ▼
Missing Value Treatment
      │
      ▼
Duplicate Removal
      │
      ▼
Text Standardization
      │
      ▼
Business Rule Validation
      │
      ▼
Outlier Review
      │
      ▼
Clean Dataset
      │
      ▼
Analytics / Machine Learning
```

A systematic workflow ensures that every dataset meets quality standards before use.

---

# 17. Building an Automated Data Cleaning Pipeline

Instead of cleaning data manually each time, create a reusable function.

```python id="pipeline01"
def clean_dataset(df):

    # Remove duplicate rows
    df = df.drop_duplicates()

    # Standardize city names
    if "City" in df.columns:
        df["City"] = (
            df["City"]
              .str.strip()
              .str.title()
        )

    # Fill missing Age with median
    if "Age" in df.columns:
        df["Age"] = (
            df["Age"]
              .fillna(
                  df["Age"].median()
              )
        )

    # Fill missing City
    if "City" in df.columns:
        df["City"] = (
            df["City"]
              .fillna("Unknown")
        )

    return df
```

Run the cleaning pipeline.

```python id="pipeline02"
clean_df = clean_dataset(df)
```

Reusable pipelines improve consistency and reduce manual effort.

---

# 18. Data Quality Checklist

Before analysis, verify the following:

| Check                    | Status |
| ------------------------ | ------ |
| Correct data types       | ☐      |
| Missing values handled   | ☐      |
| Duplicates removed       | ☐      |
| Text standardized        | ☐      |
| Business rules validated | ☐      |
| Outliers reviewed        | ☐      |
| Date formats verified    | ☐      |
| Unique IDs validated     | ☐      |
| Memory optimized         | ☐      |
| Cleaning documented      | ☐      |

Following a checklist reduces the chance of introducing errors into downstream analyses.

---

# 19. Performance Optimization During Cleaning

Cleaning large datasets should be efficient.

### Clean Only Required Columns

```python id="perf01"
df[
    [
        "City",
        "Age"
    ]
]
```

Avoid processing unnecessary columns.

---

### Vectorized String Operations

Instead of loops:

```python id="perf02"
df["City"] = (
    df["City"]
      .str.title()
)
```

Vectorized operations are significantly faster.

---

### Optimize Data Types

```python id="perf03"
df["Age"] = (
    pd.to_numeric(
        df["Age"],
        downcast="integer"
    )
)
```

Reducing memory usage speeds up processing.

---

# 20. Enterprise Case Study

## Scenario

You are a **Senior Data Engineer** at **HealthCare Analytics Ltd.**

Patient records arrive daily from multiple hospitals.

Before generating reports, the data must be cleaned.

Available data:

* Patient ID
* Hospital
* Age
* Diagnosis
* Admission Date
* Discharge Date
* Billing Amount

---

## Business Questions

### Question 1

Identify missing values.

```python id="case01"
df.isna().sum()
```

---

### Question 2

Remove duplicate patients.

```python id="case02"
df.drop_duplicates(
    subset="Patient ID"
)
```

---

### Question 3

Standardize hospital names.

```python id="case03"
df["Hospital"] = (
    df["Hospital"]
      .str.strip()
      .str.title()
)
```

---

### Question 4

Fill missing billing amounts.

```python id="case04"
df["Billing Amount"] = (
    df["Billing Amount"]
      .fillna(
          df["Billing Amount"].median()
      )
)
```

---

### Question 5

Validate patient ages.

```python id="case05"
invalid_age = (
    df[
        ~df["Age"].between(
            0,
            120
        )
    ]
)
```

---

# 21. Business Insights

After cleaning the dataset, analysts observe:

* Duplicate patient records are eliminated, improving reporting accuracy.
* Standardized hospital names prevent incorrect grouping during analysis.
* Missing billing amounts are handled consistently, preserving valuable records.
* Business rule validation identifies invalid patient ages before reporting.
* Automated cleaning pipelines reduce manual effort and improve reproducibility.

---

# 22. Practice Exercises

## Beginner

1. Count missing values.
2. Fill missing values using the mean.
3. Remove duplicate rows.
4. Standardize a text column.
5. Validate a numerical range.

---

## Intermediate

6. Fill missing values using the median.
7. Apply forward fill to a time-series dataset.
8. Perform interpolation.
9. Standardize multiple text columns.
10. Build a reusable cleaning function.

---

## Advanced

11. Build an automated cleaning pipeline.
12. Apply conditional imputation by group.
13. Validate business rules.
14. Create a cleaning checklist.
15. Prepare a production-ready dataset.

---

# 23. Interview Questions

## Beginner

1. Why is data cleaning important?
2. What is the difference between `NaN` and `None`?
3. When should you use `dropna()`?
4. What is imputation?
5. Why standardize text?

---

## Intermediate

6. Compare mean and median imputation.
7. When should you use forward fill?
8. Explain interpolation.
9. How do you detect duplicate records?
10. Why validate business rules after cleaning?

---

## Advanced

11. Design a production-ready data cleaning pipeline.
12. Explain MCAR, MAR, and MNAR.
13. How would you clean a dataset with millions of rows?
14. Compare different imputation strategies.
15. How does data cleaning improve machine learning performance?

---

# 24. Cheat Sheet

| Task              | Syntax              |
| ----------------- | ------------------- |
| Missing Values    | `isna()`            |
| Count Missing     | `isna().sum()`      |
| Drop Rows         | `dropna()`          |
| Fill Missing      | `fillna()`          |
| Forward Fill      | `ffill()`           |
| Backward Fill     | `bfill()`           |
| Interpolation     | `interpolate()`     |
| Remove Duplicates | `drop_duplicates()` |
| Strip Spaces      | `.str.strip()`      |
| Title Case        | `.str.title()`      |
| Lowercase         | `.str.lower()`      |

---

# 25. Mini Project

## Enterprise Data Cleaning Pipeline

Using any retail, banking, healthcare, telecom, logistics, HR, or e-commerce dataset:

Complete the following tasks:

* Inspect the dataset.
* Detect missing values.
* Choose appropriate imputation methods.
* Remove duplicate records.
* Standardize text fields.
* Validate business rules.
* Check numerical ranges.
* Build an automated cleaning pipeline.
* Document **five executive-level business insights**.
* Recommend **three long-term improvements** for data quality.

### Example Business Insights

* Duplicate removal significantly improved reporting accuracy.
* Median imputation preserved numerical distributions better than mean imputation.
* Text standardization reduced inconsistencies in regional reporting.
* Business rule validation identified invalid records before analysis.
* Automated cleaning pipelines ensured consistent preprocessing across datasets.

---

# 26. Summary

Congratulations! 🎉

Today you mastered **Advanced Missing Data Handling & Data Cleaning Strategies**.

You learned how to:

* Understand different missing data mechanisms.
* Detect and analyze missing values.
* Apply multiple imputation techniques.
* Use forward fill, backward fill, and interpolation.
* Handle duplicates intelligently.
* Standardize text data.
* Validate business rules.
* Build reusable cleaning pipelines.

These skills are fundamental for production ETL pipelines, business intelligence, analytics, and machine learning.

---

# 27. What's Next?

In **Day 40**, you'll learn **Advanced Time Series Analysis with Pandas**.

Topics include:

* DateTime Indexing
* Time-Based Filtering
* Resampling
* Rolling Windows
* Expanding Windows
* Time Shifting
* Lag Features
* Moving Averages
* Seasonal Analysis
* Business Time-Series Analytics

Time-series analysis is widely used in finance, forecasting, sales analytics, IoT, healthcare, and operations management.

---

<div align="center">

# Day 39 Complete!

You've mastered **Advanced Data Cleaning & Missing Value Handling**, one of the most critical stages of every analytics workflow.

By combining statistical imputation, business validation, text standardization, and automated cleaning pipelines, you're now prepared to work with production-quality datasets.

</div>
