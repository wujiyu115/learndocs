swift基础入门
===================

## 环境
PlayGround: Swift 的 playground 就像是一个可交互的文档，它是用来练手学swift,可实时查看代码效果,Xcode=>File => New => Playground.

## 基础语法
### 注释
```
//这是一行注释

/* 这也是一条注释，
但跨越多行 */

/* 这是第一个多行注释的开头

/* 这是嵌套的第二个多行注释 */

这是第一个多行注释的结尾 */
```

### 分号
与其它语言不同的是,Swift不要求在每行语句的结尾使用分号(;),但当你在同一行书写多条语句时,必须用分号隔开：
```
var myString = "Hello, World!"; print(myString)
```

### Swift缩进
Swift对空格的使用有一定的要求，但是又不像Python对缩进的要求那么严格。
在Swift中，运算符不能直接跟在变量或常量的后面。例如下面的代码会报错：
```
let a= 1 + 2 //error:prefix/postfix '=' is reserved 运算符不能直接跟在变量或常量的后
正确写法:
let a = 1 + 2

let c = a|b  //error:'|' is not a  postfix unary operator 操作符两边需要空格
正确写法:
let c = a | b
```

### 标识符
+ 区分大小写，Myname与myname是两个不同的标识符；
+ 标识符首字符可以以下划线（_）或者字母开始，但不能是数字；
+ 标识符中其他字符可以是下划线（_）、字母或数字。

### 数据类型
Int,UInt,Double,Float,Boolean,String

### 变量定义
```
let a = 1  //常量
var b:int =  1 //变量
typealias myint = int //别名，有点像宏定义
```

### 类型推断
非显示变量定义时swift会处行推断你的数据类型，当推断浮点数的类型时，Swift总是会选择Double而不是Float。


## Swift 可选(Optionals)类型
给无默认值的包装一个ni默认值
```
var optionalInteger: Int?
等价于
var optionalInteger: Optional<Int>
```
感叹号（!）强制解析
```
var myString:String?
myString = "Hello, Swift!"
if myString != nil {
   // 强制解析
   print( myString! )
}else{
   print("myString 值为 nil")
}
```
自动解析(!)
```
var myString:String!
myString = "Hello, Swift!"
if myString != nil {
   print(myString)
}else{
   print("myString 值为 nil")
}
```
可选绑定
使用可选绑定（optional binding）来判断可选类型是否包含值，如果包含就把值赋给一个临时常量或者变量。可选绑定可以用在if和while语句中来对可选类型的值进行判断并把值赋给一个常量或者变量。
```
var myString:String?
myString = "Hello, Swift!"
if let yourString = myString {
   print("你的字符串值为 - \(yourString)")
}else{
   print("你的字符串没有值")
}
```

## 控制符
```
func a() -> Bool
{
    print("a")
    return true
}

func b() -> Bool
{
    print("b")
    return false
}
if (a() || b())
{
    print("c")
}
```

## error
