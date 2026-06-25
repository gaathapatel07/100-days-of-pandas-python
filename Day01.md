# Day 01 - Loading & Exploring Your First Dataset

## Objective

Learn how to load a dataset into Pandas and perform basic exploration to understand its structure before analysis.

---

## Dataset

Use any CSV dataset. Recommended:
- Titanic Dataset
- Sample Superstore Dataset
- Iris Dataset

Place the dataset in your project folder.

Example:

```
Day01/
│── data/
│   └── titanic.csv
│── Day01.ipynb
└── Day01.md
```

---

## Import Pandas

```python
import pandas as pd
```

---

## Load the Dataset

```python
df = pd.read_csv("data/titanic.csv")
```

---

## View the First 5 Rows

```python
df.head()
```

---

## View the Last 5 Rows

```python
df.tail()
```

---

## Find the Dataset Shape

```python
df.shape
```

Example Output:

```
(891, 12)
```

Meaning:
- 891 rows
- 12 columns

---

## Display Column Names

```python
df.columns
```

---

## Check Data Types

```python
df.dtypes
```

---

## Display Dataset Information

```python
df.info()
```

This shows:
- Number of rows
- Number of columns
- Missing values
- Data types
- Memory usage

---

## Generate Summary Statistics

```python
df.describe()
```

Useful statistics include:
- Count
- Mean
- Standard Deviation
- Minimum
- Maximum
- Quartiles

---

## Find Missing Values

```python
df.isnull().sum()
```

---

## Practice Questions

1. How many rows are in the dataset?
2. How many columns are present?
3. Which column contains the most missing values?
4. Which columns are numerical?
5. Which columns are categorical?
6. What is the average value of each numerical column?
7. Which column has the highest maximum value?

---

## Key Functions Learned

| Function | Purpose |
|----------|---------|
| `pd.read_csv()` | Load a CSV file |
| `head()` | Display first 5 rows |
| `tail()` | Display last 5 rows |
| `shape` | Get dataset dimensions |
| `columns` | List all column names |
| `dtypes` | View data types |
| `info()` | Display dataset information |
| `describe()` | Generate summary statistics |
| `isnull().sum()` | Count missing values |

---

## What I Learned

- How to load a dataset using Pandas.
- How to inspect the structure of a DataFrame.
- How to identify data types.
- How to detect missing values.
- How to generate descriptive statistics.
- Why data exploration is the first step in every data analysis project.

---

## Challenge

Without looking at the documentation, answer these questions using only Pandas:

- Total number of rows?
- Total number of columns?
- Which column has the most missing values?
- How many numerical columns are there?
- How many categorical columns are there?

---
