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

