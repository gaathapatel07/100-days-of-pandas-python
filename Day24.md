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


# 17. Real-World Business Case Study

## Scenario

You are working as a **Senior Data Analyst** at **RetailHub**, a multinational retail company.

The customer dataset contains over **15 million records** with the following categorical columns:

* Gender
* Customer Segment
* Payment Method
* Product Category
* City
* Membership Level

Management has noticed that the analytics pipeline consumes excessive memory and machine learning models cannot directly process these text-based variables.

Your responsibility is to optimize the dataset for reporting and predictive analytics.

---

# Business Questions

### Question 1

Convert repeated text columns into categorical data.

```python id="case_cat01"
categorical_columns = [
    "Gender",
    "Customer Segment",
    "Payment Method",
    "Product Category"
]

for column in categorical_columns:
    df[column] = (
        df[column]
          .astype("category")
    )
```

---

### Question 2

Measure memory usage before and after optimization.

```python id="case_cat02"
df.memory_usage(
    deep=True
)
```

---

### Question 3

Create encoded features for machine learning.

```python id="case_cat03"
encoded_df = pd.get_dummies(
    df,
    columns=[
        "Gender",
        "Payment Method"
    ],
    drop_first=True
)
```

---

### Question 4

Generate label codes.

```python id="case_cat04"
df["Segment Code"] = (
    df["Customer Segment"]
      .cat.codes
)
```

---

### Question 5

Create ordered customer loyalty levels.

```python id="case_cat05"
from pandas.api.types import CategoricalDtype

membership_order = CategoricalDtype(
    categories=[
        "Bronze",
        "Silver",
        "Gold",
        "Platinum"
    ],
    ordered=True
)

df["Membership Level"] = (
    df["Membership Level"]
      .astype(membership_order)
)
```

---

# 18. Category vs Object

Understanding the difference between **Object** and **Category** data types is essential.

| Feature             | Object                         | Category                  |
| ------------------- | ------------------------------ | ------------------------- |
| Storage             | Stores every string separately | Stores unique values once |
| Memory Usage        | High                           | Very Low                  |
| Sorting             | Slower                         | Faster                    |
| GroupBy Performance | Moderate                       | Faster                    |
| Suitable for ML     | Requires Encoding              | Easy to Encode            |
| Best For            | High-cardinality text          | Repeated labels           |

---

# 19. Memory Benchmark

Suppose a dataset contains **5 million rows**.

| Data Type | Approximate Memory |
| --------- | -----------------: |
| Object    |             420 MB |
| Category  |              52 MB |

Memory Reduction:

```text id="memorybench01"
420 MB
   ↓
52 MB

≈ 88% Reduction
```

For enterprise datasets, category conversion can reduce memory consumption dramatically.

---

# 20. Performance Optimization

### Convert Only Low-Cardinality Columns

Good candidates:

* Gender
* Region
* Department
* Product Category
* Customer Segment

Avoid:

* Email Address
* Customer ID
* Transaction ID
* Phone Number

These columns contain mostly unique values and benefit little from category conversion.

---

### Measure Before Optimizing

```python id="perf_cat01"
before = (
    df.memory_usage(
        deep=True
    ).sum()
)
```

Optimize:

```python id="perf_cat02"
df["Department"] = (
    df["Department"]
      .astype("category")
)
```

Measure again.

```python id="perf_cat03"
after = (
    df.memory_usage(
        deep=True
    ).sum()
)
```

Memory saved:

```python id="perf_cat04"
saved = before - after

print(saved)
```

Always measure the impact of optimization rather than assuming improvements.

---

# 21. Business Insights

After optimizing the customer dataset, you discover:

* Memory usage decreases significantly after converting repeated text columns into categories.
* Dashboard loading becomes noticeably faster.
* Customer segmentation becomes easier using encoded variables.
* Ordered categories simplify loyalty-level analysis.
* Machine learning preprocessing becomes more efficient after encoding categorical variables.

These improvements reduce infrastructure costs while improving analytical performance.

---

# 22. Practice Exercises

## Beginner

1. Convert a text column into the category data type.
2. Display all categories.
3. Display category codes.
4. Measure memory usage.
5. Count category frequencies.

---

## Intermediate

6. Create ordered categories.
7. Rename existing categories.
8. Add a new category.
9. Remove unused categories.
10. Perform label encoding.

---

## Advanced

11. Apply One-Hot Encoding to multiple columns.
12. Compare memory usage before and after optimization.
13. Build a preprocessing pipeline for categorical variables.
14. Prepare a dataset for machine learning.
15. Write five recommendations for improving categorical data management.

---

# 23. Interview Questions

## Beginner

1. What is categorical data?
2. Difference between nominal and ordinal variables?
3. Why use the category data type?
4. What are category codes?
5. How do you view available categories?

---

## Intermediate

6. Difference between Label Encoding and One-Hot Encoding?
7. What is the Dummy Variable Trap?
8. Why remove unused categories?
9. When should ordered categories be used?
10. How does category conversion reduce memory usage?

---

## Advanced

11. Explain an end-to-end preprocessing workflow for categorical variables.
12. Compare Object and Category data types.
13. How does categorical encoding affect machine learning models?
14. How would you optimize a dataset containing 100 million records?
15. What factors determine the best encoding strategy for a categorical variable?

---

# 24. Cheat Sheet

| Task                     | Syntax                            |
| ------------------------ | --------------------------------- |
| Convert to Category      | `astype("category")`              |
| View Categories          | `.cat.categories`                 |
| Category Codes           | `.cat.codes`                      |
| Rename Categories        | `.cat.rename_categories()`        |
| Add Categories           | `.cat.add_categories()`           |
| Remove Unused Categories | `.cat.remove_unused_categories()` |
| Ordered Categories       | `CategoricalDtype()`              |
| Label Encoding           | `.cat.codes`                      |
| One-Hot Encoding         | `pd.get_dummies()`                |
| Drop First Dummy         | `drop_first=True`                 |
| Memory Usage             | `memory_usage(deep=True)`         |

---

# 25. Mini Project

## Customer Segmentation & Memory Optimization Pipeline

Using any retail, banking, healthcare, HR, or telecom dataset:

Complete the following tasks:

* Identify categorical columns.
* Convert suitable columns into the category data type.
* Compare memory usage before and after optimization.
* Create ordered categories where appropriate.
* Apply Label Encoding to ordinal variables.
* Apply One-Hot Encoding to nominal variables.
* Generate category frequency reports.
* Export the optimized dataset.
* Write **five executive-level business insights**.
* Recommend **three improvements** for future data collection and preprocessing.

### Example Business Insights

* Converting repeated text columns to categories reduced memory usage by over 80%.
* One-Hot Encoding improved compatibility with machine learning models.
* Ordered membership levels simplified customer loyalty analysis.
* Category-based grouping accelerated dashboard queries.
* Standardized categorical values improved reporting consistency across departments.

---

# 26. Summary

Congratulations! 🎉

Today you mastered **Categorical Data, Encoding & Memory Optimization** in Pandas.

You learned how to:

* Identify nominal and ordinal variables.
* Convert object columns into categories.
* Reduce memory consumption.
* Create ordered categorical variables.
* Apply Label Encoding and One-Hot Encoding.
* Prepare datasets for machine learning.
* Optimize analytical performance.

These techniques are essential for scalable data analysis, business intelligence, feature engineering, and predictive modeling.

---

# 27. What's Next?

In **Day 25**, you'll learn **Advanced Apply(), Map(), ApplyMap(), Pipe() & Vectorization**.

Topics include:

* `map()`
* `replace()`
* `apply()`
* `applymap()` *(and modern alternatives)*
* `pipe()`
* Lambda Functions
* Custom Functions
* Vectorization
* Performance Comparison
* Writing Efficient Pandas Code

These concepts help you write cleaner, faster, and more reusable data transformation pipelines.

---

<div align="center">

# 🎉 Day 24 Complete!

You've mastered one of the most valuable preprocessing techniques in modern data analytics and machine learning.

By understanding categorical variables, memory optimization, and encoding strategies, you can build faster analytics pipelines, reduce resource consumption, and prepare high-quality datasets for predictive models.

⭐ **Next → Day 25: Advanced `apply()`, `map()`, `pipe()` & Vectorization** ⚡🐼

</div>
