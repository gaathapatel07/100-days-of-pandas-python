# Day 01 — Introduction to Pandas & Data Exploration

<div align="center">

# 100 Days of Pandas

### Day 01 · Building Your Foundation in Data Analysis

*"Before uncovering insights, you must first understand your data."*

![Difficulty](https://img.shields.io/badge/Difficulty-Beginner-success)
![Topic](https://img.shields.io/badge/Topic-Pandas-blue)
![Day](https://img.shields.io/badge/Day-01-orange)

</div>

---

# Table of Contents

1. Introduction
2. Why Learn Pandas?
3. Learning Objectives
4. Prerequisites
5. What is Data?
6. Types of Data
7. Data Analysis Lifecycle
8. What is Pandas?
9. Why Companies Use Pandas
10. Installing Pandas
11. Your First Pandas Program
12. Summary

---


# 1. Introduction

Welcome to **Day 01** of the **100 Days of Pandas** challenge.

Pandas is one of the most widely used Python libraries for working with structured data. Whether you're analyzing sales, cleaning customer records, preparing datasets for machine learning, or building business dashboards, Pandas is often the first tool you'll use.

This series is designed to take you from the fundamentals to advanced techniques through small, consistent lessons. Instead of memorizing functions, you'll learn how to think like a data analyst by working with practical examples and real-world datasets.

---

# 2. Why Learn Pandas?

Modern organizations generate enormous amounts of data every day. Turning that raw data into useful information requires tools that are both powerful and easy to use. Pandas provides exactly that.

With Pandas, you can:

* Load datasets from CSV, Excel, SQL databases, JSON, and many other formats.
* Explore large datasets within seconds.
* Clean messy or incomplete data.
* Filter, sort, and transform information efficiently.
* Calculate statistics and summarize data.
* Prepare datasets for visualization and machine learning.

### Real-World Applications

Pandas is commonly used for:

*  Sales Analysis
*  Financial Reporting
*  Healthcare Analytics
*  E-commerce Analytics
*  Social Media Analytics
*  Supply Chain Optimization
*  Streaming Platform Analysis
*  Machine Learning Preprocessing

---

# 3. Learning Objectives

By the end of Day 01, you will be able to:

* Explain what Pandas is and why it is used.
* Install and import the Pandas library.
* Understand the concept of structured data.
* Differentiate between a Series and a DataFrame.
* Load datasets into Python.
* Perform an initial inspection of a dataset.
* Recognize the importance of data exploration before analysis.

---

# 4. Prerequisites

Before starting this lesson, ensure you have:

* Basic knowledge of Python
* Python 3.x installed
* VS Code or Jupyter Notebook
* Internet connection (for downloading datasets)

If you are completely new to Python, it is recommended that you first understand variables, lists, dictionaries, and functions before learning Pandas.

---

# 5. What is Data?

Data is a collection of raw facts, observations, or measurements that can be processed to produce meaningful information.

Examples of raw data include:

| Customer | Age | City   | Purchase Amount |
| -------- | --: | ------ | --------------: |
| Alice    |  24 | Mumbai |            2500 |
| Rahul    |  31 | Delhi  |            1800 |
| Sarah    |  28 | Pune   |            3200 |

At this stage, the table simply contains facts. Through analysis, these facts can answer business questions such as:

* Which city generates the highest revenue?
* What is the average customer age?
* Which customers spend the most?

The process of converting raw data into useful insights is called **data analysis**.

---

# 6. Types of Data

Understanding different types of data is essential because the way data is stored determines how it should be analyzed.

## 6.1 Structured Data

Structured data follows a fixed format and is usually stored in tables.

Example:

| Product  | Price | Quantity |
| -------- | ----: | -------: |
| Laptop   | 65000 |       12 |
| Mouse    |   900 |       45 |
| Keyboard |  1800 |       20 |

Examples include:

* CSV files
* Excel spreadsheets
* SQL databases

This is the type of data Pandas is primarily designed to work with.

---

## 6.2 Semi-Structured Data

Semi-structured data does not follow a strict tabular format but still contains an organized structure.

Examples include:

* JSON
* XML
* HTML

These formats are common when working with APIs and web applications.

---

## 6.3 Unstructured Data

Unstructured data has no predefined format.

Examples include:

* Images
* Videos
* Audio files
* Emails
* Social media posts
* PDF documents

Analyzing unstructured data often requires additional tools beyond Pandas.

---

# 7. Data Analysis Lifecycle

Every data analytics project follows a similar workflow.

```text
Collect Data
      │
      ▼
Load Data
      │
      ▼
Explore Data
      │
      ▼
Clean Data
      │
      ▼
Analyze Data
      │
      ▼
Visualize Results
      │
      ▼
Business Decisions
```

In this series, we will gradually learn each stage of this workflow.

Today, our focus is on the **Load Data** and **Explore Data** stages.

---

## Key Takeaways

* Pandas is the most popular Python library for structured data analysis.
* Data analysis begins with understanding the dataset, not building models.
* Structured data is the primary format used in business analytics.
* Every successful project starts with exploring and understanding the available data before making decisions.

---

### Coming Up Next

In the next section, we'll dive deeper into **What is Pandas?**, explore its history, understand why it became the industry standard, and create our first Pandas program.
