# 🐼 Day 05 — Exploratory Data Analysis (EDA): Understanding Your Data

<div align="center">

# 100 Days of Pandas

### Day 05 · Exploring Data Before Making Decisions

*"The goal of Exploratory Data Analysis is not to confirm what you think—it is to discover what the data is trying to tell you."*

![Difficulty](https://img.shields.io/badge/Difficulty-Intermediate-yellow)
![Topic](https://img.shields.io/badge/Topic-Exploratory%20Data%20Analysis-blue)
![Day](https://img.shields.io/badge/Day-05-orange)

</div>

---

# 📚 Table of Contents

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

