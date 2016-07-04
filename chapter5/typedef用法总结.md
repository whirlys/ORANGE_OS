# [关于typedef的用法总结](http://www.cnblogs.com/csyisong/archive/2009/01/09/1372363.html)


    不管实在C还是C++代码中，typedef这个词都不少见，当然出现频率较高的还是在C代码中。typedef与#define有些相似，但更多的是不同，特别是在一些复杂的用法上，就完全不同了，看了网上一些C/C++的学习者的博客，其中有一篇关于typedef的总结还是很不错，由于总结的很好，我就不加修改的引用过来了，以下是引用的内容(红色部分是我自己写的内容)。

用途一：

## 定义一种类型的别名，而不只是简单的宏替换。可以用作同时声明指针型的多个对象。比如：

char* pa, pb; // 这多数不符合我们的意图，它只声明了一个指向字符变量的指针，

// 和一个字符变量；

以下则可行：

typedef char* PCHAR;

PCHAR pa, pb;  

这种用法很有用，特别是char* pa, pb的定义，初学者往往认为是定义了两个字符型指针，其实不是，而用typedef char* PCHAR就不会出现这样的问题，减少了错误的发生。

用途二:
## 用在旧的C代码中，帮助struct。以前的代码中，声明struct新对象时，必须要带上struct，即形式为： struct 结构名对象名，如：

struct tagPOINT1

 {
    int x;

    int y; 
};

struct tagPOINT1 p1;

而在C++中，则可以直接写：结构名对象名，即：tagPOINT1 p1;

typedef struct tagPOINT
{
    int x;

    int y;
}POINT;

POINT p1; // 这样就比原来的方式少写了一个struct，比较省事，尤其在大量使用的时

候,或许，在C++中，typedef的这种用途二不是很大，但是理解了它，对掌握以前的旧代

码还是有帮助的，毕竟我们在项目中有可能会遇到较早些年代遗留下来的代码。

用途三：

## 用typedef来定义与平台无关的类型。

比如定义一个叫 REAL 的浮点类型，在目标平台一上，让它表示最高精度的类型为：

typedef long double REAL;

在不支持 long double 的平台二上，改为：

typedef double REAL;

在连 double 都不支持的平台三上，改为：

typedef float REAL;

也就是说，当跨平台时，只要改下 typedef 本身就行，不用对其他源码做任何修改。

标准库就广泛使用了这个技巧，比如size_t。另外，因为typedef是定义了一种类型的新别名，不是简单的字符串替换，所以它比宏来得稳健。
     这个优点在我们写代码的过程中可以减少不少代码量哦！

用途四：

## 为复杂的声明定义一个新的简单的别名。方法是：在原来的声明里逐步用别名替换一部

分复杂声明，如此循环，把带变量名的部分留到最后替换，得到的就是原声明的最简化

版。举例： 

 原声明：void (*b[10]) (void (*)());

变量名为b，先替换右边部分括号里的，pFunParam为别名一：

typedef void (*pFunParam)();

再替换左边的变量b，pFunx为别名二：

typedef void (*pFunx)(pFunParam);

原声明的最简化版：

pFunx b[10];
 
原声明：doube(*)() (*e)[9];

变量名为e，先替换左边部分，pFuny为别名一：

typedef double(*pFuny)();

再替换右边的变量e，pFunParamy为别名二

typedef pFuny (*pFunParamy)[9];

原声明的最简化版：

pFunParamy e;

理解复杂声明可用的“右左法则”：从变量名看起，先往右，再往左，碰到一个圆括号

就调转阅读的方向；括号内分析完就跳出括号，还是按先右后左的顺序，如此循环，直

到整个声明分析完。举例：

int (*func)(int *p);

首先找到变量名func，外面有一对圆括号，而且左边是一个*号，这说明func是一个指针

；然后跳出这个圆括号，先看右边，又遇到圆括号，这说明(*func)是一个函数，所以

func是一个指向这类函数的指针，即函数指针，这类函数具有int*类型的形参，返回值

类型是int。

int (*func[5])(int *);

func右边是一个[]运算符，说明func是具有5个元素的数组；func的左边有一个*，说明

func的元素是指针（注意这里的*不是修饰func，而是修饰func[5]的，原因是[]运算符

优先级比*高，func先跟[]结合）。跳出这个括号，看右边，又遇到圆括号，说明func数

组的元素是函数类型的指针，它指向的函数具有int*类型的形参，返回值类型为int。

这种用法是比较复杂的，出现的频率也不少，往往在看到这样的用法却不能理解，相信以上的解释能有所帮助。

*****以上为参考部分，以下为本人领悟部分*****

使用示例：

1.比较一：
```
#include <iostream>

using namespace std;

typedef int (*A) (char, char);

int ss(char a, char b)
{
    cout<<"功能1"<<endl;

    cout<<a<<endl;

    cout<<b<<endl;

    return 0;
}
 
int bb(char a, char b)
{

    cout<<"功能2"<<endl;

    cout<<b<<endl;

    cout<<a<<endl;

    return 0;

}

void main()
{

    A a;

    a = ss;

    a('a','b');

    a = bb;

    a('a', 'b');
}
```
2.比较二：
```
typedef int (A) (char, char);

void main()
{

    A *a;

    a = ss;

    a('a','b');

    a = bb;

    a('a','b');
}
 
```
两个程序的结果都一样：

功能1

a

b

功能2

b

a

 

*****以下是参考部分*****

参考自：http://blog.hc360.com/portal/personShowArticle.do?articleId=57527

## typedef 与 #define的区别：

案例一：

通常讲，typedef要比#define要好，特别是在有指针的场合。请看例子：

typedef char *pStr1;

#define pStr2 char *;

pStr1 s1, s2;

pStr2 s3, s4;
在上述的变量定义中，s1、s2、s3都被定义为char *，而s4则定义成了char，不是我们

所预期的指针变量，根本原因就在于#define只是简单的字符串替换而typedef则是为一

个类型起新名字。

案例二：

下面的代码中编译器会报一个错误，你知道是哪个语句错了吗？

typedef char * pStr;

char string[4] = "abc";

const char *p1 = string;

const pStr p2 = string;

p1++;

p2++;

　　是p2++出错了。这个问题再一次提醒我们：typedef和#define不同，它不是简单的

文本替换。上述代码中const pStr p2并不等于const char * p2。const pStr p2和

const long x本质上没有区别，都是对变量进行只读限制，只不过此处变量p2的数据类

型是我们自己定义的而不是系统固有类型而已。因此，const pStr p2的含义是：限定数

据类型为char *的变量p2为只读，因此p2++错误。虽然作者在这里已经解释得很清楚了，可我在这个地方仍然还是糊涂的，真的希望哪位高手能帮忙指点一下，特别是这一句“只不过此处变量p2的数据类型是我们自己定义的而不是系统固有类型而已”，难道自己定义的类型前面用const修饰后，就不能执行更改运算，而系统定义的类型却可以？
