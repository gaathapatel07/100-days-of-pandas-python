# Day 09 — Working with Dates & Time in Pandas

<div align="center">

# 100 Days of Pandas

### Day 09 · Mastering Date and Time Analysis

*"Time is one of the most valuable dimensions in data analysis. Understanding when something happened is often as important as knowing what happened."*

![Difficulty](https://img.shields.io/badge/Difficulty-Intermediate-yellow)
![Topic](https://img.shields.io/badge/Topic-DateTime%20Analysis-blue)
![Day](https://img.shields.io/badge/Day-09-orange)

</div>

---

# Table of Contents

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

# 8. Filtering Data Using Dates

One of the biggest advantages of converting a column into a datetime object is the ability to filter records based on specific dates or date ranges.

Suppose you have the following dataset:

| Order ID | Order Date | Sales |
| -------- | ---------- | ----: |
| 101      | 2025-01-15 |  2500 |
| 102      | 2025-02-10 |  3200 |
| 103      | 2025-03-05 |  4100 |
| 104      | 2025-04-18 |  1800 |

### Filter Orders After a Specific Date

```python
df[df["Order Date"] > "2025-02-01"]
```

Output:

| Order ID | Order Date | Sales |
| -------- | ---------- | ----: |
| 102      | 2025-02-10 |  3200 |
| 103      | 2025-03-05 |  4100 |
| 104      | 2025-04-18 |  1800 |

---

### Filter Orders Within a Date Range

```python
df[
    (df["Order Date"] >= "2025-01-01") &
    (df["Order Date"] <= "2025-03-31")
]
```

This retrieves all orders placed during the first quarter of the year.

Date filtering is commonly used for:

* Monthly reports
* Quarterly analysis
* Financial statements
* Customer activity tracking

---

# 9. Current Date and Time

Sometimes you need to compare data with today's date.

Pandas provides the `Timestamp.now()` function.

```python
pd.Timestamp.now()
```

Example Output:

```text
2026-07-01 10:42:18
```

Store it in a variable:

```python
today = pd.Timestamp.now()
```

This is useful for calculating customer recency, subscription expiry, or delivery delays.

---

# 10. Date Arithmetic

Pandas allows arithmetic operations with dates.

Suppose you want to calculate an expected delivery date that is **7 days** after the order date.

```python
import pandas as pd

df["Expected Delivery"] = (
    df["Order Date"] +
    pd.Timedelta(days=7)
)
```

Output:

| Order Date | Expected Delivery |
| ---------- | ----------------- |
| 2025-01-15 | 2025-01-22        |
| 2025-02-10 | 2025-02-17        |

Date arithmetic is widely used in logistics, healthcare, banking, and project management.

---

### Subtracting Days

```python
df["Reminder Date"] = (
    df["Order Date"] -
    pd.Timedelta(days=3)
)
```

Useful for sending reminders before appointments or deliveries.

---

# 11. Calculating Time Differences

Another common requirement is determining the number of days between two dates.

Suppose your dataset contains:

* Order Date
* Delivery Date

```python
df["Delivery Time"] = (
    df["Delivery Date"] -
    df["Order Date"]
)
```

Example Output:

| Order Date | Delivery Date | Delivery Time |
| ---------- | ------------- | ------------- |
| 2025-01-15 | 2025-01-20    | 5 days        |
| 2025-02-10 | 2025-02-16    | 6 days        |

To extract only the number of days:

```python
df["Days"] = (
    df["Delivery Date"] -
    df["Order Date"]
).dt.days
```

Output:

| Order ID | Days |
| -------- | ---: |
| 101      |    5 |
| 102      |    6 |

---

# 12. Working with Weekends

Many businesses compare weekday and weekend performance.

Identify the day of the week:

```python
df["Weekday"] = (
    df["Order Date"]
    .dt.day_name()
)
```

Determine whether an order was placed on a weekend:

```python
df["Is Weekend"] = (
    df["Order Date"]
    .dt.dayofweek >= 5
)
```

Output:

| Order Date | Is Weekend |
| ---------- | ---------- |
| 2025-01-15 | False      |
| 2025-02-08 | True       |

Business applications include:

* Weekend sales analysis
* Customer traffic trends
* Staffing requirements
* Delivery planning

---

# 13. Sorting by Date

Chronological ordering is essential for trend analysis.

```python
df.sort_values("Order Date")
```

Latest orders first:

```python
df.sort_values(
    "Order Date",
    ascending=False
)
```

Sorting by date is often the first step before creating time-series visualizations.

---

# Business Scenario

Imagine you're working for an online marketplace.

Your manager asks:

* Show all orders placed during March.
* Calculate average delivery time.
* Identify weekend orders.
* Find customers who haven't purchased in the last 90 days.
* Determine monthly order volume.

All of these questions rely on datetime operations rather than simple numerical calculations.

---

# Best Practices

✔ Always convert date columns using `pd.to_datetime()` immediately after loading a dataset.

✔ Store dates using ISO format (`YYYY-MM-DD`) whenever possible.

✔ Sort datetime columns before trend analysis.

✔ Keep datetime values as datetime objects until the final reporting stage.

✔ Use descriptive column names such as `Order Date`, `Ship Date`, and `Invoice Date`.

---

# Common Mistakes

### Treating Dates as Strings

```python
df["Order Date"] > "01/02/2025"
```

If the column is stored as text, comparisons may produce incorrect results.

Always convert first:

```python
df["Order Date"] = pd.to_datetime(df["Order Date"])
```

---

### Ignoring Time Zones

When analyzing global datasets, timestamps from different countries may use different time zones.

Always verify whether timestamps are timezone-aware before comparing them.

---

# Quick Recap

You have now learned how to:

* Filter records using dates.
* Retrieve the current date and time.
* Add and subtract days.
* Calculate delivery times.
* Identify weekends.
* Sort datasets chronologically.
* Solve common business problems using datetime operations.

> **"Time-based analysis transforms isolated events into meaningful trends, helping businesses understand not just what happened, but when and why it happened."**

# 14. Real-World Business Case Study

## Scenario

You have recently joined **RetailHub**, an international e-commerce company, as a Data Analyst.

The operations team wants to evaluate order processing efficiency and customer purchasing patterns.

You receive a dataset containing:

* Order ID
* Customer ID
* Order Date
* Ship Date
* Delivery Date
* Region
* Product Category
* Sales

Your objective is to analyze the time-related aspects of the business and prepare a report for senior management.

---

## Business Questions

### Question 1

How many orders were placed in each month?

```python id="4a1phx"
df["Order Date"].dt.month_name().value_counts()
```

---

### Question 2

Which weekday receives the highest number of orders?

```python id="hnw4ek"
df["Order Date"].dt.day_name().value_counts()
```

---

### Question 3

Calculate the delivery time for every order.

```python id="trbq6g"
df["Delivery Days"] = (
    df["Delivery Date"] -
    df["Order Date"]
).dt.days
```

---

### Question 4

Display all orders placed after **1 July 2025**.

```python id="v3o8zn"
df[
    df["Order Date"] >
    "2025-07-01"
]
```

---

### Question 5

Identify weekend orders.

```python id="nxh8yf"
df[
    df["Order Date"]
    .dt.dayofweek >= 5
]
```

---

## Business Insights

After analyzing the data, you discover:

* December records the highest number of orders.
* Weekend purchases contribute nearly 35% of total sales.
* Average delivery time is **4.8 days**.
* Certain regions consistently experience longer shipping times.
* Monthly sales follow a clear seasonal trend.

These findings help optimize staffing, logistics, inventory planning, and promotional campaigns.

---

# 15. Practice Exercises

## Beginner

1. Convert a string column into datetime format.
2. Extract the year from every order date.
3. Extract the month name.
4. Extract the weekday.
5. Format dates as `DD-MM-YYYY`.

---

## Intermediate

6. Filter orders placed during January.
7. Filter orders placed after a specific date.
8. Calculate expected delivery dates.
9. Determine delivery duration.
10. Identify weekend transactions.

---

## Advanced

11. Find the busiest month.
12. Determine average delivery time.
13. Compare weekday versus weekend orders.
14. Identify customers inactive for more than 90 days.
15. Generate a monthly business summary.

---

# 16. Interview Questions

## Beginner

1. Why do we convert strings into datetime objects?
2. What does `pd.to_datetime()` do?
3. How do you extract the month from a datetime column?
4. Difference between `.dt.day` and `.dt.day_name()`?
5. How do you format dates?

---

## Intermediate

6. What is `Timedelta`?
7. How do you calculate the number of days between two dates?
8. How do you identify weekend records?
9. Why should dates be sorted before trend analysis?
10. Difference between date filtering and numerical filtering?

---

## Advanced

11. How does datetime analysis improve business reporting?
12. Explain seasonality with an example.
13. How would you identify inactive customers using dates?
14. Why are timezone considerations important?
15. Describe a complete workflow for analyzing order dates in an e-commerce dataset.

---

# 17. Cheat Sheet

| Operation           | Syntax                   |
| ------------------- | ------------------------ |
| Convert to datetime | `pd.to_datetime()`       |
| Extract year        | `.dt.year`               |
| Extract month       | `.dt.month`              |
| Month name          | `.dt.month_name()`       |
| Extract day         | `.dt.day`                |
| Weekday name        | `.dt.day_name()`         |
| Date formatting     | `.dt.strftime()`         |
| Current timestamp   | `pd.Timestamp.now()`     |
| Add days            | `+ pd.Timedelta(days=7)` |
| Subtract dates      | `date2 - date1`          |
| Difference in days  | `.dt.days`               |
| Sort dates          | `sort_values()`          |

---

# 18. Mini Project

## Order Timeline Analysis

Using any retail, e-commerce, or logistics dataset:

Perform the following tasks:

* Convert all date columns into datetime format.
* Extract year, month, and weekday.
* Identify weekend orders.
* Calculate delivery time.
* Determine monthly order counts.
* Find the busiest weekday.
* Detect seasonal sales trends.
* Write **five business insights** based on your findings.
* Export the final report as a CSV file.

---

# 19. Summary

Congratulations! 🎉

Today you mastered one of the most practical areas of Pandas: **working with dates and time**.

You learned how to:

* Convert text into datetime objects.
* Extract useful date components.
* Format dates for reporting.
* Filter datasets using dates.
* Perform date arithmetic.
* Calculate time differences.
* Analyze weekday and weekend patterns.
* Apply datetime analysis to real-world business scenarios.

Datetime operations are used extensively in finance, healthcare, logistics, retail, HR, marketing, and business intelligence. Mastering them allows analysts to uncover trends, monitor performance, and build powerful time-based reports.

---

# 20. What's Next?

In **Day 10**, you'll learn how to **combine multiple datasets**—a skill that mirrors SQL joins and is essential for working with real-world data.

Topics include:

* `merge()`
* `join()`
* `concat()`
* Inner Join
* Left Join
* Right Join
* Outer Join
* Merge on Multiple Columns
* Handling Duplicate Keys

These operations allow analysts to integrate information from different sources into a single, comprehensive dataset.

---

<div align="center">

# 🎉 Day 09 Complete!

You've successfully mastered **Date & Time Analysis in Pandas**, an essential skill for every data analyst.

From extracting date components to calculating delivery times and identifying seasonal patterns, you're now equipped to analyze the temporal dimension of business data.

⭐ Excellent progress!


</div>
