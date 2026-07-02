# Day 23 — Advanced Date & Time Handling in Pandas

<div align="center">

# 100 Days of Pandas

### Day 23 · Mastering Date & Time Analysis

*"Time is one of the most valuable dimensions in data analysis. Understanding when something happened is often just as important as what happened."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Date%20%26%20Time-blue)
![Day](https://img.shields.io/badge/Day-23-orange)

</div>

---

# Table of Contents

1. Introduction
2. Why Date & Time Matter
3. Learning Objectives
4. Understanding DateTime Objects
5. Converting Strings to DateTime
6. Extracting Date Components
7. Creating Date Ranges
8. Summary

---

# 1. Introduction

Almost every real-world dataset contains date or time information.

Examples include:

* Order Dates
* Invoice Dates
* Login Times
* Customer Registration Dates
* Employee Joining Dates
* Sensor Timestamps
* Stock Market Transactions
* Flight Departure Times

Dates are not just values—they provide context, trends, seasonality, and business insights.

Pandas provides a comprehensive DateTime API that enables efficient manipulation, filtering, and analysis of temporal data.

---

# 2. Why Date & Time Matter

Imagine an e-commerce company.

The management team asks:

* Which month generated the highest revenue?
* How many orders were placed this week?
* What is the busiest day of the month?
* Which quarter performed best?
* How many customers registered during weekends?

Without proper DateTime handling, answering these questions becomes difficult.


---

# 3. Learning Objectives   

By the end of this lesson, you will be able to:

* Convert text into DateTime objects.
* Extract date components.
* Generate date ranges.
* Filter data using dates.
* Perform date-based business analysis.

---

# 4. Understanding DateTime Objects

Dates imported from CSV or Excel files are often treated as strings.

Example:

| Order Date |
| ---------- |
| 2026-01-15 |
| 2026-02-20 |
| 2026-03-10 |

Data type:

```python id="date01"
df.dtypes
```

Output:

```text id="date02"
Order Date    object
```

To perform calculations, convert the column into a DateTime object.

---

# 5. Converting Strings into DateTime

Use `pd.to_datetime()`.

```python id="date03"
df["Order Date"] = (
    pd.to_datetime(
        df["Order Date"]
    )
)
```

Now check the data type.

```python id="date04"
df.dtypes
```

Output:

```text id="date05"
Order Date    datetime64[ns]
```

The column now supports date arithmetic and filtering.

---

## Handling Different Date Formats

Dates are not always stored consistently.

Examples:

```text id="date06"
2026-07-15

15/07/2026

Jul 15, 2026

15-Jul-2026
```

Specify the format explicitly.

```python id="date07"
df["Order Date"] = (
    pd.to_datetime(
        df["Order Date"],
        format="%d/%m/%Y"
    )
)
```

Common format codes:

| Code | Meaning         | Example |
| ---- | --------------- | ------- |
| `%Y` | Four-digit year | 2026    |
| `%y` | Two-digit year  | 26      |
| `%m` | Month           | 07      |
| `%d` | Day             | 15      |
| `%H` | Hour (24-hour)  | 18      |
| `%M` | Minute          | 45      |
| `%S` | Second          | 30      |

---

## Handling Invalid Dates

Some datasets contain incorrect date values.

Example:

```text id="date08"
2026-02-30
```

Safely convert them.

```python id="date09"
df["Order Date"] = (
    pd.to_datetime(
        df["Order Date"],
        errors="coerce"
    )
)
```

Invalid dates become:

```text id="date10"
NaT
```

`NaT` stands for **Not a Time**.

---

# 6. Extracting Date Components

Once converted, individual components can be extracted using `.dt`.

---

## Year

```python id="dt01"
df["Year"] = (
    df["Order Date"]
      .dt.year
)
```

---

## Month

```python id="dt02"
df["Month"] = (
    df["Order Date"]
      .dt.month
)
```

---

## Month Name

```python id="dt03"
df["Month Name"] = (
    df["Order Date"]
      .dt.month_name()
)
```

Example:

| Date       | Month Name |
| ---------- | ---------- |
| 2026-01-15 | January    |
| 2026-07-08 | July       |

---

## Day

```python id="dt04"
df["Day"] = (
    df["Order Date"]
      .dt.day
)
```

---

## Day Name

```python id="dt05"
df["Weekday"] = (
    df["Order Date"]
      .dt.day_name()
)
```

Example:

| Date       | Weekday  |
| ---------- | -------- |
| 2026-07-13 | Monday   |
| 2026-07-18 | Saturday |

---

## Quarter

```python id="dt06"
df["Quarter"] = (
    df["Order Date"]
      .dt.quarter
)
```

Output:

| Date       | Quarter |
| ---------- | ------- |
| 2026-02-15 | 1       |
| 2026-05-10 | 2       |
| 2026-08-08 | 3       |

Quarterly analysis is widely used in finance and business reporting.

---

# 7. Creating Date Ranges

Generate continuous sequences of dates.

```python id="range01"
dates = pd.date_range(
    start="2026-01-01",
    end="2026-01-10"
)
```

Output:

```text id="range02"
2026-01-01

2026-01-02

2026-01-03

...
```

---

## Generate Monthly Dates

```python id="range03"
pd.date_range(
    start="2026-01-01",
    periods=12,
    freq="M"
)
```

Output:

```text id="range04"
2026-01-31

2026-02-28

2026-03-31
...
```

Common frequency codes:

| Code  | Meaning     |
| ----- | ----------- |
| `D`   | Daily       |
| `W`   | Weekly      |
| `M`   | Month End   |
| `MS`  | Month Start |
| `Q`   | Quarter End |
| `Y`   | Year End    |
| `H`   | Hourly      |
| `min` | Minute      |

---

# Business Example

A retail company stores every customer purchase with an order timestamp.

Using DateTime operations, analysts generate reports such as:

* Monthly revenue trends
* Quarterly performance
* Weekend sales
* Peak shopping periods
* Year-over-year growth

Without converting timestamps into proper DateTime objects, these analyses would be difficult and inefficient.

---

# Best Practices

✔ Convert date columns immediately after importing data.

✔ Handle invalid dates using `errors="coerce"`.

✔ Store timestamps in `datetime64[ns]` format.

✔ Extract only the components required for analysis.

✔ Validate date formats before merging datasets.

---

# Common Mistakes

### Keeping Dates as Strings

Incorrect:

```python id="mistake01"
df["Order Date"]
```

Data type:

```text id="mistake02"
object
```

Always convert using:

```python id="mistake03"
pd.to_datetime()
```

---

### Ignoring Invalid Dates

Always inspect converted dates.

```python id="mistake04"
df["Order Date"].isna().sum()
```

This helps identify records that failed conversion.

---

# Key Takeaways

After completing this section, you should understand:

* How DateTime objects differ from strings.
* How to convert text into dates.
* How to handle invalid dates safely.
* How to extract meaningful date components.
* How to generate date ranges for analysis.

> **"Dates are more than timestamps—they reveal trends, seasonality, customer behavior, and the rhythm of business operations."**

