selenium简介
==========================

##  Selenium 简介

Selenium是一个用于Web应用程序测试的工具。Selenium测试直接运行在浏览器中，就像真正的用户在操作一样。支持的浏览器包括IE、Mozilla Firefox、Mozilla Suite等。这个工具的主要功能包括：测试与浏览器的兼容性——测试你的应用程序看是否能够很好得工作在不同浏览器和操作系统之上。测试系统功能——创建衰退测试检验软件功能和用户需求。支持自动录制动作和自动生成。Net、Java、Perl等不同语言的测试脚本。Selenium 是ThoughtWorks专门为Web应用程序编写的一个验收测试工具

## Selenium Python环境安装
```shell
pip install selenium
#如果是friefox下载webdriver加入环境变量
https://github.com/mozilla/geckodriver/releases
```

## 简单截图功能
```py
# -*- coding: utf-8 -*-  指定编码
#行注释
'''
Created on 2016年10月17日
@author: far
'''

from selenium import webdriver
from selenium.webdriver.firefox.firefox_binary import FirefoxBinary


binary = FirefoxBinary(r"D:\Program Files (x86)\Mozilla Firefox\firefox.exe")
brower = webdriver.Firefox(firefox_binary=binary)
url = "http://www.baidu.com"
brower.set_window_size(1200,900)
brower.get(url)

brower.save_screenshot("test.png")
brower.close()

```