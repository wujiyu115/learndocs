python控制流程
==========================

## 条件判断
### `if`判断
```py
# if
if True:
    print("It is True")

#if ..else
age =18
if age>=18:
    print("adlut")
else:
    print("child")

#if ..elif..else
v=1
if v==1:
    print("v1")
elif v==2:
    print("v2")
else:
    print("vother")
```

## 循环
python 循环作用对象:`list` ,`tuple`,`dict`,`set`,`str`,`generator`,这些可以直接作用于for循环的对象统称为可迭代对象:`Iterable`
### `for`循环
```python
#for...in
names = ['a', 'b', 'c']
for name in names:
    print(name)

# 加索引
for index,name in enumerate(names):
    print(index,name)

# 多个变量
for x, y in [(1, 1), (2, 4), (3, 9)]:
    print(x,y)

#循环字符串
fo_str = "hello"
for word in fo_str:
    print(word)

```


###` while`循环
```py
n = 0
while n <5:
    n = n +1
```

## 表达式
```py
  print(11/2) #5.5
  print(11//2)#5
  print(-11//2)#-6
  print(11.0//2)#5.0
  print(11**2)#121
  print(11%2)#1
```
1.  / 运算符执行浮点除法。即便分子和分母都是 int，它也返回一个 float 浮点数。
2.  // 运算符执行古怪的整数除法。如果结果为正数，可将其视为朝向小数位取整（不是四舍五入），但是要小心这一点。
3.  当整数除以负数， // 运算符将结果朝着最近的整数“向上”四舍五入。从数学角度来说，由于 −6 比 −5 要小，它是“向下”四舍五入，如果期望将结果取整为 −5，它将会误导你。
4.  // 运算符并非总是返回整数结果。如果分子或者分母是 float，它仍将朝着最近的整数进行四舍五入，但实际返回的值将会是 float 类型。
5.  ** 运算符的意思是“计算幂”，112结果为 121 。
6.  % 运算符给出了进行整除之后的余数。11 除以 2 结果为 5 以及余数 1，因此此处的结果为 1。 7  不支持自增(++)自减(—)