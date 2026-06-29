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

# 8. Replacing Text Using `str.replace()`

In real-world datasets, the same information is often represented in multiple ways.

For example:

| Department      |
| --------------- |
| HR              |
| Human Resources |
| Human Resource  |
| hr              |
| H.R.            |

Although these values refer to the same department, Pandas treats them as different categories.

The `str.replace()` method standardizes inconsistent text.

---

## Syntax

```python id="w7n2pa"
df["Column"].str.replace(
    "old",
    "new"
)
```

---

## Example

```python id="x4k8me"
df["Department"] = (
    df["Department"]
    .str.replace(
        "Human Resources",
        "HR"
    )
)
```

Output:

| Before          | After |
| --------------- | ----- |
| Human Resources | HR    |
| Human Resource  | HR    |
| HR              | HR    |

Standardizing values improves grouping, filtering, and reporting.

---

# 9. Searching Text with `str.contains()`

Analysts frequently need to search for rows containing specific words or patterns.

Example:

Find all customers whose email contains `"gmail"`.

```python id="u5t9rg"
df[
    df["Email"]
    .str.contains("gmail")
]
```

Output:

| Name  | Email                                     |
| ----- | ----------------------------------------- |
| Alice | [alice@gmail.com](mailto:alice@gmail.com) |
| Emma  | [emma@gmail.com](mailto:emma@gmail.com)   |

---

## Case-Insensitive Search

```python id="m6r4wy"
df[
    df["Email"]
    .str.contains(
        "gmail",
        case=False
    )
]
```

This matches:

```text id="0bzvqm"
gmail

Gmail

GMAIL
```

without requiring identical capitalization.

---

# 10. Finding the Beginning or End of Text

### Starts With

```python id="j9x1hl"
df["Email"].str.startswith("a")
```

Example Output:

| Email                                     | Result |
| ----------------------------------------- | ------ |
| [alice@gmail.com](mailto:alice@gmail.com) | True   |
| [rahul@yahoo.com](mailto:rahul@yahoo.com) | False  |

---

### Ends With

```python id="f4z8kc"
df["Email"].str.endswith(".com")
```

Useful for:

* Email validation
* File extensions
* Website URLs
* Product codes

---

# 11. Splitting Strings

Many datasets store multiple pieces of information within one column.

Example:

```text id="o8e3ps"
John Smith

Alice Brown

Emma Watson
```

Separate first and last names.

```python id="q3v7na"
df[
    ["First Name", "Last Name"]
] = (
    df["Customer Name"]
    .str.split(
        " ",
        expand=True
    )
)
```

Output:

| Customer Name | First Name | Last Name |
| ------------- | ---------- | --------- |
| John Smith    | John       | Smith     |
| Emma Watson   | Emma       | Watson    |

Splitting text is useful for names, addresses, and product codes.

---

# 12. Extracting Text Using Regular Expressions

Sometimes only a specific part of a string is needed.

Example:

Extract the numeric employee ID.

```text id="o0a2mr"
EMP-1023

EMP-2045

EMP-3088
```

```python id="e6k5zt"
df["Employee ID"] = (
    df["Employee Code"]
    .str.extract(
        r"(\d+)"
    )
)
```

Output:

| Employee Code | Employee ID |
| ------------- | ----------: |
| EMP-1023      |        1023 |
| EMP-2045      |        2045 |
| EMP-3088      |        3088 |

The regular expression `\d+` matches one or more digits.

---

# 13. Calculating String Length

Sometimes analysts need to measure text length.

Example:

```python id="s8h2qx"
df["Customer Name"].str.len()
```

Output:

| Customer Name | Length |
| ------------- | -----: |
| John Smith    |     10 |
| Alice Brown   |     11 |
| Emma Watson   |     12 |

Applications include:

* Password validation
* Product code verification
* Data quality checks

---

# 14. Chaining String Operations

Multiple cleaning operations can be combined into a single statement.

Example:

```python id="r4m9cj"
df["City"] = (
    df["City"]
    .str.strip()
    .str.lower()
    .str.replace(
        "-",
        " "
    )
    .str.title()
)
```

This performs:

1. Remove extra spaces.
2. Convert to lowercase.
3. Replace hyphens with spaces.
4. Convert to title case.

Instead of four separate lines, everything is completed in one readable pipeline.

---

# Business Example

Suppose a customer database contains:

```text id="m3u5fr"
 MUMBAI

mumbai

Mumbai

MUMBAI

mumbai-
```

After applying a cleaning pipeline:

```python id="v6y1kn"
(
    df["City"]
    .str.strip()
    .str.lower()
    .str.replace("-", "")
    .str.title()
)
```

The result becomes:

```text id="y1c7as"
Mumbai

Mumbai

Mumbai

Mumbai

Mumbai
```

This ensures accurate reports and eliminates duplicate categories caused by inconsistent formatting.

---

# Best Practices

✔ Clean text before performing `groupby()` or `value_counts()`.

✔ Standardize capitalization across the dataset.

✔ Remove extra spaces before merging DataFrames.

✔ Validate text using `startswith()`, `endswith()`, or `contains()`.

✔ Chain string operations to create clean, readable code.

---

# Common Mistakes

### Forgetting Missing Values

If a column contains `NaN`, some string operations may produce unexpected results.

Safely handle missing values first:

```python id="p8t6lm"
df["City"] = (
    df["City"]
    .fillna("")
    .str.strip()
)
```

---

### Applying String Methods to Numeric Columns

The `.str` accessor only works with string-like data.

Check the data type before applying string operations:

```python id="z2g4qb"
df.dtypes
```

Convert if necessary:

```python id="t1w7yd"
df["Code"] = (
    df["Code"]
    .astype(str)
)
```

---

# Quick Recap

You have now learned how to:

* Replace inconsistent values.
* Search text using `contains()`.
* Validate strings using `startswith()` and `endswith()`.
* Split strings into multiple columns.
* Extract information using regular expressions.
* Measure string length.
* Chain multiple cleaning operations together.

> **"Text cleaning is more than formatting—it transforms inconsistent, messy data into reliable information that supports accurate analysis and better business decisions."**
