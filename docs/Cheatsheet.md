
## Scientific Python Cheatsheet

[IPGP](https://github.com/IPGP) / [Scientific Python Cheatsheet](https://ipgp.github.io/scientific_python_cheat_sheet/)

### NumPy


```py
# array initialization
np.array([2, 3, 4])             # direct initialization
np.empty(20, dtype=np.float32)  # single precision array with 20 entries
np.zeros(200)                   # initialize 200 zeros
np.ones((3,3), dtype=np.int32)  # 3 x 3 integer matrix with ones
np.eye(200)                     # ones on the diagonal
np.zeros_like(a)                # returns array with zeros and the shape of a
np.linspace(0., 10., 100)       # 100 points from 0 to 10
np.arange(0, 100, 2)            # points from 0 to <100 with step width 2
np.logspace(-5, 2, 100)         # 100 log-spaced points between 1e-5 and 1e2
np.copy(a)                      # copy array to new memory
```

```py
# array properties and operations
a.shape                # a tuple with the lengths of each axis
len(a)                 # length of axis 0
a.ndim                 # number of dimensions (axes)
a.sort(axis=1)         # sort array along axis
a.flatten()            # collapse array to one dimension
a.conj()               # return complex conjugate
a.astype(np.int16)     # cast to integer
np.argmax(a, axis=2)   # return index of maximum along a given axis
np.cumsum(a)           # return cumulative sum
np.any(a)              # True if any element is True
np.all(a)              # True if all elements are True
np.argsort(a, axis=1)  # return sorted index array along axis
```
