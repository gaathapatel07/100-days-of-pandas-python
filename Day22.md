# Day 22 — Advanced String Operations & Regular Expressions (Regex)

<div align="center">

# 100 Days of Pandas

### Day 22 · Mastering Text Processing & Pattern Matching

*"Structured data tells you what happened. Text data tells you why it happened."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-String%20Operations%20%26%20Regex-blue)
![Day](https://img.shields.io/badge/Day-22-orange)

</div>

---


# Table of Contents

1. Introduction
2. Why String Processing Matters
3. Learning Objectives
4. The `.str` Accessor
5. Changing Text Case
6. Removing Unwanted Characters
7. Searching Text
8. Summary

---

# 1. Introduction

Text data appears in almost every business dataset.

Examples include:

* Customer names
* Email addresses
* Phone numbers
* Product descriptions
* Employee IDs
* Website URLs
* Addresses
* Customer feedback

Unlike numerical data, text often contains inconsistencies such as:

* Extra spaces
* Mixed capitalization
* Typographical errors 
* Missing values
* Unwanted symbols

Before meaningful analysis can begin, text must be cleaned and standardized.


Pandas provides the powerful `.str` accessor, allowing analysts to perform vectorized string operations efficiently.

---

# 2. Why String Processing Matters

Imagine an online retailer stores customer cities like this:

| Customer | City  |
| -------- | ----- |
| Alice    | Delhi |
| Rahul    | delhi |
| Priya    | DELHI |
| Arjun    | Delhi |

Although these values represent the same city, Pandas treats them as different categories.

Without cleaning:

* GroupBy produces incorrect counts.
* Dashboards display duplicate regions.
* Machine learning models learn inconsistent categories.

Standardizing text ensures reliable analysis.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Perform vectorized string operations.
* Standardize text formatting.
* Search for text patterns.
* Replace unwanted characters.
* Prepare textual data for analysis.

---

# 4. The `.str` Accessor

The `.str` accessor applies string methods to every value in a Series.

Example:

```python id="str01"
df["Name"].str.lower()
```

Instead of writing loops, Pandas processes the entire column efficiently.

Common `.str` operations include:

* `lower()`
* `upper()`
* `title()`
* `strip()`
* `replace()`
* `contains()`
* `startswith()`
* `endswith()`
* `split()`

---

# 5. Changing Text Case

## Convert to Lowercase

```python id="case01"
df["City"] = (
    df["City"]
      .str.lower()
)
```

Example:

| Original | Result |
| -------- | ------ |
| Delhi    | delhi  |
| MUMBAI   | mumbai |
| Pune     | pune   |

---

## Convert to Uppercase

```python id="case02"
df["City"] = (
    df["City"]
      .str.upper()
)
```

Output:

| Original | Result |
| -------- | ------ |
| Delhi    | DELHI  |
| Pune     | PUNE   |

---

## Convert to Title Case

```python id="case03"
df["City"] = (
    df["City"]
      .str.title()
)
```

Output:

| Original  | Result    |
| --------- | --------- |
| delhi     | Delhi     |
| MUMBAI    | Mumbai    |
| bangalore | Bangalore |

Title case is commonly used in business reports.

---

# 6. Removing Unwanted Characters

Real-world datasets often contain unnecessary spaces.

Example:

| Customer  |
| --------- |
| " Alice " |
| " Rahul"  |
| "Priya "  |

Remove extra spaces.

```python id="strip01"
df["Customer"] = (
    df["Customer"]
      .str.strip()
)
```

---

## Remove Left Spaces

```python id="strip02"
df["Customer"] = (
    df["Customer"]
      .str.lstrip()
)
```

---

## Remove Right Spaces

```python id="strip03"
df["Customer"] = (
    df["Customer"]
      .str.rstrip()
)
```

These methods improve consistency before grouping or merging datasets.

---

# 7. Searching Text

## Check Whether Text Contains a Word

Suppose we want all email addresses from Gmail.

```python id="contains01"
df[
    df["Email"]
      .str.contains(
          "gmail"
      )
]
```

This returns only rows containing the specified text.

---

## Case-Insensitive Search

```python id="contains02"
df[
    df["Email"]
      .str.contains(
          "gmail",
          case=False
      )
]
```

This matches:

* Gmail
* gmail
* GMAIL

without requiring identical capitalization.

---

## Finding Missing Text Safely

Some text columns contain missing values.

Prevent errors by using:

```python id="contains03"
df[
    df["Email"]
      .str.contains(
          "gmail",
          na=False
      )
]
```

Rows with missing values are treated as `False`.

---

# Business Example

A marketing company stores customer email addresses collected from multiple campaigns.

Problems include:

* Mixed capitalization
* Leading and trailing spaces
* Different email domains
* Inconsistent customer names

Using Pandas string methods, analysts standardize the data before segmentation and campaign analysis.

---

# Best Practices

✔ Standardize text before grouping.

✔ Remove unnecessary spaces.

✔ Use lowercase for comparisons.

✔ Preserve original data when possible.

✔ Handle missing values before applying string methods.

---

# Common Mistakes

### Forgetting Missing Values

Incorrect:

```python id="mistake01"
df["Email"].str.contains("gmail")
```

If missing values exist, unexpected results may occur.

Prefer:

```python id="mistake02"
df["Email"].str.contains(
    "gmail",
    na=False
)
```

---

### Mixing Different Capitalizations

Before grouping:

```text
Delhi
delhi
DELHI
```

After standardization:

```text
Delhi
Delhi
Delhi
```

Consistent text improves reporting accuracy.

---

# Key Takeaways

After completing this section, you should understand:

* The purpose of the `.str` accessor.
* How to standardize capitalization.
* How to remove unwanted spaces.
* How to search text efficiently.
* Why text cleaning improves data quality.

> **"Consistent text transforms fragmented information into meaningful categories, enabling accurate analysis and reliable business insights."**

# 8. Checking the Beginning of a String

Sometimes you need to filter records that start with a specific pattern.

Examples:

* Employee IDs
* Product Codes
* Invoice Numbers
* Customer IDs

---

## Using `startswith()`

Suppose employee IDs begin with **EMP**.

```python id="start01"
df[
    df["Employee ID"]
      .str.startswith("EMP")
]
```

### Example

| Employee ID |
| ----------- |
| EMP001      |
| EMP002      |
| MGR101      |

Output:

| Employee ID |
| ----------- |
| EMP001      |
| EMP002      |

---

## Case-Insensitive Search

```python id="start02"
df[
    df["Employee ID"]
      .str.lower()
      .str.startswith("emp")
]
```

This ensures consistent matching regardless of capitalization.

---

# 9. Checking the End of a String

Use `endswith()` to identify values ending with a specific pattern.

Example:

```python id="end01"
df[
    df["Email"]
      .str.endswith(".com")
]
```

Output:

| Email                                     |
| ----------------------------------------- |
| [alice@gmail.com](mailto:alice@gmail.com) |
| [john@yahoo.com](mailto:john@yahoo.com)   |

Useful for:

* File extensions
* Email domains
* URLs
* Product SKUs

---

# 10. Replacing Text

Replace incorrect or outdated values.

Suppose the dataset contains:

| City      |
| --------- |
| Bombay    |
| Bangalore |
| Madras    |

Update them.

```python id="replace01"
df["City"] = (
    df["City"]
      .str.replace(
          "Bombay",
          "Mumbai"
      )
)
```

Replace multiple values.

```python id="replace02"
df["City"] = (
    df["City"]
      .replace({
          "Bombay":"Mumbai",
          "Madras":"Chennai",
          "Calcutta":"Kolkata"
      })
)
```

---

# 11. Splitting Strings

Many datasets combine multiple values in one column.

Example:

| Name        |
| ----------- |
| Alice Smith |
| Rahul Patel |

Split the names.

```python id="split01"
df["Name"].str.split()
```

Output:

```text id="splittext01"
["Alice","Smith"]

["Rahul","Patel"]
```

---

## Splitting into Multiple Columns

```python id="split02"
df[
    ["First Name","Last Name"]
] = (
    df["Name"]
      .str.split(
          " ",
          expand=True
      )
)
```

Output:

| First Name | Last Name |
| ---------- | --------- |
| Alice      | Smith     |
| Rahul      | Patel     |

---

# 12. Joining Strings

Combine multiple columns.

```python id="join01"
df["Full Name"] = (
    df["First Name"]
    + " "
    + df["Last Name"]
)
```

Output:

| Full Name   |
| ----------- |
| Alice Smith |
| Rahul Patel |

---

# 13. Extracting Text

Sometimes only part of a string is needed.

Suppose employee IDs look like:

```text id="extract01"
EMP001

EMP245

EMP999
```

Extract only the numeric portion.

```python id="extract02"
df["Employee ID"].str.extract(
    r"(\d+)"
)
```

Output:

| Employee Number |
| --------------: |
|               1 |
|             245 |
|             999 |

---

# 14. Calculating String Length

Count the number of characters.

```python id="length01"
df["Name Length"] = (
    df["Customer"]
      .str.len()
)
```

Output:

| Customer    | Length |
| ----------- | -----: |
| Alice       |      5 |
| Rahul       |      5 |
| Christopher |     11 |

Applications include:

* Password validation
* Product code verification
* Customer name analysis

---

# 15. Counting Words

Determine the number of words in customer feedback.

```python id="words01"
df["Word Count"] = (
    df["Feedback"]
      .str.split()
      .str.len()
)
```

Output:

| Feedback          | Word Count |
| ----------------- | ---------: |
| Very good service |          3 |
| Excellent support |          2 |

Useful for:

* NLP preprocessing
* Review analysis
* Survey responses

---

# 16. Chaining Multiple String Operations

Complex cleaning tasks often combine several methods.

Example:

```python id="chain01"
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

Input:

```text id="chain02"
 DELHI-

mumbai

BANGALORE
```

Output:

```text id="chain03"
Delhi

Mumbai

Bangalore
```

Method chaining produces concise, readable code.

---

# Business Example

A multinational retailer stores customer names collected from online forms.

Issues include:

* Mixed capitalization
* Extra spaces
* Combined first and last names
* Inconsistent city names
* Invalid product codes

String operations standardize the dataset before customer segmentation and reporting.

---

# Best Practices

✔ Use vectorized string methods instead of loops.

✔ Apply `strip()` before changing capitalization.

✔ Use `expand=True` when splitting into multiple columns.

✔ Standardize values before grouping or merging.

✔ Chain operations for cleaner code.

---

# Common Mistakes

### Forgetting `expand=True`

Incorrect:

```python id="mistake03"
df["Name"].str.split(" ")
```

This returns a list.

Correct:

```python id="mistake04"
df["Name"].str.split(
    " ",
    expand=True
)
```

This creates separate DataFrame columns.

---

### Applying String Methods to Numeric Columns

Always verify the data type.

```python id="mistake05"
df["Sales"].dtype
```

Convert to string only when necessary.

```python id="mistake06"
df["Sales"] = (
    df["Sales"]
      .astype(str)
)
```

---

# Quick Recap

You have now learned how to:

* Search using `startswith()` and `endswith()`.
* Replace text values.
* Split strings into multiple columns.
* Join text columns.
* Extract portions of strings.
* Count characters and words.
* Chain multiple string operations efficiently.

> **"Well-structured text data transforms inconsistent records into reliable information, enabling accurate reporting, customer segmentation, and advanced analytics."**
