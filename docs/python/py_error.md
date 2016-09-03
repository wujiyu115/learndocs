python 异常处理
========================

## 异常捕捉
异常捕捉可以使我们的程序更加健壮
当我们认为某些代码可能会出错时，就可以用`try`来运行这段代码，如果执行出错，则后续代码不会继续执行，而是直接跳转至错误处理代码，即`except`语句块，执行完`except`后，如果有`finally`语句块，则执行`finally`语句块，至此，执行完毕
```py
try:
    print('try...')
    r = 10 / int('a')
    print('result:', r)
except ValueError as e:
    print('ValueError:', e)
except ZeroDivisionError as e:
    print('ZeroDivisionError:', e)
else:
    print('no error!')
finally:
    print('finally...')
print('END')
'''
try...
ValueError: invalid literal for int() with base 10: 'a'
finally...
END
'''
```

## 记录错误
`traceback`:堆栈信息
`logging`:日志记录
```py
import logging,traceback

def foo(s):
    return 10 / int(s)

def bar(s):
    return foo(s) * 2

def main():
    try:
        bar('0')
    except Exception as e:
        print(traceback.print_exc() )
        # logging.exception(e)

main()
print('END')
```

##  抛出错误
自定义`error`类继承Python已有的内置的错误类型（比如ValueError，TypeError）,`raise`语句抛出一个错误的实例
```py
class SelfError(ValueError):
    pass

def foo(s):
    n = int(s)
    if n==0:
        raise SelfError('invalid value: %s' % s)
    return n

foo('0')
```