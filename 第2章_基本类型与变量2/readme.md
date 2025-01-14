### 2.4 复合类型

复合类型(compound type)是指基于其他类型定义的类型。C++语言有以下几种复合类型:

* 复合类型
  * 数组类型([])
  * 引用类型
    * 左值引用(&)
    * 右值引用(&&)
  * 指针类型(*)
    * 普通指针
    * 与数组有关的指针
    * 函数指针
    * 类成员指针

在变量的声明语句中，一条声明语句由一个类型说明符和紧随其后的一个声明符列表组成。

>类型说明符(type specifier)就是指的是类型的名字

>声明符(declarator)是声明的一部分，包括了被声明的变量名和类型修饰符。
每个声明符命名了一个变量并指定该变量为与基本数据类型有关的某种类型。
声明语句中可以没有类型修饰符。

>类型修饰符也是声明的一部分，用来说明该变量的一些其他特性，如&, *, const等。
每种类型修饰符所作用的实体(作用于类型或者变量)和放置的位置(放置在所作用的实体的左右)有所不同。
有些类型修饰符只作用于变量，如&, *(只能放在变量的左边)。
有些类型修饰符既能作用于类型也能作用于变量，如const。
某些类型修饰符可以同时存在于一个声明语句中。
当某类型修饰符作用于变量时，是对其作用的那个变量起作用，并不能影响到其他变量，所以要注意在一个声明语句中存在多个变量声明的情况。

>面对一条比较复杂的有多个类型修饰符的声明语句时，*要根据每个类型修饰符的修饰实体与放置位置，从里到外(有圆括号从圆括号内读)，从右向左开始阅读，离修饰实体最近的符号对变量的类型有最直接的影响*，这样有助于弄清楚它的真实含义。
如：
> ```int *(&arry)[10] = ptrs; //arry是数组的引用，该数组含有10个指针```

>复合类型变量的初始化方式与其他类型变量一样，可以直接初始化和拷贝初始化。

#### 2.41 容器

在介绍复合类型中的数组类型之前，我们首先要介绍一下容器的概念。

##### 2.411 容器的介绍

容器也就是容纳对象的集合，定义了一个容器对象就是定义了一个能容纳对象的集合。

普通的对象存储的是单个的数据，而容器类对象则是存储着多个数据的对象，且每个数据的类型都相同。

容器类对象中的每个元素可以看做为一个未命名的对象。我们可以对这些对象进行访问。

##### 2.412 容器的性质

容器类对象作为容纳对象的集合，有两种属性：
* 容量可变性
* 有序性

容量可变性：
是指容器类对象在定义后是否可以改变容纳对象的数目。

> 比如定义了一个能容纳5个元素的容器类对象，当想同时存5个以上的元素时，如果该容器对象支持这种此操作，那么其就具备容量可变性，反之不具备

有序性：
是指容器类对象的元素是否是以某种规律所排列存放的，比如按元素的大小排列。

对于有有序性的容器来说，我们可以按照其位置顺序来进行元素访问

##### 2.413 容器元素的访问

每个元素在容器中都有其存放位置的，我们可以通过其位置来访问该元素。

大多数容器都提供两种访问操作，对于标准库里的容器还提供了叫做迭代器的指针的访问操作：
* 下标运算符
* 指向容器元素的指针
  * 迭代器

通过迭代器的元素访问操作我们之后会在介绍标准库容器时进行介绍。

###### 2.4131 下标运算符的元素访问

大多数容器都提供一种用下标运算符来访问其元素的操作，形式为
> 容器对象名[索引]

> [ ]为下标运算符，下标运算符中必须要指明所要访问的元素的位置编号(也就是索引)。通过这种形式就能访问容器对象中的对应元素了。

在使用下标运算符来访问元素时，要注意==每个容器对象的索引范围==，使用的索引必须要在范围内，否则会出现未定义行为。

大多数容器的索引是从0开始的，且该容器的最大索引值比该容器对象的容量值小1。以一个包含10个元素的数组为例，它的索引从0到9,而非从1到10。

> 下标运算符([])为一元运算符，运算对象在中间。
> 运算对象为右值，运算结果为左值

```c++
int a[] = {3, 4, 2}; //含有3个整数的数组
cout << a[0];  //输出首元素(索引为0的元素)的值，为3。
a[0] = 8;
cout << a[0];  //首元素的值被改为8，所以输出8。
```

###### 2.4132 指向容器元素的指针的元素访问

大多数指针不支持容器的元素访问，只有指向容器元素的指针才能够进行元素访问。

指向容器元素的指针和普通指针一样，都需要用地址来初始化或赋值才能使用。之后会介绍指针的相关信息。

对于容器元素访问，指向容器元素的指针既可以像普通指针一样使用解引用来访问，也可以使用下标运算符来访问。

**1. 使用下标运算符来访问**

指向容器元素的指针的下标访问操作和容器的下标元素访问操作一样，使用时将该指针放在容器名的位置上就行，该操作也遵循各种容器下标访问的规则。

```c++
#include <iostream>
using namespace std;
int main()
{
    // 含有5个元素的数组arr
    int arr[5] = {3,8,6,78,35};
    // 指向数组arr元素的指针p
    int *p = arr;
    // 使用p的下标访问操作
    // 输出6
    cout << p[2] << endl;
    // 使用p的下标访问操作来修改arr的元素
    p[3] = 43;
    // 输出43
    cout << arr[3] << endl;
    return 0;
}
```

**2. 使用算术和解引用运算符来访问**

指向容器元素的指针与普通指针的另一个区别就是：指向容器元素的指针还可以与整型类型的字面值或者变量做一些算术和赋值运算，用于便捷的访问该容器内的元素：
* 加法运算符
  * 指针 + 整型
* 减法运算符
  * 指针 - 整型
  * 指针 - 指针
* 递增运算符
  * 前置版本：++指针
  * 后置版本：指针++
* 递减运算符
  * 前置版本：--指针
  * 后置版本：指针--

***指针与整型的加减：***

指针与整型的加减是指该指针向前或者向后移动给定整型值的单位的元素位置：

比如一个指针a指向容器的第3个元素，则a+3是指该指针a向后移动到指向容器的第6个元素；a-3是指该指针a向前移动到指向容器的第0个元素。

***指针与指针的加减：***

指针与指针的加减只有这两个指针在指向同一容器的元素才有意义。

指针与指针的加减是指这两个指针之间的元素数量的相对间隔：

比如指针a指向容器的第3个元素，指针b指向容器的第6个元素。
则a-b的值为-3，指的是指针a在指针b前面，且中间间隔3个元素；同样的，b-a的值为3，指的是指针b在指针a后面，且中间间隔3个元素。

***递增递减运算符：***

递增递减运算符和加法减法差不多：
* 前置版本(++指针/--指针)的运算符
  首先将运算对象加1(或减1)，然后将改变后的对象作为求值结果。
* 后置版本(指针++/指针--)的运算符
  首先将运算对象加1(或减1)，但是求值结果是运算对象改变之前那个值的副本(右值)。

```c++
// 以数组容器为例:\
建立一个5个元素为int的数组array，一个指针指向数组array的首元素，一个指向数组array的尾元素。
int array[5] = {3,5,8,9,0};
int *p_head = array;
int *p_tail = &array[4];
// 输出4，指p_tail和p_head之间间隔4个元素。
cout << p_tail - p_head << "\n";
// 输出3 8，分别指向数组array的第1和第3个元素。
cout << *p_head << " " << *(p_head+2) << "\n";
// 输出0 8，分别指向数组array的第5和第3个元素。
cout << *p_tail << " " << *(p_tail-2) << "\n";
// 输出5 5，指向数组array的第2个元素。
cout << *++p_head << " ";
cout << *p_head++ << "\n";
// 输出9 9，指向数组array的第4个元素
cout << *--p_tail << " ";
cout << *p_tail-- << "\n";
```

```c++
// 以标准库的vector容器为例:\
建立一个5个元素为int的vector容器v1，一个指针指向v1的首元素，一个指向数组v1的尾元素。
vector<int> v1 = {3,5,8,9,0};
int *p_head = &v1[0];
int *p_tail = &v1[4];
// 输出4，指p_tail和p_head之间间隔4个元素。
cout << p_tail - p_head << "\n";
// 输出3 8，分别指向v1的第1和第3个元素。
cout << *p_head << " " << *(p_head+2) << "\n";
// 输出0 8，分别指向v1的第5和第3个元素。
cout << *p_tail << " " << *(p_tail-2) << "\n";
// 输出5 5，指向v1的第2个元素。
cout << *++p_head << " ";
cout << *p_head++ << "\n";
// 输出9 9，指向v1的第4个元素
cout << *--p_tail << " ";
cout << *p_tail-- << "\n";
```

#### 2.42 数组类型

数组是一种复合类型，数组可以看成是一种容器。

##### 2.421 数组的定义

数组作为一种容量不可变的有序容器，不能随意向数组中增删元素。因为数组的大小固定。

>数组类型修饰符[ ]只作用于变量，且放置在变量的右边。

定义数组类变量的语句形式为：
>基本类型(类型说明符) 变量名\[容量大小] (可选: 初始化);

```c++
unsigned ent = 42; //不是常量表达式
constexpr unsigned sz = 42;  // 常量表达式，
int arr [10] ; //含有10个整数的数组
int *parr [sz] ; //含有42个整型指针的数组
string bad[cnt] ; //错误：ent不是常量表达式
string strs [get_size()] ; // 当 get_size是 constexpr时正确，否则错误
```

定义数组的时候必须指定数组的类型，不允许用auto关键字由初始值的列表推断类型。
因为数组的元素应为对象，因此不存在引用的数组。

> 数组容量的大小是放在数组类型修饰符[ ]中的，因为容量的大小也是属于数组类型的一部分，所以必须要为常量表达式。

数组初始化只能用列表初始化的形式，如果数组没有被显式初始化，则数组的元素被默认初始化。

> 当数组被显式初始化时，可以省略其容量大小，编译器会根据初始值的数量计算并推测出来；否则必须指定其容量大小。
> 当指明了数组的容量大小时，初始化该数组的初始值数量不能超出指定的大小。如果指明的容量大小比提供的初始值数量大，则用提供的初始值初始化靠前的元素，剩下的元素执行值初始化。

```c++
const unsigned sz = 3;
int ial[sz] ={0, 1, 2}; //含有3个元素的数组，元素值分别是0,1,2
int a2[] = {0, 1, 2}; //维度是3的数组
int a3[5]= {0, 1, 2}; // 等价于 a3[] = {0, 1, 2, 0, 0}
string a4[3] ={ "hi", "bye"}; // 等价于 a4[] = {"hi", "bye", ""}
int a5[2]= {0, 1 ,2}; //错误：初始值过多
```

##### 2.412 字符数组的初始化

字符数组有一种额外的初始化语句形式，我们可以用字符串字面值对此类数组初始化。当使用这种方式时，一定要注意字符串字面值的结尾处还有一个空字符，这个空字符也会像字符串的其他字符一样被拷贝到字符数组中去：

```c++
char al[] = {'C', '+', '+'};  //列表初始化，没有空字符
char a2[] = {'C', '+', '+', '\0'}; //列表初始化，含有显式的空字符
char a3[] = "C++"; // 自动添加表示字符串结束的空字符，等价于a3[] = {'C', '+', '+', '\0'};
char a4[6] = "Daniel"; //错误：没有空间可存放空字符！
```

数组不支持拷贝，赋值和被赋值，所以不能将数组的内容拷贝给其他数组作为其初始值，也不能用数组或其他容器为其他数组赋值，同时，数组也不能作为函数的返回类型与形参类型：

```c++
int a[] = {0, 1, 2}; //含有3个整数的数组
int a2[] = a; //错误：不允许使用一个数组初始化另一个数组
a2 = a; //错误：不能把一个数组直接赋值给另一个数组
```

##### 2.413 数组元素的访问

数组和其他容器一样，可以用下标运算符或者指向数组元素的指针来访问元素。数组的索引也是从0开始的。

```c++
int a[] = {3, 4, 2}; //含有3个整数的数组
cout << a[0];  //输出首元素(索引为0的元素)的值，为3。
a[0] = 8;
cout << a[0];  //首元素的值被改为8，所以输出8。
```

> 在使用数组下标访问的时候，通常将其定义为size_t类型。size_t是一种机器相关的无符号类型，它被设计得足够大以便能表示内存中任意对象的大小。在cstddef头文件中定义了size_t类型，这个文件是C标准库stddef.h头文件的C++语言版。

```c++
// 建立一个5个元素为int的数组array，一个指针指向数组array的首元素，一个指向数组array的尾元素。
int array[5] = {3,5,8,9,0};
int *p_head = array;
int *p_tail = &array[4];
// 输出4，指p_tail和p_head之间间隔4个元素。
cout << p_tail - p_head << "\n";
// 输出3 8，分别指向数组array的第1和第3个元素。
cout << *p_head << " " << *(p_head+2) << "\n";
// 输出0 8，分别指向数组array的第5和第3个元素。
cout << *p_tail << " " << *(p_tail-2) << "\n";
// 输出5 5，指向数组array的第2个元素。
cout << *++p_head << " ";
cout << *p_head++ << "\n";
// 输出9 9，指向数组array的第4个元素
cout << *--p_tail << " ";
cout << *p_tail-- << "\n";
```

##### 2.414 多维数组

严格来说，C++语言中没有多维数组，多维数组其实是数组的数组，也可以说成是嵌套数组，当定义一个多维数组时，外层数组的每个元素也是一个数组。

一般我们常用的是二维数组，二维数组是指其元素也是数组的数组；三维数组是指其元素是二维数组的数组，以此类推。

对于二维数组来说，常把第一个维度称作行，第二个维度称作列。

当我们定义一个多维数组时，通常使用两个维度來定义它：一个维度表示数组本身的大小，另外一个维度表示其元素（也是数组）的大小：

> 实际上，定义数组时对数组类型修饰符的数量并没有限制，因此只要愿意就可以定义这样一个数组：它的元素还是数组，下一级数组的元素还是数组，再下一级数组的元素还是数组，以此类推。

```c++
int ia[3][4]; //大小为3的数组，每个元素是含有4个整数的数组

//大小为10的数组，它的每个元素都是大小为20的数组，
//这些数组的元素是含有30个整数的数组
//将这个数组的所有元素初始化为0
int arr[10][20] [30] = {0}; 
```

在第一条语句中，我们定义的名字是ia,显然ia是一个含有3个元素的数组。接着观察右边发现，ia的元素也有自己的维度，所以ia的元素本身又都是含有4个元素的数组。再观察左边知道，真正存储的元素是整数。因此最后可以明确第一条语句的含义：它定义了一个大小为3的数组，该数组的每个元素都是含有4个整数的数组。
使用同样的方式理解arr的定义。首先arr是一个大小为10的数组，它的每个元素都是大小为20的数组，这些数组的元素又都是含有30个整数的数组。

###### 2.4141 多维数组的初始化

多维数组的初始化与普通数组类似，只能用列表初始化形式，但多维数组允许每个初始化值是由花括号括起来的一组值的列表，也就是支持初始化列表的嵌套，和普通初始化列表一样，嵌套的初始化列表的初始值数量不能超过对应的容量大小。

>初始化列表的嵌套并非必需，只要满足数组初始化规则就行

```c++
int ia[3][4] = 
{
    {0, 1, 2, 3}，//第1行的初始值
    {4, 5, 6, 7}，//第2行的初始值
    {8, 9, 10, 11} //第3行的初始值
};  //三个元素，每个元素都是大小为4的数组

//没有标识每行的花括号，与ia的初始化语句是等价的
int ia2[3][4] = {0,1,2,3,4,5,6,7,8,9,10,11};
```

> 和普通数组一样，如果多维数组没有被显式初始化，则元素被默认初始化。

> 当多维数组被显式初始化时，可以省略其第一维的容量大小，后面维度的容量大小不能省略。
> 
> 编译器自动推断第一维的大小视通过后面维度的大小以及所给初始值的多少来决定的，公式为：
> ```f_size = ceil(n/sum(s_size));```
> f_size为第一维的大小，sum(s_size)为后面维度大小的总和，n为对应的初始值数量，ceil为向上取整函数。

> 当指明了多维数组的维度的容量大小时，初始化某维度的初始值数量不能超出该维度指定的大小。如果指明的容量大小比提供的初始值数量大，则用提供的初始值初始化该维度靠前的元素，该维度剩下的元素执行值初始化。

```c++
//显式地初始化每行的首元素，其他元素执行值初始化
int ia[3][4] = {{ 0 }, { 4 }, { 8 }};
//显式地初始化第1行，其他元素执行值初始化
int ix[3][4] = {0, 3, 6, 9};
```

###### 2.4142 多维数组的元素的访问

和普通数组一样，可以使用下标运算符来访问多维数组的元素，此时数组的每个维度对应一个下标运算符。
如果表达式含有的下标运算符数量和数组的维度一样多，该表达式的结果将是给定类型的元素；反之，如果表达式含有的下标运算符数量比数组的维度小，则表达式的结果将是给定索引处的一个内层数组

```c++
//用arr的首元素为ia最后一行的最后一个元素賦值
ia[2][3] = arr[0][0][0];
int (&row)[4] = ia[1] ; //把row绑定到ia的第二个容量为4的数组上
```

#### 2.43 引用类型

引用(reference)类型可以看作为某个对象起了另外一个名字。
一般在初始化变量时，初始值会被拷贝到新建的对象中。然而定义变量引用时，程序会把用来初始化该引用变量的对象与该变量绑定(bind)在一起，而不是将初始值拷贝给引用变量。

一旦初始化完成，引用将和它的初始值对象一直绑定在一起，无法解绑。
该引用也就相当于其初始值对象的别名，所以之后所有对引用的操作也就是对其初始值对象的操作。

> 注意当右值引用绑定到字面值时，编译器是根据该字面值的数据创建一个含有该数据的临时对象并绑定到该右值引用上的。所以对该右值引用的操作并不是对其绑定的字面值的操作，而是对这个临时对象的操作。

> 所以非常量的右值引用绑定后，也能进行正常的赋值等操作。
> 更广泛的说，所有非常量的左值和右值都能进行赋值等操作。

```c++
int ins = 50;
int &rint = ins;
int &&rrint = 10;
// 正确：rint绑定到ins，ins的值被改为15
rint = 15;
// 正确：rrint绑定到一个值为10的临时对象上，\
该对象的值被改为35
rrint = 35;
```

因为无法令引用重新绑定到另外一个对象，因此==定义的引用必须显式初始化==。
> 可以用extern只声明而不定义引用。

引用操作如```int &i = p;```
的原理其实就是将i这个变量的内存地址改为p变量名的内存地址值，这两个变量的地址是一样的，指向同一个实体。

>引用类型修饰符只作用于变量，且放置在变量的左边。

引用并非对象，相反的，它只是为一个已经存在的对象所起的另外一个名字，所以不能定义引用的引用。

> 无特殊说明的情况下，所有引用的类型都要和与之绑定的对象严格匹配(也就是两个变量的类型要一模一样，就算是两个能隐式转换的类型都不行)

##### 2.431 左值引用

左值引用顾名思义，也就是只能绑定在左值表达式的引用，所以我们不能将一个左值非常量引用直接绑定到一个右值上。

左值引用因为是绑定到左值上的别名，所以绑定后的左值引用也就是一个左值。

定义左值引用的语句形式为：
>基本类型(类型说明符) &变量名 (可选: 初始化);

如：
```c++
int ival = 1024; 
int &refVal = ival; // refVal指向ival (是ival的另一个名字) 
int &refVal2; //报错：引用必须被初始化
```

##### 2.432 右值引用

右值引用顾名思义，也就是只能绑定在右值表达式的引用，所以我们不能将一个右值引用直接绑定到一个左值上。

一般来说，右值是以下几种表达式：
* 临时对象
  * 类型转换完毕的表达式
  * 返回右值的表达式
  * 非引用返回类型的函数的返回值
* 字面值常量

通过右值的特性可以看出，右值引用所绑定的对象都是即将要被销毁的，它们都没有其他的用户。
所以右值引用就直接将这些对象的内存地址变为右值引用的地址，也就接管了所引用对象的资源。

右值引用绑定到右值上之后，该右值引用就接管了其右值，所以该右值就不再是临时对象了，它就有了和其他变量一样的生存期了，所以该对象就不再是右值了，而是左值。因此绑定后的右值引用也是一个左值。

定义右值引用的语句形式为：
>基本类型(类型说明符) &&变量名 (可选: 初始化);

如：
```c++
int i = 42;
int &r = i; // 正确：r引用i
int &&rr = i; // 错误：不能将一个右值引用绑定到一个左值上
int &r2 = i * 42; // 错误：i*42是一个右值
const int &r3 = i * 42; // 正确：我们可以将一个const的引用绑定到一个右值上
int &&rr2 = i * 42; // 正确：将rr2绑定到乘法结果上
```

> 注意当右值引用绑定到字面值时，编译器是根据该字面值的数据创建一个含有该数据的临时对象并绑定到该右值引用上的。所以对该右值引用的操作并不是对其绑定的字面值的操作，而是对这个临时对象的操作。

变量可以看作只有一个运算对象而没有运算符的表达式，所以变量表达式是左值，因此我们不能将一个右值引用直接绑定到一个变量上，即使这个，变量是右值引用类型或者const左值引用也不行。

```c++
int &&rrl = 42; //正确：字面常量是右值
int &&rr2 = rrl; //错误：表达式rrl是左值
```

**标准库move函数**

因为右值引用的规定，我们不能直接将左值绑定在右值变量上。但是我们可以通过一些操作，使左值变为右值临时对象，这样就能绑定在右值引用上。
在头文件utility中，我们可以通过调用一个名为move的新标准库函数来获得绑定到左值上的右值引用。

```c++
/*
move调用告诉编译器：我们有一个左值，但我们希望像一个右值一样处理它。
我们必须认识到，调用move就意味着承诺：
除了对rrl赋值或销毁它外，我们将不再使用它。在调用move之后，我们不能对移后源对象的值做任何假设。
*/
int &&rr3 = std::move(rrl); // ok
```

#### 2.44 指针类型

指针（pointer）是另外一种类型的复合类型。
之前也说过，每个对象在定义时，编译器会在程序运行时为其分配存储空间，也就是内存地址，里面存储着其对象的数据，而对象存储空间的大小则由其数据类型的尺寸所决定。每个对象的内存地址一般都由一个16进制的数字来表示。
指针所存储的，也就是初始化或赋值该指针的对象的内存地址值。所以通过指针，可以对其指向的对象进行一些操作。

>与引用类似，指针也实现了对其他对象的间接访问。
所以一个指针变量可以近似等价于一个引用。
但指针可以只声明，也可以被赋值。
```c++
int or = 8;
// 以下的定义语句近似等价
int &r = or;
int *ptr = &or;
```

根据指针指向的对象的类型的不同，指针的分类一般为以下这几种

* 指针类型(*)
    * 普通指针
      * 空指针
      * 空类型指针
      * 指向指针的指针
    * 数组指针
    * 函数指针
    * 类成员指针

>指针类型修饰符只作用于变量，且放置在变量的左边。

>无特殊说明的情况下，所有指针类型变量都要和与之绑定的对象严格匹配(也就是两个变量的类型要一模一样，就算是两个能隐式转换的类型都不行)

##### 2.441 普通指针

一般定义指针类型变量的语句形式为：

>基本类型(类型说明符) *变量名 (可选: 初始化);

```c++
int *ipl, *ip2; // ipl和ip2都是指向int型对象的指针
double dp, *dp2; // dp2是指向double型对象的指针，dp是double型对象
```

因为指针存放的是某个对象的地址，不能直接用其他非地址值来为其初始化或赋值。(在64位机器上，所有类型的指针的尺寸一般为8字节，且内存地址是以16进制的整型字面值来表示的。但指针与整型没有隐式转换，所以都是可以用显式转换将整型值作为地址值赋值给指针，但不推荐这样做)

```c++
double dval;
double *pd = &dval; //正确：初始值是double型对象的地址
//错误，不能直接用非地址值初始化指针
double *pd2 = dval;
double *pd3 = 3.6;
double *pd4 = pd;  //正确：初始值是pd指针所指向的对象的地址
```

###### 2.4411 获取对象地址

所以我们必须要获取该对象的地址，获取其地址需要使用取地址符（操作符&）:
形式为
>&对象名

> 取地址符(&)为一元运算符，运算对象在右侧。
> 运算对象为左值，运算结果为右值。
> 返回一个指向该运算对象的指针。

```c++
double dval;
double *pd = &dval; //正确：初始值是double型对象的地址
double *pd2 = pd; //正确：初始值是指向double对象的指针
int *pi = pd; //错误：指针pi的类型和pd的类型不匹配
pi = &dval; //错误：试图把double型对象的地址赋给int型指针
```

指针和引用不一样：任何指针，包括底层const指针，都不能指向任何字面值或者临时对象，指针只能指向各种变量(也就是指针只能指向左值)。

```c++
const int ins = 8;
// 正确：底层const指针能够指向const变量。
const int *pi = &ins;
// 错误：指针不能指向临时对象。
const int *pi = &(ins + 2);
// 错误：指针不能指向字面值。
const int *pi = &10;
```

###### 2.4412 访问指针指向的对象

如果指针指向了一个对象，则允许使用解引用符（操作符*）来访问该对象:
形式为
> *对象名

> 解引用符(*)为一元运算符，运算对象在右侧。
> 运算对象为右值，运算结果为左值。
> 返回一个运算对象所指向的对象的引用。

> 对指针使用解引用符时必须注意该指针确实指向了某个可见的存在的对象，否则就会出现未定义行为

```c++
int ival = 42;
int *p = &ival; // p存放着变量ival的地址，或者说p是指向变量ival的指针 
cout « *p; //由符号*得到指针p所指的对象，输出42
*p = 0; //由符号*得到指针p所指的对象，即可经由p为变量ival赋值
cout « *p; // 输出0
```

###### 2.4413 指向指针的引用

引用本身不是一个对象，因此不能定义指向引用的指针。但指针是对象，所以存在对指针的引用：

```c++
int i = 42;
int *p; // p是一个int型指针
int *&r = p; // r是一个对指针p的引用
r = &i; // r引用了一个指针，因此给r賦值&i就是令p指向i
*r = 0; // 解引用r得到i，也就是p指向的对象，将i的值改为0
```

###### 2.4414 空指针

空指针（null pointer）是指不指向任何对象的指针。
在试图使用一个指针之前代码可以首先检查它是否为空。
以下列出几个生成空指针的方法：
```c++
int *pl = nullptr; // 等价于 int *pl = 0;
int *p2 = 0; //直接将p2初始化为字面值常量0
// 需要首先#include cstdlib
int *p3 = NULL; // 等价于 int *p3 = 0;
```
把int变量直接赋给指针是错误的操作，即使int变量的值恰好等于0也不行。 
```c++
int zero = 0;
pi = zero; //错误：不能把int变量直接赋给指针
```

###### 2.4415 空类型指针

空类型指针(void\*)是一种特殊的指针类型，可用于存放任意对象的地址。
一个void\*指针存放着一个地址，这一点和其他指针类似。不同的是，我们对该地址中到底是个什么类型的对象并不了解。
因为我们并不知道这个对象到底是什么类型，也就无法确定能在这个对象上做哪些操作，所以不能直接操作void\*指针所指的对象。

```c++
double obj = 3.14, *pd = &obj;
//正确：void*能存放任意类型对象的地址
void *pv = &obj ; // obj可以是任意类型的对象
pv = pd; // pv可以存放任意类型的指针

cout << *pd;  // 正确：可以解引用double指针
cout << *pv;  // 错误：不能解引用void*指针
```

###### 2.4416 指向指针的指针

因为指针类型修饰符作用于变量，且指针类型不像引用类型，是对象，所以像其他对象一样，也有自己的地址，因此允许把指针的地址再存放到另一个指针当中。

**指向指针的指针的定义**

通过\*的个数可以区分指针的级別。也就是说，\*\*表示指向指针的指针，\*\*\*表示指向指针的指针的指针，以此类推：

```c++
int ival = 1024;
int *pi = &ival; // pi指向一个int型的数
int **ppi = &pi; // ppi指向一个int型的指针
```

下图描述了它们之间的关系。
![ptr](../image/2021-05-14-02-50-14.png)

**指向指针的指针的解引用**

解引用int型指针会得到一个int型的数，同样，解引用指向指针的指针会得到一个指针。此时为了访问最原始的那个对象，需要对指针的指针做两次解引用：

```c++
int ival = 1024;
int *pi = &ival;
int **ppi = &pi;
cout << "The value of ival\n" << "direct value: " << ival << "\n" 
<< "indirect value: " << *pi << "\n" << "doubly indirect value: " << **ppi << endl;
// 该程序使用三种不同的方式输出了变量ival的值：第一种直接输出；第二种通过int型指针pi输出；第三种两次解引用ppi,取得ival的值。
```

##### 2.442 有关数组的指针

以下是有关数组的指针分类：
* 指向数组元素的指针
* 指向数组的指针

###### 2.4421 指向数组元素的指针

通常情况下，使用取地址符来获取指向某个对象的指针，取地址符可以用于任何对象。数组的元素也是对象，对数组使用下标运算符得到该数组指定位置的元素。因此像其他对象一样，对数组的元素使用取地址符就能得到指向该元素的指针: 

```c++
string nums [] = {"one", "two", "three" }; // 数组的元素是 string 对象 
string *p = &nums [0] ; // p 指向 nums 的第一个
```

然而，数组还有一个特性：在很多用到数组名字的地方，编译器都会自动地将其替换为一个指向数组首元素的指针

```c++
string *p2 = nums; // 等价于 p2 = &nums[0]
```

> 不能用临时数组的地址来初始化或赋值指向数组元素的指针

```c++
int *ap;
ap = (int[3]){6,8,1}; // 错误，不能临时数组赋值
```

###### 2.4422 指向数组的指针

指向数组的指针所存储的并不是普通变量的地址，而是某个数组的地址，因此指向数组的指针的类型必须也是数组类型

```c++
int two_arr[][3] = {3,6,8,4,2,5,3,7,8};  // 定义二维int数组
// 正确：两种指向二维int数组的指针定义等价
int (*p)[3] = two_arr; 
int (*p2)[3] = &two_arr[0];

int (*p3)[3] = two_arr[0];  // 错误：二维int数组指针不能用int赋值
int (*p4)[] = two_arr;  // 错误：定义指向数组的指针不能省略数组的维度
int *p4[3] = two_arr;  // 错误：指针数组不能用二维int数组初始化
```

> 和普通数组一样，多维数组名默认转换为指向该数组首元素的指针，但其首元素也是数组，所以此时不能用指向数组元素的指针来保存其地址，必须要用指向该数组的指针来保存

> 和普通数组一样，不能用临时数组的地址来初始化或赋值指向数组的指针

> 定义指向数组的指针时不能省略数组的维度，且维度的数量与容量大小必须与初始化或赋值该指针的对象维度一一对应

> 定义指向数组的指针时不能缺少括号，否则就变成定义指针数组了

当我们访问指向数组的指针的最里层的元素时，需要逐层访问。所以使用指向数组的指针时也需要逐层解引用来访问

```c++
int two_arr[][3] = {3,6,8,4,2,5,3,7,8};  // 定义二维int数组
int (*p)[3] = two_arr; 
cout << **p; // 正确：输出第一行数组的首元素3
cout << *p; // 错误：不能输出指针所存的内存值
```

##### 2.443 函数指针

函数指针指向的是函数而非对象。和其他指针一样，函数指针指向某种特定的函数类型。函数的类型由它的返回类型和形参类型共同决定，与函数名无关。

```c++
//比较两个string对象的长度，该函数类型为
// bool (const string&, const string&);
bool lengthCompare(const string &, const string &);
```

**函数指针的定义**

定义一个指向某函数的指针，只需要先写出该函数的声明(函数形参名不要写)，然后用指针来替换掉函数声明中的函数名即可。
语句形式为：
> 返回类型 (*指针变量名) (形参类型1, 形参类型2, ...) (可选: 初始化);

> 指针两端的括号必不可少，如果不写这对括号，则定义的是一个返回值为指针的函数

```c++
//比较两个string对象的长度，该函数类型为
// bool (const string&, const string&);
bool lengthCompare(const string &, const string &);

// pf指向一个函数，该函数的参数是两个const string的引用，返回值是bool类型 
bool (*pf) (const string &, const string &); //未初始化

//声明一个名为pf2的函数，该函数返回bool*
bool *pf2(const string &, const string &);
```

因为函数的类型由它的返回类型和形参类型共同决定，所以当我们定义指向重载函数的指针时，必须清晰地指明到底应该定义哪个函数，也就是指针类型必须与其精确匹配(但是可以忽略顶层const)。

> 所以定义指向重载函数的指针时，不能使用auto类型和decltype(函数名)形式。

```c++
void ff(int*);
void ff(unsigned int);
void (*pf1)(unsigned int) = ff; // pf1指向ff (unsigned)

void (*pf2) (int) = ff; //错误：没有任何一个ff与该形参列表匹配
double (*pf3) (int*) = ff; //错误：ff和pf3的返回类型不匹配

// 以下都为错误：无法判断是哪个ff
auto pf4 = ff;
decltype(ff) (*pf5) = ff;
```

**函数指针的初始化和赋值**

和数组名一样，当我们把函数名作为一个值使用时，该函数会自动地转换成指针。

```c++
pf = lengthCompare; // pf 指向名为 lengthCompare 的函数
pf = &lengthCompare; //等价的赋值语句：取地址符是可选的
```

初始化和赋值某函数指针的对象的类型必须与该函数指针精确匹配，也就是返回类型和参数表类型一模一样(但是可以忽略顶层const)

> 可以用运算符函数来初始化和赋值函数指针，只要与该指针的类型精确匹配就行

```c++
// Tef和Te4是两个类类型，假设Tef永远比Te4大，定义了一个关系运算符
bool operator==(Tef tef, Te4 te4)
{ return true; }
bool (*p) (Tef, Te4) = operator==;  // 正确：类型精确匹配
```

在指向不同函数类型的指针间不存在转换规则。但是和往常一样，我们可以为函数指针赋一个nullptr或者值为0的整型常量表达式，表示该指针没有指向任何一个函数。

```c++
string::size_type sumLength(const strings, const strings);
bool cstringCompare(const char*, const char*);
pf = 0; //正确：pf不指向任何函数
pf = sumLength; //错误：返回类型不匹配
pf = cstringCompare; //错误：形参类型不匹配
pf = lengthCompare; //正确：函数和指针的类型精确匹配
```

**函数指针的使用**

函数指针是指向某函数的指针，所以可以通过函数指针来调用该函数。
当我们使用函数指针时，可以像调用函数的形式一样，直接调用该函数，而无须提前解引用。
语句形式为
> (*指针变量名)/指针变量名 实参表;

```c++
pf = lengthCompare; // pf指向名为lengthCompare的函数
pf = &lengthCompare; //等价的赋值语句：取地址符是可选的
bool bl = pf("hello", "goodbye"); //调用 lengthCompare函数
bool b2 = (*pf) ("hello", "goodbye"); // 一个等价的调用
bool b3 = lengthCompare ("hello", "goodbye"); // 另一个等价的调用
```

##### 2.444 类成员指针

成员指针(pointer to member)是指可以指向类的非静态成员的指针。

其他指针一般情况下是指向一个对象，但是成员指针指示的是类的成员，而非类的对象。所以当初始化一个这样的指针时，我们令其指向类的某个成员，但不要指定该成员所属的对象。直到使用成员指针时，才提供成员所属的对象。

类的静态成员不属于任何对象，因此无须特殊的指向静态成员的指针，指向静态成员的指针与普通指针没有什么区别。

> 成员指针的类型不仅要指明指向的成员的类型，还需指明包含该成员的类。

根据成员的类型，成员指针分为两种
* 数据成员指针
* 函数成员指针

###### 2.4441 成员指针的定义

和其他指针一样，在声明成员指针时我们也使用*来表示当前声明的名字是一个指针。与普通指针不同的是，成员指针还必须包含成员所属的类，该类不能是不完全类型。因此定义成员指针的语句形式为：

*数据成员指针定义：*

> 数据成员类型说明符(可含类型修饰符) 包含该成员的类名::*指针名(可含类型修饰符) (可选: 初始化);

```c++
// pdata是一个常量指针，可以指向一个常量（非常量）Screen对象的string成员
const string Screen::*const pdata;
```

*函数成员指针定义：*

> 函数成员的返回类型 (包含该成员的类名::*指针名(可含类型修饰符)) 函数成员的形参表 (可选 类型修饰符) (可选: 初始化);

如果成员函数是const成员或者引用成员函数，则我们必须将const限定符或引用限定符包含进来。

函数成员指针也是函数指针，指针名两端的括号必不可少

```c++
char (Screen::*pmf2)(Screen::pos, Screen::pos) const;
//错误：非成员函数p不能使用const限定符
char Screen::*p(Screen::pos, Screen::pos) const;
```

###### 2.4442 成员指针的初始化或赋值

当我们初始化一个成员指针（或者向它赋值）时，必须指定它所指的成员，而非指定一个特定的该类对象的成员。且指定的成员不能是不可访问的。

初始化或赋值成员指针的语句形式为：
> &类名::成员名;

通常来说，初始化或赋值一个数据成员指针时，成员的类型、包含该成员的类名之间都要严格匹配(但是可以忽略顶层const)。

但当成员指针所指向成员的所属的类存在继承关系时:
> 对于数据成员指针：
> 可以用该成员指针所指向成员的*所属类的基类或派生类*对该成员指针初始化或赋值，前提是指定的成员的类型严格匹配且该成员也存在并可见于指向成员的所属类中(注意派生类同名成员覆盖的问题)。
> 
> 对于函数成员指针：
> 可以用该成员指针所指向成员的*所属类的基类*对该成员指针初始化或赋值，前提是指定的成员的类型严格匹配且该成员也存在并可见于指向成员的所属类中(注意派生类同名成员覆盖的问题)。

*数据成员指针的初始化或赋值*

```c++
// pdata可以指向一个常量（非常量）Screen对象的string成员
const string Screen::*pdata;
Screen scr;
pdata = &Screen::contents; // 正确：指定了它所指的成员
const string Screen::*pdata2 = &scr.contents;  // 错误：不能指定特定的该类对象的成员

// pdat可以指向一个常量（非常量）Te2对象的int成员。
// Te是Te2的公有基类，且Te有int成员tins。
// Te3，Te4是Te2的公有派生类，Te4定义了自己的int成员tins
const int Te2::*pdat;
// 两个都正确：Te，Te3分别是Te2的基类和派生类，且这三类都有从Te继承的int成员tins。
pdata = &Te3::tins;
pdata = &Te::tins;
// 错误：Te4是Te2的派生类，虽然Te4有从Te继承的int成员tins
// 但Te4定义了自己的同名成员tins，隐藏了继承的int成员tins
// 所以编译器判断Te4的tins为Te4自己定义的那个版本
// Te2中不存在Te4定义的tins版本，因此出错
pdata = &Te4::tins;
```

*函数成员指针的初始化或赋值*

和函数指针一样：
* 函数成员指针的类型要严格匹配(但是可以忽略顶层const)。
* 成员函数名也可以隐式转换为指向该函数的指针。
* 可以用auto类型和decltype(函数名)形式自动判断类型，但是不能用于有重载的成员函数。

> 函数成员指针是否指向const成员要与其初始化或赋值一致。

函数成员指针所指的成员必须是非构造和析构函数，可以是虚函数和运算符函数。

和普通函数指针不同的是，在成员函数和指向该成员的指针之间不存在自动转换规则

```c++
char (Screen::*pmf2)(Screen::pos, Screen::pos) const;
pmf2 = &Screen::get;
```

```c++
// pmf指向一个Screen成员，该成员不接受任何实参且返回类型是char
pmf = &Screen::get; //必须显式地使用取地址运算符
pmf = Screen::get; //错误：在成员函数和指针之间不存在自动转换规则
```

```c++
// pfun可以指向一个Te2对象的成员，该成员不接受任何实参且返回类型是void。
// Te是Te2的公有基类。
// Te3，Te4是Te2的公有派生类，这四个类都定义了自己的void prr()虚函数和非虚函数void no_vir()
void (Te2::*pfun) ();
// 三个都正确，Te是Te2的公有基类。
pfun = &Te2::prr;
pfun = &Te::prr;
pfun = &Te::no_vir;
// 错误：Te3和Te4是Te2的派生类
pfun = &Te3::prr;
pfun = &Te4::no_vir;
```

###### 2.4443 成员指针的使用

必须清楚的一点是，当我们初始化一个成员指针或为成员指针赋时，该指针并没有指向任何数据。成员指针指定了成员而非该成员所属的对象，只有当需要使用成员指针时我们才提供类对象的信息。

> 和普通指针一样，成员指针必须要初始化后才能使用。

因为是类成员指针，所以我们使用成员指针时必须要先使用成员访问运算符，再使用解引用才能获得该对象的成员。

> 我们提供的类对象必须存在之前成员指针所指定的成员，并且该成员还能被访问。

成员指针解引用所得的成员，一般来说是该指针初始化或赋值阶段时指定的类中的指定成员(静态类型的对应成员)。
但当成员指针所指向成员的所属的类存在继承关系时。对于所属的类有虚函数的函数成员指针来说，如果函数成员指针之前指定的是虚函数，那么成员指针解引用所得的成员函数到底是哪个版本，(动态类型的对应成员)则要根据提供对象的类型来决定(也就是动态绑定)

> 和初始化或赋值阶段一样。通常来说，提供对象的类型要与定义成员指针指明的所属类严格匹配。
> 
> 但当成员指针所指向成员的所属的类存在继承关系时。
> 提供对象的类可以是成员指针所指向成员的*所属类的派生类*。

*数据成员指针的使用：*

使用数据成员指针的语句形式为(这两种都返回该对象成员的引用):
> 类对象名.*成员指针名;
> 或
> 指向类对象的指针名->*成员指针名;

```c++
Screen myScreen, *pScreen = &myScreen;
// .*解引用pdata以获得myScreen对象的contents成员
auto s = myScreen.*pdata;
// ->*解引用pdata以获得pScreen所指对象的contents成员
s = pScreen->*pdata;
```

*函数成员指针的使用：*

使用函数成员指针的语句形式为:
> (类对象名.*成员指针名) 实参表;
> 或
> (指向类对象的指针名->*成员指针名) 实参表;

因为是调用运算符的优先级要高于指针指向成员运算符的优先级，所以的括号必不可少

```c++
Screen myScreen, *pScreen = &myScreen;
//通过pScreen所指的对象调用pmf所指的函数
char cl = (pScreen->*pmf)();
//通过myScreen对象将实参0, 0传给含有两个形参的get函数
char c2 = (myScreen.*pmf2) (0, 0);

// 错误示范：以下这两个等价，这行代码的意思是调用一个名为pmf的函数，然后使用该函数的返回值作为指针指向成员运算符(.*)的运算对象。然而pmf并不是一个函数，因此代码将发生错误
myScreen.*pmf();
myScreen.*(pmf());
```

###### 2.4443 成员函数指针与可调用对象

可调用对象就是指某一个对象或者表达式，如果可以对其使用调用运算符，则称它为可调用对象，我们之后会详细介绍。

调用形式是由可调用对象的返回类型以及所有形参类型所组成，一种调用形式对应一个函数类型。
例如：```int(int, int)```
是一个函数类型，它接受两个int，返回一个int。

与普通的函数指针不同，成员函数指针不是一个可调用对象，这样的指针不支持函数调用运算符。
所以为了能够让成员函数指针向可调用对象一样可以通过函数调用符来进行调用，我们可以有三种方法(这几种方法都定义在functional头文件中)：
* function类
* bind函数
* mem_fn函数

要知道非静态的成员函数都有一个隐式的this形参，该形参为指向所属类对象的常量指针，当该成员函数为const成员时，该形参指向的就是常量版本。
所以要将成员函数指针转换为可调用对象时，必须要显式指出该成员函数指针的第一个形参。

但是显式指出的第一个形参不一定非要和this形参的类型一样，该形参类型可以是所属类的类型以及所属类类型的指针和引用。

function类需要显式指定成员的调用形式，且生成的可调用对象只有一种调用方式；bind函数和mem_fn函数不需要显式指定成员的调用形式，且这两种生成的可调用对象的第一个形参既能接受该成员所属类的对象，也能接受该成员所属类的对象的地址。

**function类**

之后我们会讲到function类的具体使用，现在只需知道function类是一个模板，该模板需要显式提供一个有着返回类型和形参表的调用形式，该形式指定所创建的对象能够保存哪些可调用对象的数据。

function类对象需要用一个调用形式匹配的可调用对象来初始化才能使用，该对象也能被匹配的可调用对象赋值。
function类对象的调用形式和其显式声明的一样。

```c++
struct Cls
{
    void prints() { cout << "print\n"; }
};
Cls obj;
// 生成一个函数成员指针，指向Cls类的prints函数
void (Cls::*ptr) () = Cls::prints;
// 这三种形式都正确
// f1调用形式为void (Cls)
function<void (Cls)> f1 = ptr;
// f1调用形式为void (Cls&)
function<void (Cls&)> f2 = ptr;
// f1调用形式为void (Cls*)
function<void (Cls*)> f2 = ptr;
```

**bind函数**

之后我们也会讲到bind函数的具体使用，现在只需知道bind函数可以根据给定对象来生成一个与给定对象的形参表不一样的可调用对象。
bind函数的使用形式为：
> bind(原始可调用对象, 调用原始可调用对象的实参表)

```c++
struct Cls
{
    void prints() { cout << "print\n"; }
};
Cls obj;
void (Cls::*pf) () = Cls::prints;
// 根据函数成员指针生成的可调用对象func
auto func = bind(pf, std::placeholders::_1);
// 正确：接受Cls的对象obj，输出print
func(obj);
// 正确：接受Cls的对象obj的地址，输出print
func(&obj);
```

**mem_fn函数**

mem_fn函数接受一个函数成员的地址，可以根据该函数成员的类型自动推断可调用对象的类型，而无须用户显式地指定。
mem_fn函数的使用形式为：
> mem_fn(函数成员的地址)

和auto等一样，当直接将函数成员的地址赋给mem_fn函数时，该函数成员不能有重载。

```c++
struct Cls
{
    void prints() { cout << "print\n"; }
};
Cls obj;
void (Cls::*pf) () = Cls::prints;
// 根据函数成员指针生成的可调用对象func
auto func = mem_fn(pf);
// 正确：接受Cls的对象obj，输出print
func(obj);
// 正确：接受Cls的对象obj的地址，输出print
func(&obj);
```
