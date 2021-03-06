---
layout: post
title: c++文件读写总结
description: ""
category: program
analytics: true
tags: [c++, file, io, fstream]
---

一、ASCII 输出
为了使用下面的方法, 你必须包含头文件&lt;fstream.h&gt;(译者注：在标准C+*中，已经使用<fstream>取代&lt; fstream.h&gt;，所有的C*+标准头文件都是无后缀的。)。这是 &lt;iostream.h&gt;的一个扩展集, 提供有缓冲的文件输入输出操作. 事实上, &lt;iostream.h&gt; 已经被&lt;fstream.h&gt;包含了, 所以你不必包含所有这两个文件, 如果你想显式包含他们，那随便你。我们从文件操作类的设计开始, 我会讲解如何进行ASCII I/O操作。如果你猜是"fstream," 恭喜你答对了！ 但这篇文章介绍的方法,我们分别使用"ifstream"?和 "ofstream" 来作输入输出。
如果你用过标准控制台流"cin"?和 "cout," 那现在的事情对你来说很简单。 我们现在开始讲输出部分，首先声明一个类对象。
ofstream fout; 这就可以了，不过你要打开一个文件的话, 必须像这样调用ofstream::open()。

fout.open("output.txt"); 你也可以把文件名作为构造参数来打开一个文件.

ofstream fout("output.txt"); 这是我们使用的方法, 因为这样创建和打开一个文件看起来更简单. 顺便说一句, 如果你要打开的文件不存在，它会为你创建一个, 所以不用担心文件创建的问题. 现在就输出到文件，看起来和"cout"的操作很像。对不了解控制台输出"cout"的人, 这里有个例子。

int num = 150;
char name\[\] = "John Doe";
fout &lt;&lt; "Here is a number: " &lt;&lt; num &lt;&lt; "\\n";
fout &lt;&lt; "Now here is a string: " &lt;&lt; name &lt;&lt; "\\n"; 现在保存文件，你必须关闭文件，或者回写文件缓冲. 文件关闭之后就不能再操作了, 所以只有在你不再操作这个文件的时候 " type="text/javascript"&gt; 才调用它，它会自动保存文件。回写缓冲区会在保持文件打开的情况下保存文件, 所以只要有必要就使用它。 回写看起来像另一次输出, 然后调用方法关闭。像这样：

fout &lt;&lt; flush; fout.close(); 现在你用文本编辑器打开文件，内容看起来是这样：

Here is a number: 150 Now here is a string: John Doe 很简单吧! 现在继续文件输入, 需要一点技巧, 所以先确认你已经明白了流操作，对 "&lt;&lt;" 和"&gt;&gt;" 比较熟悉了, 因为你接下来还要用到他们。继续…

二、ASCII 输入
输入和"cin" 流很像. 和刚刚讨论的输出流很像, 但你要考虑几件事情。在我们开始复杂的内容之前, 先看一个文本：

12 GameDev 15.45 L This is really awesome! 为了打开这个文件，你必须创建一个in-stream对象,?像这样。

ifstream fin("input.txt"); 现在读入前四行. 你还记得怎么用"&lt;&lt;" 操作符往流里插入变量和符号吧？好,?在 "&lt;&lt;" (插入)?操作符之后，是"&gt;&gt;" (提取) 操作符. 使用方法是一样的. 看这个代码片段.

int number;
float real;
char letter, word\[8\];
fin &gt;&gt; number; fin &gt;&gt; word; fin &gt;&gt; real; fin &gt;&gt; letter; 也可以把这四行读取文件的代码写为更简单的一行。

fin &gt;&gt; number &gt;&gt; word &gt;&gt; real &gt;&gt; letter; 它是如何运作的呢? 文件的每个空白之后, "&gt;&gt;" 操作符会停止读取内容, 直到遇到另一个&gt;&gt;操作符. 因为我们读取的每一行都被换行符分割开(是空白字符), "&gt;&gt;" 操作符只把这一行的内容读入变量。这就是这个代码也能正常工作的原因。但是，可别忘了文件的最后一行。

This is really awesome! 如果你想把整行读入一个char数组, 我们没办法用"&gt;&gt;"?操作符，因为每个单词之间的空格（空白字符）会中止文件的读取。为了验证：

char sentence\[101\]; fin &gt;&gt; sentence; 我们想包含整个句子, "This is really awesome!" 但是因为空白, 现在它只包含了"This". 很明显, 肯定有读取整行的方法, 它就是getline()。这就是我们要做的。

fin.getline(sentence, 100); 这是函数参数. 第一个参数显然是用来接受的char数组. 第二个参数是在遇到换行符之前，数组允许接受的最大元素数量. 现在我们得到了想要的结果：“This is really awesome!”。
你应该已经知道如何读取和写入ASCII文件了。但我们还不能罢休，因为二进制文件还在等着我们。

三、二进制 输入输出
二进制文件会复杂一点, 但还是很简单的。 首先你要注意我们不再使用插入和提取操作符(译者注：&lt;&lt; 和 &gt;&gt; 操作符). 你可以这么做，但它不会用二进制方式读写。你必须使用read() 和write() 方法读取和写入二进制文件. 创建一个二进制文件, 看下一行。

ofstream fout("file.dat", ios::binary); 这会以二进制方式打开文件, 而不是默认的ASCII模式。首先从写入文件开始。函数write() 有两个参数。 第一个是指向对象的char类型的指针, 第二个是对象的大小（译者注：字节数）。 为了说明，看例子。

int number = 30; fout.write((char **)(&number), sizeof(number)); 第一个参数写做"(char**)(&number)". 这是把一个整型变量转为char \*指针。如果你不理解，可以立刻翻阅C++的书籍，如果有必要的话。第二个参数写作"sizeof(number)". sizeof() 返回对象大小的字节数. 就是这样!
二进制文件最好的地方是可以在一行把一个结构写入文件。 如果说，你的结构有12个不同的成员。 用ASCII?文件，你不得不每次一条的写入所有成员。 但二进制文件替你做好了。 看这个。

struct OBJECT { int number; char letter; } obj;
obj.number = 15;
obj.letter = ‘M’;
fout.write((char \*)(&obj), sizeof(obj)); 这样就写入了整个结构! 接下来是输入. 输入也很简单，因为read()?函数的参数和 write()是完全一样的, 使用方法也相同。

ifstream fin("file.dat", ios::binary); fin.read((char \*)(&obj), sizeof(obj)); 我不多解释用法, 因为它和write()是完全相同的。二进制文件比ASCII文件简单, 但有个缺点是无法用文本编辑器编辑。 接着, 我解释一下ifstream 和ofstream 对象的其他一些方法作为结束.

四、更多方法
我已经解释了ASCII文件和二进制文件, 这里是一些没有提及的底层方法。

检查文件
你已经学会了open() 和close() 方法, 不过这里还有其它你可能用到的方法。
方法good() 返回一个布尔值，表示文件打开是否正确。
类似的，bad() 返回一个布尔值表示文件打开是否错误。 如果出错，就不要继续进一步的操作了。
最后一个检查的方法是fail(), 和bad()有点相似, 但没那么严重。

读文件
方法get() 每次返回一个字符。
方法ignore(int,char) 跳过一定数量的某个字符, 但你必须传给它两个参数。第一个是需要跳过的字符数。 第二个是一个字符, 当遇到的时候就会停止。 例子,

fin.ignore(100, ‘\\n’); 会跳过100个字符，或者不足100的时候，跳过所有之前的字符，包括 ‘\\n’。
方法peek() 返回文件中的下一个字符, 但并不实际读取它。所以如果你用peek() 查看下一个字符, 用get() 在peek()之后读取，会得到同一个字符, 然后移动文件计数器。
方法putback(char) 输入字符, 一次一个, 到流中。我没有见到过它的使用，但这个函数确实存在。

写文件
只有一个你可能会关注的方法.?那就是 put(char), 它每次向输出流中写入一个字符。

打开文件
当我们用这样的语法打开二进制文件:

ofstream fout("file.dat", ios::binary); "ios::binary"是你提供的打开选项的额外标志. 默认的, 文件以ASCII方式打开, 不存在则创建, 存在就覆盖. 这里有些额外的标志用来改变选项。

ios::app 添加到文件尾
ios::ate 把文件标志放在末尾而非起始。
ios::trunc 默认. 截断并覆写文件。
ios::nocreate 文件不存在也不创建。
ios::noreplace 文件存在则失败。

文件状态
我用过的唯一一个状态函数是eof(), 它返回是否标志已经到了文件末尾。 我主要用在循环中。 例如, 这个代码断统计小写‘e’ 在文件中出现的次数。

ifstream fin("file.txt");
char ch; int counter;
while (!fin.eof()) {
ch = fin.get();
if (ch == ‘e’) counter**;
}
fin.close(); 我从未用过这里没有提到的其他方法。 还有很多方法，但是他们很少被使用。参考C++书籍或者文件流的帮助文档来了解其他的方法。

结论
你应该已经掌握了如何使用ASCII文件和二进制文件。有很多方法可以帮你实现输入输出，尽管很少有人使用他们。我知道很多人不熟悉文件I/O操作，我希望这篇文章对你有所帮助。 每个人都应该知道. 文件I/O还有很多显而易见的方法,?例如包含文件 &lt;stdio.h&gt;. 我更喜欢用流是因为他们更简单。 祝所有读了这篇文章的人好运, 也许以后我还会为你们写些东西

无论读写都要包含<fstream>头文件

读：从外部文件中将数据读到程序中来处理
对于程序来说，是从外部读入数据，因此定义输入流，即定义输入流对象:ifsteam infile，infile就是输入流对象。
这个对象当中存放即将从文件读入的数据流。假设有名字为myfile.txt的文件，存有两行数字数据，具体方法：

{% highlight cpp %}
int a,b;
ifstream infile;
infile.open("myfile.txt");      //注意文件的路径
infile>>a>>b;                   //两行数据可以连续读出到变量里
infile.close();
{% endhighlight %}

如果是个很大的多行存储的文本型文件可以这么读：

{% highlight cpp %}
char buf[1024];                //临时保存读取出来的文件内容
string message;
ifstream infile;
infile.open("myfile.js");
if(infile.is_open())          //文件打开成功,说明曾经写入过东西
{
 while(infile.good() && !infile.eof())
 {
   memset(buf,0,1024);
   infile.getline(buf,1204);
   message = buf;
   ......                     //这里可能对message做一些操作
   cout<<message<<endl;
 }
 infile.close();
}
{% endhighlight %}

写：将程序中处理后的数据写到文件当中
对程序来说是将数据写出去，即数据离开程序，因此定义输出流对象ofstream outfile，outfile就是输出流对象，这个对象用来存放将要写到文件当中的数据。具体做法：

{% highlight cpp %}
ofstream outfile;
outfile.open("myfile.bat");  //myfile.bat是存放数据的文件名
if(outfile.is_open())
{
  outfile<<message<<endl;    //message是程序中处理的数据
  outfile.close();
}
else
{
  cout<<"不能打开文件!"<<endl;
}
{% endhighlight %}

c++对文件的读写操作的例子

/\*/从键盘读入一行字符，把其中的字母依次放在磁盘文件fa2.dat中，再把它从磁盘文件读入程序，
将其中的小写字母改成大写字母，再存入磁盘fa3.dat中\*/

{% highlight cpp %}
#include<fstream>
#include<iostream>
#include<cmath>
using namespace std;
//////////////从键盘上读取字符的函数
void read_save(){
	char c[80];
	ofstream outfile("f1.dat");//以输出方工打开文件
	if(!outfile){
		cerr<<"open error!"<<endl;//注意是用的是cerr
		exit(1);
	}
	cin.getline(c,80);//从键盘读入一行字符
	for(int i=0;c[i]!=0;i++) //对字符一个一个的处理，直到遇到'/0'为止
		if(c[i]>=65&&c[i]<=90||c[i]>=97&&c[i]<=122){//保证输入的字符是字符
			outfile.put(c[i]);//将字母字符存入磁盘文件
			cout<<c[i]<<"";
		}
	cout<<endl;
	outfile.close();
}
void creat_data(){
	char ch;
	ifstream infile("f1.dat",ios::in);//以输入的方式打开文件
	if(!infile){
		cerr<<"open error!"<<endl;
		exit(1);
	}
	ofstream outfile("f3.dat");//定义输出流f3.dat文件
	if(!outfile){
		cerr<<"open error!"<<endl;
		exit(1);
	}
	while(infile.get(ch)){//当读取字符成功时
		if(ch<=122&&ch>=97)
		ch=ch-32;
		outfile.put(ch);
		cout<<ch;
	}
	cout<<endl;
	infile.close();
	outfile.close();
}
int main(){
	read_save();
	creat_data();
	system("pause");
	return 0;
}
{% endhighlight %}

转自:http://blog.csdn.net/shtianhai/archive/2008/05/02/2363841.aspx
