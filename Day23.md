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

# 8. Filtering Data by Date

One of the most common business tasks is filtering records based on dates.

Suppose we want all orders placed after **1st July 2026**.

```python id="filter01"
df[
    df["Order Date"] >
    "2026-07-01"
]
```

---

## Filter Between Two Dates

Retrieve orders placed during January 2026.

```python id="filter02"
df[
    (
        df["Order Date"] >=
        "2026-01-01"
    )
    &
    (
        df["Order Date"] <=
        "2026-01-31"
    )
]
```

---

## Using `.loc`

```python id="filter03"
df.loc[
    df["Order Date"] >=
    "2026-06-01"
]
```

Filtering by date is essential for monthly, quarterly, and yearly business reporting.

---

# 9. Date Arithmetic

DateTime objects support arithmetic operations.

Suppose shipping takes seven days.

```python id="arith01"
df["Delivery Date"] = (
    df["Order Date"] +
    pd.Timedelta(days=7)
)
```

Output:

| Order Date | Delivery Date |
| ---------- | ------------- |
| 2026-07-01 | 2026-07-08    |
| 2026-07-05 | 2026-07-12    |

---

## Subtract Dates

Calculate delivery duration.

```python id="arith02"
df["Delivery Time"] = (
    df["Delivery Date"]
    -
    df["Order Date"]
)
```

Output:

```text id="arith03"
7 days

5 days

10 days
```

---

# 10. Understanding `Timedelta`

A `Timedelta` represents the difference between two dates or times.

Example:

```python id="timedelta01"
pd.Timedelta(days=5)
```

Output:

```text id="timedelta02"
5 days
```

---

## Add Hours

```python id="timedelta03"
df["Timestamp"] = (
    df["Timestamp"]
    +
    pd.Timedelta(hours=3)
)
```

---

## Add Minutes

```python id="timedelta04"
df["Timestamp"] = (
    df["Timestamp"]
    +
    pd.Timedelta(minutes=45)
)
```

`Timedelta` supports:

* Days
* Hours
* Minutes
* Seconds
* Weeks

---

# 11. Working with Business Days

Businesses often ignore weekends when calculating deadlines.

Generate business days.

```python id="business01"
pd.bdate_range(
    start="2026-07-01",
    end="2026-07-10"
)
```

Output:

```text id="business02"
2026-07-01

2026-07-02

2026-07-03

2026-07-06

2026-07-07

...
```

Notice that Saturday and Sunday are excluded.

---

## Add Business Days

```python id="business03"
from pandas.tseries.offsets import BDay

df["Deadline"] = (
    df["Order Date"]
    +
    BDay(5)
)
```

Five business days are added while skipping weekends.

---

# 12. Working with Time Zones

Global companies often receive timestamps from multiple countries.

Suppose timestamps are stored without a timezone.

```python id="tz01"
df["Timestamp"] = (
    df["Timestamp"]
      .dt.tz_localize("UTC")
)
```

---

## Convert to Another Time Zone

```python id="tz02"
df["Timestamp"] = (
    df["Timestamp"]
      .dt.tz_convert(
          "Asia/Kolkata"
      )
)
```

Common time zones:

| Time Zone        | Region         |
| ---------------- | -------------- |
| UTC              | Universal Time |
| Asia/Kolkata     | India          |
| Europe/London    | United Kingdom |
| America/New_York | Eastern US     |
| Asia/Tokyo       | Japan          |

---

# 13. Shifting Dates

Sometimes previous or future values are required.

Shift by one day.

```python id="shift01"
df["Previous Sales"] = (
    df["Sales"]
      .shift(1)
)
```

Example:

| Date  | Sales | Previous Sales |
| ----- | ----: | -------------: |
| 1 Jul |   100 |            NaN |
| 2 Jul |   120 |            100 |
| 3 Jul |   150 |            120 |

Shifting is widely used in time-series feature engineering.

---

# 14. Rolling Windows

Rolling windows calculate statistics over a moving period.

Calculate a 3-day moving average.

```python id="rolling01"
df["Rolling Average"] = (
    df["Sales"]
      .rolling(3)
      .mean()
)
```

Example:

| Day | Sales | Rolling Avg |
| --: | ----: | ----------: |
|   1 |   100 |         NaN |
|   2 |   120 |         NaN |
|   3 |   140 |         120 |
|   4 |   160 |         140 |

Rolling calculations smooth fluctuations and reveal trends.

---

# 15. Resampling Time-Series Data

Suppose sales are recorded daily.

Convert them into monthly totals.

```python id="resample01"
df.resample(
    "M",
    on="Order Date"
)["Sales"].sum()
```

Frequency codes:

| Code | Meaning   |
| ---- | --------- |
| D    | Daily     |
| W    | Weekly    |
| M    | Monthly   |
| Q    | Quarterly |
| Y    | Yearly    |

Resampling is frequently used for business reporting and forecasting.

---

# Business Example

A logistics company tracks package deliveries.

Using DateTime operations, analysts:

* Calculate delivery durations.
* Skip weekends when estimating deadlines.
* Convert timestamps from UTC to local office time.
* Generate monthly shipment summaries.
* Compute moving averages to monitor operational performance.

These insights improve planning and customer satisfaction.

---

# Best Practices

✔ Store all timestamps in DateTime format.

✔ Use `Timedelta` for date calculations.

✔ Use business-day offsets for operational deadlines.

✔ Standardize timestamps to UTC before converting to local time.

✔ Use rolling calculations for trend analysis.

---

# Common Mistakes

### Performing Arithmetic on Strings

Incorrect:

```python id="mistake05"
df["Order Date"] + 5
```

Always convert to DateTime first.

---

### Ignoring Time Zones

When analyzing global datasets, timestamps from different regions should be standardized before comparison.

---

### Using Calendar Days Instead of Business Days

For business processes such as shipping or banking, use `BDay()` instead of adding calendar days.

---

# Quick Recap

You have now learned how to:

* Filter data using dates.
* Perform date arithmetic.
* Use `Timedelta`.
* Generate business days.
* Work with time zones.
* Shift time-series data.
* Calculate rolling statistics.
* Resample time-series datasets.

> **"Time-series analysis transforms raw timestamps into meaningful business insights, helping organizations understand trends, seasonality, operational efficiency, and customer behavior."**

# 16. Real-World Business Case Study

## Scenario

You are working as a **Senior Business Data Analyst** at **RetailHub**, a multinational e-commerce company.

Every transaction includes:

* Order Date
* Delivery Date
* Customer Registration Date
* Payment Date
* Region
* Sales
* Profit

Management wants to answer several business questions:

* Which month generated the highest revenue?
* Which quarter was most profitable?
* What is the average delivery time?
* How many orders were placed on weekends?
* Which days experience the highest sales volume?

Your task is to prepare and analyze the DateTime data using Pandas.

---

# Business Questions

### Question 1

Extract the month from each order.

```python id="case_date01"
df["Month"] = (
    df["Order Date"]
      .dt.month_name()
)
```

---

### Question 2

Calculate delivery duration.

```python id="case_date02"
df["Delivery Days"] = (
    df["Delivery Date"]
    -
    df["Order Date"]
).dt.days
```

---

### Question 3

Find orders placed during weekends.

```python id="case_date03"
weekend_orders = df[
    df["Order Date"]
      .dt.dayofweek >= 5
]
```

`dayofweek`

| Value | Day       |
| ----: | --------- |
|     0 | Monday    |
|     1 | Tuesday   |
|     2 | Wednesday |
|     3 | Thursday  |
|     4 | Friday    |
|     5 | Saturday  |
|     6 | Sunday    |

---

### Question 4

Calculate monthly revenue.

```python id="case_date04"
monthly_sales = (
    df.groupby(
        df["Order Date"]
          .dt.month_name()
    )["Sales"]
    .sum()
)
```

---

### Question 5

Generate quarterly profit.

```python id="case_date05"
quarterly_profit = (
    df.groupby(
        df["Order Date"]
          .dt.quarter
    )["Profit"]
    .sum()
)
```

---

# 17. Building Business KPIs

Date columns help organizations generate important performance indicators.

Examples:

* Daily Revenue
* Weekly Orders
* Monthly Growth
* Quarterly Profit
* Year-over-Year Sales
* Customer Retention
* Delivery Performance

Example:

Daily sales summary.

```python id="kpi01"
daily_sales = (
    df.groupby(
        df["Order Date"]
          .dt.date
    )["Sales"]
    .sum()
)
```

Monthly average order value.

```python id="kpi02"
monthly_aov = (
    df.groupby(
        df["Order Date"]
          .dt.to_period("M")
    )["Sales"]
    .mean()
)
```

---

# 18. Advanced Resampling

Resampling changes the frequency of time-series data.

Suppose sales are recorded hourly.

Convert them into daily totals.

```python id="resample02"
daily_sales = (
    df.resample(
        "D",
        on="Order Date"
    )["Sales"]
    .sum()
)
```

Weekly averages.

```python id="resample03"
weekly_sales = (
    df.resample(
        "W",
        on="Order Date"
    )["Sales"]
    .mean()
)
```

Quarterly totals.

```python id="resample04"
quarterly_sales = (
    df.resample(
        "Q",
        on="Order Date"
    )["Sales"]
    .sum()
)
```

Resampling enables flexible reporting at different time intervals.

---

# 19. Performance Optimization

Large time-series datasets require efficient processing.

### Convert Dates Immediately

```python id="perf_date01"
df["Order Date"] = (
    pd.to_datetime(
        df["Order Date"]
    )
)
```

Avoid repeatedly converting the same column.

---

### Set DateTime as Index

```python id="perf_date02"
df = df.set_index(
    "Order Date"
)
```

This improves the performance of filtering, slicing, and resampling.

---

### Sort Dates

```python id="perf_date03"
df = df.sort_index()
```

Chronological ordering improves the accuracy of rolling windows and resampling operations.

---

# 20. Business Insights

After analyzing the dataset, you discover:

* Weekend sales consistently exceed weekday sales.
* The fourth quarter contributes the highest annual revenue.
* Average delivery time is approximately four days.
* Monthly revenue follows a clear seasonal pattern.
* Holiday periods produce significant increases in sales volume.

These findings help management improve inventory planning, staffing, and marketing campaigns.

---

# 21. Practice Exercises

## Beginner

1. Convert strings to DateTime.
2. Extract the year and month.
3. Extract the weekday.
4. Filter orders after a given date.
5. Generate a weekly date range.

---

## Intermediate

6. Calculate delivery duration.
7. Add business days to an order date.
8. Convert timestamps between time zones.
9. Calculate a rolling average.
10. Generate monthly sales summaries.

---

## Advanced

11. Build a quarterly KPI report.
12. Resample hourly data into daily totals.
13. Analyze weekend versus weekday sales.
14. Create a customer retention timeline.
15. Write five recommendations based on seasonal trends.

---

# 22. Interview Questions

## Beginner

1. What is a DateTime object?
2. Why use `pd.to_datetime()`?
3. What does `.dt` do?
4. What is `NaT`?
5. How do you extract the month from a date?

---

## Intermediate

6. Difference between `Timedelta` and `DateOffset`?
7. What is resampling?
8. Why use business days?
9. How do you filter records by date?
10. Why are time zones important?

---

## Advanced

11. Explain a complete time-series workflow.
12. Compare rolling windows and resampling.
13. Design KPIs using DateTime data.
14. How would you optimize time-series analysis for millions of records?
15. Describe a real-world forecasting workflow using Pandas.

---

# 23. Cheat Sheet

| Task                | Syntax             |
| ------------------- | ------------------ |
| Convert to DateTime | `pd.to_datetime()` |
| Extract Year        | `.dt.year`         |
| Extract Month       | `.dt.month`        |
| Month Name          | `.dt.month_name()` |
| Day                 | `.dt.day`          |
| Weekday             | `.dt.day_name()`   |
| Quarter             | `.dt.quarter`      |
| Date Arithmetic     | `+ pd.Timedelta()` |
| Business Days       | `BDay()`           |
| Time Zone           | `tz_localize()`    |
| Convert Time Zone   | `tz_convert()`     |
| Shift               | `shift()`          |
| Rolling Average     | `rolling().mean()` |
| Resample            | `resample()`       |

---

# 24. Mini Project

## Time-Series Sales Analytics Dashboard

Using any retail, banking, logistics, healthcare, or finance dataset:

Complete the following tasks:

* Convert all date columns into DateTime format.
* Handle invalid dates using `errors="coerce"`.
* Extract year, month, weekday, and quarter.
* Calculate delivery duration.
* Identify weekend orders.
* Generate daily, weekly, monthly, and quarterly summaries.
* Compute rolling averages for sales.
* Resample data at different frequencies.
* Export the processed dataset.
* Write **five executive-level business insights**.
* Recommend **three business strategies** based on seasonal trends.

### Example Business Insights

* Sales consistently peak during weekends.
* Fourth-quarter revenue significantly exceeds other quarters.
* Delivery times are longest during holiday seasons.
* Customer registrations increase at the beginning of each month.
* Rolling averages reveal steady long-term revenue growth despite short-term fluctuations.

---

# 25. Summary

Congratulations! 🎉

Today you mastered **Advanced Date & Time Handling** in Pandas.

You learned how to:

* Convert text into DateTime objects.
* Extract meaningful date components.
* Filter records using dates.
* Perform date arithmetic with `Timedelta`.
* Work with business days and time zones.
* Apply rolling windows and resampling.
* Build business KPIs using temporal data.

These techniques are fundamental for financial analysis, operational reporting, customer analytics, forecasting, and time-series machine learning.

---

# 26. What's Next?

In **Day 24**, you'll learn **Categorical Data, Encoding & Memory Optimization**.

Topics include:

* Understanding Categorical Data
* Category Data Type
* Label Encoding
* One-Hot Encoding
* Dummy Variables
* Ordered Categories
* Memory Optimization
* Category Operations
* Feature Engineering for Machine Learning

These concepts are essential for building efficient analytical pipelines and preparing datasets for machine learning.

---
 

<div align="center">

#  Day 23 Complete!

You've mastered one of the most valuable aspects of Pandas—working with dates and time.

From extracting temporal features to building business KPIs, resampling data, and analyzing trends, you now have the skills needed for professional time-series analytics.

</div>
