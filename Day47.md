# Day 47 — Advanced Data Visualization with Pandas & Matplotlib

## Introduction

Data visualization is the graphical representation of data that helps analysts understand patterns, trends, relationships, and outliers quickly.

While tables contain raw information, visualizations communicate insights more effectively to stakeholders, managers, and executives.

Visualization is an essential skill in:

- Data Analytics
- Business Intelligence
- Data Science
- Financial Analysis
- Marketing Analytics
- Operations Analytics

---

# Topics Covered

- Introduction to Data Visualization
- Why Visualization Matters
- Line Charts
- Bar Charts
- Histograms
- Box Plots
- Scatter Plots
- Pie Charts
- Area Charts
- Customizing Charts

---

# 1. What is Data Visualization?

Data visualization converts numerical information into graphical representations.

Instead of:

| Month | Sales |
|--------|-------|
|Jan|1500|
|Feb|1700|
|Mar|1900|

A line chart immediately shows the upward trend.

Visualization helps answer questions such as:

- Are sales increasing?
- Which region performs best?
- Are there seasonal trends?
- Which products generate the highest revenue?

---

# 2. Why Data Visualization Matters

Imagine an executive reviewing 50,000 rows of sales data.

A simple dashboard can instantly show:

- Revenue trends
- Top-performing products
- Regional sales
- Customer growth

Visualizations make complex data easier to understand.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

- Create common business charts.
- Customize visualizations.
- Choose appropriate chart types.
- Identify trends and outliers.
- Communicate insights visually.

---

# 4. Line Charts

Line charts display trends over time.

```python
import matplotlib.pyplot as plt

df.plot(

    x="Month",

    y="Sales",

    kind="line"

)

plt.show()
```

Applications:

- Monthly sales
- Stock prices
- Website traffic
- Temperature trends

---

## Multiple Lines

```python
df.plot(

    x="Month",

    y=["Sales", "Profit"],

    kind="line"

)

plt.show()
```

Useful for comparing metrics.

---

# 5. Bar Charts

Bar charts compare categories.

```python
df.plot(

    x="Region",

    y="Revenue",

    kind="bar"

)

plt.show()
```

Applications:

- Revenue by region
- Sales by category
- Employee performance
- Department expenses

---

## Horizontal Bar Chart

```python
df.plot(

    x="Region",

    y="Revenue",

    kind="barh"

)

plt.show()
```

---

# 6. Histograms

Histograms display numerical distributions.

```python
df["Sales"].plot(

    kind="hist",

    bins=20

)

plt.show()
```

Applications:

- Customer age distribution
- Salary distribution
- Product prices

---

# 7. Box Plots

Box plots identify outliers.

```python
df.plot(

    column="Sales",

    kind="box"

)

plt.show()
```

Shows:

- Median
- Quartiles
- Outliers

Widely used during EDA.

---

# 8. Scatter Plots

Scatter plots visualize relationships between two numerical variables.

```python
df.plot(

    x="Revenue",

    y="Profit",

    kind="scatter"

)

plt.show()
```

Applications:

- Revenue vs Profit
- Height vs Weight
- Advertising vs Sales

---

# 9. Pie Charts

Pie charts show proportions.

```python
df.set_index("Region")["Revenue"].plot(

    kind="pie",

    autopct="%1.1f%%"

)

plt.ylabel("")

plt.show()
```

Best for:

- Market share
- Budget allocation
- Revenue contribution

Use pie charts only when there are a small number of categories.

---

# 10. Area Charts

Area charts show cumulative trends.

```python
df.plot(

    x="Month",

    y="Sales",

    kind="area"

)

plt.show()
```

Applications:

- Cumulative revenue
- Population growth
- Website visitors

---

# 11. Customizing Charts

Add title.

```python
plt.title("Monthly Sales")
```

Label axes.

```python
plt.xlabel("Month")

plt.ylabel("Revenue")
```

Add grid.

```python
plt.grid(True)
```

Adjust figure size.

```python
plt.figure(figsize=(10,5))
```

Rotate labels.

```python
plt.xticks(rotation=45)
```

---

# Business Example

A retail company creates visualizations for executives.

Charts include:

- Monthly revenue trend
- Sales by region
- Product category revenue
- Customer age distribution
- Profit vs Revenue
- Market share by region

These charts help executives make informed business decisions.

---

# Best Practices

✔ Choose the right chart type.

✔ Label axes clearly.

✔ Add informative titles.

✔ Avoid unnecessary clutter.

✔ Keep visualizations simple and readable.

---

# Common Mistakes

### Using Pie Charts for Too Many Categories

Pie charts become difficult to interpret with many slices.

---

### Missing Labels

Always include axis labels and chart titles.

---

### Overloading Charts

Avoid displaying too many variables in a single chart.

---

# Key Takeaways

After completing this section, you should understand:

- Line charts
- Bar charts
- Histograms
- Box plots
- Scatter plots
- Pie charts
- Area charts
- Basic chart customization

> **"Effective visualizations transform raw data into compelling stories that support faster and better business decisions."**

---



The next section covers:

- Subplots
- Correlation Heatmaps
- Time-Series Visualization
- Grouped Charts
- Stacked Charts
- Business Dashboard Visualizations
- Advanced Chart Customization
- Visualization Best Practices

# 12. Subplots

Subplots allow multiple charts to be displayed in a single figure.

```python
df[["Sales", "Profit"]].plot(

    subplots=True,

    figsize=(10,6)

)

plt.show()
```

Applications:

- KPI dashboards
- Comparing multiple metrics
- Financial reports

---

# 13. Correlation Heatmaps

Correlation heatmaps help visualize relationships between numerical variables.

First calculate the correlation matrix.

```python
corr = df.corr(numeric_only=True)

print(corr)
```

Using Matplotlib:

```python
plt.figure(figsize=(8,6))

plt.imshow(corr, cmap="coolwarm")

plt.colorbar()

plt.xticks(

    range(len(corr.columns)),

    corr.columns,

    rotation=90

)

plt.yticks(

    range(len(corr.columns)),

    corr.columns

)

plt.title("Correlation Matrix")

plt.show()
```

Interpretation:

- Dark Red → Strong Positive Correlation
- Dark Blue → Strong Negative Correlation
- Light Colors → Weak Correlation

---

# 14. Time-Series Visualization

Visualize trends over time.

```python
df["Order Date"] = pd.to_datetime(
    df["Order Date"]
)

df.plot(

    x="Order Date",

    y="Revenue",

    kind="line",

    figsize=(10,5)

)

plt.show()
```

Monthly trend.

```python
monthly = (

    df.groupby(

        df["Order Date"].dt.month

    )["Revenue"]

      .sum()

)

monthly.plot()

plt.show()
```

Applications:

- Sales forecasting
- Stock prices
- Website traffic
- Energy consumption

---

# 15. Grouped Bar Charts

Compare multiple variables across categories.

```python
pivot = pd.pivot_table(

    df,

    values="Revenue",

    index="Region",

    columns="Category",

    aggfunc="sum"

)

pivot.plot(

    kind="bar"

)

plt.show()
```

Applications:

- Revenue by region and category
- Department performance
- Quarterly comparisons

---

# 16. Stacked Bar Charts

Display cumulative contributions.

```python
pivot.plot(

    kind="bar",

    stacked=True

)

plt.show()
```

Applications:

- Product contribution
- Expense breakdown
- Revenue composition

---

# 17. Business Dashboard Visualizations

Typical executive dashboard includes:

- Monthly Revenue Trend
- Revenue by Region
- Top Products
- Customer Distribution
- Profit Trend
- Sales by Category

Example:

```python
fig, ax = plt.subplots(

    2,

    2,

    figsize=(12,8)

)
```

Each subplot can display one KPI chart.

---

# 18. Advanced Chart Customization

Change line style.

```python
df.plot(

    y="Sales",

    linestyle="--"

)

plt.show()
```

Change marker.

```python
df.plot(

    y="Sales",

    marker="o"

)

plt.show()
```

Transparency.

```python
df.plot(

    y="Sales",

    alpha=0.7

)

plt.show()
```

Legend location.

```python
plt.legend(

    loc="upper left"

)
```

Save chart.

```python
plt.savefig(

    "sales_chart.png",

    dpi=300,

    bbox_inches="tight"

)
```

---

# 19. Choosing the Right Chart

| Business Question | Recommended Chart |
|-------------------|-------------------|
| Trend over time | Line Chart |
| Compare categories | Bar Chart |
| Distribution | Histogram |
| Detect outliers | Box Plot |
| Relationship | Scatter Plot |
| Percentage contribution | Pie Chart |
| Cumulative values | Area Chart |
| Correlation | Heatmap |
| Multiple KPIs | Subplots |
| Category composition | Stacked Bar Chart |

---

# 20. Business Example

A retail company prepares an executive dashboard.

Charts include:

- Monthly sales trend
- Revenue by region
- Profit by category
- Customer age distribution
- Revenue vs Profit
- Quarterly revenue comparison
- Product contribution
- Correlation heatmap

Executives quickly identify:

- Best-performing regions
- Seasonal trends
- High-profit products
- Customer purchasing behavior

---

# Best Practices

✔ Choose the appropriate visualization.

✔ Keep colors consistent.

✔ Label every chart clearly.

✔ Highlight key insights.

✔ Avoid unnecessary chart decorations.

---

# Common Mistakes

### Using the Wrong Chart

For example, using a pie chart for 20 categories reduces readability.

---

### Ignoring Scale

Improper axis scaling can misrepresent trends.

---

### Overcrowding Dashboards

Limit dashboards to the most important KPIs.

---

# Quick Recap

You have now learned how to:

- Create subplots.
- Build correlation heatmaps.
- Visualize time-series data.
- Create grouped and stacked bar charts.
- Customize charts professionally.
- Design business dashboards.

> **"A good visualization does more than display data—it communicates insights clearly, enabling faster and better decision-making."**

---


The final section will cover:

- Enterprise Dashboard Workflow
- Automated Visualization Pipelines
- Production Best Practices
- Interview Questions (20+)
- Practice Exercises
- Cheat Sheet
- Mini Project
- Executive Business Insights
- Complete Day 47 Summary

# 21. Enterprise Dashboard Workflow

Professional organizations follow a structured workflow to transform raw data into interactive dashboards and visual reports.

```
Raw Dataset
      │
      ▼
Data Collection
      │
      ▼
Data Cleaning
      │
      ▼
Exploratory Data Analysis
      │
      ▼
KPI Selection
      │
      ▼
Chart Selection
      │
      ▼
Dashboard Design
      │
      ▼
Insight Generation
      │
      ▼
Business Presentation
```

A well-designed workflow ensures dashboards are accurate, informative, and easy for stakeholders to interpret.

---

# 22. Automated Visualization Pipeline

Instead of writing plotting code repeatedly, create reusable visualization functions.

```python
import matplotlib.pyplot as plt

def plot_sales_trend(df):

    plt.figure(figsize=(10,5))

    plt.plot(df["Month"], df["Sales"], marker="o")

    plt.title("Monthly Sales")

    plt.xlabel("Month")

    plt.ylabel("Sales")

    plt.grid(True)

    plt.show()
```

Use the function:

```python
plot_sales_trend(df)
```

Benefits:

- Reusable
- Consistent formatting
- Easier maintenance
- Faster reporting

---

# 23. Production Best Practices

### Understand the Audience

Executives prefer high-level KPIs, while analysts often require detailed visualizations.

---

### Keep Charts Simple

Remove unnecessary elements that distract from the main message.

---

### Use Consistent Colors

Maintain the same color scheme across dashboards for better readability.

---

### Highlight Key Insights

Use annotations or titles to draw attention to important findings.

---

### Label Everything

Always include:

- Chart title
- Axis labels
- Legends (when required)

---

### Avoid Misleading Visuals

Use appropriate scales and chart types to represent data accurately.

---

# 24. Enterprise Case Study

## Scenario

A national retail company wants an executive dashboard for quarterly performance.

The dashboard should answer:

- Which region generated the highest sales?
- Which product category is most profitable?
- How has monthly revenue changed?
- Which customers contribute the most revenue?
- What seasonal trends exist?

---

## Monthly Revenue Trend

```python
monthly = (

    df.groupby(

        df["Order Date"].dt.month

    )["Revenue"]

      .sum()

)

monthly.plot(

    kind="line",

    marker="o"

)

plt.show()
```

---

## Revenue by Region

```python
df.groupby(

    "Region"

)["Revenue"]

.sum()

.plot(

    kind="bar"

)

plt.show()
```

---

## Profit by Category

```python
df.groupby(

    "Category"

)["Profit"]

.sum()

.plot(

    kind="barh"

)

plt.show()
```

---

## Revenue Distribution

```python
df["Revenue"].plot(

    kind="hist",

    bins=25

)

plt.show()
```

---

## Revenue vs Profit

```python
df.plot(

    x="Revenue",

    y="Profit",

    kind="scatter"

)

plt.show()
```

---

# 25. Executive Business Insights

After analyzing the dashboard, management concludes:

- The West region contributes the highest revenue.
- Electronics products generate the largest profit.
- Sales increase significantly during festive seasons.
- A small percentage of customers account for a large share of revenue.
- Revenue and profit show a strong positive relationship.
- Customer purchases are concentrated in a few product categories.
- Most monthly revenue growth occurs during the final quarter.

These insights support strategic planning, inventory management, and marketing decisions.

---

# 26. Practice Exercises

## Beginner

1. Create a line chart.
2. Create a bar chart.
3. Plot a histogram.
4. Build a scatter plot.
5. Draw a box plot.

---

## Intermediate

6. Create grouped bar charts.
7. Create stacked bar charts.
8. Plot monthly revenue trends.
9. Create a correlation heatmap.
10. Build multiple subplots.

---

## Advanced

11. Build an executive dashboard.
12. Create reusable plotting functions.
13. Generate KPI visualizations.
14. Compare business regions visually.
15. Design a complete reporting dashboard.

---

# 27. Interview Questions

## Beginner

1. What is data visualization?
2. Why are visualizations important?
3. When should you use a line chart?
4. What is a histogram?
5. What does a scatter plot show?

---

## Intermediate

6. Explain box plots.
7. What are grouped bar charts?
8. What are stacked charts?
9. How do you visualize time-series data?
10. What is a correlation heatmap?

---

## Advanced

11. How do you design an executive dashboard?
12. Explain dashboard KPIs.
13. How do you choose the correct visualization?
14. How do you improve dashboard readability?
15. What visualization mistakes should analysts avoid?

---

# 28. Cheat Sheet

| Task | Syntax |
|------|--------|
| Line Chart | `plot(kind="line")` |
| Bar Chart | `plot(kind="bar")` |
| Horizontal Bar | `plot(kind="barh")` |
| Histogram | `plot(kind="hist")` |
| Box Plot | `plot(kind="box")` |
| Scatter Plot | `plot(kind="scatter")` |
| Pie Chart | `plot(kind="pie")` |
| Area Chart | `plot(kind="area")` |
| Subplots | `plot(subplots=True)` |
| Save Figure | `plt.savefig()` |
| Grid | `plt.grid(True)` |
| Title | `plt.title()` |
| Labels | `plt.xlabel(), plt.ylabel()` |

---

# 29. Mini Project

## Retail Sales Dashboard

Using any retail, banking, healthcare, HR, finance, or telecom dataset:

Build a dashboard containing:

- Monthly Revenue Trend
- Sales by Region
- Revenue by Product Category
- Profit Distribution
- Customer Distribution
- Revenue vs Profit Scatter Plot
- Correlation Heatmap
- Executive KPI Summary

Finally, write:

- Five business insights
- Three strategic recommendations

### Example Insights

- West region consistently generates the highest revenue.
- Electronics contribute over 40% of total profit.
- Revenue peaks during the festive season.
- Premium customers contribute a disproportionate share of sales.
- Sales and profit have a strong positive correlation.

---

# 30. Summary

Congratulations! 🎉

Today you mastered **Advanced Data Visualization with Pandas & Matplotlib**.

You learned how to:

- Create line, bar, histogram, box, scatter, pie, and area charts.
- Build grouped and stacked visualizations.
- Create subplots and correlation heatmaps.
- Visualize time-series data.
- Design business dashboards.
- Build reusable visualization pipelines.
- Apply production-level visualization best practices.
- Present business insights effectively.

These skills are essential for Data Analytics, Business Intelligence, and Data Science, where communicating insights clearly is just as important as analyzing the data.

---

# 31. What's Next?

## Day 48 — Time Series Analysis with Pandas

Topics include:

- Date & Time Fundamentals
- Working with `datetime`
- Date Ranges
- Time Indexing
- Resampling
- Rolling Windows
- Expanding Windows
- Lag Features
- Time-Based Grouping
- Forecasting Preparation
- Business Time-Series Analysis

Time-series analysis is widely used in finance, sales forecasting, weather prediction, healthcare, and business analytics.

---

# Day 47 Complete!

You have successfully completed **Advanced Data Visualization with Pandas & Matplotlib**.

You can now create professional charts, dashboards, and executive reports that communicate data-driven insights effectively.

