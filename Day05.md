# Day 05 — Exploratory Data Analysis (EDA): Understanding Your Data

<div align="center">

# 100 Days of Pandas

### Day 05 · Exploring Data Before Making Decisions

*"The goal of Exploratory Data Analysis is not to confirm what you think—it is to discover what the data is trying to tell you."*

![Difficulty](https://img.shields.io/badge/Difficulty-Intermediate-yellow)
![Topic](https://img.shields.io/badge/Topic-Exploratory%20Data%20Analysis-blue)
![Day](https://img.shields.io/badge/Day-05-orange)

</div>

---


#  Table of Contents

1. Introduction to EDA
2. Why Exploratory Data Analysis Matters
3. Learning Objectives
4. The EDA Workflow
5. Descriptive Statistics
6. Understanding Data Distribution
7. Measuring Central Tendency
8. Summary

---

# 1. Introduction to Exploratory Data Analysis

Before creating dashboards, building machine learning models, or presenting reports, analysts must first understand the dataset they are working with. This process is known as **Exploratory Data Analysis (EDA)**.

EDA is the practice of examining a dataset to understand its structure, identify patterns, detect anomalies, discover relationships between variables, and uncover potential data quality issues.

Unlike formal statistical modeling, EDA focuses on asking questions, investigating trends, and developing intuition about the data before drawing conclusions.

Think of EDA as the "investigation phase" of a data analytics project.

---

# 2. Why Exploratory Data Analysis Matters

Imagine you receive a dataset containing one million customer transactions.

Without exploring the data, you don't know:

* Which products generate the most revenue.
* Whether the dataset contains extreme outliers.
* If customer ages are realistic.
* Whether sales increase during certain months.
* If missing values could affect your analysis.

Jumping directly into visualization or predictive modeling without first exploring the data can lead to misleading conclusions.

EDA helps analysts:

* Understand the overall structure of the dataset.
* Detect unusual patterns.
* Identify inconsistencies.
* Generate meaningful business questions.
* Decide which preprocessing steps are necessary.

In short, EDA transforms raw data into a story waiting to be understood.

---

# 3. Learning Objectives

By the end of Day 05, you will be able to:

* Explain the purpose of Exploratory Data Analysis.
* Generate descriptive statistics using Pandas.
* Understand measures of central tendency.
* Explore the distribution of numerical variables.
* Identify potential outliers and unusual observations.
* Develop meaningful business questions from raw datasets.

---

# 4. The EDA Workflow

A typical exploratory data analysis process follows these stages:

```text
Load Dataset
      │
      ▼
Inspect Dataset
      │
      ▼
Clean Data
      │
      ▼
Generate Statistics
      │
      ▼
Study Distributions
      │
      ▼
Find Relationships
      │
      ▼
Generate Business Insights
```

Each stage builds upon the previous one. Skipping any step increases the risk of drawing incorrect conclusions.

---

# 5. Descriptive Statistics

Descriptive statistics summarize the main characteristics of a dataset using numerical measures.

Instead of examining thousands of rows individually, descriptive statistics provide a concise overview.

Consider an employee salary dataset.

```python
import pandas as pd

df = pd.read_csv("employees.csv")
```

To generate descriptive statistics:

```python
df.describe()
```

Example Output:

| Statistic |  Salary |
| --------- | ------: |
| Count     |    1000 |
| Mean      |  62,500 |
| Std       |   9,820 |
| Min       |  32,000 |
| 25%       |  54,000 |
| 50%       |  61,500 |
| 75%       |  69,000 |
| Max       | 105,000 |

These values provide an immediate understanding of the dataset without manually inspecting every record.

---

# 6. Understanding Data Distribution

A distribution describes how values are spread across a dataset.

Understanding distributions helps answer questions such as:

* Are most employees paid similarly?
* Are there unusually high salaries?
* Is customer spending evenly distributed?
* Are sales concentrated within a small group of customers?

A well-understood distribution allows analysts to identify patterns that would otherwise remain hidden.

Some common distribution shapes include:

* **Normal Distribution** – Values cluster around the average.
* **Right-Skewed Distribution** – Most values are small, with a few extremely large observations.
* **Left-Skewed Distribution** – Most values are high, with a few unusually low observations.
* **Uniform Distribution** – Values are spread relatively evenly across the range.

Recognizing the distribution of your data helps determine which statistical methods and cleaning techniques are appropriate.

---

# 7. Measures of Central Tendency

Measures of central tendency describe the "typical" value within a dataset.

## Mean

The **mean** is the arithmetic average.

```python
df["Salary"].mean()
```

Use the mean when the data does not contain significant outliers.

---

## Median

The **median** is the middle value after sorting the data.

```python
df["Salary"].median()
```

The median is more robust when the dataset contains unusually high or low values.

---

## Mode

The **mode** is the value that appears most frequently.

```python
df["Department"].mode()
```

Mode is especially useful for categorical variables such as departments, product categories, or customer regions.

---

# Key Takeaways

After completing this section, you should be able to:

* Define Exploratory Data Analysis.
* Explain why EDA is essential before deeper analysis.
* Interpret the output of `describe()`.
* Differentiate between the mean, median, and mode.
* Understand why data distributions matter in analytics.

> **"Exploratory Data Analysis is not about finding answers immediately—it's about asking better questions and allowing the data to guide your investigation."**

---

# 8. Understanding Variability in Data

While measures like the **mean**, **median**, and **mode** describe the center of a dataset, they do not explain how the data is spread out.

Two datasets can have the same average but completely different distributions.

Consider the following examples:

### Dataset A

```text
20, 22, 24, 26, 28
```

### Dataset B

```text
5, 10, 24, 38, 43
```

Both datasets have a similar average, but Dataset B is much more spread out.

Understanding variability helps analysts identify consistency, risk, and unusual observations within a dataset.

---

# 9. Variance

Variance measures how far each observation is from the mean.

A **low variance** indicates that values are close to the average.

A **high variance** indicates that values are widely dispersed.

Calculate variance using Pandas:

```python
df["Salary"].var()
```

Example:

Suppose two departments have the same average salary of ₹60,000.

Department A:

```text
59000
60000
61000
60000
59500
```

Department B:

```text
30000
45000
60000
75000
90000
```

Although both departments have the same average salary, Department B has significantly higher variance.

This indicates greater inconsistency in salaries.

---

# 10. Standard Deviation

Standard deviation is the square root of variance.

Unlike variance, standard deviation is expressed in the same unit as the original data.

Calculate standard deviation:

```python
df["Salary"].std()
```

Interpretation:

* Low standard deviation → Data points are close together.
* High standard deviation → Data points are spread apart.

Example:

A retail company analyzing daily sales may observe:

| Store   | Average Sales | Standard Deviation |
| ------- | ------------: | -----------------: |
| Store A |       ₹50,000 |             ₹2,000 |
| Store B |       ₹50,000 |            ₹18,000 |

Although both stores have the same average sales, Store B experiences much larger fluctuations.

---

# 11. Understanding Quartiles

Quartiles divide a dataset into four equal parts.

They help analysts understand where observations are concentrated.

The four quartiles are:

* **Q1 (25th Percentile)** → First quarter of the data.
* **Q2 (50th Percentile)** → Median.
* **Q3 (75th Percentile)** → Third quarter of the data.
* **Q4** → Highest values.

Example:

```python
df["Salary"].describe()
```

Output:

| Statistic | Salary |
| --------- | -----: |
| 25%       | 48,000 |
| 50%       | 61,000 |
| 75%       | 73,000 |

Interpretation:

* 25% of employees earn less than ₹48,000.
* Half of employees earn less than ₹61,000.
* 75% of employees earn less than ₹73,000.

Quartiles provide more insight than simply knowing the average salary.

---

# 12. Percentiles

Percentiles divide data into one hundred equal parts.

For example:

* 90th percentile
* 95th percentile
* 99th percentile

These are widely used in competitive exams, business analytics, and finance.

Example:

```python
df["Salary"].quantile(0.90)
```

If the result is:

```text
₹92,000
```

It means:

**90% of employees earn ₹92,000 or less, while only 10% earn more.**

---

# 13. Detecting Outliers

Outliers are observations that differ significantly from the rest of the dataset.

Example:

```text
45000
47000
49000
51000
53000
1200000
```

The salary of **₹12,00,000** is clearly unusual.

Outliers can arise due to:

* Data entry mistakes
* Fraudulent transactions
* Exceptional cases
* Measurement errors

Outliers should always be investigated before analysis.

---

# 14. Using the IQR Method

One common way to detect outliers is the **Interquartile Range (IQR)**.

The IQR is calculated as:

```text
IQR = Q3 − Q1
```

Steps:

1. Calculate Q1
2. Calculate Q3
3. Compute IQR
4. Determine the lower and upper bounds

```text
Lower Bound = Q1 − 1.5 × IQR

Upper Bound = Q3 + 1.5 × IQR
```

Values outside these bounds are considered potential outliers.

Example in Pandas:

```python
Q1 = df["Salary"].quantile(0.25)
Q3 = df["Salary"].quantile(0.75)

IQR = Q3 - Q1

lower = Q1 - 1.5 * IQR
upper = Q3 + 1.5 * IQR

outliers = df[(df["Salary"] < lower) | (df["Salary"] > upper)]
```

This method is widely used because it is simple, effective, and resistant to extreme values.

---

# 15. Business Interpretation

Imagine you're analyzing salaries across different departments.

Most employees earn between **₹45,000** and **₹80,000**, but one employee appears to earn **₹12,00,000**.

Possible explanations include:

* Senior executive compensation
* Data entry error
* Annual salary recorded instead of monthly salary
* Incorrect currency conversion

Instead of deleting the value immediately, analysts investigate the reason behind the anomaly.

Understanding context is just as important as identifying statistical outliers.

---

# Key Takeaways

You have now learned how to:

* Measure variability using variance and standard deviation.
* Interpret quartiles and percentiles.
* Detect unusual observations.
* Identify outliers using the IQR method.
* Apply statistical reasoning to real-world business data.

> **"Statistics describe the data, but thoughtful interpretation transforms those statistics into meaningful business insights."**
