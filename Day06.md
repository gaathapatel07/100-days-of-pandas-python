#  Day 06 — Data Visualization with Matplotlib

<div align="center">

# 100 Days of Pandas

### Day 06 · Transforming Data into Meaningful Visualizations

*"A single well-designed visualization can communicate insights more effectively than thousands of rows of data."*

![Difficulty](https://img.shields.io/badge/Difficulty-Intermediate-yellow)
![Topic](https://img.shields.io/badge/Topic-Data%20Visualization-blue)
![Day](https://img.shields.io/badge/Day-06-orange)

</div>

---

# Table of Contents

1. Introduction
2. Why Data Visualization Matters
3. Learning Objectives
4. What is Matplotlib?
5. Installing Matplotlib
6. Your First Plot
7. Understanding Figure & Axes
8. Anatomy of a Chart
9. Best Practices
10. Summary

---

# 1. Introduction

Numbers alone rarely tell the complete story.

Imagine receiving a spreadsheet containing **250,000 rows of sales data**. Although every transaction is recorded accurately, identifying trends, seasonal patterns, or unusual behavior simply by reading the numbers would be nearly impossible.

Data visualization bridges this gap by transforming numerical information into visual representations that are easier to understand and interpret.

As humans, we naturally recognize shapes, colors, trends, and patterns much faster than tables of numbers. This makes visualization one of the most valuable skills for analysts, researchers, and business professionals.

In this chapter, you'll begin learning **Matplotlib**, the most widely used Python library for creating static visualizations.

---

# 2. Why Data Visualization Matters

Suppose a retail company wants answers to the following questions:

* Which month generated the highest revenue?
* Which products perform best?
* Which region contributes the most sales?
* Are profits increasing over time?
* Are there any unusual spikes in customer purchases?

Finding these answers using raw tables would be difficult.

However, visualizations make these insights immediately obvious.

Instead of reading thousands of rows, analysts use charts to:

* Discover trends
* Compare categories
* Detect anomalies
* Understand distributions
* Communicate findings to stakeholders

Visualization transforms raw data into actionable business intelligence.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Explain the importance of data visualization.
* Create your first chart using Matplotlib.
* Understand the relationship between Figures and Axes.
* Customize chart titles and labels.
* Apply visualization best practices.
* Select the appropriate chart for different business questions.

---

# 4. What is Matplotlib?

Matplotlib is an open-source Python library used for creating static, interactive, and publication-quality visualizations.

It was introduced by **John D. Hunter** in 2003 and has since become one of the most popular visualization libraries in Python.

Matplotlib supports a wide variety of chart types, including:

* Line Charts
* Bar Charts
* Histograms
* Pie Charts
* Scatter Plots
* Box Plots
* Area Charts
* Heatmaps (with additional libraries)

Because many other visualization libraries are built on top of Matplotlib, understanding its fundamentals provides a strong foundation for advanced analytics.

---

# 5. Installing Matplotlib

Install Matplotlib using pip:

```bash
pip install matplotlib
```

Import the library:

```python
import matplotlib.pyplot as plt
```

The alias `plt` is the community standard and is used throughout this series.

---

# 6. Your First Plot

Let's create a simple line chart.

```python
import matplotlib.pyplot as plt

months = ["Jan", "Feb", "Mar", "Apr", "May"]

sales = [120, 150, 170, 160, 210]

plt.plot(months, sales)

plt.show()
```

This produces a basic line chart representing monthly sales.

Although simple, it already communicates much more effectively than a table.

---

# 7. Understanding Figure & Axes

Every Matplotlib visualization consists of two fundamental components.

## Figure

The **Figure** represents the entire chart or canvas.

Think of it as the sheet of paper on which the visualization is drawn.

---

## Axes

The **Axes** represent the plotting area where the data is displayed.

A figure may contain one or multiple axes.

Diagram:

```text
+--------------------------------------+
|               Figure                 |
|                                      |
|   +------------------------------+   |
|   |            Axes              |   |
|   |                              |   |
|   |        Your Plot Here        |   |
|   |                              |   |
|   +------------------------------+   |
|                                      |
+--------------------------------------+
```

Understanding this distinction becomes important when creating multiple charts within a single figure.

---

# 8. Anatomy of a Chart

Every professional chart contains several key elements.

```text
                Chart Title

       ↑
       |
Y Axis |           ●
       |      ●
       |   ●
       |______________________________
              X Axis
```

A good chart should always include:

* A descriptive title
* Clearly labeled axes
* Appropriate scale
* Readable values
* Minimal visual clutter

---

# 9. Best Practices

✔ Choose the right chart for your data.

✔ Keep charts simple and uncluttered.

✔ Label axes clearly.

✔ Use descriptive titles.

✔ Avoid unnecessary decorative elements.

✔ Ensure charts answer a business question.

Remember:

> **A chart should communicate information—not decoration.**

---

# Key Takeaways

After completing this section, you should understand:

* Why visualization is essential in data analysis.
* The role of Matplotlib.
* How to install and import Matplotlib.
* The structure of Figures and Axes.
* The anatomy of an effective chart.
* Visualization best practices.

> **"The purpose of a visualization is not to impress with complexity, but to communicate information with clarity."**

# 10. Line Chart

A **Line Chart** is used to display trends or changes over time.

It is one of the most common visualizations in business analytics because it clearly shows whether a value is increasing, decreasing, or remaining stable.

### Best Used For

* Monthly Sales
* Daily Website Visitors
* Temperature Changes
* Stock Prices
* Revenue Growth

---

## Example

```python
import matplotlib.pyplot as plt

months = ["Jan", "Feb", "Mar", "Apr", "May", "Jun"]
sales = [120, 145, 160, 155, 180, 210]

plt.figure(figsize=(8,5))
plt.plot(months, sales)

plt.title("Monthly Sales")
plt.xlabel("Month")
plt.ylabel("Sales")

plt.show()
```

---

## Business Interpretation

The chart immediately reveals:

* Overall sales trend
* Peak sales month
* Sales decline (if any)
* Seasonal patterns

Instead of reading six numbers individually, the trend becomes obvious.

---

# 11. Bar Chart

A **Bar Chart** compares values across different categories.

Each bar represents a category, and its height corresponds to the value.

### Best Used For

* Sales by Region
* Profit by Category
* Students by Grade
* Revenue by Product
* Customers by City

---

## Example

```python
regions = ["North","South","East","West"]

sales = [520,410,630,580]

plt.figure(figsize=(8,5))
plt.bar(regions, sales)

plt.title("Sales by Region")
plt.xlabel("Region")
plt.ylabel("Sales")

plt.show()
```

---

## Business Interpretation

A manager can quickly identify:

* Best-performing region
* Lowest-performing region
* Sales comparison across locations

---

# 12. Histogram

A **Histogram** shows how numerical data is distributed.

Unlike bar charts, histograms group continuous values into intervals called **bins**.

### Best Used For

* Salary Distribution
* Customer Age Distribution
* Product Price Distribution
* Daily Website Visitors

---

## Example

```python
ages = [18,21,20,19,24,25,30,31,32,35,36,38,40]

plt.figure(figsize=(8,5))

plt.hist(ages,bins=5)

plt.title("Customer Age Distribution")
plt.xlabel("Age")
plt.ylabel("Frequency")

plt.show()
```

---

## Business Interpretation

A histogram helps answer:

* Which age group is most common?
* Is the data normally distributed?
* Are there unusually high or low values?

---

# 13. Scatter Plot

Scatter plots show the relationship between two numerical variables.

Each point represents one observation.

### Best Used For

* Sales vs Profit
* Height vs Weight
* Advertising Budget vs Revenue
* Study Hours vs Marks

---

## Example

```python
sales = [10,20,30,40,50]

profit = [2,5,7,9,11]

plt.figure(figsize=(8,5))

plt.scatter(sales, profit)

plt.title("Sales vs Profit")
plt.xlabel("Sales")
plt.ylabel("Profit")

plt.show()
```

---

## Business Interpretation

Scatter plots help identify:

* Positive relationships
* Negative relationships
* Clusters
* Outliers

---

# 14. Pie Chart

Pie charts display the proportion of different categories within a whole.

### Best Used For

* Market Share
* Revenue Contribution
* Product Category Distribution

Use pie charts only when showing **parts of a whole**.

---

## Example

```python
labels = ["Electronics","Furniture","Clothing"]

sales = [45,30,25]

plt.figure(figsize=(6,6))

plt.pie(
    sales,
    labels=labels,
    autopct="%1.1f%%"
)

plt.title("Revenue by Category")

plt.show()
```

---

## Business Interpretation

Managers can instantly understand:

* Which category contributes the most revenue.
* Percentage contribution of each category.
* Overall business composition.

---

# 15. Box Plot

A **Box Plot** summarizes the distribution of numerical data and highlights potential outliers.

### Best Used For

* Salary Analysis
* Exam Scores
* Delivery Time
* Product Prices

---

## Example

```python
salary = [
42000,
45000,
47000,
50000,
52000,
55000,
58000,
61000,
150000
]

plt.figure(figsize=(6,5))

plt.boxplot(salary)

plt.title("Employee Salary Distribution")

plt.show()
```

---

## Business Interpretation

A box plot quickly shows:

* Median
* Quartiles
* Spread of data
* Potential outliers

This makes it one of the most useful charts during Exploratory Data Analysis.

---

# 16. Choosing the Right Chart

Selecting the correct chart is just as important as creating one.

| Business Question | Recommended Chart |
| ----------------- | ----------------- |
| Sales over time   | Line Chart        |
| Compare regions   | Bar Chart         |
| Customer ages     | Histogram         |
| Sales vs Profit   | Scatter Plot      |
| Market share      | Pie Chart         |
| Detect outliers   | Box Plot          |

Choosing the wrong visualization can hide important insights or mislead the audience.

---

# Best Practices

✔ Keep titles short but descriptive.

✔ Always label the X-axis and Y-axis.

✔ Avoid unnecessary colors or decorations.

✔ Choose the visualization that best answers the business question.

✔ Focus on clarity rather than complexity.

---

# Quick Recap

By now, you should be able to:

* Create a Line Chart.
* Build a Bar Chart.
* Understand Histograms.
* Use Scatter Plots for relationships.
* Represent proportions using Pie Charts.
* Detect outliers using Box Plots.

> **"The best visualization is the one that answers a business question clearly, accurately, and with the least amount of effort from the viewer."**
