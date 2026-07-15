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

# 8. One-Hot Encoding

Machine Learning algorithms cannot directly process categorical variables.

One-Hot Encoding converts each category into a separate binary column.

Example Dataset

| City |
|------|
|Delhi|
|Mumbai|
|Delhi|
|Pune|

Apply One-Hot Encoding.

```python
encoded = pd.get_dummies(
    df,
    columns=["City"]
)
```

Output

| City_Delhi | City_Mumbai | City_Pune |
|------------|------------|-----------|
|1|0|0|
|0|1|0|
|1|0|0|
|0|0|1|

---

# 9. Label Encoding

Instead of creating multiple columns, Label Encoding assigns an integer to each category.

```python
from sklearn.preprocessing import LabelEncoder

encoder = LabelEncoder()

df["Department"] = encoder.fit_transform(
    df["Department"]
)
```

Example

| Department | Encoded |
|------------|--------:|
|HR|0|
|IT|1|
|Sales|2|

**Use Label Encoding only when the encoded numbers do not imply an incorrect ordering or when the model can handle categorical labels appropriately.**

---

# 10. Binning (Discretization)

Continuous variables can be divided into groups.

Example:

```python
df["Age Group"] = pd.cut(

    df["Age"],

    bins=[0,18,35,60,100],

    labels=[
        "Child",
        "Young Adult",
        "Adult",
        "Senior"
    ]

)
```

Output

| Age | Age Group |
|----:|-----------|
|16|Child|
|24|Young Adult|
|48|Adult|
|72|Senior|

---

## Equal Frequency Binning

```python
df["Income Group"] = pd.qcut(

    df["Income"],

    q=4,

    labels=[
        "Low",
        "Medium",
        "High",
        "Very High"
    ]

)
```

Useful when each bin should contain roughly the same number of observations.

---

# 11. Interaction Features

Interaction features combine two or more variables.

Example:

```python
df["Sales_per_Customer"] = (

    df["Sales"]

    /

    df["Customers"]

)
```

Revenue per employee.

```python
df["Revenue_per_Employee"] = (

    df["Revenue"]

    /

    df["Employees"]

)
```

These features often provide more useful information than the original variables alone.

---

# 12. Polynomial Features

Polynomial features capture non-linear relationships.

Example:

```python
from sklearn.preprocessing import PolynomialFeatures

poly = PolynomialFeatures(
    degree=2,
    include_bias=False
)

new_features = poly.fit_transform(

    df[["Sales"]]

)
```

Generated features:

- Sales
- Sales²

For two variables (Sales, Profit), the output includes:

- Sales
- Profit
- Sales²
- Sales × Profit
- Profit²

Polynomial features are useful when relationships are non-linear.

---

# 13. Group-Based Features

Features can be created using grouped statistics.

Average sales by region.

```python
df["Regional Average"] = (

    df.groupby("Region")["Sales"]

      .transform("mean")

)
```

Maximum salary by department.

```python
df["Department Max Salary"] = (

    df.groupby("Department")["Salary"]

      .transform("max")

)
```

Median order value.

```python
df["Median Order"] = (

    df.groupby("Category")["Revenue"]

      .transform("median")

)
```

These features provide contextual information.

---

# 14. Rolling Features

Useful for time-series forecasting.

7-day moving average.

```python
df["Sales MA 7"] = (

    df["Sales"]

      .rolling(7)

      .mean()

)
```

30-day rolling maximum.

```python
df["Rolling Max"] = (

    df["Sales"]

      .rolling(30)

      .max()

)
```

Cumulative sales.

```python
df["Cumulative Sales"] = (

    df["Sales"]

      .cumsum()

)
```

Rolling features help capture trends over time.

---

# 15. Advanced Feature Engineering Pipeline

```python
df = (

    df

    .assign(

        Profit=lambda x:

            x["Revenue"] - x["Cost"],

        Profit_Margin=lambda x:

            (
                x["Profit"]

                /

                x["Revenue"]

            ) * 100,

        Average_Price=lambda x:

            x["Revenue"]

            /

            x["Quantity"]

    )

)
```

This pipeline creates multiple useful business features in a single step.

---

# Business Example

An online shopping platform wants to predict customer spending.

The analytics team creates:

- Profit
- Profit Margin
- Average Selling Price
- Customer Age Group
- Purchase Month
- Weekend Purchase Indicator
- Regional Average Spending
- Customer Lifetime Revenue

These engineered features significantly improve prediction accuracy.

---

# Best Practices

✔ Create features with business value.

✔ Use meaningful feature names.

✔ Normalize or scale numerical variables when required.

✔ Avoid highly correlated or redundant features.

✔ Document every engineered feature.

---

# Common Mistakes

### One-Hot Encoding High-Cardinality Columns

Columns with thousands of unique values (e.g., Customer IDs) create too many new columns.

---

### Data Leakage

Never create features using information from the future.

Example:

Using next month's sales to predict today's sales.

---

### Overengineering

Adding too many unnecessary features can increase model complexity without improving performance.

---

# Quick Recap

You have now learned how to:

- Apply One-Hot Encoding.
- Use Label Encoding.
- Create bins from continuous variables.
- Build interaction features.
- Generate polynomial features.
- Create group-based features.
- Generate rolling features.
- Build feature engineering pipelines.

> **"Good feature engineering transforms raw data into meaningful business intelligence, allowing machine learning models to learn richer patterns and produce better predictions."**

---



The final section will cover:

- Enterprise Feature Engineering Architecture
- Automated Feature Pipelines
- Feature Selection
- Production Best Practices
- Interview Questions (20+)
- Practice Exercises
- Cheat Sheet
- Mini Project
- Executive Business Insights
- Complete Day 45 Summary

# 16. Enterprise Feature Engineering Architecture

Modern machine learning pipelines separate feature engineering into reusable stages.

```
Raw Dataset
      │
      ▼
Data Cleaning
      │
      ▼
Missing Value Handling
      │
      ▼
Encoding
      │
      ▼
Scaling
      │
      ▼
Feature Engineering
      │
      ▼
Feature Selection
      │
      ▼
Model Training
      │
      ▼
Model Evaluation
      │
      ▼
Production Deployment
```

Each stage performs one specific task, making the pipeline easier to maintain, debug, and scale.

---

# 17. Automated Feature Engineering Pipeline

Instead of creating features manually every time, build reusable functions.

```python
def engineer_features(df):

    df = df.assign(

        Profit=lambda x:
            x["Revenue"] - x["Cost"],

        Profit_Margin=lambda x:
            (
                x["Revenue"] - x["Cost"]
            ) / x["Revenue"] * 100,

        Average_Price=lambda x:
            x["Revenue"] / x["Quantity"]

    )

    df["Order Date"] = pd.to_datetime(
        df["Order Date"]
    )

    df["Year"] = df["Order Date"].dt.year
    df["Month"] = df["Order Date"].dt.month
    df["Weekday"] = df["Order Date"].dt.day_name()

    return df
```

Run the pipeline.

```python
featured_df = engineer_features(df)
```

Reusable feature pipelines reduce duplication and improve consistency.

---

# 18. Feature Selection

Not every feature improves model performance.

Feature selection removes irrelevant or redundant variables.

Suppose the dataset contains:

- Customer ID
- Revenue
- Cost
- Profit
- Region
- Email

Email and Customer ID may not contribute to prediction.

Select useful columns.

```python
selected = df[

    [

        "Revenue",

        "Cost",

        "Profit",

        "Region"

    ]

]
```

---

## Drop Unnecessary Columns

```python
df = df.drop(

    columns=[

        "Customer ID",

        "Email"

    ]

)
```

Removing irrelevant features often improves model performance and reduces training time.

---

# 19. Production Best Practices

### Create Business-Relevant Features

Every feature should answer a business question.

Example:

Instead of

```
Feature1
```

Prefer

```
Customer Lifetime Value
```

---

### Keep Pipelines Reusable

Create reusable functions instead of repeating feature engineering code.

---

### Document Every Feature

Example:

```
Profit

Revenue - Cost
```

Future developers should understand how each feature is calculated.

---

### Avoid Data Leakage

Never use future information while creating training features.

Incorrect:

Using next month's revenue to predict today's sales.

Correct:

Use only historical information available at prediction time.

---

### Validate Features

Check for:

- Missing values
- Infinite values
- Duplicate columns
- Incorrect data types

Example:

```python
df.isna().sum()
```

---

# 20. Enterprise Case Study

## Scenario

You are a **Machine Learning Engineer** at an online retail company.

Objective:

Predict whether a customer will make another purchase.

Available data:

- Customer ID
- Revenue
- Cost
- Quantity
- Order Date
- Region

Feature Engineering Steps:

```python
df["Profit"] = (

    df["Revenue"]

    -

    df["Cost"]

)

df["Average Price"] = (

    df["Revenue"]

    /

    df["Quantity"]

)

df["Order Date"] = pd.to_datetime(
    df["Order Date"]
)

df["Order Month"] = (
    df["Order Date"].dt.month
)
```

Additional Features:

```python
df["Weekend"] = (

    df["Order Date"]

    .dt.dayofweek >= 5

)
```

These features help the model identify seasonal purchasing behavior.

---

# 21. Business Insights

After implementing feature engineering:

- Profit Margin becomes a stronger predictor than Revenue alone.
- Customer purchase behavior differs between weekdays and weekends.
- Regional average sales improve demand prediction.
- Aggregated customer features increase model accuracy.
- Well-designed features reduce the need for highly complex algorithms.

---

# 22. Practice Exercises

## Beginner

1. Create a Profit column.
2. Calculate Profit Margin.
3. Extract Year and Month.
4. Create a Weekend indicator.
5. Scale the Sales column.

---

## Intermediate

6. Apply One-Hot Encoding.
7. Create interaction features.
8. Build group-based features.
9. Create rolling averages.
10. Build a reusable feature engineering function.

---

## Advanced

11. Design an enterprise feature pipeline.
12. Engineer customer-level features.
13. Create forecasting features.
14. Build automated feature engineering.
15. Prepare a dataset for machine learning.

---

# 23. Interview Questions

## Beginner

1. What is feature engineering?
2. Why is feature engineering important?
3. What is One-Hot Encoding?
4. What is Label Encoding?
5. Why scale numerical variables?

---

## Intermediate

6. Explain interaction features.
7. What is feature binning?
8. Why create rolling features?
9. What is feature selection?
10. What is data leakage?

---

## Advanced

11. Design a feature engineering pipeline.
12. Compare One-Hot and Label Encoding.
13. How do engineered features improve model performance?
14. How would you engineer features for customer churn prediction?
15. Explain production feature engineering workflows.

---

# 24. Cheat Sheet

| Task | Syntax |
|------|--------|
| New Feature | `df["New"] = ...` |
| One-Hot Encoding | `pd.get_dummies()` |
| Label Encoding | `LabelEncoder()` |
| Binning | `pd.cut()` |
| Equal Frequency Binning | `pd.qcut()` |
| Scaling | `StandardScaler()` |
| Min-Max Scaling | `MinMaxScaler()` |
| Rolling Mean | `rolling().mean()` |
| Group Feature | `groupby().transform()` |
| Date Feature | `.dt.year`, `.dt.month` |

---

# 25. Mini Project

## Customer Purchase Prediction Feature Pipeline

Using any retail, banking, healthcare, HR, telecom, or finance dataset:

Complete the following tasks:

- Create at least **10 new features**.
- Generate date-based features.
- Build interaction features.
- Apply categorical encoding.
- Scale numerical variables.
- Create grouped features.
- Select the most useful features.
- Build an automated feature engineering pipeline.
- Write **five executive-level business insights**.
- Recommend **three improvements** for future feature engineering.

### Example Business Insights

- Profit Margin predicts customer behavior better than Revenue alone.
- Weekend purchases show higher average order values.
- Group-level features improve customer segmentation.
- Encoded categorical variables improve machine learning compatibility.
- Feature engineering significantly increases predictive capability.

---

# 26. Summary

Congratulations! 🎉

Today you mastered **Advanced Feature Engineering with Pandas**.

You learned how to:

- Create new business features.
- Build mathematical features.
- Extract date and time features.
- Apply One-Hot and Label Encoding.
- Perform feature scaling.
- Create interaction and grouped features.
- Build rolling features.
- Select useful features.
- Design production-ready feature engineering pipelines.

These techniques are fundamental to machine learning, predictive analytics, recommendation systems, fraud detection, customer segmentation, and forecasting.

---

# 27. What's Next?

## 🐼 Day 46 — Advanced Exploratory Data Analysis (EDA) with Pandas

Topics include:

- Statistical Profiling
- Distribution Analysis
- Correlation Analysis
- Outlier Detection
- Missing Data Visualization
- Categorical Analysis
- Numerical Analysis
- Feature Relationships
- Business Insight Generation
- Automated EDA Reports

EDA is one of the most important stages in every Data Science and Data Analytics project because it helps uncover hidden patterns before modeling.

---

# Day 45 Complete!

You have successfully completed **Advanced Feature Engineering with Pandas**.

You can now transform raw datasets into high-quality, machine-learning-ready features that improve predictive performance and business insights.

