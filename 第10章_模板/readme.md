# 第十章 模板

## 10.1 模板概念

模板是C++中泛型编程的基础。一个模板就是一个创建类或函数的蓝图或者说公式。
模板本身不是类或函数，相反，可以将模板看作为编译器生成类或函数编写的一份说明。

编译器根据模板创建类或函数的过程称为实例化(instantiation),当使用模板时，需要指出编译器应把类或函数实例化成何种类型。

## 10.2 模板分类

根据模板类型，C++的模板可以分为两种：
* 函数模板
* 类模板

模板的声明和定义和普通的函数和类的声明和定义类似，要在其声明或定义之前加上关键字template和模板参数列表。

模板的声明和定义都不能在函数或函数模板内，但可以在类或类模板内。

```c++
// 函数模板prints的声明
template<int* ptr, class ty>
inline ty prints(int val = ptr);
// 函数模板prints的定义
template<int* ptr, class ty>
inline ty prints(int val) { cout << val << "\n"; return ty(); }
// 类模板Cls的声明
template<typename ty, int int_par>
struct Cls;
// 类模板Cls的定义
template<typename ty, int int_par>
struct Cls { ty data_mem; void prints() { cout << data_mem << " " << int_par << "\n"; } };
```

## 10.3 模板的声明与定义

模板也有声明和定义两种形式，形式为：
> 模板的声明:
> template 模板参数列表 函数或类的声明语句
> 
> 模板的定义:
> template 模板参数列表 函数或类的定义语句

模板参数列表(template parameter list)类似于函数形参表，是一个由逗号`,`分隔的一个或多个模板形参(template parameter)的列表，该列表用`<>`包围起来，且该形参表不能为空，必须至少有一个形参。

> 对于函数模板来说函数模板的类型限定符和存储说明符(如constexpr、inline和static等)是放在模板参数列表后，函数声明或定义语句前的。

和函数一样，模板的声明要与其定义严格一致，具体来说:
* 对于函数模板来说：
  模板形参表要一致(也就是形参表中的形参数量，顺序和类型(可以忽略顶层const)都要相同)，函数首部也要一致(包括函数声明或定义前的类型限定符也要一致)。
* 对于类模板来说：
  模板形参表要一致，类名也要相同。

否则就变成了其他的模板声明，无意义了。

> 函数模板支持重载，只要该模板的模板形参表或者函数形参表不一致就行。
> 类模板不支持重载，所以类名相同但模板形参表不同的模板会导致重复定义；而且同作用域下的类模板名也不能与其他的类类型同名。

和函数形参一样，非参数包的模板形参和函数模板中的非参数包的函数形参都可以有默认实参，它们的规则大部分与普通默认实参的规则一样(比如默认模板实参要放在形参表末尾等)，只有以下的区别：
* 对于模板形参的默认实参来说：
  一个模板的声明(包括模板友元的声明语句)和定义之间可以对同一个模板形参有不同的默认实参，但是该模板形参的默认实参只会按照该形参第一个出现的默认实参来决定。
* 对于函数形参的默认实参来说：
  不能像普通函数一样可以用多个声明来添加默认实参。
  默认实参只能在该模板第一次出现的语句中(声明或定义语句都可以，也包括模板友元的声明语句)，其他语句都不能出现默认实参。

```c++
// 函数模板prints的声明，三个模板形参都有了默认实参
template <class = string, class = int, int = 5> void prints();
// 函数模板prints的定义
template <class ty1, class ty2, int val> 
void prints() 
{ 
    ty1 str = "str "; 
    ty2 ins = 58;
    cout << str << ins; 
}
// 输出str 58
prints();
```

## 10.4 模板形参

模板参数遵循普通的作用域规则，一个模板参数名的可用范围是在其声明之后，至模板声明或定义结束之前。
与任何其他名字一样，模板参数会隐藏外层作用域中声明的相同名字。

> 同一个模板形参表中，各个模板形参的名字不能相同。
> 函数模板中的函数形参和局部变量名或者类模板的成员名也不能与所属的模板形参表中的形参名相同。

和函数形参一样：
* 模板声明中的模板形参名不必与定义中相同，甚至还可以省略。
* 如果我们不需要用某个模板形参，我们就可以在模板定义中省略该形参的名字。

模板形参和函数形参不同，模板形参根据其类型不同，可以分为两种：
* 类型形参
* 非类型形参
* 模板类形参

### 10.41 类型形参

类型形参在模板形参的声明形式为：
> typename/class 类型形参名

关键字typename和class的含义相同，可以互换使用。

类型形参也就是表示类型的形参，我们可以将类型参数看作类型说明符，就像内置类型或类类型说明符一样使用。
所以类型参数可以用来指定返回类型或函数的参数类型，以及在函数体内用于变量声明或类型转换。

对于类型形参的实参来说，可以是任意可用的类型说明符(包括内置类型或者模板的实例)，类型说明符可以包括类型修饰符，但是不能有存储说明符和类型限定符等符号。

```c++
// 含有两个类型形参的函数模板prints
template<typename ty, class ty2> 
void prints(ty val, ty2 val2) { }
```

### 10.42 非类型形参

非类型形参在模板形参的声明形式为：
> 类型说明符(可含类型修饰符) 非类型形参名

非类型形参和函数形参一样，表示一个对象而非一个类型。
不过非类型形参中的类型说明符有很大的限制，只能是以下的类型：
* 整数类型
* 指针或左值引用类型

对于非类型形参的实参来说，该实参必须是常量表达式，必须要在编译时就能获得。

根据形参的类型，非类型形参的实参还有以下的限制条件：
* 对于整数类型的非类型形参的实参来说：
  该实参只能是整数类型的常量表达式(可以不是对应类型的)。
* 对于指针或左值引用类型的非类型形参的实参来说：
  其必须要有静态的生存期(不能是局部非静态变量和动态对象)，且其类型基本上要精确匹配，除了允许从非底层const向底层const的类型转换，其他的转换一律不行。
  因为实参要是常量表达式，所以要遵循常量表达式的规则。
  * 对于左值引用类型有以下的特殊规定:
    * 实参不能是解引用的所得到的对象。
   * 对于指针非类型形参还有以下的特殊规定：
      * 实参只能是地址，也就是用取地址符所得到的地址值，相应的指针对象是不能当做实参的(数组和函数是被转换成对应类型的地址，所以可以用)。

```c++
// 含有一个类型形参，一个非类型形参的函数模板prints
template<typename ty, int tval = 8> 
void prints(ty val, int val2 = tval) { }
```

### 10.43 模板类形参

对于模板形参来说，还有一种特殊的形参，也就叫做模板类形参。
我们可以将一个类模板当做某个模板形参来使用，此时我们只需要在模板形参表写上关键字template、对应类模板的模板形参表和关键字typename或class就行。

模板类形参在模板形参的声明形式为：
> template 类模板的模板形参表 typename/class 模板类形参名

关键字typename和class的含义相同，可以互换使用。

类模板的模板形参表中的模板形参可以有形参名和默认实参，如果填入了默认实参，则使用该模板类形参时会以此时的默认实参为主。

对于模板类形参的实参来说，该实参必须是与对应的模板类形参的模板形参表一致的类模板。

```c++
// 模板prints含有一个模板类形参，且该模板类形参的模板形参表中含有默认实参
template<template<class = string, int = 3> typename cls> 
void prints()
{
    cls<> ob;
    cout << ob.str << " " << ob.ins;
}
// 类模板Cls
template<class ty, int val = 18> 
struct Cls
{
    ty str = "str";
    int ins = val;
};
// 正确：输出str 3
prints<Cls>();
```

### 10.44 可变数目形参

可变数目形参就是指一个可以接受零个或多个对应类型实参的特殊形参，可变数目形参也叫做参数包(parameter packet)。

C++中存在两种参数包：
* 模板参数包(template parameter packet)：
  是在模板形参表中的声明的参数包。
* 函数参数包(function parameter packet)：
  是在模板中的函数形参表(包括函数模板中的函数形参表和类模板中声明或定义的函数以及成员函数模板中的函数形参表)中声明的参数包。
  函数参数包不是之前所说的省略符形参，函数参数包是基于模板参数包所形成的特殊形参。

> 所有参数包都不能有默认实参。

不管是哪一种参数包，含有参数包的形参表都还可以含有其他的非参数包形参，但是每个形参表最多只能含有一个参数包，且必须要放在该形参表的末尾。

含有模板参数包的模板叫做可变参数模板(variadic template)。

根据模板形参的类型，模板参数包一共有三种：
* 类型参数包
* 非类型参数包
* 模板类参数包

模板参数包的声明形式与普通模板形参类似，任何普通模板形参声明表达式的关键字或者说明符(或修饰符)后，形参名前加上省略符`...`，就成了一个模板参数包的声明。

模板参数包的声明形式为：
> 类型参数包的声明：
> typename/class ... 参数包名

> 非类型参数包的声明：
> 类型说明符(可含类型修饰符) ... 参数包名

> 模板类参数包的声明：
> template 类模板的模板形参表 typename/class ... 参数包名

```c++
// 类模板Cls的模板形参表中含有1个类型参数包
template <int val, typename ty, class ... tys>
struct Cls{};

// 类模板Cls2的模板形参表中含有1个非类型参数包
template <typename ty, int ...vals>
struct Cls2{};

// 类模板Cls3的模板形参表中含有1个模板类参数包
template <int val, template <class ty> class ...temps>
struct Cls3{};
```

对于模板参数包来说，我们可以将其对应类型的零个或多个实参传递给该参数包，每个实参以逗号`,`分隔。

```c++
template <int ...vals>
struct Cls {};
// 实例obj中的模板参数包vals中含有5个int模板形参
Cls<15,8,123,25,8> obj;
```

### 10.45 非参数包的模板形参使用

各种非参数包的模板形参可以根据自身的类型，用在模板各种能用该类型的地方。
比如类型形参可以用于各种需要类型的地方，比如当做函数返回类型、形参类型、变量类型等；非类型形参可以当作函数默认实参或者用于需要值的某些类型中等；模板类形参以此类推。

当我们将模板类型形参或者模板类形参实例当做类类型使用时，我们可以访问其成员，且编译器也不会在模板定义时去确定是否有该成员以及该成员的类型。

所以这样会出现一些问题：
之前我们介绍过我们可以用作用域运算符`::`来访问一个类类型的静态成员或者类类型成员。
因为编译器不会在模板定义时去确认成员，所以默认情况下，编译器会认为通过作用域运算符访问的成员为静态成员，所以为了能够访问类类型成员，我们就要显式指定访问的是类类型成员。

我们通过使用关键字typename来实现显式访问类类型成员，使用形式为：
> typename 模板类型形参名(或模板类形参实例)::类类型成员名

typename要紧跟在该模板类型形参名或模板类形参实例后面，所以static等修饰限定符要放在typename之前。

```c++
#include <vector>
template<typename ty> 
void prints(ty obj) 
{
    // 错误：编译器认为cls_int为静态成员，所以出错。
    ty::cls_int val2 = 35;
    // 正确：显式指定cls_int为类类型成员。
    typename ty::cls_int val = 35;
    // 错误：编译器认为iterator为静态成员，所以出错。
    typename std::vector<numType>::iterator iter;
    // 正确：显式指定iterator为类类型成员。
    typename std::vector<numType>::iterator iter2;
    cout << obj.ins + val;
}
struct Cls
{
   typedef int cls_int;
   cls_int ins = 48;
};
// 输出83
prints(Cls());
```

### 10.46 参数包的使用

参数包并不是普通的形参，它是包含了多个同类型参数的形参。

对于参数包来说，只有两种用法：
* 包扩展
* 获取包大小

> 参数包或者包扩展不能单独成为一个语句，也就是不能单独使用，必须要结合其他真正能用上的操作才行。

#### 10.461 包扩展

包扩展是使用参数包最基本的用法，我们只有扩展（expand）了某个参数包后，才能使用该参数包。

##### 10.4611 包扩展的形式

包扩展的形式为：
> 扩展模式...

扩展模式（pattern）是指包含有某参数包名的一个非声明或定义的表达式(比如类型模板参数包的引用，非类型模板参数包与其他表达式的运算，函数参数包的调用等等)。扩展模式会对该参数包中的所有参数应用该表达式。

扩展模式后面必须要紧跟一个省略符`...`。
对于模板参数包来说，模式和省略符之间不能有空白符；而函数参数包则可以有。
> 省略符优先级比算术运算符等符号要高，所以包扩展时要注意优先级的问题。

```c++
// 类型模板参数包tys的扩展
const tys&... 
// 模板类模板参数包temps的扩展
temps<int>...
// 非类型模板参数包vals的扩展
(vals + 20)...
// 函数参数包objs的扩展
prints(objs)...
```

> 未命名的参数包无法扩展，因为无法指定未命名的对象。

##### 10.4612 包扩展的作用

包的扩展就是将该参数包在使用扩展的位置替换为一个包含该参数包所有参数的列表，该列表中的每个参数都应用了其扩展模式，该列表没有括号包围，且每个参数之间由逗号`,`分隔。

```c++
// 类型模板参数包tys，等价于\
const ty1&, const ty2&, const ty3&, ···
const tys&... 
// 模板类模板参数包temps，等价于\
temp1<int>, temp2<int>, temp3<int>, ···
temps<int>...
// 非类型模板参数包vals，等价于\
((val1 + 20), (val2 + 20), (val3 + 20), ···)
(vals + 20)...
// 函数参数包objs，等价于\
prints(obj1), prints(obj2), prints(obj3), ···
prints(objs)...
```

###### 10.46121 函数参数包

对于类型模板参数包和模板类模板参数包来说，它们的扩展还有另一种作用，就是可以声明函数参数包。

> 只有这种形式才能声明函数参数包。

> 函数参数包只能出现在模板的函数形参表中，不能出现在其他地方。函数参数包也遵循参数包的各种规则。

声明函数参数包的形式为：
> 类型或模板类的模板参数包的扩展 函数参数包名

只有扩展模式为模板类模板参数包实例时，才能用模板类模板参数包来声明函数参数包。

函数参数包的类型为声明该参数包的模板参数包的扩展类型。
和模板参数包一样，函数参数包可以包含与其类型相同的零个或多个参数(如不需要该参数包参与模板实参推断，则参数还可以是能隐式转成该参数包类型的)。

```c++
// 输出函数模板
template <class ty>
int prints(ty val)
{ cout << val << " "; return 0;}
// 该函数模板的函数形参表中含有函数参数包obj
template <typename ...tys>
void makes(const tys... objs)
{
    // 等价于\
    vector<int> v1 = {obj1,obj2,obj3,···};
    // 其中的形参类型都为常量类型。
    vector<int> v1 = {objs...};
    static int first = 1;
    if (first--)
        // 等价于\
        makes(prints(obj1), prints(obj2), prints(obj3), ···); 
        makes(prints(objs)...);
}

int main()
{
    // 输出15 478 15 48
    makes(15, 478, 15, 48);
    return 0;
}
```

> 调用包含函数参数包的函数时(包括函数模板实例)，该函数参数包所接受的实参数量不能小于声明该参数包的模板参数包所含的参数数量(主要说的是非模板实参推断的实例化)，否则出错。

#### 10.462 获取参数包的大小

当我们需要知道包中含有多少参数时，可以使用`sizeof...`运算符。

> `sizeof`和省略符`...`之间不能有空白符

> `sizeof...`运算符为一元运算符，运算对象在右侧括号内。
> 运算对象为左值。运算结果为右值。

`sizeof...`运算符的使用形式为：
> sizeof... (参数包名)

该运算符的运算对象只能是参数包名，不能为其他的对象。

`sizeof...`返回一个常量表达式，该表达式的值就是所给参数包当前所含的参数数量。
类似`sizeof`运算符，不会对其运算对象求值。

```c++
template <typename ...tys>
void prints(tys... obj) 
{ cout << sizeof...(obj); }
int main()
{
    // 输出5
    prints(3.15, "str", 2, true, 's');
    return 0;
}
```

## 10.5 模板实例化

模板的实例化(instantiate)也就是编译器根据模板形参表里的实参或者函数模板中的函数实参来推断出模板形参的类型或者值是什么，并使用这个版本。
当编译器实例化一个模板吋，它使用实际的模板实参代替对应的模板参数来创建出模板的新“实例”，这些通常被称为模板的实例(instantiation)。

模板的实例也可以说是根据模板实参，替换掉模板函数或者类里面的所有模板形参，所创建的一个具体的，有着固定内存大小的函数或者类。

要注意模板并不是函数或者类，模板只是一个所谓的蓝图，只有模板的实例才是函数或者类。

> 所以函数模板的实例可以被当做可调用对象；类模板的实例可以被当做类型说明符使用。

实例创建后会一直保存在同文件中，直到程序结束。
所以当一个模板的实例将要被创建时，编译器会检查同文件中该模板是否已经创建过相同的实例，如果是，则会直接调用之前创建的实例进行操作，否则就创建一个新的实例。

当编译器遇到一个模板的定义或声明时，它并不进行实例化，只有使用模板时，才有可能进行实例化。

### 10.51 模板编译流程

在介绍实例化的方式前，我们需要介绍一下模板编译的流程。

模板直到实例化吋才会生成代码，这一特性影响了我们何时才会获知模板内代码的编译错误。通常，编译器会在三个阶段报告错误：
1. 编译模板自身阶段：
   在这个阶段，编译器会对所有非模板形参的名字进行名字查找(不会进行类型检查)以及各种语法错误检查，例如忘记分号、变量名拼错，包扩展使用错误等，但也就这么多了。
2. 模板使用阶段：
   在此阶段，编译器仍然没有很多可检查的，编译器通常会检查模板中的模板实参数目是否正确。
   对于函数模板，还会检查函数形参的数量和类型是否匹配。
3. 模板实例化阶段：
   编译器只有在这个阶段才能发现类型相关的错误，依赖于编译器如何管理实例化，这类错误可能在链接时才会报告。
   在这一阶段，编译器会检查该模板内的模板调用和函数调用是否匹配。
   对于含有包扩展参数的调用表达式，编译器会根据该包所含的实参数量，检查是否符合调用，如果该调用中又含有调用，则沿着调用链向里继续检查，直到所有调用都符合才行，否则编译出错。

```c++
template <typename ...tys>
void prints(string str, tys... obj) 
{
    cout << str << " ";
    // 没有用，因为以下是编译阶段进行的检查。
    if (sizeof...(obj) > 0)
    // 该语句是该模板进行递归调用
    // 如果该模板要实例化，则在实例化阶段，\
    编译器会沿着调用链检查所有的调用是否匹配，\
    其中当obj中的参数数量为0时，无法匹配到该函数模板，所以实例化会出错。
    prints(obj...);
}
int main()
{
    // 调用出错
    prints(string("str"), string("str2"), string("str3"), string("str4"));
    return 0;
}
```

```c++
void prints() {}
template <typename ...tys>
void prints(string str, tys... obj) 
{
    cout << str << " ";
    // 该语句是该模板进行递归调用
    // 如果该模板要实例化，则在实例化阶段，\
    编译器会沿着调用链检查所有的调用是否匹配，\
    其中当obj中的参数数量为0时，会调用空形参表的重载函数，所以会成功实例化。
    prints(obj...);
}
int main()
{
    // 调用正确：
    // 输出str str2 str3 str4
    prints(string("str"), string("str2"), string("str3"), string("str4"));
    return 0;
}
```

### 10.52 实例化分类

模板实例化的方法有以下几种：
* 隐式实例化
* 显式实例化

### 10.53 隐式实例化

隐式实例化是指我们在使用模板时所自动进行的实例化。

隐式实例化的方法分为两种：
* 使用显式模板实参列表
* 模板实参推断

#### 10.531 显式模板实参列表

显式模板实参列表(explicit template argument)也叫做模板实参表，它显式给出模板的各个实参，编译器通过这些实参来实例化具体的函数或类。

使用模板实参表的形式为：
> 模板名 <模板实参表>

模板实参表类似于函数实参表，是一个由逗号`,`分隔的多个模板实参的列表(可以为空)。

和函数实参表一样，模板实参表中的模板实参的数量，顺序和类型要一致。对于有默认实参的模板形参，我们也可以在实参表中省略其实参。

对于所有模板形参来说，实参赋值到形参时，会且仅会进行以下的隐式类型转换(不会进行其他的隐式类型转换)：
* 编译器会忽略顶层const。
* 对于非引用的函数形参来说，函数或者数组名会转换成对应的指针。

> 所以非类型的模板形参的实参要与其形参类型一致才行。

```c++
template <class ty, int val>
struct Cls {};
// 错误：4.8为double，与int不一致
Cls<int, 4.8> obj;
// 正确
Cls<int, 4> obj2;
```

当我们使用了模板实参表后，编译器就会生成一个实例，该实例也就是一个具体的函数或类，我们就可以像使用普通的函数或类一样来使用该实例了。

```c++
template <typename ty, int val>
void prints(ty v1, int multiple = val) 
{ cout << v1 * multiple << "\n"; }
template <class ty, int val = 35>
struct Cls
{
    ty str = "str";
    int ins = val;
};
// 使用函数模板实参表，\
输出64.8
prints<double, 8>(8.1);
// 使用类模板实参表，\
输出35 str
Cls<string> ob;
cout << ob.ins << " " << ob.str;
```

对于==类模板的隐式实例化==来说，其成员并不是在生成实例后就全部实例化了。
默认情况下，==类模板的成员只有在其使用时才被实例化==。
这一特性使得即使某种类型不能符合模板操作的要求，我们仍然能用该类型来实例化类模板。

```c++
template <typename ty>
struct Cls
{
    ty ob = 15;
    void prints() { cout << ob.str; }
};
// 正确：虽然int类型没有str成员，但此时prints没有实例化。
Cls<int> obj;
// 正确：输出15
cout << obj.ob;
```

#### 10.532 模板实参推断

编译器利用函数模板调用中的函数实参来确定其模板参数的过程被称为模板实参推断(template argument deduction)。

模板实参推断只用于函数模板，模板实参推断也就是指当我们调用一个函数模板时，编译器通常会用函数实参来为我们推断模板实参而不需要显式提供模板实参。

模板实参推断只能在以下这两种情况下使用：
* 类似函数调用形式来调用函数模板。
* 用函数模板来初始化或赋值函数指针。

类似函数调用形式来调用函数模板的形式为：
> 函数模板名(函数实参表)

当我们用函数模板来初始化或赋值函数指针时，编译器自动使用指针的各个对应类型来推断模板的实参，此时也必须遵循函数指针的初始化或赋值规则(比如形参数量类型等都要一致)。

当我们使用了没有默认实参的模板类型形参或模板类形参作为函数形参的类型时，编译器推断的类型与使用auto说明符的推断类型一样(也支持引用折叠等操作)：

* 当函数实参的类型为以下这些时，编译器会自动对其进行类型转换(对于其他类型则不会进行类型转换)：
  * 编译器会忽略顶层const。
  * 对于非引用的函数形参来说，函数或者数组名会转换成对应的指针。
* 每个模板类型形参的类型以第一个使用该形参的函数形参的实参来决定，所以之后使用该形参的函数形参的实参必须要与第一个的一致，否则出错。

```c++
template <typename ty>
void prints(ty v1) {}
template <typename ty>
void prints2(ty v1, ty v2) {}
const int ins = 48;
int ar[3] = {8,15,6};
// ty为int
prints(ins);
// ty为int*
prints(ar);
// 正确：两个实参的类型一致
prints2(3.45, 6.15);
// 错误：第一个为double，第二个为int
prints2(3.45, 6);
// 正确
void (*ptr) (string) = prints;
// 正确
void (*ptr2) (int, int) = prints2;
// 错误：第一个为double，第二个为int
void (*ptr3) (int, double) = prints2;
```

> 对于满足以下任意一个条件的函数形参来说，其适用于正常的类型的转换：
> * 其类型不含有模板类型形参以及模板类形参。
> * 其类型含有有默认实参的模板形参。

对于函数指针的初始化或赋值来说，如果两边都需要使用类型自动推断的操作时(也就是函数指针的类型为auto或decltype，且函数模板的某函数形参的类型使用了没有默认实参的模板类型形参或模板类形参时)则不能使用模板实参推断，只能使用模板实参表来隐式实例化。

```c++
template <typename ty>
void prints(ty v1, ty v2) {}
// 以下两个都错误：不能使用模板实参推断，因为两边都需要使用类型自动推断
auto ptr = prints;
decltype(prints) *ptr2 = prints;

// 以下两个都正确：使用模板实参表，类型为\
void (*) (int, int)
auto ptr3 = prints<int>;
decltype(prints<int>) *ptr4 = prints;
```

对于模板实参推断，其还有以下的限制：
1. 当使用函数调用形式的模板实参推断时，对于有默认实参的函数形参来说，满足以下任意一种情况时才能在隐式实例化时省略该实参，否则不能省略：
   * 该函数形参类型不是模板形参。
   * 该函数形参类型是有默认实参的模板形参。
   * 该函数形参类型不含有模板形参。
   * 该函数形参类型含有有默认实参的模板形参。
2. 模板实参推断只能用于满足以下这个条件的函数模板，不满足的函数模板不能使用模板实参推断：
   * 对于函数调用形式的模板实参推断，函数模板中所有没有默认实参的模板形参都应该出现在==函数形参表的形参类型==中。
   * 对于函数指针初始化或赋值的模板实参推断，函数模板中所有没有默认实参的模板形参都应该出现在函数形参表的==形参类型或者返回类型==中。

```c++
int ar[8] = {3,8,6,3};
template <class ty1, class ty2 = int, int val = 8>
void prints(ty1 v1, ty2 v2 = 54, int (&v3)[val] = ar, double v4 = 10.5) { cout << v1 << v2 << *v3 << v4; }
template <class ty1, class ty2, int val>
void prints2(ty1 v1, ty2 v2 = 54, int (&v3)[val] = ar, double v4 = 10.5) { cout << v1 << v2 << *v3 << v4; }
int main()
{
    // 隐式实例化正确：可以省略v2,v3,v4的实参
    prints("str");
    // 隐式实例化错误：v2形参的类型为无默认实参的模板形参\
    v3形参的类型含有无默认实参的模板形参\
    所以不能省略v2,v3的实参
    prints2("str");
}
```

```c++
// 函数模板prints的没有默认实参的模板形参ty出现在了函数形参表中
template <typename ty, int val = 8>
void prints(ty v1, int v2 = val) {}
// 函数模板prints2的没有默认实参的模板形参ty没有出现在函数形参表中
template <typename ty, int val = 8>
ty prints2(int v1, int v2 = val) {}
// 以下三个都正确
prints(6.5);
void (*ptr) (double, int) = prints;
double (*ptr2) (int, int) = prints2;
// 错误:不能用模板实参推断
prints2(8);
```

### 10.54 显式实例化

根据模板实例化的特性，当两个或多个独立编译的源文件使用了相同的模板，并提供了相同的模板参数时，每个文件中就都会有该模板的一个实例。

这样就会导致一个程序会有多个相同的实例，所以，此时我们需要进行显式实例化来消除这种情况。

> 显式实例化只能出现在全局作用域和命名空间中，不能出现在其他的局部作用域内。
> 且显式实例化要在模板定义语句所在的作用域内，否则出错。

显式实例化的形式有两种：
* 显式实例化定义
* 显式实例化声明

显式实例化定义的形式为：
> template 模板的隐式实例化声明

显式实例化声明的形式为：
> extern template 模板的隐式实例化声明

模板的隐式实例化声明类似于隐式实例化，也就是该模板中的所有模板形参都含有模板实参的声明形式。
对于类模板来说，就是其类关键字加上使用显式实参表的形式；对于函数模板来说，是显式实参表或者实参推断的形式的函数声明形式(也就是含有返回类型和函数形参表的函数声明，其中所有模板形参的类型都换为该形参的实参)。

```c++
// 函数模板prints
template <typename ty, int val, class ty2>
ty prints(ty2 v1, int multiple = val) {}
// 类模板Cls
template <typename ty, class ty2, int val>
struct Cls {};

// 函数模板prints的显式实例化定义
template int prints<int, 8, double>(double, int);
// 函数模板prints的显式实例化声明
extern template int prints<int, 8, double>(double, int);
// 类模板Cls的显式实例化定义
template struct Cls<string, int, 48>;
// 类模板Cls的显式实例化声明
extern template struct Cls<string, int, 48>;
```

显式实例化的声明和定义要与对应的模板的声明一致。
而且对于同一个模板实例来说，其显式实例化的声明和定义中的模板实参要一致(可以忽略顶层const；对于非类型形参的实参值是相同的或者能隐式转换成同一个值就行)。

> 同一作用域中，显式实例化声明必须要在显式实例化定义前面，否则会出错。

当编译器遇到显式实例化声明时，编译器不会在该处生成实例化代码，显式实例化声明只是承诺同作用域中有其对应的显式实例化定义。

当编译器遇到显式实例化定义时，编译器就会在该处生成实例化代码。
和隐式实例化不同的是，==显式实例化定义会实例化该模板的所有成员，包括内联的成员函数==。因此，我们用来显式实例化类模板的实参必须能用于该模板的所有成员。

> 对于一个给定的实例化版本，同作用域中可能有多个显式实例化声明，但有且只有一个对应的显式实例化定义。

> 对于显式实例化定义来说，同作用域中如果在显式实例化定义语句之前已经存在对应声明的全部特例化时，则其所有相同声明的显式实例化定义就不会生效，所以此时可以有多个同声明的显式实例化定义。

当我们==使用模板(隐式实例化)时，编译器会检查同作用域中是否有相同模板实参的显式实例化，如果有，则按照该显式实例化的实例来进行操作，而不是再创建一个新的实例==。

```c++
// Application.cpp
// 这些模板类型必须在程序其他位置进行实例化
extern template class Blob<string>;
extern template int compare(const int&, const int&);
Blob<string> sa1, sa2; // 实例化会出现在其他位置
// Blob<int>及其接受initializer_list的构造函数在本文件中实例化
Blob<int> a1 = {0,1,2,3,4,5,6,7,8,9};
Blob<int> a2(al) ; // 拷贝构造函数在本文件中实例化
int i = compare (a1[0], a2[0] ) ; // 实例化出现在其他位置

// templateBuild.h
// 实例化文件必须为每个在其他文件中的显式类型或者函数实例化的声明\
提供一个对应的显式实例化定义。
template int compare(const int&, const int&);
template class Blob<string>; // 实例化类模板的所有成员
```

## 10.6 模板特例化

对于大多数模板来说，通过实例化生成的实例是足够的，但是对于某些特殊的模板实参时，生成的实例可能是不合适的，所以我们有时想编写一些特殊的版本实例来进行一些操作，此时，我们就可以用模板特例化。

模板特例化是模板的一种特殊性质，我们可以使用模板特例化来生成一些特殊的模板实例，模板特例化有以下两种形式：
* 全部特例化
* 部分特例化(偏例化)

模板特例化都是建立在原始模板的基础上的，所以模板特例化只能用于可见的模板中。

关于模板特例化语句的出现位置，有以下规定：
* 对于全部特例化来说，全部特例化语句只能出现在全局作用域和命名空间中，不能出现在其他的局部作用域内。
* 对于部分特例化来说，部分特例化语句还可以出现在类类型和类模板的类体中。
* 与原始模板声明或定义语句的位置关系：
  * 和显式实例化一样，除了成员模板，其他模板的特例化的声明语句要与模板定义语句所在的作用域相同，否则出错。
  * 而对于成员模板来说：
    1. 成员模板的全部特例化的声明语句要与其类模板所在的作用域相同，如果该类模板也是成员模板，则在该类模板的类模板，一直到包含其所有类模板的非类模板的作用域，否则出错。
    2. 成员模板的部分特例化的出现位置只要从模板定义语句所在的位置到包含其所有类模板的非类模板的作用域内的任意位置就行，否则出错。

> 模板特例化也可以有声明语句，只要写成特例化定义形式对应的声明语句形式就行。

### 10.61 全部特例化

常见的模板特例化就是全部特例化(template specialization)，全部特例化是指我们可以编写一个特殊版本的实例来对特定的模板实参进行一些操作。

要注意全部特例化的本质是一个模板实例，而非模板。

全部特例化有两种定义形式：
1. > template <> 函数或者类的定义语句
2. > template <> 类模板成员的定义语句

第1种形式适用于各种模板，而第2种形式只适用于类模板的静态成员、类类型成员和成员模板。

第1种形式中：
* 函数或者类的定义语句是指原始模板中对应的函数或类的定义语句，其中所有的模板形参都被替换为实参，这些实参也需遵循模板实参的规则。
语句中为所有模板形参都提供实参的方式必须要与隐式实例化方式类似，用显式模板实参表或者模板实参推断来提供(和显式模板实参表或者模板实参推断的用法一样，满足省略实参的条件时也可以不提供实参)。

第2种形式中：
* 第2种形式是只特例化特定成员而不是特例化整个模板。
  类模板成员的定义语句是指该模板的某个成员的定义语句，就和类类型成员在类外定义一样，我们要在该定义语句的成员名之前加上该类模板名、尖括号`<>`包围的模板实参表和作用域运算符`::`来表示我们是在定义类模板的该特殊实例的成员。
  > 要注意这种形式中，该成员定义语句中的声明部分(如果是成员模板就是模板声明部分)要与模板中该成员的声明部分一样，否则出错。

> 全部特例化也就是代替编译器手动生成一个特殊的实例，和隐式实例化一样，类模板的成员只有在其使用时才被实例化。

当我们==定义或声明一个全部特例化时，编译器会检查是否已存在相同声明的实例的定义，如果已存在，则出错；否则就生成该特殊实例==，该特殊实例的生成规则为：
* 对于第1种形式，该特殊实例是按全部特例化中的定义语句来生成的。
* 对于第2种形式，除了全部特例化所指定的特殊成员，该特殊实例的其他成员是按原始模板的定义语句来生成的；对于该全部特例化语句所指定的成员，则使用该语句中的定义语句来生成该成员。

> 所以，我们如果要使用模板的全部特例化时，必须要在全部特例化定义语句之后才行，否则会导致编译出错。

> 一个模板可以有多个不同实例的全部特例化。因此，我们不能定义或声明已有实例的全部特例化实例版本。

```c++
template <class ty, typename ty2>
void prints(ty v1, ty2 v2) { cout << v1 << " " << v2; }
// 函数模板prints的全部特例化
template <> void prints(double num, int multiple) { cout << num * multiple; }
// 调用的是普通实例版本\
输出str 15
prints("str", 15);
// 调用的是特殊实例版本\
输出497.2
prints(45.2, 11);

template <class ty>
struct Cls { static ty ob; };
// 类模板Cls的第1种全部特例化形式
template <>
struct Cls<string> { string ob = "str";  void prints() { cout << ob; } };
// 类模板Cls的第2种全部特例化形式
template <> double Cls<double>::ob = 125.48;
// 调用的是普通实例版本
Cls<int> obj;
// 调用的是第1种特殊实例版本
Cls<string> obj2;
// 调用的是第2种特殊实例版本
Cls<double> obj3;
// 错误：普通实例版本没有prints成员
obj.prints();
// 正确：输出str
obj2.prints();
// 正确：输出125.48
cout << obj3.ob;
```

### 10.62 部分特例化

部分特例化(partial specialization)也叫做偏例化，它只适用于类模板。

> 同一个类模板可以有多个不同的部分特例化。

部分特例化是另一种特例化方法，我们可以在特例化时只指定一部分模板形参的实参，或者只指定模板类型形参的一部分而非直接提供一个具体的实参(比如指定成其类型的引用、指针或者常量类型等等)，或者是类型模板参数包的扩展模式包含其他关键字。

> 只有模板类型形参才可以指定一部分(当然也可以全部指定，也就是提供具体的实参)，指定其他非参数包类型的模板形参时必须要提供具体的实参。

> 也只有类型模板参数包的扩展模式才能包含其他关键字，其他的模板参数包的扩展模式只能含有该参数包名，不能有其他的。

部分特例化时，至少要指定一个形参或者类型模板参数包的非只含有包名的扩展，但也不能将模板形参全都指定具体的实参，否则出错。

> 对于没有默认实参的未命名模板形参，则必须要对该形参指定具体实参，否则部分特例化定义时无法在实参表表示该形参。

部分特例化的定义形式为：
> template 未指定实参的模板形参表 类的定义语句

部分特例化本质就是一个模板，所以我们必须要在该模板的模板形参表中按原始模板的形参声明顺序，列出原始模板的所有未指定、只指定部分的和扩展的模板形参，不能列出指定了具体实参的形参。该模板形参表也可以有默认实参。

类的定义语句类似全部特例化中的定义语句，是指原始模板中对应的函数或类的定义语句，且必须要在显式实参表中按原始模板的形参声明顺序，列出原始模板的所有模板实参(对于有默认实参的形参，可以省略其实参)，列出规则为：
* 对于未指定、只指定部分和扩展的模板形参，则在显式实参表中直接写出该模板形参的名字、指定的部分以及该扩展。
* 对于已指定具体实参的模板形参，则在表中写上该实参。

> 对于模板参数包来说，在显式实参表中如果不指定具体实参给该参数包，则必须要用其扩展形式，因为参数包只有扩展后才能用。

因为部分特例化就是原始模板的一个重载模板，所以当要生成一个实例时，如果该实例中的每个模板实参与该部分特例化中的显式实参表所表示的实参一模一样时(对于类型实参来说，就是要能转换成对应的实参，且转换所需的内容更少，还要该类型实参的主要特性与实例实参相同)，则编译器会优先调用该部分特例化来生成该实例，否则就调用原始模板。

> 对于常量、引用和指针类型等特性，主要特性就是指其常量、引用和指针，对于显式实参表中没有显式标注常量、引用或者指针等特性的类型实参，其主要特性就不是该特性。
> 
> 指向常量对象的非常量指针或引用，其主要特性是指针或者引用，不是常量。
> 而对于指向非常量或常量对象的常量指针，其主要特性都是常量，而不是指针类型。

> 如果使用模板时已经存在了相同声明的实例，则编译器是不会调用对应的部分特例化来生成实例的。

```c++
template <class ty, typename ty2>
struct Cls { ty ob; };
// 类模板Cls的部分特例化
template <typename ty>
struct Cls<ty*, string> { string ob = "str"; void prints () { cout << ob; } };
// 调用普通模板来生成实例
Cls<int, string> obj;
// 模板实参匹配，所以调用部分特例化模板来生成实例
Cls<int*, string> obj2;
// 错误：普通模板的实例中没有prints成员
obj.prints();
// 正确：输出str
obj2.prints();
```

```c++
template <class ty, typename ty2>
struct Cls { static void prints() { cout << "original\n"; } };
// 该部分特例化中的显式实参表的第一个类型实参的主要特性为常量类型
template <class ty, typename ty2>
struct Cls<const ty, ty2> { static void prints() { cout << "partial spec\n"; } };
// 该部分特例化中的显式实参表的第一个类型实参的主要特性为指针类型
template <class ty, typename ty2>
struct Cls<ty*, ty2> { static void prints() { cout << "partial spec2\n"; } };
// 该部分特例化中的显式实参表的第一个类型实参的主要特性为引用类型
template <class ty, typename ty2>
struct Cls<ty&, ty2> { static void prints() { cout << "partial spec3\n"; } };
// 该部分特例化中的显式实参表的第一个类型实参的主要特性为引用类型
template <class ty, typename ty2>
struct Cls<const ty&, ty2> { static void prints() { cout << "partial spec4\n"; } };

int main()
{
    // 第一个实参的主要特性为普通对象，所以调用原始模板\
    输出original
    Cls<int, string>::prints();
    // 第一个实参的主要特性为常量，第一个偏例化的主要特性与其相同，所以调用该模板\
    输出partial spec
    Cls<const int, string>::prints();
    // 第一个实参的主要特性为指针，第二个偏例化的主要特性与其相同，所以调用该模板\
    输出partial spec2
    Cls<const int*, string>::prints();
    // 第一个实参的主要特性为常量，第一个偏例化的主要特性与其相同，所以调用该模板\
    输出partial spec
    Cls<int *const, string>::prints();
    // 第一个实参的主要特性为引用，第三偏例化能转换成该实参，且主要特性与其相同，所以调用该模板\
    输出partial spec3
    Cls<int&, string>::prints();
    // 第一个实参的主要特性为引用，第三，第四偏例化能转换成该实参，且主要特性与其相同，但第四偏例化转换所需的内容更少，所以调用该模板\
    输出partial spec4
    Cls<const int&, string>::prints();
    return 0;
}
```

## 10.7 类模板再探

类模板是可以生成类实例的一种模板。
所以类模板中的类的定义可以包含各种类类型所能拥有的属性，比如有数据、函数以及类类型成员，可以有友元和基类等。

> 所以类模板中不能有任何模板的实例化和全部特例化语句。

类模板不是类，它只是一个生成类的蓝图，所以任何需要访问类模板成员的操作都需要通过该模板的实例或者实例的对象来进行。

一个类模板的每个实例都形成一个独立的类型。该类型与其他生成的实例的类型都没有关联，也不会对任何其他实例的类型的成员有任何特殊的访问权限。

类模板中类的定义与类类型一样，除了接下来所介绍的特性外，其他的都遵循类类型的各种规则。

### 10.71 类模板别名

我们可以定义类型别名，因为类模板的实例也是一个类型，所以我们也可以定义类模板实例的别名。

```c++
template <typename ty> struct OtCls {};
template <class ty, int val, template <typename t> typename temp>
struct Cls {};
// typedef定义类型别名
typedef Cls<int, 15, OtCls> Newcls;
// using定义类型别名
using Newcls2 = Cls<string, -8, OtCls>;
```

c++11标准中，我们还可以用`using`关键字来定义类模板别名(不能用`typedef`)。

使用关键字`using`定义类模板别名的语句形式为
> template 模板形参表 using 类模板别名 = 类型说明符(可含有类型修饰符);

定义类模板别名有些类似于类模板的定义：
* 类模板别名定义时的模板形参表和普通模板定义时的模板形参表一样，可以有各种模板形参，可以有默认实参等。
* 等号`=`右边可以含有类型修饰符，作用于类型说明符。
* 类型说明符可以是普通的类，也可以是类模板的实例。
  如果是类模板的实例，则该实例中的实参可以是该别名定义中的模板形参名或者模板形参的特性。
  此时该实例对应的实参是由该using语句定义的类模板别名的实例所决定的。

使用关键字`using`定义的类模板别名是一个含有给定模板形参表的类模板，该类模板所产生的实例也就是语句中等号`=`右边所对应的类。

```c++
template <typename ty> struct OtCls {};
template <class ty, int val, template <typename t> typename temp>
struct Cls {};
// 模板形参表为<int, typename>的类模板别名Newcls
template <int val, typename ty> using Newcls = Cls<ty, val, OtCls>*;
// 模板形参表为<typename>的类模板别名Newcls2
template <typename ty> using Newcls2 = const Cls<string, 25, OtCls>;

// 等价于\
Cls<string, 15, OtCls>* n_obj;
Newcls<15, string> n_obj;
// 等价于\
Cls<int, 48, OtCls>* n_obj2;
Newcls<48, int> n_obj2;
// 等价于\
const <string, 25, OtCls> n2_obj;
Newcls2<int> n2_obj;
// 等价于\
const <string, 25, OtCls> n2_obj2;
Newcls2<double> n2_obj2;
```

### 10.72 类模板的特性

类模板既有模板的特性，也有类的特性。

因为类模板是一个模板，当模板成员或者友元需要使用该模板类型时(比如定义构造函数等)，除了一个例外，我们必须要像类模板的隐式实例化一样提供模板实参表来使用，该实参表的实参要与模板形参一一对应(对于有默认实参的形参，可以省略其实参)。

对于该实参表的实参，可以是以下任意一种(由于未命名形参没有名字，所以其只能用第1种方式)：
1. 实参可以是具体的对象、类型或者模板(对应非类型、类型、模板类形参和模板参数包)：
  此时该位置的模板类型对应的模板形参的实参就是所给的实参。
2. 实参可以是含有模板形参名的非声明或定义的复杂表达式(比如类型形参的引用，非类型形参与其他表达式的运算等等)或者模板参数包的扩展：
  此时该位置的模板类型对应的模板形参的实参还是由该模板的实例所决定。

```c++
template <class ty, int val, typename ty2 = int>
struct Cls
{
    ty mem = ty();
    int ins = val;
    void prints() 
    {
        // 使用该模板类型，所以需要提供模板实参表。
        Cls<int, val + 10> obj; 
        cout << obj.mem + obj.ins; 
    }
};
int main()
{
    Cls<string, 105, int> ob;
    // 输出115
    ob.prints();
    return 0;
}
```

该例外就是：
当我们在模板的作用域内使用该模板类型时，可以不用提供模板实参表，此时编译器会自动帮我们加上模板实参表，每个实参都为对应的形参名(无名的形参就填一个无名标记)。

```c++
template <class ty, int val, typename>
struct Cls
{
    ty mem;
    int ins;
    // 等价于\
    Cls<ty, val, <unnamed>> prints();
    Cls prints();
    // 等价于\
    Cls<ty, val, <unnamed>> (ty mem): mem(mem), ins(val) {}
    Cls(ty mem): mem(mem), ins(val) {}
};
```

类模板和类类型一样，其成员和类类型的成员一样，既可以在模板内定义，也可以在模板外定义。

和类类型一样，因为类模板的类体也是一个作用域：
* 所以在模板内我们可以直接访问可访问的成员而不需要用模板类型及其对象来访问。
* 当我们在类模板外定义其成员时，我们并不在模板的作用域中，直到成员名遇到类名时才表示进入模板的作用域。

### 10.72 类模板成员的类外定义

类模板的成员可以在类外定义，不过因为是模板的成员，所以类外定义的形式与类类型的不一样。

因为是类模板的成员，类模板的每个实例都有其自己版本的成员。因此，类模板的成员具有和所在类模板相同声明的模板形参表。

类成员的类外定义的形式为：
> template 模板形参表 成员的定义语句(其中包含模板类型)

定义在类模板之外的成员必须以关键字template开始，后接类模板形参表，该形参表中的形参名可以与其模板的形参名不一样。
但是该形参表必须要与类模板的形参表的数量，类型和顺序一致(可以忽略顶层const)，而且该形参表中每个形参都必须要有形参名，且都不能有默认实参。

与往常一样，当我们在类模板外定义一个成员时，也必须说明该成员属于哪个模板类型。因此需要在类外定义语句的成员名之前加上模板类型和作用域运算符`::`。
因为类模板外定义不在模板的作用域中，所以使用该模板类型时必须要提供模板实参表，且该模板实参表的实参要与该成员的模板形参表中的形参一模一样(不能是具体的实参或者形参的特性)。

```c++
template <class ty, int val, typename = int>
struct Cls
{
    // 类模板成员prints的类内声明
    void prints();
};
// 类模板成员prints的类外定义
template <typename t, int v, class t2>
void Cls<t, v, t2>::prints() { cout << "external"; }

int main()
{
    Cls<string, 105> ob;
    // 输出external
    ob.prints();
    return 0;
}
```

### 10.73 成员模板

一个类，无论是类类型还是类模板，都可以包含本身是模板的成员。这种成员被称为成员模板(member template)。

根据模板的类型，成员模板也分为两种：
* 成员函数模板
* 成员类模板

> 成员函数模板的函数不能是虚函数，但可以是静态、常量等函数。

#### 10.731 成员模板的定义

根据成员模板在类类型和类模板的区别，成员模板的定义分为两种：
* 类类型的成员模板定义
* 类模板的成员模板定义

不管是哪一种成员模板，其定义与其对应的成员定义一样，既可以在类内定义，也可以在类外定义，也遵循成员定义的各种规则。

##### 10.7311 类类型的成员模板定义

对于在类类型中的成员模板，其定义与类类型的成员定义一样。

以下是类类型中的成员模板的类内定义：
```c++
struct Cls
{
    string str = "str";
    int ins = 8;
    // 类类型中的成员函数模板prints的类内定义
    template <typename cfy>
    static void prints(cfy obj)
    { cout << obj; }

    // 类类型中的成员类模板NestCls的类内定义
    template <class ccy, int ccval>
    struct NestCls
    { 
        ccy mem;
        static void Nprints()
        { cout << ccval; }
    };
};
```

和类类型的类外成员定义一样，需要用作用域运算符表明定义的是类的成员，且还必须提供该成员模板自己的模板形参表(该模板形参表与类模板普通成员的类外定义中的模板形参表性质一样，有不能有默认实参等性质)。

以下是类类型中的成员模板的类外定义：
```c++
struct Cls
{
    string str = "str";
    int ins = 8;
    // 类类型中的成员函数模板prints的类内声明
    template <typename cfy>
    static void prints(cfy obj);

    // 类类型中的成员类模板NestCls的类内声明
    template <class ccy, int ccval>
    struct NestCls;
};
// 类类型中的成员函数模板prints的类外定义
template <typename cfy>
void Cls::prints(cfy obj)
{ cout << obj; }

// 类类型中的成员类模板NestCls的类外定义
template <class ccy, int ccval>
struct Cls::NestCls
{ 
    ccy mem;
    // 类类型中的成员类模板NestCls的函数成员Nprints的类内声明
    static void Nprints();
};

// 类类型中的成员类模板NestCls的函数成员Nprints的类外定义
template <class ccy, int ccval>
void Cls::NestCls<ccy, ccval>::Nprints()
{ cout << ccval; }
```

##### 10.7312 类模板的成员模板定义

成员模板也是模板，定义形式和其对应类型的模板定义形式一样，类模板的成员模板和其所在的类模板之前各有自己的独立的模板形参。

> 成员模板的模板形参名不能与所在类模板的模板形参名相同，否则出错。

以下是类模板中的成员模板的类内定义：
```c++
template <class ty, typename ty2, int val>
struct Cls
{
    ty mem;
    ty2 mem2;
    int ins = val;
    // 成员函数模板prints的类内定义
    template <typename cfy>
    static void prints(cfy obj)
    { cout << obj; }

    // 成员类模板NestCls的类内定义
    template <class ccy, int ccval>
    struct NestCls
    { 
        ccy mem;
        static void Nprints()
        { cout << ccval; }
    };
};
```

因为成员模板也是模板，所以对于类模板的成员模板来说，该成员模板在类外定义时，既要提供所在类模板相同声明的模板形参表，也要提供该成员模板自己的模板形参表(该模板形参表与类模板普通成员的类外定义中的模板形参表性质一样，有不能有默认实参等性质)。

以下是类模板中的成员模板的类外定义：
```c++
template <class ty, typename ty2, int val>
struct Cls
{
    ty mem;
    ty2 mem2;
    int ins = val;
    // 成员函数模板prints的类内声明
    template <typename cfy>
    static void prints(cfy obj);

    // 成员类模板NestCls的类内声明
    template <class ccy, int ccval>
    struct NestCls;
};
// 成员函数模板prints的类外定义
template <class ty, typename ty2, int val>
template <typename cfy>
void Cls<ty, ty2, val>::prints(cfy obj) { cout << obj; }

// 成员类模板NestCls的类外定义
template <class ty, typename ty2, int val>
template <class ccy, int ccval>
struct Cls<ty, ty2, val>::NestCls
{
    ccy mem;
    // 成员类模板NestCls的函数成员Nprints的类内声明
    static void Nprints();
};

// 成员类模板NestCls的函数成员Nprints的类外定义
template <class ty, typename ty2, int val>
template <class ccy, int ccval>
void Cls<ty, ty2, val>::NestCls<ccy, ccval>::Nprints() { cout << ccval; }
```

#### 10.732 成员模板的实例化

成员模板的实例化和普通模板的实例化一样，既可以隐式实例化，也可以显式实例化，同时也遵循模板显式实例化的各种规则。

> 根据显式实例化的规则，显式实例化语句不能出现在类体中。

对于类模板的成员模板来说，因为成员模板是类模板的成员，所以在实例化时必须还要提供该成员模板所在类模板的模板实参。

以下为成员模板的隐式实例化：
```c++
int main()
{
    // 类类型Cls的成员函数模板prints的隐式实例化\
    为模板实参推断
    Cls::prints("str");
    // 类模板Cls的成员函数模板prints的隐式实例化\
    为模板实参推断
    Cls<int, string, 8>::prints("str");

    // 类类型Cls的成员类模板NestCls的隐式实例化\
    为使用显式模板实参表
    Cls::NestCls<double, 10>::Nprints();
    // 类模板Cls的成员类模板NestCls的隐式实例化\
    为使用显式模板实参表
    Cls<int, string, 8>::NestCls<double, 10>::Nprints();
    return 0;
}
```

以下为成员模板的显式实例化：
```c++
// 类类型Cls的成员函数模板prints的显式实例化声明
extern template void Cls::prints(string);
// 类类型Cls的成员函数模板prints的显式实例化定义
template void Cls::prints(string);
// 类模板Cls的成员函数模板prints的显式实例化声明
extern template void Cls<int, string, 8>::prints(string);
// 类模板Cls的成员函数模板prints的显式实例化定义
template void Cls<int, string, 8>::prints(string);

// 类类型Cls的成员类模板NestCls的显式实例化声明
extern template struct Cls::NestCls<double, 10>;
// 类类型Cls的成员类模板NestCls的显式实例化定义
template struct Cls::NestCls<double, 10>;
// 类模板Cls的成员类模板NestCls的显式实例化声明
extern template struct Cls<int, string, 8>::NestCls<double, 10>;
// 类模板Cls的成员类模板NestCls的显式实例化定义
template struct Cls<int, string, 8>::NestCls<double, 10>;
```

#### 10.733 成员模板的特例化

成员模板的特例化和普通模板的特例化一样，既可以全部特例化，也可以部分特例化，同时也遵循模板特例化的各种规则。

> 根据模板特例化的规则，全部特例化语句不能出现在类体中，而部分特例化可以出现在类体中。

以下是类类型中的成员模板的特例化定义：
```c++
struct Cls
{
    // 类类型Cls的成员函数模板prints的定义
    template <typename cfy>
    static void prints(cfy obj)
    { cout << obj << "\n"; }

    // 类类型Cls的成员类模板NestCls的定义
    template <class ccy, int ccval>
    struct NestCls
    {
        ccy mem;
        static void Nprints()
        { cout << ccval << "\n"; }
    };

    // 类类型Cls的成员类模板NestCls的部分特例化的类内定义
    template <class ccy>
    struct NestCls<ccy*, 15>
    {
        static void Nprints()
        { cout << "internal partial spec\n"; }
    };
};
// 类类型Cls的成员函数模板prints的全部特例化定义
template <>
void Cls::prints(int obj)
{ cout << obj << " spec\n"; }

// 类类型Cls的成员类模板NestCls的第1种形式的全部特例化的定义
template <>
struct Cls::NestCls<double, 15>
{
    static void Nprints()
    { cout << "spec No.1\n"; }
};

// 类类型Cls的成员类模板NestCls的第2种形式的全部特例化的定义
template <>
void Cls::NestCls<string, 15>::Nprints()
{ cout << "spec No.2\n"; }

// 类类型Cls的成员类模板NestCls的部分特例化的类外定义
template <class ccy, int ccval>
struct Cls::NestCls<ccy&, ccval>
{
    static void Nprints()
    { cout << "external partial spec\n"; }
};

int main()
{
    // 调用成员函数模板prints的普通实例\
    输出3.5
    Cls::prints(3.5);

    // 调用成员函数模板prints的特例化实例\
    输出3 spec
    Cls::prints(3);

    // 调用成员类模板NestCls的普通实例\
    输出20
    Cls::NestCls<string, 20>::Nprints();

    // 调用成员类模板NestCls的第1种形式的全部特例化实例\
    输出spec No.1
    Cls::NestCls<double, 15>::Nprints();

    // 调用成员类模板NestCls的第2种形式的全部特例化实例\
    输出spec No.2
    Cls::NestCls<string, 15>::Nprints();

    // 调用成员类模板NestCls的类内定义的部分特例化生成的实例\
    输出internal partial spec
    Cls::NestCls<string*, 15>::Nprints();

    // 调用成员类模板NestCls的类外定义的部分特例化生成的实例\
    输出external partial spec
    Cls::NestCls<string&, 15>::Nprints();
    return 0;
}
```

对于类模板的成员模板来说，因为成员模板是类模板的成员，所以在类外特例化时必须还要提供该成员模板所在类模板的模板实参。

因为类模板也是模板，所以在声明或定义该类模板的成员模板的全部特例化语句时，该类模板也必须全部特例化，且其要用全部特例化的第2种形式，否则出错。

以下是类模板中的成员模板的特例化定义：
```c++
template <typename ty, int val>
struct Cls
{
    // 类模板Cls的成员函数模板prints的定义
    template <typename cfy>
    static void prints(cfy obj)
    { cout << obj << "\n"; }

    // 类模板Cls的成员类模板NestCls的定义
    template <class ccy, int ccval>
    struct NestCls
    {
        ccy mem;
        static void Nprints()
        { cout << ccval << "\n"; }
    };
    
    // 类模板Cls的成员类模板NestCls的部分特例化的类内定义
    template <class ccy>
    struct NestCls<ccy*, 15>
    {
        static void Nprints()
        { cout << "internal partial spec\n"; }
    };
};
// 类模板Cls的成员函数模板prints的全部特例化定义
template <>
template <>
void Cls<int, 15>::prints(int obj)
{ cout << obj << " spec2\n"; }

// 类模板Cls的成员类模板NestCls的第1种形式的全部特例化的定义
template <>
template <>
struct Cls<int, 15>::NestCls<double, 15>
{
    static void Nprints()
    { cout << "spec No.1\n"; }
};

// 类模板Cls的成员类模板NestCls的第2种形式的全部特例化的定义
template <>
template <>
void Cls<int, 15>::NestCls<string, 15>::Nprints()
{ cout << "spec No.2\n"; }

// 类模板Cls的成员类模板NestCls的部分特例化的类外定义
template <typename ty, int val>
template <class ccy, int ccval>
struct Cls<ty, val>::NestCls<ccy&, ccval>
{
    static void Nprints()
    { cout << "external partial spec\n"; }
};

int main()
{
    // 调用成员函数模板prints的普通实例\
    输出3.5
    Cls<int, 15>::prints(3.5);

    // 调用成员函数模板prints的特例化实例\
    输出3 spec
    Cls<int, 15>::prints(3);

    // 调用成员类模板NestCls的普通实例\
    输出20
    Cls<int, 15>::NestCls<string, 20>::Nprints();

    // 调用成员类模板NestCls的第1种形式的全部特例化实例\
    输出spec No.1
    Cls<int, 15>::NestCls<double, 15>::Nprints();

    // 调用成员类模板NestCls的第2种形式的全部特例化实例\
    输出spec No.2
    Cls<int, 15>::NestCls<string, 15>::Nprints();

    // 调用成员类模板NestCls的类内定义的部分特例化生成的实例\
    输出internal partial spec
    Cls<int, 15>::NestCls<string*, 15>::Nprints();

    // 调用成员类模板NestCls的类外定义的部分特例化生成的实例\
    输出external partial spec
    Cls<int, 15>::NestCls<string&, 15>::Nprints();
    return 0;
}
```

### 10.74 类模板与友元

之前我们在介绍类类型时谈到过友元，友元不仅可以是函数或者类，还可以是模板。

类模板可以有任何类类型所具有的属性，所以类模板也可以拥有友元。

#### 10.741 模板实例友元

对于各种模板实例友元来说，和普通友元一样，遵循各种友元声明的规则：
* 函数模板实例友元的声明规则和友元函数相同，所以函数模板实例友元可以是在定义类的作用域中不存在的实体。
* 类模板实例友元的声明规则和友元类相同，所以类模板实例友元必须是定义类所能访问到的。

对于类模板中的普通友元(包括内置类型和类类型友元)和模板实例友元来说，该类模板的所有实例都将视它们为友元。

> 我们也可以将类模板的模板类型形参以及模板类形参的实例声明为友元，其访问权限和类模板中的普通友元一样。

```c++
// 类类型Fcls的声明
struct Fcls;

// 类模板Cls的定义
template <class ty, int val>
class Cls
{
    // 函数prints为类Cls的友元
    friend void prints();
    // 类型形参ty为类Cls的友元
    friend ty;
    // 类类型Fcls为类Cls的友元
    friend Fcls;

    int ins = val;
    string str = "strCls";
    ty mem = ty();
public:
    static void cprint() { ty::fprints(); }
};
// 函数prints的定义
void prints() 
{ Cls<string, 35> obj; cout << obj.str << " " << obj.ins << "\n"; }

// 类类型Fcls的定义
struct Fcls
{
    static void fprints() { Cls<int, 5> obj; cout << obj.str << " " << obj.ins << "\n"; }
};

int main()
{
    // 输出strCls 35
    prints();
    // 输出strCls 5
    Fcls::fprints();
    // 输出strCls 5
    Cls<Fcls, 74>::cprint();
    return 0;
}
```

#### 10.742 模板友元

和模板实例友元一样，模板友元也遵循各种友元声明的规则：
* 函数模板友元的声明规则和友元函数相同，所以函数模板友元可以是在定义类的作用域中不存在的实体。
* 类模板友元的声明规则和友元类相同，所以类模板友元必须是定义类所能访问到的。

模板友元的声明语句和普通模板的声明语句类似，为：
> template 模板参数列表 friend 函数或类的声明语句

我们还可以声明类模板的函数成员、类类型成员和成员模板为友元，该友元的声明和它们在类模板的类外声明类似，规则也一样，需要写上所在类模板的形参表、关键字`friend`和类模板类型以及作用域运算符`::`。

```c++
// 类模板Fcls的定义
template <class ty, int val> struct Fcls
{
    static void fprints();
    template <class cty> struct Nest;
};
class Cls
{
    // 函数模板prints为类Cls的友元
    template <typename ty, int val> friend void prints(ty = val);
    // 类模板Fcls为类Cls的友元
    template <class ty, int val> friend struct Fcls;
    // 类模板Fcls的成员函数fprints为类Cls的友元
    template <class ty, int val> friend void Fcls<ty, val>::fprints();
    // 类模板Fcls的成员类模板Nest为类Cls的友元
    template <class ty, int val> template <class cty> friend struct Fcls<ty, val>::Nest;
    int ins = 76;
    string str = "strCls";
};
// 函数模板prints的定义
template <typename ty, int val> void prints(ty v1) { Cls obj; cout << obj.str << " " << v1 << "\n"; }
// 类模板Fcls的成员函数fprints的类外定义
template <class ty, int val> 
void Fcls<ty, val>::fprints() { Cls obj; cout << obj.ins << "\n"; }
// 类模板Fcls的成员类模板Nest的类外定义
template <class ty, int val>
template <class cty>
struct Fcls<ty, val>::Nest
{ static void fnprints() { Cls obj; cout << obj.ins << " " << obj.str << "\n"; } };
```

模板友元的访问权限为：
* 对于类类型的模板友元来说，该模板友元的所有实例都是该类类型的友元。
* 对于类模板的模板友元来说，该类模板的所有实例都将视其模板友元的任意实例为友元。

```c++
int main()
{
    // 输出strCls 6
    prints<int, 6>();
    // 输出76
    Fcls<int, 39>::fprints();
    // 输出76 strCls
    Fcls<double, 9>::Nest<int>::fnprints();
    return 0;
}
```

## 10.8 模板重载

函数模板可以被另一个相同名字的模板或者一个普通非模板同名函数重载。只要它们之间的模板形参表或者函数形参表不一致就行。

不过类模板不支持重载，所以类名相同但模板形参表不同的模板会导致重复定义。

> 类模板的部分特例化虽然是模板，但这不是重载，部分特例化与重载有很大区别，部分特例化不支持随意拓展或者改变模板形参表。

含有函数模板的函数匹配和普通函数的匹配规则类似，只有某些不同，以下是含有函数模板的函数匹配流程：
* 对于一个函数调用，其候选函数不仅仅是在调用点可访问的普通同名函数，还包括所有的可行的函数模板实例。
  可行的函数模板实例是指：
  编译器根据实参类型，推断所有在调用点可访问的同名模板所对应的函数实例。并在其中排除掉所有的不可行函数实例，最后留下的都是可行的函数实例，也就是可行的函数模板实例。
  > 编译器只是推断实参所对应的实例，而不是真正的生成实例。
* 与往常一样，编译器会排除所有不可行的普通同名函数，然后对所有可行函数(包括普通函数和模板所推断的函数)按形参类型进行优先级排序。
* 与往常一样，如果有且只有一个最佳函数，则调用该函数；但如果有多个匹配度同样好的函数，那么当这些同样好的函数中有且只有一个非模板函数，则调用该函数；否则，此调用无匹配或者有歧义。

```c++
// 对于调用prints(&ins)，该模板对应的函数实例声明为：\
void prints(const int* &);
template <class ty>
void prints(const ty &obj) { cout << "Temp func No.1\n"; }
// 对于调用prints(&ins)，该模板对应的函数实例声明为：\
void prints(int*);
template <class ty>
void prints(ty* obj) { cout << "Temp func No.2\n"; }
// 普通函数
void prints(const int* obj) { cout << "Func No.1\n"; }
int main()
{
    int ins = 15;
    // 最佳函数的声明为\
    void prints(int*);\
    所以就调用template <class ty> void prints(ty* obj)模板\
    输出Temp func No.2
    prints(&ins);
    return 0;
}
```

```c++
// 对于调用prints(&ins)，该模板对应的函数实例声明为：\
void prints(const int* &);
template <class ty>
void prints(const ty &obj) { cout << "Temp func No.1\n"; }
// 对于调用prints(&ins)，该模板对应的函数实例声明为：\
void prints(int*);
template <class ty>
void prints(ty* obj) { cout << "Temp func No.2\n"; }
// 普通函数
void prints(int* obj) { cout << "Func No.1\n"; }
int main()
{
    int ins = 15;
    // 最佳函数的声明为\
    void prints(int*);\
    有两个函数满足该声明，其中一个是普通函数，所以调用普通函数的版本\
    输出Func No.1
    prints(&ins);
    return 0;
}
```

对于含有模板参数包的模板来说，如果该模板调用时显式提供了模板实参(使用模板实参表)，则不管有无模板参数包，其模板的实例优先级是一样的。
如果模板调用时没有显式提供模板实参，且某些重载函数中还含有函数参数包，则对于所有的可行模板实例来说，没有函数参数包的模板实例优先级最高，其次是含有其他非函数参数包的形参最多的实例，以此类推，只含有函数参数包的实例优先级最低。

```c++
template <typename ty>
void prints(ty obj) { cout << "common\n"; }
template <class ty, typename ...tys>
void prints(ty obj, tys... objs) { cout << "omission\n"; }
template <class ty, class ty2, typename ...tys>
void prints(ty obj, ty2 obj2, tys... objs) { cout << "omission2\n"; }
template <class ty, class ty2, class ty3, typename ...tys>
void prints(ty obj, ty2 obj2, ty3 obj3, tys... objs) { cout << "omission3\n"; }
int main()
{
    // 输出common
    prints(45);
    // 输出omission2
    prints(45, "str");
    // 输出omission3
    prints(45, "str", 23.4);
    return 0;
}
```

