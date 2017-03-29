# Skynet 入门

`skynet`现在默认使用了自带的[lua5.3](http://cloudwu.github.io/lua53doc/)版本,对官方的原版有些定制化的地方,[这里](https://github.com/ejoy/lua/commit/eb5ae31d6fdd3e6e989b35ff1fa2954d0f520d7f)是主要修改内容.

 + [多个lua的vm中共享Proto对象](http://lua-users.org/lists/lua-l/2014-03/msg00489.html)
 +  [共享string表](http://blog.codingnow.com/2015/08/lua_vm_share_string.html)
 +  [还有一些为了debug的修改](http://blog.codingnow.com/2015/03/skynet_signal.html)

 

## 编译skynet

```shell
#clone项目
git clone https://github.com/cloudwu/skynet.git
cd skynet
#编译，平台类型linux, macosx, freebsd 
make linux
```
### 可能要的依赖库
```
autoconf
readline
ncurses
gcc>4.6.0
```

## 测试
```shell
#一个终端启动skynet服务
./skynet examples/config	
#另一个终端来用socket来连接skynet
./3rd/lua/lua examples/client.lua
```


## 目录结构

```shell
├── 3rd #第三方库如lua,jemalloc
├── Makefile
├── examples #示例代码
├── lualib # lua库
├── lualib-src # c写的导出给lua用的库
├── platform.mk #make平台环境定义
├── service # lua服务
├── service-src # c服务
├── skynet-src # skynet核心代码
└── test #测试代码
```


