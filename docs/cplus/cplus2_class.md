c++ 第二章 面向对象编程
==========================

## **一 类**

+ 类后面要加上分号
```c++
Student{
private:
	int age;
	int sex;
public:
	void setAge(int age);
	void printAge();
};
```

+ 一般为了更好组织代码，在头文件中定义类声明，在cpp中实现类定义
Student.h

```c++
Student{
private:
	int age;
	int sex;
public:
	void setAge(int age);
	void printAge();
	void friend  friends(Student &amp;a);

};
```
```c++
#include <stdio.h>
#include "stdafx.h"
void  Student::setAge(int ages){
	age = ages;
}
void Student::printAge(){
	printf("%d",age);
}
void  friends(Student &amp;stu){
	printf("%d",stu.age);
}
```

+ 构造函数和析构函数
构造函数没有返回值 :类名();
析构函数声明 : ~类名();
```c++
#ifndef  _SCHOOL_BOY_H_
#define  _SCHOOL_BOY_H_
#include "Student.h"
 SchoolBoy: public Student{
public:
	 void sayHi();
	 SchoolBoy();
	 ~SchoolBoy();

};
#endif
```

```c++
#include "stdafx.h"
#include "SchoolBoy.h"
#include <iostream>

SchoolBoy::SchoolBoy(){
	printf("%s\n","init schoolboy");
}

SchoolBoy::~SchoolBoy(){
		std::cout<<"bye\n";
}

void SchoolBoy::sayHi(){
	printf("%s\n","hi");
}
```

## 二 友元

是一种定义在类外部的普通函数或类，但它需要在类体内进行说明，为了与该类的成员函数加以区别，在说明时前面加以关键字friend。友元不是成员函数，但是它可以访问类中的私有成员。友元的作用在于提高程序的运行效率，但是，它破坏了类的封装性和隐藏性，使得非成员函数可以访问类的私有成员

1.**友元函数**
 语法与普通函数相同，调用也相同，与成员函数不同的是它不属于任何类,但它能够引用类中的私有成员。如 `void friend friends(Student &amp;a); `这个友元函数可以访问Student的私有成员age.

 2.**友元类**
 友元类的所有成员函数都是另一个类的友元函数，都可以访问另一个类中的隐藏信息（包括私有成员和保护成员）。当希望一个类可以存取另一个类的私有成员时，可以将该类声明为另一类的友元类。定义友元类的语句格式如下： friend class 类名;
 其中：friend和class是关键字，类名必须是程序中的一个已定义过的类。
 例如，以下语句说明类B是类A的友元类：
```c++
 class A
 {
	 public:
	 friend class B;
 };
```
经过以上说明后，类B的所有成员函数都是类A的友元函数，能存取类A的私有成员和保护成员。

3.**使用友元类时注意：**

+ 友元关系不能被继承。
+ 友元关系是单向的，不具有交换性。若类B是类A的友元，类A不一定是类B的友元，要看在类中是否有相应的声明。
+ 友元关系不具有传递性。若类B是类A的友元，类C是B的友元，类C不一定是类A的友元，同样要看类中是否有相应的申明

4.**成员访问控制符**

+ `public`:无任何限制
+ `private`:只能被类声明中的成员函数和友元访问。
+ `protected`:可以被1.该类中的函数、2.其友元函数访问 3.子类的函数。但不能被该类的对象访问.

##  三 **继承**

** 使用`:` 来表示继承,多重继承用`,`号来隔开**

```c++
#ifndef  _SCHOOL_BOY_H_
#define  _SCHOOL_BOY_H_
#include "Student.h"
 SchoolBoy: public Student{
public:
	 void sayHi();
};
#endif // _SCHOOL_BOY_H_
```



### **派生类继承访问权限**
1.如果子类从父类继承时使用的继承限定符是public，那么

+ 父类的public成员成为子类的public成员，允许类以外的代码访问这些成员
+ 父类的private成员仍旧是父类的private成员，子类成员不可以访问这些成员；
+ 父类的protected成员成为子类的protected成员，只允许子类成员访问；

2.如果子类从父类继承时使用的继承限定符是private，那么

+ 父类的public成员成为子类的private成员，只允许子类成员访问；
+ 父类的private成员仍旧是父类的private成员，子类成员不可以访问这些成员；
+ 父类的protected成员成为子类的private成员，只允许子类成员访问；

3.如果子类从父类继承时使用的继承限定符是protected，那么

+ 父类的public成员成为子类的protected成员，只允许子类成员访问；
+ 父类的private成员仍旧是父类的private成员，子类成员不可以访问这些成员；
+ 父类的public成员成为子类的protected成员，只允许子类成员访问；