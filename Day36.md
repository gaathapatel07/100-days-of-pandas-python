# 🐼 Day 36 — Advanced Visualization with Pandas

<div align="center">

# 100 Days of Pandas

### Day 36 · Exploring & Communicating Data Through Visualizations

*"A well-designed visualization turns complex datasets into clear insights that drive better decisions."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Visualization-blue)
![Day](https://img.shields.io/badge/Day-36-orange)

</div>

---

# 📚 Table of Contents

1. Introduction
2. Why Visualization Matters
3. Learning Objectives
4. Line Charts
5. Bar Charts
6. Horizontal Bar Charts
7. Area Charts
8. Summary

---

# 1. Introduction

Visualization is one of the final steps in the data analysis process.

Instead of reading thousands of rows, charts help identify:

* Trends
* Patterns
* Relationships
* Outliers
* Comparisons
* Seasonal behavior

Pandas provides built-in plotting functions powered by **Matplotlib**.

---

# 2. Why Visualization Matters

Imagine a retail company with five years of sales data.

A spreadsheet containing 500,000 rows is difficult to interpret.

A simple line chart immediately answers questions such as:

* Are sales increasing?
* Which months performed best?
* Are there seasonal patterns?

Visualization makes analytical findings accessible to both technical and non-technical audiences.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Create common business charts.
* Compare categories visually.
* Identify trends.
* Customize basic plots.
* Choose the appropriate chart type.

---

# 4. Line Charts

Line charts are ideal for continuous data, especially time series.

Example:

```python id="line01"
df.plot(
    x="Date",
    y="Sales",
    kind="line"
)
```

Output:

A line showing how sales change over time.

---

## Multiple Lines

```python id="line02"
df.plot(
    x="Date",
    y=[
        "Sales",
        "Profit"
    ],
    kind="line"
)
```

Useful for comparing multiple metrics over the same period.

---

## Customize the Chart

```python id="line03"
df.plot(
    x="Date",
    y="Sales",
    kind="line",
    figsize=(10,5),
    title="Monthly Sales Trend",
    grid=True
)
```

Common customization options:

| Parameter | Purpose      |
| --------- | ------------ |
| `figsize` | Chart size   |
| `title`   | Chart title  |
| `grid`    | Display grid |
| `legend`  | Show legend  |

---

# 5. Bar Charts

Bar charts compare categorical values.

Example:

| Region |  Sales |
| ------ | -----: |
| North  | 120000 |
| South  |  98000 |
| East   |  76000 |
| West   | 112000 |

```python id="bar01"
df.plot(
    x="Region",
    y="Sales",
    kind="bar"
)
```

Bar charts are excellent for:

* Regional sales
* Department performance
* Product comparisons
* Employee productivity

---

## Multiple Bars

```python id="bar02"
df.plot(
    x="Region",
    y=[
        "Sales",
        "Profit"
    ],
    kind="bar"
)
```

This enables side-by-side comparison.

---

# 6. Horizontal Bar Charts

When category names are long, horizontal bars improve readability.

```python id="bar03"
df.plot(
    x="Product",
    y="Revenue",
    kind="barh"
)
```

Useful for:

* Product rankings
* Customer rankings
* Survey results

---

# 7. Area Charts

Area charts emphasize cumulative changes over time.

```python id="area01"
df.plot(
    x="Month",
    y="Sales",
    kind="area"
)
```

---

## Stacked Area Chart

```python id="area02"
df.plot(
    x="Month",
    y=[
        "Sales",
        "Profit"
    ],
    kind="area",
    stacked=True
)
```

Applications:

* Revenue contribution
* Market share
* Product category trends

---

# Business Example

A retail company wants to build a monthly management report.

Visualizations include:

* Line chart for sales trend.
* Bar chart for regional comparison.
* Horizontal bar chart for top-selling products.
* Area chart showing cumulative monthly revenue.

These visualizations help executives identify trends and compare performance quickly.

---

# Best Practices

✔ Use line charts for continuous time-series data.

✔ Use bar charts for category comparisons.

✔ Add clear titles and labels.

✔ Keep visualizations simple and readable.

✔ Avoid unnecessary chart elements.

---

# Common Mistakes

### Using Line Charts for Unordered Categories

Incorrect:

```text id="mistake01"
Delhi

Mumbai

Pune

Jaipur
```

These categories have no natural order.

Use a **bar chart** instead.

---

### Overcrowding Charts

Displaying too many lines or categories can make a chart difficult to interpret.

Focus on the most relevant metrics.

---

### Missing Titles

Always provide descriptive titles.

Example:

```text id="mistake02"
Monthly Sales Trend (2026)
```

instead of

```text id="mistake03"
Chart 1
```

---

# Key Takeaways

After completing this section, you should understand:

* How to create line charts.
* How to compare categories using bar charts.
* When to use horizontal bars.
* How area charts show cumulative trends.
* Why selecting the right chart improves communication.

> **"The purpose of visualization is not simply to create attractive charts—it is to communicate meaningful insights clearly and accurately."**

