# Day 48 — Time Series Analysis with Pandas

## Introduction

Time Series Analysis is the process of analyzing data collected over time to identify trends, seasonal patterns, cycles, and future behavior.

Unlike ordinary datasets, time series data has a **time component**, meaning the order of observations matters.

Examples include:

- Daily stock prices
- Monthly sales
- Website traffic
- Weather reports
- Hospital admissions
- Electricity consumption

Pandas provides powerful tools to manipulate and analyze time-based data efficiently.

---

# Topics Covered

- Introduction to Time Series
- Datetime Data Type
- Converting Strings to Dates
- Extracting Date Components
- Creating Date Ranges
- Time Indexing
- Filtering by Date
- Resampling
- Rolling Windows
- Expanding Windows

---

# 1. What is Time Series Data?

Time series data consists of observations recorded at regular intervals.

Example:

| Date | Sales |
|------|-------|
|2025-01-01|1200|
|2025-01-02|1350|
|2025-01-03|1420|
|2025-01-04|1500|

Unlike categorical data, time series maintains chronological order.

---

# 2. Why Time Series Analysis Matters

Businesses use time series analysis to:

- Forecast future sales
- Detect trends
- Identify seasonality
- Monitor KPIs
- Predict demand
- Detect anomalies

Industries using time series:

- Finance
- Retail
- Healthcare
- Manufacturing
- Banking
- Telecommunications

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

- Work with datetime objects.
- Convert strings into dates.
- Extract useful date information.
- Create date ranges.
- Index data by time.
- Perform resampling.
- Calculate rolling statistics.

---

# 4. Datetime Data Type

Convert a string column into datetime.

```python
df["Order Date"] = pd.to_datetime(
    df["Order Date"]
)
```

Check data type.

```python
df["Order Date"].dtype
```

Output:

```
datetime64[ns]
```

Datetime enables powerful time-based operations.

---

# 5. Extracting Date Components

Extract year.

```python
df["Year"] = df["Order Date"].dt.year
```

Extract month.

```python
df["Month"] = df["Order Date"].dt.month
```

Extract month name.

```python
df["Month Name"] = df["Order Date"].dt.month_name()
```

Extract day.

```python
df["Day"] = df["Order Date"].dt.day
```

Extract weekday.

```python
df["Weekday"] = df["Order Date"].dt.day_name()
```

Extract quarter.

```python
df["Quarter"] = df["Order Date"].dt.quarter
```

---

# 6. Creating Date Ranges

Generate a sequence of dates.

```python
dates = pd.date_range(

    start="2025-01-01",

    periods=10,

    freq="D"

)

print(dates)
```

Monthly range.

```python
pd.date_range(

    "2025-01-01",

    periods=12,

    freq="M"

)
```

Hourly range.

```python
pd.date_range(

    "2025-01-01",

    periods=24,

    freq="H"

)
```

---

# 7. Setting a Date Index

Use dates as the DataFrame index.

```python
df = df.set_index("Order Date")
```

View the index.

```python
df.index
```

Benefits:

- Faster filtering
- Time slicing
- Resampling
- Rolling calculations

---

# 8. Filtering by Date

Retrieve data for a specific year.

```python
df["2025"]
```

Filter a specific month.

```python
df["2025-03"]
```

Filter a date range.

```python
df.loc["2025-01-01":"2025-03-31"]
```

---

# 9. Resampling

Resampling changes the frequency of time-series data.

Monthly sales.

```python
monthly_sales = df["Sales"].resample("M").sum()
```

Weekly sales.

```python
weekly_sales = df["Sales"].resample("W").sum()
```

Quarterly sales.

```python
quarterly_sales = df["Sales"].resample("Q").sum()
```

Annual sales.

```python
yearly_sales = df["Sales"].resample("Y").sum()
```

---

# 10. Rolling Windows

Rolling calculations analyze moving periods.

7-day moving average.

```python
df["Sales"].rolling(7).mean()
```

30-day moving average.

```python
df["Sales"].rolling(30).mean()
```

Rolling sum.

```python
df["Sales"].rolling(7).sum()
```

Rolling maximum.

```python
df["Sales"].rolling(7).max()
```

Rolling windows smooth short-term fluctuations.

---

# 11. Expanding Windows

Expanding windows calculate cumulative statistics.

Cumulative average.

```python
df["Sales"].expanding().mean()
```

Cumulative sum.

```python
df["Sales"].expanding().sum()
```

Cumulative maximum.

```python
df["Sales"].expanding().max()
```

Useful for tracking long-term business performance.

---

# Business Example

An e-commerce company analyzes two years of sales data.

Using time-series analysis, analysts discover:

- Sales increase during festive seasons.
- Mondays have the lowest average revenue.
- Quarterly growth is steadily increasing.
- A 30-day moving average removes daily fluctuations.
- Rolling statistics reveal long-term growth trends.

These insights help management improve demand forecasting and inventory planning.

---

# Best Practices

✔ Always convert date columns to `datetime`.

✔ Use a datetime index for time-series operations.

✔ Choose the correct resampling frequency.

✔ Use rolling averages to identify trends.

✔ Consider seasonality when analyzing data.

---

# Common Mistakes

### Treating Dates as Strings

String dates cannot use Pandas' powerful datetime functions.

---

### Ignoring Missing Dates

Missing dates can distort trend analysis and forecasts.

---

### Choosing Incorrect Resampling Frequency

Daily, weekly, monthly, or yearly aggregation should match the business problem.

---

# Key Takeaways

After completing this section, you should understand:

- Datetime conversion
- Date component extraction
- Date ranges
- Time indexing
- Date filtering
- Resampling
- Rolling windows
- Expanding windows

> **"Time-series analysis reveals how data evolves over time, enabling organizations to identify trends, forecast demand, and make proactive business decisions."**

---

## Next (Day 48 – Part 2)

The next section covers:

- Shift & Lag Operations
- Difference Calculations
- Percentage Change
- Time-Based Grouping
- Rolling Correlation
- Time-Series Visualization
- Business Trend Analysis
- Forecasting Preparation

# 12. Shift Operations

The `shift()` function moves data forward or backward within a column.

Shift values down by one row.

```python
df["Previous Day Sales"] = df["Sales"].shift(1)
```

Shift values up by one row.

```python
df["Next Day Sales"] = df["Sales"].shift(-1)
```

Example

| Date | Sales | Previous Day Sales |
|------|-------:|-------------------:|
|01-Jan|1200|NaN|
|02-Jan|1350|1200|
|03-Jan|1500|1350|

Applications:

- Compare current and previous values
- Build lag features
- Detect sudden changes

---

# 13. Lag Features

Lag features store previous observations as new variables.

Previous day's revenue.

```python
df["Revenue_Lag1"] = df["Revenue"].shift(1)
```

Previous week's revenue.

```python
df["Revenue_Lag7"] = df["Revenue"].shift(7)
```

Previous month's sales.

```python
df["Sales_Lag30"] = df["Sales"].shift(30)
```

Lag features are widely used in forecasting models.

---

# 14. Difference Calculations

Calculate the difference between consecutive observations.

```python
df["Sales Difference"] = df["Sales"].diff()
```

Difference over seven days.

```python
df["Weekly Difference"] = df["Sales"].diff(7)
```

Example

| Day | Sales | Difference |
|----|-------:|-----------:|
|1|100|NaN|
|2|120|20|
|3|115|-5|

Positive values indicate growth, while negative values indicate decline.

---

# 15. Percentage Change

Calculate growth rate between observations.

```python
df["Growth Rate"] = df["Sales"].pct_change()
```

Convert to percentage.

```python
df["Growth %"] = (
    df["Sales"]
      .pct_change()
      * 100
)
```

Example

| Month | Sales | Growth % |
|-------|-------:|---------:|
|Jan|1000|NaN|
|Feb|1200|20|
|Mar|1500|25|

Useful for financial and business performance analysis.

---

# 16. Time-Based Grouping

Group records using time intervals.

Monthly sales.

```python
monthly_sales = df.groupby(

    pd.Grouper(freq="M")

)["Sales"].sum()
```

Quarterly revenue.

```python
quarterly = df.groupby(

    pd.Grouper(freq="Q")

)["Revenue"].sum()
```

Yearly profit.

```python
yearly = df.groupby(

    pd.Grouper(freq="Y")

)["Profit"].sum()
```

This method is useful when the DataFrame has a datetime index.

---

# 17. Rolling Correlation

Rolling correlation measures how the relationship between two variables changes over time.

30-day rolling correlation.

```python
rolling_corr = (

    df["Sales"]

    .rolling(30)

    .corr(df["Profit"])

)
```

Applications:

- Financial markets
- Demand analysis
- Revenue vs Profit trends

---

# 18. Time-Series Visualization

Daily sales trend.

```python
df["Sales"].plot(

    figsize=(10,5),

    title="Daily Sales"

)
```

Monthly revenue.

```python
monthly_sales.plot(

    kind="line",

    marker="o"

)
```

Moving average.

```python
df["Sales"].rolling(

    30

).mean().plot(

    label="30-Day Average"

)

df["Sales"].plot(

    alpha=0.5,

    label="Daily Sales"

)

plt.legend()

plt.show()
```

Moving averages help visualize long-term trends by reducing daily fluctuations.

---

# 19. Business Trend Analysis

Questions answered using time-series analysis:

- Is revenue increasing every quarter?
- Which month has the highest sales?
- Are weekends more profitable?
- Which season generates maximum revenue?
- Are there recurring business cycles?

Example:

Monthly average revenue.

```python
df.groupby(

    df.index.month

)["Revenue"].mean()
```

Average weekday sales.

```python
df.groupby(

    df.index.day_name()

)["Sales"].mean()
```

Quarter-wise profit.

```python
df.groupby(

    df.index.quarter

)["Profit"].sum()
```

---

# 20. Preparing Data for Forecasting

Before forecasting:

- Convert dates to `datetime`
- Sort observations chronologically
- Handle missing values
- Remove duplicate records
- Create lag features
- Calculate rolling averages
- Detect seasonality
- Engineer date-based features

Example:

```python
df = df.sort_index()

df["Lag1"] = df["Sales"].shift(1)

df["Rolling7"] = df["Sales"].rolling(7).mean()
```

The resulting dataset is suitable for forecasting models.

---

# Business Example

An online retailer analyzes three years of transaction data.

The analysis reveals:

- Revenue grows steadily during the fourth quarter each year.
- Weekend sales consistently exceed weekday sales.
- A 30-day moving average highlights long-term growth despite daily fluctuations.
- Lag features improve sales forecasting accuracy.
- Quarterly grouping reveals seasonal demand patterns that guide inventory planning.

---

# Best Practices

✔ Sort data chronologically before analysis.

✔ Use lag features for forecasting tasks.

✔ Analyze both absolute differences and percentage growth.

✔ Combine rolling statistics with visualizations.

✔ Select grouping frequencies that align with business objectives.

---

# Common Mistakes

### Using Unsorted Dates

Unsorted time-series data produces incorrect lag and rolling calculations.

---

### Ignoring Missing Time Periods

Missing dates can distort trend analysis and forecasting results.

---

### Comparing Different Time Frequencies

Avoid comparing daily data directly with monthly aggregates without proper resampling.

---

# Quick Recap

You have now learned how to:

- Shift observations.
- Create lag features.
- Calculate differences.
- Compute percentage growth.
- Group data by time intervals.
- Measure rolling correlations.
- Visualize time-series trends.
- Prepare data for forecasting.

> **"Time-series analysis helps organizations move beyond understanding the past to anticipating the future through trends, seasonality, and forecasting-ready data."**

---

## Next (Day 48 – Final Part)

The final section will cover:

- Enterprise Time-Series Workflow
- Automated Time-Series Pipelines
- Production Best Practices
- Interview Questions (20+)
- Practice Exercises
- Cheat Sheet
- Mini Project
- Executive Business Insights
- Complete Day 48 Summary

# 21. Enterprise Time-Series Workflow

Professional organizations follow a structured workflow for analyzing time-based data to ensure accurate forecasting and reliable business insights.

```
Raw Time-Series Data
        │
        ▼
Import Dataset
        │
        ▼
Convert to Datetime
        │
        ▼
Set Datetime Index
        │
        ▼
Handle Missing Dates
        │
        ▼
Exploratory Time-Series Analysis
        │
        ▼
Feature Engineering
(Lag, Rolling, Date Features)
        │
        ▼
Trend & Seasonality Analysis
        │
        ▼
Forecast Preparation
        │
        ▼
Business Insights & Reporting
```

Following this workflow improves data quality, reproducibility, and forecasting accuracy.

---

# 22. Automated Time-Series Pipeline

Create a reusable function to summarize time-series datasets.

```python
def time_series_summary(df, column):

    summary = {

        "Start Date": df.index.min(),

        "End Date": df.index.max(),

        "Total Records": len(df),

        "Missing Values": df[column].isna().sum(),

        "Average": df[column].mean(),

        "Maximum": df[column].max(),

        "Minimum": df[column].min()

    }

    return pd.DataFrame(

        summary.items(),

        columns=["Metric", "Value"]

    )
```

Run the function.

```python
report = time_series_summary(df, "Sales")

print(report)
```

Example Output

| Metric | Value |
|---------|-------|
|Start Date|2023-01-01|
|End Date|2025-12-31|
|Total Records|1095|
|Missing Values|0|
|Average|5,240|
|Maximum|12,350|
|Minimum|890|

---

# 23. Production Best Practices

### Always Use Datetime Objects

Convert date columns using `pd.to_datetime()` before analysis.

---

### Sort Data Chronologically

```python
df = df.sort_index()
```

Correct ordering is essential for rolling calculations and forecasting.

---

### Handle Missing Dates

Fill or interpolate missing periods before analysis.

```python
df = df.asfreq("D")
```

---

### Choose Appropriate Frequency

Use frequencies that align with business objectives.

Examples:

- Hourly
- Daily
- Weekly
- Monthly
- Quarterly
- Yearly

---

### Engineer Time Features

Useful features include:

- Year
- Month
- Quarter
- Weekday
- Weekend
- Holiday Indicator

These features often improve forecasting models.

---

# 24. Enterprise Case Study

## Scenario

A nationwide supermarket chain wants to forecast future sales.

Business questions:

- Which months have the highest demand?
- Are weekends more profitable?
- Is revenue growing every quarter?
- What is the long-term trend?
- Are there seasonal spikes?

---

### Monthly Revenue

```python
monthly = df["Revenue"].resample("M").sum()
```

---

### Quarterly Revenue

```python
quarterly = df["Revenue"].resample("Q").sum()
```

---

### 30-Day Moving Average

```python
moving_avg = df["Revenue"].rolling(30).mean()
```

---

### Growth Rate

```python
growth = df["Revenue"].pct_change() * 100
```

---

### Lag Feature

```python
df["Revenue_Lag1"] = df["Revenue"].shift(1)
```

These analyses provide the foundation for accurate sales forecasting.

---

# 25. Executive Business Insights

After completing the analysis, analysts discover:

- Revenue increases steadily throughout the year.
- Sales consistently peak during festive seasons.
- Weekend transactions are significantly higher than weekdays.
- Quarterly revenue shows positive year-over-year growth.
- Rolling averages reveal sustained long-term growth despite short-term fluctuations.
- Lag features demonstrate strong dependence on previous sales values.
- Seasonal demand patterns support better inventory and staffing decisions.

These insights help executives improve forecasting, budgeting, and strategic planning.

---

# 26. Practice Exercises

## Beginner

1. Convert a column to datetime.
2. Extract month and year.
3. Set the datetime column as the index.
4. Filter records for one month.
5. Create a daily date range.

---

## Intermediate

6. Calculate monthly sales.
7. Create rolling averages.
8. Calculate cumulative revenue.
9. Create lag features.
10. Compute percentage growth.

---

## Advanced

11. Analyze seasonal trends.
12. Build an automated time-series report.
13. Compare quarterly performance.
14. Prepare forecasting-ready data.
15. Generate executive business insights.

---

# 27. Interview Questions

## Beginner

1. What is time-series data?
2. Why should dates be converted to datetime?
3. What is resampling?
4. What is a rolling window?
5. What is a lag feature?

---

## Intermediate

6. Explain `shift()`.
7. Difference between rolling and expanding windows.
8. What is percentage change?
9. How does `pd.Grouper()` work?
10. Why is sorting important?

---

## Advanced

11. Explain seasonality.
12. Explain trend analysis.
13. How would you prepare time-series data for forecasting?
14. How do rolling averages reduce noise?
15. Design an enterprise time-series workflow.

---

# 28. Cheat Sheet

| Task | Syntax |
|------|--------|
| Convert to Datetime | `pd.to_datetime()` |
| Date Range | `pd.date_range()` |
| Set Date Index | `set_index()` |
| Filter Dates | `loc[]` |
| Resample | `resample()` |
| Rolling Mean | `rolling().mean()` |
| Rolling Sum | `rolling().sum()` |
| Expanding Mean | `expanding().mean()` |
| Shift | `shift()` |
| Difference | `diff()` |
| Percentage Change | `pct_change()` |
| Time Grouping | `pd.Grouper()` |

---

# 29. Mini Project

## Retail Sales Time-Series Analysis

Using any retail, banking, finance, healthcare, or telecom dataset:

Perform the following:

- Convert date columns to datetime.
- Set a datetime index.
- Analyze monthly sales.
- Calculate rolling averages.
- Compute cumulative revenue.
- Generate lag features.
- Calculate percentage growth.
- Identify seasonal trends.
- Visualize sales over time.
- Prepare forecasting-ready data.

Finally, write:

- Five executive business insights.
- Three recommendations for improving future business performance.

### Example Insights

- Sales increase sharply during festive months.
- Weekends consistently outperform weekdays.
- Quarterly revenue has grown year over year.
- Rolling averages reveal a stable upward trend.
- Previous month's sales strongly influence current sales.

---

# 30. Summary

Congratulations! 🎉

Today you mastered **Time Series Analysis with Pandas**.

You learned how to:

- Work with datetime objects.
- Extract date-based features.
- Create date ranges.
- Index data using dates.
- Filter time-based records.
- Resample data.
- Calculate rolling and expanding statistics.
- Create lag features.
- Analyze trends and growth.
- Prepare data for forecasting.
- Build enterprise-ready time-series workflows.

These techniques are fundamental for forecasting, demand planning, financial analysis, and business intelligence.

---

# 31. What's Next?

## 🐼 Day 49 — Advanced String Operations with Pandas

Topics include:

- String Accessor (`.str`)
- Case Conversion
- Trimming & Cleaning Text
- Splitting & Joining Strings
- Replacing Text
- Regular Expressions
- Pattern Matching
- Text Extraction
- Text Feature Engineering
- Business Applications

String processing is essential for cleaning customer names, product descriptions, email addresses, reviews, and other text-based data before analysis.

---

#  Day 48 Complete!

You have successfully completed **Time Series Analysis with Pandas**.

You can now confidently analyze chronological datasets, identify trends and seasonality, engineer time-based features, and prepare data for forecasting and business decision-making.

