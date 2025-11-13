# 🐼 Complete Beginner-Friendly Pandas Tutorial

Learn Pandas step by step with beginner-friendly examples. This tutorial covers Series, DataFrames, CSV handling, data cleaning, merging, grouping, and reshaping.

---

## ⚙️ Requirements

- Python 3.8+
- pandas >= 1.3
- Jupyter Notebook or VS Code recommended

---

## 📘 Table of Contents

- [1️⃣ Pandas Series](#1-pandas-series)  
- [2️⃣ Pandas DataFrame Creation](#2-pandas-dataframe-creation)  
- [3️⃣ Column Operations](#3-column-operations)  
- [4️⃣ CSV Export & Import](#4-csv-export--import)  
- [5️⃣ Data Cleaning & Replacement](#5-data-cleaning--replacement)  
- [6️⃣ Interpolation](#6-interpolation)  
- [7️⃣ Merging & Concatenation](#7-merging--concatenation)  
- [8️⃣ Joining & Appending](#8-joining--appending)  
- [9️⃣ Grouping & Aggregation](#9-grouping--aggregation)  
- [1️⃣0️⃣ Reshaping Data: Melt & Pivot Tables](#10-reshaping-data-melt--pivot-tables)

---

<details>
<summary><strong>1️⃣ Pandas Series</strong></summary>

```python
import pandas as pd

# Create a Series with custom index and name
x = [1, 2, 3, 4, 5]
var = pd.Series(x, index=['a','b','c','d','e'], dtype=float, name="python")
print(var)
```

**Sample Output:**

```
a    1.0
b    2.0
c    3.0
d    4.0
e    5.0
Name: python, dtype: float64
```

**Key Concepts:**

- Series creation with `pd.Series()`
- Custom index and name
- Specify `dtype`
- Operations like replace, fill, and aggregation

</details>

---

<details>
<summary><strong>2️⃣ Pandas DataFrame Creation</strong></summary>

```python
# From list
var = pd.DataFrame([1,2,3,4,5,6,7])

# From dictionary
var1 = pd.DataFrame({
    "a":[1,2,3,4],
    "b":[5,6,7,8],
    "c":[9,10,11,12]
}, columns=["b","a","c"], index=["r1","r2","r3","r4"])
print(var1)
```

**Sample Output:**

```
   b  a   c
r1 5  1   9
r2 6  2  10
r3 7  3  11
r4 8  4  12
```

**Key Concepts:**

- Create DataFrame from lists, dicts, or Series
- Specify column order and index labels
- Access and modify cells

</details>

---

<details>
<summary><strong>3️⃣ Column Operations</strong></summary>

```python
# Add new column using arithmetic
var1["D"] = var1["a"] % var1["b"]

# Add column with condition
var1["Python"] = var1["a"].le(3)  # True if 'a' <= 3
print(var1)
```

**Sample Output:**

```
   b  a   c  D  Python
r1 5  1   9  1   True
r2 6  2  10  2   True
r3 7  3  11  3   True
r4 8  4  12  4  False
```

**Tips:**

- `.insert()` can place a column at a specific location
- `.le()`, `.ge()`, `.lt()`, `.gt()` for conditional operations
- Modify or delete columns easily

</details>

---

<details>
<summary><strong>4️⃣ CSV Export & Import</strong></summary>

```python
# Export DataFrame to CSV
var1.to_csv("test.csv", index=False, header=["col1","col2","col3","col4","col5"])

# Read CSV
read_csv = pd.read_csv("test.csv")
print(read_csv)
```

**Sample Output:**

```
   col1  col2  col3  col4  col5
0     5     1     9     1  True
1     6     2    10     2  True
2     7     3    11     3  True
3     8     4    12     4 False
```

**Key Concepts:**

- Control rows with `nrows`
- Select columns with `usecols`
- Skip rows with `skiprows`
- Set index column with `index_col`
- Override headers with `names`

</details>

---

<details>
<summary><strong>5️⃣ Data Cleaning & Replacement</strong></summary>

```python
# Replace values
var1.replace(to_replace=True, value="Yes", inplace=True)
var1.replace([1,2,3], value=100, inplace=True)
print(var1)
```

**Sample Output:**

```
   b    a   c   D  Python
0  5  100   9 100    Yes
1  6  100  10 100    Yes
2  7  100  11 100    Yes
3  8    4  12   4  False
```

**Key Concepts:**

- Replace exact values or using regex
- Forward-fill (`ffill`) and backward-fill (`bfill`)
- Limit number of fills with `limit`

</details>

---

<details>
<summary><strong>6️⃣ Interpolation</strong></summary>

```python
# Introduce missing values
var1["D"] = [1.0, None, 3.0, None]

# Interpolate missing values
var1.interpolate(method="linear", inplace=True)
print(var1)
```

**Sample Output:**

```
   b    a   c    D  Python
0  5  100   9  1.0    Yes
1  6  100  10  2.0    Yes
2  7  100  11  3.0    Yes
3  8    4  12  3.0  False
```

**Tips:**

- Fill missing values with linear trends
- Control direction and limit with `method` and `limit`

</details>

---

<details>
<summary><strong>7️⃣ Merging & Concatenation</strong></summary>

```python
var2 = pd.DataFrame({"A":[1,2,3], "B":[10,11,12]})

# Merge by index
merged = pd.merge(var1, var2, left_index=True, right_index=True, suffixes=("_left","_right"))
print(merged)
```

**Sample Output:**

```
   b_left    a_left  c  D  Python  A  B
0       5      100   9  1    Yes  1 10
1       6      100  10  2    Yes  2 11
2       7      100  11  3    Yes  3 12
3       8        4  12  3  False  3 12
```

**Key Concepts:**

- Merge by column or index
- Concatenate along rows or columns
- Handle overlapping columns with `suffixes`

</details>

---

<details>
<summary><strong>8️⃣ Joining & Appending</strong></summary>

```python
# Join DataFrames by index
joined = var1.join(var2, how="outer", lsuffix="_var1", rsuffix="_var2")

# Append vertically
appended = pd.concat([var1, var2], ignore_index=True)

print(joined)
print(appended)
```

**Sample Output (Joined):**

```
   b_var1  a_var1   c    D  Python  A   B
0       5     100   9  1.0    Yes  1  10
1       6     100  10  2.0    Yes  2  11
2       7     100  11  3.0    Yes  3  12
3       8       4  12  3.0  False  3  12
```

**Tips:**

- Use `how="left|right|outer|inner"` for join
- `ignore_index=True` resets the index after append

</details>

---

<details>
<summary><strong>9️⃣ Grouping & Aggregation</strong></summary>

```python
# Group by a column
grouped = var1.groupby("Python")

# Access specific group
print(grouped.get_group("Yes"))

# Aggregate
print(grouped.max())
```

**Sample Output (Group "Yes"):**

```
   b    a   c    D Python
0  5  100   9  1.0   Yes
1  6  100  10  2.0   Yes
2  7  100  11  3.0   Yes
```

**Tips:**

- `.groupby()` to group data
- Access groups with `.get_group()`
- Aggregate using `.sum()`, `.max()`, `.min()`, `.mean()`

</details>

---

<details>
<summary><strong>1️⃣0️⃣ Reshaping Data: Melt & Pivot Tables</strong></summary>

```python
# Melt: wide → long
melted = pd.melt(var1, id_vars=['Python'], var_name="variable", value_name="value")

# Pivot table with aggregation
pivot_table = var1.pivot_table(index='D', values=['a','b'], aggfunc='sum', margins=True)

print(melted)
print(pivot_table)
```

**Sample Output (Melted):**

```
   Python variable  value
0     Yes      b     5
1     Yes      b     6
2     Yes      b     7
...
```

**Sample Output (Pivot Table):**

```
        a    b
D             
1     100    5
2     100    6
3     104   19
All   304   30
```

**Tips:**

- `pd.melt()` converts wide → long format
- Pivot tables aggregate and summarize data
- `margins=True` adds totals

</details>
