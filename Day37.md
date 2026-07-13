# Day 37 — Advanced Exploratory Data Analysis (EDA) with Pandas

<div align="center">

# 100 Days of Pandas

### Day 37 · Discovering Insights Before Modeling

*"Exploratory Data Analysis is where data transforms from raw numbers into meaningful business understanding."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Exploratory%20Data%20Analysis-blue)
![Day](https://img.shields.io/badge/Day-37-orange)

</div>

---

# Table of Contents

1. Introduction
2. What is Exploratory Data Analysis?
3. Why EDA Matters
4. Learning Objectives
5. Initial Data Exploration
6. Dataset Overview
7. Summary Statistics
8. Summary

---

# 1. Introduction

Exploratory Data Analysis (EDA) is the process of understanding a dataset before drawing conclusions or building predictive models.

Rather than immediately creating dashboards or training machine learning algorithms, analysts first investigate:

* Dataset structure
* Missing values
* Data types
* Statistical summaries
* Relationships between variables
* Outliers
* Business patterns

EDA helps ensure that later analyses are based on accurate and meaningful data.

---

# 2. What is Exploratory Data Analysis?

EDA is the systematic investigation of data to answer questions such as:

* What does the dataset contain?
* Is the data complete?
* Are there unusual values?
* Which variables appear related?
* Are there seasonal or business patterns?
* Does the dataset contain errors?

EDA combines statistics, visualization, and domain knowledge.

---

# 3. Why EDA Matters

Imagine an online retailer provides a sales dataset.

Before building a sales forecasting model, analysts need to know:

* How many records exist?
* Which columns contain missing values?
* Which products sell the most?
* Are there duplicate transactions?
* Are sales normally distributed?
* Which variables correlate with profit?

EDA answers these questions efficiently.

---

# 4. Learning Objectives

By the end of this lesson, you will be able to:

* Perform a structured EDA workflow.
* Understand dataset characteristics.
* Generate statistical summaries.
* Identify data quality issues.
* Discover business insights.

---

# 5. Initial Data Exploration

After loading a dataset, begin with a quick inspection.

Display the first rows.

```python id="eda01"
df.head()
```

Display the last rows.

```python id="eda02"
df.tail()
```

Random sample.

```python id="eda03"
df.sample(5)
```

These commands provide a quick understanding of the dataset.

---

# 6. Dataset Overview

Determine the dataset size.

```python id="overview01"
df.shape
```

Example output

```text id="overview02"
(50000, 12)
```

Meaning:

* 50,000 rows
* 12 columns

---

List column names.

```python id="overview03"
df.columns
```

Inspect data types.

```python id="overview04"
df.dtypes
```

Detailed summary.

```python id="overview05"
df.info()
```

Example

```text id="overview06"
RangeIndex: 50000 entries

Data columns: 12

Memory usage: 4.6 MB
```

---

# 7. Summary Statistics

Generate descriptive statistics.

```python id="stats01"
df.describe()
```

Example

| Statistic | Sales |
| --------- | ----: |
| Count     | 50000 |
| Mean      |  5820 |
| Std       |  1450 |
| Min       |   800 |
| 25%       |  4800 |
| 50%       |  5600 |
| 75%       |  6600 |
| Max       | 18000 |

---

Include categorical columns.

```python id="stats02"
df.describe(
    include="all"
)
```

This summarizes:

* Numerical variables
* Categorical variables
* Missing values
* Most frequent categories

---

## Individual Statistics

Mean

```python id="stats03"
df["Sales"].mean()
```

Median

```python id="stats04"
df["Sales"].median()
```

Mode

```python id="stats05"
df["Region"].mode()
```

Standard deviation

```python id="stats06"
df["Sales"].std()
```

Variance

```python id="stats07"
df["Sales"].var()
```

---

## Quantiles

```python id="stats08"
df["Sales"].quantile(0.25)

df["Sales"].quantile(0.50)

df["Sales"].quantile(0.75)
```

Quantiles help understand the spread of the data.

---

# Business Example

A retail company receives a new dataset.

Analysts begin by:

* Checking dataset size.
* Inspecting column names.
* Reviewing data types.
* Calculating descriptive statistics.
* Understanding revenue distribution.

This initial exploration helps determine the next analytical steps.

---

# Best Practices

✔ Always inspect the dataset before analysis.

✔ Review data types immediately after importing.

✔ Generate descriptive statistics early.

✔ Understand the scale of numerical variables.

✔ Keep notes about unusual findings during exploration.

---

# Common Mistakes

### Jumping Directly Into Modeling

Always perform EDA before training machine learning models or creating dashboards.

---

### Ignoring Data Types

Incorrect data types can produce misleading statistics or calculation errors.

Always review:

```python id="mistake01"
df.info()
```

---

### Looking Only at the First Few Rows

Use `head()`, `tail()`, and `sample()` together to obtain a broader understanding of the dataset.

---

# Key Takeaways

After completing this section, you should understand:

* What EDA is.
* Why EDA is essential.
* How to inspect a dataset.
* How to generate descriptive statistics.
* How to understand the overall structure of data.

> **"Exploratory Data Analysis is the foundation of every successful analytics project. Understanding your data first leads to more reliable insights and better decisions."**

# 8. Missing Value Analysis

Missing data is one of the most common issues in real-world datasets.

Determine missing values in each column.

```python id="missing01"
df.isna().sum()
```

Example

| Column | Missing Values |
| ------ | -------------: |
| Age    |             12 |
| Sales  |              0 |
| Region |              3 |

---

## Missing Value Percentage

```python id="missing02"
(
    df.isna().sum()
    /
    len(df)
) * 100
```

This helps prioritize which columns need cleaning.

---

## Visualizing Missing Patterns

Sort missing values.

```python id="missing03"
missing = (
    df.isna()
      .sum()
      .sort_values(
          ascending=False
      )
)
```

This quickly highlights the columns with the highest number of missing entries.

---

# 9. Duplicate Analysis

Duplicate records can distort KPIs.

Count duplicates.

```python id="duplicate01"
df.duplicated().sum()
```

Display duplicates.

```python id="duplicate02"
duplicates = (
    df[
        df.duplicated(
            keep=False
        )
    ]
)
```

Remove duplicates.

```python id="duplicate03"
df = (
    df.drop_duplicates()
)
```

---

# 10. Univariate Analysis

Univariate analysis studies **one variable at a time**.

Example:

Sales distribution.

```python id="uni01"
df["Sales"].describe()
```

Unique categories.

```python id="uni02"
df["Region"].value_counts()
```

Relative frequency.

```python id="uni03"
df["Region"].value_counts(
    normalize=True
)
```

Output

| Region | Percentage |
| ------ | ---------: |
| North  |       0.35 |
| South  |       0.28 |
| East   |       0.22 |
| West   |       0.15 |

---

# 11. Bivariate Analysis

Bivariate analysis examines relationships between **two variables**.

Average sales by region.

```python id="bi01"
df.groupby(
    "Region"
)["Sales"].mean()
```

Average profit by category.

```python id="bi02"
df.groupby(
    "Category"
)["Profit"].mean()
```

Cross-tabulation.

```python id="bi03"
pd.crosstab(
    df["Region"],
    df["Category"]
)
```

This reveals relationships between categorical variables.

---

# 12. Correlation Analysis

Correlation measures the strength of relationships between numerical variables.

Compute the correlation matrix.

```python id="corr01"
df.corr(
    numeric_only=True
)
```

Example

|          | Sales | Profit | Quantity |
| -------- | ----: | -----: | -------: |
| Sales    |  1.00 |   0.82 |     0.67 |
| Profit   |  0.82 |   1.00 |     0.45 |
| Quantity |  0.67 |   0.45 |     1.00 |

---

## Strong Correlation

```text id="corr02"
+1.0
```

Strong positive relationship.

---

## Weak Correlation

```text id="corr03"
0.05
```

Very little relationship.

---

## Negative Correlation

```text id="corr04"
-0.75
```

One variable increases while the other decreases.

---

# 13. Outlier Analysis

Outliers can influence averages and machine learning models.

Calculate quartiles.

```python id="outlier01"
Q1 = (
    df["Sales"]
      .quantile(0.25)
)

Q3 = (
    df["Sales"]
      .quantile(0.75)
)
```

Calculate IQR.

```python id="outlier02"
IQR = Q3 - Q1
```

Determine boundaries.

```python id="outlier03"
lower = Q1 - 1.5 * IQR

upper = Q3 + 1.5 * IQR
```

Detect outliers.

```python id="outlier04"
outliers = (
    df[
        (df["Sales"] < lower)
        |
        (df["Sales"] > upper)
    ]
)
```

Always investigate outliers before removing them.

---

# 14. Distribution Analysis

Measure skewness.

```python id="dist01"
df["Sales"].skew()
```

Interpretation:

| Value | Meaning      |
| ----: | ------------ |
|   ≈ 0 | Symmetric    |
|   > 0 | Right-skewed |
|   < 0 | Left-skewed  |

---

Measure kurtosis.

```python id="dist02"
df["Sales"].kurt()
```

Kurtosis describes the heaviness of the distribution tails.

---

# 15. Business-Driven EDA

Professional analysts explore data based on business questions.

Examples:

**Revenue by Region**

```python id="business01"
df.groupby(
    "Region"
)["Revenue"].sum()
```

---

**Top Products**

```python id="business02"
df.groupby(
    "Product"
)["Revenue"]\
.sum()\
.sort_values(
    ascending=False
)
.head(10)
```

---

**Monthly Revenue**

```python id="business03"
df.groupby(
    "Month"
)["Revenue"].sum()
```

---

**Customer Spending**

```python id="business04"
df.groupby(
    "Customer"
)["Revenue"].sum()
```

Business-focused EDA connects statistical findings to real decision-making.

---

# Business Example

An e-commerce company explores its sales dataset.

Analysts investigate:

* Missing customer information.
* Duplicate transactions.
* Best-selling product categories.
* Correlation between discount and profit.
* Revenue trends across regions.
* High-value customer segments.

These insights guide pricing, marketing, and inventory decisions.

---

# Best Practices

✔ Check missing values before analysis.

✔ Investigate duplicates carefully.

✔ Combine statistical summaries with business understanding.

✔ Analyze both numerical and categorical variables.

✔ Validate unusual findings before reporting them.

---

# Common Mistakes

### Assuming Correlation Implies Causation

A strong correlation does **not** prove that one variable causes another.

Additional analysis is required to establish causal relationships.

---

### Removing Every Outlier

Some outliers represent legitimate business events, such as exceptionally large orders or promotional campaigns.

Review them before deciding whether to exclude them.

---

### Ignoring Categorical Variables

EDA should include both numerical and categorical data.

Frequency tables and group-based summaries often reveal valuable business insights.

---

# Quick Recap

You have now learned how to:

* Analyze missing values.
* Detect duplicate records.
* Perform univariate analysis.
* Perform bivariate analysis.
* Measure correlations.
* Detect outliers.
* Analyze distributions.
* Conduct business-focused exploratory analysis.

> **"Effective EDA combines statistical techniques with business understanding, allowing analysts to uncover patterns, identify risks, and discover opportunities hidden within the data."**

# 16. Enterprise EDA Workflow

Professional organizations follow a structured exploratory workflow before any reporting or modeling.

```text id="workflow01"
Raw Dataset
      │
      ▼
Initial Inspection
      │
      ▼
Data Quality Assessment
      │
      ▼
Missing Value Analysis
      │
      ▼
Duplicate Detection
      │
      ▼
Statistical Summary
      │
      ▼
Relationship Analysis
      │
      ▼
Business Insights
      │
      ▼
Visualization & Reporting
      │
      ▼
Machine Learning / Dashboard
```

Following a consistent workflow improves reproducibility and reduces the chance of overlooking important issues.

---

# 17. Automated EDA Report

Instead of manually checking every aspect of a dataset, create a reusable function.

```python id="auto01"
def eda_summary(df):

    summary = {
        "Rows": len(df),
        "Columns": len(df.columns),
        "Missing Values":
            df.isna().sum().sum(),
        "Duplicate Rows":
            df.duplicated().sum(),
        "Memory Usage (MB)":
            round(
                df.memory_usage(
                    deep=True
                ).sum() / 1024**2,
                2
            )
    }

    return pd.DataFrame(
        summary.items(),
        columns=[
            "Metric",
            "Value"
        ]
    )
```

Run the report.

```python id="auto02"
eda_report = eda_summary(df)

print(eda_report)
```

Example Output

| Metric            | Value |
| ----------------- | ----: |
| Rows              | 50000 |
| Columns           |    12 |
| Missing Values    |    15 |
| Duplicate Rows    |     3 |
| Memory Usage (MB) |  4.82 |

---

# 18. EDA Checklist

Before beginning visualization or machine learning, review the following checklist.

| Task                        | Status |
| --------------------------- | ------ |
| Inspect dataset structure   | ☐      |
| Check data types            | ☐      |
| Review missing values       | ☐      |
| Remove duplicates           | ☐      |
| Generate summary statistics | ☐      |
| Analyze distributions       | ☐      |
| Check correlations          | ☐      |
| Detect outliers             | ☐      |
| Review business KPIs        | ☐      |
| Document observations       | ☐      |

This checklist helps ensure that no critical step is missed.

---

# 19. Performance Optimization During EDA

EDA on large datasets should be efficient.

### Sample Large Datasets

```python id="perf01"
sample = df.sample(
    n=10000,
    random_state=42
)
```

Sampling speeds up exploratory analysis while preserving overall patterns.

---

### Analyze Only Required Columns

```python id="perf02"
df[
    [
        "Sales",
        "Profit"
    ]
].describe()
```

Avoid unnecessary computations on unrelated columns.

---

### Use Efficient Data Types

```python id="perf03"
df["Region"] = (
    df["Region"]
      .astype("category")
)
```

Efficient data types reduce memory usage during exploration.

---

# 20. Enterprise Case Study

## Scenario

You are a **Senior Data Analyst** at **RetailHub**.

The company has launched operations in multiple new cities.

Management wants a complete exploratory analysis before creating executive dashboards.

Available data:

* Orders
* Revenue
* Profit
* Customers
* Region
* Product Category
* Discounts

---

## Business Questions

### Question 1

Generate descriptive statistics.

```python id="case01"
df.describe(
    include="all"
)
```

---

### Question 2

Analyze missing values.

```python id="case02"
df.isna().sum()
```

---

### Question 3

Identify duplicate records.

```python id="case03"
df.duplicated().sum()
```

---

### Question 4

Measure relationships between numerical variables.

```python id="case04"
df.corr(
    numeric_only=True
)
```

---

### Question 5

Identify the highest revenue regions.

```python id="case05"
df.groupby(
    "Region"
)["Revenue"]\
.sum()\
.sort_values(
    ascending=False
)
```

---

# 21. Business Insights

After completing the EDA, analysts identify:

* Revenue is concentrated in a small number of regions.
* Most products generate moderate sales, while a few products contribute disproportionately to total revenue.
* Missing values are limited and can be addressed without major data loss.
* Profit is positively correlated with revenue, though some high-revenue products have relatively low profit margins.
* Customer purchasing behavior varies across regions, suggesting opportunities for localized marketing strategies.

---

# 22. Practice Exercises

## Beginner

1. Display the first and last five rows.
2. Check the shape of the dataset.
3. Review data types.
4. Generate descriptive statistics.
5. Count missing values.

---

## Intermediate

6. Remove duplicate records.
7. Calculate missing value percentages.
8. Perform correlation analysis.
9. Detect outliers using the IQR method.
10. Analyze category frequencies.

---

## Advanced

11. Build an automated EDA report.
12. Create an EDA checklist.
13. Perform business-focused exploratory analysis.
14. Optimize EDA for a large dataset.
15. Document business insights from a real-world dataset.

---

# 23. Interview Questions

## Beginner

1. What is Exploratory Data Analysis?
2. Why is EDA important?
3. What does `describe()` return?
4. How do you identify missing values?
5. Why check duplicate records?

---

## Intermediate

6. Explain univariate and bivariate analysis.
7. What is correlation analysis?
8. How do you detect outliers?
9. Why is data type validation important during EDA?
10. How would you summarize a new dataset?

---

## Advanced

11. Design a complete EDA workflow for a retail dataset.
12. How would you perform EDA on a dataset with 100 million rows?
13. Explain how EDA supports machine learning projects.
14. How would you automate EDA reporting?
15. What are the most common mistakes analysts make during EDA?

---

# 24. Cheat Sheet

| Task               | Syntax               |
| ------------------ | -------------------- |
| First Rows         | `head()`             |
| Last Rows          | `tail()`             |
| Random Sample      | `sample()`           |
| Shape              | `shape`              |
| Data Types         | `dtypes`             |
| Dataset Info       | `info()`             |
| Summary Statistics | `describe()`         |
| Missing Values     | `isna().sum()`       |
| Duplicates         | `duplicated().sum()` |
| Correlation        | `corr()`             |
| Value Counts       | `value_counts()`     |
| Quantiles          | `quantile()`         |

---

# 25. Mini Project

## Complete Exploratory Data Analysis Report

Using any retail, banking, healthcare, HR, telecom, logistics, or e-commerce dataset:

Complete the following tasks:

* Inspect dataset structure.
* Validate data types.
* Analyze missing values.
* Detect duplicate records.
* Generate descriptive statistics.
* Analyze numerical distributions.
* Perform correlation analysis.
* Detect outliers.
* Build an automated EDA summary.
* Document **five executive-level business insights**.
* Recommend **three areas for further investigation**.

### Example Business Insights

* Revenue is concentrated among a relatively small number of products.
* Certain regions consistently outperform others in sales.
* Missing values are minimal and unlikely to significantly affect overall analysis.
* Profit margins vary considerably across product categories.
* Strong relationships between selected business metrics suggest opportunities for targeted optimization.

---

# 26. Summary

Congratulations! 🎉

Today you mastered **Advanced Exploratory Data Analysis (EDA)** with Pandas.

You learned how to:

* Explore new datasets systematically.
* Assess data quality.
* Analyze missing values and duplicates.
* Generate descriptive statistics.
* Perform univariate and bivariate analysis.
* Measure correlations.
* Detect outliers.
* Automate EDA reporting.

These techniques form the foundation of every successful analytics, visualization, and machine learning project.

---

# 27. What's Next?

In **Day 38**, you'll learn **Advanced Data Aggregation & Business Analytics**.

Topics include:

* Advanced `groupby()` techniques
* Multi-level aggregation
* Custom aggregation functions
* Pivot tables vs GroupBy
* Cohort analysis
* Customer segmentation
* Business KPI calculations
* Funnel analysis
* Sales performance analytics
* Executive reporting

These techniques are widely used in business intelligence, financial reporting, customer analytics, and executive dashboards.

---

<div align="center">

# Day 37 Complete!

You've mastered **Exploratory Data Analysis (EDA)**, one of the most valuable skills for every Data Analyst and Data Scientist.

By systematically exploring data before reporting or modeling, you can identify issues early, uncover meaningful business patterns, and build more reliable analytical solutions.

 **Next → Day 38: Advanced Data Aggregation & Business Analytics** 

</div>
