常用内建模块
================================

## re模块(正则)

### `match`判定是否有匹配
```py
import re
test = 'qqq'
print(re.match(r'q', test))
print(re.match(r's', test))
'''
<_sre.SRE_Match object at 0x01D07A30>
None
'''
```

### split
空格分割
```py
re.split(r'\s+', 'a b   c')
'''
['a', 'b', 'c']
'''
```

### 分组提取
`()`用于分组,下面是提取区号和电话号码`group(0)`返回原始字符串
```py
m = re.match(r'^(\d{3})-(\d{3,8})$', '010-12345')
print(m.group(0),m.group(1),m.group(2))
'''
('010-12345', '010', '12345')
'''
```

### 贪婪匹配
`?`为非贪婪匹配
```py
re.match(r'^(\d+)(0*)$', '102300').groups()
re.match(r'^(\d+?)(0*)$', '102300').groups()
'''
('102300', '')
('1023', '00')
'''
```

### 预编译正则表达式
```py
re_telephone = re.compile(r'^(\d{3})-(\d{3,8})$')
re_telephone.match('010-12345').groups()
```

## datetime(时间处理)
```py
from datetime import datetime,timedelta,timezone

print(datetime.now())

#指定时间创建对象
dt = datetime(2016, 7, 26, 12, 0,0)
print(type(dt),dt)

#datetime转换为timestamp
#1970年1月1日 00:00:00 UTC+00:00时区的时刻称为epoch time，记为0
print("datetime转换为timestamp")
print(dt.timestamp())

#timestamp转换为datetime
print("timestamp转换为datetime")
t = 1469505600
print(datetime.fromtimestamp(t))# 本地时间,东八区
print(datetime.utcfromtimestamp(t)) # UTC时间

#str转换为datetime
print("str转换为datetime")
cday = datetime.strptime('2016-7-26 12:00:00', '%Y-%m-%d %H:%M:%S')
print(cday)

# datetime转换为str
print("datetime转换为str")
now = datetime.now()
print(now.strftime('%Y-%m-%d %H:%M:%S'))

#datetime加减
print("datetime加减")
now = datetime.now()
print(now)
now =now + timedelta(days=2, hours=12)
print(now)

print("时区转换")
# 拿到UTC时间，并强制设置时区为UTC+0:00:
utc_dt = datetime.utcnow().replace(tzinfo=timezone.utc)
print(utc_dt)
# astimezone()将转换时区为北京时间:
bj_dt = utc_dt.astimezone(timezone(timedelta(hours=8)))
print(bj_dt)
# astimezone()将转换时区为东京时间:
tokyo_dt = utc_dt.astimezone(timezone(timedelta(hours=9)))
print(tokyo_dt)
# astimezone()将bj_dt转换时区为东京时间:
tokyo_dt2 = bj_dt.astimezone(timezone(timedelta(hours=9)))
print(tokyo_dt2)
'''
2016-07-27 12:46:08.216245
<class 'datetime.datetime'> 2016-07-26 12:00:00
datetime转换为timestamp
1469505600.0
timestamp转换为datetime
2016-07-26 12:00:00
2016-07-26 04:00:00
str转换为datetime
2016-07-26 12:00:00
datetime转换为str
2016-07-27 12:46:08
datetime加减
2016-07-27 12:46:08.235246
2016-07-30 00:46:08.235246
时区转换
2016-07-27 04:46:08.236246+00:00
2016-07-27 12:46:08.236246+08:00
2016-07-27 13:46:08.236246+09:00
2016-07-27 13:46:08.236246+09:00
'''
```

## collections

### namedtuple
`namedtuple`是一个函数，它用来创建一个自定义的`tuple`对象，并且规定了`tuple`元素的个数，并可以用属性而不是索引来引用`tuple`的某个元素
```py
from collections import namedtuple
Point = namedtuple('Point', ['x', 'y'])
p = Point(1, 2)
print(p,p.x,p.y)
print(isinstance(p,Point))
print(isinstance(p,tuple))
'''
Point(x=1, y=2) 1 2
True
True
'''
```

### deque
deque是为了高效实现插入和删除操作的双向列表，适合用于队列和栈
```py
from collections import deque
q = deque(['a', 'b', 'c'])
q.pop()
q.popleft()
q.append('x')
q.appendleft('y')
print(q)
'''
deque(['y', 'b', 'x'])
'''
```

### defaultdict
使用dict时，如果引用的Key不存在，就会抛出KeyError。如果希望key不存在时，返回一个默认值，就可以用defaultdict：
```py
from collections import defaultdict
dd = defaultdict(lambda: 'N/A')
dd['key1'] = 'abc'
print(dd['key1'] )
print(dd['key2'] )
'''
abc
N/A
'''
```

### OrderedDict
使用dict时，Key是无序的,使用dict时，Key是无序的,OrderedDict保证了插入顺序
```py
from collections import OrderedDict
d = dict([('a', 1), ('b', 2), ('c', 3)])
od =OrderedDict([('a', 1), ('b', 2), ('c', 3)])
print(d.keys())
print(od.keys())
'''
dict_keys(['a', 'c', 'b'])
odict_keys(['a', 'b', 'c'])
'''
```

### Counter
`Counter`是一个简单的计数器
```py
from collections import Counter
c = Counter()
for ch in 'programming':
     c[ch] = c[ch] + 1
print(c)
'''
Counter({'m': 2, 'g': 2, 'r': 2, 'n': 1, 'i': 1, 'o': 1, 'a': 1, 'p': 1})
'''
```

## base64
```py
import base64
b=base64.b64encode(b'binary\x00string')
print(b)
s  = base64.b64decode(b)
print(s)
#由于标准的Base64编码后可能出现字符+和/，在URL中就不能直接作为参数，所以又有一种"url safe"的base64编码，其实就是把字符+和/分别变成-和_：
bu = base64.urlsafe_b64encode(b'i\xb7\x1d\xfb\xef\xff')
print(bu)
su = base64.urlsafe_b64decode(bu)
print(su)
```

## struct(进制数据类型的转换)
`>`表示字节顺序是`big-endian`，也就是网络序，`I`表示4字节无符号整数
```py
import struct
b = struct.pack('>I', 10240099)
print(b)
v = struct.unpack('>I',b)
print(v)
'''
b'\x00\x9c@c'
(10240099,)
'''
```
struct模块定义的数据类型可以参考Python官方文档：
https://docs.python.org/3/library/struct.html#format-characters

## hashlib

```py
import hashlib

md5 = hashlib.md5()
md5.update("hello".encode('utf-8'))
md5.update("world".encode('utf-8'))
print(md5.hexdigest())

#sha1
sha1 = hashlib.sha1()
sha1.update('how to use sha1 in '.encode('utf-8'))
sha1.update('python hashlib?'.encode('utf-8'))
print(sha1.hexdigest())
'''
fc5e038d38a57032085441e7fe7010b0
2c76b57293ce30acef38d98f6046927161b46a44
'''
```

## itertools
```py
import itertools
#创建无限迭代器
na = itertools.count(1)
for x in na:
    print(x)
    pass
'''12345...'''

#循环重复迭代
cs = itertools.cycle('ABC')
'''ABCABC'''

#指定次数重复
ns = itertools.repeat('A', 4)
for x in ns:
    print(x)
    pass
'''AAAA'''

#迭代对象串联起来
for c in itertools.chain('ABC', 'XYZ'):
    print(c)
'''ABCXYZ'''

#相邻的重复元素分级
for key, group in itertools.groupby('AaBBAA', lambda c: c.upper()):
    print(key, list(group))
'''
A ['A', 'a']
B ['B', 'B']
A ['A', 'A']
'''
```