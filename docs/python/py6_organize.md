python模块组织
========================

## python模块
模块提高了代码的可维护性
一个`abc.py`的文件就是一个名字叫`abc`的模块
每一个包目录下面都会有一个`__init__.py`的文件，这个文件是必须存在的，否则，Python就把这个目录当成普通目录
```
package1/
    __init__.py
    subPack1/
        __init__.py
        module_11.py
        module_12.py
        module_13.py
    subPack2/
        __init__.py
        module_21.py
        module_22.py

我们在module_11.py中定义一个函数:

def funA():
    print "funcA in module_11"
    return

在顶层目录（也就是package1所在的目录）运行python:

from package1.subPack1.module_11 import funcA
funcA()
----->funcA in module_11
```

## 使用模块
`import` 关键字用于导入模块
`from` 标明引用模块路径
```py
import sys
if __name =="__main__":
    print(sys.argv)
```

##模块搜索路径
Python解释器会搜索当前目录、所有已安装的内置模块和第三方模块，搜索路径存放在sys模块的path变量中

### 添加自己的搜索目录:
####  直接修改sys.path，添加要搜索的目录
```py
import sys
sys.path.append('/Users/michael/my_py_scripts')
```
#### 设置环境变量`PYTHONPATH`