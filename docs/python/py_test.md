python 测试与调试
========================

## 调试
参考:<https://www.ibm.com/developerworks/cn/linux/l-cn-pythondebugger/>  
刚写出来的程序不可能100%没有错误,语法错误可以在错误信息中定位到,逻辑错误则需要一定的调试技巧.

### print
`print`是所有语言最简单粗暴的调试方式,在程序逻辑各处加上打印,看输出
```py
def foo(s):
	n = int(s)
	print('>>> n = %d' % n)
	return 10 / n

def main():
	foo('0')
main()
```

###断言
`assert`的意思是，表达式结果应该是`True`，否则报错
```py
def foo(s):
	n = int(s)
	assert n != 0, 'n is zero!'
	return 10 / n

def main():
	foo('0')

main()
```

### pdb
pdb是python自带的调试器,可以单步调试,查看单步中的状态
`python3 -m pdb test.py` 启动pdb调试,

| 命令                | 解释                       |
|---------------------|----------------------------|
| break 或 b 设置断点 | 设置断点                   |
| continue 或 c       | 继续执行程序               |
| list 或 l           | 查看当前行的代码段         |
| step 或 s           | 进入函数                   |
| return 或 r         | 执行代码直到从当前函数返回 |
| exit 或 q           | 中止并退出                 |
| next 或 n           | 执行下一行                 |
| pp                  | 打印变量的值               |
| help                | 帮助                       |


### ide
#### PyCharm
#### PyDev

## 单元测试
单元测试是用来对一个模块、一个函数或者一个类来进行正确性检验的测试工作
为了编写单元测试，我们需要引入Python自带的`unittest`模块

+ `assertEqual`:是否相等
+ `assertTrue` :条件是否满足
+ `setUp`:每调用一个测试方法前执行
+ `tearDown`:每调用一个测试方法后执行
```py
import unittest

class Logic(object):
	"""docstring for Logic"""
	def __init__(self, arg):
		super(Logic, self).__init__()
		self.arg = arg

class Test(unittest.TestCase):
	def test_init(self):
		l = Logic(1)
		self.assertEqual(l.arg,2)

if __name__ == '__main__':
	unittest.main()
```
运行测试:

+ `python3 -m unittest test`
+ `python test.py`