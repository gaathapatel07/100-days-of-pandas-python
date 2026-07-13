
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
