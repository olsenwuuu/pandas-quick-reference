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
