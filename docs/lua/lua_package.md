lua初探
==========================

##  文档
+ <http://cloudwu.github.io/lua53doc/>



##   包管理工具
### luarocks
```
$ wget http://luarocks.org/releases/luarocks-2.3.0.tar.gz
$ tar zxpf luarocks-2.3.0.tar.gz
$ cd luarocks-2.3.0
$ ./configure; sudo make bootstrap
$ sudo luarocks install luasocket
$ lua
Lua 5.3.2 Copyright (C) 1994-2015 Lua.org, PUC-Rio
> require "socket"
```


## 加载动态链接库
```lua
--so导出函数名为:luaopen_skynet_core
require "skyet.core"
```

## 数值类型

32/64位浮点数IEEE的标准:

| s (符号位)|   Exp(指数位) |  Fraction(有效数位)|
| ---     |:-----------:| --:|
| 1bit |   8bit | 23bit |
| 1bit |   11bit | 52bit |

其中:1bit表示符号位(0表示正,1表示负).8bit表示指数(0~255),实际指数取值还要减去127,即指数取值区间为-127~128,23bit表示尾数

1. 零值：按上述的浮点表述形式如果指数部分全部为0，并且尾数全部为0，则表示为浮点0.0，并且规定-0 = +0
2. 非规格化值：如果指数全部为0，尾数非0，则表示非规格化的值，16进制看到的就是[80xxxxxx]h或者[00xxxxxx]h
3. 无穷值：如果指数全部为1，尾数全部为0，则根据符号位分别表示正无穷大和负无穷大，16进制看到的就是[FF800000]h或者[7F800000]h
4. NAN：如果指数全部为1，尾数非0，则表示这个值不是一个真正的值（Not A Number）。NAN又分成两类：QNAN（Quiet NAN）和SNAN（Singaling NAN）。QNAN与SNAN的不同之处在于，QNAN的尾数部分最高位定义为1，SNAN最高位定义为0；QNAN一般表示未定义的算术运算结果，最常见的莫过于除0运算；SNAN一般被用于标记未初始化的值，以此来捕获异常。
```
会返回 NaN 的运算有如下三种：
1. 操作数中至少有一个是 NaN 的运算
2. 未定义操作
    下列除法运算：0/0、∞/∞、∞/−∞、−∞/∞、−∞/−∞
    下列乘法运算：0×∞、0×-∞
    下列加法运算：∞ + (−∞)、(−∞) + ∞
    下列减法运算：∞ - (−∞)、(−∞) - ∞
3. 产生复数结果的实数运算。例如：
    对负数进行开方运算
    对负数进行对数运算
    对比-1小或比+1大的数进行反正弦或反余弦运算
```
5. INF / inf：这个值表示“无穷大 (infinity 的缩写)”，即超出了计算机可以表示的浮点数的最大范围（或者说超过了 double 类型的最大值）。例如，当用 0 除一个整数时便会得到一个1.#INF / inf值；相应的，如果用 0 除一个负整数也会得到 -1.#INF / -inf值。
6. IND / nan：这个的情况更复杂，一般来说，它们来自于任何未定义结果（非法）的浮点数运算。"IND"是 indeterminate 的缩写，而"nan"是 not a number 的缩写。产生这个值的常见例子有：对负数开平方，对负数取对数，0.0/0.0，0.0*∞, ∞/∞ 等。


```lua
local function isnan(x) return x ~= x end

print(isnan(0/0))  --True
print(isnan(math.huge))  --False
print( isnan(0)) --False
```


### 多字段排序
```lua
local a = {{id=1,lv=1,point_type=1},{id=1,lv=3,point_type=1},{id=1,lv=2,point_type=1},{id=1,lv=2,point_type=2}}
table.sort( a, function (a,b )
    if a.point_type==b.point_type then
        return a.lv>b.lv
    else
        return a.point_type>b.point_type
    end
end )
```