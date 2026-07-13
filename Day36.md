#  Day 36 — Advanced Visualization with Pandas

<div align="center">

# 100 Days of Pandas

### Day 36 · Exploring & Communicating Data Through Visualizations

*"A well-designed visualization turns complex datasets into clear insights that drive better decisions."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Visualization-blue)
![Day](https://img.shields.io/badge/Day-36-orange)

</div>

---

#  Table of Contents

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

# 8. Histograms

Histograms display the distribution of numerical data.

Example:

Analyze customer ages.

```python id="hist01"
df["Age"].plot(
    kind="hist",
    bins=20
)
```

Output:

A histogram showing how customers are distributed across different age groups.

---

## Customize Histogram

```python id="hist02"
df["Sales"].plot(
    kind="hist",
    bins=15,
    figsize=(8,5),
    title="Sales Distribution"
)
```

Applications:

* Customer age analysis
* Income distribution
* Sales distribution
* Exam scores

---

# 9. Box Plots

Box plots summarize data using:

* Minimum
* First Quartile (Q1)
* Median
* Third Quartile (Q3)
* Maximum
* Outliers

```python id="box01"
df.plot(
    y="Sales",
    kind="box"
)
```

Output:

A box plot highlighting spread and outliers.

---

## Multiple Box Plots

```python id="box02"
df.plot(
    y=[
        "Sales",
        "Profit"
    ],
    kind="box"
)
```

Useful for comparing distributions.

---

# 10. Scatter Plots

Scatter plots visualize relationships between two numerical variables.

Example:

```python id="scatter01"
df.plot(
    x="Sales",
    y="Profit",
    kind="scatter"
)
```

Applications:

* Sales vs Profit
* Height vs Weight
* Marketing Spend vs Revenue
* Experience vs Salary

---

## Identify Correlation

Strong upward trend:

Positive correlation

Strong downward trend:

Negative correlation

Random points:

Little or no correlation

---

# 11. Pie Charts

Pie charts display proportional contributions.

Example:

Regional sales.

```python id="pie01"
df.set_index(
    "Region"
).plot(
    y="Sales",
    kind="pie",
    autopct="%1.1f%%"
)
```

Applications:

* Market share
* Revenue contribution
* Expense breakdown
* Customer segmentation

---

# 12. Density (KDE) Plots

Kernel Density Estimation (KDE) creates a smooth distribution curve.

```python id="kde01"
df["Sales"].plot(
    kind="density"
)
```

Useful for:

* Understanding distributions
* Comparing datasets
* Detecting skewness

---

# 13. Hexbin Plots

Scatter plots become cluttered with large datasets.

Hexbin plots group nearby points into hexagonal bins.

```python id="hex01"
df.plot(
    x="Sales",
    y="Profit",
    kind="hexbin",
    gridsize=20
)
```

Applications:

* Large datasets
* Transaction analysis
* Customer behavior
* Financial analytics

---

# 14. Creating Subplots

Display multiple visualizations together.

```python id="subplot01"
df.plot(
    subplots=True,
    figsize=(10,8)
)
```

Each numerical column receives its own chart.

---

## Selected Columns

```python id="subplot02"
df[
    [
        "Sales",
        "Profit"
    ]
].plot(
    subplots=True,
    layout=(2,1),
    figsize=(8,6)
)
```

Useful for KPI dashboards.

---

# 15. Plot Customization

Customize chart appearance.

```python id="custom01"
df.plot(
    x="Date",
    y="Sales",
    kind="line",
    figsize=(12,6),
    title="Monthly Sales",
    xlabel="Month",
    ylabel="Revenue",
    grid=True,
    legend=True
)
```

Common customization options:

| Parameter | Purpose            |
| --------- | ------------------ |
| `title`   | Chart title        |
| `xlabel`  | X-axis label       |
| `ylabel`  | Y-axis label       |
| `figsize` | Figure size        |
| `grid`    | Grid lines         |
| `legend`  | Display legend     |
| `rot`     | Rotate axis labels |

---

# Choosing the Right Chart

| Data Type                      | Recommended Chart |
| ------------------------------ | ----------------- |
| Time Series                    | Line Chart        |
| Category Comparison            | Bar Chart         |
| Distribution                   | Histogram         |
| Outlier Detection              | Box Plot          |
| Relationship Between Variables | Scatter Plot      |
| Percentage Contribution        | Pie Chart         |
| Large Scatter Dataset          | Hexbin Plot       |
| Distribution Curve             | Density Plot      |

---

# Business Example

A banking institution analyzes customer data.

Visualizations include:

* Histogram of customer ages.
* Box plot of account balances.
* Scatter plot of income vs loan amount.
* Pie chart of account types.
* KDE plot of monthly spending.

These charts help analysts understand customer behavior and identify unusual patterns.

---

# Best Practices

✔ Select charts based on the type of data.

✔ Label axes clearly.

✔ Add descriptive titles.

✔ Keep charts uncluttered.

✔ Use subplots for dashboard-style reporting.

---

# Common Mistakes

### Using Pie Charts with Too Many Categories

Pie charts become difficult to interpret with many slices.

If there are more than five or six categories, consider using a bar chart.

---

### Ignoring Outliers

Always inspect box plots before drawing conclusions from averages.

---

### Overlapping Labels

Rotate labels when necessary.

```python id="mistake01"
rot=45
```

to improve readability.

---

# Quick Recap

You have now learned how to:

* Create histograms.
* Analyze distributions with box plots.
* Explore relationships using scatter plots.
* Display proportions with pie charts.
* Visualize density distributions.
* Handle large datasets using hexbin plots.
* Build dashboards using subplots.
* Customize Pandas visualizations.

> **"Effective visualizations transform raw numbers into intuitive stories, helping analysts uncover trends, relationships, and opportunities that might otherwise remain hidden."**
