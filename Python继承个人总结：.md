#### Python继承个人总结：



- 单继承，适合用super()

  1. 不写self
  2. 参数不写全，参数和父类相同

- 单继承实例

  ```python
  class Human(object):
  
      def __init__(self, name, age):
          self.name = name
          self.age = age
      
      def speak(self):
          print("My name is %s, I am %d yeas old" % (self.name, self.age))
  
  
  class Girls(Human):
  
      def __init__(self, gender, name, age):
          self.gender = gender
          super().__init__(name, age)
      
      def speak(self):
          print("My name is %s, I am %d yeas old" % (self.name, self.age))
          if self.gender == "female":
              print("I am a girl")
          elif self.gender == "male":
              print("I am a boy")
          else:
              print("Gender is wrong")
  ```

  

- 多继承（继承两个没有关联的类），用类名代码更少

  1. 写self
  2. 参数不写全，参数和父类相同

  ```python
  class Human(object):
  
      def __init__(self, name, age):
          self.name = name
          self.age = age
      
      def speak(self):
          print("My name is %s, I am %d yeas old" % (self.name, self.age))
          
  
  class Tool(object):
  
      def __init__(self,tname):
          self.tname = tname 
      
      def tshow(self):
          print("I am a %s" % (self.tname))
  
  
  class Boy(Human, Tool):
      
      def __init__(self, gender, name, age, tname):
          self.gender = gender
          Human.__init__(self, name, age)
          Tool.__init__(self, tname)
      
      def b_show(self):
          print("My name is %s, I am %d yeas old" % (self.name, self.age))
          if self.gender == "female":
              print("I am a girl")
          elif self.gender == "male":
              print("I am a boy")
          else:
              print("Gender is wrong")
          print("I have a tool ,its name is %s" % self.tname)      
  print("*"*50)
  h = Boy("male", "han", 19, "axe")
  h.speak()
  print("*"*50)
  h.tshow()
  print("*"*50)
  h.b_show()
  ```

  

- 多继承（继承两个没有关联的类），也可以用super()，适用继承多个类
  1. 不定长参数带来的问题，所有init函数中都加(*args, **kwargs), 防止传递参数时出错
  2. 不定长参数带来的问题，所有super函数中都加(*args, **kwargs), 防止传递参数时出错
  3. 父类中都必须有super函数
  4. 子类中不写self
  5. 子类中参数写全继承自父类的参数

```python
class Human(object):

    def __init__(self, name, age, *args, **kwargs):
        self.name = name
        self.age = age
        super().__init__(*args, **kwargs)

    def speak(self):
        print("My name is %s, I am %d yeas old" % (self.name, self.age))



class Tool(object):

    def __init__(self, tname, *args, **kwargs):
        self.tname = tname
        super().__init__(*args, **kwargs)

    def tshow(self):
        print("I am a %s" % (self.tname))


class Boy(Human, Tool):

    def __init__(self, gender, name, age, tname, *args, **kwargs):
        self.gender = gender
        # Human.__init__(self, name, age)
        # Tool.__init__(self, tname)
        super().__init__(name, age, tname, *args, **kwargs)

    def b_show(self):
        print("My name is %s, I am %d yeas old" % (self.name, self.age))
        if self.gender == "female":
            print("I am a girl")
        elif self.gender == "male":
            print("I am a boy")
        else:
            print("Gender is wrong")
        print("I have a tool ,its name is %s" % self.tname)


print("*"*50)
h = Boy("male", "han", 19, "axe")
h.speak()
print("*"*50)
h.tshow()
print("*"*50)
h.b_show()
print("*"*50)
print(Boy.__mro__)
```

