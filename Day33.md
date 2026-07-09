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
