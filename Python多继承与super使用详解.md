Python多继承与super使用详解
===

### **0.问题的提出**

- 如果**不同的父类**中存在 **同名的方法**，**子类对象**在调用方法时，会调用**哪一个父类中**的方法呢？

**Python 中的 MRO —— 方法搜索顺序**

- `Python` 中针对 **类** 提供了一个**内置属性** `__mro__` 可以查看**方法**搜索顺序
- MRO 是 `method resolution order`，主要用于**在多继承时判断 方法、属性 的调用 路径**

```
print(C.__mro__)  #C是多继承后的类名
```

**输出结果**

```
(<class '__main__.C'>, <class '__main__.A'>, <class '__main__.B'>, <class 'object'>)
```

- 在搜索方法时，是按照 `__mro__` 的输出结果 **从左至右** 的顺序查找的

- 如果在当前类中 **找到方法，就直接执行，不再搜索**

- 如果 **没有找到，就查找下一个类** 中是否有对应的方法，**如果找到，就直接执行，不再搜索**

- 如果找到最后一个类，还没有找到方法，程序报错

  ### 1.多继承的使用

```python
#1.多继承：子类有多个父类
 
class Human:
    def __init__(self, sex):
        self.sex = sex
 
    def p(self):
        print("这是Human的方法")
 
 
class Person:
    def __init__(self, name):
        self.name = name
 
    def p(self):
        print("这是Person的方法")
 
    def person(self):
        print("这是我person特有的方法")
 
 
class Teacher(Person):
    def __init__(self, name, age):
        super().__init__(name)
        self.age = age
 
 
class Student(Human, Person):
    def __init__(self, name, sex, grade):
    #要想调用特定父类的构造器可以使用父类名.__init__方式。多继承使用super，会有一个坑，具体参考后面
       Human.__init__(self,sex)
       Person.__init__(self,name)
       self.grade = grade
 
 
class Son(Human, Teacher):
    def __init__(self, sex, name, age, fan):
        Human.__init__(self, sex)
        Teacher.__init__(self, name, age)
        self.fan = fan
 
 
# ------创建对象 -------------
stu = Student("tom", "male", 88)
print(stu.name,stu.sex,stu.grade)
stu.p()  # 虽然父类Human和Person都有同名P()方法 ，但是调用的是括号里的第一个父类Human的方法
 
 
son1 = Son("jerry", "female", 18, "打球")
son1.person()  # 可以调用父类的父类的方法。
son1.p()  # 子类调用众多父类中同名的方法，按继承的顺序查找。
=====================================================================================
tom male 88
这是Human的方法
这是我person特有的方法
这是Human的方法
```

**总结：**

**1.需要注意圆括号中继承父类的顺序，若是父类中有相同的方法名，而在子类使用时未指定，python从左至右搜索 即方法在子类中未找到时，从左到右查找父类中是否包含方法。** **2.支持多层父类继承，子类会继承父类所有的属性和方法，包括父类的父类的所有属性 和 方法。**

### **2.多继承的使用注意事项**   

```python
#1.多继承子类对父类构造方法的调用
class Human:
    def __init__(self,sex):
        self.sex = sex
    def p(self):
        print("这是Human的方法")
    def str1(self):
        print("this si"+str(self.sex))
 
class Person:
    def __init__(self,name):
        self.name = name
    def p(self):
        print("这是Person的方法")
    def person(self):
        print("这是我person特有的方法")
 
    def str2(self):
        print( "this is:" + str(self.name))
 
class Student(Human,Person): #注意子类如果没有构造方法时，按括号内父类的继承顺序去继承父类构造方法，只继承一个
    def prin(self):
        print("student")
#------创建对象 -------------
#stu1=Studnent("男","tom")报错。
stu = Student("sex") #这里继承的是Huma的构造方法。
stu.p()
stu.str1() 
#stu.str2()报错，因为即使human和person都是一个参数的构造方法，但是这里继承调用的是第一个Human的构造方法
====================================================================================
这是Human的方法
this sisex
```

**总结：子类从多个父类派生，而子类又没有自己的构造函数时，**

**（1）按顺序继承，哪个父类在最前面且它又有自己的构造函数，就继承它的构造函数；**

**（2）如果最前面第一个父类没有构造函数，则继承第2个的构造函数，第2个没有的话，再往后找，以此类推。**

### **3.多继承时使用super调用父类属性方法的注意事项**

**3.1不使用super调用父类方法，使用父类名.方法名的形式。**

```python
class Parent(object):
    def __init__(self, name):
        print('parent的init开始被调用')
        self.name = name
        print('parent的init结束被调用')
 
class Son1(Parent):
    def __init__(self, name, age):
        print('Son1的init开始被调用')
        self.age = age
        Parent.__init__(self, name) #直接使用父类名.方法名的方式调用父类的__init__方法
        print('Son1的init结束被调用')
 
class Son2(Parent):
    def __init__(self, name, gender):
        print('Son2的init开始被调用')
        self.gender = gender
        Parent.__init__(self, name) #
        print('Son2的init结束被调用')
 
class Grandson(Son1, Son2):
    def __init__(self, name, age, gender):
        print('Grandson的init开始被调用')
        Son1.__init__(self, name, age)  # 单独调用父类的初始化方法
        Son2.__init__(self, name, gender)
        print('Grandson的init结束被调用')
 
gs = Grandson('grandson', 12, '男') 
print('姓名：', gs.name)
print('年龄：', gs.age)
print('性别：', gs.gender)
 
'''执行结果如下：
Grandson的init开始被调用
Son1的init开始被调用
parent的init开始被调用
parent的init结束被调用
Son1的init结束被调用
Son2的init开始被调用
parent的init开始被调用
parent的init结束被调用
Son2的init结束被调用
Grandson的init结束被调用
姓名： grandson
年龄： 12
性别： 男
'''
```

**注意：上面代码里当在子类中通过父类名调用时，parent被执行了2次**

**3.2 使用super调用父类中的方法，注意分析程序的执行顺序。**

```python
class Parent(object):
    def __init__(self, name, *args, **kwargs):  # 为避免多继承报错，使用不定长参数，接受参数
        print('parent的init开始被调用')
        self.name = name
        print('parent的init结束被调用')
 
class Son1(Parent):
    def __init__(self, name, age, *args, **kwargs):  # 为避免多继承报错，使用不定长参数，接受参数
        print('Son1的init开始被调用')
        self.age = age
        super().__init__(name, *args, **kwargs)  # 为避免多继承报错，使用不定长参数，接受参数
        print('Son1的init结束被调用')
 
class Son2(Parent):
    def __init__(self, name, gender, *args, **kwargs):  # 为避免多继承报错，使用不定长参数，接受参数
        print('Son2的init开始被调用')
        self.gender = gender
        super().__init__(name, *args, **kwargs)  # 为避免多继承报错，使用不定长参数，接受参数
        print('Son2的init结束被调用')
 
class Grandson(Son1, Son2):
    def __init__(self, name, age, gender):
        print('Grandson的init开始被调用')
        # 多继承时，相对于使用类名.__init__方法，要把每个父类全部写一遍
        # 而super只用一句话，执行了全部父类的方法，这也是为何多继承需要全部传参的一个原因
        # super(Grandson, self).__init__(name, age, gender) 效果和下面的一样
        super().__init__(name, age, gender)
        print('Grandson的init结束被调用')
 
print(Grandson.__mro__) #搜索顺序
 
gs = Grandson('grandson', 12, '男')
 
print('姓名：', gs.name)
print('年龄：', gs.age)
print('性别：', gs.gender)
 
'''结果如下：
(<class '__main__.Grandson'>, <class '__main__.Son1'>, <class '__main__.Son2'>, <class '__main__.Parent'>, <class 'object'>)
Grandson的init开始被调用
Son1的init开始被调用
Son2的init开始被调用
parent的init开始被调用
parent的init结束被调用
Son2的init结束被调用
Son1的init结束被调用
Grandson的init结束被调用
姓名： grandson
年龄： 12
性别： 男
'''
```

 **注意：在上面模块中，当在子类中通过super调用父类方法时，parent被执行了1次。**

**super调用过程：上面gs初始化时，先执行grandson中init方法, 其中的init有super调用，每执行到一次super时，都会从__mro__方法元组中顺序查找搜索。所以先调用son1的init方法，在son1中又有super调用，这个时候就就根据__mro__表去调用son2的init，然后在son2中又有super调用，这个就根据mro表又去调用parent中的init，直到调用object中的init. 所以上面的打印结果如此，要仔细分析执行过程。**

### ***\*尖叫提示：\****

1.  **super().__init__相对于类名.__init__，在单继承上用法基本无差**
2. **但在多继承上有区别，super方法能保证每个父类的方法只会执行一次，而使用类名的方法会导致方法被执行多次，具体看前面的输出结果**
3. **多继承时，使用super方法，对父类的传参数，应该是由于python中super的算法导致的原因，必须把参数全部传递，否则会报错**
4. **单继承时，使用super方法，则不能全部传递，只能传父类方法所需的参数，否则会报错**
5. **多继承时，相对于使用类名.__init__方法，要把每个父类全部写一遍, 而使用super方法，只需写一句话便执行了全部父类的方法，这也是为何多继承需要全部传参的一个原因**

**3.3单继承使用super调用父类方法**

```python
class Parent(object):
    def __init__(self, name):
        print('parent的init开始被调用')
        self.name = name
        print('parent的init结束被调用')
 
class Son1(Parent):
    def __init__(self, name, age):
        print('Son1的init开始被调用')
        self.age = age
        super().__init__(name)  # 单继承不能提供全部参数
        print('Son1的init结束被调用')
 
class Grandson(Son1):
    def __init__(self, name, age, gender):
        print('Grandson的init开始被调用')
        super().__init__(name, age)  # 单继承不能提供全部参数
        print('Grandson的init结束被调用')
 
gs = Grandson('grandson', 12, '男')
print('姓名：', gs.name)
print('年龄：', gs.age)
```

