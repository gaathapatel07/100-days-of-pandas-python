# Day 21 — Missing Data Handling & Advanced Data Cleaning

<div align="center">

# 100 Days of Pandas

### Day 21 · Cleaning Data for Reliable Analysis

*"Clean data is the foundation of every accurate insight, dashboard, and machine learning model."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Data%20Cleaning-blue)
![Day](https://img.shields.io/badge/Day-21-orange)

</div>

---


# Table of Contents

1. Introduction
2. Why Data Cleaning Matters
3. Learning Objectives
4. Understanding Missing Data
5. Detecting Missing Values
6. Removing Missing Values
7. Filling Missing Values
8. Summary

---

# 1. Introduction

In the real world, datasets are rarely perfect.

Missing values, duplicate records, incorrect formats, inconsistent spellings, and invalid entries are common challenges that analysts face daily.

Examples include:

* Customers who forgot to enter their phone numbers.
* Products with missing prices.
* Survey respondents skipping questions.
* Sensors temporarily failing to record readings.
* Employees with incomplete personal information.

Before performing analysis or building machine learning models, these issues must be identified and corrected.

Pandas provides a comprehensive set of tools for cleaning data efficiently.

---

# 2. Why Data Cleaning Matters

Imagine you're working for an e-commerce company.

The sales dataset contains:

| Order ID | Customer | City   | Sales |
| -------- | -------- | ------ | ----: |
| 1001     | Alice    | Delhi  |  5200 |
| 1002     | Rahul    | NaN    |  6100 |
| 1003     | Priya    | Mumbai |   NaN |
| 1004     | Arjun    | Delhi  |  7200 |

If missing values are ignored:

* Sales reports become inaccurate.
* Dashboards display incorrect KPIs.
* Machine learning models produce unreliable predictions.
* Business decisions become less trustworthy.

Cleaning the data ensures higher accuracy and consistency.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Detect missing values.
* Remove incomplete records.
* Fill missing values using different strategies.
* Understand when each method should be used.
* Build cleaner datasets for analysis.

---

# 4. Understanding Missing Data

Missing values represent unknown or unavailable information.

In Pandas, missing values are typically represented as:

```python
NaN
```

which stands for **Not a Number**.

Although originally designed for numerical data, `NaN` is commonly used to represent missing values in many data types.

Example:

| Name  | Age | Salary |
| ----- | --: | -----: |
| Alice |  25 |  52000 |
| Rahul | NaN |  61000 |
| Priya |  28 |    NaN |

Missing values require careful handling because many calculations ignore or propagate them.

---

# 5. Detecting Missing Values

## Using `isnull()`

Check whether values are missing.

```python id="null01"
df.isnull()
```

Output:

| Name  | Age   | Salary |
| ----- | ----- | ------ |
| False | False | False  |
| False | True  | False  |
| False | False | True   |

`True` indicates a missing value.

---

## Using `isna()`

`isna()` is identical to `isnull()`.

```python id="null02"
df.isna()
```

Both functions can be used interchangeably.

---

## Counting Missing Values

Calculate the number of missing values in each column.

```python id="null03"
df.isnull().sum()
```

Example Output:

| Column | Missing Values |
| ------ | -------------: |
| Age    |              1 |
| Salary |              1 |

This is one of the most frequently used commands during exploratory data analysis.

---

## Total Missing Values

Calculate the total number of missing values in the entire dataset.

```python id="null04"
df.isnull().sum().sum()
```

This provides an overall measure of data completeness.

---

## Percentage of Missing Values

Understanding percentages is often more useful than raw counts.

```python id="null05"
(
    df.isnull().sum()
    /
    len(df)
) * 100
```

Example Output:

| Column | Missing (%) |
| ------ | ----------: |
| Age    |        10.0 |
| Salary |         5.0 |

This helps prioritize which columns require attention.

---

# 6. Removing Missing Values

Sometimes incomplete records should be removed.

Use `dropna()`.

```python id="drop01"
df.dropna()
```

This removes every row containing at least one missing value.

---

## Removing Columns

Delete columns containing missing values.

```python id="drop02"
df.dropna(
    axis=1
)
```

Here:

* `axis=0` → Rows (default)
* `axis=1` → Columns

---

## Removing Rows with All Missing Values

```python id="drop03"
df.dropna(
    how="all"
)
```

Only rows where **every value** is missing are removed.

---

## Removing Rows Based on a Threshold

Keep rows that contain at least three non-missing values.

```python id="drop04"
df.dropna(
    thresh=3
)
```

Threshold-based filtering is useful when datasets contain partial information.

---

# 7. Filling Missing Values

Instead of deleting information, analysts often replace missing values.

## Fill with a Constant

```python id="fill01"
df.fillna(0)
```

Useful for:

* Missing quantities.
* Missing counts.
* Default numeric values.

---

## Fill Individual Columns

```python id="fill02"
df["City"] = (
    df["City"]
      .fillna("Unknown")
)
```

Categorical variables are often filled using descriptive labels.

---

## Fill with the Mean

```python id="fill03"
df["Salary"] = (
    df["Salary"]
      .fillna(
          df["Salary"].mean()
      )
)
```

Suitable for approximately symmetric numerical distributions.

---

## Fill with the Median

```python id="fill04"
df["Age"] = (
    df["Age"]
      .fillna(
          df["Age"].median()
      )
)
```

Median is preferred when outliers exist.

---

## Fill with the Mode

```python id="fill05"
df["City"] = (
    df["City"]
      .fillna(
          df["City"].mode()[0]
      )
)
```

Useful for categorical variables.

---

# Business Example

A healthcare company maintains patient records.

Several entries are incomplete because patients skipped optional questions.

Instead of removing these records entirely, analysts:

* Replace missing ages with the median age.
* Fill missing cities using the most common city.
* Replace missing test results only after consulting domain experts.

This preserves valuable information while improving data quality.

---

# Best Practices

✔ Investigate why data is missing before filling it.

✔ Use the median when numerical data contains outliers.

✔ Use the mode for categorical variables.

✔ Avoid deleting large amounts of valuable information.

✔ Always validate the cleaned dataset after modifications.

---

# Common Mistakes

### Filling Every Column with Zero

Incorrect:

```python id="mistake01"
df.fillna(0)
```

This may introduce unrealistic values for names, cities, or dates.

Instead, choose filling strategies appropriate for each column.

---

### Removing Too Much Data

Deleting rows indiscriminately can reduce the dataset and introduce bias.

Always evaluate the impact before using `dropna()`.

---


# Key Takeaways

After completing this section, you should understand:

* How to detect missing values.
* Different methods for removing incomplete records.
* Appropriate strategies for filling missing data.
* When to use the mean, median, or mode.
* Why careful data cleaning improves analytical accuracy.

> **"Effective data cleaning is not about removing imperfections—it is about making informed decisions that preserve the integrity and usefulness of the data."**

# 8. Forward Fill (`ffill()`)

Sometimes missing values should inherit the previous valid value.

This technique is called **Forward Fill**.

It is especially useful for:

* Time-series data
* Sensor readings
* Stock prices
* Inventory records
* Weather observations

---

## Example Dataset

| Date       | Temperature |
| ---------- | ----------: |
| 2025-01-01 |          28 |
| 2025-01-02 |         NaN |
| 2025-01-03 |          31 |
| 2025-01-04 |         NaN |

Apply Forward Fill.

```python id="ffill01"
df.ffill()
```

### Output

| Date       | Temperature |
| ---------- | ----------: |
| 2025-01-01 |          28 |
| 2025-01-02 |          28 |
| 2025-01-03 |          31 |
| 2025-01-04 |          31 |

The last valid value is propagated forward.

---

## Filling Selected Columns

```python id="ffill02"
df["Temperature"] = (
    df["Temperature"]
      .ffill()
)
```

This affects only the selected column.

---

# 9. Backward Fill (`bfill()`)

Backward Fill replaces missing values using the **next valid observation**.

```python id="bfill01"
df.bfill()
```

Example Output:

| Date       | Temperature |
| ---------- | ----------: |
| 2025-01-01 |          28 |
| 2025-01-02 |          31 |
| 2025-01-03 |          31 |
| 2025-01-04 |          35 |

Backward filling is useful when future observations are more appropriate than previous ones.

---

# 10. Interpolation

Interpolation estimates missing values mathematically instead of copying neighboring values.

Example:

| Day | Sales |
| --: | ----: |
|   1 |   100 |
|   2 |   NaN |
|   3 |   140 |

Apply interpolation.

```python id="interp01"
df["Sales"] = (
    df["Sales"]
      .interpolate()
)
```

Output:

| Day | Sales |
| --: | ----: |
|   1 |   100 |
|   2 |   120 |
|   3 |   140 |

Interpolation is commonly used in:

* Scientific research
* Engineering
* IoT
* Environmental monitoring
* Financial forecasting

---

# 11. Detecting Duplicate Records

Duplicate rows can distort business reports and KPIs.

Example:

| Order ID | Customer | Sales |
| -------- | -------- | ----: |
| 1001     | Alice    |  5200 |
| 1001     | Alice    |  5200 |
| 1002     | Rahul    |  6100 |

Identify duplicates.

```python id="dup01"
df.duplicated()
```

Returns:

```text id="duptext01"
False
True
False
```

---

## Count Duplicate Rows

```python id="dup02"
df.duplicated().sum()
```

This reports the total number of duplicate records.

---

# 12. Removing Duplicate Records

Remove duplicate rows.

```python id="dup03"
df.drop_duplicates()
```

---

## Remove Duplicates Based on Specific Columns

```python id="dup04"
df.drop_duplicates(
    subset=[
        "Order ID"
    ]
)
```

This keeps only one record for each unique Order ID.

---

## Keep the Last Duplicate

```python id="dup05"
df.drop_duplicates(
    subset="Order ID",
    keep="last"
)
```

Options include:

* `"first"` (default)
* `"last"`
* `False` (remove all duplicates)

---

# 13. Cleaning Text Data

Real-world datasets often contain inconsistent text.

Example:

| City  |
| ----- |
| Delhi |
| delhi |
| DELHI |
| Delhi |

These values should represent the same city.

---

## Remove Extra Spaces

```python id="text01"
df["City"] = (
    df["City"]
      .str.strip()
)
```

---

## Convert to Lowercase

```python id="text02"
df["City"] = (
    df["City"]
      .str.lower()
)
```

---

## Convert to Uppercase

```python id="text03"
df["City"] = (
    df["City"]
      .str.upper()
)
```

---

## Convert to Title Case

```python id="text04"
df["City"] = (
    df["City"]
      .str.title()
)
```

Output:

```text id="title01"
Delhi
Mumbai
Bangalore
```

This creates consistent formatting.

---

# 14. Replacing Incorrect Values

Suppose customers entered inconsistent city names.

```text id="replace01"
Bombay
Mumbai
MUMBAI
```

Standardize them.

```python id="replace02"
df["City"] = (
    df["City"]
      .replace(
          {
              "Bombay":"Mumbai",
              "MUMBAI":"Mumbai"
          }
      )
)
```

Consistent values improve grouping and reporting.

---

# 15. Correcting Data Types

Imported datasets often assign incorrect data types.

Check data types.

```python id="dtype01"
df.dtypes
```

Convert Sales into numeric values.

```python id="dtype02"
df["Sales"] = (
    pd.to_numeric(
        df["Sales"]
    )
)
```

Convert dates.

```python id="dtype03"
df["Order Date"] = (
    pd.to_datetime(
        df["Order Date"]
    )
)
```

Correct data types improve performance and reduce errors.

---

# Business Example

A retail company receives customer records from multiple branches.

Problems include:

* Duplicate customers
* Different spellings of cities
* Missing phone numbers
* Blank product categories
* Numeric values stored as text

Cleaning these issues ensures:

* Accurate dashboards
* Reliable customer analytics
* Better forecasting
* Higher-quality machine learning models

---

# Best Practices

✔ Standardize text before grouping.

✔ Remove duplicate records carefully.

✔ Verify data types after importing.

✔ Prefer interpolation only for continuous numerical data.

✔ Document every cleaning step for reproducibility.

---

# Common Mistakes

### Blindly Removing Duplicates

Not every duplicate row is incorrect.

Always verify whether duplicate records represent repeated transactions or genuine data-entry errors.

---

### Converting Text Without Cleaning

Applying `.str.lower()` before removing extra spaces may still leave inconsistent values.

Recommended order:

1. `strip()`
2. `replace()`
3. `lower()` or `title()`

---

### Interpolating Categorical Data

Interpolation should only be applied to continuous numerical variables.

Never interpolate names, cities, or product categories.

---

# Quick Recap

You have now learned how to:

* Fill missing values using Forward Fill and Backward Fill.
* Estimate missing numerical values with interpolation.
* Detect and remove duplicate records.
* Standardize text formatting.
* Replace inconsistent values.
* Correct data types for accurate analysis.

> **"High-quality analysis begins with high-quality data. Every cleaned value improves the reliability of insights, dashboards, and predictive models."**

# 16. Detecting Outliers

Not every unusual value is incorrect, but outliers should always be investigated.

Outliers may indicate:

* Data entry errors
* Fraudulent transactions
* Equipment malfunctions
* Exceptional business events

Identifying outliers helps improve data quality and prevents misleading analysis.

---

## Using the IQR Method

The **Interquartile Range (IQR)** is one of the most commonly used techniques for detecting outliers.

### Step 1: Calculate Quartiles

```python id="outlier01"
Q1 = df["Sales"].quantile(0.25)

Q3 = df["Sales"].quantile(0.75)
```

---

### Step 2: Calculate IQR

```python id="outlier02"
IQR = Q3 - Q1
```

---

### Step 3: Define Outlier Limits

```python id="outlier03"
lower_limit = Q1 - 1.5 * IQR

upper_limit = Q3 + 1.5 * IQR
```

---

### Step 4: Detect Outliers

```python id="outlier04"
outliers = df[
    (df["Sales"] < lower_limit) |
    (df["Sales"] > upper_limit)
]
```

These records should be reviewed before deciding whether to remove or retain them.

---

# 17. Detecting Outliers Using Z-Score

For approximately normally distributed data, the **Z-score** measures how many standard deviations a value lies from the mean.

```python id="zscore01"
from scipy.stats import zscore

df["Z_Score"] = (
    zscore(df["Sales"])
)
```

Identify extreme values.

```python id="zscore02"
outliers = df[
    df["Z_Score"].abs() > 3
]
```

A Z-score greater than **3** or less than **−3** is commonly treated as an outlier.

---

# 18. Data Validation

Before analysis, verify that the dataset satisfies basic quality rules.

### Example Validation Rules

| Column     | Rule                   |
| ---------- | ---------------------- |
| Age        | Must be greater than 0 |
| Sales      | Cannot be negative     |
| Quantity   | Must be an integer     |
| Email      | Cannot be blank        |
| Order Date | Must be a valid date   |

Validation prevents invalid records from entering reports or predictive models.

---

## Example Validation

```python id="validate01"
invalid_sales = df[
    df["Sales"] < 0
]
```

Check for impossible ages.

```python id="validate02"
invalid_age = df[
    df["Age"] < 0
]
```

---

# 19. Complete Data Cleaning Workflow

A professional data cleaning workflow follows a structured process.

```text id="workflow01"
Raw Dataset
      │
      ▼
Inspect Dataset
      │
      ▼
Identify Missing Values
      │
      ▼
Handle Missing Values
      │
      ▼
Remove Duplicates
      │
      ▼
Standardize Text
      │
      ▼
Correct Data Types
      │
      ▼
Validate Data
      │
      ▼
Detect Outliers
      │
      ▼
Export Clean Dataset
```

Following a consistent workflow improves reliability and reproducibility.

---

# 20. Real-World Business Case Study

## Scenario

You are working as a **Senior Data Analyst** at **RetailHub**, a multinational retail company.

The company receives sales data from hundreds of stores every day.

Unfortunately, the dataset contains:

* Missing customer details
* Duplicate invoices
* Inconsistent city names
* Incorrect data types
* Negative sales values
* Outlier transactions

Your responsibility is to prepare the dataset before it is used for executive dashboards and forecasting.

---

# Business Questions

### Question 1

Count missing values.

```python id="case_clean01"
df.isnull().sum()
```

---

### Question 2

Replace missing salaries with the median.

```python id="case_clean02"
df["Salary"] = (
    df["Salary"]
      .fillna(
          df["Salary"].median()
      )
)
```

---

### Question 3

Remove duplicate invoices.

```python id="case_clean03"
df.drop_duplicates(
    subset="Invoice ID"
)
```

---

### Question 4

Standardize city names.

```python id="case_clean04"
df["City"] = (
    df["City"]
      .str.strip()
      .str.title()
)
```

---

### Question 5

Identify abnormal sales transactions.

```python id="case_clean05"
Q1 = df["Sales"].quantile(0.25)

Q3 = df["Sales"].quantile(0.75)

IQR = Q3 - Q1

outliers = df[
    (df["Sales"] < Q1 - 1.5 * IQR) |
    (df["Sales"] > Q3 + 1.5 * IQR)
]
```

---

# 21. Business Insights

After cleaning the dataset, you discover:

* Duplicate invoices inflated reported sales.
* Standardizing city names improved regional reporting accuracy.
* Missing values were concentrated in older records.
* Several extreme sales values represented genuine bulk purchases rather than errors.
* Data validation significantly improved dashboard reliability.

These findings ensure that business decisions are based on trustworthy information.

---

# 22. Practice Exercises

## Beginner

1. Detect missing values.
2. Remove incomplete rows.
3. Fill missing values using the mean.
4. Identify duplicate rows.
5. Remove duplicate records.

---

## Intermediate

6. Apply Forward Fill.
7. Apply Backward Fill.
8. Standardize text values.
9. Convert incorrect data types.
10. Detect outliers using IQR.

---

## Advanced

11. Detect outliers using Z-score.
12. Build a complete cleaning pipeline.
13. Validate numerical columns.
14. Prepare a dataset for machine learning.
15. Write five recommendations to improve data quality.

---

# 23. Interview Questions

## Beginner

1. Why is data cleaning important?
2. What is `NaN`?
3. Difference between `isnull()` and `isna()`?
4. What is `dropna()`?
5. What is `fillna()`?

---

## Intermediate

6. Difference between Forward Fill and Backward Fill?
7. When should interpolation be used?
8. Why remove duplicates?
9. What is IQR?
10. Why standardize text?

---

## Advanced

11. Explain an end-to-end data cleaning workflow.
12. Compare IQR and Z-score for outlier detection.
13. How does poor data quality affect machine learning?
14. Describe validation rules for a sales dataset.
15. How would you clean a dataset with millions of records?

---

# 24. Cheat Sheet

| Task                  | Syntax              |
| --------------------- | ------------------- |
| Detect Missing Values | `isnull()`          |
| Count Missing Values  | `isnull().sum()`    |
| Remove Missing Rows   | `dropna()`          |
| Fill Missing Values   | `fillna()`          |
| Forward Fill          | `ffill()`           |
| Backward Fill         | `bfill()`           |
| Interpolate           | `interpolate()`     |
| Detect Duplicates     | `duplicated()`      |
| Remove Duplicates     | `drop_duplicates()` |
| Strip Spaces          | `str.strip()`       |
| Lowercase             | `str.lower()`       |
| Title Case            | `str.title()`       |
| Replace Values        | `replace()`         |
| Convert to Numeric    | `pd.to_numeric()`   |
| Convert to Date       | `pd.to_datetime()`  |

---

# 25. Mini Project

## Customer Data Quality Improvement System

Using any retail, healthcare, banking, HR, or e-commerce dataset:

Perform the following tasks:

* Detect missing values and calculate their percentage.
* Handle missing values using suitable techniques.
* Remove duplicate records.
* Standardize categorical values.
* Correct incorrect data types.
* Detect outliers using both the IQR and Z-score methods.
* Validate important business rules (e.g., Sales ≥ 0, Age > 0).
* Export the cleaned dataset.
* Write **five executive-level business insights**.
* Recommend **three improvements** to improve future data collection.

### Example Business Insights

* Duplicate customer records caused inflated customer counts.
* Missing salary values were concentrated in legacy employee records.
* Standardized city names improved regional sales reporting.
* Most detected outliers were legitimate high-value transactions rather than errors.
* Data validation reduced inconsistencies before dashboard generation.

---

# 26. Summary

Congratulations! 🎉

Today you mastered **Missing Data Handling & Advanced Data Cleaning** in Pandas.

You learned how to:

* Detect and handle missing values.
* Remove or impute incomplete records.
* Apply Forward Fill, Backward Fill, and interpolation.
* Detect and remove duplicates.
* Standardize text values.
* Correct data types.
* Detect outliers using IQR and Z-score.
* Validate datasets before analysis.

These techniques are essential for producing reliable datasets that support accurate dashboards, statistical analysis, and machine learning.

---

# 27. What's Next?

In **Day 22**, you'll learn **Advanced String Operations & Regular Expressions (Regex) in Pandas**.

Topics include:

* String Accessor (`.str`)
* Searching and Filtering Text
* `contains()`
* `startswith()`
* `endswith()`
* `replace()`
* Splitting and Joining Strings
* Extracting Patterns with Regular Expressions
* Email, Phone Number & URL Validation
* Text Cleaning for Real-World Datasets

These techniques are widely used in customer analytics, web scraping, natural language preprocessing, and enterprise data cleaning.

---

<div align="center">

# 🎉 Day 21 Complete!

You've mastered one of the most valuable skills in data analytics—transforming messy, incomplete, and inconsistent datasets into clean, trustworthy data ready for analysis.

These techniques form the backbone of every professional data science, business intelligence, and machine learning workflow.

⭐ **Next → Day 22: Advanced String Operations & Regular Expressions** 🔤🐼

</div>
