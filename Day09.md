# 🐼 Day 09 — Working with Dates & Time in Pandas

<div align="center">

# 100 Days of Pandas

### Day 09 · Mastering Date and Time Analysis

*"Time is one of the most valuable dimensions in data analysis. Understanding when something happened is often as important as knowing what happened."*

![Difficulty](https://img.shields.io/badge/Difficulty-Intermediate-yellow)
![Topic](https://img.shields.io/badge/Topic-DateTime%20Analysis-blue)
![Day](https://img.shields.io/badge/Day-09-orange)

</div>

---

# 📚 Table of Contents

1. Introduction
2. Why Date & Time Matter
3. Learning Objectives
4. Understanding DateTime Data
5. Converting Strings to Dates
6. Exploring Date Components
7. Date Formatting
8. Summary

---

# 1. Introduction

Dates and times appear in almost every real-world dataset.

Whether you're analyzing customer purchases, employee attendance, stock prices, website traffic, or hospital records, understanding **when** an event occurred is critical.

However, dates are often stored as plain text when data is imported from CSV or Excel files. Before performing any time-based analysis, these values must be converted into proper datetime objects.

Pandas provides powerful tools for parsing, formatting, filtering, and analyzing dates efficiently.

---

# 2. Why Date & Time Matter

Imagine you're working as a Data Analyst for an online retail company.

Management asks questions such as:

* Which month generated the highest sales?
* Which day receives the most orders?
* How many customers purchased during weekends?
* Are sales increasing year after year?
* Which quarter performed best?

These questions cannot be answered until the **Order Date** column is converted into a datetime format.

Date analysis transforms raw timestamps into meaningful business insights.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Convert strings into datetime objects.
* Extract years, months, days, and weekdays.
* Format datetime values.
* Filter records using dates.
* Perform basic date arithmetic.
* Build time-based business reports.

---

# 4. Understanding DateTime Data

Consider the following dataset:

| Order ID | Order Date |
| -------- | ---------- |
| 101      | 2025-01-15 |
| 102      | 2025-01-20 |
| 103      | 2025-02-08 |
| 104      | 2025-03-11 |

Although these values look like dates, Pandas may interpret them as strings (`object`) when reading a CSV file.

Check the data type:

```python
df.dtypes
```

Possible Output:

```text
Order ID        int64
Order Date     object
Sales         float64
```

Notice that **Order Date** is stored as an `object`.

To perform date calculations, we must convert it into the `datetime64[ns]` data type.

---

# 5. Converting Strings to Dates

Pandas provides the `pd.to_datetime()` function for converting date strings into datetime objects.

### Syntax

```python
pd.to_datetime(column)
```

Example:

```python
df["Order Date"] = pd.to_datetime(df["Order Date"])
```

Verify the conversion:

```python
df.dtypes
```

Output:

```text
Order Date    datetime64[ns]
```

Now Pandas recognizes the column as a datetime object and enables date-based operations.

---

# 6. Exploring Date Components

Once a column is converted into datetime format, Pandas provides the `.dt` accessor to retrieve individual components.

### Extract Year

```python
df["Year"] = df["Order Date"].dt.year
```

Output:

| Order Date | Year |
| ---------- | ---: |
| 2025-01-15 | 2025 |
| 2025-01-20 | 2025 |
| 2025-02-08 | 2025 |

---

### Extract Month

```python
df["Month"] = df["Order Date"].dt.month
```

Output:

| Order Date | Month |
| ---------- | ----: |
| 2025-01-15 |     1 |
| 2025-01-20 |     1 |
| 2025-02-08 |     2 |

---

### Extract Day

```python
df["Day"] = df["Order Date"].dt.day
```

---

### Extract Day Name

```python
df["Weekday"] = df["Order Date"].dt.day_name()
```

Example Output:

| Order Date | Weekday   |
| ---------- | --------- |
| 2025-01-15 | Wednesday |
| 2025-01-20 | Monday    |
| 2025-02-08 | Saturday  |

This is particularly useful for analyzing weekday versus weekend performance.

---

### Extract Month Name

```python
df["Month Name"] = df["Order Date"].dt.month_name()
```

Instead of displaying `1`, `2`, and `3`, the output becomes:

| Order Date | Month Name |
| ---------- | ---------- |
| 2025-01-15 | January    |
| 2025-02-08 | February   |
| 2025-03-11 | March      |

This format is easier to understand in reports and dashboards.

---

# 7. Formatting Dates

Sometimes you need to display dates in a specific format for reports or exports.

Use the `strftime()` method.

### Example

```python
df["Formatted Date"] = df["Order Date"].dt.strftime("%d-%m-%Y")
```

Output:

| Order Date | Formatted Date |
| ---------- | -------------- |
| 2025-01-15 | 15-01-2025     |
| 2025-02-08 | 08-02-2025     |

---

### Common Formatting Codes

| Code | Meaning            | Example   |
| ---- | ------------------ | --------- |
| `%Y` | Four-digit year    | 2025      |
| `%y` | Two-digit year     | 25        |
| `%m` | Month number       | 01        |
| `%B` | Full month name    | January   |
| `%b` | Short month name   | Jan       |
| `%d` | Day of month       | 15        |
| `%A` | Full weekday name  | Wednesday |
| `%a` | Short weekday name | Wed       |

---

# Key Takeaways

After completing this section, you should understand:

* Why datetime conversion is necessary.
* How to use `pd.to_datetime()`.
* How to extract years, months, days, and weekdays.
* How to format dates for reports.
* How datetime data supports time-based business analysis.

> **"Dates are more than timestamps—they reveal trends, seasonality, customer behavior, and the timing behind every business decision."**

