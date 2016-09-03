Python 模块管理
==========================

## Python 包管理工具
###  distutils
python 标准库的一部分，2000年发布。使用它能够进行python模块的安装和发布setup.py就是利用distutils的功能写成.
简单打包示例:创建两个文件test.py和setup.py,setup.py内空如下:
```py
from distutils.core import setup
setup(
    name='test',
    version='1.0',
    author='far',
    author_email='wujiyu115@gmail.com',
    url='htt://www.farwmarth.com',
    py_modules=['test'],
)
```
运行`python setup.py sdist`,会生成一个"test-1.0.zip"包.解压缩使用`python setup.py install` 就可以安装并使用test模块了

#### distutils包管理
```shell
#安装包
python setup.py install
#打包成压缩文件
python setup.py sdist
#打包成exe安装
python setup.py bdist_wininst
```

###  setuptools,distribute
一个为了增强 distutils 而开发的集合，2004年发布。它包含了 easy_install
这个工具setuptools是一个项目的名称,是基础组件,而 easy_install 是这个项目中提供的工具,它依赖基础组件工作.setuptools使用了一种新的文件格式（.egg），可以为Python包创建 egg文件。setuptools 可以识别.egg文件，并解析、安装它.
至于distribute，它是setuptools的一个分支版本。分支的原因是有一部分开发者认为 setuptools 开发太慢。但现在，distribute 又合并回了 setuptools 中，所以可以认为它们是同一个东西.
总的来说:

+ setuptools/distribute 都扩展了 distutils，提供了更多的功能
+ easy_install是基于setuptools/distribute的一个工具，方便了包的安装和省级
[setuptools包](https://pypi.python.org/pypi/setuptools)

#### 安装
```shell
#windows 安装 :下载  https://bootstrap.pypa.io/ez_setup.py 并执行python
#linux
wget https://bootstrap.pypa.io/ez_setup.py -O - | python
#mac
curl https://bootstrap.pypa.io/ez_setup.py -o - | python
```

#### 反安装
在windows上如果用exe或者是msi安装的,用安装包添加/删除项来进行反安装
其他情况先删除所有在`site-packages`目录和`sys.pyath`目录下的所有带`setuptools*` 和 `distribute*` 文件,然后移除在`scripts` 目录的`easy_install`和   `easy_install-x.x`可执行文件

#### 用easy_install管理包
[installing-easy-install](https://pythonhosted.org/setuptools/easy_install.html#installing-easy-install)
```shell
#版本
easy_install --version
#根据包名安装
easy_install SQLObject
#安装解压的目录
easy_install .
#安装egg,网址或者本地路径
easy_install http://example.com/downloads/ExamplePackage-2.0-py2.4.egg

#升级
easy_install --upgrade PyProtocols
easy_install "SomePackage==2.0"

#移除
easy_install -m PackageName
```



### pip

#### 安装pip
```shell
##安装pip
easy_install pip
## 安装pip2
curl https://bootstrap.pypa.io/get-pip.py | python
```

####  pip包管理
```shell
#安装:
pip install SomePackage
#安装指定版本号
pip install SomePackage==1.0.4
#指定源安装
pip install SomePackage -i https://pypi.doubanio.com/
#升级
pip install --upgrade SomePackage
#卸载:
pip uninstall SomePackage
#查看已安装
pip list
#查看可升级
pip list --outdated
#查看安装的路径及文件信息
pip show --files SomePackage


# 使用pip导出依赖文件列表
pip freeze > requirements.txt
# 根据依赖文件列表，自动安装对应的软件包
pip install -r requirements.txt
```

#### 显示python安装的模块
```shell
#在pyhton环境中输入
help('modules')
#列出某一模块版本
import yaml; print(yaml.__version__)
```

#### 显示python库路径
```shell
python -c "import sys;print(sys.path)"
```

#### 不同环境pip安装
```shell
python3 -m pip install xxx
```

### wheel,eggs
[wheel doc](http://wheel.readthedocs.io/en/latest/#)
wheel 本质上是一个 zip 包格式，它使用 .whl 扩展名，用于 python 模块的安装，它的出现是为了替代 Eggs。

+ 对纯python或者c扩展安装速度更快
+ 不用执意任意代码如:setup.py
+ 在win或者mac上安装c扩展不需要编译环境
+ 测试和持续集成提供更好的缓存
+ 创建pyc文件有来匹配编译器
+ 更好的跨平台安装能力

#### 使用
```
# Make sure you have the latest pip that supports wheel
pip install --upgrade pip

# Install wheel
pip install wheel

# Build a directory of wheels for pyramid and all its dependencies
pip wheel --wheel-dir=/tmp/wheelhouse pyramid

# Install from cached wheels
pip install --use-wheel --no-index --find-links=/tmp/wheelhouse pyramid

# Install from cached wheels remotely
pip install --use-wheel --no-index --find-links=https://wheelhouse.example.com/ pyramid
```

## Python 虚拟环境
### virtualenv
virtualenv用于创建独立的Python环境，多个Python相互独立，互不影响.
实测默认情况下虚拟环境不会依赖系统环境的global site-packages。比如系统环境里安装了MySQLdb模块，在虚拟环境里import MySQLdb会提示ImportError。如果想依赖系统环境的第三方软件包，可以使用参数`--system-site-packages`

安装
```shell
sudo apt-get install python-virtualenv
#or
pip install virtualenv
```
使用
```shell
#创建
virtualenv  test
#启动虚拟环境
cd test
source ./bin/activate
#退出虚拟环境
deactivate
```

### Virtualenvwrapper
Virtaulenvwrapper是virtualenv的扩展包，可以方便的创建、删除、复制、切换不同的虚拟环境。

安装
```shell
sudo easy_install virtualenvwrapper
mkdir $HOME/.virtualenvs
#在~/.bashrc中添加行：
export WORKON_HOME=$HOME/.virtualenvs
#在~/.bashrc中添加行：
source /usr/local/bin/virtualenvwrapper.sh
#运行：
 source ~/.bashrc
```
使用
```shell
#列出虚拟环境列表
workon
#也可以使用
lsvirtualenv
#新建虚拟环境
mkvirtualenv [虚拟环境名称]
#启动/切换虚拟环境
workon [虚拟环境名称]
#删除虚拟环境
rmvirtualenv [虚拟环境名称]
#离开虚拟环境
deactivate
```

## python技巧


### Consider a more secure location (set with .set_extraction_path or the PYTHON_EGG_CACHE environment variable)
```
chmod g-wx,o-wx . /home/far/.python-eggs/
```


### InsecurePlatformWarning: A true SSLContext object is not available.
```
 pip install pyopenssl ndg-httpsclient pyasn1
```

### pip cache location
```
%USERPROFILE%\AppData\Local\pip\cache
```


###  python安装easy_install失败的解决办法
错误详情:
```
File "C:\Python27\lib\mimetypes.py", line 249, in enum_types
ctype = ctype.encode(default_encoding) # omit in 3.x!
UnicodeDecodeError: 'ascii' codec can't decode byte 0xd7 in position 9: ordinal not in range(128)
```

解决:
在python安装目录Lib的`mimetypes.py`中，
`default_encoding = sys.getdefaultencoding()`
这句之前加上设置默认编码，添加如下语句：
```py
reload(sys)
sys.setdefaultencoding("cp936")
```

### 兼容python2和python3

#### 2to3
```
python C:\Python33\Tools\Scripts\2to3.py -w 2to3Test.py
```
https://github.com/mitsuhiko/python-modernize
http://haoluobo.com/2013/01/python2and3/