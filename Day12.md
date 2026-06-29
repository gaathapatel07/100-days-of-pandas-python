# Day 12 — Advanced String Operations & Data Cleaning

<div align="center">

# 100 Days of Pandas

### Day 12 · Cleaning Messy Text Data Like a Data Analyst

*"Clean text leads to clean insights. Before analyzing data, ensure that your text is accurate, consistent, and standardized."*

![Difficulty](https://img.shields.io/badge/Difficulty-Intermediate-yellow)
![Topic](https://img.shields.io/badge/Topic-String%20Operations%20%26%20Data%20Cleaning-blue)
![Day](https://img.shields.io/badge/Day-12-orange)

</div>

---

# Table of Contents

1. Introduction
2. Why String Cleaning Matters
3. Learning Objectives
4. Understanding String Data
5. The `.str` Accessor
6. Changing Letter Case
7. Removing Unwanted Spaces
8. Summary

---

# 1. Introduction

In real-world datasets, textual data is rarely clean.

A customer database may contain names written in different cases, cities with extra spaces, inconsistent abbreviations, or accidental typing mistakes.

For example, all of the following may refer to the same city:

```text id="c1eg8h"
Mumbai
mumbai
 MUMBAI
Mumbai
MUMBAI
```

Although these values represent the same location, Pandas treats them as different strings.

If left uncleaned, they can lead to inaccurate counts, duplicate categories, and misleading reports.

This is why text cleaning is one of the most important stages of data preparation.

---

# 2. Why String Cleaning Matters

Imagine you're analyzing customer registrations.

The **City** column contains:

| Customer | City   |
| -------- | ------ |
| Alice    | Mumbai |
| Rahul    | mumbai |
| Emma     | Mumbai |
| David    | MUMBAI |
| Sarah    | Mumbai |

If you generate a frequency table immediately:

```python id="4pkbxb"
df["City"].value_counts()
```

You might see:

| City   | Count |
| ------ | ----: |
| Mumbai |     2 |
| mumbai |     1 |
| MUMBAI |     1 |
| Mumbai |     1 |

The analysis incorrectly suggests multiple cities.

After standardization:

| City   | Count |
| ------ | ----: |
| Mumbai |     5 |

Cleaning text ensures that analytical results accurately reflect the underlying data.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Understand the importance of string cleaning.
* Use the `.str` accessor.
* Convert text to lowercase, uppercase, and title case.
* Remove leading and trailing spaces.
* Standardize textual data before analysis.

---

# 4. Understanding String Data

In Pandas, textual information is typically stored as the **object** or **string** data type.

Examples include:

* Customer Names
* Product Categories
* Departments
* Cities
* Countries
* Email Addresses
* Phone Numbers

To perform string operations, Pandas provides the powerful `.str` accessor.

---

# 5. The `.str` Accessor

The `.str` accessor allows vectorized string operations on an entire column.

Instead of looping through every row, Pandas applies the operation efficiently to all values.

Example:

```python id="p1f7kt"
df["City"].str.lower()
```

This converts every city name to lowercase.

The `.str` accessor includes dozens of useful methods for cleaning and transforming text data.

---

# 6. Changing Letter Case

Consistent capitalization improves readability and prevents duplicate categories.

### Convert to Lowercase

```python id="o7g2wd"
df["City"] = df["City"].str.lower()
```

Example:

| Before | After  |
| ------ | ------ |
| Mumbai | mumbai |
| MUMBAI | mumbai |
| Mumbai | mumbai |

---

### Convert to Uppercase

```python id="g2d9mh"
df["Department"] = df["Department"].str.upper()
```

Output:

| Before  | After   |
| ------- | ------- |
| Sales   | SALES   |
| Hr      | HR      |
| Finance | FINANCE |

---

### Convert to Title Case

Title case capitalizes the first letter of every word.

```python id="j5q1xe"
df["Customer Name"] = (
    df["Customer Name"]
    .str.title()
)
```

Example:

| Before        | After         |
| ------------- | ------------- |
| john smith    | John Smith    |
| ALICE JOHNSON | Alice Johnson |
| emma watson   | Emma Watson   |

Title case is commonly used for names, cities, and addresses.

---

# 7. Removing Unwanted Spaces

Extra spaces are among the most common data quality issues.

Consider:

```text id="4o8fvn"
" Mumbai"

"Mumbai "

" Mumbai "
```

Although these appear identical, they are different strings.

Use `strip()` to remove spaces from both ends.

```python id="k3r5lm"
df["City"] = (
    df["City"]
    .str.strip()
)
```

---

### Remove Left Spaces

```python id="b7t4ep"
df["City"] = (
    df["City"]
    .str.lstrip()
)
```

---

### Remove Right Spaces

```python id="w2x8ad"
df["City"] = (
    df["City"]
    .str.rstrip()
)
```

Cleaning spaces is especially important before:

* Merging datasets
* Creating Pivot Tables
* Running `value_counts()`
* Performing GroupBy operations

---

# Key Takeaways

After completing this section, you should understand:

* Why inconsistent text causes inaccurate analysis.
* The purpose of the `.str` accessor.
* How to standardize capitalization.
* How to remove unwanted spaces.
* Why text cleaning is an essential part of data preparation.

> **"Small inconsistencies in text can lead to large inconsistencies in analysis. Clean your strings before trusting your results."**

