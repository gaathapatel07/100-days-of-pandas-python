# Day 49 — Advanced String Operations with Pandas

## Introduction

Real-world datasets rarely contain perfectly formatted text. Customer names may have inconsistent capitalization, email addresses may contain extra spaces, product descriptions may include unwanted symbols, and reviews often require cleaning before analysis.

Pandas provides the powerful **`.str` accessor**, which allows vectorized string operations on entire columns without writing loops.

String operations are essential for:

- Data Cleaning
- Natural Language Processing (NLP)
- Customer Analytics
- Marketing Analytics
- Data Validation
- Feature Engineering

---

# Topics Covered

- Introduction to String Operations
- The `.str` Accessor
- Case Conversion
- Removing Whitespace
- String Length
- Splitting Strings
- Joining Strings
- Replacing Text
- Finding Text
- String Slicing
- Regular Expressions (Regex)
- Pattern Matching
- Text Extraction
- String Counting
- Padding Strings
- Encoding & Decoding
- Creating Dummy Variables
- Text Feature Engineering
- Advanced Text Cleaning
- Enterprise Workflow
- Automated Text Cleaning Pipeline
- Production Best Practices
- Enterprise Case Study
- Practice Exercises
- Interview Questions
- Cheat Sheet
- Mini Project

---

# 1. What are String Operations?

String operations manipulate textual data stored in DataFrame columns.

Example dataset:

| Customer | Email |
|-----------|-------------------------|
| john doe | john@gmail.com |
| ALICE SMITH | alice@yahoo.com |
| Bob Brown | bob@hotmail.com |

Goals include:

- Standardizing names
- Cleaning emails
- Extracting domains
- Validating formats

---

# 2. Why String Operations Matter

Businesses rely heavily on textual information such as:

- Customer names
- Product names
- Email addresses
- Reviews
- Addresses
- Cities

Poor-quality text causes duplicate records, inaccurate reports, and unreliable analytics.

---

# 3. The `.str` Accessor

The `.str` accessor applies string methods to every value in a Series.

```python
df["Customer"].str.upper()
```

Advantages:

- Vectorized
- Fast
- Handles entire columns
- More readable than loops

---

# 4. Case Conversion

Uppercase

```python
df["Customer"].str.upper()
```

Lowercase

```python
df["Customer"].str.lower()
```

Title Case

```python
df["Customer"].str.title()
```

Capitalize

```python
df["Customer"].str.capitalize()
```

---

# 5. Removing Whitespace

```python
df["Customer"].str.strip()
```

Left spaces

```python
df["Customer"].str.lstrip()
```

Right spaces

```python
df["Customer"].str.rstrip()
```

---

# 6. String Length

```python
df["Customer"].str.len()
```

Useful for validating:

- Phone numbers
- IDs
- Postal codes

---

# 7. Splitting Strings

```python
df["Email"].str.split("@")
```

Create new columns.

```python
df[["Username","Domain"]] = df["Email"].str.split("@", expand=True)
```

---

# 8. Joining Strings

```python
df["Full Name"] = df["First Name"] + " " + df["Last Name"]
```

---

# 9. Replacing Text

```python
df["City"].str.replace("Bombay","Mumbai")
```

Replace spaces.

```python
df["Customer"].str.replace("  "," ", regex=False)
```

---

# 10. Finding Text

Contains

```python
df["Email"].str.contains("gmail")
```

Starts with

```python
df["Email"].str.startswith("john")
```

Ends with

```python
df["Email"].str.endswith(".com")
```

---

# 11. String Slicing

First five characters

```python
df["Customer"].str[:5]
```

Last four characters

```python
df["Customer"].str[-4:]
```

---

# 12. Regular Expressions (Regex)

Regex allows searching and manipulating text using patterns.

Find all email addresses.

```python
df["Email"].str.contains(r"@")
```

Check if a phone number contains only digits.

```python
df["Phone"].str.match(r"^\d+$")
```

Find words beginning with A.

```python
df["Customer"].str.contains(r"^A")
```

---

# 13. Pattern Matching

Find Gmail users.

```python
gmail = df[
    df["Email"].str.contains("gmail")
]
```

Find customers ending with "son".

```python
df["Customer"].str.endswith("son")
```

Find names beginning with J.

```python
df["Customer"].str.startswith("J")
```

---

# 14. Text Extraction

Extract usernames.

```python
df["Email"].str.extract(r"(.*)@")
```

Extract domains.

```python
df["Email"].str.extract(r"@(.*)")
```

Extract numbers.

```python
df["Product"].str.extract(r"(\d+)")
```

---

# 15. String Counting

Count occurrences.

```python
df["Review"].str.count("good")
```

Count digits.

```python
df["Phone"].str.count(r"\d")
```

Count spaces.

```python
df["Customer"].str.count(" ")
```

---

# 16. Padding Strings

Pad on the left.

```python
df["ID"].str.pad(5, side="left", fillchar="0")
```

Equivalent.

```python
df["ID"].str.zfill(5)
```

Example

```
23

↓

00023
```

---

# 17. Encoding & Decoding

Encode strings.

```python
df["Customer"].str.encode("utf-8")
```

Decode strings.

```python
df["Encoded"].str.decode("utf-8")
```

Useful while reading data from external systems.

---

# 18. Creating Dummy Variables

Convert categories into binary columns.

```python
pd.get_dummies(df["Department"])
```

Example

| Department | HR | IT | Sales |
|------------|---:|---:|------:|
|HR|1|0|0|
|Sales|0|0|1|

Useful for Machine Learning.

---

# 19. Text Feature Engineering

Create useful features.

Username length.

```python
df["Username Length"] = df["Username"].str.len()
```

Email provider.

```python
df["Provider"] = df["Email"].str.split("@").str[1]
```

First letter.

```python
df["Initial"] = df["Customer"].str[0]
```

Word count.

```python
df["Words"] = df["Review"].str.split().str.len()
```

---

# 20. Advanced Text Cleaning

Remove punctuation.

```python
df["Review"] = df["Review"].str.replace(
    r"[^\w\s]",
    "",
    regex=True
)
```

Remove digits.

```python
df["Review"] = df["Review"].str.replace(
    r"\d+",
    "",
    regex=True
)
```

Remove multiple spaces.

```python
df["Review"] = df["Review"].str.replace(
    r"\s+",
    " ",
    regex=True
)
```

Convert to lowercase.

```python
df["Review"] = df["Review"].str.lower()
```

---

# 21. Enterprise String Processing Workflow

```
Raw Text
    │
    ▼
Whitespace Removal
    │
    ▼
Case Standardization
    │
    ▼
Regex Cleaning
    │
    ▼
Feature Extraction
    │
    ▼
Validation
    │
    ▼
Analytics / Machine Learning
```

---

# 22. Automated Text Cleaning Pipeline

```python
def clean_text(series):

    return (

        series

        .str.strip()

        .str.lower()

        .str.replace(
            r"[^\w\s]",
            "",
            regex=True
        )

        .str.replace(
            r"\s+",
            " ",
            regex=True
        )

    )
```

Usage

```python
df["Review"] = clean_text(df["Review"])
```

---

# 23. Production Best Practices

✔ Always preserve the original text column.

✔ Standardize capitalization.

✔ Remove extra spaces.

✔ Validate important text fields.

✔ Use Regex carefully.

✔ Use vectorized `.str` methods instead of loops.

---

# 24. Enterprise Case Study

A telecom company collects customer information from multiple channels.

Problems:

- Mixed capitalization
- Extra spaces
- Invalid email formats
- Duplicate customer names

Solution:

- Standardize names
- Clean emails
- Extract domains
- Validate formats
- Remove unwanted symbols

Result:

- Better reporting
- Cleaner dashboards
- Improved customer segmentation
- Higher machine learning accuracy

---

# 25. Practice Exercises

## Beginner

1. Convert names to uppercase.
2. Remove whitespace.
3. Calculate string length.
4. Split emails.
5. Join first and last names.

---

## Intermediate

6. Extract domains.
7. Replace city names.
8. Find Gmail users.
9. Count words.
10. Remove punctuation.

---

## Advanced

11. Build a text-cleaning pipeline.
12. Validate emails using Regex.
13. Create text features.
14. Build dummy variables.
15. Prepare customer reviews for analysis.

---

# 26. Interview Questions

### Beginner

1. What is the `.str` accessor?
2. Why use vectorized string methods?
3. Difference between `strip()` and `replace()`?
4. How do you split strings?
5. How do you join strings?

### Intermediate

6. What is Regex?
7. Explain `contains()`.
8. Explain `extract()`.
9. Difference between `match()` and `contains()`?
10. How do you validate email addresses?

### Advanced

11. How would you clean customer names?
12. Explain feature engineering using text.
13. How do you process millions of text records?
14. Explain dummy variable creation.
15. Design an enterprise text-cleaning pipeline.

---

# 27. Cheat Sheet

| Task | Syntax |
|------|--------|
| Uppercase | `.str.upper()` |
| Lowercase | `.str.lower()` |
| Title Case | `.str.title()` |
| Strip | `.str.strip()` |
| Length | `.str.len()` |
| Split | `.str.split()` |
| Join | `+` |
| Replace | `.str.replace()` |
| Contains | `.str.contains()` |
| Starts With | `.str.startswith()` |
| Ends With | `.str.endswith()` |
| Slice | `.str[:]` |
| Extract | `.str.extract()` |
| Count | `.str.count()` |
| Pad | `.str.pad()` |
| Zfill | `.str.zfill()` |
| Encode | `.str.encode()` |
| Decode | `.str.decode()` |
| Dummy Variables | `pd.get_dummies()` |

---

# 28. Mini Project

## Customer Data Cleaning System

Using a customer dataset:

Perform the following:

- Standardize names.
- Remove whitespace.
- Validate emails.
- Extract usernames.
- Extract domains.
- Clean reviews.
- Remove punctuation.
- Remove numbers.
- Create username-length feature.
- Generate dummy variables.
- Produce a clean dataset for analytics.

Write:

- Five business insights.
- Three recommendations.

---

# 29. Summary

Congratulations! 

Today you mastered **Advanced String Operations with Pandas**.

You learned:

- `.str` accessor
- Regex
- Pattern matching
- Text extraction
- String cleaning
- Feature engineering
- Dummy variable creation
- Enterprise text pipelines

These techniques are widely used in customer analytics, NLP, business intelligence, fraud detection, marketing analytics, and machine learning.

---

# 30. What's Next?

## 🐼 Day 50 — Advanced Pandas Performance Optimization

Topics include:

- Memory Optimization
- Efficient Data Types
- Vectorization
- Query Optimization
- Apply vs Vectorization
- Chunk Processing
- Efficient GroupBy
- Performance Benchmarking
- Enterprise Best Practices

Optimizing Pandas code is crucial when working with datasets containing millions of rows.

---

# Day 49 Complete!

You have successfully completed **Advanced String Operations with Pandas**.

You can now confidently clean, validate, transform, and engineer text data for analytics and machine learning.


