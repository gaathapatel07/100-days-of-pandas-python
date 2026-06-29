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

# 15. Real-World Business Case Study

## Scenario

You are working as a **Data Analyst** at **RetailHub**, an e-commerce company.

The marketing department wants to launch a customer segmentation campaign. Before analysis begins, you inspect the customer database and notice several data quality issues.

Examples include:

| Customer Name   | City      | Email             | Department        |
| --------------- | --------- | ----------------- | ----------------- |
| `john smith`    | `MUMBAI`  | `John@gmail.com`  | `Human Resources` |
| `ALICE JOHNSON` | `mumbai`  | `alice@GMAIL.COM` | `HR`              |
| `emma watson`   | `Mumbai-` | `emma@yahoo.com`  | `Human Resource`  |

These inconsistencies must be resolved before building reports or dashboards.

---

## Business Questions

### Question 1

Standardize customer names.

```python id="a2b7nq"
df["Customer Name"] = (
    df["Customer Name"]
    .str.strip()
    .str.title()
)
```

---

### Question 2

Standardize city names.

```python id="b9m4te"
df["City"] = (
    df["City"]
    .str.strip()
    .str.replace("-", "")
    .str.title()
)
```

---

### Question 3

Convert email addresses to lowercase.

```python id="c7x5fr"
df["Email"] = (
    df["Email"]
    .str.lower()
)
```

---

### Question 4

Standardize department names.

```python id="d8q2hy"
df["Department"] = (
    df["Department"]
    .replace({
        "Human Resources": "HR",
        "Human Resource": "HR",
        "H.R.": "HR",
        "hr": "HR"
    })
)
```

---

### Question 5

Identify Gmail users.

```python id="e3w6jk"
gmail_users = df[
    df["Email"]
    .str.contains(
        "gmail",
        case=False,
        na=False
    )
]
```

---

## Business Insights

After cleaning the dataset, you discover:

* Customer names now follow a consistent format.
* Duplicate city names caused by inconsistent capitalization have been eliminated.
* Department values have been standardized, improving reporting accuracy.
* Gmail is the most commonly used email provider.
* Overall data quality has improved significantly, making the dataset suitable for analysis.

---

# 16. Practice Exercises

## Beginner

1. Convert all names to title case.
2. Convert city names to lowercase.
3. Remove leading and trailing spaces.
4. Replace `"Human Resources"` with `"HR"`.
5. Find all email addresses ending with `.com`.

---

## Intermediate

6. Identify Gmail users.
7. Split customer names into first and last names.
8. Extract numeric values from employee codes.
9. Calculate the length of customer names.
10. Combine multiple string operations into a single pipeline.

---

## Advanced

11. Standardize three categorical columns.
12. Clean inconsistent product category names.
13. Validate email addresses using string methods.
14. Prepare a customer dataset for dashboard reporting.
15. Write five recommendations to improve future data collection quality.

---

# 17. Interview Questions

## Beginner

1. What is the purpose of the `.str` accessor?
2. Difference between `lower()` and `upper()`?
3. What does `strip()` do?
4. How does `replace()` work?
5. What is `contains()` used for?

---

## Intermediate

6. Difference between `startswith()` and `contains()`?
7. Why should text be standardized before grouping?
8. How do you split a string into multiple columns?
9. What is the purpose of `expand=True`?
10. How do you extract text using regular expressions?

---

## Advanced

11. Explain a real-world string cleaning workflow.
12. Why are inconsistent text values dangerous in business reporting?
13. How can regular expressions simplify text extraction?
14. Describe how you would clean a customer database before analysis.
15. How do chained string operations improve code readability?

---

# 18. Cheat Sheet

| Operation           | Syntax                    |
| ------------------- | ------------------------- |
| Lowercase           | `.str.lower()`            |
| Uppercase           | `.str.upper()`            |
| Title Case          | `.str.title()`            |
| Remove spaces       | `.str.strip()`            |
| Remove left spaces  | `.str.lstrip()`           |
| Remove right spaces | `.str.rstrip()`           |
| Replace text        | `.str.replace()`          |
| Search text         | `.str.contains()`         |
| Starts with         | `.str.startswith()`       |
| Ends with           | `.str.endswith()`         |
| Split text          | `.str.split(expand=True)` |
| Extract text        | `.str.extract()`          |
| String length       | `.str.len()`              |

---

# 19. Mini Project

## Customer Data Standardization

Using any customer dataset:

Perform the following tasks:

* Standardize customer names.
* Clean city names.
* Convert all email addresses to lowercase.
* Standardize department names.
* Remove extra spaces.
* Identify Gmail users.
* Split names into first and last names.
* Extract employee IDs from employee codes.
* Export the cleaned dataset.
* Write **five business insights** explaining how cleaning improved the quality of the data.

Example insights:

* Duplicate city names were reduced after standardization.
* Department names are now consistent across all records.
* Gmail accounts represent the majority of customer email addresses.
* Customer names now follow a uniform format, improving readability.
* The cleaned dataset is suitable for reporting, visualization, and predictive modeling.

---

# 20. Summary

Congratulations! 🎉

Today you learned how to clean and standardize textual data using Pandas.

You explored:

* Using the `.str` accessor.
* Standardizing text with `lower()`, `upper()`, and `title()`.
* Removing unwanted spaces.
* Replacing inconsistent values.
* Searching and validating text.
* Splitting and extracting string data.
* Building reusable text-cleaning pipelines.

These techniques are used extensively in customer analytics, HR systems, CRM platforms, finance, healthcare, and business intelligence.

---

# 21. What's Next?

In **Day 13**, you'll learn **Working with Missing Data & Advanced Data Quality Techniques**.

Topics include:

* Advanced `fillna()` strategies
* Forward Fill (`ffill`)
* Backward Fill (`bfill`)
* Interpolation
* Detecting invalid values
* Data validation
* Outlier handling
* Building robust data-cleaning pipelines

These techniques help transform imperfect datasets into reliable, analysis-ready data.

---

<div align="center">

# 🎉 Day 12 Complete!

You've mastered one of the most important parts of real-world data preparation: **cleaning and standardizing text data**.

The skills you've learned today are used daily by data analysts to prepare datasets for reporting, visualization, machine learning, and business intelligence.

⭐ Fantastic progress!

**Next → Day 13: Advanced Missing Data Handling & Data Quality** 🧹📊🐼

</div>
