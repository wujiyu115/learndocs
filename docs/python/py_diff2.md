Python 2.7.x 和 Python 3.x 的主要区别
=======================

许多 Python 初学者想知道他们应该从 Python 的哪个版本开始学习。对于这个问题我的答案是 “你学习你喜欢的教程的版本，然后检查他们之间的不同。"

但是如果你开始一个新项目，并且有选择权？我想说的是目前没有对错，只要你计划使用的库 Python 2.7.x 和 Python 3.x 双方都支持的话。尽管如此，当在编写它们中的任何一个的代码，或者是你计划移植你的项目的时候，是非常值得看看这两个主要流行的 Python 版本之间的差别的，以便避免常见的陷阱。

## 使用 __future__ 模块
用途:在 Python 2 里导入那些在 Python 3 里才生效的模块和函数:
```py
from __future__ import division
```

## print 函数
很琐碎，而 print 语法的变化可能是最广为人知的了，但是仍值得一提的是： Python 2 的 print 声明已经被 print() 函数取代了，这意味着我们必须包装我们想打印在小括号中的对象。
Python 2 不具有额外的小括号问题。但对比一下，如果我们按照 Python 2 的方式不使用小括号调用 print 函数，Python 3 将抛出一个语法异常（SyntaxError）。

### Python 2
```py
print 'Python', python_version()
print 'Hello, World!'
print('Hello, World!')
print "text", ; print 'print more text on the same line'
```
### Python 3
```py
print('Python', python_version())
print('Hello, World!')

print("some text,", end="") 
print(' print more text on the same line')
```

##  整除 Integer division
如果你正在移植代码，这个变化是特别危险的。或者你在 Python 2 上执行 Python 3 的代码。因为这个整除的变化表现在它会被忽视（即它不会抛出语法异常）。

因此，我还是倾向于使用一个 float(3)/2 或 3/2.0 代替在我的 Python 3 脚本保存在 Python 2 中的 3/2 的一些麻烦（并且反而过来也一样，我建议在你的 Python 2 脚本中使用 from __future__ import division）

### Python 2
```py
print 'Python', python_version()
print '3 / 2 =', 3 / 2
print '3 // 2 =', 3 // 2
print '3 / 2.0 =', 3 / 2.0
print '3 // 2.0 =', 3 // 2.0

'''
Python 2.7.6
3 / 2 = 1
3 // 2 = 1
3 / 2.0 = 1.5
3 // 2.0 = 1.0
'''
```

### Python 3
```py
print('Python', python_version())
print('3 / 2 =', 3 / 2)
print('3 // 2 =', 3 // 2)
print('3 / 2.0 =', 3 / 2.0)
print('3 // 2.0 =', 3 // 2.0)
'''
Python 3.4.1
3 / 2 = 1.5
3 // 2 = 1
3 / 2.0 = 1.5
3 // 2.0 = 1.0
'''
```

## Unicode
Python 2 有 `ASCII str()` 类型，`unicode()` 是单独的，不是` byte` 类型。
现在， 在 Python 3，我们最终有了` Unicode (utf-8)` 字符串，以及一个字节类：`byte` 和` bytearrays`。
###  Python 2
```py
print type(unicode('this is like a python3 str type'))
'''output is :<type 'unicode'>'''

print type(b'byte type does not exist')
'''output is : <type 'str'>'''

print 'they are really' + b' the same'
'''output is :they are really the same'''

print type(bytearray(b'bytearray oddly does exist though'))
'''output is :<type 'bytearray'>'''
```

### Python 3
```py
print('Python', python_version(), end="")
print(' has', type(b' bytes for storing data'))
'''Python 3.4.1 has <class 'bytes'>'''

print('and Python', python_version(), end="")
print(' also has', type(bytearray(b'bytearrays')))
'''and Python 3.4.1 also has <class 'bytearray'>'''

```


## xrange
Python 2 里的range() 和 xrange() 还是有点区别的. xrange() 更像一个生成器.range()生成的是list.
Python 3 里取消了xrange() 代之以range() 实际上 Python 3 把很多返回为List的改成了itrable.


## Raising exceptions

### python2
```py
raise IOError, "file error"
```
### python3
```py
raise (IOError, "file error")
```

## Handling exceptions

### python2
```py
try:
    let_us_cause_a_NameError
except NameError, err:
    print err, '--> our error message'
```
### python3
```py
try:
    let_us_cause_a_NameError
except NameError as err:
    print(err, '--> our error message')
```

## next() 函数 和 .next() 方法
`.next()`在python中被移除,两个版本都可以用`next()`

### python2
```py
my_generator = (letter for letter in 'abcdefg')

next(my_generator)
my_generator.next()
```
### python3
```py
my_generator = (letter for letter in 'abcdefg')

next(my_generator)
```

## For 循环变量和全局命名空间泄漏
在 Python 3.x 中 for 循环变量不会再导致命名空间泄漏
### python2
```py
i = 1
print 'before: i =', i

print 'comprehension: ', [i for i in range(5)]

print 'after: i =', i

'''
before: i = 1
comprehension:  [0, 1, 2, 3, 4]
after: i = 4
'''
```

### python3
```py
i = 1
print('before: i =', i)

print('comprehension:', [i for i in range(5)])

print('after: i =', i)

'''
before: i = 1
comprehension: [0, 1, 2, 3, 4]
after: i = 1
'''
```

## 比较不可排序类型
在 Python 3 中的另外一个变化就是当对不可排序类型做比较的时候，会抛出一个类型错误。
### python2
```py
print "(1, 2) > 'foo' = ", (1, 2) > 'foo'
'''
(1, 2) > 'foo' =  True
'''
```
### python3
```py
print("[1, 2] > 'foo' = ", [1, 2] > 'foo')
'''
TypeError
'''
```

## 通过 `input()`解析用户的输入
python 2里 我们有`input()`和`raw_input()`,区别是`raw_input()`会讲输入返回str.而i`nput()`会试图执行输入.
Python 3 里的`input()`返回字符串,不再有`raw_input()`,如果想执行输入,可以`eval(input())`

## 返回可迭代对象，而不是列表
### Python 2
```py
print 'Python', python_version() 

print range(3) 
print type(range(3))

'''
Python 2.7.6
[0, 1, 2]
<type 'list'>
'''
```
### Python 3
```py
print('Python', python_version())

print(range(3))
print(type(range(3)))
print(list(range(3)))

'''
Python 3.4.1
range(0, 3)
<class 'range'>
[0, 1, 2]
'''
```
在 Python 3 中一些经常使用到的不再返回列表的函数和方法：
+ zip()
+ map()
+ filter()
+ dictionary's .keys() method
+ dictionary's .values() method
+ dictionary's .items() method

## Link
+ [What’s New In Python 3.0?](https://docs.python.org/3/whatsnew/3.0.html)
