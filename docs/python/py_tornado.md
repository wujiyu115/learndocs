python web框架 tornado 初探
==========================

## Tornado
tornado 是一个基于python的非阻塞式 web 服务器

###  Module Map
+ Web framework
    * tornado.web — RequestHandler and Application 类
    * tornado.template —  可扩展输出模板
    * tornado.escape — 转义和字符串操作
    * tornado.locale — 国际化支持
    * tornado.websocket —  利用websocket双向通信

+ HTTP servers and clients
    - tornado.httpserver — 非阻塞http服务器
    - tornado.httpclient —  异步http请求
    - tornado.httputil — 处理HTTP标头和URL
    - tornado.http1connection – HTTP/1.x 客户端/服务器实现

+ Asynchronous networking
    * tornado.ioloop —  事件驱动主循环
    * tornado.iostream — 非阻塞socket包装类
    * tornado.netutil — 其他网络工具
    * tornado.tcpclient —基础的tcpclient
    * tornado.tcpserver — 基础的tcpserver

+ Coroutines and concurrency
    + tornado.gen — 简化异步代码的模块
    + tornado.concurrent —  同步相关的threads 和 futures
    + tornado.locks – 同步原语
    + tornado.queues – 协程队列
    + tornado.process — 多处理器工具类

+ Integration with other services
    - tornado.auth —  登录验证
    - tornado.wsgi —  适配其他python,web框架
    - tornado.platform.asyncio — 和python3.4的异步IO库asyncio做适配
    - tornado.platform.caresresolver — 异步DNS解析,C-Ares
    - tornado.platform.twisted — 适配Twisted框架

+ Utilities
    + tornado.autoreload — Automatically detect code changes in development
    + tornado.log —  日志
    + tornado.options — 命令行选项
    + tornado.stack_context — 异步回调异常处理
    + tornado.testing — 异步代码的测试用例支持
    + tornado.util — 常规工具类


### hello world
```py
import tornado.ioloop
import tornado.web
class MainHandler(tornado.web.RequestHandler):
  def get(self):
    self.write("Hello, world,"+subdomain)

def make_app():
  return tornado.web.Application([
    (r"/", MainHandler),
  ])

if __name__ == "__main__":
  app = make_app()
  app.listen(8888)
  tornado.ioloop.IOLoop.current().start()

  #在浏览器中输入`http://localhost:8888/` 可以看到`Hello,world`的输出
```



###  handler
####  StaticFileHandler
```py
def make_app():
  return tornado.web.Application([
    (r"/static/(.*)", tornado.web.StaticFileHandler, {"path": "C:\\Users\\Administrator.PC-20150720QKHJ\\Desktop\\static"}),
  ])

  # 输入: http://localhost:8888/static/a.txt 可访问static目录下的文件内容
```

#### get_argument 获取参数
```py
class MainHandler(tornado.web.RequestHandler):
    def get(self):
        self.write('<html><body><form action="/" method="post">'
                   '<input type="text" name="message">'
                   '<input type="submit" value="Submit">'
                   '</form></body></html>')

    def post(self):
        self.set_header("Content-Type", "text/plain")
        self.write("You wrote " + self.get_argument("message"))

```

+ arguments - 所有的 GET 或 POST 的参数
+ files - 所有通过 multipart/form-data POST 请求上传的文件
+ path - 请求的路径（ ? 之前的所有内容）
+ headers - 请求的开头信息

### coroutine
```py
import tornado.ioloop
import tornado.web
from tornado import gen
from tornado.httpclient import AsyncHTTPClient

class MainHandler(tornado.web.RequestHandler):
    @gen.coroutine
    def get(self):
        response = yield self.fetch_json("http://www.baidu.com")
        self.write(response)

    @gen.coroutine
    def fetch_json(self, url):
        response = yield AsyncHTTPClient().fetch(url)
        raise gen.Return(response.body)

application = tornado.web.Application([
    (r"/", MainHandler),
])

if __name__ == "__main__":
    application.listen(8888)
    tornado.ioloop.IOLoop.current().start()
```

###  tornado   泛域名
```py
import tornado.ioloop
import tornado.web
class MainHandler(tornado.web.RequestHandler):
  def get(self):
    subdomain = self.request.host.split(".")[0]
    self.write("Hello, world,"+subdomain)

class DomainHandler(tornado.web.RequestHandler):
  def get(self):
    self.write("Hello, a.com")

def make_app():
  return tornado.web.Application([
    (r"/", MainHandler),
  ])

if __name__ == "__main__":
  app = make_app()
  app.add_handlers(r"^(www\.)?a\.com$", [
    (r"/", DomainHandler),
  ])
  app.listen(8888)
  tornado.ioloop.IOLoop.current().start()

  # 在Host中加入这行 127.0.0.1 a.com
  #浏览器中输入:http://a.com:8888  则输出:Hello, a.com
```

###  tornado ngnix 反向代理配置
如果在tornado层不想做泛域名解析,可以通过ngnix的反向代理指向tornado的端口,ngnix配置如下:
```
upstream tornado {
    server 127.0.0.1:8888;
}
server {
      listen 80;
      server_name test.com www.test.com;
      location = /favicon.ico {
          rewrite (.*) /static/favicon.ico;
      }
      location = /robots.txt {
          rewrite (.*) /static/robots.txt;
      }
      location / {
          proxy_pass_header Server;
          proxy_set_header Host $http_host;
          proxy_redirect off;
          proxy _set_header X-Real-IP $remote_addr;
          proxy_set_header X-Scheme $scheme;
          proxy_pass http://tornado;
      }
}
```


### nginx反向代理长链接
```
upstream chat_cluster{
     ##多server负载,根据ip_hash作分流也可以用weight权重分流
    server 127.0.0.1:10000;
    server 127.0.0.1:10001;
    ip_hash;
    keepalive 1024;
}

server
{
    listen 80;
    server_name chat.rootk.com;

    location / {
        proxy_pass http://chat_cluster;
        proxy_http_version 1.1;
        # very important, nginx will waitting for the response from tornado
        # if the time have passed more than 7200, nginx send http 504 to client
        proxy_read_timeout 7200;
        proxy_set_header Connection "";
        proxy_set_header Host $host;
        proxy_set_header X-Real-Ip $remote_addr;
        proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
    }
}
```