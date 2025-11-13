---

````markdown
# 🐼 Complete Beginner-Friendly Pandas Tutorial

This repository is a **comprehensive guide to Python Pandas**, covering everything from Series and DataFrames to CSV handling, data cleaning, merging, grouping, and reshaping.  
It is designed for beginners who want to **learn Pandas step by step**.

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
- [10️⃣ Reshaping Data: Melt & Pivot Tables](#10-reshaping-data-melt--pivot-tables)

---

<details>
<summary><strong>1️⃣ Pandas Series</strong></summary>

```python
import pandas as pd

x = [1,2,3,4,5]
var = pd.Series(x, index=['a','b','c','d','e'], dtype=float, name="python")
print(var)
````

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

* Series creation with custom index and name
* Specify `dtype`
* Operations like replace, fill, and aggregation

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

* Lists, dictionaries, and Series as input
* Column order and index labels
* Accessing and modifying cells

</details>

---

<details>
<summary><strong>3️⃣ Column Operations</strong></summary>

```python
var1["D"] = var1["a"] % var1["b"]
var1["Python"] = var1["a"].le(3)
print(var1)
```

**Sample Output:**

```
   b  a   c  D  Python
r1 5  1   9  1    True
r2 6  2  10  2    True
r3 7  3  11  3    True
r4 8  4  12  4   False
```

* Add, modify, delete columns
* Conditional operations (`.le()`)
* `.insert()` can place a column at a specific location

</details>

---

<details>
<summary><strong>4️⃣ CSV Export & Import</strong></summary>

```python
var1.to_csv("test.csv", index=False, header=["col1","col2","col3","col4","col5"])
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

* Control rows with `nrows`
* Select columns with `usecols`
* Skip rows with `skiprows`
* Set index column with `index_col`
* Override headers with `names`

</details>

---

<details>
<summary><strong>5️⃣ Data Cleaning & Replacement</strong></summary>

```python
var1.replace(to_replace="True", value="Yes", inplace=True)
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

* Replace exact values or regex
* Forward-fill (`ffill`) and backward-fill (`bfill`)
* Limit number of fills with `limit`

</details>

---

<details>
<summary><strong>6️⃣ Interpolation</strong></summary>

```python
var1["D"] = [1, None, 3, None]
var1.interpolate(method="linear", axis=0, inplace=True)
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

* Fill missing values with linear trends
* Control direction and limit of interpolation

</details>

---

<details>
<summary><strong>7️⃣ Merging & Concatenation</strong></summary>

```python
var2 = pd.DataFrame({"A":[1,2,3], "B":[10,11,12]})
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

* Merge by column or index
* Concatenate along rows or columns
* Handle overlapping columns with suffixes

</details>

---

<details>
<summary><strong>8️⃣ Joining & Appending</strong></summary>

```python
joined = var1.join(var2, how="outer", lsuffix="_var1", rsuffix="_var2")
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

* Join by index
* Outer, left, right joins
* Append vertically and reset index

</details>

---

<details>
<summary><strong>9️⃣ Grouping & Aggregation</strong></summary>

```python
grouped = var1.groupby("Python")
print(grouped.get_group("Yes"))
print(grouped.max())
```

**Sample Output (Group "Yes"):**

```
   b    a   c    D Python
0  5  100   9  1.0   Yes
1  6  100  10  2.0   Yes
2  7  100  11  3.0   Yes
```

* `.groupby()` to group data
* Access specific groups
* Aggregate using `sum()`, `max()`, `min()`

</details>

---

<details>
<summary><strong>10️⃣ Reshaping Data: Melt & Pivot Tables</strong></summary>

```python
melted = pd.melt(var1, id_vars=['Python'], var_name="variable", value_name="value")
pivot_table = var1.pivot_table(index=['D'], values=['a','b'], aggfunc='sum', margins=True)
print(melted)
print(pivot_table)
```

**Sample Output (Melted):**

```
   Python variable  value
0     Yes      b   5
1     Yes      b   6
...
```

**Sample Output (Pivot Table):**

```
      a    b
D           
1   100    5
2   100    6
3   104   19
All 304   30
```

* Melt converts wide → long
* Pivot table aggregates and summarizes data
* Use `margins=True` to add totals

</details>

---

## 🚀 How to Run

1. Install Python 3.8+
2. Install Pandas:

```bash
pip install pandas
```

3. Run any script:

```bash
python script_name.py
```

---

## 📂 Suggested File Structure

```
Pandas-Tutorial/
├── series_demo.py
├── dataframe_demo.py
├── csv_demo.py
├── cleaning_demo.py
├── merge_concat_demo.py
├── join_append_demo.py
├── groupby_demo.py
├── reshape_demo.py
└── README.md
```

---

