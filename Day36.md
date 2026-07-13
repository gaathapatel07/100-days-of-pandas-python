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

# 16. Dashboard Design Principles

Business dashboards should answer important questions quickly.

A good dashboard should:

* Highlight key performance indicators (KPIs)
* Compare performance over time
* Reveal trends and patterns
* Support business decisions

Example dashboard layout:

```text id="dashboard01"
+---------------------------------------------+
|            Monthly Sales Dashboard          |
+---------------------------------------------+
| Revenue | Profit | Orders | Customers       |
+---------------------------------------------+
| Line Chart: Sales Trend                     |
+---------------------------------------------+
| Bar Chart: Regional Sales                   |
+---------------------------------------------+
| Pie Chart: Category Contribution            |
+---------------------------------------------+
| Box Plot: Sales Distribution                |
+---------------------------------------------+
```

Keep dashboards simple and focused.

---

# 17. Enterprise Visualization Workflow

Professional organizations follow a structured reporting workflow.

```text id="workflow01"
Raw Data
     │
     ▼
Data Cleaning
     │
     ▼
Aggregation
     │
     ▼
Feature Engineering
     │
     ▼
Visualization
     │
     ▼
Business Dashboard
     │
     ▼
Executive Decision
```

Visualization is the bridge between analysis and decision-making.

---

# 18. Performance Optimization for Plotting

Large datasets can make charts slow.

### Aggregate Before Plotting

Instead of plotting millions of rows:

```python id="perf01"
df.plot(
    x="Date",
    y="Sales"
)
```

Aggregate first.

```python id="perf02"
monthly = (
    df.groupby("Month")["Sales"]
      .sum()
)

monthly.plot()
```

---

### Select Required Columns

```python id="perf03"
df[
    [
        "Sales",
        "Profit"
    ]
].plot()
```

Avoid plotting unnecessary columns.

---

### Sample Very Large Data

```python id="perf04"
sample = (
    df.sample(
        n=10000,
        random_state=42
    )
)

sample.plot(
    x="Sales",
    y="Profit",
    kind="scatter"
)
```

Sampling often preserves overall patterns while improving performance.

---

# 19. Common Visualization Mistakes

### Using the Wrong Chart

| Goal         | Recommended Chart          |
| ------------ | -------------------------- |
| Trend        | Line Chart                 |
| Comparison   | Bar Chart                  |
| Distribution | Histogram                  |
| Relationship | Scatter Plot               |
| Composition  | Pie Chart (few categories) |

---

### Too Many Colors

Limit colors to emphasize important information.

Avoid distracting color palettes that reduce readability.

---

### Missing Labels

Always include:

* Chart title
* X-axis label
* Y-axis label
* Legend (when necessary)

---

### Misleading Axis Scales

Be cautious when changing axis limits, as truncated scales can exaggerate or minimize differences.

Ensure scales accurately represent the data.

---

# 20. Enterprise Case Study

## Scenario

You are a **Senior Business Analyst** at **RetailHub**.

The executive team wants a monthly dashboard.

Available data:

* Sales
* Profit
* Orders
* Customers
* Product Category
* Region

---

## Business Questions

### Question 1

Monthly sales trend.

```python id="case01"
monthly = (
    df.groupby("Month")["Sales"]
      .sum()
)

monthly.plot(
    kind="line"
)
```

---

### Question 2

Regional sales.

```python id="case02"
df.groupby("Region")["Sales"]\
  .sum()\
  .plot(kind="bar")
```

---

### Question 3

Revenue contribution.

```python id="case03"
df.groupby("Category")["Revenue"]\
  .sum()\
  .plot(
      kind="pie",
      autopct="%1.1f%%"
  )
```

---

### Question 4

Sales distribution.

```python id="case04"
df["Sales"].plot(
    kind="hist",
    bins=20
)
```

---

### Question 5

Sales vs Profit.

```python id="case05"
df.plot(
    x="Sales",
    y="Profit",
    kind="scatter"
)
```

---

# 21. Business Insights

After creating the dashboard, analysts identify:

* Revenue consistently increases during the final quarter.
* The North region generates the highest sales.
* A small number of product categories contribute most of the revenue.
* Sales distribution is right-skewed, indicating a few high-value transactions.
* Higher sales generally correspond to higher profit, though some high-sales orders have relatively low profitability.

---

# 22. Practice Exercises

## Beginner

1. Create a line chart.
2. Create a bar chart.
3. Plot a histogram.
4. Create a box plot.
5. Create a pie chart.

---

## Intermediate

6. Compare two variables with a scatter plot.
7. Create multiple subplots.
8. Customize titles and axis labels.
9. Plot grouped sales by region.
10. Build a simple dashboard.

---

## Advanced

11. Visualize time-series data.
12. Build a business reporting dashboard.
13. Compare multiple KPIs in one report.
14. Optimize plotting for a large dataset.
15. Design an executive-level visualization workflow.

---

# 23. Interview Questions

## Beginner

1. Why is visualization important?
2. When should you use a line chart?
3. What does a histogram show?
4. When is a box plot useful?
5. What information does a scatter plot provide?

---

## Intermediate

6. How do you customize Pandas plots?
7. Why are subplots useful?
8. How do you visualize time-series data?
9. What is the purpose of KDE plots?
10. When should you use a hexbin plot?

---

## Advanced

11. Design an executive dashboard for retail analytics.
12. How do you choose the right chart for a business problem?
13. How would you visualize millions of records efficiently?
14. Explain common visualization mistakes and how to avoid them.
15. Compare Pandas plotting with specialized visualization libraries.

---

# 24. Cheat Sheet

| Task           | Syntax                 |
| -------------- | ---------------------- |
| Line Chart     | `plot(kind="line")`    |
| Bar Chart      | `plot(kind="bar")`     |
| Horizontal Bar | `plot(kind="barh")`    |
| Area Chart     | `plot(kind="area")`    |
| Histogram      | `plot(kind="hist")`    |
| Box Plot       | `plot(kind="box")`     |
| Scatter Plot   | `plot(kind="scatter")` |
| Pie Chart      | `plot(kind="pie")`     |
| Density Plot   | `plot(kind="density")` |
| Hexbin Plot    | `plot(kind="hexbin")`  |
| Subplots       | `subplots=True`        |

---

# 25. Mini Project

## Executive Sales Dashboard

Using any retail, banking, healthcare, telecom, HR, or logistics dataset:

Complete the following tasks:

* Create a line chart for sales trends.
* Compare regional performance with a bar chart.
* Show category contribution using a pie chart.
* Analyze distributions with a histogram and box plot.
* Explore relationships using a scatter plot.
* Build a dashboard with multiple subplots.
* Customize titles, labels, and legends.
* Optimize plotting for large datasets.
* Write **five executive-level business insights**.
* Recommend **three visualization improvements** for future reporting.

### Example Business Insights

* Sales show consistent year-over-year growth.
* The North region contributes the highest revenue.
* A small number of product categories generate the majority of sales.
* Most transactions fall within a moderate sales range, with a few high-value outliers.
* Strong positive correlation exists between sales and profit, though exceptions indicate opportunities for margin improvement.

---

# 26. Summary

Congratulations! 🎉

Today you mastered **Advanced Visualization with Pandas**.

You learned how to:

* Create line, bar, area, histogram, box, scatter, pie, density, and hexbin plots.
* Customize charts for clarity and readability.
* Build dashboards using subplots.
* Choose appropriate chart types for different business questions.
* Optimize plotting for large datasets.

These skills are essential for exploratory data analysis (EDA), executive reporting, business intelligence, and communicating analytical findings.

---

# 27. What's Next?

In **Day 37**, you'll learn **Advanced Exploratory Data Analysis (EDA) with Pandas**.

Topics include:

* Systematic EDA workflow
* Univariate analysis
* Bivariate analysis
* Multivariate analysis
* Correlation analysis
* Missing data analysis
* Outlier analysis
* Business-focused EDA
* Automated profiling
* Generating actionable insights

Mastering EDA will help you discover patterns, detect anomalies, and prepare datasets for visualization, statistical analysis, and machine learning.

---

<div align="center">

# 🎉 Day 36 Complete!

You've mastered **Advanced Visualization with Pandas**, giving you the ability to transform complex datasets into clear, actionable visual stories.

By combining efficient analysis with effective visualization, you're now equipped to communicate insights that support data-driven decision-making.

⭐ **Next → Day 37: Advanced Exploratory Data Analysis (EDA) with Pandas** 🔍📊🐼

</div>
