# 第十四章 固有的不可移植特性

# 14.1 不可移植特性的概念

为了支持低层编程，C++定义了一些固有的不可移植(nonportable)的特性。所谓不可移植的特性是指因机器或者操作系统而异的特性，当我们将含有不可移植特性的程序从一台机器或者操作系统转移到另一台机器或者操作系统上时，通常需要重新编写该程序。

对于C++的不可移植特性，一般有以下几种：
* 算术类型的所占字节的大小
* 位域
* volatile限定符
* 链接指示

算术类型的所占字节的大小我们在将数据类型时介绍过，接下来我们主要介绍后三种不可移植特性。

# 14.2 位域

类可以将其非静态数据成员定义成位域(bit-field),在一个位域中含有一定数量的二进制位。
当一个程序需要向其他程序或硬件设备传递二进制数据吋，通常会用到位域。

位域的声明形式是在成员名字之后紧跟一个冒号以及一个常量表达式。

位域的声明形式为：
> 类型说明符(可含类型修饰符) 位域名: 常量表达式;

位域的类型必须是整型或枚举类型。
> 因为带符号位域的行为是由具体实现确定的，所以在通常情况下我们使用无符号类型保存一个位域。

声明中的常量表达式用于说明该位域所占的二进制位数的数量。

> 位域不能有类内初始值。

> 一般来说, 在类的内部连续定义的位域内存空间会被压缩在同一整数的相邻位，从而提供存储压缩。

取地址运算符`&`不能作用于位域，因此任何指针都无法指向类的位域。

位域和其他的整型变量一样，可以被初始化或赋值，也可以初始化或赋值其他对象，只要所给的值的类型是该位域的类型或者能隐式转换成该类型就行。
因为位域所占的空间只有给定位数的大小，所以当位域被初始化或赋值时，从低位向高位储存每位的值，直至达到规定位数；如果用于初始化或赋值的值的位数小于位域时，则位域也是从低位向高位储存每位的值，高位如有空，则其位值为0。用位域来初始化或赋值与其类似。

> 位域在内存中的布局是与机器相关的。

```c++
struct Cls
{
    // 位域bits的声明
    unsigned bits : 3; 
    Cls(): bits(9) {}
};
// 位域bits被int型字面值初始化
// 字面值9的最低的4位二进制为
// 1001
// 位域bits的位数为3位，按照赋值规则，bits初始化后的位数为
// 001
// 按无符号int型就表示数字1
Cls obj;
// 输出1
cout << obj.bits;
```

# 14.3 volatile限定符

直接处理硬件的程序常常包含这样的数据元素，它们的值由程序直接控制之外的过程控制。例如，程序可能包含一个由系统时钟定时更新的变量。
当对象的值可能在程序的控制或检测之外被改变时，应该将该对象声明为volatile，关键字volatile告诉编译器不应对这样的对象进行优化。

volatile限定符的用法和const几乎一样，遵循各种const的规则，且const能和volatile同时作用与一个对象中。

一般定义volatile对象的语句形式为
> volatile 类型说明符 变量名 (初始化);
> 或
> 类型说明符 volatile 变量名 (初始化);

类的非静态函数成员能定义成const成员函数，所以非静态函数成员也能定义成volatile的。
volatile对象为类类型时，其不能使用该类型的非volatile的非静态成员函数。

```c++
struct Cls
{
    // volatile的成员函数prints
    void prints(int) volatile
    { cout << "vo"; }
    void nprints() {}
};
volatile Cls obj;
// 正确：volatile对象可以调用其volatile成员函数prints
obj.prints(15);
// 错误：volatile对象不能使用非volatile的非静态成员函数
obj.nprints();
```

volatile限定符的用法有一点与const不同，这也就是：
我们不能使用合成的拷贝/移动构造函数及赋值运算符来初始化或赋值类类型中的volatile数据成员。
所以我们必须自定义这些函数来初始化或赋值volatile数据成员。

# 14.4 链接指示

C++程序有时需要调用其他语言编写的函数，或者其他语言有时也需要调用C++编写的函数，最常见的是C和C++。
C++使用链接指示（linkage directive）指出任意非C++函数所用的语言。

> 链接指示语句只能用c++编译器编译，所以所有涉及到链接指示语句的文件要用c++编译器编译。

> 要想把C++代码和其他语言（包括C语言）编写的代码放在一起使用(也就是相互都可以使用)，要求我们所使用的c++编译器必须能够有权访问该语言的编译器，并且也兼容这个语言的编译器。

## 14.41 C++调用其他语言的函数

像所有其他名字一样，其他语言中的函数名字也必须在C++中进行声明，并且该声明必须指定返回类型和形参列表。
对于其他语言编写的函数来说，编译器检查其调用的方式与处理普通C++函数的方式相同，但是生成的代码有所区别。

链接指示可以有两种形式：单个的或复合的：

> extern 字符串字面值 函数声明或定义语句
> 
> extern 字符串字面值 { 多个函数的声明或定义语句 }

这两种形式都是首先包含一个关键字`extern`，后面跟是一个字符串字面值以及一个普通的函数声明或定义。

字符串字面值指出了编写函数所用的语言。比如`"C"`、`"Ada"`、`"FORTRAN"`等。

函数声明或定义语句也就是其他语言编写的函数，这些函数必须指定返回类型和形参列表。

对于第二种花括号括起的形式，它可以支持多个其他语言函数的链接指示。
花括号的作用是将其中的多个声明或定义聚合在一起，一次性建立多个链接，花括号中声明或定义的函数名字就是可见的，就好像在花括号之外声明或定义的一样。
花括号中还可以包含`#include指令`，当一个`#include指令`被放置在的花括号中时，头文件中的所有普通函数声明或定义都会被认为是由链接指示的语言编写的。

链接指示可以嵌套，如果花括号中包含带自带链接指示的函数，则该函数的还是按自己的链接来，不受外层链接指示的影响。

> 对于某个带有链接指示的函数声明或定义，其函数的其他声明最好也应带有链接指示，且链接指示必须一致。

```c++
// 文件test.c
extern "C" void print();
extern "C"
{
    int ret();
    void get(int);
}
void print() { /*···*/ }
int ret() { /*···*/ }
void get(int) { /*···*/ }

// 文件test.cpp
#include "test.c"
int main()
{
    print();
    int ins = ret();
    get(5);
}
```

### 14.411 链接指示指针

我们还可以声明或定义指向其他语言的函数的指针，该指针的声明或定义与普通的指针声明或定义类似，就在前面加上关键字`extern`和表示语言的字符串字面值

链接指示指针的形式为：
> extern 字符串字面值 函数指针声明或定义语句

该函数指针声明或定义语句也必须指定返回类型和形参列表。

```c++
// pf指向一个C函数，该函数接受一个int返回void
extern "C" void (*pf) (int);
```

链接指示指针可以初始化、赋值和使用，但是它只能对该语言对应声明的函数使用，C++的函数不能和它起作用，否则出错。

```c++
//指向一个C++函数 
void (*pf1) (int);
//指向一个C函数
extern "C" void (*pf2) (int);
//错误：pfl和pf2的类型不同
pf1 = pf;
```

如果链接指示指针的返回类型或者某形参是函数指针类型时，这些返回类型或者形参也是链接指示指针。

```c++
// f1是一个C函数，它的形参是一个指向C函数的指针
extern "C" void f1(void(*) (int));
```

如果我们希望某个C++函数的返回类型或者某形参是链接指示指针类型时，则我们必须使用类型别名，来声明这些链接指示指针可以当做C++函数的返回类型或者某形参。

形式为：
> extern 字符串字面值 typedef 函数指针声明语句

```c++
// FC是一个指向C函数的指针
extern "C" typedef void FC(int);
// f2是一个C++函數，该函数的形参是指向C函数的指针
void f2(FC*);
```

## 14.42 其他语言调用C++的函数

我们也可以令C++函数在其他语言编写的程序中可用，也可以在其他语言中声明或定义指向C++函数的指针(如果该语言有指针的话)。

这些也是用链接指示来声明或定义的，形式一模一样，只要在表示语言的字符串字面值中写上所要用C++函数的语言就行。

```c++
// calc函数可以被C程序调用
extern "C" double calc(double dparm) { /* ...*/ }
// ptr函数指针可以在C程序中指向C++函数
extern "C" double (ptr*) (double);
```

值得注意的是，可被其他语言使用的C++函数的返回类型或形参类型会受到很多限制。
例如，我们不太可能把一个C++类的对象传给C程序，因为C程序根本无法理解构造函数、析构函数以及其他类特有的操作。

因为C语言也不支持函数重载，所以我们在定义能被C程序使用的C++函数时不能定义多个名字相同的函数。

```c++
//错误：两个extern "C"函数的名字相同
extern "C" void print(const char*);
extern "C" void print(int);
```

