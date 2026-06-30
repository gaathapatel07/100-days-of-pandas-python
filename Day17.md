# 🐼 Day 17 — Data Visualization with Pandas & Matplotlib

<div align="center">

# 100 Days of Pandas

### Day 17 · Transforming Data into Meaningful Visualizations

*"Numbers explain what happened. Visualizations explain why it matters."*

![Difficulty](https://img.shields.io/badge/Difficulty-Intermediate-yellow)
![Topic](https://img.shields.io/badge/Topic-Data%20Visualization-blue)
![Day](https://img.shields.io/badge/Day-17-orange)

</div>

---

# 📚 Table of Contents

1. Introduction
2. Why Visualization Matters
3. Learning Objectives
4. Introduction to Matplotlib
5. Plotting with Pandas
6. Line Charts
7. Customizing Charts
8. Summary

---

# 1. Introduction

Data analysis is not complete until insights are communicated effectively.

Businesses rarely make decisions by reading raw tables containing thousands of rows. Instead, they rely on charts, dashboards, and visual reports to quickly understand trends, patterns, and anomalies.

Data visualization converts raw numbers into graphical representations that are easier to interpret and communicate.

Pandas integrates seamlessly with **Matplotlib**, allowing analysts to create professional visualizations using just a few lines of code.

---

# 2. Why Data Visualization Matters

Imagine a company records sales for every day of the year.

A table containing **365 rows** provides detailed information but makes it difficult to identify long-term trends.

| Date       | Sales |
| ---------- | ----: |
| 2025-01-01 |  5200 |
| 2025-01-02 |  6100 |
| 2025-01-03 |  5800 |
| ...        |   ... |

A line chart immediately answers questions such as:

* Are sales increasing over time?
* Which months performed best?
* Are there seasonal trends?
* Were there sudden drops or spikes?

Visualizations help decision-makers understand information much faster than raw data.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Create charts directly from Pandas DataFrames.
* Understand when to use different chart types.
* Customize titles, labels, and colors.
* Build publication-ready visualizations.
* Present business insights visually.

---

# 4. Introduction to Matplotlib

**Matplotlib** is the most widely used visualization library in Python.

Pandas uses Matplotlib internally for plotting.

Import the required libraries:

```python
import pandas as pd
import matplotlib.pyplot as plt
```

Most Pandas visualizations are built using:

```python
df.plot()
```

Matplotlib then renders the final chart.

---

# 5. Plotting with Pandas

Suppose we have monthly sales data.

| Month | Sales |
| ----- | ----: |
| Jan   |  5200 |
| Feb   |  6100 |
| Mar   |  5800 |
| Apr   |  6900 |
| May   |  7200 |

Create a simple line chart.

```python
df.plot(
    x="Month",
    y="Sales"
)

plt.show()
```

This automatically generates a basic line chart with minimal code.

---

# 6. Line Charts

A **Line Chart** connects data points using straight lines.

It is best suited for:

* Time-series analysis
* Sales trends
* Website traffic
* Temperature changes
* Stock prices
* Revenue growth

Example:

```python
df.plot(
    x="Month",
    y="Sales",
    kind="line"
)

plt.show()
```

---

## Adding Markers

Markers make individual observations easier to identify.

```python
df.plot(
    x="Month",
    y="Sales",
    kind="line",
    marker="o"
)

plt.show()
```

Popular marker styles include:

| Marker | Description |
| ------ | ----------- |
| `"o"`  | Circle      |
| `"s"`  | Square      |
| `"^"`  | Triangle    |
| `"*"`  | Star        |
| `"D"`  | Diamond     |

---

# 7. Customizing Charts

Professional charts should include descriptive titles and axis labels.

Example:

```python
df.plot(
    x="Month",
    y="Sales",
    marker="o",
    figsize=(10,5)
)

plt.title("Monthly Sales Performance")

plt.xlabel("Month")

plt.ylabel("Sales (₹)")

plt.grid(True)

plt.show()
```

---

## Changing Figure Size

```python
plt.figure(figsize=(12,6))
```

Larger figures improve readability when presenting dashboards or reports.

---

## Adding Grid Lines

```python
plt.grid(True)
```

Grid lines help viewers estimate values more accurately.

---

## Rotating Axis Labels

```python
plt.xticks(rotation=45)
```

Useful when category names or dates are long.

---

# Business Example

A retail company wants to monitor monthly revenue.

Instead of reviewing hundreds of transaction records, a line chart immediately reveals:

* Revenue growth over time.
* Seasonal sales peaks.
* Sudden declines.
* Performance trends across months.

Visualization transforms complex datasets into actionable business insights.

---

# Best Practices

✔ Choose the appropriate chart type for the data.

✔ Add informative titles and labels.

✔ Keep charts clean and uncluttered.

✔ Use readable figure sizes.

✔ Include grid lines when they improve interpretation.

✔ Always label units such as ₹, %, or quantities.

---

# Common Mistakes

### Plotting Unsorted Time-Series Data

Always sort chronological data before plotting.

```python
df = df.sort_values("Date")
```

---

### Missing Axis Labels

A chart without labels forces viewers to guess what the values represent.

Always include descriptive axis labels and titles.

---

# Key Takeaways

After completing this section, you should understand:

* Why visualization is important in analytics.
* How Pandas integrates with Matplotlib.
* How to create line charts.
* How to customize titles, labels, and figure sizes.
* Why clear visualizations improve business communication.

> **"A well-designed chart communicates insights in seconds that might take pages of tables to explain."**

