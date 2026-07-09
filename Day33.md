# Day 33 — Advanced Feature Engineering & Data Transformation

<div align="center">

# 100 Days of Pandas

### Day 33 · Creating Better Features for Better Insights

*"Raw data rarely produces great models. Great features transform raw information into meaningful business intelligence."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Feature%20Engineering-blue)
![Day](https://img.shields.io/badge/Day-33-orange)

</div>

---

# Table of Contents

1. Introduction
2. What is Feature Engineering?
3. Why Feature Engineering Matters
4. Learning Objectives
5. Creating New Features
6. Mathematical Feature Transformations
7. Date & Time Feature Extraction
8. Summary

---

# 1. Introduction

Feature Engineering is the process of creating new variables (features) from existing data to improve analysis and machine learning models.

Instead of using raw columns directly, analysts transform them into more meaningful representations.

Examples include:

* Revenue from Quantity × Price
* Customer Age from Date of Birth
* Profit Margin from Profit and Revenue
* Weekend Indicator from Date
* Customer Tenure from Signup Date

These engineered features often contain more useful information than the original columns.

---

# 2. What is Feature Engineering?

Suppose we have:

| Quantity | Price |
| -------: | ----: |
|        2 |   500 |
|        5 |   300 |
|        3 |   700 |

Instead of using both columns separately, create:

| Revenue |
| ------: |
|    1000 |
|    1500 |
|    2100 |

```python id="feature01"
df["Revenue"] = (
    df["Quantity"] *
    df["Price"]
)
```

The new feature provides direct business value.

---

# 3. Why Feature Engineering Matters

Businesses often ask questions such as:

* Which customers are high-value?
* Which products generate the highest profit?
* How long has a customer been active?
* What is the profit margin?
* Is an order placed on a weekend?

These questions require creating new features from existing data.

---

# 4. Learning Objectives

By the end of this lesson, you will be able to:

* Create new features.
* Transform numerical variables.
* Extract useful date components.
* Build business-oriented features.
* Prepare datasets for machine learning.

---

# 5. Creating New Features

New features are often simple mathematical combinations of existing columns.

### Revenue

```python id="feature02"
df["Revenue"] = (
    df["Quantity"] *
    df["Price"]
)
```

---

### Profit Margin

```python id="feature03"
df["Profit Margin"] = (
    df["Profit"]
    /
    df["Revenue"]
) * 100
```

---

### Average Order Value

```python id="feature04"
df["Average Order Value"] = (
    df["Revenue"]
    /
    df["Orders"]
)
```

---

### Discount Amount

```python id="feature05"
df["Discount Amount"] = (
    df["MRP"] -
    df["Selling Price"]
)
```

---

# 6. Mathematical Feature Transformations

Mathematical transformations help reduce skewness, stabilize variance, or prepare data for modeling.

---

## Square Root Transformation

```python id="math01"
df["Sqrt Sales"] = (
    np.sqrt(
        df["Sales"]
    )
)
```

---

## Log Transformation

```python id="math02"
df["Log Sales"] = (
    np.log1p(
        df["Sales"]
    )
)
```

`log1p()` safely handles zero values.

---

## Square Transformation

```python id="math03"
df["Sales Squared"] = (
    df["Sales"] ** 2
)
```

---

## Absolute Value

```python id="math04"
df["Absolute Profit"] = (
    df["Profit"].abs()
)
```

Useful when working with gains and losses.

---

# 7. Date & Time Feature Extraction

Dates contain valuable information.

Suppose the dataset has an **Order Date** column.

```python id="date01"
df["Order Date"] = (
    pd.to_datetime(
        df["Order Date"]
    )
)
```

Extract the year.

```python id="date02"
df["Year"] = (
    df["Order Date"]
      .dt.year
)
```

Extract the month.

```python id="date03"
df["Month"] = (
    df["Order Date"]
      .dt.month
)
```

Extract the day.

```python id="date04"
df["Day"] = (
    df["Order Date"]
      .dt.day
)
```

Extract the weekday.

```python id="date05"
df["Weekday"] = (
    df["Order Date"]
      .dt.day_name()
)
```

Extract the quarter.

```python id="date06"
df["Quarter"] = (
    df["Order Date"]
      .dt.quarter
)
```

---

## Weekend Indicator

```python id="date07"
df["Is Weekend"] = (
    df["Order Date"]
      .dt.dayofweek >= 5
)
```

Output

| Order Date | Is Weekend |
| ---------- | ---------- |
| 2026-07-10 | False      |
| 2026-07-11 | True       |

---

# Business Example

An e-commerce company stores:

* Order Date
* Quantity
* Price
* Profit

Analysts create:

* Revenue
* Profit Margin
* Quarter
* Weekday
* Weekend Indicator

These engineered features help identify seasonal trends, customer behavior, and profitability patterns.

---

# Best Practices

✔ Create features that answer business questions.

✔ Keep feature names descriptive.

✔ Reuse existing columns instead of duplicating logic.

✔ Validate newly created features.

✔ Remove temporary columns if they are no longer required.

---

# Common Mistakes

### Dividing by Zero

Incorrect:

```python id="mistake01"
df["Profit"] / df["Revenue"]
```

If `Revenue` contains zero, the result may be invalid.

Check or replace zero values before division.

---

### Keeping Dates as Strings

Always convert dates before extracting components.

```python id="mistake02"
pd.to_datetime()
```

---

### Creating Redundant Features

Avoid generating multiple features that convey exactly the same information unless there is a clear analytical reason.

---

# Key Takeaways

After completing this section, you should understand:

* What feature engineering is.
* How to create business-oriented features.
* How to perform mathematical transformations.
* How to extract useful information from dates.
* Why engineered features improve analysis and machine learning.

> **"Feature engineering transforms raw observations into meaningful variables that better capture business behavior and improve analytical outcomes."**

# 8. Binning Data with `cut()`

Continuous numerical values are often grouped into categories.

Suppose customer ages range from 18 to 80.

Instead of analyzing every individual age, group them into age brackets.

```python id="cut01"
df["Age Group"] = pd.cut(
    df["Age"],
    bins=[0,18,30,45,60,100],
    labels=[
        "Child",
        "Young Adult",
        "Adult",
        "Middle Age",
        "Senior"
    ]
)
```

### Output

| Age | Age Group   |
| --: | ----------- |
|  22 | Young Adult |
|  38 | Adult       |
|  67 | Senior      |

Binning simplifies reporting and visualization.

---

# 9. Quantile Binning with `qcut()`

Unlike `cut()`, `qcut()` creates bins containing approximately the same number of observations.

```python id="qcut01"
df["Sales Tier"] = pd.qcut(
    df["Sales"],
    q=4,
    labels=[
        "Low",
        "Medium",
        "High",
        "Premium"
    ]
)
```

Output

| Sales | Sales Tier |
| ----: | ---------- |
|  2500 | Low        |
|  6200 | Medium     |
|  9800 | High       |
| 14500 | Premium    |

Useful for:

* Customer segmentation
* Credit scoring
* Marketing campaigns

---

# 10. One-Hot Encoding

Machine learning algorithms usually require numerical inputs.

Categorical values such as:

| City   |
| ------ |
| Delhi  |
| Mumbai |
| Delhi  |

can be converted into dummy variables.

```python id="onehot01"
encoded = pd.get_dummies(
    df,
    columns=["City"]
)
```

Output

| City_Delhi | City_Mumbai |
| ---------- | ----------- |
| 1          | 0           |
| 0          | 1           |
| 1          | 0           |

Each category becomes a separate binary column.

---

## Drop First Category

To reduce redundancy:

```python id="onehot02"
encoded = pd.get_dummies(
    df,
    columns=["City"],
    drop_first=True
)
```

---

# 11. Label Encoding

Sometimes categories have an inherent order.

Example:

| Education    |
| ------------ |
| School       |
| Graduate     |
| Postgraduate |

Encode them manually.

```python id="label01"
education_map = {
    "School":1,
    "Graduate":2,
    "Postgraduate":3
}

df["Education"] = (
    df["Education"]
      .map(education_map)
)
```

Output

| Education |
| --------: |
|         1 |
|         2 |
|         3 |

Label encoding is appropriate only when categories have a meaningful order.

---

# 12. Scaling Numerical Features

Machine learning models often perform better when features have similar scales.

Example:

| Feature |  Value |
| ------- | -----: |
| Salary  | 850000 |
| Age     |     25 |

The large difference in magnitude can affect certain algorithms.

---

## Min-Max Normalization

Scale values between 0 and 1.

```python id="scale01"
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()

df["Scaled Sales"] = (
    scaler.fit_transform(
        df[["Sales"]]
    )
)
```

---

## Standardization (Z-Score)

Standardization centers data around a mean of 0 with a standard deviation of 1.

```python id="scale02"
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()

df["Standard Sales"] = (
    scaler.fit_transform(
        df[["Sales"]]
    )
)
```

---

# 13. Business Feature Engineering

Real-world analytics often requires creating domain-specific features.

### Customer Tenure

```python id="business01"
df["Tenure"] = (
    (
        pd.Timestamp.today()
        -
        df["Signup Date"]
    ).dt.days
)
```

---

### Discount Percentage

```python id="business02"
df["Discount %"] = (
    (
        df["MRP"]
        -
        df["Selling Price"]
    )
    /
    df["MRP"]
) * 100
```

---

### Average Revenue Per Customer

```python id="business03"
df["ARPC"] = (
    df["Revenue"]
    /
    df["Customers"]
)
```

---

### Profit Category

```python id="business04"
df["Profit Category"] = pd.cut(
    df["Profit"],
    bins=[-100000,0,10000,50000,1000000],
    labels=[
        "Loss",
        "Low Profit",
        "Medium Profit",
        "High Profit"
    ]
)
```

---

# 14. Preparing Data for Machine Learning

A typical preprocessing workflow:

```text id="workflow01"
Raw Data
     │
     ▼
Handle Missing Values
     │
     ▼
Remove Duplicates
     │
     ▼
Feature Engineering
     │
     ▼
Encoding
     │
     ▼
Scaling
     │
     ▼
Train/Test Split
     │
     ▼
Machine Learning Model
```

Feature engineering bridges raw data and predictive modeling.

---

# Business Example

An online retailer wants to predict customer churn.

Analysts create:

* Customer tenure
* Average order value
* Purchase frequency
* Discount percentage
* Customer spending tier

Then:

* Encode categorical variables.
* Scale numerical features.
* Train predictive models.

The engineered features improve model performance compared with using raw data alone.

---

# Best Practices

✔ Engineer features that solve business problems.

✔ Use `qcut()` for balanced customer segmentation.

✔ Use One-Hot Encoding for nominal categories.

✔ Use Label Encoding only for ordinal data.

✔ Scale numerical features before training distance-based models.

---

# Common Mistakes

### Using Label Encoding for Unordered Categories

Incorrect:

```text id="mistake01"
Red = 1

Blue = 2

Green = 3
```

This incorrectly implies an order.

Prefer One-Hot Encoding for nominal categories.

---

### Scaling Before Handling Missing Values

Always clean missing values before applying scalers.

---

### Creating Too Many Dummy Variables

High-cardinality columns (e.g., thousands of unique cities) can create an excessive number of columns.

Consider grouping rare categories or using alternative encoding techniques.

---

# Quick Recap

You have now learned how to:

* Bin continuous variables using `cut()`.
* Create quantile-based groups with `qcut()`.
* Apply One-Hot Encoding.
* Perform Label Encoding.
* Scale and standardize numerical features.
* Engineer business-oriented variables.
* Prepare datasets for machine learning.

> **"Well-designed features capture meaningful business patterns, enabling more accurate analysis, better decision-making, and stronger machine learning models."**

# 15. Enterprise Feature Engineering Workflow

Professional organizations rarely train machine learning models on raw data.

Instead, they follow a structured preprocessing workflow.

```text id="workflow01"
Raw Dataset
      │
      ▼
Data Cleaning
      │
      ▼
Handle Missing Values
      │
      ▼
Feature Engineering
      │
      ▼
Encoding
      │
      ▼
Scaling
      │
      ▼
Feature Selection
      │
      ▼
Model Training
```

Each stage improves data quality and predictive performance.

---

# 16. Building an End-to-End Feature Engineering Pipeline

Create a reusable preprocessing function.

```python id="pipeline01"
def prepare_data(df):

    df = (
        df
        .drop_duplicates()
        .dropna()
    )

    df["Revenue"] = (
        df["Quantity"] *
        df["Price"]
    )

    df["Profit Margin"] = (
        df["Profit"]
        /
        df["Revenue"]
    ) * 100

    df["Month"] = (
        df["Order Date"]
          .dt.month
    )

    df = pd.get_dummies(
        df,
        columns=["City"],
        drop_first=True
    )

    return df
```

Execute:

```python id="pipeline02"
processed_df = prepare_data(df)
```

Reusable pipelines make preprocessing consistent across projects.

---

# 17. Feature Selection

Not every feature improves a model.

Example dataset:

| Feature       | Useful? |
| ------------- | ------- |
| Customer ID   | ❌       |
| Revenue       | ✅       |
| Profit Margin | ✅       |
| Customer Name | ❌       |
| Age           | ✅       |

Remove irrelevant columns.

```python id="feature01"
df = df.drop(
    columns=[
        "Customer ID",
        "Customer Name"
    ]
)
```

Keeping only meaningful features reduces noise and improves model performance.

---

# 18. Performance Optimization

Feature engineering can become expensive for very large datasets.

### Prefer Vectorized Operations

Instead of loops:

```python id="perf01"
for i in range(len(df)):
    df.loc[i, "Revenue"] = (
        df.loc[i, "Quantity"] *
        df.loc[i, "Price"]
    )
```

Use vectorized calculations:

```python id="perf02"
df["Revenue"] = (
    df["Quantity"] *
    df["Price"]
)
```

---

### Reuse Engineered Features

Instead of repeatedly calculating:

```python id="perf03"
df["Profit"] / df["Revenue"]
```

Store the result.

```python id="perf04"
df["Profit Margin"] = (
    df["Profit"]
    /
    df["Revenue"]
)
```

---

### Convert Categorical Columns

```python id="perf05"
df["Region"] = (
    df["Region"]
      .astype("category")
)
```

This reduces memory usage and speeds up many operations.

---

# 19. Enterprise Case Study

## Scenario

You are working as a **Senior Data Scientist** at **RetailHub**.

The company wants to predict whether customers will make another purchase.

Available data:

* Customer Age
* City
* Order Date
* Quantity
* Price
* Profit
* Orders

---

## Business Questions

### Question 1

Create Revenue.

```python id="case01"
df["Revenue"] = (
    df["Quantity"] *
    df["Price"]
)
```

---

### Question 2

Calculate Profit Margin.

```python id="case02"
df["Profit Margin"] = (
    df["Profit"]
    /
    df["Revenue"]
) * 100
```

---

### Question 3

Extract Month.

```python id="case03"
df["Month"] = (
    df["Order Date"]
      .dt.month
)
```

---

### Question 4

Create Customer Spending Tier.

```python id="case04"
df["Spending Tier"] = (
    pd.qcut(
        df["Revenue"],
        q=4,
        labels=[
            "Low",
            "Medium",
            "High",
            "Premium"
        ]
    )
)
```

---

### Question 5

One-Hot Encode City.

```python id="case05"
df = pd.get_dummies(
    df,
    columns=["City"],
    drop_first=True
)
```

---

# 20. Business Insights

After feature engineering, analysts discover:

* Revenue is a stronger predictor than Quantity or Price individually.
* Customer spending tiers help identify premium customers.
* Seasonal features improve demand forecasting.
* Profit Margin provides better business insight than raw Profit.
* Engineered features improve model accuracy and interpretability.

---

# 21. Practice Exercises

## Beginner

1. Create a Revenue column.
2. Create a Profit Margin column.
3. Extract Year from a Date column.
4. Bin Age using `cut()`.
5. Encode a categorical column with `get_dummies()`.

---

## Intermediate

6. Use `qcut()` for customer segmentation.
7. Normalize a numerical feature.
8. Standardize another numerical feature.
9. Create a reusable preprocessing function.
10. Remove unnecessary columns.

---

## Advanced

11. Build a complete preprocessing pipeline.
12. Engineer five business features.
13. Compare `cut()` and `qcut()`.
14. Prepare a dataset for machine learning.
15. Evaluate which engineered features are most useful.

---

# 22. Interview Questions

## Beginner

1. What is feature engineering?
2. Why create new features?
3. Difference between `cut()` and `qcut()`?
4. What is One-Hot Encoding?
5. What is Label Encoding?

---

## Intermediate

6. When should you scale features?
7. Difference between normalization and standardization?
8. Why remove unnecessary features?
9. How do date features improve models?
10. Why use `get_dummies()`?

---

## Advanced

11. Design a preprocessing pipeline for customer churn prediction.
12. Explain business feature engineering with examples.
13. Compare vectorized feature creation and loops.
14. How would you engineer features for a recommendation system?
15. What practices improve reproducibility in feature engineering?

---

# 23. Cheat Sheet

| Task             | Syntax               |
| ---------------- | -------------------- |
| Create Feature   | `df["New"] = ...`    |
| Log Transform    | `np.log1p()`         |
| Square Root      | `np.sqrt()`          |
| Absolute Value   | `.abs()`             |
| Extract Year     | `.dt.year`           |
| Extract Month    | `.dt.month`          |
| Weekend Flag     | `.dt.dayofweek >= 5` |
| Binning          | `pd.cut()`           |
| Quantile Binning | `pd.qcut()`          |
| One-Hot Encoding | `pd.get_dummies()`   |
| Label Encoding   | `.map()`             |
| Normalization    | `MinMaxScaler()`     |
| Standardization  | `StandardScaler()`   |

---

# 24. Mini Project

## Customer Purchase Prediction Dataset

Using any retail, banking, telecom, healthcare, or e-commerce dataset:

Complete the following tasks:

* Handle missing values.
* Create Revenue and Profit Margin.
* Extract date-based features.
* Create customer tenure.
* Segment customers using `qcut()`.
* Apply One-Hot Encoding.
* Scale numerical features.
* Remove unnecessary columns.
* Build a reusable preprocessing pipeline.
* Write **five executive-level business insights**.
* Recommend **three new features** that could improve prediction accuracy.

### Example Business Insights

* Revenue is a more informative feature than Quantity or Price alone.
* Premium spending tiers contribute disproportionately to overall revenue.
* Seasonal features reveal recurring purchasing patterns.
* Encoded categorical variables improve machine learning compatibility.
* A standardized preprocessing pipeline ensures consistent model inputs.

---

# 25. Summary

Congratulations! 

Today you mastered **Advanced Feature Engineering & Data Transformation**.

You learned how to:

* Create business-focused features.
* Apply mathematical transformations.
* Extract useful date components.
* Bin continuous variables.
* Encode categorical variables.
* Scale and standardize numerical features.
* Build reusable preprocessing pipelines.

These techniques are essential for machine learning, predictive analytics, recommendation systems, fraud detection, and customer analytics.

---

# 26. What's Next?

In **Day 34**, you'll learn **Advanced Data Validation, Quality Assurance & Error Detection**.

Topics include:

* Data Validation Rules
* Constraint Checking
* Outlier Detection
* Consistency Checks
* Data Quality Metrics
* Schema Validation
* Error Reporting
* Automated Validation Pipelines
* Data Auditing
* Enterprise Data Quality Frameworks

These concepts are critical for building reliable, production-ready data pipelines and ensuring trustworthy analytical results.

---

<div align="center">

# Day 33 Complete!

You've mastered **Feature Engineering & Data Transformation**, one of the most impactful stages in any analytics or machine learning workflow.

By creating meaningful features and preparing high-quality datasets, you've taken an important step toward building production-ready analytical solutions.

</div>
