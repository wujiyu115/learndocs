c++　windows　环境搭建
===================

##  一　安装mingw-get-inst
 这个是c++的win上的编译环境.下载地址:[http://sourceforge.net/projects/mingw/files/](http://sourceforge.net/projects/mingw/files/ "http://sourceforge.net/projects/mingw/files/")

![image](http://farwmarth.bestnewbee.com/images/uploads/2013/03/image11.png)

![image](http://farwmarth.bestnewbee.com/images/uploads/2013/03/image12.png)

它会自动联网下载包

![image](http://farwmarth.bestnewbee.com/images/uploads/2013/03/1343535491_9244.jpg)

安装完后配置环境变量

+ 打开环境变量，添加mingw_home变量，内容 x:\Program Files\MinGW
+ 设置path变量，编辑path变量添加 %mingw_home%\bin;%mingw_home%\msys\1.0\bin;
+ 添加LIBRARY_PATH变量，内容 %mingw_home%\lib
+ 添加C_INCLUDE_PATH变量，内容 %mingw_home%\include

改名,将mingw的bin目录下的mingw32-make.exe拷贝一份改名为make.exe

![image](http://farwmarth.bestnewbee.com/images/uploads/2013/03/image13.png)

重启电脑。

## 二 下载　eclipse for c/c++　
 我下载的是 eclipse-cpp-juno-SR2-win32 [http://www.eclipse.org/downloads/packages/eclipse-ide-cc-developers/junosr2](http://www.eclipse.org/downloads/packages/eclipse-ide-cc-developers/junosr2 "http://www.eclipse.org/downloads/packages/eclipse-ide-cc-developers/junosr2")

解压之后打开，配置如下.

![image](http://farwmarth.bestnewbee.com/images/uploads/2013/03/image14.png)

三　hello,world
 新建c++工程,选择MinGW GCC

![image](http://farwmarth.bestnewbee.com/images/uploads/2013/03/image15.png)

编译运行.编译点击锤子，运行点击三角形的按钮。
![image](http://farwmarth.bestnewbee.com/images/uploads/2013/03/image16.png)

![image](http://farwmarth.bestnewbee.com/images/uploads/2013/03/image17.png)