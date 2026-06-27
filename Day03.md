
#  Day 03 — Sorting, Renaming & Transforming Data in Pandas

<div align="center">

# 100 Days of Pandas

### Day 03 · Organizing and Transforming Data Like a Data Analyst

*"Clean, well-structured data is the foundation of every meaningful analysis."*

![Difficulty](https://img.shields.io/badge/Difficulty-Beginner-success)
![Topic](https://img.shields.io/badge/Topic-Data%20Transformation-blue)
![Day](https://img.shields.io/badge/Day-03-orange)

</div>

---

#  Table of Contents

1. Introduction
2. Why Data Transformation Matters
3. Learning Objectives
4. Sorting Data
5. Sorting Multiple Columns
6. Ascending vs Descending Order
7. Renaming Columns
8. Creating New Columns
9. Updating Existing Columns
10. Dropping Columns
11. Setting & Resetting Index
12. Common Mistakes
13. Business Case Study
14. Practice Exercises
15. Interview Questions
16. Cheat Sheet
17. Mini Project
18. Summary
19. What's Next?

---

# 1. Introduction

In the previous lessons, you learned how to load datasets, inspect their structure, and retrieve specific rows and columns using powerful indexing and filtering techniques.

However, real-world datasets are rarely ready for analysis immediately. Before building reports or creating visualizations, analysts often need to reorganize, rename, and transform their data into a cleaner and more meaningful format.

Data transformation improves readability, simplifies analysis, and prepares datasets for advanced operations such as aggregation, visualization, and machine learning.

Today's lesson focuses on organizing data efficiently using sorting, renaming, creating new columns, and working with indexes.

---

# 2. Why Data Transformation Matters

Imagine you're working for an online retail company with a dataset containing thousands of customer orders.

Your manager asks you to answer questions like:

* Which products generated the highest revenue?
* Which customers placed the largest orders?
* Can you calculate the total amount after applying discounts?
* Can you rename technical column names before sharing the report with management?

These tasks require more than just selecting rows—they require transforming the dataset into a format that is easier to understand and analyze.

This is where Pandas becomes an incredibly powerful tool.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Sort datasets using one or multiple columns.
* Arrange data in ascending and descending order.
* Rename columns for better readability.
* Create new columns using existing data.
* Update existing column values.
* Remove unnecessary columns.
* Set and reset indexes effectively.
* Prepare datasets for deeper analysis.

---

# 4. Sorting Data

Sorting helps arrange records in a meaningful order, making it easier to identify trends, top performers, and outliers.

Suppose we have the following employee dataset:

| Employee | Department | Salary |
| -------- | ---------- | -----: |
| Alice    | HR         |  50000 |
| Rahul    | Sales      |  62000 |
| Emma     | IT         |  71000 |
| David    | Finance    |  68000 |
| Sophia   | IT         |  85000 |

To sort employees by salary in ascending order:

```python
df.sort_values(by="Salary")
```

Output:

| Employee | Salary |
| -------- | -----: |
| Alice    |  50000 |
| Rahul    |  62000 |
| David    |  68000 |
| Emma     |  71000 |
| Sophia   |  85000 |

By default, Pandas sorts values in **ascending order**.

---

# 5. Sorting in Descending Order

To display the highest values first, use the `ascending=False` parameter.

```python
df.sort_values(by="Salary", ascending=False)
```

Output:

| Employee | Salary |
| -------- | -----: |
| Sophia   |  85000 |
| Emma     |  71000 |
| David    |  68000 |
| Rahul    |  62000 |
| Alice    |  50000 |

Descending order is commonly used to identify:

* Top-selling products
* Highest-paid employees
* Best-performing regions
* Most profitable customers

---

# 6. Sorting by Multiple Columns

Sometimes a single column isn't enough.

Suppose you want to sort employees by:

1. Department
2. Salary (Highest First)

```python
df.sort_values(
    by=["Department", "Salary"],
    ascending=[True, False]
)
```

Pandas first groups records by department and then sorts salaries within each department.

This technique is frequently used in business reporting and dashboard preparation.

---

# Key Takeaways

* Sorting organizes data into meaningful order.
* `sort_values()` is one of the most frequently used Pandas functions.
* Multiple-column sorting enables more advanced analysis.
* Understanding sorting is essential before performing grouping, aggregation, or visualization.

> **"Well-organized data makes patterns easier to discover and decisions easier to make."**

---

# 7. Renaming Columns

As datasets grow larger, column names often become difficult to understand. Many datasets contain abbreviations, inconsistent naming conventions, or spaces that make analysis less intuitive.

Renaming columns improves readability and helps create cleaner, more maintainable code.

Consider the following dataset:

| Cust_ID | Cust_Name | Sales_Amt |
| ------- | --------- | --------: |
| 101     | Alice     |      2500 |
| 102     | Rahul     |      1800 |
| 103     | Emma      |      3200 |

Although these column names are valid, they are not very descriptive.

To rename them:

```python
df.rename(columns={
    "Cust_ID": "Customer ID",
    "Cust_Name": "Customer Name",
    "Sales_Amt": "Sales Amount"
})
```

### Renaming a Single Column

```python
df.rename(columns={
    "Sales_Amt": "Sales"
})
```

---

### Saving the Changes

By default, `rename()` does **not** modify the original DataFrame.

```python
new_df = df.rename(columns={
    "Sales_Amt": "Sales"
})
```

or

```python
df.rename(
    columns={"Sales_Amt": "Sales"},
    inplace=True
)
```

---

### Best Practices

✔ Use meaningful column names.

✔ Avoid unnecessary abbreviations.

✔ Maintain consistent naming throughout the project.

✔ Prefer lowercase or snake_case when building production pipelines.

Example:

Instead of

```
Customer Name
```

consider

```
customer_name
```

---

# 8. Creating New Columns

One of the most powerful features of Pandas is the ability to create new columns from existing data.

Suppose your dataset contains:

| Product  | Price | Quantity |
| -------- | ----: | -------: |
| Laptop   | 60000 |        2 |
| Mouse    |   800 |        5 |
| Keyboard |  1800 |        3 |

You can calculate the total amount for each order.

```python
df["Total Amount"] = df["Price"] * df["Quantity"]
```

Output:

| Product  | Price | Quantity | Total Amount |
| -------- | ----: | -------: | -----------: |
| Laptop   | 60000 |        2 |       120000 |
| Mouse    |   800 |        5 |         4000 |
| Keyboard |  1800 |        3 |         5400 |

Creating calculated columns is one of the most common tasks in business analytics.

---

### Creating Columns Using Conditions

Suppose you want to classify employees based on salary.

```python
df["High Salary"] = df["Salary"] > 70000
```

Output:

| Employee | Salary | High Salary |
| -------- | -----: | ----------- |
| Alice    |  50000 | False       |
| Rahul    |  62000 | False       |
| Emma     |  71000 | True        |
| Sophia   |  85000 | True        |

---

# 9. Updating Existing Columns

Sometimes the data already exists but needs modification.

Example:

Increase every employee's salary by 10%.

```python
df["Salary"] = df["Salary"] * 1.10
```

Or increase by a fixed amount.

```python
df["Salary"] = df["Salary"] + 5000
```

Since DataFrames are mutable, assigning a new value to an existing column replaces the old values.

---

### Updating Text Columns

Convert all employee names to uppercase.

```python
df["Employee"] = df["Employee"].str.upper()
```

Convert names to lowercase.

```python
df["Employee"] = df["Employee"].str.lower()
```

Capitalize names.

```python
df["Employee"] = df["Employee"].str.title()
```

---

# 10. Dropping Columns

Many datasets contain columns that are unnecessary for analysis.

Removing such columns simplifies the dataset and improves readability.

Suppose your dataset contains:

* Customer ID
* Name
* Phone Number
* Address
* Purchase Amount

If the phone number is not required, remove it.

```python
df.drop(columns=["Phone Number"])
```

---

### Removing Multiple Columns

```python
df.drop(columns=[
    "Phone Number",
    "Address"
])
```

---

### Saving the Changes

```python
df.drop(
    columns=["Phone Number"],
    inplace=True
)
```

---

# Common Mistakes

### Forgetting to Save Changes

```python
df.rename(columns={
    "Sales": "Revenue"
})
```

The original DataFrame remains unchanged.

---

Correct:

```python
df.rename(
    columns={"Sales": "Revenue"},
    inplace=True
)
```

---

### Dropping a Non-Existing Column

```python
df.drop(columns=["Age"])
```

If `"Age"` does not exist, Pandas raises a `KeyError`.

Always verify available columns.

```python
df.columns
```

---

# Business Scenario

You are preparing a monthly sales report for senior management.

The raw dataset contains technical column names such as:

```
cust_id
cust_nm
prod_qty
amt
```

Before presenting the report, you:

* Rename columns to meaningful names.
* Remove unnecessary fields.
* Create a **Total Revenue** column.
* Convert text into a consistent format.

These simple transformations make reports easier to understand and reduce confusion for business stakeholders.

---

# Quick Recap

By now, you should be able to:

* Rename columns using `rename()`.
* Create calculated columns.
* Update existing values.
* Apply basic string transformations.
* Remove unnecessary columns.
* Write cleaner and more readable datasets.

> **"Raw data rarely arrives in the perfect format. Transforming it into a clean, meaningful dataset is one of the most valuable skills a data analyst can develop."**

---

# 11. Understanding Indexes in Pandas

Every DataFrame in Pandas has an **index**, which acts as a unique identifier for each row. By default, Pandas assigns a numerical index starting from `0`.

Example:

| Index | Employee | Department | Salary |
| ----: | -------- | ---------- | -----: |
|     0 | Alice    | HR         |  50000 |
|     1 | Rahul    | Sales      |  62000 |
|     2 | Emma     | IT         |  71000 |
|     3 | David    | Finance    |  68000 |

Although the default index works well in many situations, there are times when a more meaningful column—such as **Employee ID**, **Customer ID**, or **Order ID**—should be used as the index.

---

# 12. Setting an Index

The `set_index()` function allows you to replace the default numerical index with one or more columns.

Suppose the dataset contains an `Employee ID` column.

```python
df.set_index("Employee ID")
```

Output:

| Employee ID | Employee | Department | Salary |
| ----------: | -------- | ---------- | -----: |
|         101 | Alice    | HR         |  50000 |
|         102 | Rahul    | Sales      |  62000 |
|         103 | Emma     | IT         |  71000 |

Using meaningful indexes improves readability and makes it easier to retrieve records.

---

### Saving the Changes

```python
df.set_index("Employee ID", inplace=True)
```

---

# 13. Resetting the Index

Sometimes you need to restore the default numerical index.

Use `reset_index()`.

```python
df.reset_index()
```

Output:

| Index | Employee ID | Employee | Department | Salary |
| ----: | ----------: | -------- | ---------- | -----: |
|     0 |         101 | Alice    | HR         |  50000 |
|     1 |         102 | Rahul    | Sales      |  62000 |
|     2 |         103 | Emma     | IT         |  71000 |

To update the original DataFrame:

```python
df.reset_index(inplace=True)
```

---

# 14. Why Indexes Matter

Indexes make data retrieval faster and more organized.

Examples include:

* Looking up customer records using Customer ID.
* Finding products using Product ID.
* Accessing employee information using Employee ID.
* Working with dates in time-series datasets.

Choosing the right index can improve both code readability and analytical workflows.

---

# 15. Real-World Business Case Study

## Scenario

You are working as a Data Analyst for **RetailHub**, an online retail company.

The management team wants a report showing the company's top-performing products and high-value customers.

The raw dataset contains the following columns:

* Order ID
* Customer Name
* Product
* Category
* Quantity
* Price
* Discount
* Region
* Sales

### Tasks

1. Sort products by Sales in descending order.
2. Rename technical column names for better readability.
3. Create a **Revenue** column by multiplying Quantity and Price.
4. Remove unnecessary columns such as Internal Notes.
5. Set **Order ID** as the index.
6. Reset the index before exporting the report.

These transformations prepare the dataset for dashboards and business presentations.

---

# 16. Practice Exercises

## Beginner

1. Sort employees by salary in ascending order.
2. Sort employees by salary in descending order.
3. Rename the **Salary** column to **Monthly Salary**.
4. Create a new column called **Annual Salary**.
5. Drop an unnecessary column.

---

## Intermediate

6. Sort data by Department and Salary.
7. Create a **Bonus** column equal to 10% of Salary.
8. Convert employee names to uppercase.
9. Set Employee ID as the index.
10. Reset the index.

---

## Advanced

11. Sort by multiple columns with different sort orders.
12. Rename multiple columns at once.
13. Remove multiple columns together.
14. Create a new column using two existing columns.
15. Build a clean DataFrame ready for reporting.

---

# 17. Interview Questions

### Beginner

1. What is `sort_values()`?
2. How do you sort data in descending order?
3. Difference between `rename()` and changing `df.columns`?
4. Why do we create new columns?
5. What is an index?

---

### Intermediate

6. Difference between `set_index()` and `reset_index()`?
7. What happens when `inplace=True` is used?
8. Why should meaningful indexes be preferred?
9. Difference between sorting one column and multiple columns?
10. When would you drop a column?

---

### Advanced

11. Why is data transformation important before visualization?
12. Can a DataFrame have multiple indexes?
13. How do indexes improve data retrieval?
14. When should indexes be reset?
15. Explain how transformed datasets improve business decision-making.

---

# 18. Cheat Sheet

| Operation             | Syntax                                         |
| --------------------- | ---------------------------------------------- |
| Sort ascending        | `df.sort_values("Sales")`                      |
| Sort descending       | `df.sort_values("Sales", ascending=False)`     |
| Sort multiple columns | `df.sort_values(["Region","Sales"])`           |
| Rename column         | `df.rename(columns={"Old":"New"})`             |
| Create new column     | `df["Revenue"] = df["Price"] * df["Quantity"]` |
| Update column         | `df["Salary"] *= 1.10`                         |
| Drop column           | `df.drop(columns=["Phone"])`                   |
| Set index             | `df.set_index("Employee ID")`                  |
| Reset index           | `df.reset_index()`                             |

---

# 19. Mini Project

## Employee Performance Report

Using any employee dataset:

Create a clean analytical report by:

* Sorting employees by salary.
* Renaming all technical column names.
* Creating **Annual Salary**.
* Creating a **Bonus** column.
* Removing unnecessary fields.
* Setting Employee ID as the index.
* Resetting the index before exporting.
* Writing **five business insights** based on the transformed data.

---

# 20. Summary

Congratulations! 

Today you learned how to organize and transform datasets using Pandas.

You explored:

* Sorting datasets efficiently.
* Sorting by multiple columns.
* Renaming columns.
* Creating calculated columns.
* Updating existing values.
* Removing unnecessary columns.
* Working with indexes.
* Preparing datasets for business reporting.

These are essential preprocessing techniques used in almost every real-world analytics project.

---

# 21. What's Next?

In **Day 04**, you'll learn one of the most important topics in data analysis:

* Missing Values
* Handling Null Data
* Filling Missing Values
* Removing Missing Values
* Detecting Duplicate Records
* Data Cleaning Fundamentals

Data cleaning is one of the most time-consuming tasks in analytics, and mastering it will significantly improve the quality of your analyses.

---

<div align="center">

##  Day 03 Complete!

You've now built a strong foundation in **loading, exploring, filtering, sorting, transforming, and indexing data with Pandas**.

⭐ If you're enjoying this series, consider starring the repository and following along as we continue toward mastering Pandas in 100 days.



</div>

