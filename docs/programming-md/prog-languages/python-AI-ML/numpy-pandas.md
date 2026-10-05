# Numpy & Pandas Notes

## Jargon

Numpy = numerical Python

These arrays are homogeneous, meaning they contain elements of the same data type, which allows for optimized storage and computation.

## Basics

- NumPy arrays have a **fixed size** at creation, unlike Python lists (which can grow dynamically). Changing the size of an ndarray will create a new array and delete the original.
- The elements in a NumPy array are all required to be of the same data type, and thus will be the same size in memory.

NumPy exists to solve a single fundamental dilemma: Python is productive for humans to write, but notoriously slow for computers to crunch numbers.  
The point of `NumPy` and related libraries is to be able to write concise, simple python syntax but running `C` underneath for maximum speed.

### Broadcasting

Subject to certain constraints, the smaller array is “broadcast” across the larger array so that they have compatible shapes. Broadcasting provides a means of vectorizing array operations so that looping occurs in C instead of Python.

Treating arrays as mathematical vectors rather than programmatic lists, and executing hardware instructions on those vectors simultaneously.

```python
import numpy as np
a = np.array([1.0, 2.0, 3.0])
b = 2.0
a * b
```

## Data Types

Pandas typically uses these common `dtype` objects:

- `object`: Often used for text or mixed-type columns. Usually represents categorical data (both nominal and ordinal).
- `int64`: Represents integer values. Often corresponds to discrete numerical data.
  - `float64`: Represents floating-point (decimal) numbers. Often corresponds to continuous numerical data.
- `bool`: Represents Boolean (True/False) values. This is a type of categorical data.
- `category`: A specific Pandas type optimized for `categorical data` (can represent nominal or ordinal).
- `datetime64`: Represents date and time values.

## NDArray (N-Dimensional Array)

### Basics NDArray

NumPy’s main object is the homogeneous multidimensional array. It is a table of elements (usually numbers), all of the same type, indexed by a tuple of non-negative integers. In NumPy dimensions are called `axes`.

- Array axes in NumPy are numbered, starting at zero
- Axis 0 is the direction along the rows (Oy)
- Axis 1 is the direction along the columns (Ox)

- 1D array: `array([20.1, 19.5, 25.3, 12.3]) # shape (4,)`
- 2D array (matrix); a grid with rows and columns (e.g., a table): `[ [1,2], [3,4] ]`
- 3D (tensor):

`[1, 2, 1]`, has one axis. That axis has 3 elements in it, so we say it has a length of 3.

 In the example below, the array has 2 axes. The first axis has a length of 2, the second axis has a length of 3.

```python
[[1., 0., 0.],
 [0., 1., 2.]]
```

---

The number of dimensions and items in an array is defined by its `shape`, which is a tuple of N non-negative integers that specify the **sizes of each dimension**.

- A shape of `(3,)` means it is a 1-dimensional array containing 3 elements. `[1, 2, 3]`
  - Has one `axis 0` (`ndim = 1`)
  - The concepts of "rows" and "columns" strictly exist only in 2D arrays. A `(3,)` array is a 1D array
- `(150,)` => 1D array with 150 elements (row vector)
  - `(150, 1)` => column vector

- Shape `(1,3)` => 2D array (matrix); 1 row, 3 columns `[[1, 2, 3]]`
  - axis 0, axis 1
- `(4, 5)` => 4 rows, 5 columns (4x5 matrix)
- `(150, 4)` => 150 rows, 4 columns

- `shape (3,4,5)` => 3 layers, each layer is 4x5 matrix

`some_array[2,1,0]` => 3rd layer, second row, first column (zero-indexed)

---

- Attributes of an `ndarray` object:
  - `ndarray.ndim`: the number of axes (dimensions) of the array.
  - `ndarray.shape`: This is a tuple of integers indicating the size of the array in each dimension. For a matrix with $n$ rows and $m$ columns, `shape` will be `(n,m)` or `(row, column)`. The length of the `shape` tuple is therefore the number of axes, `ndim`.
  - `ndarray.size`: the total number of elements of the array. This is equal to the product of the elements of `shape`.
  - `ndarray.dtype`: an object describing the type of the elements in the array. An ndarray contains items of the same type and size.

```python
# 3 rows, 5 columns
>>> a = np.arange(15).reshape(3, 5)
>>> a
array([[ 0,  1,  2,  3,  4],
       [ 5,  6,  7,  8,  9],
       [10, 11, 12, 13, 14]])
```

---

`numpy.reshape()` Returns a reshaped ndarray without changing data.

The syntax is: `.reshape(1st axis, 2nd axis, 3rd axis)` or `.reshape(Oy, Ox, Oz)` or `.reshape(number of row, number of column, height)`

---

```python
# 2 axes
# axis 0 has length 2
# axis 1 has length 3
[[1., 0., 0.],
 [0., 1., 2.]]
```

- 2D array (matrix) => Oy đi `[0, - vô cực]`, Ox đi `[0, + vô cực]`
- 3D array (cube, lists of matrices) => Oy đi `[0, - vô cực]`, Ox đi `[0, + vô cực]`, Oz đi `[0, + vô cực]` mũi tên hướng về phía đi vào trong màn hình

---

Khi print 3D array thì nó print last axis (Oz) top to bottom

### Shape Manipulation

k

## Pandas

In pure `NumPy` or standard Python lists, data is purely positional (index 0, index 1, index 2). If you reorder, filter, or combine two lists, matching up the data depends entirely on you keeping track of the array indices.  
In Pandas, **the label is glued to the value**. The relationship between an observation and its identity travels together automatically through almost every operation.

## DataFrame

The most common data structure in Pandas is the `DataFrame`. A DataFrame is essentially a two-dimensional table with labeled axes (rows and columns). You can load data into a DataFrame from various sources, including CSV (Comma Separated Values) files, Excel spreadsheets, databases, and more.

- NumPy 2D `ndarray`
  - Homogeneous: Every single element must share the exact same `dtype` (e.g., all `float64`).
- Pandas `DataFrame`
  - Heterogeneous: Each column can have its own `dtype` (e.g., Column A is `int`, B is `float`, C is `str`).

### Indexing and selecting data

In Python, `__getitem__` is a special magic (dunder) method that allows your custom objects to use square-bracket indexing and slicing (`obj[key]`). When you write `obj[key]`, Python automatically translates it to `obj.__getitem__(key)` under the hood.

`df[label]` evaluates to `df.__getitem__(label)`

---

k

## Series

`Series` is a one-dimensional labeled array capable of holding any data type (integers, strings, floating point numbers, Python objects, etc.).

A single column in a data frame is a series. So `df['Column_name]` is a series and you can call `.seri_method()` on it.

## Other methods

`np.arange()` returns an `ndarray` of evenly spaced values.

`arange` can be called with a varying number of positional arguments:

- `arange(stop)`: Values are generated within the half-open interval `[0, stop)` (in other words, the interval including start but excluding stop).

Python's built-in `range()` có chức năng tương tự `np.arange()` nhưng chỉ nên dùng trong `for` loops. Còn ngoài ra thì cứ dùng `arange()`.

---

df.duplicated() returns boolean Series denoting duplicate rows. By default, for each set of duplicated values, the first occurrence is set on `False` and all others on `True`.

## Jupyter

a `.ipynb` (Interactive Python Notebook) file is a raw JSON file.

`jupyter lab` => open in a web browser
