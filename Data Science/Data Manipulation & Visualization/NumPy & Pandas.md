# NumPy
## Array Initialization
```Python
import numpy as np
np.random.seed(42)  # seed for reproducibility
x1 = np.random.randint(10, size=6)  # One-dimensional array
x2 = np.random.randint(10, size=(3, 4))  # Two-dimensional array
x3 = np.random.randint(10, size=(3, 4, 5))  # Three-dimensional array
# np.random.randint(low, [high], size, dtype) :
# Return random integers from the “discrete uniform” distribution of the specified dtype in the “half-open” interval [low, high).
# If high is None (the default), then results are from [0, low).

x = np.arange(5)
# [0, 1, 2, 3, 4]
x = np.ones(1,2,3) # also np.zeros
# [[[1., 1., 1.], [1., 1., 1.]]]
x = np.zeros_like(x)
# [[[0., 0., 0.], [0., 0., 0.]]]
np.array([1, 2, 3])
# [1, 2, 3]
```
## Array Attributes
```Python
x3:
ndim(the number of dimensions):  3
shape(the size of each dimension): (3, 4, 5)
size(the total size of the array)::  60
dtype: int32
itemsize: 4 (bytes)
nbytes: 240 (bytes)
```
## Array Indexing & Slicing
```Python
# [dimension1, dimension2, dimension3]
x[0, 0, 0]
x[start:stop:step, start:stop:step, start:stop:step]
# assigning array slicing to a variable DOES NOT create a new array like lists in vanilla Python
# use x.copy() to copy
```
  
```Python
grid = np.arange((1,10))
grid = grid.reshape((3,3))
x = np.array([1, 2, 3])
x = x[:, np.newaxis]
# array([[1], [2], [3]])

x = np.ones(1,2,3) # x.shape: (1, 2, 3)
x = np.transpose(x, (1,0,2)) # x.shape (2, 1, 3)

# pivot: convert a row to a col
```
## Array Operations
```Python
.concatenate
.vstack
.hstack
.vsplit
.hsplit
```

```python
np.power(arr, exponent)
np.exp(arr)
```
## Mask
```Python
x < 3 # [True, True, True, False, False]
x[x < 3] # [0, 1, 2]
```
isna:
This function takes `Series` or `DataFrame` and indicates whether values are missing (`NaN` in numeric arrays, `None` or `NaN` in object arrays, `NaT` in datetimelike)
# Pandas
>Based on Numpy

```Python
df.to_numpy()
```
## Info
```Python
import pandas as pd
# init df 1
source = 'https://raw.githubusercontent.com/justmarkham/DAT8/master/data/chipotle.tsv'
# source = 'your/local/path/to/iris.data'
df = pd.read_csv(source, sep='\t')
# init df 2
df = pd.DataFrame([['October 1', 67], ['October 2', 72], ['October 3', 58], ['October 4', 69], ['October 5', 77]], index = ['Day 1', 'Day 2', 'Day 3', 'Day 4', 'Day 5'], columns = ['Month', 'Temperature'])
# init df 3
df = pd.DataFrame({'foo': ['one', 'one', 'one', 'two', 'two', 'two'],
                    'bar': ['A', 'B', 'C', 'A', 'B', 'C'],
                    'baz': [1, 2, 3, 4, 5, 6],
                    'zoo': ['x', 'y', 'z', 'q', 'w', 't']})

# number of entries
df.shape[0]
# number of columns
df.shape[1]
# list columns
df.columns
# more info
df.info()
# columns
df.<column name>
```
## Select, Aggregation & Sorting
```Python
# row select
df.head(10) # first 10 entries(default 5)
df.tail() # last entries(default 5)
df[(df['item_name'] == 'Chicken Bowl') & (df['quantity'] == 1)]

# col select
df[['name', 'age']]

# label based selection
df.loc[[0, 2]]
df.loc[5]
df.loc[df['age'] > 30]

# position based selection
df.iloc[rowstart:rowend:rowstep, colstart:colend:colstep]

# random row select
df.sample(frac=0.5)
df.sample(n=0.5)

# aggregation
## aggregation functions: sum, value_counts, count, mean, max, min
df.groupby('department')['salary'].sum()
df.groupby('department')['salary'].agg(['mean', 'sum', 'max'])
df.groupby('department').agg({
    'salary': ['mean', 'max'],
    'bonus': 'sum'
})

# sort cols
df.sort_values(by = "item_price", ascending = False)
```
## Reshape & Update
```Python
# create/modify col
employees['newcol'] = employees['oldcol'] * 2
employees['newcol'] = [9096, 56300, 2206, 13186, 149152, 48866]

# drop dup rows
customers.drop_duplicates(subset=["email"], keep="last")

# concat 
pd.concat([df1, df2])
```

**pivot**: the process of turning a col of values into column labels
```
# pivot: 
# return a table formed by index, columns as columns labels and values
# `index` 是你希望展开为索引的列，`columns` 是你希望展开为列标签的列，`values` 是你希望填充到新表中的值
weather.pivot(index="month", columns="city", values="temperature")
# more powerful than pivot: allow values to aggregate
weather.pivot_table(index='month', columns='city', values='temperature', aggfunc='sum')

# melt: unpivot value_vars from column labels
# `id_vars` 是你希望保留为标识列的列，`value_vars`是将要unpivot的列标签元素，`var_name` 是你希望将剩余的宽列变成的新列名称，`value_name` 是原始数据中每个宽列值的名称。
# return a table of column labels from id_vars, var_name and value_name
report.melt(id_vars=['product'], value_vars=['quarter'], var_name="quarter", value_name="sales")
```
## Plot

> Pandas uses Matplotlib behind the scenes to make plots
```Docker
DataFrame.plot(kind, style)
```