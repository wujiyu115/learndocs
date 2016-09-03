其他特性
================================

## 切片(Slice)
切片大大简化了取元素的操作
```py
#记住切片包含头不包含尾
L = [1,2,3,4]
#第1,2个
print(L[0:2])
#复制原来的list
print(L[:])
```

## 列表生成式
```py
#列表生成器-->[1, 2, 3, 4]
print(list(range(1,5)))
# 生成1*1,2*2... -->[1, 4, 9, 16]
print([x*x for x in range(1,5)])
#组合生成-->['ac', 'ad', 'bc', 'bd']
print([m+n for m in 'ab' for n in "cd"])
#调用函数
print([a.upper() for a in ["a","b"]])
```

## 生成器(generator)
列表生成式是直接生成列表,所以在生成大列表会消耗大量的内存.

### `rang`生成
解决办法只是在生成生成器时把`[]`改成了`()`
pyton2的关键字是用`xrang`
python3的关键字还是用`rang`
```py
#python2
g = (x * x for x in xrange(10))
#python3
g = (x * x for x in range(10))
<generator object <genexpr> at 0x00557620>
print(g)
for x in g:
    print(x)
    pass

```

### `yield`生成器

### 基本用法
```py
def ye():
    yield 1
    yield 2

g= ye()
print(g)
for x in g:
    print(x)
'''
output is :
<generator object ye at 0x00473E18>
1
2
'''
```

#### yield中return的作用
`yield` 中无法`return`具体值连`None`值都不行,`yield`中`reutrn`是用来中止生成的
```py
def ye2():
    yield 1
    return None
    yield 2
g2 = ye2()
for x in g2:
        print(x)
'''
output is :
1
'''
```


+ `generator` 是用来产生一系列值的
+ `yield` 则像是`generator`函数的返回结果
+ `yield` 唯一所做的另一件事就是保存一个`generator`函数的状态
+ `generator`就是一个特殊类型的迭代器（`iterator`）
+ 和迭代器相似，我们可以通过使用`next()`来从`generator`中获取下一个值
+ 通过隐式地调用`next()`来忽略一些值

### 迭代器
判断一个对象是否是`Iterable`对象
```py
from collections import Iterable,Iterator

print(isinstance([],Iterable))
print(isinstance([],Iterator))
print(isinstance((x for x in range(10)), Iterable))
print(isinstance((x for x in range(10)), Iterator))
'''
output is :
True
False
True
True
'''
```

+ 凡是可作用于`for`循环的对象都是`Iterable`类型；
+ 凡是可作用于`next()`函数的对象都是`Iterator`类型，它们表示一个惰性计算的序列；
+ 集合数据类型如`list`、`dict`、`str`等是`Iterable`但不是`Iterator`，不过可以通过`iter()`函数获得一个`Iterator`对象

其实`for`也是通过`Iterator`来完成的
```py
for x in [1, 2, 3, 4, 5]:
    print(x)
    pass
#等价于
it = iter([1, 2, 3, 4, 5])
while True:
    try:
        x = next(it)
        print(x)
    except StopIteration:
        break
```

## map
`map()`函数接收两个参数，一个是函数，一个是`Iterable`，`map`将传入的函数依次作用到序列的每个元素，并把结果作为新的`Iterator`返回
```py
def f(x):
    return x*x
#r是一个Iterator
r = map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])
print(list(r))
```

## reduce
`reduce`把一个函数作用在一个序列`[x1, x2, x3, ...]`上，这个函数必须接收两个参数，`reduce`把结果继续和序列的下一个元素做累积计算，其效果就是：
```py
reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)
```

```py
from functools import reduce
def add(x, y):
    return x + y
print(reduce(add, [1, 3, 5, 7, 9]))
```

## filter
`filter()`函数可以用来筛选一个队列

`filter()`也接收一个函数和一个序列.和`map()`不同的是，`filter()`把传入的函数依次作用于每个元素,然后根据返回值是`True`还是`False`决定保留还是丢弃该元素.
选择奇数:
```py
def odd(n):
    return n%2==1

odd_arr = filter(odd,[1,2,3,4,5])
print(odd_arr)
'''
[1,3,5]
'''
```

## sorted
`sorted()`函数用于排序,原型:`sort([cmp[, key[, reverse]]])`

### 数字
```py
#数字
#普通小到大
print(sorted([1,5,4]))
#普通大到小
print(sorted([1,5,4],reverse = True))
#按自定义规则,绝对值
print(sorted([1,-5,4],key=abs))
'''
[1, 4, 5]
[5, 4, 1]
[1, 4, -5]
'''
```

### 字符串
默认情况下，对字符串排序，是按照ASCII的大小比较的 根据ASCII排:0-9(对应数值48-59),A-Z(对应数值65-90),a-z(对应数值97-122)
```py
#字符串
#默认ascii序
print(sorted(['a','C','b']))
print(sorted(['boat','cat','boot','too','to','To']))
#忽略大小写
print(sorted(['a','C','b'],key=str.lower))
#从短到长
print(sorted(['boat','cat','boot','too','to','To'],key=len))
'''
['C', 'a', 'b']
['To', 'boat', 'boot', 'cat', 'to', 'too']
['a', 'b', 'C']
['to', 'To', 'cat', 'too', 'boat', 'boot']
'''
```

## 装饰器
`decorator`就是一个返回函数的高阶函数,在不改变原有函数的结构下运行时添加功能,类似于java中的代理类.

```py
import time

def log(func):
    def wrapper(*args,**kw):
        print("call%s()"% func.__name__)
        return func(*args,**kw)
    return wrapper

@log
def now():
    print(time.time())

now()
'''
call,now()
1464665858.0853982
'''
```

### 带函数的装饰器:
`functools.wraps`:`__name__`等属性复制到`wrapper()`
```py
import functools

def log(text):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
            print('%s %s():' % (text, func.__name__))
            return func(*args, **kw)
        return wrapper
    return decorator

@log('execute')
def now():
    print(time.time())

now()
'''
execute now():
1464666390.31384
'''
```

## Partial function
```py
#16进制转换成10进制
print(int('100', base=16))
print(int('100',16))
#使用functools.partial构造
import functools
int2 = functools.partial(int, base=16)
print(int2('100'))
'''
256
256
256
'''
```