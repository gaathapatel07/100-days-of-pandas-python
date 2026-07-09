# 🐼 Day 29 — Advanced Missing Data Handling & Data Cleaning

<div align="center">

# 100 Days of Pandas

### Day 29 · Cleaning Data for Accurate Analysis

*"The quality of your analysis depends directly on the quality of your data."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Data%20Cleaning-blue)
![Day](https://img.shields.io/badge/Day-29-orange)

</div>

---

# 📚 Table of Contents

1. Introduction
2. Why Data Cleaning Matters
3. Learning Objectives
4. Understanding Missing Data
5. Detecting Missing Values
6. Counting Missing Values
7. Removing Missing Values
8. Summary

---

# 1. Introduction

Real-world datasets are rarely perfect.

Common problems include:

* Missing values
* Duplicate records
* Incorrect spellings
* Invalid dates
* Wrong data types
* Extra spaces
* Mixed formatting
* Outliers

Before performing analysis or building machine learning models, datasets must be cleaned.

Pandas provides a rich collection of tools for identifying, correcting, and removing data quality issues.

---

# 2. Why Data Cleaning Matters

Imagine an online retail company.

The sales dataset contains:

| Customer | Sales |
| -------- | ----: |
| Alice    |  5200 |
| Rahul    |   NaN |
| Priya    |  4800 |

If the missing value is ignored:

* Revenue calculations become inaccurate.
* Average sales decrease incorrectly.
* Dashboards become unreliable.
* Machine learning models may fail.

Cleaning the data ensures trustworthy business insights.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Detect missing values.
* Remove missing records.
* Fill missing values intelligently.
* Identify duplicate records.
* Build reliable data-cleaning pipelines.

---

# 4. Understanding Missing Data

Missing values are represented as:

```text id="missing01"
NaN
```

or

```text id="missing02"
None
```

or

```text id="missing03"
NaT
```

depending on the data type.

Example

| Customer | Sales |
| -------- | ----: |
| Alice    |  5200 |
| Rahul    |   NaN |
| Priya    |  4800 |

The missing sales value should be handled before analysis.

---

# 5. Detecting Missing Values

Use `isna()`.

```python id="missing04"
df.isna()
```

Output

| Customer | Sales |
| -------- | ----: |
| False    | False |
| False    |  True |
| False    | False |

`True` indicates a missing value.

---

## Detect Missing Values in One Column

```python id="missing05"
df["Sales"].isna()
```

---

## Detect Non-Missing Values

```python id="missing06"
df.notna()
```

Returns `True` wherever values are present.

---

# 6. Counting Missing Values

Count missing values in each column.

```python id="missing07"
df.isna().sum()
```

Output

| Column   | Missing |
| -------- | ------: |
| Customer |       0 |
| Sales    |       1 |
| Profit   |       3 |

---

## Total Missing Values

```python id="missing08"
df.isna().sum().sum()
```

Returns the total number of missing values in the DataFrame.

---

## Percentage of Missing Values

```python id="missing09"
(
    df.isna()
      .mean()
      * 100
)
```

Output

| Column   | Missing % |
| -------- | --------: |
| Customer |        0% |
| Sales    |        5% |
| Profit   |       12% |

This helps prioritize cleaning efforts.

---

# 7. Removing Missing Values

## Remove Rows with Missing Values

```python id="drop01"
clean_df = (
    df.dropna()
)
```

Every row containing at least one missing value is removed.

---

## Remove Columns with Missing Values

```python id="drop02"
df.dropna(
    axis=1
)
```

Columns containing missing values are removed.

---

## Remove Rows Only When Every Value Is Missing

```python id="drop03"
df.dropna(
    how="all"
)
```

---

## Remove Rows Based on Specific Columns

```python id="drop04"
df.dropna(
    subset=[
        "Sales",
        "Profit"
    ]
)
```

Rows are removed only if the specified columns contain missing values.

---

## Minimum Non-Missing Values Required

Keep rows with at least three valid values.

```python id="drop05"
df.dropna(
    thresh=3
)
```

---

# Business Example

A hospital maintains patient records.

Some rows contain:

* Missing patient age.
* Missing diagnosis.
* Missing admission dates.

Analysts:

* Identify missing values.
* Remove incomplete records where necessary.
* Preserve valuable records with only minor missing information.

This improves reporting quality while minimizing unnecessary data loss.

---

# Best Practices

✔ Measure missing values before cleaning.

✔ Understand why data is missing.

✔ Remove rows only when appropriate.

✔ Preserve important business records whenever possible.

✔ Document all cleaning decisions.

---

# Common Mistakes

### Removing Too Much Data

Using:

```python id="mistake01"
df.dropna()
```

may remove thousands of valuable records.

Always inspect the percentage of missing values first.

---

### Ignoring Missing Values

Many Pandas functions skip missing values automatically, but not every algorithm does.

Always assess missing data before analysis.

---

# Key Takeaways

After completing this section, you should understand:

* What missing values are.
* How to detect missing data.
* How to count missing values.
* How to remove missing values safely.
* Why thoughtful data cleaning is critical for reliable analysis.

> **"Cleaning data is not about removing imperfections—it is about ensuring that every business decision is based on trustworthy information."**

