---
layout: post
title: c/c++支持可变参数的函数
description: ""
category: program
analytics: true
tags: [c++, function, arg, 函数]
---

c/c++支持可变参数的函数

一、为什么要使用可变参数的函数？
一般我们编程的时候，函数中形式参数的数目通常是确定的，在调用时要依次给出与形式参数对应的所有实际参数。但在某些情况下希望函数的参数个数可以根据需 要确定，因此c语言引入可变参数函数。这也是c功能强大的一个方面，其它某些语言，比如fortran就没有这个功能。
典型的可变参数函数的例子有大家熟悉的printf()、scanf()等。
二、c/c++如何实现可变参数的函数？
为了支持可变参数函数，C语言引入新的调用协议， 即C语言调用约定 \_*cdecl . 采用C/C++语言编程的时候，默认使用这个调用约定。如果要采用其它调用约定，必须添加其它关键字声明，例如WIN32 API使用PASCAL调用约定，函数名字之前必须加*\_stdcall关键字。
采用C调用约定时，函数的参数是从右到左入栈，个数可变。由于函数体不能预先知道传进来的参数个数，因此采用本约定时必须由函数调用者负责堆栈清理。举个例子：

{% highlight cpp %}
//C调用约定函数
int __cdecl Add(int a, int b)
{
	return (a + b);
}
{% endhighlight %}

函数调用：
Add(1, 2);
//汇编代码是：
push 2 ;参数b入栈
push 1 ;参数a入栈
call @Add ;调用函数。其实还有编译器用于定位函数的表达式这里把它省略了
add esp,8 ;调用者负责清栈

如果调用函数的时候使用的调用协议和函数原型中声明的不一致，就会导致栈错误，这是另外一个话题，这里不再细说。
另外c/c++编译器采用宏的形式支持可变参数函数。这些宏包括va\_start、va\_arg和va\_end等。之所以这么做，是为了增加程序的可移植性。屏蔽不同的硬件平台造成的差异。
支持可变参数函数的所有宏都定义在stdarg.h 和 varargs.h中。例如标准ANSI形式下，这些宏的定义是：

{% highlight cpp %}
typedef char * va_list; //字符串指针
#define _INTSIZEOF(n)   ( (sizeof(n) + sizeof(int) - 1) & ~(sizeof(int) - 1) )
#define va_start(ap,v) ( ap = (va_list)&v + _INTSIZEOF(v) )
#define va_arg(ap,t)    ( *(t *)((ap += _INTSIZEOF(t)) - _INTSIZEOF(t)) )
#define va_end(ap)      ( ap = (va_list)0 )
{% endhighlight %}

使用宏\_INTSIZEOF是为了按照整数字节对齐指针，因为c调用协议下面，参数入栈都是整数字节(指针或者值)。
三、如何定义这类的函数。
可变参数函数在不同的系统下，采用不同的形式定义。
1、用ANSI标准形式时，参数个数可变的函数的原型声明是：

{% highlight cpp %}
type funcname(type para1, type para2, ...);
{% endhighlight %}

关于这个定义，有三点需要说明：
一般来说，这种形式至少需要一个普通的形式参数，可变参数就是通过三个'.'来定义的。所以"……"不表示省略，而是函数原型的一部分。type是函数返回值和形式参数的类型。
例如：

{% highlight cpp %}
int MyPrintf(char const* fmt, ...);
{% endhighlight %}

但是，我们也可以这样定义函数：

{% highlight cpp %}
void MyFunc(...);
{% endhighlight %}

但是，这样的话，我们就无法使用函数的参数了，因为无法通过上面所讲的宏来提取每个参数。所以除非你的函数代码中的确没有用到参数表中的任何参数，否则必须在参数表中使用至少一个普通参数。
注意，可变参数只能位于函数参数表的最后。不能这样：

{% highlight cpp %}
void MyFunc(..., int i);
{% endhighlight %}

2、采用与UNIX 兼容系统下的声明方式时，参数个数可变的函数原型是：

{% highlight cpp %}
type funcname(va_alist);
{% endhighlight %}

但是要求函数实现的时候，函数名字后面必须加上va\_dcl.例如：

{% highlight cpp %}
#include 
int average( va_list );
void main( void )
{
   ...//代码
}
/* UNIX兼容形式*/
int average( va_alist )
va_dcl
{
   ...//代码
}
{% endhighlight %}

这种形式不需要提供任何普通的形式参数。type是函数返回值的类型。va\_dcl是对函数原型声明中参数va\_alist的详细声明，实际是一个宏定义。根据平台的不同，va\_dcl的定义稍有不同。
在varargs.h中，va\_dcl的定义后面已经包括了一个分号。因此函数实现的时候，va\_dcl后不再需要加上分号了。
3、采用头文件stdarg.h编写的程序是符合ANSI标准的，可以在各种操作系统和硬件上运行;而采用头文件varargs.h的方式仅仅是为了与以 前的程序兼容，两种方式的基本原理是一致的，只是在语法形式上有一些细微的区别。 所以一般编程的时候使用stdarg.h.下面的所有例子代码都采用ANSI标准格式。

四、可变参数函数的基本使用方法
下面通过若干例子，说明如何实现可变参数函数的定义和调用。

{% highlight cpp %}
//================================ 例子程序1 ===============
#include < stdio.h >
#include < string.h >
#include < stdarg.h >
/* 函数原型声明，至少需要一个确定的参数，注意括号内的省略号 */
int demo( char *, ... );
void main( void )
{
	demo("DEMO", "This", "is", "a", "demo!", "\0");
}
int demo( char *msg, ... )
{
	va_list argp; /* 定义保存函数参数的结构 */
	int argno = 0; /* 纪录参数个数 */
	char *para; /* 存放取出的字符串参数 */
	// 使用宏va_start, 使argp指向传入的第一个可选参数，
	// 注意 msg是参数表中最后一个确定的参数，并非参数表中第一个参数
	va_start( argp, msg );
	while (1)
	{
		//取出当前的参数，类型为char *
		//如果不给出正确的类型，将得到错误的参数
		para = va_arg( argp, char *);
		if ( strcmp( para, "\0") == 0 ) /* 采用空串指示参数输入结束 */
		   break;
		printf("参数 #%d 是: %s\n", argno, para);
		argno++;
	}
	va_end( argp ); /* 将argp置为NULL */
	return 0;
}
{% endhighlight %}

//输出结果
参数 \#0 是: This
参数 \#1 是: is
参数 \#2 是: a
参数 \#3 是: demo!
注意到上面的例子没有使用第一个参数，下面的例子将使用所有参数

{% highlight cpp %}
//================================ 例子程序2 ===============
#include <stdio.h>
#include <stdarg.h>
int average( int first, ... ); //输入若干整数，求它们的平均值
void main( void )
{
   /* 调用3个整数(-1表示结尾) */
   printf( "Average is: %d\n", average(2,3,4, -1));
   /*调用4个整数*/
   printf( "Average is: %d\n", average(5,7,9, 11,-1));
   /*只有结束符的调用*/
   printf( "Average is: %d\n", average(-1) );
}
/* 返回若干整数平均值的函数 */
int average( int first, ... )
{
   int count = 0, sum = 0, i = first;
   va_list marker;
   va_start( marker, first ); //初始化
   while( i != -1 )
   {
      sum += i; //先加第一个参数
      count++;
      i = va_arg( marker, int);//取下一个参数
   }
   va_end( marker );
   return( sum ? (sum / count) : 0 );
}
{% endhighlight %}

//输出结果
Average is: 3
Average is: 8
Average is: 0

五、关于可变参数的传递问题
有人问到这个问题，假如我定义了一个可变参数函数，在这个函数内部又要调用其它可变参数函数，那么如何传递参数呢？上面的例子都是使用宏va\_arg逐个把参数提取出来使用，能否不提取，直接把它们传递给另外的函数呢？
我们先看printf的实现：

{% highlight cpp %}
int __cdecl printf (const char *format, ...)
{
	va_list arglist;
	int buffing;
	int retval;
	va_start(arglist, format); //arglist指向format后面的第一个参数
	...//不关心其它代码
	retval = _output(stdout,format,arglist); //把format格式和参数传递给output函数
	...//不关心其它代码
	return(retval);
}
{% endhighlight %}

我们先模仿这个函数写一个：

{% highlight cpp %}
#include <stdio.h>
#include <stdarg.h>
int mywrite(char *fmt, ...)
{
     va_list arglist;
     va_start(arglist, fmt);
     return printf(fmt,arglist);
}
void main()
{
	int i=10, j=20;
	char buf[] = "This is a test";
	double f= 12.345;
	mywrite("String: %s\nInt: %d, %d\nFloat :%4.2f\n", buf, i, j, f);
}
{% endhighlight %}

运行一下看看，哈，错误百出。仔细分析原因，根据宏的定义我们知道 arglist是一个指针，它指向第一个可变的参数，但是所有的参数都位于栈中，所以arglist指向栈中某个位置，通过arglist的值，我们可以直接查看栈里面的内容：
arglist -&gt; 指向栈里面，内容包括
0067FD78 E0 FD 67 00 //指向字符串"This is a test"
0067FD7C 0A 00 00 00 //整数 i 的值
0067FD80 14 00 00 00 //整数 j 的值
0067FD84 71 3D 0A D7 //double 变量 f， 占用8个字节
0067FD88 A3 B0 28 40
0067FD8C 00 00 00 00
如果直接调用 printf(fmt， arglist); 仅仅是把arglist指针的值0067FD78入栈，然后把格式字符串入栈，相当于调用：
printf(fmt， 0067FD78);
自然这样的调用肯定会出现错误。
我们能不能逐个把参数提取出来，再传递给其它函数呢？先考虑一次性把所有参数传递进去的问题。
如果调用的是系统库函数，这种情况下是不可能的。因为提取参数是在运行态，而参数入栈是在编译的时候确定的。无法让编译器预知运行态的事情给出正确的参数 入栈代码。而我们在运行态虽然可以提取每个参数，但是无法将参数一次性全部压栈，即使使用汇编代码实现起来也是很困难的，因为不单是一个简单的push代 码就可以做到。
如果接受参数的函数也是我们自己写的，自然我们可以把arglist指针入栈，然后在函数中自己解析arglist指针里面的参数，逐个提取出来处理。但 是这样做似乎没有什么意义，一方面，这个函数没有必要也做成可变参数函数，另一方面直接在第一个函数中解析参数，然后处理不是更简单么？
我们唯一可以做到的是，逐个解析参数，然后循环中调用其它可变参数函数，每次传递一个参数。这里又有一个问题，就是参数表中的不可变参数的传递问题，有些情况下不能简单的传递，以上面的例子为例， 通常我们解析参数的同时，还需要解析格式字符串：

{% highlight cpp %}
#include <windows.h>
#include <stdio.h>
#include <stdarg.h>
//测试一下这个，开个玩笑
void t(...)
{
	printf("\n");
}
int mywrite(char *fmt, ...)
{
	va_list arglist;
	va_start(arglist, fmt);
	char temp[255];
	strcpy(temp, fmt); //Copy the Format string
	char Format[255];
	char *p = strchr(temp,'%');
	int i=0;
	int iParam;
	double fParam;
	while(p != NULL)
	{
	   while((*p<'a' || *p>'z') && (*p!=0) ) p++;
	   if(*p == 0)break;
	   p++;
	   //格式字符串
	   int nChar = p - temp;
	   strncpy(Format,temp, nChar);
	   Format[nChar] = 0;
	   //参数
	   if(Format[nChar-1] != 'f')
	   {
		iParam = va_arg( arglist, int);
		printf(Format, iParam);
	   }
	   else
	   {
		fParam = va_arg( arglist, double);
		printf(Format, fParam);
	   }
	   i++;
	   if(*p == 0) break;
	   strcpy(temp, p);
	   p = strchr(temp, '%');
	}
	if(temp[0] != 0)
	   printf(temp);
	return i;
}
void main()
{
	int i=10, j=20;
	char buf[] = "This is a test";
	double f= 123.456;
	mywrite("String: %s\nInt: %d, %d\nFloat :%4.2f\nEnd", buf, i, j, f, 0);
	t("aaa", i);
}
{% endhighlight %}

//输出：
String: This is a test
Int: 10, 20
Float :123.46
End

当然这里的解析是不完善的。
