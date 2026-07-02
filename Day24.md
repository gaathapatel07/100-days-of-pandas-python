# Day 24 — Categorical Data, Encoding & Memory Optimization

<div align="center">

# 100 Days of Pandas

### Day 24 · Working Efficiently with Categorical Variables

*"Not all data is numerical. Understanding categorical data is essential for building efficient analytical and machine learning workflows."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Categorical%20Data-blue)
![Day](https://img.shields.io/badge/Day-24-orange)

</div>

---

# Table of Contents

1. Introduction
2. Why Categorical Data Matters
3. Learning Objectives
4. Understanding Categorical Data
5. Category Data Type
6. Memory Optimization
7. Category Operations
8. Summary

---

# 1. Introduction

Real-world datasets contain a mixture of numerical and categorical variables.

Examples of categorical data include:

* Gender
* Department
* City
* Product Category
* Payment Method
* Customer Segment
* Education Level
* Blood Group
* Marital Status

Unlike numerical values, categorical variables represent labels or groups rather than measurable quantities.

Pandas provides a dedicated **Category** data type that stores repeated values efficiently and improves analytical performance.

---

# 2. Why Categorical Data Matters

Imagine an e-commerce company with **10 million customer records**.

One column stores customer gender.

Instead of storing:

```text id="cat01"
Male

Female

Male

Female

Male
```

millions of times as strings, Pandas stores each unique category only once and replaces repeated values with compact integer codes.

Benefits include:

* Lower memory consumption
* Faster grouping
* Improved sorting
* Better performance
* Easier machine learning preprocessing

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Understand categorical variables.
* Convert object columns into categories.
* Reduce memory usage.
* Inspect category metadata.
* Perform category-specific operations.

---

# 4. Understanding Categorical Data

Categorical variables describe qualities instead of quantities.

### Examples

| Customer | Gender |
| -------- | ------ |
| Alice    | Female |
| Rahul    | Male   |
| Priya    | Female |

Unlike numerical variables, categories cannot usually be added or averaged.

Categories are divided into two major types.

---

## Nominal Categories

No natural ordering exists.

Examples:

* Gender
* Country
* City
* Department
* Blood Group

Example:

| City   |
| ------ |
| Delhi  |
| Mumbai |
| Pune   |

Changing the order does not change their meaning.

---

## Ordinal Categories

Categories have a meaningful order.

Examples:

* Education Level
* Shirt Size
* Customer Satisfaction
* Performance Rating

Example:

| Satisfaction |
| ------------ |
| Poor         |
| Average      |
| Good         |
| Excellent    |

Order matters even though the values are textual.

---

# 5. Category Data Type

Initially, text columns usually appear as:

```python id="dtype01"
df.dtypes
```

Output:

```text id="dtype02"
Gender    object
```

Convert the column.

```python id="dtype03"
df["Gender"] = (
    df["Gender"]
      .astype("category")
)
```

Now check the type.

```python id="dtype04"
df.dtypes
```

Output:

```text id="dtype05"
Gender    category
```

The column now occupies significantly less memory.

---

# Viewing Categories

Display all unique categories.

```python id="cat02"
df["Gender"].cat.categories
```

Output:

```text id="cat03"
Index(
[
'Female',
'Male'
]
)
```

---

# Category Codes

Internally, Pandas stores integer codes.

```python id="cat04"
df["Gender"].cat.codes
```

Output:

| Gender | Code |
| ------ | ---: |
| Female |    0 |
| Male   |    1 |
| Female |    0 |

The displayed labels remain unchanged, but storage becomes much more efficient.

---

# 6. Memory Optimization

Measure memory usage.

```python id="memory01"
df.memory_usage(
    deep=True
)
```

After converting repeated text columns into categories, memory consumption often decreases dramatically.

Example:

| Data Type | Memory Usage |
| --------- | -----------: |
| Object    |        18 MB |
| Category  |         2 MB |

Large datasets benefit the most from category conversion.

---

## Checking Total Memory

```python id="memory02"
df.memory_usage(
    deep=True
).sum()
```

This returns the total memory consumed by the DataFrame.

---

# 7. Category Operations

Count occurrences.

```python id="cat05"
df["Gender"].value_counts()
```

Output:

| Gender | Count |
| ------ | ----: |
| Female |  5400 |
| Male   |  4600 |

---

Sort categories.

```python id="cat06"
df.sort_values(
    "Gender"
)
```

Filtering works exactly like normal text columns.

```python id="cat07"
df[
    df["Gender"] ==
    "Female"
]
```

Categories integrate seamlessly with other Pandas operations.

---

# Business Example

An HR department maintains employee records.

Columns include:

* Department
* Gender
* Job Title
* Employment Type
* Office Location

These columns contain relatively few unique values but millions of repeated entries.

Converting them into categorical variables reduces memory usage, speeds up reporting, and improves dashboard performance.

---

# Best Practices

✔ Convert repeated text columns into categories.

✔ Leave high-cardinality columns (e.g., email addresses) as strings.

✔ Measure memory before and after optimization.

✔ Document category definitions for future reference.

✔ Use categories before building machine learning pipelines.

---

# Common Mistakes

### Converting High-Cardinality Columns

Avoid converting columns where almost every value is unique.

Examples:

* Email Address
* Customer ID
* Transaction ID

These columns gain little benefit from the category data type.

---

### Assuming Categories Are Numeric

Category codes are internal representations and should not be treated as meaningful numerical values.

---

# Key Takeaways 

After completing this section, you should understand:

* What categorical data represents.
* The difference between nominal and ordinal categories.
* How to convert object columns into categories.
* How categories improve memory efficiency.
* How to inspect and work with category metadata.

> **"Categorical data allows analysts to represent repeated information efficiently, reducing memory usage while improving performance across analytical workflows."**

# 8. Ordered Categories

Some categorical variables have a natural order.

Examples include:

* Customer Satisfaction
* Education Level
* Employee Performance
* Product Ratings
* T-Shirt Sizes

Unlike nominal categories, ordinal categories should preserve their ranking.

---

## Creating Ordered Categories

Suppose customer satisfaction levels are:

```text id="order01"
Poor

Average

Good

Excellent
```

Create an ordered categorical column.

```python id="order02"
from pandas.api.types import CategoricalDtype

satisfaction_order = CategoricalDtype(
    categories=[
        "Poor",
        "Average",
        "Good",
        "Excellent"
    ],
    ordered=True
)

df["Satisfaction"] = (
    df["Satisfaction"]
      .astype(satisfaction_order)
)
```

---

## Comparing Ordered Categories

```python id="order03"
df[
    df["Satisfaction"] >
    "Average"
]
```

Output:

| Satisfaction |
| ------------ |
| Good         |
| Excellent    |

Ordered categories allow meaningful comparisons.

---

# 9. Renaming Categories

Business terminology often changes.

Suppose:

```text id="rename01"
M

F
```

Rename the categories.

```python id="rename02"
df["Gender"] = (
    df["Gender"]
      .cat.rename_categories({
          "M":"Male",
          "F":"Female"
      })
)
```

Output:

| Gender |
| ------ |
| Male   |
| Female |

This improves readability without changing the underlying structure.

---

# 10. Adding New Categories

Sometimes additional categories become necessary.

```python id="addcat01"
df["Department"] = (
    df["Department"]
      .cat.add_categories(
          ["Research"]
      )
)
```

Now the new category exists even if no rows currently use it.

---

# 11. Removing Unused Categories

Suppose a category is no longer present.

```python id="removecat01"
df["Department"] = (
    df["Department"]
      .cat.remove_unused_categories()
)
```

Removing unused categories keeps the metadata clean and reduces unnecessary storage.

---

# 12. Label Encoding

Many machine learning algorithms require numerical inputs.

Label Encoding assigns an integer to each category.

Example:

| City   | Label |
| ------ | ----: |
| Delhi  |     0 |
| Mumbai |     1 |
| Pune   |     2 |

Using category codes:

```python id="label01"
df["City Label"] = (
    df["City"]
      .astype("category")
      .cat.codes
)
```

Output:

| City   | Label |
| ------ | ----: |
| Delhi  |     0 |
| Mumbai |     1 |
| Delhi  |     0 |

---

## When to Use Label Encoding

Suitable for:

* Ordinal variables
* Tree-based machine learning models
* Decision Trees
* Random Forest
* XGBoost
* LightGBM

Avoid using label encoding for nominal categories with linear models because it may introduce an artificial order.

---

# 13. One-Hot Encoding

For nominal variables, One-Hot Encoding is generally preferred.

Original data:

| Payment Method |
| -------------- |
| Cash           |
| Card           |
| UPI            |

Create dummy variables.

```python id="dummy01"
pd.get_dummies(
    df["Payment Method"]
)
```

Output:

| Cash | Card | UPI |
| ---: | ---: | --: |
|    1 |    0 |   0 |
|    0 |    1 |   0 |
|    0 |    0 |   1 |

Each category becomes its own binary feature.

---

# 14. One-Hot Encoding Multiple Columns

Encode several categorical columns simultaneously.

```python id="dummy02"
pd.get_dummies(
    df,
    columns=[
        "Gender",
        "Department"
    ]
)
```

The specified columns are replaced with encoded features.

---

# 15. Avoiding the Dummy Variable Trap

In regression models, one dummy variable may be redundant because it can be inferred from the others.

Avoid this by dropping one category.

```python id="dummy03"
pd.get_dummies(
    df,
    columns=["Gender"],
    drop_first=True
)
```

Example:

Instead of:

| Male | Female |
| ---: | -----: |
|    1 |      0 |
|    0 |      1 |

Store only:

| Male |
| ---: |
|    1 |
|    0 |

The missing category is implied.

---

# 16. Feature Engineering Using Categories

Categorical variables can be transformed into meaningful machine learning features.

Example:

Customer Segment:

| Segment |
| ------- |
| Gold    |
| Silver  |
| Bronze  |

Encoded features allow machine learning algorithms to use customer segment information effectively.

Other examples include:

* Product Categories
* Shipping Methods
* Customer Types
* Education Levels
* Subscription Plans

---

# Business Example

A banking institution builds a customer churn prediction model.

The dataset contains:

* Gender
* Marital Status
* Occupation
* Education Level
* Account Type

Before training the model:

* Ordinal variables are label encoded.
* Nominal variables are one-hot encoded.
* High-cardinality variables are reviewed carefully.
* Categories are standardized for consistency.

The resulting dataset becomes suitable for predictive modeling.

---

# Best Practices

✔ Use ordered categories only when a natural ranking exists.

✔ Use One-Hot Encoding for nominal variables.

✔ Use Label Encoding primarily for ordinal variables.

✔ Remove unused categories regularly.

✔ Document category mappings used during preprocessing.

---

# Common Mistakes

### Label Encoding Nominal Categories

Incorrect interpretation:

```text id="mistake01"
Red = 0

Blue = 1

Green = 2
```

These numbers do **not** indicate that Green is greater than Blue.

For unordered variables, One-Hot Encoding is usually a better choice.

---

### One-Hot Encoding High-Cardinality Columns

Encoding columns such as Customer ID or Email Address can create thousands of unnecessary columns.

Consider alternative encoding strategies for very high-cardinality variables.

---

### Forgetting `drop_first=True`

For linear regression models, retaining every dummy variable may introduce multicollinearity.

Use:

```python id="mistake02"
drop_first=True
```

when appropriate.

---

# Quick Recap

You have now learned how to:

* Create ordered categories.
* Rename categories.
* Add and remove categories.
* Perform Label Encoding.
* Perform One-Hot Encoding.
* Avoid the Dummy Variable Trap.
* Prepare categorical variables for machine learning.

> **"Encoding transforms human-readable categories into machine-readable features, enabling algorithms to learn meaningful patterns while preserving valuable business information."**
