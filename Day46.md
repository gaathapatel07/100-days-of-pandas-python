# Day 46 — Advanced Exploratory Data Analysis (EDA) with Pandas

## Introduction

Exploratory Data Analysis (EDA) is the process of analyzing datasets to understand their structure, identify patterns, detect anomalies, discover relationships, and generate business insights before building statistical or machine learning models.

EDA is one of the most important phases of every Data Analytics and Data Science project because **better understanding of the data leads to better business decisions and better models.**

---

# Topics Covered

- What is EDA?
- Why EDA Matters
- Dataset Overview
- Statistical Profiling
- Numerical Feature Analysis
- Categorical Feature Analysis
- Distribution Analysis
- Correlation Analysis
- Missing Data Analysis
- Outlier Detection

---

# 1. What is Exploratory Data Analysis?

EDA is the process of investigating a dataset before performing advanced analysis.

Goals:

- Understand the dataset
- Detect missing values
- Identify outliers
- Find relationships
- Discover hidden patterns
- Validate assumptions

Example Questions:

- Which region has the highest sales?
- Which product category is most profitable?
- Are there seasonal trends?
- Are there unusual observations?

---

# 2. Why EDA Matters

Suppose a company wants to predict customer churn.

Before building a model, analysts should know:

- Missing values
- Distribution of customer ages
- Correlation between variables
- Outliers
- Class imbalance

Without EDA, model performance often suffers.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

- Explore any dataset efficiently.
- Summarize numerical and categorical data.
- Detect missing values.
- Identify outliers.
- Analyze distributions.
- Measure relationships between variables.
- Generate business insights.

---

# 4. Dataset Overview

View the first records.

```python
df.head()
```

View the last records.

```python
df.tail()
```

Shape of dataset.

```python
df.shape
```

Column names.

```python
df.columns
```

Data types.

```python
df.dtypes
```

General information.

```python
df.info()
```

---

# 5. Statistical Profiling

Generate descriptive statistics.

```python
df.describe()
```

Include categorical columns.

```python
df.describe(include="all")
```

Calculate median.

```python
df.median(numeric_only=True)
```

Calculate variance.

```python
df.var(numeric_only=True)
```

Calculate standard deviation.

```python
df.std(numeric_only=True)
```

---

# 6. Numerical Feature Analysis

Calculate mean.

```python
df["Sales"].mean()
```

Maximum.

```python
df["Sales"].max()
```

Minimum.

```python
df["Sales"].min()
```

Range.

```python
df["Sales"].max() - df["Sales"].min()
```

Quantiles.

```python
df["Sales"].quantile([0.25,0.5,0.75])
```

---

# 7. Categorical Feature Analysis

Unique values.

```python
df["Region"].unique()
```

Number of unique values.

```python
df["Region"].nunique()
```

Frequency counts.

```python
df["Region"].value_counts()
```

Percentage distribution.

```python
df["Region"].value_counts(normalize=True) * 100
```

---

# 8. Distribution Analysis

Understand how numerical values are distributed.

Histogram.

```python
df["Sales"].plot.hist()
```

Density plot.

```python
df["Sales"].plot.kde()
```

Skewness.

```python
df["Sales"].skew()
```

Kurtosis.

```python
df["Sales"].kurt()
```

Interpretation:

- Skewness > 0 → Right-skewed
- Skewness < 0 → Left-skewed
- Skewness ≈ 0 → Approximately symmetric

---

# 9. Correlation Analysis

Correlation measures relationships between numerical variables.

Correlation matrix.

```python
df.corr(numeric_only=True)
```

Correlation with Sales.

```python
df.corr(numeric_only=True)["Sales"]
```

Interpretation:

| Correlation | Meaning |
|------------|---------|
|1|Perfect Positive|
|0|No Relationship|
|-1|Perfect Negative|

---

# 10. Missing Data Analysis

Count missing values.

```python
df.isna().sum()
```

Percentage.

```python
(df.isna().mean()*100).round(2)
```

Rows containing missing values.

```python
df[df.isna().any(axis=1)]
```

---

# 11. Outlier Detection

Use the Interquartile Range (IQR) method.

```python
Q1 = df["Sales"].quantile(0.25)

Q3 = df["Sales"].quantile(0.75)

IQR = Q3 - Q1

outliers = df[
    (df["Sales"] < Q1 - 1.5*IQR) |
    (df["Sales"] > Q3 + 1.5*IQR)
]
```

Count outliers.

```python
len(outliers)
```

---

# Business Example

An e-commerce company performs EDA before forecasting sales.

Analysts discover:

- Missing customer ages.
- Sales concentrated during weekends.
- One region contributing 45% of revenue.
- High-value outliers caused by bulk purchases.
- Strong correlation between advertising spend and sales.

These findings guide both business decisions and model development.

---

# Best Practices

✔ Perform EDA before modeling.

✔ Understand every variable.

✔ Investigate missing values.

✔ Validate unusual observations.

✔ Document important findings.

---

# Common Mistakes

### Ignoring Outliers

Outliers may indicate either data quality issues or valuable business events.

---

### Looking Only at Averages

Always examine the full distribution, not just the mean.

---

### Ignoring Categorical Variables

Categories often contain valuable business insights.

---

# Key Takeaways

After completing this section, you should understand:

- Dataset structure.
- Statistical summaries.
- Numerical and categorical analysis.
- Distribution analysis.
- Correlation analysis.
- Missing value analysis.
- Outlier detection.

> **"Exploratory Data Analysis is the bridge between raw data and meaningful insights. A thorough EDA uncovers hidden patterns that drive better business decisions and stronger predictive models."**

---



The next section covers:

- GroupBy Analysis
- Pivot Tables
- Crosstabs
- Multi-level Aggregations
- Feature Relationships
- Time-based EDA
- Automated Insight Generation
- Business KPI Analysis

# 12. GroupBy Analysis

Grouping helps summarize data by categories.

Calculate total sales by region.

```python
df.groupby("Region")["Sales"].sum()
```

Average sales by region.

```python
df.groupby("Region")["Sales"].mean()
```

Maximum revenue by department.

```python
df.groupby("Department")["Revenue"].max()
```

Multiple aggregations.

```python
df.groupby("Region").agg({

    "Sales": ["sum", "mean", "max"],

    "Profit": ["sum", "mean"]

})
```

Example Output

| Region | Total Sales | Average Sales |
|---------|------------:|--------------:|
|North|250000|5200|
|South|210000|4900|
|East|180000|4700|
|West|300000|5600|

---

# 13. Pivot Tables

Pivot tables summarize large datasets efficiently.

Total revenue by region.

```python
pd.pivot_table(

    df,

    values="Revenue",

    index="Region",

    aggfunc="sum"

)
```

Multiple aggregations.

```python
pd.pivot_table(

    df,

    values="Sales",

    index="Region",

    columns="Category",

    aggfunc="mean"

)
```

Example Output

| Region | Electronics | Clothing |
|---------|------------:|----------:|
|North|5800|4200|
|South|5300|3900|

---

# 14. Crosstab Analysis

Crosstabs analyze relationships between categorical variables.

```python
pd.crosstab(

    df["Gender"],

    df["Purchased"]

)
```

Percentage table.

```python
pd.crosstab(

    df["Gender"],

    df["Purchased"],

    normalize="index"

) * 100
```

Example Output

| Gender | Yes | No |
|---------|----:|---:|
|Male|62%|38%|
|Female|57%|43%|

---

# 15. Multi-Level Aggregation

Aggregate using multiple grouping variables.

```python
df.groupby(

    ["Region", "Category"]

).agg(

    Revenue=("Revenue", "sum"),

    Profit=("Profit", "mean"),

    Orders=("Order ID", "count")

)
```

This provides deeper insights across multiple business dimensions.

---

# 16. Feature Relationship Analysis

Analyze relationships between variables.

Revenue vs Profit correlation.

```python
df[

    ["Revenue", "Profit"]

].corr()
```

Covariance.

```python
df[

    ["Sales", "Profit"]

].cov()
```

Strong correlations may indicate important business relationships.

---

# 17. Time-Based EDA

Convert date column.

```python
df["Order Date"] = pd.to_datetime(
    df["Order Date"]
)
```

Monthly sales.

```python
monthly_sales = (

    df.groupby(

        df["Order Date"].dt.month

    )["Sales"]

      .sum()

)
```

Weekday sales.

```python
weekday_sales = (

    df.groupby(

        df["Order Date"].dt.day_name()

    )["Sales"]

      .mean()

)
```

Quarterly revenue.

```python
quarterly = (

    df.groupby(

        df["Order Date"].dt.quarter

    )["Revenue"]

      .sum()

)
```

These analyses reveal seasonal trends.

---

# 18. Business KPI Analysis

Calculate Total Revenue.

```python
df["Revenue"].sum()
```

Average Order Value.

```python
df["Revenue"].mean()
```

Highest Revenue.

```python
df["Revenue"].max()
```

Total Orders.

```python
len(df)
```

Unique Customers.

```python
df["Customer ID"].nunique()
```

Revenue per Customer.

```python
df["Revenue"].sum() / df["Customer ID"].nunique()
```

---

# 19. Automated Insight Generation

Generate quick summary statistics.

```python
summary = {

    "Rows": len(df),

    "Columns": len(df.columns),

    "Missing Values": df.isna().sum().sum(),

    "Duplicate Rows": df.duplicated().sum(),

    "Average Revenue": df["Revenue"].mean(),

    "Maximum Revenue": df["Revenue"].max(),

    "Unique Customers": df["Customer ID"].nunique()

}

report = pd.DataFrame(

    summary.items(),

    columns=["Metric", "Value"]

)
```

Example Output

| Metric | Value |
|---------|-------|
|Rows|50000|
|Columns|12|
|Missing Values|15|
|Duplicate Rows|2|
|Average Revenue|5250|
|Maximum Revenue|75000|
|Unique Customers|8400|

---

# 20. Business Example

A supermarket chain performs EDA before launching a marketing campaign.

Analysts discover:

- Weekend sales are 28% higher than weekday sales.
- Electronics generate the highest average revenue.
- The West region contributes the highest profit.
- A small number of customers account for a large share of revenue.
- Missing values occur mainly in optional customer profile fields.

These insights help management optimize promotions, staffing, and inventory.

---

# Best Practices

✔ Explore every variable before modeling.

✔ Compare categories using GroupBy.

✔ Use Pivot Tables for multidimensional summaries.

✔ Generate KPIs automatically.

✔ Document important business insights.

---

# Common Mistakes

### Ignoring Category-Level Analysis

Different categories often behave differently.

---

### Looking Only at Overall Totals

Analyze data by region, department, product, or customer segment.

---

### Ignoring Time Trends

Many business metrics vary across months, quarters, and weekdays.

---

# Quick Recap

You have now learned how to:

- Perform GroupBy analysis.
- Build Pivot Tables.
- Create Crosstabs.
- Perform multi-level aggregations.
- Analyze feature relationships.
- Conduct time-based EDA.
- Calculate business KPIs.
- Generate automated insight reports.

> **"EDA transforms raw numbers into business understanding. Every aggregation, comparison, and trend tells a story that supports better decisions."**

---

## Next (Day 46 – Final Part)

The final section will cover:

- Enterprise EDA Workflow
- Automated EDA Pipelines
- Production Best Practices
- Interview Questions (20+)
- Practice Exercises
- Cheat Sheet
- Mini Project
- Executive Business Insights
- Complete Day 46 Summary

# 21. Enterprise EDA Workflow

Professional organizations follow a structured Exploratory Data Analysis workflow before building dashboards or machine learning models.

```
Raw Dataset
      │
      ▼
Data Import
      │
      ▼
Data Cleaning
      │
      ▼
Data Validation
      │
      ▼
Missing Value Analysis
      │
      ▼
Statistical Profiling
      │
      ▼
Univariate Analysis
      │
      ▼
Bivariate Analysis
      │
      ▼
Multivariate Analysis
      │
      ▼
Business KPI Analysis
      │
      ▼
Business Insights
      │
      ▼
Reporting / Machine Learning
```

A structured workflow ensures consistent, reproducible, and reliable analysis.

---

# 22. Automated EDA Pipeline

Instead of repeating the same analysis for every dataset, create a reusable EDA function.

```python
def perform_eda(df):

    report = {

        "Rows": len(df),

        "Columns": len(df.columns),

        "Missing Values":
        df.isna().sum().sum(),

        "Duplicate Rows":
        df.duplicated().sum(),

        "Memory Usage (MB)":
        round(
            df.memory_usage(deep=True).sum()
            / 1024**2,
            2
        )

    }

    return pd.DataFrame(

        report.items(),

        columns=[
            "Metric",
            "Value"
        ]

    )
```

Run the pipeline.

```python
eda_report = perform_eda(df)

print(eda_report)
```

---

# 23. Production Best Practices

### Understand the Business Problem

EDA should answer business questions rather than simply producing statistics.

---

### Explore Every Variable

Review both:

- Numerical variables
- Categorical variables

Ignoring either may hide valuable insights.

---

### Investigate Outliers

Outliers may indicate:

- Data entry errors
- Fraud
- High-value customers
- Exceptional events

Never remove them without understanding the business context.

---

### Validate Assumptions

Always verify:

- Missing values
- Data types
- Duplicate records
- Invalid categories

before proceeding to modeling.

---

### Document Findings

Maintain a record of:

- Important trends
- Anomalies
- Data quality issues
- Business insights

Documentation improves collaboration.

---

# 24. Enterprise Case Study

## Scenario

You are a **Senior Data Analyst** at a retail company.

Management wants answers to:

- Which region generates the highest revenue?
- Which products perform best?
- Which customers spend the most?
- Are there seasonal sales patterns?
- Are there unusual transactions?

EDA Process:

### Dataset Overview

```python
df.info()
```

---

### Revenue by Region

```python
df.groupby("Region")["Revenue"].sum()
```

---

### Monthly Revenue

```python
df.groupby(

    df["Order Date"].dt.month

)["Revenue"].sum()
```

---

### Top Customers

```python
df.groupby(

    "Customer ID"

)["Revenue"]

.sum()

.nlargest(10)
```

---

### Detect Outliers

```python
Q1 = df["Revenue"].quantile(0.25)

Q3 = df["Revenue"].quantile(0.75)

IQR = Q3 - Q1

outliers = df[
    (df["Revenue"] < Q1 - 1.5 * IQR) |
    (df["Revenue"] > Q3 + 1.5 * IQR)
]
```

---

# 25. Business Insights

After performing EDA, the analysts discovered:

- West region generated the highest annual revenue.
- Electronics contributed the largest share of profit.
- Weekend sales consistently exceeded weekday sales.
- Approximately 8% of customers generated over 40% of total revenue.
- Most missing values occurred in optional customer profile fields.
- Seasonal demand increased significantly during festive months.
- A few extremely large transactions represented bulk corporate purchases rather than errors.

These insights guided inventory planning, marketing campaigns, and customer segmentation.

---

# 26. Practice Exercises

## Beginner

1. Display dataset information.
2. Calculate descriptive statistics.
3. Count missing values.
4. Count duplicate rows.
5. Calculate average sales.

---

## Intermediate

6. Analyze sales by region.
7. Create a Pivot Table.
8. Build a Crosstab.
9. Detect outliers using IQR.
10. Analyze monthly sales trends.

---

## Advanced

11. Build an automated EDA function.
12. Generate executive KPIs.
13. Analyze feature relationships.
14. Prepare a business insight report.
15. Design a complete EDA workflow.

---

# 27. Interview Questions

## Beginner

1. What is Exploratory Data Analysis?
2. Why is EDA important?
3. What does `describe()` do?
4. How do you detect missing values?
5. What are outliers?

---

## Intermediate

6. Explain GroupBy analysis.
7. What is a Pivot Table?
8. What is a Crosstab?
9. How do you analyze correlations?
10. Explain IQR outlier detection.

---

## Advanced

11. Design an enterprise EDA workflow.
12. Explain automated EDA pipelines.
13. How do you generate business insights from EDA?
14. How would you perform EDA on a dataset with millions of rows?
15. Compare univariate, bivariate, and multivariate analysis.

---

# 28. Cheat Sheet

| Task | Syntax |
|------|--------|
| First Rows | `head()` |
| Last Rows | `tail()` |
| Dataset Shape | `shape` |
| Data Types | `dtypes` |
| Dataset Info | `info()` |
| Statistics | `describe()` |
| Missing Values | `isna().sum()` |
| Duplicates | `duplicated().sum()` |
| Correlation | `corr()` |
| Group Analysis | `groupby()` |
| Pivot Table | `pivot_table()` |
| Crosstab | `pd.crosstab()` |
| Outlier Detection | `quantile()` |
| Frequency Counts | `value_counts()` |

---

# 29. Mini Project

## Enterprise Exploratory Data Analysis Dashboard

Using any retail, banking, healthcare, HR, finance, telecom, or logistics dataset:

Complete the following tasks:

- Explore dataset structure.
- Generate descriptive statistics.
- Analyze numerical variables.
- Analyze categorical variables.
- Detect missing values.
- Detect duplicates.
- Identify outliers.
- Perform correlation analysis.
- Build Pivot Tables and Crosstabs.
- Generate executive KPIs.
- Write **five executive-level business insights**.
- Recommend **three business actions** based on your findings.

### Example Business Insights

- West region consistently generated the highest revenue.
- Electronics products produced the highest average profit.
- Revenue peaked during festive seasons.
- A small percentage of customers generated a large portion of sales.
- Missing values were concentrated in optional customer information.

---

# 30. Summary

Congratulations! 🎉

Today you mastered **Advanced Exploratory Data Analysis (EDA) with Pandas**.

You learned how to:

- Explore datasets efficiently.
- Generate descriptive statistics.
- Analyze numerical and categorical variables.
- Detect missing values and duplicates.
- Identify outliers.
- Measure feature relationships.
- Perform GroupBy, Pivot Table, and Crosstab analysis.
- Generate business KPIs.
- Build automated EDA pipelines.
- Extract actionable business insights.

These skills are fundamental for Data Analytics, Business Intelligence, Data Science, and Machine Learning because every successful project begins with a deep understanding of the data.

---

# 31. What's Next?

## 🐼 Day 47 — Advanced Data Visualization with Pandas & Matplotlib

Topics include:

- Line Charts
- Bar Charts
- Histograms
- Box Plots
- Scatter Plots
- Pie Charts
- Area Charts
- Subplots
- Time Series Visualization
- Business Dashboard Visualizations
- Visualization Best Practices

Effective visualizations help communicate insights clearly and support better business decision-making.

---

# 🎉 Day 46 Complete!

You have successfully completed **Advanced Exploratory Data Analysis (EDA) with Pandas**.

You can now confidently explore unknown datasets, uncover meaningful patterns, generate executive-level insights, and prepare high-quality data for analytics and machine learning.

⭐ **Next → Day 47: Advanced Data Visualization with Pandas & Matplotlib** 📊🐼
