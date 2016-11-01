python爬虫初探
========================

## pyspider
`github` : <https://github.com/binux/pyspider>

其他文档
+ <http://docs.pyspider.org/en/latest/>
+ <http://blog.binux.me/2015/01/pyspider-tutorial-level-1-html-and-css-selector/>

### 安装
### 安装依赖环境和python
```shell
sudo apt-get install python python-dev python-distribute python-pip libcurl4-openssl-dev libxml2-dev libxslt1-dev python-lxml
sudo apt-get install python-pip
```

#### 安装PhantomJS
官网:<http://phantomjs.org/>
PhantomJS是一个基于 WebKit 的非界面渲染html的引擎,用于 页面自动化 ， 网络监测 ， 网页截屏 ，以及 无界面测试 等
```shell
sudo apt-get install phantomjs
```

#### 安装pyspider
```shell
pip install pyspider
```

####  错误
+ connect to scheduler rpc error: error(111, 'Connection refused')
```shell
pip install -U supernova
```


###  启动
```shell
pyspider
```

###  创建项目
打开`localhost:5000`打开`dashboard`,`create new project`
![spider](/_static/spider.png)

创建脚本:我这里以抓取一下云风的所有博文链接为例:
```python
# -*- coding: utf-8 -*-
from pyspider.libs.base_handler import *
import re
class Handler(BaseHandler):
    crawl_config = {
        "message_queue": "redis://localhost:6379/db"
    }
    #开启启动任务入口,一般为主页
    @every(minutes=24 * 60)
    def on_start(self):
        self.crawl('http://blog.codingnow.com/', callback=self.index_page)

    #根据标签过滤,页面上有个[enable css selector helper]可以快速定位
    @config(age=10 * 24 * 60 * 60)
    def index_page(self, response):
        for each in response.doc('.module-archives li > a[href^="http"]').items():
            if re.match("^http://blog.codingnow.com.*/$", each.attr.href):
                self.crawl(each.attr.href, callback=self.second_index_page)

    #我这里做了一个二级抓取,第一级为日期分类
    @config(age=10 * 24 * 60 * 60)
    def second_index_page(self, response):
        for each in response.doc('.permalink[href^="http"]').items():
            if re.match("^http://blog.codingnow.com", each.attr.href):
                self.crawl(each.attr.href, callback=self.detail_page)

     #详情页我只抓了title和url.这里也可以分析正文
    @config(priority=2)
    def detail_page(self, response):
        return {
            "url": response.url,
            "title": response.doc('title').text(),
        }

```
![spider_result.png](/_static/spider_result.png)


<http://demo.pyspider.org/>

## scrapy
`doc` : <http://doc.scrapy.org/en/0.24/>

### 安装scrapy
liux安装
```shell
pip install scrapy
```

windows上安装
[Twisted](https://pypi.python.org/packages/2.7/T/Twisted/Twisted-13.0.0.win32-py2.7.msi)
[lxml](https://pypi.python.org/packages/cb/b6/848494ec3987338fada4c33644300f43f038490bc34700ce4494d9c7baa0/lxml-3.5.0.win32-py2.7.exe#md5=3fb7a9fb71b7d0f53881291614bd323c)
```shell
#Twisted， lxml先下载编译好的包安装再安装scrapy
pip install scrapy
```



### 创建工程
`scrapy startproject codeingnow`

结构如下:
```shell
codeingnow
│  scrapy.cfg  # 项目配置文件
└─codeingnow
    │  items.py #项目items文件
    │  pipelines.py #项目管道文件
    │  settings.py #项目配置文件
    └─spiders
```
在`spiders`创建爬虫程序 `codeingnow_scrapy.py` 

```py
# -*- coding: utf-8 -*-
import scrapy
class CodeingNowSpider(scrapy.Spider):
    name = 'codeingnow'
    start_urls = ['http://blog.codingnow.com/']

    def parse(self, response):
        for url in response.css('div[class*=module-archives]  a::attr(href)').re('^http://blog.codingnow.com.*/$'):
            yield scrapy.Request(url, self.parse_content_url)
            # break

    def parse_content_url(self, response):
        for content_url in response.css('a[class*=permalink]::attr(href)').extract():
            yield {'content_url': content_url}
```


+ 直接运行spider:`scrapy runspider codeingnow_scrapy.py`
![scrapy.png](/_static/scrapy.png)

+  在工程目录中运行,并导出数据为json
`scrapy crawl codeingnow -o items.json -t json`
结果:
![scrapy.png](/_static/scrapy_json.png)