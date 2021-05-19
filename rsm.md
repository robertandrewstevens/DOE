https://pythonhosted.org/pyDOE/rsm.html

# Response Surface Designs

In this section, the following kinds of response surface designs will be described:

- Box-Behnken

- Central Composite

>Hint: All available designs can be accessed after a simple import statement:


```python
# pip install --upgrade pyDOE2
```


```python
# from pyDOE import *
from pyDOE2 import *
import numpy as np
import pandas as pd
```

## Box-Behnken (`bbdesign`)

http://www.itl.nist.gov/div898/handbook/pri/section3/gifs/bb.gif

Box-Behnken designs can be created using the following simple syntax:

```
bbdesign(n, center)
```

where n is the number of factors (at least 3 required) and center is the number of center points to include. If no inputs given to center, then a pre-determined number of points are automatically included.

## Examples

The default 3-factor Box-Behnken design:


```python
bbd3 = bbdesign(3)
bbd3
```




    array([[-1., -1.,  0.],
           [ 1., -1.,  0.],
           [-1.,  1.,  0.],
           [ 1.,  1.,  0.],
           [-1.,  0., -1.],
           [ 1.,  0., -1.],
           [-1.,  0.,  1.],
           [ 1.,  0.,  1.],
           [ 0., -1., -1.],
           [ 0.,  1., -1.],
           [ 0., -1.,  1.],
           [ 0.,  1.,  1.],
           [ 0.,  0.,  0.],
           [ 0.,  0.,  0.],
           [ 0.,  0.,  0.]])




```python
bbd3.shape
```




    (15, 3)




```python
np.corrcoef(bbd3.T)
```




    array([[1., 0., 0.],
           [0., 1., 0.],
           [0., 0., 1.]])



A customized design with four factors, but only a single center point:


```python
bbd4_1 = bbdesign(4, center=1)
bbd4_1
```




    array([[-1., -1.,  0.,  0.],
           [ 1., -1.,  0.,  0.],
           [-1.,  1.,  0.,  0.],
           [ 1.,  1.,  0.,  0.],
           [-1.,  0., -1.,  0.],
           [ 1.,  0., -1.,  0.],
           [-1.,  0.,  1.,  0.],
           [ 1.,  0.,  1.,  0.],
           [-1.,  0.,  0., -1.],
           [ 1.,  0.,  0., -1.],
           [-1.,  0.,  0.,  1.],
           [ 1.,  0.,  0.,  1.],
           [ 0., -1., -1.,  0.],
           [ 0.,  1., -1.,  0.],
           [ 0., -1.,  1.,  0.],
           [ 0.,  1.,  1.,  0.],
           [ 0., -1.,  0., -1.],
           [ 0.,  1.,  0., -1.],
           [ 0., -1.,  0.,  1.],
           [ 0.,  1.,  0.,  1.],
           [ 0.,  0., -1., -1.],
           [ 0.,  0.,  1., -1.],
           [ 0.,  0., -1.,  1.],
           [ 0.,  0.,  1.,  1.],
           [ 0.,  0.,  0.,  0.]])




```python
bbd4_1.shape
```




    (25, 4)




```python
np.corrcoef(bbd4_1.T)
```




    array([[1., 0., 0., 0.],
           [0., 1., 0., 0.],
           [0., 0., 1., 0.],
           [0., 0., 0., 1.]])



## Central Composite (`ccdesign`)

http://www.itl.nist.gov/div898/handbook/pri/section3/gifs/fig5.gif

Central composite designs can be created and customized using the syntax:

```
ccdesign(n, center, alpha, face)
```

where

- `n` is the number of factors,

- `center` is a 2-tuple of center points (one for the factorial block, one for the star block, default (4, 4)),

- `alpha` is either “orthogonal” (or “o”, default) or “rotatable” (or “r”)

- `face` is either “circumscribed” (or “ccc”, default), “inscribed” (or “cci”), or “faced” (or “ccf”).

http://www.itl.nist.gov/div898/handbook/pri/section3/gifs/ccd2.gif

The two optional keyword arguments alpha and face help describe how the variance in the quadratic approximation is distributed. Please see the NIST web pages if you are uncertain which options are suitable for your situation.

Note:

- ‘ccc’ and ‘cci’ can be rotatable designs, but ‘ccf’ cannot

- If face is specified, while alpha is not, then the default value of alpha is ‘orthogonal’

## Examples

Simplest input, assuming default kwargs:


```python
ccd2 = ccdesign(2)
ccd2
```




    array([[-1.        , -1.        ],
           [ 1.        , -1.        ],
           [-1.        ,  1.        ],
           [ 1.        ,  1.        ],
           [ 0.        ,  0.        ],
           [ 0.        ,  0.        ],
           [ 0.        ,  0.        ],
           [ 0.        ,  0.        ],
           [-1.41421356,  0.        ],
           [ 1.41421356,  0.        ],
           [ 0.        , -1.41421356],
           [ 0.        ,  1.41421356],
           [ 0.        ,  0.        ],
           [ 0.        ,  0.        ],
           [ 0.        ,  0.        ],
           [ 0.        ,  0.        ]])




```python
ccd2.shape
```




    (16, 2)




```python
np.corrcoef(ccd2.T)
```




    array([[1., 0.],
           [0., 1.]])



More customized input, say, for a set of computer experiments where there isn’t variability so we only need a single center point:


```python
ccd3 = ccdesign(3, center=(0, 1), alpha='r', face='cci')
ccd3
```




    array([[-0.59460356, -0.59460356, -0.59460356],
           [ 0.59460356, -0.59460356, -0.59460356],
           [-0.59460356,  0.59460356, -0.59460356],
           [ 0.59460356,  0.59460356, -0.59460356],
           [-0.59460356, -0.59460356,  0.59460356],
           [ 0.59460356, -0.59460356,  0.59460356],
           [-0.59460356,  0.59460356,  0.59460356],
           [ 0.59460356,  0.59460356,  0.59460356],
           [-1.        ,  0.        ,  0.        ],
           [ 1.        ,  0.        ,  0.        ],
           [ 0.        , -1.        ,  0.        ],
           [ 0.        ,  1.        ,  0.        ],
           [ 0.        ,  0.        , -1.        ],
           [ 0.        ,  0.        ,  1.        ],
           [ 0.        ,  0.        ,  0.        ]])




```python
ccd3.shape
```




    (15, 3)




```python
np.round(np.corrcoef(ccd3.T), 3)
```




    array([[1., 0., 0.],
           [0., 1., 0.],
           [0., 0., 1.]])



## More Information

If the user needs more information about appropriate designs, please consult the following articles on Wikipedia:

- Box-Behnken designs
    + http://en.wikipedia.org/wiki/Box-Behnken_design

- Central composite designs
    + http://en.wikipedia.org/wiki/Central_composite_design

There is also a wealth of information on the NIST website about the various design matrices that can be created as well as detailed information about designing/setting-up/running experiments in general:

http://www.itl.nist.gov/div898/handbook/pri/pri.htm

Any questions, comments, bug-fixes, etc. can be forwarded to the [author](tisimst@gmail.com).
