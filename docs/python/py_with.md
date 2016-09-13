Python 中的关键字with详解
======================

转载自:https://github.com/zgs225/yuezlog/blob/master/source/_posts/2016-09-10-python-zhong-de-guan-jian-zi-with-xiang-jie.markdown

在 Python 2.5 中，`with`关键字被加入。它将常用的 `try ... except ... finally ...`模式很方便的被复用。看一个最经典的例子：

``` python
with open('file.txt') as f:
    content = f.read()
```

在这段代码中，无论`with`中的代码块在执行的过程中发生任何情况，文件最终都会被关闭。如果代码块在执行的过程中发生了一个异常，那么在这个异常被抛出前，程序会先将被打开的文件关闭。

再看另外一个例子。

在发起一个数据库事务请求的时候，经常会用类似这样的代码：

``` python
db.begin()

try:
    # do some actions
except:
    db.rollback()
    raise
finally:
    db.commit()
```

如果将发起事务请求的操作变成可以支持`with`关键字的，那么用像这样的代码就可以了：

``` python
with transaction(db):
    # do some actions
```

下面，详细的说明一下`with`的执行过程，并用两种常用的方式实现上面的代码。

## with 的一般执行过程

一段基本的`with`表达式，其结构是这样的：

``` python
with EXPR as VAR:
    BLOCK
```

其中：`EXPR`可以是任意表达式；`as VAR`是可选的。其一般的执行过程是这样的：

1. 计算`EXPR`，并获取一个上下文管理器。
2. 上下文管理器的`__exit()__`方法被保存起来用于之后的调用。
3. 调用上下文管理器的`__enter()__`方法。
4. 如果`with`表达式包含`as VAR`，那么`EXPR`的返回值被赋值给`VAR`。
5. 执行`BLOCK`中的表达式。
6. 调用上下文管理器的`__exit()__`方法。如果`BLOCK`的执行过程中发生了一个异常导致程序退出，那么异常的`type`、`value`和`traceback`(即`sys.exc_info()`的返回值)将作为参数传递给`__exit()__`方法。否则，将传递三个`None`。

将这个过程用代码表示，是这样的：

``` python
mgr = (EXPR)
exit = type(mgr).__exit__ # 这里没有执行
value = type(mgr).__enter__(mgr)
exc = True

try:
    try:
        VAR = value # 如果有 as VAR
        BLOCK
    except:
        exc = False
        if not exit(mgr, *sys.exc_info()):
            raise
finally:
    if exc:
        exit(mgr, None, None, None)
```

这个过程有几个细节：

+ 如果上下文管理器中没有`__enter()__`或者`__exit()__`中的任意一个方法，那么解释器会抛出一个`AttributeError`。
+ 在`BLOCK`中发生异常后，如果`__exit()__`方法返回一个可被看成是`True`的值，那么这个异常就不会被抛出，后面的代码会继续执行。

接下来，用两种方法来实现上面来实现上面的过程的吧。

## 实现上下文管理器类

第一种方法是实现一个类，其含有一个实例属性`db`和上下文管理器所需要的方法`__enter()__`和`__exit()__`。

``` python
class transaction(object):
    def __init__(self, db):
        self.db = db
        
    def __enter__(self):
        self.db.begin()
        
    def __exit__(self, type, value, traceback):
        if type is None:
            db.commit()
        else:
            db.rollback()
```

了解`with`的执行过程后，这个实现方式是很容易理解的。下面介绍的实现方式，其原理理解起来要复杂很多。

## 使用生成器装饰器

在Python的标准库中，有一个装饰器可以通过生成器获取上下文管理器。使用生成器装饰器的实现过程如下：

``` python
from contextlib import contextmanager

@contextmanager
def transaction(db):
    db.begin()
    
    try:
        yield db
    except:
        db.rollback()
        raise
    else:
        db.commit()
```

第一眼上看去，这种实现方式更为简单，但是其机制更为复杂。看一下其执行过程吧：

1. Python解释器识别到`yield`关键字后，`def`会创建一个生成器函数替代常规的函数（在类定义之外我喜欢用函数代替方法）。
2. 装饰器`contextmanager`被调用并返回一个帮助函数，这个帮助函数在被调用后会生成一个`GeneratorContextManager`实例。最终`with`表达式中的`EXPR`调用的是由`contentmanager`装饰器返回的帮助函数。
3. `with`表达式调用`transaction(db)`，实际上是调用帮助函数。帮助函数调用生成器函数，生成器函数创建一个生成器。
4. 帮助函数将这个生成器传递给`GeneratorContextManager`，并创建一个`GeneratorContextManager`的实例对象作为上下文管理器。
5. `with`表达式调用实例对象的上下文管理器的`__enter()__`方法。
6. `__enter()__`方法中会调用这个生成器的`next()`方法。这时候，生成器方法会执行到`yield db`处停止，并将`db`作为`next()`的返回值。如果有`as VAR`，那么它将会被赋值给`VAR`。
7. `with`中的`BLOCK`被执行。
8. `BLOCK`执行结束后，调用上下文管理器的`__exit()__`方法。`__exit()__`方法会再次调用生成器的`next()`方法。如果发生`StopIteration`异常，则`pass`。
9. 如果没有发生异常生成器方法将会执行`db.commit()`，否则会执行`db.rollback()`。

再次看看上述过程的代码大致实现：

``` python
def contextmanager(func):
    def helper(*args, **kwargs):
        return GeneratorContextManager(func(*args, **kwargs))
    return helper
    
class GeneratorContextManager(object):
    def __init__(self, gen):
        self.gen = gen
        
    def __enter__(self):
        try:
            return self.gen.next()
        except StopIteration:
            raise RuntimeError("generator didn't yield")
            
    def __exit__(self, type, value, traceback):
        if type is None:
            try:
                self.gen.next()
            except StopIteration:
                pass
            else:
                raise RuntimeError("generator didn't stop")
        else:
            try:
                self.gen.throw(type, value, traceback)
                raise RuntimeError("generator didn't stop after throw()")
            except StopIteration:
                return True
            except:
                if sys.exc_info()[1] is not value:
                    raise
```

## 总结

Python的`with`表达式包含了很多Python特性，花点时间吃透`with`是一件非常值得的事情。

## 一些其他的例子

``` python 锁机制
@contextmanager
def locked(lock):
    lock.acquired()
    try:
        yield
    finally:
        lock.release()
```

``` python 标准输出重定向
@contextmanager
def stdout_redirect(new_stdout):
    old_stdout = sys.stdout
    sys.stdout = new_stdout
    try:
        yield
    finally:
        sys.stdout = old_stdout

with open("file.txt", "w") as f:
    with stdout_redirect(f):
        print "hello world"
```

## 引用

+ [The Python "with" Statement by Example](http://preshing.com/20110920/the-python-with-statement-by-example/)
+ [PEP 343](https://www.python.org/dev/peps/pep-0343/)