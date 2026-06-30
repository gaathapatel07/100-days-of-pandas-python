# 🐼 Day 16 — Time Series Analysis with Pandas

<div align="center">

# 100 Days of Pandas

### Day 16 · Unlocking Insights from Time-Based Data

*"Every timestamp tells a story. Time series analysis helps us uncover trends, seasonality, and patterns hidden within chronological data."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Time%20Series%20Analysis-blue)
![Day](https://img.shields.io/badge/Day-16-orange)

</div>

---

# 📚 Table of Contents

1. Introduction
2. Why Time Series Analysis Matters
3. Learning Objectives
4. Understanding Time Series Data
5. Creating a DateTime Index
6. Resampling Data
7. Frequency Conversion
8. Summary

---

# 1. Introduction

Many business datasets are organized around time.

Examples include:

* Daily sales transactions
* Hourly website traffic
* Monthly revenue reports
* Minute-by-minute stock prices
* Weather observations
* IoT sensor readings
* Hospital patient monitoring
* Energy consumption logs

Unlike ordinary datasets, time series data emphasizes the **order of observations**. The sequence of events is often as important as the values themselves.

Pandas provides specialized tools for analyzing chronological data efficiently.

---

# 2. Why Time Series Analysis Matters

Imagine you're working as a Data Analyst for an online retailer.

Management asks:

* Which month generated the highest revenue?
* Are weekend sales increasing?
* How does daily traffic compare with monthly trends?
* Which quarter experienced the strongest growth?
* Can we forecast future demand?

These questions require more than simple aggregation—they require time-aware analysis.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Create DateTime indexes.
* Resample time series data.
* Convert between different frequencies.
* Prepare datasets for forecasting.
* Analyze long-term trends.
* Build time-based business reports.

---

# 4. Understanding Time Series Data

A **time series** is a sequence of observations recorded at regular or irregular intervals.

Example:

| Date       | Sales |
| ---------- | ----: |
| 2025-01-01 |  5200 |
| 2025-01-02 |  6100 |
| 2025-01-03 |  5900 |
| 2025-01-04 |  6800 |

The **Date** column becomes the most important dimension because every observation depends on its position in time.

Time series datasets often include:

* Daily sales
* Hourly temperatures
* Weekly website visits
* Monthly profits
* Quarterly revenue

---

# 5. Creating a DateTime Index

Most time series operations require a DateTime index.

Suppose the dataset contains:

| Date       | Sales |
| ---------- | ----: |
| 2025-01-01 |  5200 |
| 2025-01-02 |  6100 |
| 2025-01-03 |  5900 |

First, convert the Date column.

```python id="ts01"
df["Date"] = pd.to_datetime(
    df["Date"]
)
```

Now set it as the index.

```python id="ts02"
df = df.set_index("Date")
```

Output:

| Date       | Sales |
| ---------- | ----: |
| 2025-01-01 |  5200 |
| 2025-01-02 |  6100 |
| 2025-01-03 |  5900 |

Many Pandas time series functions depend on a DateTime index.

---

# 6. Resampling Data

Resampling changes the frequency of observations.

For example:

Daily data can become:

* Weekly
* Monthly
* Quarterly
* Yearly

Example:

Calculate monthly sales totals.

```python id="ts03"
df.resample("M").sum()
```

Output:

| Month    |  Sales |
| -------- | -----: |
| January  | 186000 |
| February | 172000 |

Resampling is fundamental for reporting and dashboard creation.

---

## Monthly Average

Instead of totals:

```python id="ts04"
df.resample("M").mean()
```

Common aggregation functions include:

* `sum()`
* `mean()`
* `max()`
* `min()`
* `count()`

---

# 7. Frequency Conversion

Pandas supports many time frequencies.

| Frequency | Meaning     |
| --------- | ----------- |
| `"D"`     | Daily       |
| `"W"`     | Weekly      |
| `"ME"`    | Month End   |
| `"QE"`    | Quarter End |
| `"YE"`    | Year End    |
| `"h"`     | Hourly      |
| `"min"`   | Minute      |
| `"s"`     | Second      |

Examples:

Weekly totals:

```python id="ts05"
df.resample("W").sum()
```

Quarterly averages:

```python id="ts06"
df.resample("QE").mean()
```

Yearly maximum:

```python id="ts07"
df.resample("YE").max()
```

Choosing the correct frequency depends on the business question being answered.

---

# Key Takeaways

After completing this section, you should understand:

* What makes time series data unique.
* Why a DateTime index is important.
* How to resample datasets.
* How to convert between time frequencies.
* Why time-aware analysis is essential in business intelligence.

> **"Time transforms isolated observations into meaningful trends, revealing patterns that static analysis can never uncover."**

