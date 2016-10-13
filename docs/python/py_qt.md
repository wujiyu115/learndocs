开始用pyqt造窗体
==========================

## 依赖环境
+ [qt5.5.1](http://download.qt.io/official_releases/qt/5.5/5.5.1/)
+ [python3.4](https://www.python.org/downloads/release/python-343/)
+ [pyqt](http://sourceforge.net/projects/pyqt/files/PyQt5/PyQt-5.5.1/PyQt5-5.5.1-gpl-Py3.4-Qt5.5.1-x32.exe)
+ [pycharm](http://www.jetbrains.com/pycharm/)
+ qt designer

## 安装
windows下的安装过程就不再累赘了，讲下mac下的pyqt安装

+ 首先安装[sip](https://www.riverbankcomputing.com/software/sip/download)
```shell
#sip安装：
python configure.py
make
make install
```
+ 编译pyqt
```shell
#如果qt安装后没有加入环境变量，指定qmake路径
python configure.py --qmake /Users/far/Qt5.5.1/5.5/clang_64/bin/qmake
make
make install
```
编译到最后会报错找不到qpeolocation.h，解决办法是从github上下载一个头文件到PyQt-gpl-5.5.1/QtPositioning/qgeolocation.h，[地址](https://github.com/RSATom/Qt/blob/master/qtlocation/src/positioning/qgeolocation.h)

## 工具配置
1. **pycharm破解**
注册时选择在线服务器注册，填入:`http://idea.lanyus.com/`
2. **配置pycharm**
	- 配置qtdesigner工具,用qtdesigner打开`.ui`文件
	![qtdesigner](http://ww1.sinaimg.cn/large/4b5a1116gw1ezy5cutpbdj20ss0eoad8.jpg)
	- 配置pyuic工具,将`.ui`文件转换成`.py`文件
	![pyuic](http://ww2.sinaimg.cn/large/4b5a1116gw1ezy5d804cvj20ss0eoadb.jpg)
	- 然后就可以使用配置的外置工具来编辑`.ui`文件了  
	![tools](http://ww1.sinaimg.cn/large/4b5a1116gw1ezy5ddu7xej20dx0ha0wv.jpg)
	![qtdesigner_open](http://ww4.sinaimg.cn/large/4b5a1116gw1ezy5dilzpqj20hs0avwhk.jpg)

## 开始愉快地写代码

<http://zetcode.com/gui/pyqt5/introduction/>

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Author: anchen
# @Date:   2015-07-27 20:19:33
# @Last Modified by:   anchen
# @Last Modified time: 2015-07-27 20:26:29
import sys
from PyQt5.QtWidgets  import  QApplication,QWidget
from PyQt5.QtGui import QIcon

if  __name__ == '__main__':

            app =  QApplication(sys.argv)

            w = QWidget ()
            w.resize(250,150)
            w.move(300,300 )
            w.setWindowTitle("simpel window")
            w.setWindowIcon(QIcon("icon.png"))
            w.show()

            sys.exit(app.exec_())
```



