C++ 第三章 内存分配方式及指针
==========================

##  一  c++内存分配方式
在c++中，内存分配为5个区,分别为栈,堆,自由存储区,静态数据区,常量存储区. **栈**，存储单元自动被释放,里面的变量通常是局部变量、函数参数等。

+ **堆**，就是那些由 `new`分配的内存块，他们的释放编译器不去管，由我们的应用程序去控制，一般一个`new`就要对应一个 `delete`。如果程序员没有释放掉，那么在程序结束后，操作系统会自动回收。堆可以动态地扩展和收缩。
+ **自由存储区**，就是那些由`malloc`等分配的内存块，他和堆是十分相似的，不过它是用`free` 来结束自己的生命的
+ **全局/静态存储区**，全局变量和静态变量被分配到同一块内存中，在以前的 C 语言中，全局变量又分为初始化的和未初始化的（初始化的全局变量和静态变量在一块区域，未初始化的全局变量与静态变量在相邻的另一块区域，同时未被初始化的对象存储区可以通过 `void*`来访问和操纵，程序结束后由系统自行释放），在 C++ 里面没有这个区分了，他们共同占用同一块内存区。
+ **常量存储区**，这是一块比较特殊的存储区，他们里面存放的是常量，不允许修改（当然，你要通过非正当手段也可以修改，而且方法很多）

##  二 堆和栈究竟有什么区别？
主要的区别由以下几点:

+ 管理方式不同
+ 空间大小不同
+ 能否产生碎片不同
+ 生长方向不同
+ 分配方式不同
+ 分配效率不同

1. **管理方式**：
对于栈来讲，是由编译器自动管理，无需我们手工控制；对于堆来说，释放工作由程序员控制，容易产生内存泄露.
2. **空间大小**：堆内存空间大小基本无限制，栈的申请内存在1M左右
3. **碎片问题**：堆上的内存分配是不连续的，每次`new`和`delete`会导致空间的不连续，而栈结构是先进后出不会有这种结果。
4. **生长方向**：对于堆来讲，生长方向是向上的，也就是向着内存地址增加的方向；对于栈来讲，它的生长方向是向下的，是向着内存地址减小的方向增长。
5. **分配方式**：堆都是动态分配的，没有静态分配的堆。栈有2种分配方式：静态分配和动态分配。静态分配是编译器完成的，比如局部变量的分配。动态分配由 `malloc` 函数进行分配，但是栈的动态分配和堆是不同的，他的动态分配是由编译器进行释放，无需我们手工实现。
6.  **分配效率**：栈是机器系统提供的数据结构，计算机会在底层对栈提供支持：分配专门的寄存器存放栈的地址，压栈出栈都有专门的指令执行，这就决定了栈的效率比较高。堆则是 C/C++ 函数库提供的，它的机制是很复杂的，例如为了分配一块内存，库函数会按照一定的算法（具体的算法可以参考数据结构/操作系统）在堆内存中搜索可用的足够大小的空间，如果没有足够大小的空间（可能是由于内存碎片太多），就有可能调用系统功能去增加程序数据段的内存空间，这样就有机会分到足够大小的内存，然后进行返回。显然，堆的效率比栈要低得多。

## 三  指针 引用 值
**定义**

+ 指针定义: `int * p;`
+ 定义变量: `int p; `
+ 定义引用:
```c++
int p=p;int a=6; int b; //指针
b=&a; //地址传递
cout<<&a<<endl; //地址0034FBBC
cout<<a<<endl; //值 6
cout<<b<<endl; //地址0034FBBC
cout<<b<<endl;//值 6
cout<<(b==&a)<<endl; //true 地址相同
b=7; //更改指针指向的空间的值
cout<<a<<endl; //值 7
cout<<b<<endl;//值 7
int &c= a; //定义引用
cout<<c<<endl;//值7
cout<<&c<<endl; //地址0034FBBC
int d=0; b=&d;
cout<<*b<<endl;//值0
cout<<b<<endl; //地址0038FBC0
```

** 引用与指针区别**

+ 引用必须在定义时初始化，指针可以先声明后初始化。
+ 引用不能有NULL引用，指针可以为NULL
+ 引用一定初始化不能再指向其他对象，而指针可以改变所指对象。
+ 引用不占内存，只是变量的一个别名，指针会在栈区开辟一个4byte的内存区。



图示指针内存分布:

[![clip_image002](http://farwmarth.com/wp-content/uploads/2013/08/clip_image002_thumb.gif "clip_image002")](http://farwmarth.com/wp-content/uploads/2013/08/clip_image002.gif)

c++ 传值，传址，传引用
```c++
void swap(int a,int b);
void swap2(int &amp;x,int &amp;y);
void swap3(int*  a,int* b);
int _tmain(int argc, _TCHAR* argv[])
{

	int a=1,b=2;
	swap(a,b); //传值 ,参数为值拷贝,外部变量不会改变值
	cout<<a<<endl; //1
	cout<<b<<endl;//2
	int &amp;m= a;
	int  &amp;n = b;
	swap2(m,n); //传引用,值改变
	cout<<a<<endl; //2
	cout<<b<<endl; //1
	int *k= &amp;a;
	int  *j = &amp;b;
	swap3(k,j);//传指针,值改变
	cout<<a<<endl;//1
	cout<<b<<endl;//2

	return 0;
}

void swap(int a,int b){
	int temp = a;
	a = b;
	b=a;
}
void swap2(int &amp;x,int &amp;y)
{
	int temp;
	temp=x;
	x=y;
	y=temp;
}
void swap3(int*  a,int* b){
	int temp = *a;
	*a = *b;
	*b = temp;
}
```

java的传值，传引用
```java
public class Test {

	/**
	 * @param ss
	 */
	public static void main(String[] ss) {
		int a = 1;
		int b = 2;
		change(a, b);
		System.out.println(a);//1
		System.out.println(b);//2

		String m = new String("m");
		String n = new String("n");
		change2(m, n);
		System.out.println(m);//m
		System.out.println(n);//n
	}

	private static void change2(String pa1, String pa2) {
		String temp = pa1;
		pa1= pa2;
		pa2= temp;
	}

	private static void change(int a, int b) {
		int temp = a;
		a = b;
		b = temp;
	}
}
```

java的传值传引用都不会改变,交换两个引用参数不变原因如下:

[![image](http://farwmarth.com/wp-content/uploads/2013/08/image_thumb10.png "image")](http://farwmarth.com/wp-content/uploads/2013/08/image10.png)

m,n为外部变量new之后在堆区产生两个内存区。chang2方法所做的是复制引用，将复制的引用指向对象改变了，外部变量m,n却没有发生指向改变.

## 四 c++常见内存错误

+  内存分配未成功，却使用了它 ,解决方法是使用内存之前是检测一下指针是否为NULL
```c++
t = (struct btree *)malloc(sizeof(struct btree));
if (t == NULL) {
	printf("内存分配失败!\n");
	exit(EXIT_FAILURE);
}
```
+ 没有初始化的概念
+ 误认为内存的缺省初值全为0,导致引用初值错误 ,java的会基本数据类型成员发生会初始化对应的值。
+ 忘记释放内存，导致内存泄漏
+ 含有这种错误的函数每被调用一次就丢失一块内存。刚开始的时候，系统内存充足，你看不到错误。终有一次程序突然死掉，系统出现提示:内存耗尽
 动态内存的申请与释放必须配对，程序中malloc和free的使用次数一定要相同，否则肯定有错误
+ 释放了内存却继续使用它
 程序中的对象调用关系过于复杂，实在难以搞清楚某个对象究竟是否已经释放了内存，此时应该重新设计数据结构，从根本上解决对象管理的混乱局面
+ 函数的reture语句写错了，注意不要返回指向"栈内存"的“指针”或者“引用”，因为该内存在函数体结束时被自动销毁
```c++
// ConsoleApplication2.cpp : 定义控制台应用程序的入口点。
//

#include "stdafx.h"
#include "vld.h"
#include "Student.h"
#include <iostream>
using namespace std;

Student* func3();

int _tmain(int argc, _TCHAR* argv[])
{
	Student*  dss = func3();
	cout<<(dss==NULL)<<endl;  delete dss;//指针ptr指向了一个可能已经无效的地址，危险
	return 0;
}

Student* func3()
{
	Student my;        //对象my存放在桟上，退出函数体以后改地址就可能无效了。  Student* ptr = &amp;my;   //指针ptr指向了一个可能已经无效的地址，危险。  //cout<<ptr<<endl;
	return ptr;
}
```
+  使用`free`或`delete`释放了内存后，没有将指针设置为NULL。导致产生“野指针”
规则
+ 用`malloc`或`new`申请内存之后，应该立即检查指针是否为NULL。防止使用指针为NULL的内存
+ 不要忘记为数组和动态内存赋初值。防止将未被初始化的内存作为右值使用
+ 避免数组或者指针的下标越界，特别要当心发生“多1”或者“少1”的操作
+ 动态内存的申请与释放必须配对，防止内存泄漏
+ 用`free`或`delete`释放内存之后，立即将指针设置为NULL，防止产生“野指针”