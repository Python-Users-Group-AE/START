## NumPy 

```python
import numpy as np 
xp = [1, 2, 3] 
fp = [3, 2, 0] 
print(np.interp(2.5, xp, fp)) 
```

## SciPy 

```python
from scipy.interpolate import interp1d 
X = [1,2,3,4,5] # random x values 
Y = [11,2.2,3.5,-88,1] # random y values 

# test value 
interpolate_x = 2.5 

# Finding the interpolation 
y_interp = interp1d(X, Y) 
print("Value of Y at x = {} is".format(interpolate_x), y_interp(interpolate_x)) 
```