c++ 第四章  编译过程
==========================

## 一 **语言类型概念**

+ `解释型语言`:在运行期逐行解释源码为机器码,如javascript.
+ `编译型语言`:在编译期一次性将源码转换成机器码的语言，平台依赖性较高。如c,c++.

像java,actionscript。这种则属于混合型的语言了，java编译成的并非机器码，由jvm去解释所生成的java字节码，而actioncript也有avm类似的解释器。

+ `动态语言`:在运行期才去检测数据类型的语言。像javascript.
+ `静态语言`:在编译期检查数据类型,如c,c++等.



## 二 **c++编译过程**

![image](http://farwmarth.bestnewbee.com/images/uploads/2013/08/image11.png)

+ `预处理`: 对预处理指令作一些替换操作,处理#include,#define,#if等，还有注释的删除工作.
+ `编译`:将预处理文件进行进行一系列**词法分析**（[lex](http://en.wikipedia.org/wiki/Lex_(software))）、**语法分析**（[yacc](http://en.wikipedia.org/wiki/Yacc)）、**语义分析**及**优化**后生成汇编代码
+ `汇编`:汇编代码到目标文件,**目标文件**以机器码的形式包含了**编译单元**(每一个cpp文件就是一个编译单元)里所有的代码和数据，以及一些其他的信息.
目标文件除了自己数据和二进制外还提供三个表:

    + **未解决符号表**:被编译单元引用却不定义在其中的符号和地址.
    + **导出符号表**: 定义在些编译单元并提供给其他编译单元的符号和地址.
    + **地址重定向表**:本编译单元对自身引用的记录.

+ `链接`:将多个目标文件,库，链接为一个可执行文件

![image](http://farwmarth.bestnewbee.com/images/uploads/2013/08/image12.png)

 链接器进行链接时，先确定各个编译单元的位置，然后遍历编译单元的未解决符号表，并且在所有导出符号表时查找匹配的符号并填上地址。最后将目标文件的内容写在各自的位置上。一个可执行文件就链接完成了。



## 三 **java编译过程与c++的区别**

c++编译代码时，依据特定平台将未解决符表转换为特定的的内存偏移量，平台依赖性较强.

java编译器在编译器却不确定内存布局。而是在运行期，在代码通用classLoader载入完成，校验完代码后才确定了内存布局.在运行期通过查表的方式来确定一个方法所在的地址。这才是java一次编译，到处运行的原因.



[http://www.cnblogs.com/hongfenglee/archive/2012/02/18/2356808.html](http://www.cnblogs.com/hongfenglee/archive/2012/02/18/2356808.html)
[http://www.xue5.com/Developer/C++/738463_2.html](http://www.xue5.com/Developer/C++/738463_2.html)
[http://blog.163.com/zhaoxin851055@126/blog/static/81129298200987112318323/](http://blog.163.com/zhaoxin851055@126/blog/static/81129298200987112318323/)
[http://developer.51cto.com/art/201009/226884.htm](http://developer.51cto.com/art/201009/226884.htm)