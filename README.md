# C 语言

> C 语言用来**创建空间小**、**速度快**、**可移植性**高的程序。它比其他语言的抽象层次更低，更接近**机器语言**。大多数**操作系统**、其他**计算机语言**和大多数**游戏**软件都是C语言写的。

## Overview C

### 语言的工作方式

计算机只理解一种语言——**机器代码**，即一串二进制0、1流

1. 源代码：以`.c`结尾创建一个源文件开始，是供人阅读的C代码
2. **编译**：通过编译器运行源代码，编译器会**检查错误**，一旦它觉得没问题，就会编译源代码
3. **输出**：编译器会创建一个可执行文件的新文件。文件中机器代码，即计算机能够理解的0、1流，而这个文件就是**可以运行的程序**

### 不同平台C语言编译器

- Linux: gcc
  - C/C++, Objective-C,Pascal, Fortran, PL/I, Go
  - gcc有前端可后段
    - 前端可以理解某种类型的源代码（C/C++, go...）
    - 前端能够将这种语言转化为一种中间代码，所有的语言前端都能够生成同一种代码的中间代码
    - 后端负责将前端代码转化为多种平台的机器代码的系统。每种操作系统都有自己特定的可执行文件格式
  - 检查明显的错误，例如变量名拼错了；变量名重复定义；当函数名去命名变量时，我也会发出警告，等等。
  - 检查代码质量和性能
- Mac: Xcode
- Windows:
  - [Cygwin](http://www.cygwin.com) 是模拟UNIX环境
  - [MinGW](http://www.mingw.org) 编译运行程序

### 编译器标准

- ANSI C(1980)
  - `void main`
- C99(1999)
  - 必须`int main`
  - 支持 `//`，`true`和`false`关键字，但编译器还是会它们当作1和0这两个值来处理
  - `main()`没有添加`return`时，自动添加return
  - 编译选项：`-std=c99`
- C11(2011)

### 常用系统函数

- puts("输出内容换行显示")
- char ex[20];
  - scanf("%19s", ex); // 用户输入的任意字符保存在ex数组中
- atoi(字符串); 字符串转整型(<stdlib.h>)

### C 代码结构

- comment
  - /* 此处代码不会被编译器解析运行。此处一般包含一些许可证或者版权信息 */
- include
  - 告诉编译器程序要使用哪些外部代码，需要包含相关`库的头文件`
  - **`stdio.h`库的头文件**中包含**终端读写数据的代码**(`printf,scanf`等)
- main 函数
  - 程序中所有代码的起点，也是**程序执行入口**
  - main函数的返回类型是 int，返回0表示运行成功，其他值，就表示运行时出了问题

- 检查程序退出状态
  - Windows: `echo %ErrorLevel%`
  - Like Unix: `echo $?`

### 如何运行程序

> C 语言是一种`编译型语言`，计算机不会直接解释代码，而是需要将给人阅读的源代码编译(转化)为机器能够理解的机器代码，这样计算机才能够执行。为了编译代码，需要一个叫做编译器的程序。`GNU编译器套件`（GNU Compiler Collection）,也叫`gcc`，是最流行的C编译器之一。gcc可以在很多操作系统中使用，除了C语言，它还可以编译很多其他语言。而且是开源代码。

- 编译代码：`gcc -o hello hello.c`
  - `-o`：输出文件
- 执行代码：
  - Linux: `./hello`
  - Windows: `hello`
- 编译执行：`gcc hello.c -o hello && ./hello`
  - `&&` 表示 `gcc hello.c -o hello` 执行成功才会运行 `./hello`

### Windows 编码问题

- `-fexec-charset=gbk`    执行程序编码
- `-finput-charset=gbk`   文件编码

### 为什么在 Like Unix 操作系统中运行程序是必须在程序前加上 `./`

- Like Unix 操作系统中，运行程序必须指定**程序所在的目录**，除非程序的目录列在了 PATH 环境变量中。

### 检查程序退出状态

- Windows: `echo %ErrorLevel%`
- Linux: `echo $?`

## 变量

### 变量作用域

- 局部比那辆：{}内声明的变量
- 全局变量：函数外声明的变量

## 数据类型

### float 类型

float 有效位数(从第一个数开始计算，不包括.)是**7位**（默认情况保留6位），如果超出了有效位数，那么就会出现一些垃圾数据

- xcode 快捷键
  - 折叠代码： command + option + 方向键
  - 添加断点： command + \

``` c
float pi = 3.1415926525;
printf("%.10f\n", pi); // 3.1415927410 默认保留6位置

// 比格技巧
printf("%.*f\n", 3, pi); // 3.141 3:小数点保留位数
```

### double 类型

double有效位是**15位**

``` c
double pi = 3.1415926525;
printf("%.10lf\n", pi); // 3.1415926525
```

## C 语言不支持现成的字符串

> C 语言以字符为元素的数组

### 字符串

- `char *s = "hello";`
- `char s[] = {'h','e','l','l','o'};`
  - 字符串的每个字符是数组中的一个元素。这就是为什么可以通过索引来引用字符串中的某个字符
- 字符串字面值(string literal)：`char card_name[3] = "ab"; // 其中包含ab\0(哨兵字符)`
  - `\0` 是ASCII字符他的值为0。NULL 字符)
- 字符数组：`char car_name[3] = {'a,'b'};`

### 字符要从0开始编号？为什么不是1？

> 字符的索引值是一个偏移量：它表示当前要引用的这个字符到数组中第一个字符之间有多少字符。计算机在存储器中一连续字节的形式保存字符，并利用缩阴计算出字符在存储器中的位置。如果计算机知道c[0]位于存储器 1 000 000 号单元，那么就可以很快计算出c[96]在 1 000 000 ＋ 96号单元

### 为什么设立哨兵符号？难道计算机不知道字符串的长度？

> 通常不知道。记录数组的长度不是C语言的强项，字符串其实就是数组。虽然有时候可以通过分析代码科技计算出数组的长度，但一般情况下，C语言希望你来记录数组的长度

### 引号

- 单引号表示单个字符
- 双引号表示字符串
  - 用双引号定义的字符串叫做字符串字面值（String literal）
    - **字符串字面值是常量，一旦创建就不能修改**
    - 如果修改了，gcc就时显示**总线错误**(bus error：程序无法更新那一块存储器空间）

### 接受字符串

```c
char card_name[3];
scanf("%2s",card_name);


```

## 布尔类型

- 0 为假
- 非0为 真

## switch 语句

- 只能检查值类型
- 在第一个匹配的 case 语句出开始之行代码
- 在遇到 break 或到达 switch 语句的末尾前，代码会一直运行

## function

- void函数中return语句有时可以用来提前退出函数

### 链式赋值

- 赋值表达式也有返回值
- int x = 4;
- int y = x = 10;
- 在C语言中，所有表达式都有**值**

### C 语言编译

- Javascript和Python为了提高速度通常会在幕后使用一些编译技术
- C++/Objective-C 都是为了用C语言编写OOP（对抗软件复杂性的技术）
- GNU 编译器套装可以用来编译很多种语言，而C语言可能是人们在应用gcc时使用最多的语言
- 创建永无止境的循环
  - 网络服务器的程序会反复地做一件事知道有人停止它

## 指针

> 存储器中某条数据的**地址**

### 指针特性

- 避免副本
- 共享数据

使用指针的主要原因之一就是**让函数共享存储器**

### 为什么局部变量在栈里，而全局变量在全局量段里？

> 用法不同。你永远只能得到一份全局变量，但如果写了一个调用自己的函数，就会得到同一个局部变量的很多个实例。

### 存储器分配

- 栈内存
  - 函数内声明的变量
  - FILO: First In Last Out
- 堆内存
  - 动态分配的空间
- 全局量
  - 函数外声明的变量
- 常量段
  - 程序一开始运行时创建，但他们保存在只读存储器。常量时一些在程序中要用的不变量，不能修改它们的值的，比如：字符串字面值
  - Cygwin 修改常量，不报错，但这样做常常是错的
- 代码段
  - 程序代码存储在存储器地址的低位。代码段是只读的，是存储器中用来加载机器代码的部分

内存寻址从大到小 0Xffff ~ 0X0000

查找变量的存储器地址`&`运算符

存储器地址分组映射到存储芯片的不同的存储器(memery bank)

### 指针变量

> 保存存储器**地址的变量**。当声明指针变量时，需要说明指针所指向的地址中保存的数据的类型

`int *address_of_x = &x;` address_of_x 是指针变量，存储的是x变量的地址

### 读取地址中的内容

`int value = *address_of_x;` *address_of_x是读取address_of_x指针变量所指向的地址所保存变量值赋值给变量value

- `&运算符`接受一个变量(变量类型)，告诉你这个变量数据地址在哪里；告诉你变量保存在存储器中的位置
- `*运算符`接受一个指针(指针类型)，告诉你地址中保存的是什么数据
  - 可以读取地址中数据
  - 可以设置地址中数据
- 指针也叫引用，所以`*运算符`也可以描述成对指针进行解引用
- C语言按**值传递**参数

### 改变地址中的内容

`*指针变量 = 值;`

### 指针分类

- 基本类型的指针
- 指针和数组的关系

### C语言变量的名称是怎么存在的

> 在编译的时候编译器会把程序中出现的所有变量名都换成相对内存地址，变量名不占内存

### 存储在内存中的数据

- 变量的数据值
- 变量的地址值

### 变量与指针

```c
// p 是指针变量，p只能存储 int类型的变量地址
// &p=0028FF2C
int *p;

// &i=0028FF28
int i = 10;

// &j=0028FF24
int j;

// 变量 i指向内容的地址赋值给
// 指针变量p(可以存储变量内容的地址)
p = &i;

// 获取复制指针变量的内容赋值给变量j
j = *p;

printf("j=%d, j pointer=%p\n", j, &j);
printf("i=%d, i pointer=%p\n", i, &i);
printf("p=%p, p pointer=%p\n", p, &p); 

结果

j=10, j pointer=0028FF24
i=10, i pointer=0028FF28
p=0028FF28, p pointer=0028FF2C  

内存模型结构


变量名:    存储内容:   内存地址
  i:      10:         0028FF28
  p:      0028FF28:   0028FF2C
  j:      10          0028FF24

变量 i 的些信息

  变量名:      i
  变量存储地址: 0028FF28
  变量存储数据: 10

指针变量 p 的信息
  指针变量名:            p
  指针变量存储地址:      0028FF2C
  指针变量存储数据:      0028FF28
  `*p` => 指的是指针变量存储地址数据 0028FF28 地址的存储内容 10
  &p => 指的是指针变量存储地址 0028FF2C

指针变量 j 的信息
  指针变量名:            j
  指针变量存储地址:      0028FF24
  指针变量存储数据:      10
```

### sizeof 运算符

> 返回数据类型或变量在存储器中占用多少字节 (byte)

- sizeof(int) 4字节或8字节
- sizeof("hi") 3子节，包括\0

### 字符串指针

- `char msg[] = "Hello World";`
- 变量msg 是字符串Hello World的首字符H的地址
- sizeof(msg) 返回字符串指针大小
  - 32 bit OS 占用 4 byte
  - 64 bit OS 占用 8 byte

- 数组变量可以用作指针变量来使用
- 数组变量指向数组中第一个元素
- 函数参数声明为数组，它会当作指针处理
- sizeof 运算符返回某条数据占用空间的大小
- `sizeof(指针)` 32bit OS中返回4，64bit OS中返回8

### 运算符与函数的区别

> 编译器会把运算符编译为一串指令；而当程序调用函数时，会跳到独立的代码中执行
> sizeof 运算符在编译时确定存储空间的大小

### C 编译器通常会把 long 数据类型设为和存储器地址一样长

`long a = (long)p;`

### 数组变量与指针区别

- sizeof(数组变量) 返回数组长度
- sizeof(指针变量) 返回指针大小 4 or 8

- 指针变量是一个用来保存存储器中地址的变量
- 数组变量使用 & 运算符，结果就是数组变量本身
  - `char s[]="How";&s == s`

### 数组变量不能指向其他地方

> 创建指针变量时，计算机会为它分配 4 或 8 字节的存储空间。
> 创建数组变量时，会为数组空间分配存储空间，但不会为数组变量分配任何空间，编译器尽在出现它的地方把它替换成数组的起始地址。

- 注意：数组赋给指针变量，指针变量只会包含数组的地址信息，而对数组的长度一无所知，相当于丢失了一些信息。这样信息的丢失成为退化。

### 指针为什么有类型？

如果对char指针加1，指针会之乡存储器中下一个地址，那是因为char就占1子节

int指针占4字节，如果对int指针加1，编译后的代码就会对存储器地址加4

## 数组从0开始

```c
int drinks[] = {4,2,3};
drinks[0] == *drinks
drinks[1] == *(drinks+1)
drinks[2] == *(drinks+2)
drinks[2] == *(2+drinks)
drinks[3] == *(2+drinks) == 3[drinks]
a[1] == *(a+1) == *(1+a) == *(1+a) == 1[a]
a == &a[0]
```

**索引**的本质是**指针算数运算**，所以数组从0开始

指针变量具有类型，这样就能调整**指针算术运算**

### 用指针输入数据

```c
char name[40];
scanf("%39s",name);
// scanf()函数接受char指针，因为scanf()函数打算要更新数组的内容而不需要变量本身的值，它要的是变量的地址
```

### scanf() 会导致缓冲区溢出

> 缓冲区溢出很可能会导致程序出错，这种情况通常被称为**段错误或abort trap**，不管出现什么错误信息，程序都会崩溃

### fgets()

`fgets(缓冲区的指针,字符串包括\0的最大长度,stdin)  stdin: 数据将来自键盘`

### 字符串字面值不能更新

- 指向字符串字面值的指针变量不能用来修改字符串的内容: `char *cards = "JQK";`
1. 计算机加载字符串字面值;当计算机班程序载入存储器时，会版所有常数值放到常量存储区，这部分存储器是只读的。
2. 程序在栈上创建cards变量- 高位地址; 栈是存储器中计算机用来保存局部变量的部分，局部变量也就是位于函数内部的变量，cards 变量就这个地方
3. cards 变量设为 "JQK" 的地址; cards 变量将会保存**字符串字面值** "JQK" 的地址。为了防止修改，字符串字面值通常保存在子读存储器中
4. 计算机视图修改字符串; 程序视图修改 cards 变量指向的字符串中的内容时就会失败，因为字符串是只读的。


字符串字面值创建字符数组，可以修改: `char cards[] = "JQK";`;
`char cards[] = "JQK";` **没有给出数组的大小，必须立即赋值**

1. 计算机载入字符串字面值; 计算机吧程序载入存储器时，会把常量值保存到制度存储器
2. 程序在栈上新建了一个数组; 声明了数组，所以程序会创建一个足够大的数组来保存字符串"JQK"
3. 程序初始化数组; 为数组分配空间，程序还会把字符串字面值"JQK"的内容复制到栈上

原来的代码使用了指向只读字符串字面值的指针，而在新的代码中，用字符串字面值初始化了一个数组，从而得到了这些字母的副本，可以随意修改它们了。

`const char *s = "some thing";` 如果编译器试图修改字符串，就会提示编译错误

### 为什么数组变量不保存在存储器中

程序在编译期间，会版所有对数组变量的引用替换成数组的地址。也就是说在最后的可执行文件中，数组变量并不存在。既然数组变量从来不需要指向其他地方，有没有其实都一样。

- `scanf`: scanf formmatted 扫描带格式的输入

### 为什么编译不直接告诉我“不能修改字符串”

> `char *cards = "abcd";` chars声明的 `char *`，编译并不知道这个变量指向的时字符串字面值。

解决方案：加不加 const 是，字符串字面之都是只读的，但是 const修饰符表示，一旦你试图修用 const修饰过的变量去修改数组，编译器就会报错。

［示例 const char](./pointer/const_char.c)

## 结构体

> 组合不同数据类型的复合数据类型