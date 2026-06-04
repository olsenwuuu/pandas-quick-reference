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

---
## 🪓 Eliminating Missing Data: `.dropna()`

The `.dropna()` method removes any rows or columns that contain missing (`NaN`) values from your DataFrame. While filling missing data is often preferred, dropping records completely is the right choice when key identifiers (like a `User_ID` or a primary numeric metric) are missing entirely.

### 1. The Default Behavior: Dropping Whole Rows
By default, running `.dropna()` will drop **any row** that contains even a single missing value across any of its columns.

```python
# Drops any row if it has a NaN anywhere inside it
df_cleaned = df.dropna()
```
### 2. Targeting Specific Columns (subset)
Dropping an entire row just because a non-essential column (like Notes) is blank can accidentally destroy valuable data. Use the subset parameter to only drop rows if missing data is found in critical columns.
```python
# Only drop the row if 'Customer_ID' or 'Total_Sales' is missing
df_cleaned = df.dropna(subset=['Customer_ID', 'Total_Sales'])
```
### 3. Dropping Whole Columns instead of Rows (axis=1)
If a specific column is missing so much data that it is completely useless for analytics, you can drop the entire vertical column from your dataset.
```python
# Remove the entire column if it contains any NaN values
df_cleaned = df.dropna(axis=1)

# Alternative explicit syntax
df_cleaned = df.dropna(axis='columns')
```
Advanced Example
```python
# Keep rows only if they have at least 3 columns with valid data, 
# but completely ignore blanks inside the 'Internal_Notes' column.
df.dropna(thresh=3, subset=['User_ID', 'Revenue', 'Signup_Date'], inplace=True)
```
---
## 📊 Analyzing Categorical Distributions: `.value_counts()`

The `.value_counts()` method counts the occurrences of unique values within a specific Series (column). It automatically sorts the results in descending order, making it incredibly easy to see the most frequent entries in your data instantly.

### 1. Basic Usage (SQL-Style Grouping)
By default, running this on a column will count every unique entry, ignoring missing (`NaN`) values.

```python
# See how many rows belong to each country
df['born_country'].value_counts()
```
Example Output:
```python
# See how many rows belong to each country
df['born_country'].value_counts()
```
### 2. Turning Counts into Percentages (normalize)
If you want to see the relative frequency (percentages/proportions) of each category instead of the raw counts, set normalize=True.
```python
# Show the percentage distribution of countries (e.g., 0.54 = 54%)
df['born_country'].value_counts(normalize=True)
```
### 3. Including Missing Values (dropna)
By default, .value_counts() quietly skips over missing data. If you want to see how many rows are missing data compared to your valid categories, set dropna=False.
```python
# Show unique counts, including a dedicated line for missing (NaN) records
df['born_country'].value_counts(dropna=False)
```
### 4. Grouping Continuous Data into Bins (bins)
While .value_counts() is typically used for text columns, you can use the bins parameter to automatically slice continuous numbers (like ages or salaries) into discrete intervals. This is a quick way to build a text-based histogram.
```python
# Slice prices into 4 equal-sized mathematical ranges and count entries in each
df['Price'].value_counts(bins=4)
```
Example Output 
```python
(9.99, 25.00]     450
(25.00, 50.00]    120
(50.00, 75.00]     35
(75.00, 100.00]    12
dtype: int64
```
---
## 🗂️ Aggregating Data: `.groupby()`

The `.groupby()` method allows you to group rows of data together based on one or more columns and run mathematical aggregations (like sum, mean, or count) on them. It mirrors the exact structural logic of a SQL `GROUP BY` statement.

### 🧱 The Split-Apply-Combine Workflow
When you run a `.groupby()`, Pandas executes three distinct operations under the hood:
1. **Split**: Separates the DataFrame into distinct mini-tables based on the column values you specified.
2. **Apply**: Computes a statistical function (like `.mean()`) on each of those mini-tables.
3. **Combine**: Melds the calculated metrics back together into a single, polished output table.

### 1. Basic Single Aggregation
To run a basic aggregation, specify the column to group by, the numerical column you want to measure, and the math function.

```python
# Group by country and calculate the average height of athletes
df.groupby('born_country')['height_cm'].mean()
```
### 2. Multiple Aggregations at Once (.agg())
If you want to calculate multiple different statistics for your columns simultaneously, pass a list of functions into the .agg() method.
```python
# Calculate min, max, and average age per country
df.groupby('born_country')['age'].agg(['min', 'max', 'mean'])
```
### 3. Named Aggregations (Best Practice for Clean Column Names)
By default, compiling multiple metrics can create messy, multi-layered headers (MultiIndex columns). You can use Named Aggregation to explicitly name your output columns right inside the function.
```python
# Group by country and create custom-named summary columns
country_summary = df.groupby('born_country').agg(
    total_athletes=('athlete_id', 'count'),
    average_weight=('weight_kg', 'mean'),
    max_height=('height_cm', 'max')
)
```
--- 
## 🧪 Advanced Aggregations: `.agg()`

The `.agg()` method (short for aggregate) allows you to bypass the limitations of simple math functions and apply multiple, specific statistical calculations across your DataFrame columns simultaneously. 

### 1. Applying Multiple Functions to a Single Column
If you want to look at the spread of a single metric, pass a list of string functions (like `'min'`, `'max'`, `'mean'`, `'std'`, or `'count'`) directly into `.agg()`.

```python
# See the full mathematical spread of coffee prices
df['Price'].agg(['min', 'max', 'mean', 'std'])
```
### 2. Multi-Column Aggregation (The Dictionary Method)
To calculate completely different metrics for different columns at the same time, pass a Python dictionary ({ 'column' : 'function' }) into .agg().
```python
# Calculate Total Units Sold, but get the Average Price
df.groupby('Coffee Type').agg({
    'Units Sold': 'sum',
    'Price': 'mean'
})
```
### 3. Named Aggregations (The Gold Standard)
When you apply multiple aggregations, Pandas naturally creates messy, stacked column headers (MultiIndex headers) that are a pain to filter later.
Named Aggregation fixes this completely. It lets you assign clean, custom column names directly to your metrics right inside the function using the syntax: New_Column_Name=('Original_Column', 'Function').
```python
# Group by Coffee Type and generate a perfectly flat, clean summary table
coffee_summary = df.groupby('Coffee Type').agg(
    total_sales_volume=('Units Sold', 'sum'),
    average_customer_spend=('Price', 'mean'),
    unique_transactions=('Transaction_ID', 'count')
)
```
### 📊 Aggregated Summary Table
| Coffee Type | total_sales_volume | average_customer_spend | unique_transactions |
| :--- | :---: | :---: | :---: |
| **Espresso** | 740 | $3.50 | 210 |
| **Latte** | 610 | $4.75 | 145 |

---

## 🕒 Time-Shifting Data: `.shift()`

The `.shift()` method moves your data rows up or down by a specified number of periods. This is the absolute go-to tool for time-series analysis, allowing you to compare a current row's value to a previous row's value (e.g., calculating Month-over-Month growth).

### 1. Lagging Data (Looking Backward)
By default, passing a positive integer shifts the data **down**, creating a lag. This positions yesterday's data right next to today's data.

```python
# Create a new column containing the previous row's sales data
df['Previous_Month_Sales'] = df['Units Sold'].shift(1)

# Calculate Month-over-Month absolute growth
df['MoM_Growth'] = df['Units Sold'] - df['Previous_Month_Sales']
```
### 2. Leading Data (Looking Forward)
Passing a negative integer shifts the data up, pulling future rows into the current row's view.
```python
# Pull next month's price into the current row
df['Next_Month_Price'] = df['Price'].shift(-1)
```
### 3. Shifting within Categories (.groupby)
If your dataset contains multiple different categories (like different coffee shops), running a global .shift() will accidentally bleed data across distinct groups. Combine .shift() with .groupby() to isolate the movement within each group cleanly.
```python
# Shift rows safely within each individual Coffee Type
df['Prev_Sales'] = df.groupby('Coffee Type')['Units Sold'].shift(1)
```
---
## 🏅 Ordering Data: `.rank()`

The `.rank()` method assigns a numerical rank (1 through $N$) to the entries in a column. Unlike `.sort_values()`, which reorders your entire table, `.rank()` preserves your row structure and simply tells you where each row stands relative to the rest.

### 1. Basic Ranking
By default, `.rank()` assigns the number 1 to the *smallest* value (ascending). For business performance metrics (like sales or revenue), you usually want the *largest* value to be rank 1, which requires setting `ascending=False`.

```python
# Rank products by Units Sold, where the top seller is #1
df['Sales_Rank'] = df['Units Sold'].rank(ascending=False)
```
### 2. Handling Ties (method)
When two rows have the exact same value, Pandas needs to know how to break the tie. You control this using the method parameter:
### 🏅 `.rank()` Tie-Breaking Methods (`method=`)
| Method | Tie-Breaking Logic | Example Result for a Tie |
| :--- | :--- | :---: |
| **`'average'`** *(Default)* | Assigns the mathematical average rank of the tied group. | `[1, 2.5, 2.5, 4]` |
| **`'min'`** | Assigns the lowest rank to all tied rows (Matches SQL `RANK()`). | `[1, 2, 2, 4]` |
| **`'max'`** | Assigns the highest rank to all tied rows. | `[1, 3, 3, 4]` |
| **`'first'`** | Ranks based strictly on the order of appearance in the dataset. | `[1, 2, 3, 4]` |
| **`'dense'`** | Like `min`, but the next rank after a tie is always incremented by exactly +1 (Matches SQL `DENSE_RANK()`). | `[1, 2, 2, 3]` |

```python
# Rank sales using standard SQL-style tie-breaking
df['Dense_Rank'] = df['Units Sold'].rank(method='dense', ascending=False)
```
---
## 🔄 Moving Window Calculations: `.rolling()`

The `.rolling()` method creates a sliding window of a specific size over your data rows. It is primarily used to calculate **Moving Averages**, which smooth out noisy, short-term fluctuations to reveal long-term trends.

### 1. Calculating a Simple Moving Average (SMA)
To build a rolling calculation, specify your window size (number of rows) and chain an aggregation function like `.mean()` or `.sum()`.

```python
# Calculate a 7-day rolling average of units sold
df['7_Day_Avg_Sales'] = df['Units Sold'].rolling(window=7).mean()
```
### 2. Overriding the Minimum Sample Constraint (min_periods)
If you want to start calculating averages immediately without waiting for the window to fill up completely, use the min_periods parameter.
```python
# Calculate a 7-day average, but output a value even if only 1 day of data is available
df['7_Day_Avg_Sales'] = df['Units Sold'].rolling(window=7, min_periods=1).mean()
```
---
## 🪣 Running Totals: `.cumsum()`

The `.cumsum()` method (Cumulative Sum) tracks a running total of a numeric column from the very first row down to the last. This is the cornerstone metric for tracking pacing, monthly performance-to-target metrics, and lifetime customer values.

### 1. Global Running Total
Running `.cumsum()` by itself accumulates values continuously down through your entire dataset.

```python
# Create a continuous running total of all revenue generated over time
df['Lifetime_Revenue'] = df['Revenue'].cumsum()
```
### 2. Segmented Running Totals (.groupby)
In standard business reporting, you rarely want a flat running total across all categories combined. You typically need to reset the running total for each specific segment (e.g., tracking month-to-date sales for individual stores or tracking cumulative spend per distinct user).
```python
# Track the cumulative revenue growth isolated for each distinct Coffee Type
df['Cumulative_Type_Revenue'] = df.groupby('Coffee Type')['Revenue'].cumsum()
```
### ☕ Segmented Running Total Output (`.groupby + .cumsum`)

| Date | Coffee Type | Revenue | Cumulative_Type_Revenue |
| :--- | :--- | :---: | :--- |
| 2026-05-01 | **Espresso** | $100 | **$100** |
| 2026-05-01 | **Latte** | $150 | **$150** |
| 2026-05-02 | **Espresso** | $120 | **$220** *(100 + 120)* |
| 2026-05-02 | **Latte** | $130 | **$280** *(150 + 130)* |
---

## 🔍 Finding Index Locations: `.idxmax()` and `.idxmin()`

When analyzing datasets, we frequently need to find the specific row identifier (index) where a maximum or minimum value occurs—such as finding the highest-priced product or the earliest transaction timestamp. 

While `.max()` returns the **value** itself, `.idxmax()` returns the **label of the index** where that maximum value resides. 

#### 🛠️ How it Works

* **`df['column'].idxmax()`**: Returns the single index label of the maximum value in a Series.
* **`df['column'].idxmin()`**: Returns the single index label of the minimum value in a Series.

#### 💡 Real-World Example
Imagine you are analyzing a dataset of wine reviews and want to find the exact row entry for the highest-rated wine in the dataset to inspect its details.

```python
import pandas as pd

# Sample Data
data = {
    'wine_name': ['Classic Chardonnay', 'Reserve Cabernet', 'Old Vine Zinfandel'],
    'points': [88, 96, 91]
}
df = pd.DataFrame(data)

# 1. Find the index label of the maximum points
highest_points_index = df['points'].idxmax()
print(f"The highest score is at index: {highest_points_index}")
# Output: The highest score is at index: 1

# 2. Use .loc to extract the complete record for that index
best_wine_record = df.loc[highest_points_index]
print(best_wine_record)
# Output:
# wine_name    Reserve Cabernet
# points                     96
# Name: 1, dtype: object
```
---
## 🔄 Custom Row-by-Row Transformations: `.apply()`

While Pandas has highly optimized built-in functions for standard operations, real-world data pipelines often require complex, custom conditional logic that standard methods cannot handle. For these scenarios, `.apply()` serves as the ultimate tool for executing custom Python functions across a dataset.

#### 🛠️ How it Works

When you use `.apply()` with a custom function and set `axis='columns'` (or `axis=1`), Pandas acts like a loop running through the DataFrame horizontally. 

* **The `row` Argument:** Inside the custom function, the argument (typically named `row`) represents a single, individual record from the dataset structured as a Pandas Series. You can access any column value for that specific record using dot notation (e.g., `row.column_name`).
* **The Execution:** Pandas passes each row into the function one at a time, waits for it to evaluate and return a value, records that value, and moves to the next row.

#### 💡 Real-World Example
Imagine you want to assign a custom tier rating to wines based on complex, multi-layered conditions involving both the country of origin and their review points.

```python
import pandas as pd

# Sample Data
data = {
    'country': ['Canada', 'Italy', 'USA', 'France'],
    'points': [82, 96, 88, 91]
}
df = pd.DataFrame(data)

# 1. Define the custom evaluation logic for a single row
def calculate_stars(row):
    if row.country == 'Canada':
        return 3
    elif row.points >= 95:
        return 3
    elif row.points >= 85:
        return 2
    else:
        return 1

# 2. Use .apply() to run the function row-by-row across the DataFrame
df['star_rating'] = df.apply(calculate_stars, axis='columns')

print(df)
# Output:
#   country  points  star_rating
# 0  Canada      82            3
# 1   Italy      96            3
# 2     USA      88            2
# 3  France      91            2
```
---
### 📊 Aggregating Group Frequencies: `.size()`

When performing data aggregation, we frequently need to know the total number of records that belong to each category—such as counting how many wine reviews exist per country or per winery. In Pandas, chaining `.size()` after a `.groupby()` operation is the most efficient way to generate these frequency counts.

#### 🛠️ How it Works

* **The Operation:** `.groupby('column').size()` splits the data by unique categories, counts the total number of rows (including missing/null data) in each group, and returns a clean, aggregated Series.
* **The SQL Equivalent:** This is the exact functional equivalent of using `COUNT(*)` paired with a `GROUP BY` clause in SQL.

#### 💡 Code Example

```python
import pandas as pd
import numpy as np

# Sample Data with a missing (NaN) value
data = {
    'winery': ['1+1=3', '10 Knots', '1+1=3', '10 Knots', '1+1=3'],
    'points': [88, 92, np.nan, 85, 90]
}
df = pd.DataFrame(data)

# Calculate total review count per winery
winery_counts = df.groupby('winery').size()

print(winery_counts)
# Output:
# winery
# 1+1=3      3
# 10 Knots    2
# dtype: int64
```
---
### 🏷️ Renaming Columns and Indexes (`.rename()`)

The `.rename()` method changes specific column headers or row index labels by passing a dictionary mapping the old names to the new names. 

#### 1. Renaming Columns
To alter column headers, use the `columns` parameter. This is highly useful for cleaning up raw data formatting or making business metrics explicit.

```python
# Rename specific columns for clarity
df_renamed_cols = reviews.rename(columns={
    'points': 'score',
    'region_1': 'region',
    'region_2': 'sub_region'
})
```
---
### 🗂️ Establishing Row Identifiers (`.set_index()`)

The `.set_index()` method allows you to promote one or more existing columns to become the primary row index of a DataFrame. This replaces the default numeric sequence (`0, 1, 2...`) with meaningful unique identifiers, which simplifies slicing, joining, and data alignment operations.

#### 1. Setting a Single Index Column
This is the standard approach for assigning a primary business key (such as a transaction ID, date, or unique product code) as the row anchor.

```python
# Convert the 'title' column into the primary row index
df_with_index = reviews.set_index('title')
```
---
### 🧠 Concept: Automated Directory Creation (`os.makedirs()`)

* **Definition:** A specific command within Python's `os` library that forces your computer to create a new folder (or a chain of folders) directly from your script.
* **Use Case:** Use this at the very beginning of an ingestion or cleaning script to ensure the destination folders exist. Including `exist_ok=True` ensures that if the folder is already there, Python quietly skips it instead of throwing a disruptive error. This makes your pipeline completely self-healing on any computer.
* **Code Example:**
  ```python
  import os
  
  # Automatically creates 'processed' inside 'data' if it doesn't exist
  os.makedirs('../data/processed', exist_ok=True)
---
### 🧠 Concept: Data Export Alignment (`.to_csv()`)

* **Definition:** A Pandas method that converts an in-memory DataFrame into a physical, permanent CSV spreadsheet file on your storage disk.
* **Use Case:** Use this at the end of a cleaning or transformation phase to "freeze" your progress and save the clean data for the next step of the pipeline. Always pair this with index=False so Pandas doesn't inject useless row numbers into your clean file.
* **Code Example:**
```python
# Saves the master data cleanly without row-number bloat
df_master.to_csv('../data/processed/clean_streaming_activity.csv', index=False)
```
---
### Database Connection (`sqlite3.connect()`)

* **Definition:** A method that establishes a digital pipeline (or "bridge") between your Python notebook and a specific relational database file. 
* **Use Case:** Use this whenever you need to read from or write to a database. If the database file name you pass into it doesn't exist yet, SQLite will automatically create a blank database file for you on the spot.
* **Code Example:**
  ```python
  import sqlite3
  
  # Connects to the database (and creates it if it's missing)
  conn = sqlite3.connect('../data/streaming_warehouse.db')

### Table Creation & Population (`.to_sql()`)

* **Definition:** A Pandas method that takes an in-memory DataFrame and converts it directly into a structured table inside a relational database. 
* **Use Case:** Use this to move data out of Python and into your storage warehouse. Setting if_exists='replace' tells the script to overwrite the table if it already exists, which keeps your script from crashing on a second run. Setting index=False prevents Pandas from turning its row numbers into a database column.
* **Code Example:**
```python
# Pushes df_master into a SQL table named 'stg_streaming_activity'
df_master.to_sql('stg_streaming_activity', conn, if_exists='replace', index=False)
```

### Connection Closure (`.close()`)

* **Definition:** A command that officially shuts down the active digital pipeline between your Python notebook and the database file.
* **Use Case:** Use this at the very end of your database scripts. Leaving connections open can lock the database file, preventing other tools (like dbt or SQL viewers) from accessing the data, and it can waste system memory.
* **Code Example:**
```python
# Always close the gate when you are done writing data
conn.close()
```
---
### (`display()`)

* **Definition:** A specialized IPython function designed for Jupyter Notebooks and VS Code that renders objects—like Pandas DataFrames—into highly formatted, interactive, and human-readable HTML structures instead of raw text.
* **Use Case:** Use this inside notebooks whenever you want to inspect a DataFrame, plot, or matrix with clean grid lines and readable alignment. It is especially useful when you need to output multiple distinct tables or data summaries from the exact same notebook code cell.
* **Code Example:**
  ```python
  import pandas as pd

  # Run a query and display the resulting DataFrame beautifully
  df_sample = pd.read_sql_query("SELECT * FROM stg_streaming_activity LIMIT 5;", conn)
  display(df_sample)
---
### Querying Data via SQL (`pd.read_sql_query()`)

* **Definition:** A Pandas function that executes a standard SQL query string against a relational database and instantly packages the resulting rows and columns back into a Pandas DataFrame.
* **Use Case:** Use this whenever you want to leverage the speed and filtering power of SQL directly inside Python. It lets you aggregate, filter, and join massive database tables on the database side before pulling a smaller, optimized subset of data into your notebook.
* **Code Example:**
```python
# Run a SQL query and save the results into a DataFrame
query = "SELECT * FROM stg_streaming_activity LIMIT 5;"
df_sample = pd.read_sql_query(query, conn)
```
