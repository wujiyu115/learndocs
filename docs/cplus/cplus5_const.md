c++ 第五章 const
==========================

## 1 修饰普通变量和指针
``` c++
double d1 = 2;
double d2 = 4;
const double cptr=&d1;
cptr =&d2; //不能修改指针指向的对象值，而可以更改指针指向的对象地址
double const cptr1 = &d1;
cptr1 = d2;//可以修改指针指向的对象值，不可以更改指针指向的对象地址
const double const cptr2 = &d1; //两个都不能修改
```

如果const位于*的左侧，则const就是用来修饰指针所指向的变量，即指针指向为常量；   
如果const位于*的右侧，const就是修饰指针本身，即指针本身是常量。

## 2 const修饰函数参数
const修饰函数参数是它最广泛的一种用途，它表示函数体中不能修改参数的值(包括参数本身的值或者参数其中包含的值)

+ `void function(const int Var);` //传递过来的参数在函数内不可以改变(无意义，因为Var本身就是形参，我们的目的是函数内不改变传递过来的内容就好了，这个没有达到目的，会临时复制一个_Var变量，原始变量根本不会改变的)   
+ `void function(const char* Var);` //参数指针所指内容为常量不可变   
+ `void function(char* const Var);` //参数指针本身为常量不可变(也无意义,因为char* Var也是形参，我们的目的是函数内不改变传递过来的内容就好了，这个没有达到目的，还是可以改变指针所指向的内容)

对于非内部数据类型的参数而言，象`void function(A a) `这样声明的函数注定效率比较底。因为函数体内将产生A 类型的临时对象用于复制参数a，而临时对象的构造、复制、析构过程都将消耗时间。**为了提高效率，可以将函数声明改为void function(A a)，因为“引用传递”仅借用一下参数的别名而已，不需要产生临时对象。但是函数void function(A a) 存在一个缺点：**“引用传递”有可能改变参数a，这是我们不期望的。解决这个问题很容易，加const修饰即可 void function(const A a);

## 3 const修饰类对象/对象指针/对象引用
const修饰类对象表示该对象为常量对象，其中的任何成员都不能被修改。对于对象指针和对象引用也是一样。const修饰的对象，该对象的任何非const成员函数都不能被调用，因为任何非const成员函数会有修改成员变量的企图。例如：
```c++
class AAA
{
   void func1();
   void func2()  const;
};
constAAA aObj;
aObj.func1(); ×
aObj.func2(); 正确

const AAA aObj = new AAA();
aObj->func1(); ×
aObj->func2(); 正确
```

## 4 const修饰成员变量
const修饰类的成员函数，表示成员常量，不能被修改，同时它只能在初始化列表中赋值。
```c++
class A
{
   const int nValue;       //成员常量不能被修改
   A(int x):nValue(x) {}; //只能在初始化列表中赋值  ，这个需要注意
};
```

## 5 const修饰成员函数
const修饰类的成员函数，则该成员函数不能修改类中任何非const成员函数。一般写在函数的最后来修饰。
```c++
class A
{
	void function() const;
};
```

对于const类对象/指针/引用，只能调用类的const成员函数，因此，const修饰成员函数的最重要作用就是限制对于const对象的使用。

## 6 const常量与define宏定义的区别
+ 编译器处理方式不同define宏是在预处理阶段展开。const常量是编译运行阶段使用。
+ 类型和安全检查不同define宏没有类型，不做任何类型检查，仅仅是展开。const常量有具体的类型，在编译阶段会执行类型检查。
+ 存储方式不同define宏仅仅是展开，有多少地方使用，就展开多少次，不会分配内存。const常量会在内存中分配(可以是堆中也可以是栈中)。