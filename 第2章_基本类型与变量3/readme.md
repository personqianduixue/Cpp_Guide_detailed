### 2.5 常量限定符

有时我们希望定义这样一种变量，它的值不能被改变。此时可以用常量类型修饰符对变量的类型加以限定。

对于常量类型修饰符，有两种
* const
* constexpr

这两种虽然都能定义常量类型，但是还是有很大的区别的。

> 虽然没有意义，但是const和constexpr可以作用于同一个定义或声明语句中，修饰同一个类型，此时变量以类型修饰符constexpr的规则为主。

#### 2.51 const限定符

例如，用一个变量来表示缓冲区的大小。使用变量的好处是当我们觉得缓冲区大小不再合适时，很容易对其进行调整。 
另一方面，也应随时警惕防止程序一不小心改变了这个值。为了满足这一要求，可以用关键字const对变量的类型加以限定

一般定义const对象的语句形式为
> const 类型说明符 变量名 (初始化);
> 或
> 类型说明符 const 变量名 (初始化);

> 任何const对象一旦创建后其值就不能再改变，任何试图为其赋值的行为都将引发错误。

```c++
const int bufSize = 512; // 输入缓冲区大小
buf Size = 512; //错误：试图向const对象写值
```

> 类型修饰符const既可作用于类型，也可作用于变量。当无其他类型修饰符修饰变量时，默认作用于其类型。
> 作用于类型时，放置在类型的左边右边都可以。
> 作用于变量时，只能放置在变量的左边。

```c++
// 这两个定义语句形式等价，都是定义了3个const int型变量，const此时修饰的是int类型
const int j = 8, k = 9, l = 10;
int const j = 8, k = 9, l = 10;
// 定义了一个指向int的常量指针p和一个int变量ins，const此时修饰的是变量p
int *const p = nullptr, ins = 6;
```

因为const对象一旦创建后其值就不能再改变，所以==定义的const对象必须显式初始化==。一如既往，初始值可以是任意复杂的表达式。

> 但是const变量可以用extern只声明而不定义。

```c++
const int i = get_size () ; //正确：运行时初始化
const int j = 42; //正确：编译时初始化
const int k; //错误：k是一个未经初始化的
```

> 默认状态下，const对象仅在当前文件内有效，要想多文件内都有效要用之前说的extern存储修饰符

##### 2.511 非引用或指针类型的const变量

const对于非引用或指针类型的变量来说，用const修饰后的变量，其变量无法被赋值。

对于容器类来说，const的对象不仅无法被赋值，其元素也无法类赋值。

对于派生类类型来说，const的对象不仅无法被赋值，其非mutable的非静态成员也无法被赋值，且const对象无法使用非const的函数成员。

>支持拷贝初始化的非引用或指针类型的const变量与对应类型的非const变量之间能够相互初始化对方。
支持赋值的非引用或指针类型的非const变量能够被对应类型的const变量赋值。

##### 2.512 引用类型的const变量

可以把引用绑定到const对象上，就像绑定到其他对象上一样，我们称之为对常量 
的引用(reference to const)。与普通引用不同的是，对常量的引用不能被用作修改它所绑定的对象

> 要注意引用本身并不是对象，所以const只能修饰引用所绑定的对象的类型

```c++
const int ci = 1024;
const int &rl = ci; //正确：引用及其对应的对象都是常量
rl = 42;  //错误：rl是对常量的引用
int &r2 = ci;  //错误：试图让一个非常量引用指向一个常量对象
```

之前说过，引用的类型必须与其所引用对象的类型一致，不能用常量引用来初始化非常量引用。
> 右值常量引用同样遵循这个规则，不能用常量引用来初始化右值非常量引用。

> 要注意，因为右值引用只能绑定到右值对象上，对于右值来说，不存在不能被修改的右值，所以不管是右值引用还是右值常量引用都能绑定到对应类型的任意右值对象上。

```c++
const int &&r1 = 84;
const int &&r2 = std::move(r1); // 正确：右值常量引用来初始化右值常量引用。
int &&r3 = std::move(r1); // 错误：不能用右值常量引用来初始化右值非常量引用。
```

左值常量引用能够用任意表达式作为初始值，尤其，允许为一个常量引用绑定对应类型的常量或非常量引用、非常量的对象、字面值，甚至是个一般表达式(也就是左值常量引用能够绑定到右值上了)：

```c++
int i = 42;-
const int &rl = i; //允许将const int&绑定到一个普通int对象上
const int &r2 = 42; //正确：rl是一个常量引用
const int &r3 = rl * 2; //正确：r3是一个常量引用
int &r4 = rl * 2; //错误：r4是一个普通的非常量引用
```

##### 2.513 指针类型const变量

根据const在指针对象所处的不同位置，const指针可以分为两种
* 顶层const(top-level const)
* 底层const (low-level const)

顶层const表示该指针本身是个常量，更一般的，顶层const可以表示任意的对象是常量，这一点对任何数据类型都适用，如算术类型、类、指针等(引用本身就是不可被修改赋值的)。

而底层const一般用于表示指针所指的对象是一个常量，多用于与指针和引用类型

> 指针类型既可以是顶层const也可以是底层const，或者两者都是

```c++
int i = 0;
int *const pl = &i; //不能改变pl的值，这是一个顶层const
const int ci = 42; //不能改变ci的值，这是一个顶层const
const int *p2 = &ci; //允许改变p2的值，这是一个底层const
const int *const p3 = p2; // 靠右的 const 是顶层 const,靠左的是底层 const 
const int &r = ci; //用于声明引用的const都是底层const
```

顶层const和底层顶层const的区别为

* 顶层const能初始化或赋值对应类型的非const变量，非const变量也能初始化顶层const

* 非const变量能初始化底层const，但底层const却不能初始化或赋值对应类型的非const变量。

> 也就是所有的非常量都能转换为常量，而反过来，只有顶层const才能转成非常量，底层const不行。

```c++
i = ci; //正确：拷贝ci的值，ci是一个顶层const,对此操作无影响
p2 = p3; //正确：p2和p3指向的对象类型相同，p3顶层const的部分不影响
int *p = p3; //错误：p3包含底层const的定义，而p没有
p2 = p3; //正确：p2和p3都是底层
const p2 = &i; // 正确：int*能转换成 
const int*int &r = ci; //错误：普通的int&不能绑定到int常量上
const int &r2 = i; //正确：const int&可以绑定到一个普通in
```

#### 2.52 constexpr限定符

在讲解constexpr限定符之前，我们需要先弄清楚常量表达式的概念。

##### 2.521 常量表达式

常量表达式，是指值不会改变并且在编译过程中就能得到计算结果的表达式。显然，字面值属于常量表达式。

> 常量表达式不能是任何的声明或定义形式的表达式。

常量表达式的值要在编译过程中就能得到结果，指的是以下几种情况：
1. 常量表达式中参与常量表达式结果运算的子表达式必须要能在编译过程中就能得到结果，而不能要在程序运行时才能获得。
2. 常量表达式中不参与常量表达式结果运算的子表达式要在该常量表达式所对应的语句执行之前就能得到结果。

```c++
// 非常量的int变量
int ins = 85;
// 正确："literals"是字面值，不需要在该表达式运行时才能有结果。cstexpr_ins值为8。
constexpr int cstexpr_ins = ("literals", 8);
// 正确：ins是非常量的int变量，ins的值在该定义语句之前就已经获得。cstexpr_ins2值为15。
constexpr int cstexpr_ins2 = (ins, 15);
// 错误：表达式ins = 18需要在该定义语句运行时才能得到结果。
constexpr int cstexpr_ins3 = (ins = 18, 3);
// 错误：表达式ins + 18需要在该定义语句运行时才能得到结果。
constexpr int cstexpr_ins4 = (ins + 15, 6);
```

常量和常量表达式的概念不一样：
* 常量是指字面值和由const或constexpr类型说明符修饰的变量。
* 常量表达式是指
  * 满足常量表达式条件的表达式。
  * 字面值。
  * 由const或constexpr类型说明符修饰的变量，且该变量的初始值也是常量表达式。

> 所以可知==常量不一定是常量表达式，但常量表达式一定是常量==。

一个对象（或表达式）是不是常量表达式由它的数据类型和初始值共同决定的。

```c++
const int max_files = 20; // max_files 是常量表达式
const int limit = max_files + 1; // limit 是常量表达式
int staff_size = 27; // staff_size 不是常量表达式，由于它的数据类型只是一个普通int而非const，所以它不属于常量表达式
const int sz = get_size(); // sz 不是常量表达式。尽管sz本身是一个常量，但它的具体值直到运行时才能获取到，所以也不是常量表达式。
```

##### 2.522 constexpr变量

C++11新标准规定，允许将变量声明为constexpr类型以便由编译器来验证变量的值是否是一个常量表达式。

> 类型修饰符constexpr只作用于变量，但是是放置在类型的左边或右边。constexpr修饰的是该定义语句的所有定义的变量而不是某一个，所以一个语句中只需在开头标注就行。

###### 2.5221 constexpr变量的定义和初始化

声明为constexpr的变量的类型一定要是字面值类型，而且必须用常量表达式初始化，所以==constexpr变量一定是常量表达式==。

> 和const变量一样，==定义的constexpr变量也必须显式初始化==，且constexpr变量不能用extern只声明而不定义。

```c++
constexpr int mf = 20; // 20 是常量表达式
constexpr int limit = mf + 1; // mf + 1 是常量表达式
constexpr int sz = size(); // 只有当 size 是一个constexpr 函数时，才是一条正确的声明
```

> 对于类型修饰符constexpr来说，不存在像const一样有着顶层底层之分，constexpr修饰指针和引用时，只会作用于它们本身，不会影响到它们所指的对象

```c++
const int *p = nullptr; // p是一个指向整型常量的指针
constexpr int *q = nullptr; // q是一个指向整数的常量指针
```

> 尽管指针和引用都能定义成constexpr，但它们的初始值却受到严格限制。
> 一个constexpr指针的初始值必须是空指针，或者是非动态存储区的对象。
> constexpr引用也是一样，只能绑定非动态存储区的对象。

###### 2.5222 字面值类型简述

常量表达式的值需要在编译时就得到计算，因此对声明constexpr时用到的类型必须有所限制。因为这些类型一般比较简单，值也显而易见、容易得到，就把它们称为“字面值类型”。

之前所说的算术类型、引用、指针和枚举类型都属于字面值类型，除此之外，类类型中的字面值常量类也属于字面值类型，其他的类型都不是字面值类型。

###### 2.5223 constexpr函数简述

尽管不能使用普通函数作为constexpr变量的初始值，但新标准允许定义一种特殊的constexpr函数。这种函数应该足够简单以使得编译时就可以计算其结果，这样就能用constexpr函数去初始化constexpr变量了。

### 2.6 类型别名

当有时候我们所要声明或定义的类型比较长或者复杂时，为了使用时简单明了、易于理解和使用，我们可以定义类型别名。

类型别名(type alias)是一个名字，它是某种类型的同义词。

类型别名可用于单一类型，也可用于类型加多个类型修饰符的复杂类型。

> 不能定义auto和decltype的类型别名

有两种方法可用于定义类型别名：
* 使用关键字 typedef
* 使用关键字 using (该方法也叫做别名声明(alias declaration))

**typedef定义类型别名**

使用关键字typedef的语句形式为
> typedef (可选 类型修饰符) 类型说明符 (可选 类型修饰符)别名1, (可选 类型修饰符)别名2, ...;

```c++
typedef double wages; //wages是double的同义词
typedef wages base, *p; //base是double的同义词，p是double*的同义词
```

和普通声明语句一样，关键字typedef的语句可看做为一种声明语句，*关键字typedef看作为数据类型的一部分*，但此声明语句中的定义的是类型别名而不是变量。

> 类型修饰符放置的位置和用于普通声明语句一样，放在该放的位置。typedef可以放在声明语句最前面，也可以紧跟在类型说明符的前面。

```c++
// 这两个语句等价，rt1，rt2是const int&的同义词。
// pt1，pt2是const int *const的同义词。
const typedef int &rt1, *const pt1;
typedef const int &rt2, *const pt2;
```

**别名声明定义类型别名**

使用关键字using的语句形式为
> using 别名 = 类型说明符(可选 类型修饰符);

```c++
using SI = Sales_item; // SI是Sales_item的同义词
using ciptr = const int *const;  // ciptr是const int *const的同义词
```

> 类型修饰符放置的位置和用于普通声明语句一样，放在该放的位置。

> 别名声明和typedef的形式不一样，一个别名声明语句只能定义一个别名。

#### 2.61 类型别名的使用

类型别名和类型的名字等价，只要是类型的名字能出现的地方，就能使用类型别名: 

```c++
wages hourly, weekly; // 等价于 double hourly、weekly。
SI item; // 等价于 Sales_item item。
```

> 当用类型别名声明语句定义了一个类型别名时，该类型别名里的所有东西组成了一个基本类型，类型别名里的类型修饰符也是这个基本类型的一部分。要注意当该类型别名用作的类型别名声明语句时的情况：

```c++
typedef char *pstring;
const pstring cstr = 0; // cstr是指向char的常量指针
const pstring *ps; // ps是一个指针，它的对象是指向char的常量指针
const char *cstr = 0; // 是对const pstring cstr的错误理解
```

### 2.7 类型自动推断

当我们需要定义一个某表达式的类型的变量，但无法得知或很难得知其类型时，我们可以定义一种能让编译器自动分析表达式所属的类型的变量。

类型自动推断有两种方法
* auto类型说明符
* decltype类型说明符

#### 2.71 auto类型说明符

通过定义auto类型的变量，可以让编译器通过分析其变量的初始值所属的类型自动决定该变量的类型。显然，auto定义的变量必须显式初始化。

auto类型的声明形式为：
> auto(可含类型修饰符) 变量名 (可选 初始化)

```c++
//由vail和val2相加的结果可以推断出item的类型
auto item = vail + val2; // item初始化为vail和val2相加的结果
```

和普通定义语句一样，auto定义语句也能在一条语句中声明多个变量。auto以第一个变量的初始值类型来决定变量的类型。
因为一条声明语句只能有一个基本类型，因此该语句中所有变量的类型都一样。所以每个变量的初始值的类型必须严格一致(能隐式转换也不行)：

```c++
auto i = 0, *p = &i; //正确：i是整数、p是整型指针
auto sz = 0, pi = 3.14; // 错误：sz 和 pi 的类型不一致
```

**常量和auto的关系**

当常量用来初始化auto类型的变量时，编译器推断出来的类型和初始值的类型并不完全一样。

> auto默认情况下会忽略掉顶层const，所以希望推断出的类型是一个顶层const，则需要明确指定为const。

> 当初始化的值为左值时，auto会保留底层const；为右值时，auto会忽略掉底层const。

```c++
const int ci = i, &cr = ci;
auto b = ci; // b是一个整数（ci的顶层const特性被忽略掉了）
auto c = cr; // c是一个整数（cr是ci的别名，ci本身是一个顶层const） 
auto d = &i; // d是一个整型指针（整数的地址就是指向整数的指针）
auto e = &ci; // e是一个指向整数常量的指针（对常量对象取地址是一种底层const）
const auto f = ci; // ci的推演类型是int, f是const int
auto &g = ci; // g是一个整型常量引用，绑定到ci
auto &h = 42; //错误：不能为非常量引用绑定字面值
const auto &j = 42; //正确：可以为常量引用绑定字面值
```

#### 2.72 decltype类型说明符

有时会遇到这种情况：希望从表达式的类型推断出要定义的变量的类型，但是不想用该表达式的值初始化变量。
此时可以用decltype类型说明符，它的作用是返回非声明或定义形式的表达式(右值)的操作数的数据类型

> decltype类型说明符的括号内只能放表达式，不能含有类型名。

decltype类型的声明形式为：
> (可含类型修饰符)decltype(表达式) 变量名 (可选 初始化)

```c++
decltype(f()) sum = x; // sum的类型就是函数f的返回类型。
```

当decltype的括号内是一个单一变量时，则decltype返回该变量的类型(包括顶层const和引用在内)：

```c++
const int ci = 0, &cj 
decltype(ci) x = 0;
decltype(cj) y = x; 
decltype(cj) z;
// x的类型是const int
// y的类型是const int&, y绑定到变量x 
//错误：z是一个引用，必须初始化
```

如果decltype的括号内是一个复杂表达式，那么decltype返回该表达式结果会有一些不同：
* 当==表达式==的结果==是右值==时==decltype返回==该表达式结果==对应的类型==。
* 如果==是左值==时，decltype返回该表达式结果==对应的类型的引用==。

```c++
// decltype的结果可以是引用类型
int i = 42, *p = &i, &r = i;
decltype (r + 0) b; //正确：加法的结果是int，因此b是一个(未初始化的)int
decltype (*p) c; //错误：c是int&,必须初始化
```

> 当decltype的括号内是一个变量名加上了一对(或多对)括号时，该表达式的结果也是左值，所以decltype就会返回该变量类型的引用类型。

```c++
// decltype的表达式如果是加上了括号的变量，结果将是引用
//错误：d1，d2是int&,必须初始化
decltype((i)) d1;
decltype(((i))) d2; 
decltype(i) e; //正确：e是一个(未初始化的)int。
```

当类型修饰符与decltype类型说明符一起用时，这些类型修饰符都会视为修饰decltype所返回的类型。

```c++
int ins = 15;
int *p = &ins;
int &lr = ins;
// 等价于const int var1
const decltype(ins) var1;
// 等价于int *const &var2
const decltype(p) &var2;
// 因为引用不是对象，引用本身不能为const\
所以等价于int &var3
const decltype(lr) var3;
// 等价于const int &var4
const decltype(ins) &var4;
```

### 2.8 引用折叠

之前我们讲述过关于引用的各种规则。

尤其是以下这两个规则：
* 不能定义引用的引用。
* 左值非常量引用只能绑定到左值，而左值常量引用还能绑定到左值；右值引用只能绑定到右值。

对于普通情况来说，这两个规则是适用的，但是，C++语言在该 
绑定规则之外还定义了两个例外，这两种例外会允许某些特殊的绑定：
* 我们可以间接地定义一个引用的引用。
* 如果间接定义了一个引用的引用，则这些引用会进行折叠。


以下是这两种例外的说明：
1. 间接定义一个引用的引用有以下几种方法：
   * 通过使用类型別名
   * 通过使用类型自动推断
     * auto类型说明符
     * decltype类型说明符
     * 某些模板形参
2. 引用折叠会将该对象的所有引用折叠成普通的引用，根据引用的类型，折叠情况分为两种(其中X代表类型说明符和其他的类型修饰符)：
   * 以下这三种情况都会折叠成左值引用`X&`：  
     * `X& &`
     * `X& &&`
     * `X&& &`
   * 以下这一种情况会折叠成右值引用`X&&`：  
     * `X&& &&`

```c++
int ins = 15;
typedef int& int_r;
using int_rr = int&&;

// int_r_r等价于
// int& &，折叠成
// int&
typedef int_r& int_r_r;
// int_r_rr等价于
// int& &&，折叠成
// int&
typedef int_r&& int_r_rr;
// int_rr_r等价于
// int&& &，折叠成
// int&
using int_rr_r = int_rr&;
// int_rr_rr等价于
// int&& &&，折叠成
// int&&
using int_rr_rr = int_rr&&;
// 等价于int &var
int_r_r var = ins;
// 等价于int &var1
int_r_rr var1 = ins;
// 等价于int &var2
int_rr_r var2 = ins;
// 等价于int &&var3
int_rr_rr var3 = 15;
```
```c++
int ins = 15;
int &lr = ins;
int &&rr = 15;
// 等价于int& &&var
// 折叠成int& var
auto &&var = ins;
// 等价于int& &var1
// 折叠成int& var1
decltype(lr) &var1 = ins;
// 等价于int& &&var2
// 折叠成int& var2
decltype(lr) &&var2 = ins;
// 等价于int&& &var3
// 折叠成int& var3
decltype(rr) &var3 = ins;
// 等价于int&& &&var4
// 折叠成int&& var4
decltype(rr) &&var4 = 15;
```
```c++
template <class ty>
void prints(ty&& val) {}
int ins = 15;
// val的类型等价于int& &&
// 折叠成int&
prints(ins);
```

这个规则能够得出一个重要的结论，也就是我们能够使用间接方式的右值类型变量来接受任意的左右值属性对象。

