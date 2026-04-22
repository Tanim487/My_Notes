# NumPy

NumPy is Python's core library for **numerical computing**. It provides fast, powerful N-dimensional arrays and mathematical operations.

```python
import numpy as np
```

---

## Creating Arrays

```python
np.array([1, 2, 3, 4])          # from a list
np.zeros(5)                      # [0. 0. 0. 0. 0.]
np.ones(5)                       # [1. 1. 1. 1. 1.]
np.identity(5)                   # 5x5 identity matrix
np.arange(1, 20, 2)              # like range() → [1, 3, 5 ... 19]
np.linspace(10, 20, 10)          # 10 evenly spaced values between 10 and 20
arr2 = arr1.copy()               # copy an array (not a reference)
```

---

## Array Properties & Attributes

```python
arr.shape        # dimensions as tuple e.g. (3, 3)
arr.ndim         # number of dimensions
arr.size         # total number of elements
arr.itemsize     # size of each element in bytes
arr.dtype        # data type of elements

arr.astype('float')    # type casting
arr.reshape(3, 3)      # reshape into 2D 3×3 matrix
```

---

## Iterating

```python
for i in np.nditer(arr):
    print(i)
```

---

## Slicing

```python
arr[1:3, 2:4]    # rows 1-2, columns 2-3  (2D slicing)
```

---

## Operations

### Basic Operations

```python
arr1 - arr2        # element-wise subtraction
arr1 * 2           # scalar multiplication
arr1 * arr2        # element-wise multiplication
arr1 *= arr2       # in-place multiplication
arr1 < 32          # boolean comparison → returns bool array
arr1.dot(arr2)     # dot product
```

### Unary Operations

```python
arr1.min()              # minimum value
arr1.max()              # maximum value
arr1.sum(axis=0)        # sum column-wise  (axis=0 → columns, axis=1 → rows)
arr1.mean()             # mean/average
arr1.std()              # standard deviation
```

---

## Universal Functions

```python
np.exp(arr1)        # e^x for each element
np.sqrt(arr1)       # square root of each element
np.sin(arr1)        # sine of each element
np.median(arr1)     # median value
```

---

## Reshaping & Rearranging

```python
arr1.ravel()           # flatten to 1D
arr1.reshape(6, 4)     # reshape to 6×4
arr1.transpose()       # swap rows and columns
```

### Stacking

```python
np.hstack((arr1, arr2))    # stack horizontally (side by side)
np.vstack((arr1, arr2))    # stack vertically (on top of each other)
```

### Splitting

```python
np.hsplit(arr1, 2)    # split into 2 horizontally
np.vsplit(arr1, 3)    # split into 3 vertically
```

---

## Fancy Indexing

```python
arr1[[0, 2, 4]]                          # select rows 0, 2, 4

arr = np.random.randint(low=1, high=100, size=20).reshape(4, 5)

arr[arr > 50]                            # all elements greater than 50
arr[(arr > 50) & (arr % 2 == 0)]         # elements > 50 AND even
arr[(arr > 50) & (arr % 2 == 0)] = 0    # set those elements to 0
```

---

## Random & Important Functions

```python
np.random.random()                          # random float [0, 1) — changes every run
np.random.seed(1)                           # fix the seed
np.random.random()                          # same value every run after seed

np.random.uniform(3, 10, 20).reshape(4, 5) # 20 floats between 3 and 10
np.random.randint(1, 10, 6).reshape(2, 3)  # 6 ints between 1 and 10

a = np.random.randint(1, 10, 20)
np.max(a)                     # maximum value
np.min(a)                     # minimum value
np.argmax(a)                  # index of max value
np.argmin(a)                  # index of min value
np.where(a % 2 == 1, -1, a)  # replace odd numbers with -1, keep even
np.sort(a)                    # sort array
np.percentile(a, 25)          # 25th percentile (Q1)
```