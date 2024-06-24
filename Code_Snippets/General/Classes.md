Classes & Class Methods 
 
```python
class MyClass: 
    def __init__(self, A): 
        self.A = A 

    def f(self,B): 
        return self.A * B 

x = MyClass(2) 
x.B = 4 
print(x.f(5)) 
```