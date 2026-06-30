# Day 17 — Data Visualization with Pandas & Matplotlib

<div align="center">

# 100 Days of Pandas

### Day 17 · Transforming Data into Meaningful Visualizations

*"Numbers explain what happened. Visualizations explain why it matters."*

![Difficulty](https://img.shields.io/badge/Difficulty-Intermediate-yellow)
![Topic](https://img.shields.io/badge/Topic-Data%20Visualization-blue)
![Day](https://img.shields.io/badge/Day-17-orange)

</div>

---


# Table of Contents

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

# 8. Bar Charts

A **Bar Chart** is one of the most commonly used visualizations for comparing values across different categories.

Unlike line charts, which emphasize trends over time, bar charts highlight differences between categories.

Typical business use cases include:

* Sales by Product Category
* Revenue by Region
* Orders by Customer Segment
* Employees by Department
* Profit by Store

---

## Example Dataset

| Category        |  Sales |
| --------------- | -----: |
| Furniture       | 125000 |
| Technology      | 182000 |
| Office Supplies |  98000 |

Create a vertical bar chart.

```python id="bar01"
df.plot(
    x="Category",
    y="Sales",
    kind="bar"
)

plt.show()
```

This chart makes it easy to compare sales across product categories.

---

## Customizing a Bar Chart

```python id="bar02"
df.plot(
    x="Category",
    y="Sales",
    kind="bar",
    figsize=(9,5)
)

plt.title("Sales by Product Category")

plt.xlabel("Category")

plt.ylabel("Sales (₹)")

plt.grid(axis="y")

plt.show()
```

Notice that the grid is displayed only on the **Y-axis**, making the chart cleaner.

---

# 9. Horizontal Bar Charts

When category names are long, horizontal bar charts improve readability.

Example:

```python id="bar03"
df.plot(
    x="Category",
    y="Sales",
    kind="barh"
)

plt.show()
```

Horizontal bar charts are widely used for:

* Customer satisfaction surveys
* Product rankings
* Department performance
* KPI dashboards

---

# 10. Pie Charts

Pie charts show the **proportion** of each category relative to the whole.

They are suitable only when the number of categories is small.

Example:

```python id="pie01"
df.plot(
    y="Sales",
    labels=df["Category"],
    kind="pie",
    autopct="%1.1f%%",
    startangle=90
)

plt.ylabel("")

plt.show()
```

Output:

* Furniture → 31%
* Technology → 45%
* Office Supplies → 24%

---

## When to Use Pie Charts

Suitable for:

* Market Share
* Expense Distribution
* Budget Allocation
* Customer Segments

Avoid pie charts when there are many categories because comparisons become difficult.

---

# 11. Histograms

Histograms display the distribution of numerical values.

Instead of comparing categories, they show how data is spread.

Example:

```python id="hist01"
df["Sales"].plot(
    kind="hist",
    bins=15
)

plt.show()
```

The `bins` parameter controls the number of intervals.

---

## Business Applications

Histograms help answer questions such as:

* What is the typical customer purchase amount?
* Are most salaries concentrated within a certain range?
* Is revenue evenly distributed?

---

# 12. Box Plots

Box plots summarize a numerical distribution using:

* Minimum
* First Quartile (Q1)
* Median
* Third Quartile (Q3)
* Maximum
* Outliers

Example:

```python id="box01"
df["Sales"].plot(
    kind="box"
)

plt.show()
```

---

## Why Box Plots Matter

Box plots quickly reveal:

* Outliers
* Data spread
* Skewness
* Median value
* Variability

Business examples:

* Employee salaries
* Customer spending
* Delivery times
* Manufacturing quality control

---

# 13. Scatter Plots

Scatter plots visualize the relationship between two numerical variables.

Example:

| Sales | Profit |
| ----: | -----: |
|  5200 |    850 |
|  6100 |   1200 |
|  7200 |   1450 |
|  8300 |   1680 |

Create a scatter plot.

```python id="scatter01"
df.plot(
    x="Sales",
    y="Profit",
    kind="scatter"
)

plt.show()
```

---

## Business Applications

Scatter plots help answer:

* Does higher advertising increase sales?
* Does customer age affect spending?
* Does experience influence salary?
* Is there a relationship between revenue and profit?

---

# 14. Choosing the Right Chart

Selecting the correct visualization is as important as creating it.

| Goal                  | Best Chart   |
| --------------------- | ------------ |
| Compare categories    | Bar Chart    |
| Show trends           | Line Chart   |
| Show proportions      | Pie Chart    |
| Display distributions | Histogram    |
| Detect outliers       | Box Plot     |
| Show relationships    | Scatter Plot |

Choosing the wrong chart can make insights difficult to interpret.

---

# Business Example

A retail company wants to present quarterly performance.

Different chart types answer different business questions.

| Business Question              | Recommended Chart |
| ------------------------------ | ----------------- |
| Monthly Sales Trend            | Line Chart        |
| Sales by Category              | Bar Chart         |
| Market Share                   | Pie Chart         |
| Customer Purchase Distribution | Histogram         |
| Delivery Time Outliers         | Box Plot          |
| Sales vs Profit                | Scatter Plot      |

A combination of these visualizations provides a complete picture of business performance.

---

# Best Practices

✔ Choose the simplest chart that communicates the message.

✔ Keep labels readable.

✔ Use descriptive titles.

✔ Avoid unnecessary visual clutter.

✔ Limit pie charts to a small number of categories.

✔ Always label units such as ₹, %, or quantities.

---

# Common Mistakes

### Using Pie Charts with Too Many Categories

A pie chart with ten or more slices becomes difficult to interpret.

Consider using a bar chart instead.

---

### Plotting Unrelated Variables

Scatter plots should only compare variables that may have a meaningful relationship.

---

### Ignoring Outliers

Before drawing conclusions from charts, investigate unusual values that may distort the visualization.

---

# Quick Recap

You have now learned how to:

* Create Bar Charts.
* Create Horizontal Bar Charts.
* Build Pie Charts.
* Plot Histograms.
* Visualize Box Plots.
* Create Scatter Plots.
* Select the appropriate chart for different business questions.

> **"The most effective visualization is not the most attractive one—it is the one that communicates the right insight with the least effort from the audience."**
