Python 疑难杂症
==========================


## Consider a more secure location (set with .set_extraction_path or the PYTHON_EGG_CACHE environment variable)
```
chmod g-wx,o-wx . /home/far/.python-eggs/
```


## InsecurePlatformWarning: A true SSLContext object is not available.
```
 pip install pyopenssl ndg-httpsclient pyasn1
```

## pip cache location
```
%USERPROFILE%\AppData\Local\pip\cache
```


##  python安装easy_install失败的解决办法
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

## 兼容python2和python3

### 2to3
```
python C:\Python33\Tools\Scripts\2to3.py -w 2to3Test.py
```

## ImportError: No module named _sqlite3 报错解决方法
```shell
#centos
yum -y install sqlite-devel
#重新安装
yum install python

#mac
brew install sqlite3
#重新安装
brew install 2.7.9
```