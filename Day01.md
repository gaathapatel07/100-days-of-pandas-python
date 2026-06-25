# Day 01 · Getting Started with Pandas — Data Loading & Exploration

<div align="center">

# 🐼 100 Days of Pandas

### Day 01 · Data Loading & Initial Exploration

*Every great analysis begins with understanding the data.*

---

**Skills Covered**

`read_csv()` • `head()` • `tail()` • `shape` • `columns` • `dtypes` • `info()` • `describe()` • `isnull()`

**Difficulty:** 🟢 Beginner

</div>

---

# Overview

Before building dashboards, training machine learning models, or creating business reports, analysts first answer one fundamental question:

> **"What does my data actually look like?"**

Today's challenge focuses on loading a dataset and performing an initial exploratory inspection using Pandas.

By the end of this lesson, you'll be able to confidently inspect any CSV dataset and understand its structure within minutes.

---

# Business Scenario

Imagine you've just joined an e-commerce company as a Junior Data Analyst.

On your first day, your manager shares a CSV file containing customer transactions and asks you to:

* Understand the dataset structure
* Identify missing values
* Check data quality
* Summarize key statistics

Before answering any business questions, you must first explore the data.

---

# Dataset

You may use any CSV dataset.

Recommended datasets:

* Titanic
* Iris
* Superstore Sales
* Netflix Titles
* Spotify Tracks

Project Structure

```text
Day-01/
│
├── README.md
├── solution.ipynb
├── data/
│   └── dataset.csv
└── assets/
```

---

# Learning Objectives

After completing this lesson, you should be able to:

* Load datasets using Pandas
* Inspect rows and columns
* Understand dataset dimensions
* Identify numerical and categorical columns
* Detect missing values
* Generate descriptive statistics
* Evaluate dataset quality before analysis

---

# Functions Covered

| Function         | Description                         |
| ---------------- | ----------------------------------- |
| `pd.read_csv()`  | Load CSV data into a DataFrame      |
| `head()`         | Display the first five rows         |
| `tail()`         | Display the last five rows          |
| `shape`          | Return dataset dimensions           |
| `columns`        | List all column names               |
| `dtypes`         | Display each column's data type     |
| `info()`         | Provide dataset summary             |
| `describe()`     | Generate descriptive statistics     |
| `isnull().sum()` | Count missing values in each column |

---

# Challenge

Using only Pandas, answer the following questions:

### Dataset Overview

* How many rows are present?
* How many columns are present?
* What are the column names?
* What are the data types?

### Data Quality

* Which columns contain missing values?
* Which column has the highest number of missing values?
* Are there duplicate records?

### Statistical Summary

* Which numerical column has the highest average?
* Which numerical column has the largest maximum value?
* Which column has the highest standard deviation?

---

# Expected Workflow

```text
Load Dataset
      │
      ▼
Preview Data
      │
      ▼
Inspect Structure
      │
      ▼
Understand Data Types
      │
      ▼
Check Missing Values
      │
      ▼
Generate Summary Statistics
      │
      ▼
Draw Initial Observations
```

---

# Key Takeaways

By completing Day 01, you have learned how to:

* Import the Pandas library
* Load CSV datasets into DataFrames
* Inspect dataset structure efficiently
* Identify data types and missing values
* Generate statistical summaries
* Build a strong foundation for Exploratory Data Analysis (EDA)

---

# Practice Challenge

Without referring to documentation, complete the following tasks:

* Load a CSV dataset.
* Display the first and last five rows.
* Print the dataset dimensions.
* Display all column names.
* Generate descriptive statistics.
* Identify missing values.
* Count duplicate rows.
* Save your cleaned notebook.

---

# What's Next?

➡ **Day 02 — Selecting, Filtering & Indexing Data with Pandas**

We'll learn how to extract specific rows and columns, apply conditions, and answer business questions using data filtering.

---

<div align="center">

### ⭐ If you found this helpful, consider starring the repository!

**Next Stop → Day 02**

</div>

