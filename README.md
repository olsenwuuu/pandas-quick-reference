# 🐼 Pandas Cheat Sheet: DataFrames

A **DataFrame** is a 2-dimensional, size-mutable, and tabular data structure with labeled axes (rows and columns). Think of it exactly like a SQL table or an Excel spreadsheet inside your Python environment.

### 🏗️ Anatomy of a DataFrame

A DataFrame consists of three primary components:
1. **Data:** The actual values (structured as lists, dictionaries, or arrays).
2. **Index:** The row labels (defaults to numeric integers `0, 1, 2...` if not specified).
3. **Columns:** The column headers.

---

### 🛠️ Creating DataFrames From Scratch

You can initialize a DataFrame manually using `pd.DataFrame()`. Here are the two most common ways to structure your raw data:

#### 1. Using a Dictionary of Lists (Column-by-Column)
This is usually the most intuitive method. Each **key** becomes a column header, and each **value list** fills that column down the rows.

```python
import pandas as pd

# Data structured by columns
data = {
    "Product": ["Coffee", "Espresso", "Latte"],
    "Price": [3.50, 4.00, 4.50],
    "In_Stock": [True, True, False]
}

df = pd.DataFrame(data)
print(df)
```
#### 2. Using a List of Lists (Row-by-Row)
This matches the layout used to build data tables row-by-row. Each inner list represents an entire horizontal row. You pass a separate columns list to name the headers.
```python
import pandas as pd

# Data structured by rows
data = [
    ["Coffee", 3.50, True],
    ["Espresso", 4.00, True],
    ["Latte", 4.50, False]
]

df = pd.DataFrame(data, columns=["Product", "Price", "In_Stock"])
print(df)
```
### ⏱️ Essential DataFrame Inspection Commands

Once a DataFrame is loaded, use these quick helper methods to inspect the shape and structure of your data right away:

| Command | Purpose | Example |
| :--- | :--- | :--- |
| **`df.head(n)`** | View the first `n` rows of the DataFrame (defaults to 5). | `df.head(2)` |
| **`df.tail(n)`** | View the last `n` rows of the DataFrame. | `df.tail(2)` |
| **`df.shape`** | Returns a tuple representing the dimensionality `(rows, columns)`. | `df.shape` |
| **`df.info()`** | Prints a concise summary including column data types and memory usage. | `df.info()` |
| **`df.columns`**| Returns an index list of all column headers. | `df.columns` |

---

## 🎯 Data Selection: `.loc` vs `.iloc`

Pandas uses two primary properties to slice and dice DataFrames. The golden rule to remember is: **`.loc` uses labels (text), while `.iloc` uses integer positions (numbers).**

### 🔍 `.loc` (Label-Based Selection)
Use `.loc` when you want to look up data by the row or column names/labels. 

**Syntax:** `df.loc[row_label, column_label]`



#### Examples:
```python
# Select a single row by its index label
df.loc['row_three']

# Select specific rows and specific columns by name
df.loc[['row_one', 'row_two'], ['Product', 'Price']]

# Slice rows from 'A' to 'C' (Note: In .loc, the stop point 'C' IS included!)
df.loc['A':'C', 'Price']
```

### 🔢 .iloc (Integer Position-Based Selection)
Use .iloc when you want to look up data by its raw physical position or index number, ignoring whatever text label it might have (exactly like slicing a native Python list).

Syntax: df.iloc[row_position, column_position]

```python
# Select the very first row (index 0)
df.iloc[0]

# Select the first 3 rows and the first 2 columns
# (Note: In .iloc, standard Python slicing applies—the stop index is EXCLUDED!)
df.iloc[0:3, 0:2]

# Grab the very last row using negative indexing
df.iloc[-1]
```
### ⚡ Quick Reference Comparison

| Feature | `.loc` | `.iloc` |
| :--- | :--- | :--- |
| **Lookup Type** | **Labels / Names** (Strings or numbers if index is explicit) | **Integer Positions** (Always numbers `0, 1, 2...`) |
| **Slicing Behavior** | **Includes** the stop point (`'A':'C'` includes C) | **Excludes** the stop point (`0:3` only gets 0, 1, 2) |
| **Common Use Case** | Filtering data by explicit index keys or column titles. | Slicing specific matrix chunks when column names don't matter. |

---

## ⚡ Fast Scalar Selection: `.at` vs `.iat`

When you only need to get or set a **single value** in a DataFrame, Pandas provides `.at` and `.iat`. Because they don't have the overhead of handling entire rows or slices, they are significantly faster than `.loc` or `.iloc`.

The same naming rule applies: **`.at` uses labels, while `.iat` uses integer positions.**


### 🔍 `.at` (Label-Based Single Value)
Use `.at` when you want to access a single cell using its row name and column name.

**Syntax:** `df.at[row_label, column_label]`

#### Examples:
```python
# Look up a specific value by row and column name
price = df.at['row_three', 'Price']

# Instantly update/change a single value
df.at['row_three', 'Price'] = 19.99
```
---
## 📊 Sorting Data: `.sort_values()`

To reorder your DataFrame based on the values of one or more columns, Pandas uses the `.sort_values()` method. By default, it sorts data in **ascending** order (smallest to largest / A to Z).

---

### 1. Sorting by a Single Column
To sort by just one column, pass the column name as a string.

```python
# Sort rows by price from cheapest to most expensive (Ascending)
df_sorted = df.sort_values('Price')

# Sort rows from highest score to lowest score (Descending)
df_sorted = df.sort_values('Score', ascending=False)
```
### 2. Sorting by Multiple Columns
To sort by multiple criteria, pass a list of column names. Pandas will sort by the first column listed, and then use the second column to break any ties.
```python
# Sort by Category alphabetically, then by Price from lowest to highest
df_sorted = df.sort_values(['Category', 'Price'])

# Sort by Category alphabetically, BUT sort Price from highest to lowest
# (Pass a list of booleans to the ascending parameter matching your column list)
df_sorted = df.sort_values(['Category', 'Price'], ascending=[True, False])
```
---
## 🧵 Python Raw Strings (`r"..."`)

In standard Python strings, the backslash (`\`) is an **escape character** used to introduce special commands (like `\n` for a new line or `\t` for a tab). 

A **Raw String** tells Python to completely ignore all escape characters and treat every single backslash as a literal, regular character. You create one by simply putting an `r` right before the opening quote.

### ⚠️ The Problem: Standard Strings on Windows
Windows file paths use backslashes. If you try to pass a standard path to Pandas, it will often break because Python misinterprets the path characters.

```python
# ❌ THIS WILL ERROR or behave weirdly:
# Python sees '\n' and thinks you want a new line, and sees '\t' and thinks you want a tab!
df = pd.read_csv("C:\Users\name\Documents\new_data\test_file.csv")
```
The Solution: The r Prefix
Adding an r forces Python to read the path exactly as it is written.
```python
#  THIS WORKS PERFECTLY:
# The 'r' tells Python: "Treat this entire string as raw text. No escape character magic."
df = pd.read_csv(r"C:\Users\name\Documents\new_data\test_file.csv")
```
💡 Alternative Solutions (If you don't use an r-string)
If you ever forget the r, you have to modify the path manually using one of these two methods:
```python
df = pd.read_csv("C:\\Users\\name\\Documents\\new_data\\test_file.csv")
```
Forward Slashes: Switch all backslashes to forward slashes (Unix/Mac style), which Python natively accepts on all systems.
```python
df = pd.read_csv("C:/Users/name/Documents/new_data/test_file.csv")
```
---
## 🔍 Filtering Data: `.query()`

The `.query()` method allows you to filter a DataFrame using a concise, readable text string. It looks and feels very similar to writing a standard SQL `WHERE` clause, making it a favorite for analysts transitioning from SQL to Python.

---

### 1. Basic Filtering (SQL-Style)
Instead of repeating the DataFrame name like `df[df['column'] > value]`, you write the condition directly inside a string.

```python
# Traditional Pandas syntax (bulky):
df[df['Age'] > 30]

# Clean .query() syntax:
df.query("Age > 30")

# Check for text equality (Note the use of different quotes inside/outside)
df.query("Category == 'Electronics'")
```
### 2. Combining Multiple Conditions (and, or, not)
You can chain multiple conditions together using natural English words (and, or, not) or standard logical operators (&, |, ~).
```python
# Filter using 'and' / '&'
df.query("Age > 25 and Status == 'Active'")

# Filter using 'or' / '|'
df.query("Category == 'Books' or Price < 10.00")

# Filter using 'not' or checking membership with 'in'
df.query("Country in ['US', 'CA', 'MX'] and not Brand == 'Generic'")
```
### 3. Referencing Python Variables using the @ Symbol
If you want to use a dynamic variable you defined earlier in your Python code inside your query string, prefix the variable name with the @ symbol.
```python
# Define a local Python variable
target_salary = 95000

# Reference it inside the query using @
high_earners = df.query("Salary >= @target_salary")
```
---
## 🗑️ Removing Data: `.drop()`

The `.drop()` method allows you to remove rows or columns from a DataFrame. By default, Pandas assumes you want to drop **rows** unless you explicitly tell it to look at **columns** using the `axis` parameter.

---

### 1. Removing Columns
To drop columns, pass the column name (or a list of names) and specify `axis=1` (or `axis='columns'`).

```python
# Drop a single column
df_cleaned = df.drop('Unnecessary_ID', axis=1)

# Drop multiple columns at once using a list
df_cleaned = df.drop(['Created_At', 'Thumbnail_Url', 'Internal_Notes'], axis=1)

# Alternative modern syntax (explicitly naming columns, no axis needed)
df_cleaned = df.drop(columns=['Created_At', 'Thumbnail_Url'])
```
### 2. Removing Rows
To drop rows, pass the row index labels. By default, axis=0 (rows), so you don't strictly need to type the axis parameter.
```python
# Drop a single row by its index label
df_cleaned = df.drop('row_one')

# Drop multiple rows using a list of index labels
df_cleaned = df.drop([0, 1, 4]) # Useful if your index is numeric
```
## 🤝 Combining Data: `.merge()` (SQL Joins in Pandas)

If you know SQL, this behaves exactly like an `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, or `FULL OUTER JOIN`. The `.merge()` method is used to link two DataFrames together based on a shared "key" column or index. 

---

### 1. Basic Syntax
By default, if you don't specify the type of join, Pandas will execute an **inner join** (only keeping rows where the key matches in *both* tables).

```python
# Merge df1 and df2 using a shared column named 'customer_id'
df_combined = df1.merge(df2, on='customer_id')
```
### 2. Handling Different Column Names
If the linking columns are named differently in each table (e.g., user_id in the left table and customer_id in the right table), use left_on and right_on.
```python
df_combined = df1.merge(df2, left_on='user_id', right_on='customer_id')
```
### 3. Choosing Your Join Type (how)
You can control how unmatched rows are treated using the how parameter:

- how='inner' (Default): Keeps only the keys that exist in both DataFrames.

- how='left': Keeps all rows from the left DataFrame, and matches values from the right. Unmatched rows from the right get filled with NaN.

- how='right': Keeps all rows from the right DataFrame, and matches values from the left.

- how='outer': Keeps all rows from both DataFrames, filling in NaN wherever a match is missing.
```python
# Keep all records from the left table, even if they don't match the right table
df_left_joined = df1.merge(df2, on='customer_id', how='left')
```
#### ⚠️ Useful Parameters to Know
- suffixes: If both DataFrames have columns with the same name that you aren't joining on (like Created_At in both tables), Pandas will automatically append _x and _y to distinguish them. You can customize this by passing a tuple to suffixes.

- indicator: Setting indicator=True adds a special column named _merge to the output table that tells you exactly where that row came from (both, left_only, or right_only). Highly useful for data auditing!
```python
# Merge with custom suffixes and turn on the tracking indicator
df_final = df1.merge(
    df2, 
    on='product_id', 
    how='outer', 
    suffixes=('_monthly', '_yearly'),
    indicator=True
)
```
### ⚡ Quick SQL-to-Pandas Translation Cheat Sheet

| SQL Operation | Pandas `.merge()` Equivalent |
| :--- | :--- |
| `FROM table_a INNER JOIN table_b ON a.id = b.id` | `table_a.merge(table_b, on='id', how='inner')` |
| `FROM table_a LEFT JOIN table_b ON a.id = b.id` | `table_a.merge(table_b, on='id', how='left')` |
| `FROM table_a RIGHT JOIN table_b ON a.id = b.id` | `table_a.merge(table_b, on='id', how='right')` |
| `FROM table_a FULL OUTER JOIN table_b ON a.id1 = b.id2` | `table_a.merge(table_b, left_on='id1', right_on='id2', how='outer')` |

---

## ⛓️ Combining Data: `.concat()` (Concatenation)

The `pd.concat()` function is used to append DataFrames together along an axis. Think of it as either stacking rows on top of each other (like a SQL `UNION ALL`) or gluing columns together side-by-side. 

Unlike `.merge()`, which looks for matching values in columns, `.concat()` just blindly glues tables together based on their index positions or column names.

### 1. Stacking Rows (Default: `axis=0`)
Use this when you have multiple tables with the exact same columns and you want to combine them into one long dataset. 

```python
# Combine USA and GBR dataframes into one single table
# (Pass the DataFrames as a list inside the function)
combined_rows = pd.concat([usa, gbr])
```

### 2. Gluing Columns Side-by-Side (axis=1)
Use this when you have tables representing the exact same rows/subjects, but containing completely different columns of information.
```python
# Glue columns from df_demographics and df_financials side-by-side
combined_cols = pd.concat([df_demographics, df_financials], axis=1)
```
---
## 🧊 Missing Data: `np.nan` (Not a Number)

In Python and Pandas, `np.nan` stands for **Not a Number**. It comes from the NumPy library (`import numpy as np`) and is the standard data sentinel value used to represent **missing, null, or blank data** in a dataset.

### 1. How to Create or Inject Missing Data
You will often use `np.nan` when you need to manually wipe out bad data or simulate a dataset with missing values for testing.

```python
import numpy as np
import pandas as pd

# Intentionally blank out the first two rows of 'Units Sold'
df.loc[[0, 1], 'Units Sold'] = np.nan
```
#### ⚠️ It is secretly a float!
Even if your column is full of integers (like [10, 20, 30]), the moment you introduce a single np.nan, Pandas will automatically convert the entire column into decimal floats ([10.0, 20.0, NaN]). This is because Python's core engine natively treats NaN as a floating-point data type.
---
## 🔍 Detecting Missing Data: `.isna()` (and `.isnull()`)

The `.isna()` method scans your DataFrame and returns a matching table of boolean values (`True` or `False`). Every cell that contains a missing value (`NaN`, `None`) becomes `True`, and every valid cell becomes `False`.

---

### 1. Spotting Nulls Across the Whole Table
Running `.isna()` by itself on a large DataFrame creates a wall of `True`/`False` text, which isn't very helpful. Instead, data analysts chain it with `.sum()` to instantly see a breakdown of exactly how many missing values exist in each column.

```python
# Returns a clean list of columns and the total number of missing entries in each
df.isna().sum()
```
Example Output
```python
Product Name     0
Units Sold      14
Price            2
dtype: int64
```
---
## 🩹 Imputing Data: `.fillna()`

The `.fillna()` method replaces missing `NaN` values with a static value or a calculated statistic (like a mean, median, or mode) that you specify. 

### 1. Replacing with a Constant Value
This is commonly used to replace missing numeric data with a baseline like `0`, or missing categorical data with a placeholder string like `'Unknown'`.

```python
# Replace all NaN values in 'Units Sold' with 0
df['Units Sold'] = df['Units Sold'].fillna(0)

# Replace missing strings with a fallback label
df['Status'] = df['Status'].fillna('Missing_Data')
```
### 2. Replacing with a Calculated Metric (Imputation)
Instead of a hardcoded number, you can dynamically calculate a statistic from the column itself and use it to fill the blanks.
```python
# Calculate the average salary of the dataset
mean_salary = df['Salary'].mean()

# Fill missing salaries with that exact average
df['Salary'] = df['Salary'].fillna(mean_salary)
```
### 3. Forward Fill (ffill) and Backward Fill (bfill)
You can direct Pandas to look at adjacent rows to fill a gap. This is incredibly useful for sorted sequential or time-series data.
- method='ffill' (Forward Fill): Carries the last known valid value forward to fill subsequent gaps.
- method='bfill' (Backward Fill): Looks ahead and pulls the next valid value backward to fill gaps.
```python
# Carry the previous day's closing stock price forward to fill a missing day
df['Closing_Price'] = df['Closing_Price'].fillna(method='ffill')
```
---

markdown
## 📈 Estimating Continuous Gaps: `.interpolate()`

The `.interpolate()` method is an advanced mathematical tool used to fill `NaN` values by estimating them based on the surrounding data points. Instead of filling blanks with a single static number, it draws a logical mathematical bridge between the last known number and the next known number.

### 1. Linear Interpolation (Default)
By default, `.interpolate()` treats your data points as a straight line and calculates an equal mathematical stepping stone across the missing index gaps. 

Imagine your data sequence goes: `10`, `NaN`, `NaN`, `40`. Linear interpolation calculates the missing steps automatically:

```python
# Sample sequence: [10.0, NaN, NaN, 40.0]
df['Temperature'] = df['Temperature'].interpolate()

# Resulting sequence: [10.0, 20.0, 30.0, 40.0]
```
### ⚡ Data Imputation Strategy: `.fillna()` vs. `.interpolate()`

| Scenario | Best Method | Why? |
| :--- | :--- | :--- |
| **Missing Categorical Data**<br>*(e.g., Country, Color, Job Title)* | `.fillna('Unknown')` | You cannot mathematically calculate a trend or midpoint between descriptive text categories like "USA" and "GBR". |
| **Baseline Resetting**<br>*(e.g., No transactions or units sold)* | `.fillna(0)` | If an event didn't occur (like a store making zero sales), the value is an absolute structural 0, not a calculated trend. |
| **Time-Series / Sensor Data**<br>*(e.g., Hourly temperatures, Stock prices)* | `.interpolate()` | Gaps are sequential. If a sensor cuts out at 2:00 PM (70°F) and resumes at 4:00 PM (74°F), estimating 3:00 PM as 72°F is highly accurate. |
