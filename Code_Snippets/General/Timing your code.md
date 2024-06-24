# Timing your code

```python
import time 

_start = time.perf_counter() 

#Code to run 

print(f"Total time taken: {time.perf_counter() - _start:.3f}s") 
```

Example: 

![](../../assets/Code_Snippets/General/Timing%20your%20code.png)

Alternatively, you can use the following decorator before your functions to time them

```python
import time
from functools import wraps

def time_it(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start_time = time.perf_counter()
        result = func(*args, **kwargs)
        print(f'"{func.__name__}()" took {time.perf_counter() - start_time:.3f} seconds to execute')
        return result
    return wrapper
```

Example:

![](../../assets/Code_Snippets/General/Timing%20your%20code2.png)