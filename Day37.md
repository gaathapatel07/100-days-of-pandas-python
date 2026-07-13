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
