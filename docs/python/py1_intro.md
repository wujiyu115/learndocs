Python 3 入门介绍
==========================

##  python 简介

Python是一种解释型、面向对象、动态数据类型的高级程序设计语言.设计者开发时总的指导思想是，对于一个特定的问题，只要有一种最好的方法来解决就好了.Python是完全面向对象的语言。函数、模块、数字、字符串都是对象。并且完全支持继承、重载、派生、多继承，有益于增强源代码的复用性。Python支持重载运算符和动态类型,python应用很广泛(系统编程 图形处理 数学处理 文本处理 数据库编程 网络编程 Web编程 多媒体应用).

##  python 3 的变化

[https://gist.github.com/scturtle/3060332](https://gist.github.com/scturtle/3060332 "https://gist.github.com/scturtle/3060332")

##  开发环境

+ python: [http://www.python.org/getit/](http://www.python.org/getit/ "http://www.python.org/getit/")
+ IDE : eclipse + pydev

安装完pydev后 配置python解释器

[![4](http://farwmarth.com/wp-content/uploads/2013/03/4.jpg)](http://farwmarth.com/?attachment_id=471)


##  语法

###  缩进

Python开发者有意让违反了缩进规则的程序不能通过编译，以此来强制程序员养成良好的编程习惯。并且在Python语言里，缩进而非花括号或者某种关键字，被用于表示语句块的开始和退出。增加缩进表示语句块的开始，而减少缩进则表示语句块的退出。缩进成为了语法的一部分。例如

```python
if age < 21:
    print("你不能買酒。")
    print("不過你能買口香糖。")
    print("這句話處於if語句塊的外面。")
```

根据PEP的规定，必须使用4个空格来表示每级缩进。使用Tab字符和其它数目的空格虽然都可以编译通过，但不符合编码规范。支持Tab字符和其它数目的空格仅仅是为了兼容很旧的Python程序和某些有问题的编辑器。

### 注释

标示注释:以`#`号开头，一行都为注释内容
文档注释:` ‘‘‘` 三个单引号开头，三个单引号结尾


##  输入输出

### 输出:`print`
```python
#多参数输出
print("hello","world")
#格式输出
pi=3.14
print("%d,haha"%3)
```

#### 字符串格式化转换类型
| 转换类型 |          含义|
| ---     |:-----------:|
|d,i      |           带符号的十进制整数|
|o        |           不带符号的八进制|
|u           |        不带符号的十进制|
|x         |           不带符号的十六进制（小写）|
|X       |            不带符号的十六进制（大写）|
|e          |         科学计数法表示的浮点数（小写）|
|E            |       科学计数法表示的浮点数（大写）|
|f,F           |      十进制浮点数|
|g         |          如果指数大于-4或者小于精度值则和e相同，其他情况和f相同|
|G         |         如果指数大于-4或者小于精度值则和E相同，其他情况和F相同|
|C        |          单字符（接受整数或者单字符字符串）|
|r        |            字符串（使用repr转换任意python对象)|

|s        |           字符串（使用str转换任意python对象）|

### 输入:`input`
```python
name = input('please enter your name:')
print("hello",name)
```
