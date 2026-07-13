# 🐼 Day 37 — Advanced Exploratory Data Analysis (EDA) with Pandas

<div align="center">

# 100 Days of Pandas

### Day 37 · Discovering Insights Before Modeling

*"Exploratory Data Analysis is where data transforms from raw numbers into meaningful business understanding."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Exploratory%20Data%20Analysis-blue)
![Day](https://img.shields.io/badge/Day-37-orange)

</div>

---

# 📚 Table of Contents

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

