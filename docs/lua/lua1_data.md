基本概念
==================================

## 文档
+ `lua5.1` :<http://www.codingnow.com/2000/download/lua_manual.html>
+ `lua5.3` :<http://cloudwu.github.io/lua53doc/>

## 值与类型
Lua 中有八种基本类型： `nil`、`boolean`、`number`、`string`、`function`、`userdata`、 `thread` 和 `table`
userdata 类型允许将 C 中的数据保存在 Lua 变量中

## 环境与全局环境
###  `upvalue`
当执行完g1 = f1(1979)后，局部变量n的生命本该结束，但因为它已经成了内嵌函数f2(它又被赋给了变量g1)的upvalue
```lua
local function f1(n)
    -- 函数参数也是局部变量
    local function f2()
        print(n) -- 引用外包函数的局部变量
    end
    return f2
end
g1 = f1(1979)
```

### `_ENV`
lua5.2中load代码后返回函数的upvalue.原先虚无缥缈只能通过 setfenv、getfenv 访问的所谓「环境」终于实体化为一个始终存在的变量 _ENV 了
<http://timothyqiu.com/archives/lua-note-sandboxes/>

lua_setglobal/lua_getglobal都是操作lua_State注册表中LUA_RIDX_GLOBALS伪索引指向的全局变量表，与lua中访问_ENV['a']或者a是不同的。
lua_load加载lua代码后会返回一个函数，默认会给这个函数设置一个upvalue就叫_ENV，起值是LUA_RIDX_GLOBALS的全局变量表，你可以lua_setupvalue设置这个函数的upvalue，即下标1的upvalue，因为这个位置是这个函数的_ENV表存放位置（你可以通过lua_setupvalue的返回值印证这一点）

### `_G`
全局环境.
lua_State会在创建时保证LUA_RIDX_GLOBALS的全局变量表中包含一个指向自己的_G元素，这样就保证了在不调用lua_setupvalue的情况下该返回函数的_ENV['_G']是指向自己的，即LUA_RIDX_GLOBALS这个全局表。（其实你的lua解释器就是简单的lua_load后pcall的，对于一个刚启动lua_State来说是没有_ENV的，是lua解释器load你的代码时自动给带上的_ENV，其值是lua_state的LUA_RIDX_GLOBALS全局表。）
lua_state启动后在注册表里LUA_RIDX_GLOBALS下标存放的全局表一定有一个元素是指向自己的，即_G.


## 错误处理
###  抛出异常
`error (message [, level]) :`

### 捕获
#### pcall
`pcall (f [, arg1, ···])`
以保护模式 调用函数 f ,返回值`[flag,errorinfo]`,运行正常是`flag`为`true`
```lua
local  function er( ... )
    error("error")
end
local flag,errorinfo = pcall(er)
print(flag,errorinfo)
--false   ...:\lua_error.lua:7: error
```

#### xpcall
`xpcall (f, msgh [, arg1, ···])`
与`pcall`区别:自定义消息处理,不返回`errorinfo`
```lua
local flag = xpcall(function() error('error..') end, function() print(debug.traceback()) end, 33)
print(flag)
--[[
stack traceback:
    ...:\git\code_research\lua_study\lua_code\lua_error.lua:12: in function <...:\git\code_research\lua_study\lua_code\lua_error.lua:12>
    [C]: in function 'error'
    ...:\git\code_research\lua_study\lua_code\lua_error.lua:12: in function <...:\git\code_research\lua_study\lua_code\lua_error.lua:12>
    [C]: in function 'xpcall'
    ...:\git\code_research\lua_study\lua_code\lua_error.lua:12: in main chunk
    [C]: ?

false
]]--

```

### debug
+ `debug.traceback` :输出堆栈信息


## 元表及元方法
### 元表(metatable)
与js原型链(prototype)类似

+ "add": + 操作。 如果任何不是数字的值（包括不能转换为数字的字符串）做加法， Lua 就会尝试调用元方法。 首先、Lua 检查第一个操作数（即使它是合法的）， 如果这个操作数没有为 "__add" 事件定义元方法， Lua 就会接着检查第二个操作数。 一旦 Lua 找到了元方法， 它将把两个操作数作为参数传入元方法， 元方法的结果（调整为单个值）作为这个操作的结果。 如果找不到元方法，将抛出一个错误。
```lua
local met = {}
met.__add = function(op1, op2)
   for _, item in pairs(op2) do
      table.insert(op1, item)
   end
   return op1
end

local a = {1,2}
local b={3,4}
setmetatable(a,met)
local d = a +b

for k,v in pairs(d) do
    print(k,v)
end
--[[
1   1
2   2
3   3
4   4
]]
```

+ "concat": .. （连接）操作。 行为和 "add" 操作类似， 不同的是 Lua 在任何操作数即不是一个字符串 也不是数字（数字总能转换为对应的字符串）的情况下尝试元方法。

+ "len": # （取长度）操作。 如果对象不是字符串，Lua 会尝试它的元方法。 如果有元方法，则调用它并将对象以参数形式传入， 而返回值（被调整为单个）则作为结果。 如果对象是一张表且没有元方法， Lua 使用表的取长度操作（参见 §3.4.7）。 其它情况，均抛出错误。

+ "index": 索引 table[key]。 当 table 不是表或是表 table 中不存在 key 这个键时，这个事件被触发。 此时，会读出 table 相应的元方法。
尽管名字取成这样， 这个事件的元方法其实可以是一个函数也可以是一张表。 如果它是一个函数，则以 table 和 key 作为参数调用它。 如果它是一张表，最终的结果就是以 key 取索引这张表的结果。 （这个索引过程是走常规的流程，而不是直接索引， 所以这次索引有可能引发另一次元方法。）

+ "newindex": 索引赋值 table[key] = value 。 和索引事件类似，它发生在 table 不是表或是表 table 中不存在 key 这个键的时候。 此时，会读出 table 相应的元方法。
同索引过程那样， 这个事件的元方法即可以是函数，也可以是一张表。 如果是一个函数， 则以 table、 key、以及 value 为参数传入。 如果是一张表， Lua 对这张表做索引赋值操作。 （这个索引过程是走常规的流程，而不是直接索引赋值， 所以这次索引赋值有可能引发另一次元方法。）

一旦有了 "newindex" 元方法， Lua 就不再做最初的赋值操作。 （如果有必要，在元方法内部可以调用 rawset 来做赋值。）

+ "call": 函数调用操作 func(args)。 当 Lua 尝试调用一个非函数的值的时候会触发这个事件 （即 func 不是一个函数）。 查找 func 的元方法， 如果找得到，就调用这个元方法， func 作为第一个参数传入，原来调用的参数（args）后依次排在后面

## 垃圾收集
Lua 实现了一个增量标记-扫描收集器。 它使用这两个数字来控制垃圾收集循环： 垃圾收集器间歇率 和 垃圾收集器步进倍率。 这两个数字都使用百分数为单位 （例如：值 100 在内部表示 1 ）。

垃圾收集器间歇率控制着收集器需要在开启新的循环前要等待多久。 增大这个值会减少收集器的积极性。 当这个值比 100 小的时候，收集器在开启新的循环前不会有等待。 设置这个值为 200 就会让收集器等到总内存使用量达到 之前的两倍时才开始新的循环。

垃圾收集器步进倍率控制着收集器运作速度相对于内存分配速度的倍率。 增大这个值不仅会让收集器更加积极，还会增加每个增量步骤的长度。 不要把这个值设得小于 100 ， 那样的话收集器就工作的太慢了以至于永远都干不完一个循环。 默认值是 200 ，这表示收集器以内存分配的“两倍”速工作。

如果你把步进倍率设为一个非常大的数字 （比你的程序可能用到的字节数还大 10% ）， 收集器的行为就像一个 stop-the-world 收集器。 接着你若把间歇率设为 200 ， 收集器的行为就和过去的 Lua 版本一样了： 每次 Lua 使用的内存翻倍时，就做一次完整的收集。

### 垃圾收集的元方法
使用 C API,可以设置一个垃圾收集的元方法,`__gc `,来做一些特殊的资源释放操作,比如关闭数据库连接等,一个 userdata 可被回收，若它的 metatable 中有 __gc 这个域　， 垃圾收集器就不立即收回它。 取而代之的是，Lua 把它们放到一个列表中。 最收集结束后，Lua 针对列表中的每个 userdata 执行了下面这个函数的等价操作：
```lua
   function gc_event (udata)
       local h = metatable(udata).__gc
       if h then
         h(udata)
       end
     end
```
在每个垃圾收集周期的结尾，每个在当前周期被收集起来的 userdata 的结束子会以 它们构造时的逆序依次调用。 也就是说，收集列表中，最后一个在程序中被创建的 userdata 的 结束子会被第一个调用。
参考:
<http://blog.codingnow.com/cloud/LuaReferObject>

### 弱表
如果一个对象只被弱引用引用到， 垃圾收集器就会回收这个对象
一张表的元表中的 __mode 域控制着这张表的弱属性。 当 __mode 域是一个包含字符 'k' 的字符串时，这张表的所有键皆为弱引用。 当 __mode 域是一个包含字符 'v' 的字符串时，这张表的所有值皆为弱引用

值为弱表
```lua
local weakTable = {}
weakTable[1] = function() print("i am the first element") end
weakTable[2] = function() print("i am the second element") end
weakTable[3] = {10, 20, 30}

setmetatable(weakTable, {__mode = "v"})  --值为弱表
print(table.getn(weakTable))  --未gc前3个

ele = weakTable[1]  --第一个元素被引用
collectgarbage()
print(table.getn(weakTable))  --gc后只剩第一个元素

ele= nil   --引用置空
collectgarbage()
print(table.getn(weakTable))  --全部gc

```
键为弱表
```lua
local function getkeyn( t )
    local count = 0
    for k,v in pairs(t or {}) do
     count= count+1
   end
return count
end

local weakKey= {}
local t1={}
local t2={}
weakKey[t1] = 1
weakKey[t2] = 2

setmetatable(weakKey,{__mode="k"}) --键为弱表
print(getkeyn(weakKey)) --2

t1= nil
collectgarbage()
print(getkeyn(weakKey)) --1
```

## 协程
支持协程，也叫 协同式多线程。 一个协程在 Lua 中代表了一段独立的执行线程。 然而，与多线程系统中的线程的区别在于， 协程仅在显式调用一个让出（yield）函数时才挂起当前的执行.

### 接口
#### coroutine.create(f)
函数参数[接受单个参数，这个参数为coroutine的主函数]
函数返回值[返回一个thread对象]
函数作用[创建一个新的协程，协程的主函数定义了该协程内的任务流程]


#### coroutine.resume(co, [, var1, ...])
函数参数[第一个参数：coroutine.create的返回值，即一个thread对象]
函数参数[第二个参数：coroutine中执行需要的参数，是一个变长参数，可以传任意多个]
函数返回值[如果程序没有任何运行错误，则返回true，之后的返回值是前一个调用coroutine.yield中传入的参数值]
函数返回值[如果程序遇到运行错误，则返回false，加上错误信息]
函数作用[当你第一次调用coroutine.rusume方法时，coroutine从主函数的第一行开始执行，之后在coroutine开始执行后，它会一直运行到
自身终止或者是coroutine的下一次yield函数]

#### coroutine.yield(...)
函数参数[变长参数]
函数返回值[如果程序没有任何运行错误，则返回true，之后的返回值是前一个调用coroutine.resume()中传入的参数值]
函数返回值[如果程序遇到运行错误，则返回false，加上错误信息]
函数作用[挂起当前执行的协程]

#### coroutine.running()
函数参数[空]
函数返回值[返回当前协程，如果它被主线程调用的话，返回nil]

#### coroutine.status()
函数参数[空]
函数返回值[返回当前协程的状态，有suspended, running, normal, dead]

### 例子

``` lua
function foo (a)
       print("[step2] foo", a)
       return coroutine.yield(2*a)
 end

local  co = coroutine.create(function (a,b)
       print("[step1] co-body", a, b)
       local r = foo(a+1)
       print("[step4] co-body", r)
       local r, s = coroutine.yield(a+b, a-b)
       print("[step6] co-body", r, s)
       return b, "end"
 end)

 print("[step3] main", coroutine.resume(co, 1, 10))
 print("[step5] main", coroutine.resume(co, "r"))
 print("[step7] main", coroutine.resume(co, "x", "y"))
 print("[step8] main", coroutine.resume(co, "x", "y"))

--[[
[step1] co-body 1   10
[step2] foo 2
[step3] main    true    4
[step4] co-body r
[step5] main    true    11  -9
[step6] co-body x   y
[step7] main    true    10  end
[step8] main    false   cannot resume dead coroutine
]]--
```