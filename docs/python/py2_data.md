Python 数据类型
==========================
## 变量

声明变量,变量定义无需关键字,必须赋值，与js一样,变量为动态类型，变量类型可变化
```python
x=1
x=True
print(x)
```
多元赋值
```python
x,y,z = 1,2,3
```
变量交换值
```python
x,y=y,x
```

## 数据类型

python数据类型有很多种,常见

+ `Booleans` ［布尔型］或为True［真］或为False［假］。
+ `Numbers` ［数值型］可以是Integers［整数］Floats［浮点数］Fractions［分数］(1/2)甚至是ComplexNumber［复数］。
+ `Lists` ［列表］是值的有序序列。
+ `Tuples` ［元组］是有序而不可变的值序列。
+ `Sets［` 集合］是装满无序值的包裹。
+ `Dictionaries` ［字典］是键值对的无序包裹。
+ `Strings` ［字符串型］是Unicode字符序列
+ `Bytes` ［字节］和ByteArrays［字节数组］，例如:一份JPEG图像文件。
+ `None`

###  布尔类型
`True`和`False`,布尔值可以用`and`、`or`和`not`运算
```py
three = True
```

```py
>>> True and True
True
>>> True and False
False
>>> False and False
False
>>> 5 > 3 and 3 > 1
True
```

###  数值类型

python的数值类型包括int,float,分数等,int和float仅通过小数点来区分它们.
```py
#判断数据类型
one = 1
two = 2.0
print(type(one))
print(type(two))
#判断是否是int类型
print(isinstance(one, float))
# 分数
import fractions
print(fractions.Fraction(1, 2))
```

###  列表(数组) :可以有相同元素,有序
```py
a =['a','b','c','d','e']
#第一个
print(a[0])
#倒数第一个
print(a[-1])
#第二个到第四个，(包括前不包括末尾)
print(a[1:3])
#第一个到第四个
print(a[:3])
#第二个到倒数第一个
print(a[1:-1])
#第四个到最后
print(a[3:])
#rang,a=[0,1,2,3,4]
a=list(range(5))
```
输出:
```
a
e
['b', 'c']
['a', 'b', 'c']
['b', 'c', 'd']
['d', 'e']
```

#### 列表遍历
```py
x = [ 2,5,8]
for y in x:
    print (y)
```

#### 列表添加项 :
```py
a =['a','b','c','d','e']
a = a+['f']
print(a)
a.append(True)
print(a)
a.insert(0,'g')
print(a)
a.extend(['h'])
print(a)
a.append(['j']);
print(a)
```
输出:
```
['a', 'b', 'c', 'd', 'e', 'f']
['a', 'b', 'c', 'd', 'e', 'f', True]
['g', 'a', 'b', 'c', 'd', 'e', 'f', True]
['g', 'a', 'b', 'c', 'd', 'e', 'f', True, 'h']
['g', 'a', 'b', 'c', 'd', 'e', 'f', True, 'h', ['j']]
```
+ `加号连接符`:不适合处理大型列表，应该他是创建新的列表然后赋值，消耗内存
+ `insert`:是才索引位置之前插入
+ `extend`:只可接受列表
+ `append`:可接受列表和单独元素

#### 列表检索值
```py
  a = ['a','a', 'b', 'c', 'd', 'e']
  print(a.count('a'))
  print(a.index('a'))
  print('a' in a)
```
+ `count`:计算元素出现的次数
+ `index`:返回元素第一次出现的索引位置，不在在会引发异常
+ `in` :用于搜索元素是否存在于此列表中，比count速度快

#### 列表删除元素:
```py
a = ['a', 'b', 'c', 'd', 'e']
a.remove('a')
print(a)
a.pop()
print(a)
a.pop(0)
print(a)
```
+ `remove`:删除指定元素，元素不在在会引发异常
+ `pop`:不带参数时 删除最后一个元素,空列表引发异常

###  元组 :不可改变的列表，用于常量
```py
arr=(1,2,3)
a=list(arr)
print(a)
b= tuple(a)
print(b)
```
+ `list`:将元组转换成列表
+ `tuple`:将列表转换成元组

###  集合:无序，唯一
```py
sets = {1, 2, 3, 4}
```
#### 修改集合
+ `add`:只可添加一个参数,不能是集合,列表
+ `update`:只能是集合或列表,可多个参数
```py
arr  = [5,6,7]
sets = {1, 2, 3, 4}
sets.add(8)
print(sets)
sets.update(arr);
print(sets)
```

#### 删除集合
+ `discard`: 接受一个参数,不会引发异常
+ `remove`:接受一个参数,会引发异常
+ `pop`:随机弹出一个元素,会引发异常
+ `clear`:清除所有值，相当于重置集合
```py
sets = {1, 2, 3, 4}
sets.discard(1)
print(sets)
sets.remove(2)
print(sets)
sets.pop()
print(sets)
sets.clear()
print(sets)
```

#### 常见集合操作
```py
sets = {1, 2, 3, 4}
setsA = {4,5,6,7}

'''并集'''
setsB =sets.union(setsA)
print(setsB)

'''交集'''
setsC = sets.intersection(setsA)
print(setsC)

'''只出现在sets不在setsA中的元素'''
setsD = sets.difference(setsA)
print(setsD)

'''两个集合不相同的元素集合'''
setsE = sets.symmetric_difference(setsA)
print(setsE)

'''sets是否是setsA的子集(sets元素在setsA中全有)'''
print(sets.issubset(setsA))
'''sets是否是setsA的超集'''
print(sets.issuperset(setsA))
```

###  字典:有键值对的无序集合
#### 操作
```py
dic = {1000:1, 2000:2}
print(dic[1000])
dic[1000]=3
print(dic[1000])
```
字典不允许有重复值，对现有键的赋值会覆盖旧值。 添加新键值和修改值语法一样

#### 遍历字典
```py
dict={"a":"apple","b":"banana","o":"orange"}
for i in dict:
        print ("dict[%s]=" % i,dict[i] )
for (k,v) in  dict.items():
        print ("dict[%s]=" % k,v  )
```



###  字符串

#### 格式化
```py
str = 'myname is {0}'
print(str.format('wujiyu'))

dics =['kb','mb']
'''参数可以是复合元素1mb=1024kb'''
strarr = '1{0[1]}=1024{0[0]}'
print(strarr.format(dics))

'''.1 保留到小数点第一位'''
print('{0:.1f}{1}'.format(698.24,'GB'))
```
#### 其他
```py
str = 'Myname=2&pas=3'
'''小写'''
print(str.lower())
'''分段'''
print(str.split('&'))
'''计数'''
print(str.count('a'))
'''切片'''
print(str[0:3])
```

### Byte
```py
x = b'ABC'
```