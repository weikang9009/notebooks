---
redirect_from:
  - "/model/spint/sparse-categorical"
interact_link: content/model/spint/sparse_categorical.ipynb
title: 'sparse_categorical'
prev_page:
  url: /model/spint/OD_weights
  title: 'OD_weights'
next_page:
  url: /model/spint/sparse_categorical_bottleneck
  title: 'sparse_categorical_bottleneck'
comment: "***PROGRAMMATICALLY GENERATED, DO NOT EDIT. SEE ORIGINAL FILES IN /content***"
---



{:.input_area}
```python
import numpy as np
from scipy import sparse as sp
from statsmodels.tools.tools import categorical
from datetime import datetime as dt

```




{:.input_area}
```python
def spcategorical(data):
    '''
    Returns a dummy matrix given an array of categorical variables.
    Parameters
    ----------
    data : array
        A 1d vector of the categorical variable.

    Returns
    --------
    dummy_matrix
        A sparse matrix of dummy (indicator/binary) float variables for the
        categorical data.  

    '''
    if np.squeeze(data).ndim == 1:
        tmp_arr = np.unique(data)
        tmp_dummy = sp.csr_matrix((0, len(data)))
        for each in tmp_arr[:, None]:
            row = sp.csr_matrix((each == data).astype(float))
            tmp_dummy = sp.vstack([tmp_dummy, row])
        tmp_dummy = tmp_dummy.T
        return tmp_dummy
    else:
        raise IndexError("The index %s is not understood" % col)

```




{:.input_area}
```python
data = np.random.randint(1,100, 10000)
np.allclose(spcategorical(np.array(data)).toarray(), categorical(np.array(data), drop=True))
```





{:.output_data_text}
```
True
```





{:.input_area}
```python
s = dt.now()
n = 3000
o = np.tile(np.arange(n),n)
o_dums = spcategorical(np.array(o))
n = 3000
d = np.repeat(np.arange(n),n)
d_dums = spcategorical(np.array(d))
sp.hstack((o_dums, d_dums))
e = dt.now()
print e-s
```



{:.output_traceback_line}
```
---------------------------------------------------------------------------
```

{:.output_traceback_line}
```
KeyboardInterrupt                         Traceback (most recent call last)
```

{:.output_traceback_line}
```
<ipython-input-4-a64b538a6ade> in <module>()
      2 n = 3000
      3 o = np.tile(np.arange(n),n)
----> 4 o_dums = spcategorical(np.array(o))
      5 n = 3000
      6 d = np.repeat(np.arange(n),n)

```

{:.output_traceback_line}
```
<ipython-input-2-68702ba242f4> in spcategorical(data)
     18         tmp_dummy = sp.csr_matrix((0, len(data)))
     19         for each in tmp_arr[:, None]:
---> 20             row = sp.csr_matrix((each == data).astype(float))
     21             tmp_dummy = sp.vstack([tmp_dummy, row])
     22         tmp_dummy = tmp_dummy.T

```

{:.output_traceback_line}
```
/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages/scipy/sparse/compressed.pyc in __init__(self, arg1, shape, dtype, copy)
     67                         self.format)
     68             from .coo import coo_matrix
---> 69             self._set_self(self.__class__(coo_matrix(arg1, dtype=dtype)))
     70 
     71         # Read matrix dimensions given, if any

```

{:.output_traceback_line}
```
/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages/scipy/sparse/coo.pyc in __init__(self, arg1, shape, dtype, copy)
    197                     self.shape = M.shape
    198 
--> 199                 self.row, self.col = M.nonzero()
    200                 self.data = M[self.row, self.col]
    201                 self.has_canonical_format = True

```

{:.output_traceback_line}
```
KeyboardInterrupt: 
```




{:.input_area}
```python
all_dums = sp.hstack((o_dums, d_dums))
all_dums
```





{:.output_data_text}
```
<9000000x6000 sparse matrix of type '<type 'numpy.float64'>'
	with 18000000 stored elements in Compressed Sparse Column format>
```





{:.input_area}
```python
print spcategorical(np.array(data)).toarray().shape
```


{:.output_stream}
```
(10000, 99)

```
