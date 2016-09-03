函数
=========================
##  函数简介
函数其实就是功能的抽象,用于封装代码.python的[内置函数列表](https://docs.python.org/3/library/functions.html):

## 定义函数
```py
#定义基本函数
def mymax(a,b):
    if not isinstance(a, (int, float)) or not isinstance(b, (int, float)):
        raise TypeError('bad operand type')
    return a>b and a or b
    pass
print(mymax(2,1))
```

## 返回多个值
```py
#返回多个值,其实只是多个值打包在一起返回,就是一个tuple
def returnmulti(a,b):
    return a,b

c,d=returnmulti(1,2)
print(c,d)
```

##  函数参数

### 默认参数

#### 默认参数在使用是无需传入,减少调用的复杂性.
```py
def power(x, n=2):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x
    return s
#power(5),相当于调用了power(5,2)
power(5)
```

#### 保持默认参数为不变对象,或者是在内部作判断
默认参数在内存中也是一个对象,所以如果是设置为可变对象的话,会出现参数一直累加的情况
```py
def de_func(a=[]):
    a.append("test")
    return a

print(de_func())
print(de_func())

```

### 可变参数
可变参数是在参数个数不确定个数下使用,用`*`号代表可变参数,接收的是一个`tuple`
```py
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
print(calc(1,2))
print(calc(1,2,3))
```

### 关键字参数
自动将参数封装成`dict`
```py
def dict_params(**kw):
    print(kw)
dict_params(a=1)
#output: {'a': 1}
```

#### 命名关键字参数
```py
#限定参数用*号分割
def person( *, city, job):
    print( city, job)
#只能传city,job两个参数
person(city="guangzhou",job="gamedevelop")

#如果有可变参数,*号可省略
def person1( *info, city, job):
    print(info,city, job)

person1(1,2,city="guangzhou",job="gamedevelop")
```

## 参数顺序
多种参数类型可以混用,但是参数定义的顺序必须是：必选参数、默认参数、可变参数、命名关键字参数和关键字参数
```py
def f1(a, b, c=0, *args, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw)

def f2(a, b, c=0, *, d, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'd =', d, 'kw =', kw)
```

## 递归函数
函数调用其本身就是递归
```py
#计算阶乘
def fact(n):
    if n==1:
        return 1
    return n * fact(n - 1)

print(fact(5))

#计算过程
'''
===> fact(5)
===> 5 * fact(4)
===> 5 * (4 * fact(3))
===> 5 * (4 * (3 * fact(2)))
===> 5 * (4 * (3 * (2 * fact(1))))
===> 5 * (4 * (3 * (2 * 1)))
===> 5 * (4 * (3 * 2))
===> 5 * (4 * 6)
===> 5 * 24
===> 120
'''
```

## 匿名函数`lambda`
语法:冒号前是参数，可以有多个，用逗号隔开，冒号右边的返回值
```py
#普通函数
def f(x):
    return x*2
f(2)
#lambda
f=lambda x:x*2
f(2)
```