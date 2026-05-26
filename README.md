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

