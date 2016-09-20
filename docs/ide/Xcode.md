title: xcode  配置 
date: 2016-02-25 19:10:39
tags: xcode
---

###  下载
+ apple官方下载
https://developer.apple.com/downloads/
+ 网友下载的原版镜像
http://blog.csdn.net/zhangao0086/article/details/38491271

### 验证xcode是否是正版
```
spctl --assess --verbose /Applications/Xcode.app
出现
/Applications/Xcode.app: accepted
source=Apple System
则是官方来源
```

### Xcode常用的快捷键
+ `cmd + shift + o`  快速查找类，可以快速跳到指定类的源码中
+ `ctrl + 6` 列出当前文件中的所有方法
+ `cmd + 1` 切换成Project Navigator
+ `cmd + ctrl + up` 在.h和.m文件之间切换
+ `cmd + enter` 切换成standard editor
+ `cmd + opt + enter` 切换成 assistant editor
+ `cmd + shift + y` 切换console View的现实或隐藏
+ `cmd + 0` 隐藏左边得导航区
+ `cmd +opt + 0` 隐藏右边的工具区
+ `cmd + ctrl + Left/Right` 到上/下一次编辑的位置
+ `cmd + opt +j` 跳转到文件过滤区
+ `cmd + shift +F`   ** 在工程中查找**
+ `cmd + R` 运行
+ `cmd + b` 编译
+ `cmd +shift + k` 清空编译好的文件
+ `cmd + .` 结束本次调试
+ `ESC` 调出代码补全功能
+ `cmd + t` 新建一个tab栏
+ `cmd + shift + [` 在tab栏之间切换
+ `cmd + 单击` 查看该方法的实现
+ `opt + 单击` 查看该方法的实现


###  Xcode  设置及问题

####  设置
+ Xcode Release版本   -- manager Schement
更改真机调试os版本  PROJECT --> Build Settings --> Deployment --> IOS Deployment Target

+ Xcode编码
打开Xcode的Preferences-----Text     Editing---Editing----Default text encoding

+ 安装Xcode 插件管理器:
mkdir -p ~/Library/Application\ Support/Developer/Shared/Xcode/Plug-ins;
curl -L http://goo.gl/xfmmt | tar xv -C ~/Library/Application\ Support/Developer/Shared/Xcode/Plug-ins

####   问题
+ xcode5 ios7 framework not found IOKit
解决办法，打开终端：
cd /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS7.0.sdk/System/Library/Frameworks/IOKit.framework
sudo ln -s Versions/A/IOKit

+ Choose a destination with a supported architecture in order to run on this device.
其实只要把编译器改成现在的LLVM即可。 修改位置Project->Build Settings -> Build Options.


+ 使用Xcode5 SVN 出现问题
The operation couldn’t be completed. (NSURLErrorDomain error -1012.)
解决方法：
打开终端 然后输入如下命令
svn ls xxxx  (xxx是你SVN Server的地址)
这里询问你是否允许这个地址的访问，我们输入 “ p ”，然后回车即可。
验证是否OK的方法：
再次控制台输入  svn ls xxxx
当不再提示让你选择是否允许的提示，而是直接控制台出现如下信息，说明OK了

