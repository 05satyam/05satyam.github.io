---
title: "Ensemble Data-Structures Basics"
date: 2024-06-03
categories: Others
---

# 1. Pandas DataFrame

A DataFrame is a two-dimensional, size-mutable, and potentially heterogeneous tabular data structure with labeled axes (rows and columns). It is a primary data structure in the Pandas library, which is widely used in data manipulation and analysis in Python. Think of a DataFrame as a table, similar to a spreadsheet or a SQL table, where each column can have a different data type (e.g., integers, floats, strings).

## Key Features of a DataFrame:

- **Labeled Axes:** Both rows and columns are labeled, which makes it easy to manipulate and access data using these labels.
- **Size-Mutable:** You can add or remove rows and columns.
- **Heterogeneous Data:** Different columns can store different types of data (e.g., numerical, categorical).
- **Alignment:** DataFrames automatically align data for arithmetic operations based on the row and column labels.

## Creating a DataFrame

There are multiple ways to create a DataFrame in Pandas:

### From a Dictionary:

```python
import pandas as pd

data = {
    'Name': ['Alice', 'Bob', 'Charlie'],
    'Age': [25, 30, 35],
    'City': ['New York', 'San Francisco', 'Los Angeles']
}

df = pd.DataFrame(data)
print(df)

# Output
    Name  Age           City
0   Alice   25       New York
1     Bob   30  San Francisco
2  Charlie   35    Los Angeles
```

### From a list of lists
```python
data = [
    ['Alice', 25, 'New York'],
    ['Bob', 30, 'San Francisco'],
    ['Charlie', 35, 'Los Angeles']
]

df = pd.DataFrame(data, columns=['Name', 'Age', 'City'])
print(df)

#Output:
    Name  Age           City
0   Alice   25       New York
1     Bob   30  San Francisco
2  Charlie   35    Los Angeles

```

### From Sql Queries:
```python
import sqlite3

conn = sqlite3.connect('database.db')
query = "SELECT * FROM table_name"
df = pd.read_sql(query, conn)
print(df)

```

# 2. Heap

ToDo;
