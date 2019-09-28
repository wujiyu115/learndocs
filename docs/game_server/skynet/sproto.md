# sproto

通讯协议是一个描述业务数据结构的约定，主流的格式有

+ json
+ 二进制(节约流量)
+ protobuf

[sproto](https://github.com/cloudwu/sproto)是云风在`protobuf`上的精简的通讯协议版本.

`sproto`除了有基础的结构定义,压缩功能之外还提供了远程`rpc`和多个`lua vm`共享`sproto`的实现

从benchmark来看sproto在压缩大小和速度上有不错的表现


# 结构定义
sproto的结构很好理解

```
.Person {
    name 0 : string #字符串
    id 1 : integer #整数
    adult 2 : boolean #布尔
    phone 3 : *integer #数组
    money 4 : integer(2) #浮点
}

.AddressBook {
    person 0 : *Person(id) #map
}
```

# 类型
+ string : 字符串
+ binary : 二进制
+ integer : 数字类型,可以用integer(2)来表示浮点类型
+ boolean : 布尔值
+ * :表示数组

# lua api
```
local sproto = require "sproto"
local sprotocore = require "sproto.core" -- optional
```

+ sproto.new(spbin) 用二进制字符串创建一个sproto的lua对象(不是定义的结构字符串,是parser出来的)
+ sprotocore.newproto(spbin) 用二进制字符串创建一个sproto的c对象
+ sproto.sharenew(spbin) 多个vm共享sproto的c指针
+ sproto.parse(schema) 用定义的结构字符创建sproto的lua对象
+ sproto:exist_type(typename) 协议名是否定义
+ sproto:encode(typename, luatable) 协议编码
+ sproto:decode(typename, blob [,sz]) 解码,如果blob为c指针则要指定字符长度
+ sproto:pencode(typename, luatable) 协议压缩编码
+ sproto:pdecode(typename, blob [,sz]) 协议压缩解码
+ sproto:default(typename, type) 创建一个带默认值的table,数组默认值为空数组,integer为0,boolean为false

## 测试代码

```lua
local sproto = require "sproto"
local core = require "sproto.core"
#用工程里的print_r
local print_r = require "print_r"

local sp = sproto.parse [[
.Person {
	name 0 : string
	id 1 : integer
	email 2 : string
}
.AddressBook {
	person 0 : *Person(id)
}
]]

local def = sp:default "Person"
print("default table for Person")
print_r(def)
print("--------------")

local ab = {
	person = {
		[10000] = {
			name = "Alice",
			id = 10000,
		}
	},
}

collectgarbage "stop"

local code = sp:encode("AddressBook", ab)
local addr = sp:decode("AddressBook", code)
print_r(addr)

```

# Rpc Api
+ sproto:host([packagename]) 创建主机端对象.
+ host:dispatch(blob [,sz]) unpack 用sproto二进制字符串 unpack and decode数据  .session, message, .ud .
+ host:attach(sprotoobj) creates a function(protoname, message, session, ud) to pack and encode request message with sprotoobj.

+ sproto:request_encode(protoname, tbl) encode a request message with protoname.
+ sproto:response_encode(protoname, tbl) encode a response message with protoname.
+ sproto:request_decode(protoname, blob [,sz]) decode a request message with protoname.
+ sproto:response_decode(protoname, blob [,sz] decode a response message with protoname.

