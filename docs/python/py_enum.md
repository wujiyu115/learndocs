python 枚举,元类
==========================

## 枚举
将我们要定义常量组时常规的做法是定义一系列的变量,虽然简单但这样不利于管理,如:
`blue,red,green=1,2,3`,更好的做法是定义一个枚举:Python提供了`Enum`类来实现这个功能.
```py
from enum import Enum
#Enum("枚举名字",("枚举字段标识''))
Color = Enum("Color",("blue","red","green"))

print(Color.blue.value)
for name, member in Color.__members__.items():
    print(name, '=>', member, ',', member.value)
'''
1
('blue', '=>', <Color.blue: 1>, ',', 1)
('red', '=>', <Color.red: 2>, ',', 2)
('green', '=>', <Color.green: 3>, ',', 3)
'''
```
`value`值是自动附加的属性默认从`1`开始计数,如果要更加精确地控制我们可以继承自`Enum`自定义类:
```py
from enum import Enum, unique

@unique
class Color(Enum):
    blue = 0 # blue的value为0
    red = 1
    green = 2

print(Color.blue.value)
```
`unique`装饰器限定没有重复值

## metaclass
`metaclass`元类用于动态改变类的的创建行为
```py
class ListMetaclass(type):
    def __new__(cls,name,base,attrs):
        attrs["add"] = lambda self,value: self.append(value)
        return type.__new__(cls,name,base,attrs)

class MyList(list, metaclass=ListMetaclass):
    #python2
    # __metaclass__ = ListMetaclass
    """docstring for MyList"""
    def __init__(self):
        super(MyList, self).__init__()

mylist = MyList()
mylist.add(1)
print(mylist)
```