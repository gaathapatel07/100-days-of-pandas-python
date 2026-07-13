
# Day 38 — Advanced Data Aggregation & Business Analytics

<div align="center">

# 100 Days of Pandas

### Day 38 · Transforming Raw Data into Business Intelligence

*"Aggregation transforms millions of records into meaningful business insights that drive strategic decisions."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Data%20Aggregation-blue)
![Day](https://img.shields.io/badge/Day-38-orange)

</div>

---

# Table of Contents

1. Introduction
2. What is Data Aggregation?
3. Why Aggregation Matters
4. Learning Objectives
5. Advanced GroupBy
6. Multi-Level Grouping
7. Multiple Aggregations
8. Summary

---

# 1. Introduction

Organizations collect massive amounts of raw data every day.

Examples include:

* Customer transactions
* Website visits
* Banking records
* Hospital admissions
* Manufacturing logs
* Sales invoices

Raw records are useful, but decision-makers usually require summarized information.

Data aggregation transforms detailed records into concise business metrics.

---

# 2. What is Data Aggregation?

Aggregation combines multiple observations into meaningful summaries.

Example:

| Region | Sales |
| ------ | ----: |
| North  |  5000 |
| North  |  6200 |
| South  |  4800 |
| South  |  7100 |

Aggregate by region.

```python id="agg01"
df.groupby("Region")["Sales"].sum()
```

Output

| Region | Total Sales |
| ------ | ----------: |
| North  |       11200 |
| South  |       11900 |

---

# 3. Why Aggregation Matters

Businesses frequently ask questions such as:

* Which region generated the highest revenue?
* Which products are most profitable?
* What is the average order value?
* Which month achieved the highest sales?
* Which department performs best?

Aggregation provides direct answers to these questions.

---

# 4. Learning Objectives

By the end of this lesson, you will be able to:

* Perform advanced grouping.
* Aggregate multiple metrics.
* Group by multiple columns.
* Create business summaries.
* Generate KPI reports.

---

# 5. Advanced GroupBy

Group by a single column.

```python id="group01"
df.groupby(
    "Region"
)["Sales"].sum()
```

Average sales.

```python id="group02"
df.groupby(
    "Region"
)["Sales"].mean()
```

Maximum sales.

```python id="group03"
df.groupby(
    "Region"
)["Sales"].max()
```

Minimum sales.

```python id="group04"
df.groupby(
    "Region"
)["Sales"].min()
```

Count observations.

```python id="group05"
df.groupby(
    "Region"
)["Sales"].count()
```

---

# 6. Multi-Level Grouping

Group by multiple columns.

```python id="multi01"
df.groupby(
    [
        "Region",
        "Category"
    ]
)["Sales"].sum()
```

Example Output

| Region | Category   | Sales |
| ------ | ---------- | ----: |
| North  | Furniture  | 52000 |
| North  | Technology | 61000 |
| South  | Furniture  | 47000 |
| South  | Technology | 59000 |

This creates a hierarchical summary.

---

## Reset Index

Convert the grouped result into a regular DataFrame.

```python id="multi02"
summary = (
    df.groupby(
        [
            "Region",
            "Category"
        ]
    )["Sales"]
     .sum()
     .reset_index()
)
```

---

# 7. Multiple Aggregations

Businesses usually require multiple statistics simultaneously.

```python id="multiagg01"
df.groupby(
    "Region"
)["Sales"].agg(
    [
        "sum",
        "mean",
        "max",
        "min",
        "count"
    ]
)
```

Output

| Region |    Sum | Mean |   Max |  Min | Count |
| ------ | -----: | ---: | ----: | ---: | ----: |
| North  | 520000 | 6200 |  9800 | 1200 |    84 |
| South  | 610000 | 6400 | 10200 | 1400 |    95 |

---

## Named Aggregations

Assign meaningful names to aggregated columns.

```python id="multiagg02"
summary = (
    df.groupby("Region")
      .agg(
          Total_Sales=("Sales", "sum"),
          Average_Sales=("Sales", "mean"),
          Highest_Sale=("Sales", "max"),
          Orders=("Sales", "count")
      )
)
```

Output

| Region | Total_Sales | Average_Sales | Highest_Sale | Orders |
| ------ | ----------: | ------------: | -----------: | -----: |
| North  |      520000 |          6200 |         9800 |     84 |
| South  |      610000 |          6400 |        10200 |     95 |

Named aggregations make reports easier to read.

---

# Business Example

A supermarket chain wants to evaluate regional performance.

Analysts calculate:

* Total revenue by region.
* Average order value.
* Highest transaction.
* Number of orders.
* Performance by region and product category.

These summaries are shared with regional managers to monitor business performance.

---

# Best Practices

✔ Choose aggregation functions based on the business question.

✔ Use named aggregations for readable reports.

✔ Reset indexes when exporting grouped results.

✔ Validate aggregated values before reporting.

✔ Keep grouped outputs well organized.

---

# Common Mistakes

### Forgetting to Reset the Index

Grouped results often have grouped columns as the index.

Use:

```python id="mistake01"
.reset_index()
```

when you need a regular DataFrame.

---

### Applying the Wrong Aggregation

For example, calculating the average of customer IDs has little business meaning.

Always select aggregation functions that match the metric.

---

### Ignoring Missing Values

Some aggregation functions skip missing values by default.

Review the dataset beforehand to ensure summaries accurately reflect the underlying data.

---

# Key Takeaways

After completing this section, you should understand:

* How `groupby()` creates summaries.
* How to group by multiple columns.
* How to calculate multiple statistics at once.
* How named aggregations improve readability.
* Why aggregation is central to business analytics.

> **"Aggregation transforms detailed operational data into concise business intelligence, enabling organizations to monitor performance, identify trends, and make informed decisions."**

# 8. Custom Aggregation Functions

Sometimes built-in aggregation functions (`sum`, `mean`, `max`) are not enough.

You can create custom functions.

Example:

Calculate the range of sales.

```python id="custom01"
def sales_range(series):
    return (
        series.max()
        -
        series.min()
    )

df.groupby(
    "Region"
)["Sales"].agg(
    sales_range
)
```

Output

| Region | Sales Range |
| ------ | ----------: |
| North  |        8600 |
| South  |        9200 |

---

## Multiple Custom Functions

```python id="custom02"
summary = (
    df.groupby("Region")
      .agg(
          Total=("Sales","sum"),
          Average=("Sales","mean"),
          Range=("Sales",sales_range)
      )
)
```

---

# 9. Advanced Pivot Tables

Pivot tables summarize data across multiple dimensions.

Example:

```python id="pivot01"
pd.pivot_table(
    df,
    values="Sales",
    index="Region",
    columns="Category",
    aggfunc="sum"
)
```

Output

| Region | Furniture | Technology |
| ------ | --------: | ---------: |
| North  |    520000 |     610000 |
| South  |    470000 |     590000 |

---

## Multiple Aggregations

```python id="pivot02"
pd.pivot_table(
    df,
    values="Sales",
    index="Region",
    columns="Category",
    aggfunc=[
        "sum",
        "mean"
    ]
)
```

---

## Fill Missing Values

```python id="pivot03"
pd.pivot_table(
    df,
    values="Sales",
    index="Region",
    columns="Category",
    aggfunc="sum",
    fill_value=0
)
```

---

# 10. Crosstab Analysis

Crosstabs compare categorical variables.

Example:

```python id="cross01"
pd.crosstab(
    df["Region"],
    df["Category"]
)
```

Output

| Region | Furniture | Technology |
| ------ | --------- | ---------: |
| North  | 35        |         49 |
| South  | 28        |         67 |

---

## Normalize Crosstab

```python id="cross02"
pd.crosstab(
    df["Region"],
    df["Category"],
    normalize="index"
)
```

Shows percentages instead of counts.

---

# 11. Cohort Analysis

Cohort analysis groups customers based on a shared characteristic, such as their first purchase month.

Example:

```python id="cohort01"
df["Order Month"] = (
    df["Order Date"]
      .dt.to_period("M")
)

cohort = (
    df.groupby("Order Month")
      ["Customer ID"]
      .nunique()
)
```

Output

| Order Month | Customers |
| ----------- | --------: |
| 2026-01     |       520 |
| 2026-02     |       610 |
| 2026-03     |       590 |

Cohort analysis is widely used for customer retention studies.

---

# 12. Customer Segmentation

Identify customer value.

Revenue by customer.

```python id="segment01"
customer_sales = (
    df.groupby(
        "Customer"
    )["Revenue"]
      .sum()
)
```

Segment using quartiles.

```python id="segment02"
tiers = pd.qcut(
    customer_sales,
    q=4,
    labels=[
        "Bronze",
        "Silver",
        "Gold",
        "Platinum"
    ]
)
```

Example

| Customer | Tier     |
| -------- | -------- |
| Alice    | Platinum |
| Rahul    | Gold     |
| Priya    | Silver   |
| Aman     | Bronze   |

---

# 13. Business KPI Calculations

Common KPIs include:

---

## Total Revenue

```python id="kpi01"
total_revenue = (
    df["Revenue"]
      .sum()
)
```

---

## Average Order Value

```python id="kpi02"
avg_order = (
    df["Revenue"]
      .mean()
)
```

---

## Profit Margin

```python id="kpi03"
profit_margin = (
    (
        df["Profit"].sum()
        /
        df["Revenue"].sum()
    )
    * 100
)
```

---

## Customer Count

```python id="kpi04"
customers = (
    df["Customer ID"]
      .nunique()
)
```

---

# 14. Funnel Analysis

Businesses often track customer progression through different stages.

Example:

| Stage         | Users |
| ------------- | ----: |
| Website Visit | 10000 |
| Product View  |  6200 |
| Add to Cart   |  2400 |
| Checkout      |  1800 |
| Purchase      |  1450 |

Represent as a DataFrame.

```python id="funnel01"
funnel = pd.DataFrame({

    "Stage":[
        "Visit",
        "View",
        "Cart",
        "Checkout",
        "Purchase"
    ],

    "Users":[
        10000,
        6200,
        2400,
        1800,
        1450
    ]
})
```

Calculate conversion rate.

```python id="funnel02"
funnel["Conversion %"] = (
    funnel["Users"]
    /
    funnel.loc[0,"Users"]
) * 100
```

Output

| Stage    | Conversion % |
| -------- | -----------: |
| Visit    |          100 |
| View     |           62 |
| Cart     |           24 |
| Checkout |           18 |
| Purchase |         14.5 |

---

# 15. Sales Performance Analytics

Top-performing regions.

```python id="sales01"
df.groupby(
    "Region"
)["Revenue"]\
.sum()\
.sort_values(
    ascending=False
)
```

Top-performing products.

```python id="sales02"
df.groupby(
    "Product"
)["Revenue"]\
.sum()\
.nlargest(10)
```

Monthly sales.

```python id="sales03"
df.groupby(
    "Month"
)["Revenue"]\
.sum()
```

These summaries support executive reporting and business planning.

---

# Business Example

An e-commerce company wants to understand:

* Which regions generate the most revenue?
* Which customer segments are most valuable?
* Where customers drop off during checkout?
* Which product categories perform best?
* How monthly sales change over time?

Using GroupBy, Pivot Tables, Crosstabs, KPI calculations, and Funnel Analysis, analysts create executive reports that guide pricing, inventory planning, and marketing campaigns.

---

# Best Practices

✔ Use pivot tables for multidimensional summaries.

✔ Use crosstabs for categorical comparisons.

✔ Calculate KPIs consistently across reporting periods.

✔ Segment customers using meaningful business metrics.

✔ Validate aggregated results before presenting them.

---

# Common Mistakes

### Using Too Many Dimensions in Pivot Tables

Adding excessive index and column levels can make reports difficult to read.

Keep pivot tables focused on the business question.

---

### Ignoring Customer Segmentation

Analyzing all customers together may hide important behavioral differences.

Segmenting customers often reveals valuable insights.

---

### Reporting KPIs Without Context

A revenue figure alone provides limited information.

Compare KPIs across regions, products, or time periods to add meaningful context.

---

# Quick Recap

You have now learned how to:

* Build custom aggregation functions.
* Create advanced pivot tables.
* Perform crosstab analysis.
* Conduct basic cohort analysis.
* Segment customers.
* Calculate business KPIs.
* Perform funnel analysis.
* Analyze sales performance.

> **"Business analytics transforms aggregated data into strategic insights, helping organizations understand customers, measure performance, and make informed decisions."**
# 16. Enterprise Business Analytics Workflow

Organizations typically follow a structured analytics workflow.

```text id="workflow01"
Raw Business Data
        │
        ▼
Data Cleaning
        │
        ▼
Data Validation
        │
        ▼
Feature Engineering
        │
        ▼
Aggregation
        │
        ▼
Business KPIs
        │
        ▼
Visualization
        │
        ▼
Executive Dashboard
        │
        ▼
Strategic Decision Making
```

Each stage transforms operational data into actionable business intelligence.

---

# 17. Automated KPI Dashboard

Instead of manually calculating KPIs each time, create a reusable function.

```python id="dashboard01"
def business_kpis(df):

    report = {

        "Total Revenue":
        df["Revenue"].sum(),

        "Total Profit":
        df["Profit"].sum(),

        "Average Order Value":
        df["Revenue"].mean(),

        "Unique Customers":
        df["Customer ID"].nunique(),

        "Total Orders":
        len(df)
    }

    return pd.DataFrame(
        report.items(),
        columns=[
            "KPI",
            "Value"
        ]
    )
```

Run the dashboard.

```python id="dashboard02"
kpi_report = business_kpis(df)

print(kpi_report)
```

Example Output

| KPI                 |      Value |
| ------------------- | ---------: |
| Total Revenue       | ₹8,750,000 |
| Total Profit        | ₹1,430,000 |
| Average Order Value |     ₹4,920 |
| Unique Customers    |      2,140 |
| Total Orders        |      1,780 |

---

# 18. Executive Reporting Framework

Executive reports should answer business questions quickly.

Recommended structure:

```text id="framework01"
Executive Summary

↓

Revenue Analysis

↓

Profit Analysis

↓

Regional Performance

↓

Customer Segmentation

↓

Product Performance

↓

Recommendations
```

Executives prefer concise, actionable reports rather than raw tables.

---

# 19. Performance Optimization

Business reports often aggregate millions of rows.

### Aggregate Before Visualization

```python id="perf01"
monthly_sales = (
    df.groupby("Month")
      ["Revenue"]
      .sum()
)
```

Visualize the aggregated result rather than the full dataset.

---

### Cache Frequently Used KPIs

Instead of recalculating repeatedly:

```python id="perf02"
total_revenue = (
    df["Revenue"].sum()
)
```

Store the value and reuse it throughout the report.

---

### Aggregate Only Required Columns

```python id="perf03"
summary = (
    df.groupby("Region")
      [["Revenue","Profit"]]
      .sum()
)
```

Reducing unnecessary calculations improves performance.

---

# 20. Enterprise Case Study

## Scenario

You are a **Senior Business Analyst** at **RetailHub**.

Management requests a quarterly executive report.

Available data:

* Orders
* Revenue
* Profit
* Region
* Product Category
* Customer
* Discounts

---

## Business Questions

### Question 1

Calculate revenue by region.

```python id="case01"
df.groupby(
    "Region"
)["Revenue"]\
.sum()
```

---

### Question 2

Identify the top five customers.

```python id="case02"
df.groupby(
    "Customer"
)["Revenue"]\
.sum()\
.nlargest(5)
```

---

### Question 3

Calculate profit margin.

```python id="case03"
(
    df["Profit"].sum()
    /
    df["Revenue"].sum()
) * 100
```

---

### Question 4

Create a pivot table.

```python id="case04"
pd.pivot_table(
    df,
    values="Revenue",
    index="Region",
    columns="Category",
    aggfunc="sum"
)
```

---

### Question 5

Generate the KPI dashboard.

```python id="case05"
business_kpis(df)
```

---

# 21. Business Insights

After completing the analysis, the team identifies:

* Revenue is concentrated in two high-performing regions.
* Premium customers contribute a significant share of total revenue.
* Certain product categories consistently achieve higher profit margins.
* Funnel analysis reveals the largest customer drop-off occurs between adding items to the cart and completing checkout.
* Quarterly KPI reports enable management to make faster, evidence-based decisions.

---

# 22. Practice Exercises

## Beginner

1. Group sales by region.
2. Calculate average profit.
3. Count customers by category.
4. Create a simple pivot table.
5. Calculate total revenue.

---

## Intermediate

6. Build a crosstab.
7. Calculate multiple aggregations.
8. Create customer spending tiers.
9. Calculate profit margin.
10. Build a KPI summary table.

---

## Advanced

11. Build an executive KPI dashboard.
12. Perform cohort analysis.
13. Analyze funnel conversion rates.
14. Create an automated business reporting function.
15. Prepare a quarterly executive report.

---

# 23. Interview Questions

## Beginner

1. What is data aggregation?
2. What is `groupby()`?
3. What is a pivot table?
4. What is a KPI?
5. What is a crosstab?

---

## Intermediate

6. Difference between `groupby()` and `pivot_table()`?
7. What is cohort analysis?
8. How do you segment customers?
9. Explain funnel analysis.
10. How do you calculate profit margin?

---

## Advanced

11. Design an executive reporting workflow.
12. Explain customer lifetime value segmentation.
13. How would you build a business dashboard using Pandas?
14. Optimize aggregation on a dataset with millions of rows.
15. Describe how business analytics supports executive decision-making.

---

# 24. Cheat Sheet

| Task                  | Syntax             |
| --------------------- | ------------------ |
| GroupBy               | `groupby()`        |
| Sum                   | `.sum()`           |
| Mean                  | `.mean()`          |
| Count                 | `.count()`         |
| Multiple Aggregation  | `.agg()`           |
| Pivot Table           | `pd.pivot_table()` |
| Crosstab              | `pd.crosstab()`    |
| Top N Values          | `.nlargest()`      |
| Quartile Segmentation | `pd.qcut()`        |
| Unique Count          | `.nunique()`       |

---

# 25. Mini Project

## Executive Retail Business Analytics Dashboard

Using any retail, banking, healthcare, telecom, logistics, HR, or e-commerce dataset:

Complete the following tasks:

* Aggregate revenue by region and category.
* Build advanced pivot tables.
* Create crosstab reports.
* Calculate executive KPIs.
* Segment customers into spending tiers.
* Perform a simple cohort analysis.
* Analyze the customer purchase funnel.
* Generate an automated KPI dashboard.
* Write **five executive-level business insights**.
* Recommend **three business strategies** based on the analysis.

### Example Business Insights

* A small proportion of customers generate a large share of revenue.
* Premium product categories consistently achieve higher margins.
* Customer conversion decreases significantly at the checkout stage.
* Regional sales performance varies considerably, suggesting localized marketing opportunities.
* KPI dashboards allow management to monitor performance more efficiently.

---

# 26. Summary

Congratulations! 🎉

Today you mastered **Advanced Data Aggregation & Business Analytics**.

You learned how to:

* Perform advanced `groupby()` operations.
* Apply multiple and custom aggregations.
* Build pivot tables and crosstabs.
* Conduct basic cohort and funnel analyses.
* Segment customers.
* Calculate business KPIs.
* Build executive reporting workflows.

These skills are widely used in business intelligence, product analytics, financial reporting, executive dashboards, and strategic decision-making.

---

# 27. What's Next?

In **Day 39**, you'll learn **Advanced Missing Data Handling & Data Cleaning Strategies**.

Topics include:

* Advanced missing value analysis
* Missing data mechanisms (MCAR, MAR, MNAR)
* Imputation techniques
* Interpolation
* Forward & backward filling
* Conditional imputation
* Duplicate handling strategies
* Text standardization
* Data consistency checks
* Production-ready cleaning pipelines

These techniques are essential for preparing reliable datasets for analytics, reporting, and machine learning.

---

<div align="center">

# Day 38 Complete!

You've mastered **Advanced Data Aggregation & Business Analytics**, enabling you to convert operational data into executive-ready reports and actionable business insights.

By combining aggregation, KPIs, segmentation, and reporting, you've developed one of the most valuable skill sets for Data Analysts and BI professionals.


</div>
