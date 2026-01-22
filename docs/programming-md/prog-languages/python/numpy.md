# Numpy Notes

## Jargon

Numpy = numerical Python

`ndarray` = n-dimensional array

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

- 2D array => Oy đi `[0, - vô cực]`, Ox đi `[0, + vô cực]`
- 3D array => Oy đi `[0, - vô cực]`, Ox đi `[0, + vô cực]`, Oz đi `[0, + vô cực]` mũi tên hướng về phía đi vào trong màn hình

## `ndarray`

- attributes of an `ndarray` object:
  * `ndarray.ndim`: the number of axes (dimensions) of the array.
  * `ndarray.shape`: the dimensions of the array. This is a tuple of integers indicating the size of the array in each dimension. For a matrix with n rows and m columns, `shape` will be `(n,m)` or `(row, column)`. The length of the `shape` tuple is therefore the number of axes, ndim.
  * `ndarray.size`: the total number of elements of the array. This is equal to the product of the elements of `shape`.
  * `ndarray.dtype`: an object describing the type of the elements in the array. One can create or specify dtype’s using standard Python types. Additionally NumPy provides types of its own. numpy.int32, numpy.int16, and numpy.float64 are some examples.

```python
# 3 rows, 5 columns
>>> a = np.arange(15).reshape(3, 5)
>>> a
array([[ 0,  1,  2,  3,  4],
       [ 5,  6,  7,  8,  9],
       [10, 11, 12, 13, 14]])
```

### Printing Arrays

f
