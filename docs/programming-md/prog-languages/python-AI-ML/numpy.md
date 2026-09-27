# Numpy & Pandas Notes

## Jargon

Numpy = numerical Python

These arrays are homogeneous, meaning they contain elements of the same data type, which allows for optimized storage and computation.

## Basics

- NumPy arrays have a **fixed size** at creation, unlike Python lists (which can grow dynamically). Changing the size of an ndarray will create a new array and delete the original.
- The elements in a NumPy array are all required to be of the same data type, and thus will be the same size in memory.

## Axes

NumPy axes are the directions along the rows and columns.

axes = dimensions

- Array axes in NumPy are numbered, starting at zero
- Axis 0 is the direction along the rows (Oy)
- Axis 1 is the direction along the columns (Ox)

> pay very careful attention to what the axis parameter actually controls for each function.

```python
# 2 axes
# axis 0 has length 2
# axis 1 has length 3
[[1., 0., 0.],
 [0., 1., 2.]]
```

- 2D array (matrix) => Oy đi `[0, - vô cực]`, Ox đi `[0, + vô cực]`
- 3D array (cube, lists of matrices) => Oy đi `[0, - vô cực]`, Ox đi `[0, + vô cực]`, Oz đi `[0, + vô cực]` mũi tên hướng về phía đi vào trong màn hình

Khi print 3D array thì nó print last axis (Oz) top to bottom

## NDArray

- attributes of an `ndarray` object:
  - `ndarray.ndim`: the number of axes (dimensions) of the array.
  - `ndarray.shape`: the dimensions of the array. This is a tuple of integers indicating the size of the array in each dimension. For a matrix with n rows and m columns, `shape` will be `(n,m)` or `(row, column)`. The length of the `shape` tuple is therefore the number of axes, ndim.
  - `ndarray.size`: the total number of elements of the array. This is equal to the product of the elements of `shape`.
  - `ndarray.dtype`: an object describing the type of the elements in the array. One can create or specify dtype’s using standard Python types. Additionally NumPy provides types of its own. numpy.int32, numpy.int16, and numpy.float64 are some examples.

```python
# 3 rows, 5 columns
>>> a = np.arange(15).reshape(3, 5)
>>> a
array([[ 0,  1,  2,  3,  4],
       [ 5,  6,  7,  8,  9],
       [10, 11, 12, 13, 14]])
```

The syntax is: `.reshape(1st axis, 2nd axis, 3rd axis)` or `.reshape(Oy, Ox, Oz)` or `.reshape(number of row, number of column, height)`

---

- `NDArray` stands for `N-Dimensional Array`.
  - 1D array: `array([20.1, 19.5, 25.3, 12.3]) # shape (4,)`
  - 2D array (matrix); a grid with rows and columns (e.g., a table): `[ [1,2], [3,4] ]`
  - 3D (tensor):

- A shape of `(3,)` means it is a 1-dimensional array containing 3 elements.
- `shape (1,3)` => 2D, 1 row, 3 columns
  - `shape (4, 5)` => 4 rows, 5 columns (4x5 matrix)
- `shape (3,4,5)` => 3 layers, each layer is 4x5 matrix

`some_array[2,1,0]` => 3rd layer, second row, first column (zero-indexed)

## Data Types

Pandas typically uses these common `dtype` objects:

- `object`: Often used for text or mixed-type columns. Usually represents categorical data (both nominal and ordinal).
- `int64`: Represents integer values. Often corresponds to discrete numerical data.
  - `float64`: Represents floating-point (decimal) numbers. Often corresponds to continuous numerical data.
- `bool`: Represents Boolean (True/False) values. This is a type of categorical data.
- `category`: A specific Pandas type optimized for `categorical data` (can represent nominal or ordinal).
- `datetime64`: Represents date and time values.

## Data Structure

In pure `NumPy` or standard Python lists, data is purely positional (index 0, index 1, index 2). If you reorder, filter, or combine two lists, matching up the data depends entirely on you keeping track of the array indices.  
In Pandas, **the label is glued to the value**. The relationship between an observation and its identity travels together automatically through almost every operation.

---

The most common data structure in Pandas is the `DataFrame`. A DataFrame is essentially a two-dimensional table with labeled axes (rows and columns). You can load data into a DataFrame from various sources, including CSV (Comma Separated Values) files, Excel spreadsheets, databases, and more.

`Series` is a one-dimensional labeled array capable of holding any data type (integers, strings, floating point numbers, Python objects, etc.).

## Jupyter

a `.ipynb` (Interactive Python Notebook) file is a raw JSON file.

`jupyter lab` => open in a web browser

## Pandas

k
