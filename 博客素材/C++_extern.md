# extern



## 作用

1) 当它与"**C**"一起连用时，如: extern "C" void fun(int a, int b);则告诉编译器在编译fun这个函数名时按着C的规则去翻译相应的函数名而不是C++的规则，C++的规则在翻译这个函数名时会把fun这个名字变得面目全非，可能是fun@aBc_int_int#%$也可能是别的，不同的编译器采用的方法不一样，因为C++支持函数的重载
   
2) 当它与**变量**或者**函数**连用时，如在头文件中: extern int g_Int; 它的作用就是标示**变量**或者**函数**的定义在别的文件中，提示编译器遇到此变量和函数时在其他模块/文件中寻找其定义。

当使用到全局变量时，这个关键字会很有用。假设你在一个头文件中声明了全局变量的存在extern int x; 以便包含这个头文件的每个源文件都知道这个全局变量的存在，但是这个全局变量的定义可以只存在一个源文件中：int x = 0;

使用`extern int x;`告诉编译器有一个int类型的对象存在于某个头文件中。知道存在的位置并不是编译器的工作，它只需要知道这个“全局变量”的类型和名称，以便知道如何使用它。一旦编译完所有源文件，链接器将会把在所有包含了这个头文件的源文件中关于x的引用都指向那个定义过x的那个源文件中x的引用。

> 记住它是一个声明不是定义!extern 并不分配空间  而这个查找的过程是在链接的过程中进行的，因此在编译阶段即使找不到这个变量或函数也不会报错。


## 1、栗子

（1）无extern声明，有变量定义。

stdafx.cpp

```C++
#include "stdafx.h"
int g_test;
```

main.cpp

```C++
int _tmain(int argc, _TCHAR* argv[])
{
	g_test=2;
	std::cout<<g_test;
	return 0;
}
```

**提示错误**，因为此时main中不能调用到g_test。

1> error C2065: “g_test”: 未声明的标识符

1> error C2065: “g_test”: 未声明的标识符


（2）有extern声明，无变量定义。

stdafx.h

```C++
extern int g_test;
```

main.cpp

```C++
int _tmain(int argc, _TCHAR* argv[])
{
	g_test=2;
	std::cout<<g_test;
	return 0;
}
```

**提示错误**，因为extern只是声明，并没有定义，也就没有空间的分配，而这时一个链接错误，编译的时候没办法发现。

1> TestCpp.obj : error LNK2001: 无法解析的外部符号 "int g_test" (?g_test@@3HA)
1> Debug\TestCpp.exe : fatal error LNK1120: 1 个无法解析的外部命令



（3）有extern声明，也有变量定义。

stdafx.h

```C++
extern int g_test;
```

stdafx.cpp

```C++
#include "stdafx.h"
int g_test;
```

main.cpp

```C++
#include "stdafx.h"
int _tmain(int argc, _TCHAR* argv[])
{
	g_test=2;
	std::cout<<g_test;
	return 0;
}
```

**程序执行正常**。

## 2、栗子

历史遗留问题，最早的标准C编译器编译引用出来的变量和函数就是在名字前面加个下滑线，比如void foo(int a,int b);编译器引用的函数名是_foo，后来的C编译器都是遵循这个标准。

可是后来C++出现了，他的**重载特性**使得不同的编译器对它进行了不同的处理，比如void foo(int a,int b);就可能被编译器引用出来后变为_foo_int_int之类的，把函参也放进函数名中，从而实现了重载。为了保证不同厂家生产的模块之间的兼容性，并且可以调用原先开发好的C模块，就引出了一个extern "C"的方法，通过这种定义来告诉编译器：请按照C的方法来对我这个函数进行编译，保持我的名称。这样不同厂家的C和C++中的变量，函数，类就得到了一致化的处理，兼容性也就解决了。
