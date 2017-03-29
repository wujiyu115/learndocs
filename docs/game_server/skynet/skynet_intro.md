# skynet简介
skynet是云风开源的游戏服务器框架。要了解skynet可以先从博客和github的代码example入手

+ https://github.com/cloudwu/skynet/
+ https://github.com/cloudwu/skynet/wiki
+ http://blog.codingnow.com/

## 这个框架有什么特点呢?

+ 单进程多进程模型
+ 核心框架逻辑封装在`c`层,嵌入了`lua`支持
+ 服务概念,即单个提供逻辑支持的`lua`虚拟机隔离环境,各服务通过消息队列的方式通讯
+ 引入协程实现同步编程
+ 网络部分实现了封装了`epoll`等io模型.
+ 两种集群模式
+ 类`google protobuffers`的`sproto`通讯协议
+ 数据库`redis`,`mongo`,`mysql`等扩展支持

