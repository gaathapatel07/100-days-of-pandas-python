# 🐼 Day 46 — Advanced Exploratory Data Analysis (EDA) with Pandas

## 📖 Introduction

Exploratory Data Analysis (EDA) is the process of analyzing datasets to understand their structure, identify patterns, detect anomalies, discover relationships, and generate business insights before building statistical or machine learning models.

EDA is one of the most important phases of every Data Analytics and Data Science project because **better understanding of the data leads to better business decisions and better models.**

---

# 📚 Topics Covered

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

## Next (Day 46 – Part 2)

The next section covers:

- GroupBy Analysis
- Pivot Tables
- Crosstabs
- Multi-level Aggregations
- Feature Relationships
- Time-based EDA
- Automated Insight Generation
- Business KPI Analysis
