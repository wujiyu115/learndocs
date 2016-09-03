c++　第一章 基础语法,控制流程,数据类型
================================================

### 一 定义变量:
```c++
double a=2.0
```


### 二 控制流程:
1. **if ...else **
```c++
  void control() {
      int a = 10;
      if (a > 5) {
          cout << "ok" << endl;
      } else {
          cout << "not ok" << endl;
      }
  }
```

2. **switch:表达式只能是整型，字符型，枚举型**
```c++
  void control() {
      int a = 10;
      if (a > 5) {
          cout << "ok" << endl;
      } else {
          cout << "not ok" << endl;
      }
  }
```
3. **for..i**
```c++
  for (int i = 0;  i < 10; ++ i) {
          cout <<"hello";
  }
```

4. **while**
```c++
    int test=0;
    while (test < 100) {
      test++;
      cout << test<<"\t";
    }
```

### 三 数据类型
`int ,char,bool,float,double,int[],double&amp;,char*,enum`
+ **整数类型:int**
[![image](http://farwmarth.com/wp-content/uploads/2013/03/image_thumb18.png "image")](http://farwmarth.com/wp-content/uploads/2013/03/image18.png)
所有的c++编译器都应该满足c++标准所规定的整数长度关系式: `char<short int<int<long int`

+ **字符型:char**
```c++
  char ch = 'A';
  cout << ch << endl;
```
值得注意的是ASCII码并不是最终标准，所以为了代码的可移植性，不要用数字对字符变量进行赋值。

+ **枚举型:enum**
```c++
  enum Week {
          Mon, Tue, Wed, Thu, Fri, Sta, Sun
      };
  Week mon = Tue;
  cout<<mon<<endl;
```
枚举一量定义不能改变，所以常用来代替整数常量，作用更优的是常量需要初始化，而枚举是一种类型，无须定义其实体。

+ **布尔型:bool :非0为真**
还有:float,double,int[],double&amp;,char*


+ **c-串**
c-串 :从c语言中延续的字符数组，以0结尾。字符长度要加1.
```c++
const char str="Hello";
char str="Hello"; //字符串常量赋予char指针错误deprecated conversion from string constant to ‘char’
char str="Hello"; //指针地址赋予char类型错误invalid conversion from ‘const char’ to ‘char’
cout<<*str<<endl;  //Hello
cout<<str<<endl;  //Hello
cout<<str<<endl; //Hello
const char chars= "s",chars1= "s";
cout<<(chars==chars1)<<endl;   //true
char is_char[]="s",is_char1[] = "s";
cout<<(is_char==is_char1)<<endl;  //false
string s = "hello", s1 = "hello";
cout<<(s==s1)<<endl;    //true
```
[![image](http://farwmarth.com/wp-content/uploads/2013/03/image_thumb19.png "image")](http://farwmarth.com/wp-content/uploads/2013/03/image19.png)
在比较同个字符相同的c-串是用==会不相等，因为是指向两个不同位置的指针。而应该用strcmp

+ **string:**
string是一种自定义类型，可以方便地操作。不像字符指针一样蛋疼。
```c++
  string a, s1 = "hello";
  string s2 = "123";
  //比较
  cout << (a == s1) << endl; //false
  a = s1;
  cout << (a == s1) << endl; //true
  //连接
  cout << (a + s2) << endl;
  //查找
  cout << (a.find("e")) << endl
```

+ **数组:**
定义格式:`类型名 数组名[常量表达式]`
```c++
  int arr1[3] = { 1 };
  int arr2[3];
  void arrays() {
      int arr3[3] = { 1 };
      int arr4[3];
     static int arr5[3];

      int i = 0;
      for (i = 0; i < 3; ++i) {
          cout << arr1[i] << "\t";
      }
      cout << endl;
      for (i = 0; i < 3; ++i) {
          cout << arr2[i] << "\t";
      }
      cout << endl;
      for (i = 0; i < 3; ++i) {
          cout << arr3[i] << "\t";
      }
      cout << endl;
      for (i = 0; i < 3; ++i) {
          cout << arr4[i] << "\t";
      }
      cout << endl;
      for (i = 0; i < 3; ++i) {
          cout << arr5[i] << "\t";
      }

```
[![image](http://farwmarth.com/wp-content/uploads/2013/03/image_thumb20.png "image")](http://farwmarth.com/wp-content/uploads/2013/03/image20.png)

全局数组和静态数组未初始化，系统会默认为清零，局部数组未初始化会出现随机数。

+ **vector容器**
```c++
  void vectors() {

      vector<int> ve;
      for (int i = 0; i < 10; ++i) {
          ve.push_back(i);
      }

      vector<int>::iterator p = ve.begin();
      for (; p<ve.end(); p++) {
        cout<<*p;
      }

  }
```