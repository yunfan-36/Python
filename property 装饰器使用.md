property 装饰器使用

```python
class Student(object):
    def __init__(self): # 先定义一个私有属性
        self.__name = "wang"
    def get_name(self): # 获取私有属性
        return self.__name
    def set_name(self,new_name): # 更改私有属性
        self.__name = new_name
    name = property(get_name, set_name)
    
obj = Student() # 实例化对象obj
print(obj.name) # 打印obj.name  "wang"
obj.name = "li" # 设置obj.name "li"
print(obj.name) # 打印obj.name  "li"
```

