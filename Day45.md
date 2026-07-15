# Day 45 — Advanced Feature Engineering with Pandas

## Introduction

Feature Engineering is the process of creating new input variables (features) from raw data to improve analytical insights and machine learning model performance.

Many data scientists agree that **better features often improve model performance more than switching to a more complex algorithm.**

Feature engineering combines domain knowledge with data transformation techniques to extract meaningful information from existing data.

---

#  Topics Covered

- What is Feature Engineering?
- Why Feature Engineering Matters
- Creating New Features
- Mathematical Features
- Date & Time Features
- Feature Scaling
- Binning
- Categorical Encoding
- Aggregated Features
- Rolling Features
- Production Feature Pipelines

---

# 1. What is Feature Engineering?

Feature Engineering is the process of transforming raw variables into informative features.

Example:

Original Dataset

| Revenue | Cost |
|---------:|-----:|
|1200|800|
|5000|3500|

Create Profit

| Revenue | Cost | Profit |
|---------:|-----:|-------:|
|1200|800|400|
|5000|3500|1500|

Instead of using Revenue and Cost separately, Profit becomes a more meaningful feature.

---

# 2. Why Feature Engineering Matters

Suppose you're predicting customer purchases.

Original variables:

- Age
- Salary
- Purchase Amount

Engineered features:

- Purchase Frequency
- Average Monthly Spending
- Customer Lifetime Value
- Days Since Last Purchase

These features often provide better predictive power than the raw variables.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

- Create meaningful features.
- Build mathematical features.
- Extract date features.
- Encode categorical variables.
- Scale numerical variables.
- Prepare datasets for machine learning.

---

# 4. Creating New Features

New features can be calculated using existing columns.

Example:

```python
df["Profit"] = (
    df["Revenue"] -
    df["Cost"]
)
```

Profit Margin

```python
df["Profit Margin"] = (
    df["Profit"] /
    df["Revenue"]
) * 100
```

Average Selling Price

```python
df["Average Price"] = (
    df["Revenue"] /
    df["Quantity"]
)
```

---

# 5. Mathematical Features

Feature engineering often involves mathematical transformations.

Square

```python
df["Sales Squared"] = (
    df["Sales"] ** 2
)
```

Square Root

```python
import numpy as np

df["Sales Root"] = np.sqrt(
    df["Sales"]
)
```

Log Transformation

```python
df["Log Sales"] = np.log1p(
    df["Sales"]
)
```

Absolute Difference

```python
df["Difference"] = (
    abs(
        df["Revenue"] -
        df["Cost"]
    )
)
```

---

# 6. Date & Time Features

Convert to DateTime.

```python
df["Order Date"] = pd.to_datetime(
    df["Order Date"]
)
```

Extract Year.

```python
df["Year"] = (
    df["Order Date"].dt.year
)
```

Extract Month.

```python
df["Month"] = (
    df["Order Date"].dt.month
)
```

Extract Weekday.

```python
df["Weekday"] = (
    df["Order Date"].dt.day_name()
)
```

Weekend Indicator.

```python
df["Weekend"] = (
    df["Order Date"]
      .dt.dayofweek >= 5
)
```

Days Since Order.

```python
df["Days Since Order"] = (
    pd.Timestamp.today() -
    df["Order Date"]
).dt.days
```

---

# 7. Feature Scaling

Machine learning algorithms often perform better when numerical features are scaled.

Min-Max Scaling

```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()

df["Sales Scaled"] = scaler.fit_transform(
    df[["Sales"]]
)
```

Standard Scaling

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()

df["Sales Standard"] = scaler.fit_transform(
    df[["Sales"]]
)
```

---

# Business Example

A retail company wants to predict future sales.

The analytics team creates:

- Profit
- Profit Margin
- Average Selling Price
- Year
- Month
- Weekend Indicator
- Days Since Last Purchase

These engineered features improve forecasting accuracy.

---

# Best Practices

✔ Create features with business meaning.

✔ Avoid unnecessary features.

✔ Scale numerical variables when required.

✔ Keep feature names descriptive.

✔ Document every engineered feature.

---

# Common Mistakes

### Creating Redundant Features

Highly correlated features may not improve model performance.

---

### Data Leakage

Never create features using future information that would not be available at prediction time.

---

### Ignoring Business Context

Features should solve business problems rather than being created arbitrarily.

---

# Key Takeaways

After completing this section, you should understand:

- What feature engineering is.
- Why engineered features improve models.
- How to create mathematical features.
- How to build date-based features.
- Why feature scaling is important.

> **"Feature engineering transforms raw data into meaningful information, enabling machine learning models and analytics systems to deliver more accurate and actionable insights."**

---



The next section covers:

- One-Hot Encoding
- Label Encoding
- Binning
- Interaction Features
- Polynomial Features
- Group-Based Features
- Rolling Features
- Advanced Feature Engineering# Day 45 — Advanced Feature Engineering with Pandas

## Introduction

Feature Engineering is the process of creating new input variables (features) from raw data to improve analytical insights and machine learning model performance.

Many data scientists agree that **better features often improve model performance more than switching to a more complex algorithm.**

Feature engineering combines domain knowledge with data transformation techniques to extract meaningful information from existing data.

---

# Topics Covered

- What is Feature Engineering?
- Why Feature Engineering Matters
- Creating New Features
- Mathematical Features
- Date & Time Features
- Feature Scaling
- Binning
- Categorical Encoding
- Aggregated Features
- Rolling Features
- Production Feature Pipelines

---

# 1. What is Feature Engineering?

Feature Engineering is the process of transforming raw variables into informative features.

Example:

Original Dataset

| Revenue | Cost |
|---------:|-----:|
|1200|800|
|5000|3500|

Create Profit

| Revenue | Cost | Profit |
|---------:|-----:|-------:|
|1200|800|400|
|5000|3500|1500|

Instead of using Revenue and Cost separately, Profit becomes a more meaningful feature.

---

# 2. Why Feature Engineering Matters

Suppose you're predicting customer purchases.

Original variables:

- Age
- Salary
- Purchase Amount

Engineered features:

- Purchase Frequency
- Average Monthly Spending
- Customer Lifetime Value
- Days Since Last Purchase

These features often provide better predictive power than the raw variables.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

- Create meaningful features.
- Build mathematical features.
- Extract date features.
- Encode categorical variables.
- Scale numerical variables.
- Prepare datasets for machine learning.

---

# 4. Creating New Features

New features can be calculated using existing columns.

Example:

```python
df["Profit"] = (
    df["Revenue"] -
    df["Cost"]
)
```

Profit Margin

```python
df["Profit Margin"] = (
    df["Profit"] /
    df["Revenue"]
) * 100
```

Average Selling Price

```python
df["Average Price"] = (
    df["Revenue"] /
    df["Quantity"]
)
```

---

# 5. Mathematical Features

Feature engineering often involves mathematical transformations.

Square

```python
df["Sales Squared"] = (
    df["Sales"] ** 2
)
```

Square Root

```python
import numpy as np

df["Sales Root"] = np.sqrt(
    df["Sales"]
)
```

Log Transformation

```python
df["Log Sales"] = np.log1p(
    df["Sales"]
)
```

Absolute Difference

```python
df["Difference"] = (
    abs(
        df["Revenue"] -
        df["Cost"]
    )
)
```

---

# 6. Date & Time Features

Convert to DateTime.

```python
df["Order Date"] = pd.to_datetime(
    df["Order Date"]
)
```

Extract Year.

```python
df["Year"] = (
    df["Order Date"].dt.year
)
```

Extract Month.

```python
df["Month"] = (
    df["Order Date"].dt.month
)
```

Extract Weekday.

```python
df["Weekday"] = (
    df["Order Date"].dt.day_name()
)
```

Weekend Indicator.

```python
df["Weekend"] = (
    df["Order Date"]
      .dt.dayofweek >= 5
)
```

Days Since Order.

```python
df["Days Since Order"] = (
    pd.Timestamp.today() -
    df["Order Date"]
).dt.days
```

---

# 7. Feature Scaling

Machine learning algorithms often perform better when numerical features are scaled.

Min-Max Scaling

```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()

df["Sales Scaled"] = scaler.fit_transform(
    df[["Sales"]]
)
```

Standard Scaling

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()

df["Sales Standard"] = scaler.fit_transform(
    df[["Sales"]]
)
```

---

# Business Example

A retail company wants to predict future sales.

The analytics team creates:

- Profit
- Profit Margin
- Average Selling Price
- Year
- Month
- Weekend Indicator
- Days Since Last Purchase

These engineered features improve forecasting accuracy.

---

# Best Practices

✔ Create features with business meaning.

✔ Avoid unnecessary features.

✔ Scale numerical variables when required.

✔ Keep feature names descriptive.

✔ Document every engineered feature.

---

# Common Mistakes

### Creating Redundant Features

Highly correlated features may not improve model performance.

---

### Data Leakage

Never create features using future information that would not be available at prediction time.

---

### Ignoring Business Context

Features should solve business problems rather than being created arbitrarily.

---

# Key Takeaways

After completing this section, you should understand:

- What feature engineering is.
- Why engineered features improve models.
- How to create mathematical features.
- How to build date-based features.
- Why feature scaling is important.

> **"Feature engineering transforms raw data into meaningful information, enabling machine learning models and analytics systems to deliver more accurate and actionable insights."**

---



The next section covers:

- One-Hot Encoding
- Label Encoding
- Binning
- Interaction Features
- Polynomial Features
- Group-Based Features
- Rolling Features
- Advanced Feature Engineering
