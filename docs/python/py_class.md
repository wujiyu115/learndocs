python 类,继承
==========================

## 类

python 是完全面向对象的语言，你可以定义自己的类，也可以继承.所有数据类型都为对象，以引用赋值

### 类的定义 :

```py
  class DataType:
      pass
```
class作为类声明，pass表示缩进，没有其他含义

###` __init__`方法:
从形式上来看相当于java中的构造器，在类实例后调用,但是在执行INIT方法时，对象已经实例化好了

```py
  class DataType:
      def __init__(self):
          print(self)

  flib = DataType()
```
`self` 相当于java中的this,指向该实例,实例化类flib =DataType() 并不像java中一样使用显式的new关键字

### 实例变量
用`self.`变量名来声明实例变量.
```py
  class Flib:
      def __init__(self, max):
          self.max = max
      def iter(self):
          self.a = 0
          self.b = 1
          return self
      def next(self):
          fib = self.a
          if fib > self.max:
              raise StopIteration
          self.a, self.b = self.b, self.a + self.b
          return fib

```

### 动态属性与限定
`动态属性`:实例可以动态添加属性,不影响其他实例
`__slots__`:限制实例的属性,子类实例允许定义的属性就是自身的`__slots__`加上父类的`__slots__`
```py
class Student(object):
  __slots__ = ('name',)
  """docstring for Student"""
  def __init__(self):
    super(Student, self).__init__()

st = Student()
st.name = "far"
st.age =10 #error
print(st.name)

class StudentA(Student):
  __slots__ = ('name','age')
  pass

sta= StudentA()
st.name = "far1"
sta.age = 1
sta.sex = 1 #error
print(sta.age)


'''
AttributeError: 'Student' object has no attribute 'age''''
```

### @property
```py
class Student(object):

    #读
  @property
  def score(self):
    return self._score

  #写
  @score.setter
  def score(self, value):
    if not isinstance(value, int):
      raise ValueError('score must be an integer!')
    if value < 0 or value > 100:
      raise ValueError('score must between 0 ~ 100!')
    self._score = value

st = Student()
st.score = -1

'''
ValueError: score must between 0 ~ 100!
'''
```

### 访问限制
私有属性:属性的名称前加上两个下划线`__`
```py
class Student(object):

    def __init__(self, name, score):
        self.__name = name
        self.__score = score

    def print_score(self):
        print('%s: %s' % (self.__name, self.__score))

 bart = Student('Bart Simpson', 98)
 bart.__name
 '''
 AttributeError: 'Student' object has no attribute '__name'
 '''
```

###  基础方法
+ `isinstance`:判断对象类型
```py
print(isinstance(bart,Student))
print(isinstance(bart,object))
'''
True
True
'''
```
+ `type`:获取对象类型,创建新的类型
```py
def de_fun (self,name="far"):
  print("Hi,%s"%name)

#type("类名",("继承类",),dict(属性名=属性值))
Hi = type("Hi",(object,),dict(sayhi=de_fun))
hi = Hi()
hi.sayhi()
'''
Hi,far
<type 'type'>
<class '__main__.Hi'>
'''
```
+ `dir`:获得一个对象的所有属性和方法
+ `hasattr`,`getattr`:动态获取对象方法
```py
hasattr(obj, 'power') # 有属性'power'吗？
getattr(obj, 'power') # 获取属性'power'
```

### 经典类
定义:没有继承的类,注意：如果经典类被作为父类，子类调用父类的构造函数时会出错。`TypeError: must be type, not classobj`
```py
#基类（经典类）
class Person:
    def __init__(self):
        print "Hi, I am a person. "

#子类
class Student(Person):
    def __init__(self):
        super(self.__class__, self).__init__()

if __name__ == "__main__":
    student = Student()
    #出错啦！TypeError: must be type, not classobj
```


### 新式类
定义:每个类都继承于一个基类，可以是自定义类或者其它类，如果什么都不想继承，那就继承于object
如果想用super调用父类的构造函数，请使用新式类！
```py
#基类（新式类）
class Person(object):
    def __init__(self):
        print "Hi, I am a person."

#子类
class Student(Person):
    def __init__(self):
        super(self.__class__, self).__init__()

if __name__ == "__main__":
    student = Student()
```


### 定制类

+ `__str__`:格式化输出
```py
class Student(object):
  def __init__(self, name):
    self.name = name
  def __str__(self):
    return 'Student object (name: %s)' % self.name

st = Student("name")
print(st)
```
+ `__iter__`:格式化输出
如果一个类想被用于`for ... in`循环，类似list或tuple那样，就必须实现一个`__iter__()`方法，该方法返回一个迭代对象，然后，Python的for循环就会不断调用该迭代对象的`__next__()`方法拿到循环的下一个值，直到遇到`StopIteration`错误时退出循环
```py
class Fib(object):
    def __init__(self):
        self.a, self.b = 0, 1 # 初始化两个计数器a，b

    def __iter__(self):
        return self # 实例本身就是迭代对象，故返回自己

    def __next__(self):
        self.a, self.b = self.b, self.a + self.b # 计算下一个值
        if self.a > 100: # 退出循环的条件
            raise StopIteration();
        return self.a # 返回下一个值

for n in Fib():
  print(n)
```
+ `__getitem__`:下标方式访问对象
```py

class GetItem(object):
  """docstring for GetItem"""
  def __init__(self):
    super(GetItem, self).__init__()

  def __getitem__(self,n):
    if isinstance(n, int): # n是索引
      return n
    if isinstance(n, slice): # n是切片
      start = n.start
      stop = n.stop
      if start is None:
        start = 0
      L = []
      for x in range(stop):
        if x >= start:
          L.append(x)
      return L

getitem = GetItem()
print(getitem[2])
print(getitem[0:2])
'''
2
[0, 1]
'''
```
+ `__getattr__`:获取属性的操作
```py
class Student(object):

  def __init__(self):
    self.name = 'Michael'

  def __getattr__(self, attr):
    if attr=='score':
      return 99
s = Student()
print(s.score)
'''
99
'''
```
+ `__call__`:用于实例自身的调用
```py
class Student(object):
    def __init__(self, name):
        self.name = name

    def __call__(self):
        print('My name is %s.' % self.name)

st = Student('Michael')
st()
print(callable(st))
print(callable(max))
print(callable(1))
'''
My name is Michael.
True
True
False
相当于重载了括号运算符,可以用callable来判断是否是一个可调用的类型
'''
```

## 继承
继承语法: 类(父类) Python不会自动调用基本类的constructor，你得亲自专门调用它
可以多重继承
```py
  class SchoolMember:
      '''Represents any school member.'''
      def __init__(self, name, age):
          self.name = name
          self.age = age
          print'(Initialized SchoolMember: %s)'% self.name
       def tell(self):
          '''Tell my details.'''
          print'Name:"%s" Age:"%s"'% (self.name, self.age),

  class Teacher(SchoolMember):
      '''Represents a teacher.'''
      def __init__(self, name, age, salary):
          SchoolMember.__init__(self, name, age)
          self.salary = salary
          print'(Initialized Teacher: %s)'% self.name

      def tell(self):
          SchoolMember.tell(self)
          print'Salary: "%d"'% self.salary

  class Student(SchoolMember):
      '''Represents a student.'''
      def __init__(self, name, age, marks):
          SchoolMember.__init__(self, name, age)
          self.marks = marks
          print'(Initialized Student: %s)'% self.name

      def tell(self):
          SchoolMember.tell(self)
          print'Marks: "%d"'% self.marks

  t = Teacher('Mrs. Shrividya',40,30000)
  s = Student('Swaroop',22,75)
  members = [t, s]
  for member in members:
      member.tell()# works for both Teachers and Students
```


### 多重继承
```py
class Mammal(object):
  def viviparity(self):
    print("viviparity!!")

class CarnivorousMixIn(object):
  def eatmeat(self):
    print("eat meating!!")

class Dog(Mammal, CarnivorousMixIn):
  pass

dog = Dog()
dog.viviparity()
dog.eatmeat()
'''
viviparity!!
eat meating!!
'''
```

