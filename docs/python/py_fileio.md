python  文件io
==========================




## 文件读写

### 文件读取
```py
  o_file = open(os.path.realpath("python.txt"), 'r',encoding='GBK')
  print(o_file.read())
  o_file.close()
```
os.path.realpath　拿到当前脚本的目录. r 设置属性，只读，mode=’rb’读取为二进制数据 encoding  指定编码方式 o_file.close()　 显式地关闭文件，但是未销毁文件对象，而且在出现异常时不一定能关闭文件流，所有python3有另一种写法 用with来创建运行时环境，不管是否有异常在with块结束后文件都会关闭，类似于函数作用域中的局部变量
```py
  with open(os.path.realpath("python.txt"), 'r',encoding='GBK') as o_file:
      print(o_file.read())
```
每次读取一行:
```py
  line_number=0
  with open(os.path.realpath("python.txt"), 'r',encoding='GBK') as o_file:
      for a_line in o_file:
          line_number+=1
          print('{0}\t{1}'.format(line_number,a_line.rstrip()))
```
因为流对象也是一个迭代器，所以可以用for循环来读取，python会自动识别行结束符。rstrip()是去空白字符串的方法.
使用`readlines `:
```py
f = open("python.txt","r")
for  line in f.readlines():
  print(line.strip())# 把末尾的'\n'删掉
  f.close()
```
### 写入文本
```py
  with open(os.path.realpath("python.txt"), mode='w') as o_file:
      o_file.write('test success\n')
  with open(os.path.realpath("python.txt"), 'r',encoding='GBK') as o_file:
      print(o_file.read())
```
与读取文本方式相同，mode=’w’代表覆盖，mode=’a’代表追加 。

### StringIO
内存中的字符缓冲流
```py
from io import StringIO
f = StringIO()
f.write("hello\n")
f.write("far")
print(f.getvalue())
f.close()
```
### BytesIO
内存中的字节缓冲流
```py
from io import BytesIO
f = BytesIO()
f.write('中文'.encode('utf-8'))
print(f.getvalue())
f.close()
```

### XML操作

### 解析XML

python解析有Dom和Sax解析器，现在我们来用另外一个库:xml.etree.ElementTree来解析
```xml
  <data>
       <name id='1'>wujiyu</name>
       <name id='2'>wujiyu</name>
  </data>
```

```py
  import os
  import xml.etree.ElementTree as etree
  tree = etree.parse(os.path.realpath("py.xml"))
  root=tree.getroot()
  print(root)#<Element 'data' at 0x02B1A330>
  print(root[0].tag)#name
  print(root[0].attrib['id'])#1
  print(len(root.findall('name')))#2
```
所有节点都为列表，所有属性都为字典,findall用来查找元素，可嵌套查找。 对于大型xml可用lxml来代替ElementTree,lxml是一个第三方的开源库，findall查找语法更强大,api与ElementTree相同

### 创建xml

我们用下lxml吧，lxml是第三方库，需要安装，地址: [http://www.lfd.uci.edu/~gohlke/pythonlibs/#lxml](http://www.lfd.uci.edu/~gohlke/pythonlibs/#lxml) 看准对应的二进制版本

开始创建xml
```py
  from lxml import etree
  import os
  newfeed = etree.Element('feed')
  names = etree.SubElement(newfeed,'name',attrib={'old':'yes','id':'1'})
  names.text='wujiyu'
  print(etree.tostring(newfeed))

```
 tutorial：[http://lxml.de/tutorial.html](http://lxml.de/tutorial.html "http://lxml.de/tutorial.html")

 ## 文件路径操作
```py
   import  os
   # 列出当前目录文件
   print(os.listdir(path='.'))  # ['python.txt', 'init.py']
   # 切割地址
   print(os.path.split('ab/a.txt'))  # ('ab', 'a.txt')
   # 把文件名分成文件名称和扩展名
   print(os.path.splitext('ab/a.txt'))  # ('ab/a', '.txt')
   # 文件+扩展名
   print(os.path.basename('bcd/abc'))  # abc
  # 创建一级目录
  os.mkdir('test1')
  # 创建多级目录
  os.makedirs('test2/test')
  # 删除一个目录下所有东西
  os.removedirs('path')
  # 删除一个目录，而且一定要空，否则os.errer
  os.rmdir('path')
```

#### 相对路径,绝对路径
```
print("sys.path:"+sys.path[0])
print("sys.argv:"+sys.argv[0])
print("__file__:"+__file__)
print("os.getcwd():"+os.getcwd())
print("os.path.realpath():"+os.path.realpath(__file__))
print("(os.path.abspath(__file__)):"+os.path.abspath(__file__))
print("os.path.dirname(os.path.realpath()):"+os.path.dirname(os.path.realpath(__file__)))
print("os.path.dirname(os.path.abspath(__file__)):"+os.path.dirname(os.path.abspath(__file__)))

结果:

sys.path:/Users/far/Develop/git/py_search/cloudapps/auto_sign
sys.argv:/Users/far/Develop/git/py_search/cloudapps/auto_sign/test.py
__file__:/Users/far/Develop/git/py_search/cloudapps/auto_sign/zimuzu/zumuzu_tv.py
os.getcwd():/Users/far/Develop/git/py_search/cloudapps/auto_sign
os.path.realpath():/Users/far/Develop/git/py_search/cloudapps/auto_sign/zimuzu/zumuzu_tv.py
(os.path.abspath(__file__)):/Users/far/Develop/git/py_search/cloudapps/auto_sign/zimuzu/zumuzu_tv.py
os.path.dirname(os.path.realpath()):/Users/far/Develop/git/py_search/cloudapps/auto_sign/zimuzu
os.path.dirname(os.path.abspath(__file__)):/Users/far/Develop/git/py_search/cloudapps/auto_sign/zimuzu
[Finished in 0.1s]
```

## 序列化

### pickle
+ `dump`: 将对象系列化到文件中
+ `load`: 从文件中加载系列化信息
```py
import pickle

t = {"name":"far"}
with open('dump.txt', 'wb') as f:
  pickle.dump(t,f)
with open('dump.txt', 'rb') as f:
  d = pickle.load(f)
  print(d)

'''
{'name': 'far'}
'''
```

### json
#### 基础
+ `dumps`:将对象转化成字符串
+ `loads`:将字符串转化成对象
+ `load`:加载文件对象将字符串转化成对象
```py
import json

t = {"name":"far"}
t_str= json.dumps(t)
print(t_str)
d = json.loads(t_str)
print(d)
d = json.load(open("dump.json","r"))
print(d)
'''
{"name": "far"}
{'name': 'far'}
{'name': 'far'}
'''
```

#### 转换类对象
```py
import json

class Student(object):
  def __init__(self, name, age, score):
    self.name = name
    self.age = age
    self.score = score

#转换成可解析字符串对象
def student2dict(std):
  return {
    'name': std.name,
    'age': std.age,
    'score': std.score
  }

#转换成对象
def dict2student(d):
  return Student(d['name'], d['age'], d['score'])

s = Student('Bob', 20, 88)
d = json.dumps(s,default=student2dict) #自定义一个转换函数
print(d)
d =json.dumps(s,default=lambda obj: obj.__dict__)#使用对象的dict属性
print(d)
s1 = json.loads(d,object_hook=dict2student)
print(s1.name,s1.age,s1.score)
'''
{"score": 88, "name": "Bob", "age": 20}
{"score": 88, "age": 20, "name": "Bob"}
Bob 20 88
'''
```