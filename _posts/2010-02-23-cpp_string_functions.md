---
layout: post
title: C/C++ 字符串处理函数
description: ""
category: program
analytics: true
tags: [c, c++, string, 函数]
---

2008年01月15日 星期二 21:06
刚开始学C/C+*时，一直对字符串处理函数一知半解，这里列举C/C*+字符串处理函数，希望对初学者有一定的帮助。

函数名: stpcpy
功 能: 拷贝一个字符串到另一个
用 法: char \*stpcpy(char \*destin, char \*source);
程序例:

{% highlight cpp %}
#include <stdio.h> 
#include <string.h> 
int main(void) 
{ 
   char string[10]; 
   char *str1 = "abcdefghi"; 
   stpcpy(string, str1); 
   printf("%sn", string); 
   return 0; 
} 
{% endhighlight %}

函数名: strcat
功 能: 字符串拼接函数
用 法: char \*strcat(char \*destin, char \*source);
程序例:

{% highlight cpp %}
#include <string.h> 
#include <stdio.h> 
int main(void) 
{ 
   char destination[25]; 
   char *blank = " ", *c = "C++", *Borland = "Borland"; 
   strcpy(destination, Borland); 
   strcat(destination, blank); 
   strcat(destination, c); 
   printf("%sn", destination); 
   return 0; 
} 
{% endhighlight %}

函数名: strchr
功 能: 在一个串中查找给定字符的第一个匹配之处
用 法: char \*strchr(char \*str, char c);
程序例:

{% highlight cpp %}
#include <string.h> 
#include <stdio.h> 
int main(void) 
{ 
	char string[15]; 
	char *ptr, c = 'r'; 
	strcpy(string, "This is a string"); 
	ptr = strchr(string, c); 
	if (ptr) 
		printf("The character %c is at position: %dn", c, ptr-string); 
	else 
		printf("The character was not foundn"); 
	return 0; 
} 
{% endhighlight %}

函数名: strcmp
功 能: 串比较
用 法: int strcmp(char \*str1, char \*str2);
看Asic码，str1&gt;str2，返回值 &gt; 0；两串相等，返回0
程序例:

{% highlight cpp %}
#include <string.h> 
#include <stdio.h> 
int main(void) 
{ 
	char *buf1 = "aaa", *buf2 = "bbb", *buf3 = "ccc"; 
	int ptr; 
	ptr = strcmp(buf2, buf1); 
	if (ptr > 0) 
	   printf("buffer 2 is greater than buffer 1n"); 
	else 
	   printf("buffer 2 is less than buffer 1n"); 
	ptr = strcmp(buf2, buf3); 
	if (ptr > 0) 
	   printf("buffer 2 is greater than buffer 3n"); 
	else 
	   printf("buffer 2 is less than buffer 3n"); 
	return 0; 
}
{% endhighlight %}

函数名: strncmpi
功 能: 将一个串中的一部分与另一个串比较, 不管大小写
用 法: int strncmpi(char \*str1, char \*str2, unsigned maxlen);
程序例:

{% highlight cpp %}
#include <string.h> 
#include <stdio.h> 
int main(void) 
{ 
   char *buf1 = "BBB", *buf2 = "bbb"; 
   int ptr; 
   ptr = strcmpi(buf2, buf1); 
   if (ptr > 0) 
      printf("buffer 2 is greater than buffer 1n"); 
   if (ptr < 0) 
      printf("buffer 2 is less than buffer 1n"); 
   if (ptr == 0) 
      printf("buffer 2 equals buffer 1n"); 
   return 0; 
} 
{% endhighlight %}

函数名: strcpy
功 能: 串拷贝
用 法: char \*strcpy(char \*str1, char \*str2);
程序例:

{% highlight cpp %}
#include <stdio.h> 
#include <string.h> 
int main(void) 
{ 
	char string[10]; 
	char *str1 = "abcdefghi"; 
	strcpy(string, str1); 
	printf("%sn", string); 
	return 0; 
}
{% endhighlight %}

函数名: strcspn
功 能: 在串中查找第一个给定字符集内容的段
用 法: int strcspn(char \*str1, char \*str2);
程序例:

{% highlight cpp %}
#include <stdio.h> 
#include <string.h> 
#include <alloc.h> 
int main(void) 
{ 
	char *string1 = "1234567890"; 
	char *string2 = "747DC8"; 
	int length; 
	length = strcspn(string1, string2); 
	printf("Character where strings intersect is at position %dn", length); 
	return 0; 
}
{% endhighlight %}

函数名: strdup
功 能: 将串拷贝到新建的位置处
用 法: char \*strdup(char \*str);
程序例:

{% highlight cpp %}
#include <stdio.h> 
#include <string.h> 
#include <alloc.h> 
int main(void) 
{ 
	char *dup_str, *string = "abcde"; 
	dup_str = strdup(string); 
	printf("%sn", dup_str); 
	free(dup_str); 
	return 0; 
}
{% endhighlight %}

函数名: stricmp
功 能: 以大小写不敏感方式比较两个串
用 法: int stricmp(char \*str1, char \*str2);
程序例:

{% highlight cpp %}
#include <string.h> 
#include <stdio.h> 
int main(void) 
{ 
   char *buf1 = "BBB", *buf2 = "bbb"; 
   int ptr; 
   ptr = stricmp(buf2, buf1); 
   if (ptr > 0) 
      printf("buffer 2 is greater than buffer 1n"); 
   if (ptr < 0) 
      printf("buffer 2 is less than buffer 1n"); 
   if (ptr == 0) 
      printf("buffer 2 equals buffer 1n"); 
   return 0; 
}
{% endhighlight %}

函数名: strerror
功 能: 返回指向错误信息字符串的指针
用 法: char \*strerror(int errnum);
程序例:

{% highlight cpp %}
#include <stdio.h> 
#include <errno.h> 
int main(void) 
{ 
   char *buffer; 
   buffer = strerror(errno); 
   printf("Error: %sn", buffer); 
   return 0; 
}
{% endhighlight %}

函数名: strcmpi
功 能: 将一个串与另一个比较, 不管大小写
用 法: int strcmpi(char \*str1, char \*str2);
程序例:

{% highlight cpp %}
#include <string.h> 
#include <stdio.h> 
int main(void) 
{ 
   char *buf1 = "BBB", *buf2 = "bbb"; 
   int ptr; 
   ptr = strcmpi(buf2, buf1); 
   if (ptr > 0) 
      printf("buffer 2 is greater than buffer 1n"); 
   if (ptr < 0) 
      printf("buffer 2 is less than buffer 1n"); 
   if (ptr == 0) 
      printf("buffer 2 equals buffer 1n"); 
   return 0; 
}
{% endhighlight %}

函数名: strncmp
功 能: 串比较
用 法: int strncmp(char \*str1, char \*str2, int maxlen);
程序例:

{% highlight cpp %}
#include <string.h> 
#include <stdio.h> 
int  main(void) 
{ 
   char *buf1 = "aaabbb", *buf2 = "bbbccc", *buf3 = "ccc"; 
   int ptr; 
   ptr = strncmp(buf2,buf1,3); 
   if (ptr > 0) 
      printf("buffer 2 is greater than buffer 1n"); 
   else 
      printf("buffer 2 is less than buffer 1n"); 
   ptr = strncmp(buf2,buf3,3); 
   if (ptr > 0) 
      printf("buffer 2 is greater than buffer 3n"); 
   else 
      printf("buffer 2 is less than buffer 3n"); 
   return(0); 
}
{% endhighlight %}

函数名: strncmpi
功 能: 把串中的一部分与另一串中的一部分比较, 不管大小写
用 法: int strncmpi(char \*str1, char \*str2);
程序例:

{% highlight cpp %}
#include <string.h> 
#include <stdio.h> 
int main(void) 
{ 
   char *buf1 = "BBBccc", *buf2 = "bbbccc"; 
   int ptr; 
   ptr = strncmpi(buf2,buf1,3); 
   if (ptr > 0) 
      printf("buffer 2 is greater than buffer 1n"); 
   if (ptr < 0) 
      printf("buffer 2 is less than buffer 1n"); 
   if (ptr == 0) 
      printf("buffer 2 equals buffer 1n"); 
   return 0; 
}
{% endhighlight %}

函数名: strncpy
功 能: 串拷贝
用 法: char \*strncpy(char \*destin, char \*source, int maxlen);
程序例:

{% highlight cpp %}
#include <stdio.h> 
#include <string.h> 
int main(void) 
{ 
   char string[10]; 
   char *str1 = "abcdefghi"; 
   strncpy(string, str1, 3); 
   string[3] = ''; 
   printf("%sn", string); 
   return 0; 
}
{% endhighlight %}

函数名: strnicmp
功 能: 不注重大小写地比较两个串
用 法: int strnicmp(char \*str1, char \*str2, unsigned maxlen);
程序例:

{% highlight cpp %}
#include <string.h> 
#include <stdio.h> 
int main(void) 
{ 
   char *buf1 = "BBBccc", *buf2 = "bbbccc"; 
   int ptr; 
   ptr = strnicmp(buf2, buf1, 3); 
   if (ptr > 0) 
      printf("buffer 2 is greater than buffer 1n"); 
   if (ptr < 0) 
      printf("buffer 2 is less than buffer 1n"); 
   if (ptr == 0) 
      printf("buffer 2 equals buffer 1n"); 
   return 0; 
}
{% endhighlight %}

函数名: strnset
功 能: 将一个串中的所有字符都设为指定字符
用 法: char \*strnset(char \*str, char ch, unsigned n);
程序例:

{% highlight cpp %}
#include <stdio.h> 
#include <string.h> 
int main(void) 
{ 
   char *string = "abcdefghijklmnopqrstuvwxyz"; 
   char letter = 'x'; 
   printf("string before strnset: %sn", string); 
   strnset(string, letter, 13); 
   printf("string after  strnset: %sn", string); 
   return 0; 
}
{% endhighlight %}

函数名: strpbrk
功 能: 在串中查找给定字符集中的字符
用 法: char \*strpbrk(char \*str1, char \*str2);
程序例:

{% highlight cpp %}
#include <stdio.h> 
#include <string.h> 
int main(void) 
{ 
   char *string1 = "abcdefghijklmnopqrstuvwxyz"; 
   char *string2 = "onm"; 
   char *ptr; 
   ptr = strpbrk(string1, string2); 
   if (ptr) 
      printf("strpbrk found first character: %cn", *ptr); 
   else 
      printf("strpbrk didn't find character in setn"); 
   return 0; 
}
{% endhighlight %}

函数名: strrchr
功 能: 在串中查找指定字符的最后一个出现
用 法: char \*strrchr(char \*str, char c);
程序例:

{% highlight cpp %}
#include <string.h> 
#include <stdio.h> 
int main(void) 
{ 
   char string[15]; 
   char *ptr, c = 'r'; 
   strcpy(string, "This is a string"); 
   ptr = strrchr(string, c); 
   if (ptr) 
      printf("The character %c is at position: %dn", c, ptr-string); 
   else 
      printf("The character was not foundn"); 
   return 0; 
}
{% endhighlight %}

函数名: strrev 
功  能: 串倒转 
用  法: char *strrev(char *str); 
程序例: 

{% highlight cpp %}
#include <string.h> 
#include <stdio.h> 
int main(void) 
{ 
   char *forward = "string"; 
   printf("Before strrev(): %sn", forward); 
   strrev(forward); 
   printf("After strrev():  %sn", forward); 
   return 0; 
}
{% endhighlight %}

函数名: strset 
功  能: 将一个串中的所有字符都设为指定字符 
用  法: char *strset(char *str, char c); 
程序例: 

{% highlight cpp %}
#include <stdio.h> 
#include <string.h> 
int main(void) 
{ 
   char string[10] = "123456789"; 
   char symbol = 'c'; 
   printf("Before strset(): %sn", string); 
   strset(string, symbol); 
   printf("After strset():  %sn", string); 
   return 0; 
}
{% endhighlight %}

函数名: strspn 
功  能: 在串中查找指定字符集的子集的第一次出现 
用  法: int strspn(char *str1, char *str2); 
程序例: 

{% highlight cpp %}
#include <stdio.h> 
#include <string.h> 
#include <alloc.h> 
int main(void) 
{ 
   char *string1 = "1234567890"; 
   char *string2 = "123DC8"; 
   int length; 
   length = strspn(string1, string2); 
   printf("Character where strings differ is at position %dn", length); 
   return 0; 
}
{% endhighlight %}

函数名: strstr 
功  能: 在串中查找指定字符串的第一次出现 
用  法: char *strstr(char *str1, char *str2); 
程序例: 

{% highlight cpp %}
#include <stdio.h> 
#include <string.h> 
int main(void) 
{ 
   char *str1 = "Borland International", *str2 = "nation", *ptr; 
   ptr = strstr(str1, str2); 
   printf("The substring is: %sn", ptr); 
   return 0; 
}
{% endhighlight %}

函数名: strtod 
功  能: 将字符串转换为double型值 
用  法: double strtod(char *str, char **endptr); 
程序例: 

{% highlight cpp %}
#include <stdio.h> 
#include <stdlib.h> 
int main(void) 
{ 
   char input[80], *endptr; 
   double value; 
   printf("Enter a floating point number:"); 
   gets(input); 
   value = strtod(input, &endptr); 
   printf("The string is %s the number is %lfn", input, value); 
   return 0; 
}
{% endhighlight %}

函数名: strtok 
功  能: 查找由在第二个串中指定的分界符分隔开的单词 
用  法: char *strtok(char *str1, char *str2); 
程序例: 

{% highlight cpp %}
#include <string.h> 
#include <stdio.h> 
int main(void) 
{ 
   char input[16] = "abc,d"; 
   char *p; 
   /* strtok places a NULL terminator 
   in front of the token, if found */ 
   p = strtok(input, ","); 
   if (p)   printf("%sn", p); 
   /* A second call to strtok using a NULL 
   as the first parameter returns a pointer 
   to the character following the token  */ 
   p = strtok(NULL, ","); 
   if (p)   printf("%sn", p); 
   return 0; 
}
{% endhighlight %}

函数名: strtol 
功  能: 将串转换为长整数 
用  法: long strtol(char *str, char **endptr, int base); 
程序例: 

{% highlight cpp %}
#include <stdlib.h> 
#include <stdio.h> 
int main(void) 
{ 
   char *string = "87654321", *endptr; 
   long lnumber; 
   /* strtol converts string to long integer  */ 
   lnumber = strtol(string, &endptr, 10); 
   printf("string = %s  long = %ldn", string, lnumber); 
   return 0; 
}
{% endhighlight %}

函数名: strupr 
功  能: 将串中的小写字母转换为大写字母 
用  法: char *strupr(char *str); 
程序例: 

{% highlight cpp %}
#include <stdio.h> 
#include <string.h> 
int main(void) 
{ 
   char *string = "abcdefghijklmnopqrstuvwxyz", *ptr; 
   /* converts string to upper case characters */ 
   ptr = strupr(string); 
   printf("%sn", ptr); 
   return 0; 
}
{% endhighlight %}

函数名: swab 
功  能: 交换字节 
用  法: void swab (char *from, char *to, int nbytes); 
程序例: 

{% highlight cpp %}
#include <stdlib.h> 
#include <stdio.h> 
#include <string.h> 
char source[15] = "rFna koBlrna d"; 
char target[15]; 
int main(void) 
{ 
   swab(source, target, strlen(source)); 
   printf("This is target: %sn", target); 
   return 0; 
}
{% endhighlight %}

PS:isalpha()是字符函数，不是字符串函数，

isalpha 
  
  原型：extern int isalpha(int c);
  
  用法：#include <ctype.h>
  
  功能：判断字符c是否为英文字母
  
  说明：当c为英文字母a-z或A-Z时，返回非零值，否则返回零。
  
  举例：

{% highlight cpp %}
// isalpha.c
#include <syslib.h>
#include <ctype.h>
#include <stdio.h>
main()
{
   int c;
   clrscr();        // clear screen
   printf("Press a key");
   for(;;)
   {
      c=getchar();
      clrscr();
      printf("%c: %s letter",c,isalpha(c)?"is":"not");
   }
   return 0; // just to avoid warnings by compiler
}
{% endhighlight %}
     
C：

char st[100];

1. 字符串长度
   strlen(st);

2. 字符串比较
   strcmp(st1,st2);
   strncmp(st1,st2,n);   //把st1,st2的前n个进行比较。

3. 附加
   strcat(st1,st2);
   strncat(st1,st2,n);   //n表示连接上st2的前n个给st1，在最后不要加'\0'。

4. 替换
   strcpy(st1,st2);
   strncpy(st1,st2,n); //n表示复制st2的前n个给st1，在最后要加'\0'。

5. 查找
   where = strchr(st,ch)   //ch为要找的字符。
   where = strspn(st1,st2); //查找字符串。

C++：

<string>
string str;

1. 字符串长度
   len = str.length();
   len = str.size();

2. 字符串比较
   可以直接比较
   也可以:
   str1.compare(str2); 
   str1.compare(pos1,len1,str2,pos2,len2); //值为负，0 ，正。
   nops 长度到完。

3. 附加
   str1 += str2;
   或
   str1.append(str2);
   str1.append(str2.pos2,len2);
   
4. 字符串提取
   str2 = str1.substr();
   str2 = str1.substr(pos1);
   str2 = str1.substr(pos1,len1);

5. 字符串搜索
   where = str1.find(str2);
   where = str1.find(str2,pos1); pos1是从str1的第几位开始。
   where = str1.rfind(str2); 从后往前搜。

6. 插入字符串
   不是赋值语句。
   str1.insert(pos1,str2);
   str1.insert(pos1,str2,pos2,len2);
   str1.insert(pos1,numchar,char);    numchar是插入次数，char是要插入的字符。

7. 替换字符串
   str1.replace(pos1,str2);
   str1.replace(pos1,str2,pos2,len2);

8. 删除字符串
   str.erase(pos,len)
   str.clear();

9. 交换字符串
   swap(str1,str2);

10. C --> C++
   char *cstr = "Hello";
   string str1;
   cstr = cstr;
   string str2(cstr);

对于ACMer来说，C的字符串处理要比C++的方便、简单，尽量用C的字符串处理函数。

转自:http://hi.baidu.com/czyuan_acm/blog/item/2c82f5df298f8a1749540309.html
