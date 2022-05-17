# C Learning

Created: 2021-08-05 15:17:10 +0800

Modified: 2021-09-30 14:17:20 +0800

---


二分法

![· 解 释 ： 借 助 一 个 程 序 ， 那 个 程 序 能 试 图 理 解 你 的 程 序 ， 然 后 按 照 你 的 要 求 执 行 · 编 译 ： 借 助 一 个 程 序 ， 就 像 一 个 翻 译 ， 把 你 的 程 序 翻 译 成 计 算 机 真 正 能 懂 的 语 言 机 器 语 占 写 的 程 序 ， 然 后 ， 这 个 机 器 语 言 写 的 程 序 就 能 直 接 执 行 了 ](../media/学习笔记-C-Learning-image1.png){width="3.6333333333333333in" height="1.3916666666666666in"}

初始化 使用变量前先赋值一次

表达式 =

变量类型 不可改变

常量 值与类型都不可变

&:取地址符 使用scanf时 变量前要加上

整数四则运算（如/除）仍然是整数 会去掉小数部位 不会四舍五入（地板除）



运算符 算子

单目运算 取正取负 如：a*-b

先-b再*a

a=b=6 -> a=(b=6)



c没有幂次方 指数较小可以copypaste 多了就要用循环语句力

Printf("%",&) f:format

Scanf()

Int %d

float %f %.Xf

Double %lf

Const int a = ...

Fabs() 绝对值



交换变量

程序按步骤进行

中间量



断点 运行时停下来 此时可以通过分析界面看到变量值





复合赋值 四则运算与=结合：复合赋值运算符

total += a

total = total + a



total *= a+3

total = total * (a+3)



++ -- 递增递减运算符 作用就是给变量加减1

count++

count += 1

count = count + 1

**前缀后缀结果不同**

a++ 表达式输出的是以前的a 但这之后a已经加上1了

++a 表达式输出的是之后的a 此时a就是加上1后的a



16进制 %x 直接将输入的十进制整数换算为16进制数





























关系运算 结果是整数1（True）或0（False）

优先级比四则运算低 比赋值高

如int r = a>0; 如果a>0,则r=1

==/!= 比 >< 低

从左往右运算 如6>5>4 ->0





// 注释哟

/* */ 多行注释

ctrl + / 快速注释

else 不带大括号时 默认属于最近的if语句 保险起见加上大括号



if

else if

else



switch-case:

switch (控制表达式结果只能为整数) {

case 常量:

语句;

break;

case 常量:

语句;

break;

default:

语句

}



switch圆括号内整数只代表从哪个case开始执行 一直执行到break为止

while循环 循环体

do-while循环

先进循环 一次后再判断是否符合

do

{

<循环体语句>

} while (<判断语句>);



临界值

使用较小的值模拟运行程序 再代入较大的值



先保存最初的值



while里再放入scanf输入 这样可以多次输入

ret = ret * 10 + a 将原本的数左移一位 最后一位放入



For循环 计数循环 i达到某值前 "对于...当...时"

for (<初始动作>;<循环条件>;<循环动作>) {

<循环语句>

}



example:

for (int i=1;i<=n;i++) {

<...>

}



※求和初始0 求积初始1



注意循环条件与while相同 最开始检测

三个条件都可以省略 分号不能省



i=0;i<n /// i=1;i<=n 循环n次

前者最后i=n-1 后者最后i=n



※For == while

如果有固定次数，用for

如果必须执行一次，用do_while

其他情况用while



t 是



除号两边有一个是浮点数 结果就是浮点数 如 sum += 1.0 / n



循环控制

1.  定义一个变量 类似于布尔值 缺点：浪费循环次数

2.  使用break,continue语句

break 跳**出当前**循环 不进行下一轮

continue 跳**过**循环后面的语句 进行下一轮循环

接力 break

3.  goto <flag>; flag**:** 只在想要跳出多重循环时使用 不要滥用

次幂一定注意是乘自己n-1次



C语言是有类型的语言

整数：char int short long、long long

浮点数：float double、long double

逻辑： bool

指针

自定义类型



格式化名称不同

表达形式：补码 编码



sizeof() 给出类型或变量的长度 单位：字节

sizeof是静态的 表达式



整数

char 1字节 8比特

short 2字节

int 取决于编译器/cpu

int就是来表达寄存器的



long 取决于编译器/cpu

long long 8字节





负数

1.  flag 标志 表示它是负数

2.  取中间的数为0 如1000 0000为0

```{=html}
<!-- -->
```
3.  **补码**

1111 1111 纯二进制255 补码-1

补码 + 原码 = 溢出的零

如 1111 1111 + 0000 0001 = （1） 0000 0000

数的范围：char 一个字节：

0000 0000 --> 0

1111 1111 ~ 1000 0000 --> -1 ~ -128

0000 0001 ~ 0111 1111 --> 1 ~ 127

**-128 ~ 127 包括0 总共256个**



int 4/8个字节

00000000 00000000 00000000 00000000

**-2^32-1 ~ 2^32-1**



unsigned 不看作补码形式

unsigned char : 0-255











![. intgilong long • %d: int unsigned • %ld: long long unsigned long long ](../media/学习笔记-C-Learning-image2.png){width="3.408333333333333in" height="2.325in"}![](../media/学习笔记-C-Learning-image3.png){width="0.35in" height="0.45in"}![](../media/学习笔记-C-Learning-image4.png){width="2.325in" height="2.325in"}![](../media/学习笔记-C-Learning-image5.png){width="2.325in" height="2.325in"}![](../media/学习笔记-C-Learning-image6.png){width="0.15833333333333333in" height="0.2833333333333333in"}![](../media/学习笔记-C-Learning-image7.png){width="0.2in" height="0.3416666666666667in"}![](../media/学习笔记-C-Learning-image8.png){width="0.20833333333333334in" height="0.24166666666666667in"}![](../media/学习笔记-C-Learning-image9.png){width="0.175in" height="0.24166666666666667in"}![](../media/学习笔记-C-Learning-image10.png){width="0.19166666666666668in" height="0.13333333333333333in"}![](../media/学习笔记-C-Learning-image11.png){width="1.7in" height="1.7in"}![](../media/学习笔记-C-Learning-image12.png){width="1.7in" height="1.7in"}![](../media/学习笔记-C-Learning-image13.png){width="0.6666666666666666in" height="0.2916666666666667in"}![](../media/学习笔记-C-Learning-image14.png){width="0.6666666666666666in" height="0.2916666666666667in"}![](../media/学习笔记-C-Learning-image15.png){width="0.2833333333333333in" height="0.15833333333333333in"}![](../media/学习笔记-C-Learning-image16.png){width="0.2833333333333333in" height="0.15833333333333333in"}![](../media/学习笔记-C-Learning-image17.png){width="0.15in" height="0.20833333333333334in"}![](../media/学习笔记-C-Learning-image18.png){width="0.15in" height="0.20833333333333334in"}![](../media/学习笔记-C-Learning-image19.png){width="0.2in" height="0.225in"}![](../media/学习笔记-C-Learning-image20.png){width="0.6166666666666667in" height="0.21666666666666667in"}![](../media/学习笔记-C-Learning-image21.png){width="0.2in" height="0.225in"}![](../media/学习笔记-C-Learning-image22.png){width="0.20833333333333334in" height="0.13333333333333333in"}![](../media/学习笔记-C-Learning-image23.png){width="0.6166666666666667in" height="0.21666666666666667in"}![](../media/学习笔记-C-Learning-image24.png){width="0.20833333333333334in" height="0.13333333333333333in"}![](../media/学习笔记-C-Learning-image25.png){width="0.15in" height="0.44166666666666665in"}![](../media/学习笔记-C-Learning-image26.png){width="0.15in" height="0.44166666666666665in"}![](../media/学习笔记-C-Learning-image27.png){width="0.13333333333333333in" height="0.38333333333333336in"}![](../media/学习笔记-C-Learning-image28.png){width="0.21666666666666667in" height="0.30833333333333335in"}![](../media/学习笔记-C-Learning-image29.png){width="0.13333333333333333in" height="0.38333333333333336in"}![](../media/学习笔记-C-Learning-image30.png){width="0.24166666666666667in" height="0.2833333333333333in"}![](../media/学习笔记-C-Learning-image31.png){width="0.225in" height="0.25833333333333336in"}![](../media/学习笔记-C-Learning-image32.png){width="0.225in" height="0.3in"}![](../media/学习笔记-C-Learning-image33.png){width="0.25in" height="0.175in"}![](../media/学习笔记-C-Learning-image34.png){width="0.20833333333333334in" height="0.15in"}



8进制 0开头 **%o**

16进制 0x开头 **%x**

只是我们自己怎么看 内存中永远是二进制

无特殊需要 就用 int 或者 float

![O. Eint.nan ](../media/学习笔记-C-Learning-image35.png){width="1.55in" height="0.525in"}

**浮点数**

float 32bit 7位[有效数字]{.underline}

scanf("%f") printf("%f %e")

double 64bit 15位

scanf("%lf") printf("%f %e")



多位小数%.5f **四舍五入**

![](../media/学习笔记-C-Learning-image36.png){width="1.7083333333333333in" height="0.9916666666666667in"}

能准确输出的位数



比如double a = 0.0049;

printf("%.30f",a);

-->0.004899999999999999841793218991



离散的数 浮点数就是不准确

科学计数法

-5.67E+12

6.54E-4



混合输入



%d %d

%d%d

浮点数可以表示无穷大

两个浮点数f1 == f2 可能失败

1.345 是 double

1.345f 是 float



浮点数 1bit符号 11bit指数 52bit数值

专用浮点计算



没有特殊需要 double

![逃 逸 字 符 字 符 ib it in 意 义 回 退 一 格 到 下 一 个 表 格 位 换 行 回 车 字 符 意 义 双 引 号 单 引 号 反 斜 杠 本 身 ](../media/学习笔记-C-Learning-image37.png){width="4.091666666666667in" height="3.408333333333333in"}

字符类型

char是整数 也是字符

'a','1' 字符1

char d = '1'

%d,d --> 49

%c 输入输出

ASCII码 以整数代表字符



字符计算 ascii计算

两个字符相减得到整数为距离

一个字符加一个数字得到下一个字符 ++a



大小写转换：

a + ('a' - 'A')

b Backspace

t 制表符 table



类型转换

当运算符两边出现不一致的类型时 会自动转换成较大的类型

![• char ---> short ---> int ---> long ---> long long • int ---> float ---> double ](../media/学习笔记-C-Learning-image38.png){width="4.9in" height="0.8in"}



强制转换：
(类型)值 例如： (int)10.2 (short)

(short)32768 = -32768 因为short最大是32767 所以会导致溢出

(char)32768 = 0

32768 = 10000000 00000000 只取后8bits

**不改变原变量的类型与值**





布尔值

c语言最初没有bool 只有0 1



#include <stdbool.h>

就可以使用 bool

True False 但是不会输出true false



![优 先 级 运 算 符 结 合 性 从 左 到 右 从 右 到 左 （ 单 目 的 + 和 · ） 从 左 到 右 从 左 到 右 从 左 到 右 从 左 到 右 从 左 到 右 从 左 到 右 从 右 到 左 ](../media/学习笔记-C-Learning-image39.png){width="4.366666666666666in" height="3.4in"}

逻辑运算

| !    | 逻辑非 | 最高 |
|------|--------|------|
| &&   | 逻辑与 | 中间 |
| || | 逻辑或 | 最低 |





4<x<6 不对

x>4 && x<4



判断是否大写：

c >= 'A' && c <= 'Z'



![· 逻 辑 运 算 是 自 左 向 右 进 行 的 ， 如 果 左 边 的 结 果 已 经 能 够 决 定 结 果 了 ， 就 不 会 做 右 边 的 计 算 ](../media/学习笔记-C-Learning-image40.png){width="4.166666666666667in" height="1.3in"}

条件运算符

<1> ? <2> : <3>

1.  条件

2.  条件满足后的值

3.  条件不满足后的值

逗号也是一种运算符

(3+4,5+6) 逗号表达式结果是 11



for ( i=0 , j=11 ; i<j ; i++, j--) {}



函数 一块代码

返回一个值或者零个值



<函数头> {

<函数体>

}



void 函数不返回值

int 返回整数



调用： 函数名(参数);

没有参数也要()



return 停止函数的执行

return;

return 表达式;



单一出口



函数先后关系



函数原型（声明）：

将函数头放在开头

函数体可以放在后面（也要加函数头）



传给函数的只能是值

每个函数有自己的变量空间



[函数头里的叫形参]{.underline}

[给函数的叫实参]{.underline}



定义在函数里的变量叫本地变量

参数也是



![· 生 存 期 ： 什 么 时 候 这 个 变 量 开 始 出 现 了 ， 到 什 么 时 候 它 消 亡 了 · 作 用 域 ： 在 （ 代 码 的 ） 什 么 范 围 内 可 以 访 问 这 个 变 量 （ 这 个 变 量 可 以 起 作 用 ） ． 对 于 本 地 变 量 ， 这 两 个 问 题 的 答 案 是 统 一 的 ： 大 括 号 内 一 ． ． 一 块 ](../media/学习笔记-C-Learning-image41.png){width="3.35in" height="1.2833333333333334in"}

变量可以在块内

也可以在语句内 但是在语句外就消失了

里面的块定义同名

![· 本 地 变 量 是 定 义 在 块 内 的 · 它 可 以 是 定 义 在 函 数 的 块 内 ． 也 可 以 定 义 在 语 句 的 块 内 ． 甚 至 可 以 随 便 拉 一 对 大 括 号 来 定 义 变 量 · 程 序 运 行 进 入 这 个 块 之 前 ， 其 中 的 变 量 不 存 在 ， 离 开 这 个 块 ， 其 中 的 变 量 就 消 失 了 ． 块 外 面 定 义 的 变 量 在 里 面 仍 然 有 效 · 块 里 面 定 义 了 和 外 面 同 名 的 变 量 则 掩 盖 了 外 面 的 · 不 能 在 一 个 块 内 定 又 同 名 的 变 量 · 本 地 变 量 不 会 被 默 认 初 始 化 。 参 数 在 进 人 函 数 的 时 候 被 初 始 化 了 ](../media/学习笔记-C-Learning-image42.png){width="4.416666666666667in" height="2.591666666666667in"}



**数组**

int number[100]; //定义数组 单元是int

number数组可放100个int

number[cnt] = x; //对数组中第cnt位进行赋值

number[i] ...

元素数量是整数

数组从0开始 下标/索引



数组越界 最大是

segmentation fault



**注意数组初始化**

**遍历数组输出**



a[] = {1,2,5,33,6,2,9} 数组大小为7

a[7] = {2} --> 2,0,0,0,0,0,0 除了第0索引其他都为0

初始化：a[<数量>] = {0};



**定位赋值**

![int a [ 10 ] = Y; C99 0 = 3 ， 6 · 用 [ n ] 在 初 始 化 数 据 中 给 出 定 位 · 没 有 定 位 的 数 据 接 在 前 面 的 位 置 后 面 · 其 他 位 置 的 值 补 零 · 也 可 以 不 给 出 数 组 大 小 ， 让 编 译 器 算 · 特 别 适 合 初 始 数 据 稀 疏 的 数 组 ](../media/学习笔记-C-Learning-image43.png){width="4.208333333333333in" height="3.225in"}

↑{2,0,0,3,6,0,0,0,0,0}



数组大小：

sizeof 求数组占用的字节

sizeof(a) / sizeof(a[0]) 可以求出数组大小



数组不能整个赋值给另外一个数组 只能遍历

**int b[] = a;**



遍历数组

通常使用for循环 i从0到<数组长度



数组作为函数参数时，往往必须再用另一个参数来传入数组的大小



调试技巧：使用大括号 里面的变量就是局部的了



二维数组

int a[3][5]

三行五列的矩阵

![](../media/学习笔记-C-Learning-image44.png){width="3.566666666666667in" height="2.175in"}

二维数组遍历需要两次循环

a[i,j] == a[j]



行可以省略 列不可以省略

![int at] [ 5 ] { ‰ 1 ， 乙 3 ， 4}, { 荔 3 ， ‰ 5 ， 6 } " ． 列 数 是 必 须 给 出 的 ， 行 数 可 以 由 编 译 器 来 数 · 每 行 一 个 { } ， 逗 号 分 隔 · 最 后 的 逗 号 可 以 存 在 ， 有 古 老 的 传 统 · 如 果 省 略 ， 表 示 补 零 · 也 可 以 用 定 位 （ *C990NLY) ](../media/学习笔记-C-Learning-image45.png){width="3.308333333333333in" height="2.4in"}



取地址运算

& 获得变量的地址 必须对变量取地址

取地址符右边必须是明确的变量

%x十六进制

%p指针 地址 以十六进制



堆栈 分配内存自高向低



数组 0 1 2 3 4 ······

低 高



指针变量 保存地址的变量

int *p = &i; 将i的地址交给指针p p指向i

int *p 定义一个指针变量p

*p *q *是给变量的 不是给int的



可以在函数内访问其他变量



*单目运算符 *p可以作左值也能做右值



*p作为整体相当于整数

如果事先定义了p为指针 那p用%p输出的就是地址



*p = 26 改变了i的值 i=26





![· 互 相 反 作 用 · *&yptr 一 > * (&yptr) 一 > * (yptr 的 地 址 ） 一 > 得 到 那 个 地 址 上 的 变 量 一 > yptr ． &*yptr ． > &(*yptr) ． > &(y) ． > 得 到 y 的 地 址 ， 也 就 是 yptr " yptr ](../media/学习笔记-C-Learning-image46.png){width="3.6333333333333333in" height="1.6916666666666667in"}

出现在赋值号左边的是值 可以接收右值



指针应用场景：

函数需要返回多个值



函数运行状态通过return返回

实际计算结果通过指针返回



指针没有得到地址之前 不能对其赋值 访问



数组传进函数中 sizeof大小就变了

因为数组传进函数就是以指针形式传入的

函数参数表里的数组是指针

有a[] = {}则：

int a[] int *a 两者相同

sizeof(a) == sizeof(int *)2



数组变量本身表达的就是地址

但是对单个单元需要用&取地址

a == &a[0] 首地址

p[0] <==> a[0]



把数组变量当作指针用也可以

相当于 int a[] == int *const a;

**指针是CONST**

![． 表 示 不 能 通 过 这 个 指 针 去 修 改 那 个 变 量 （ 并 不 能 使 得 那 个 变 量 成 为 c 。 nst ） 0 const int p = &i; p = 26 ； / / ERROR! (*p) 是 const i = 2 & //OK p = &j;//OK ](../media/学习笔记-C-Learning-image47.png){width="3.908333333333333in" height="1.8416666666666666in"}



Const在 * 的前面 --》 指针所指的不能被修改

后面 --》 指针不能被修改

![亓 扌 旨 是 const ． 表 示 不 能 通 过 这 个 指 针 去 修 改 那 个 变 量 〈 并 不 能 使 得 那 个 变 量 成 为 c 。 nst ） const int p = &i; p = 26 ； / / ERROR! (*p) 是 const i = 26 ； //OK p = &j;//OK ](../media/学习笔记-C-Learning-image48.png){width="3.4583333333333335in" height="2.4583333333333335in"}![const 数 组 · const int al] = {I ， 2 ， 3 ， 4 ， 5 ， 6 ， } ； 数 组 变 量 已 经 是 c 。 nst 的 指 针 了 ， 这 里 的 const 表 明 数 组 的 每 个 单 元 都 是 （ 。 nst int · 所 以 必 须 通 过 初 始 化 进 行 赋 值 ](../media/学习笔记-C-Learning-image49.png){width="3.2083333333333335in" height="2.2916666666666665in"}![· 因 为 把 数 组 传 入 函 数 时 传 递 的 是 地 址 ， 所 以 那 个 函 数 内 部 可 以 修 改 数 组 的 值 · 为 了 保 护 数 组 不 被 函 数 破 坏 ， 可 以 设 置 参 数 为 const · int sum(const int al], int length); ](../media/学习笔记-C-Learning-image50.png){width="3.775in" height="1.4583333333333333in"}

指针+1 ： 下一个单元 即移动sizeof(类型)个字节

*p --> ac[0]

*(p+1) --> ac[1]

*(p+n) --> ac[n]

+ 后移 - 前移 p++ p--

p-q 指针相减 得到地址间有几个单元 (sizeof )

如果指针不是指向一片连续分配的空间,如数组，则这种运算没有意义



*p++ 先取出p指向的数据 然后将p指向下一个数据

连续空间操作 如遍历



指针可以比较 --> 地址大小比较

![0 地 址 ． 当 然 你 的 内 存 中 有 0 地 址 ， 但 是 0 地 址 通 常 是 个 不 能 随 便 碰 的 地 址 ． 所 以 你 的 指 针 不 应 该 具 有 0 值 ． 因 此 可 以 用 0 地 址 来 表 示 特 殊 的 事 情 ： ． 返 回 的 指 针 是 无 效 的 ． 指 针 没 有 被 真 正 初 始 化 （ 先 初 始 化 为 0 ） ． NULL 是 一 个 预 定 定 义 的 符 号 ， 表 示 0 地 址 ． 有 的 编 译 器 不 愿 意 你 用 0 来 表 示 0 地 址 ](../media/学习笔记-C-Learning-image51.png){width="3.6in" height="2.8333333333333335in"}



![指 针 的 类 型 ． 无 论 指 向 什 么 类 型 ， 所 有 的 指 针 的 大 小 都 是 一 样 的 ， 因 为 都 是 地 址 ． 但 是 指 向 不 同 类 型 的 指 针 是 不 能 直 接 互 相 赋 值 的 ． 这 是 为 了 避 免 用 错 指 针 ](../media/学习笔记-C-Learning-image52.png){width="3.325in" height="2.2in"}



![指 针 的 类 型 转 换 ． void* 表 示 不 知 道 指 向 什 么 东 西 的 指 针 ． 计 算 时 与 char * 相 同 （ 但 不 相 通 ） ． 指 针 也 可 以 转 换 类 型 int p = &i;void*q = (void )p, ． 这 并 没 有 改 变 p 所 指 的 变 量 的 类 型 ， 而 是 让 后 人 用 不 同 的 眼 光 通 过 p 看 它 所 指 的 变 量 我 不 再 当 你 是 int 啦 ， 我 认 为 你 就 是 个 v 。 id ！ ](../media/学习笔记-C-Learning-image53.png){width="3.566666666666667in" height="2.675in"}![． 需 要 传 入 较 大 的 数 据 时 用 作 参 数 ． 传 人 数 组 后 对 数 组 做 操 作 ． 函 数 返 回 不 止 一 个 结 果 ． 需 要 用 函 数 来 修 改 不 止 一 个 变 量 ． 动 态 申 请 的 内 存 ， ， ， ](../media/学习笔记-C-Learning-image54.png){width="6.041666666666667in" height="2.7333333333333334in"}

动态内存分配 **C99之前**不能用变量指定数组长度

malloc(字节大小)

#include <stdlib.h>;

返回的是void* 还要用(int*)转换一下

a = (int*)malloc(n*sizeof(int));

之后就是当作数组来用

最后还要清除空间 free()



申请失败则返回 0 或者 NULL

![申 请 了 没 free 一 > 长 时 间 运 行 内 存 逐 渐 下 降 ](../media/学习笔记-C-Learning-image55.png){width="4.041666666666667in" height="0.4083333333333333in"}

地址改变过就不能free了



养成习惯 malloc配上free







字符串



字符串**常量**

char *s = "Hello World";



代码段 只读 不可改写

![char* S ' He110 ， world ！ " ． ． s 是 一 个 指 针 ， 初 始 化 为 指 向 一 个 字 符 串 常 量 由 于 这 个 常 量 所 在 的 地 方 ， 所 以 实 际 上 s 是 const char* s ， 但 是 由 于 历 史 的 原 因 ， 编 译 器 接 受 不 带 const 的 写 法 。 但 是 试 图 对 s 所 指 的 字 符 串 做 写 入 会 导 致 严 重 的 后 果 ](../media/学习笔记-C-Learning-image56.png){width="4.633333333333334in" height="1.8in"}

如果需要修改字符串 可使用数组：

char s[] = "Hello World!";

"Hello World!" 被复制到s[]里 此时可写



![指 针 还 是 数 组 9 · char *str = "Hello ， · char word[] = "Hello' ． 数 组 ： 这 个 字 符 串 在 这 里 ． 作 为 本 地 变 量 空 间 自 动 被 回 收 ． 指 针 ： 这 个 字 符 串 不 知 道 在 哪 里 ． 处 理 参 数 ． 动 态 分 配 空 间 ](../media/学习笔记-C-Learning-image57.png){width="3.025in" height="2.7in"}



%s 读入一个单词 到空格tab回车为止

%7s 最多读入7个字符



**a 指针a指向一个指针 再指向一个地址

a[][10] 二维数组 每一个字符串最长为10

*a[] 指针数组 存放的全部都是指针 指向字符串



main(int argc , char const *argv[] ) 符号链接



putchar(int c) 返回int

向标准输出写一个字符

getchar(void) 返回int

从标准输出读入一个字符

EOF代表结束

Ctrl+Z 然后**回车** 即输入一个EOF



string.h





字符串函数

strlen 字符串长度





strcmp 比较字符串

0 ： s1 == s2

1 ：s1 > s2

-1 ：s1 < s2

比较得出的是 字符串不相等字符的差值

char s1[] = "abc";

char s2[] = "bbc";

printf("%d",strcmp(s1,s2));

--> -1 因为'a' - 'b' == -1



将字符串当作数组||当作指针遍历



strcpy()



char * strcpy(char *restrict dst, const char *restrict src)

返回的是dst这个字符串数组 dst是变量 src是字符串常量

restrict 不重叠

**将第二个字符串拷贝到第一个字符串**

复制一个字符串

char *dst = (char*)malloc(strlen(src)**+1**)

先申请一个内存空间

然后再strcpy(dst,src)



strcat()

char * strcat(char *restrict s1, const char *restricts2);

将s2拷贝到s1后面形成新的字符串

返回s1

s1要有足够的空间

s2的第一个字符会把s1最后的0覆盖

即dst[strlen(dst++)] = src[i++];



安全问题strcpy strcat

没有足够空间

安全版本 strncpy strncpy strncmp 参数里加一个整数

![• char * strncpy(char *restrict dst, const char *restrict src, size_t n); • char * strncat(char *restrict s I, const char *restrict s2, size_t n); • int strncmp(const char *s 1, const char *s2, size_t n); ](../media/学习笔记-C-Learning-image58.png){width="4.333333333333333in" height="1.6166666666666667in"}

strncmp 用处 只判断前n个字符



strchr(字符串,字符)

返回指针

strstr(字符串,字符串)

strcasestr()



常量符号化



枚举 用户定义的数据类型

enum NAME {NAME1,NAME2,NAME......};

类型名字不一定要有

name的类型是int 由0到n

给常量名字



可以当作数据类型使用

enum color t = red;



套路：枚举可以自动计数

enum COLOR {RED,YELLOW,GREEN,NumCOLORS};



枚举量可以指定值



![枚 举 ． 虽 然 枚 举 类 型 可 以 当 作 类 型 使 用 ， 但 是 实 际 上 很 (bu) 少 (hao) 用 ． 如 果 有 意 义 上 排 比 的 名 字 ， 用 枚 举 比 const int 方 便 · 枚 举 比 宏 (mac ℃ ） 好 ， 因 为 枚 举 有 nt 类 型 ](../media/学习笔记-C-Learning-image59.png){width="3.35in" height="2.2083333333333335in"}





结构类型 structure

struct structName {

int a;

int b;

int c;

////

}; <--分号要有



![． 和 本 地 变 量 一 样 ， 在 函 数 内 部 声 明 的 结 构 类 型 只 能 在 函 数 内 部 使 用 ． 所 以 通 常 在 函 数 外 部 声 明 结 构 类 型 ， 这 样 就 可 以 被 多 个 函 数 所 使 用 了 ](../media/学习笔记-C-Learning-image60.png){width="4.616666666666666in" height="1.5in"}

要用的时候类似于int x; char ch;这样

struct structName variantName;

或者直接：

![struct point { int x; int y; } pi,R2; ](../media/学习笔记-C-Learning-image61.png){width="2.1in" height="2.1666666666666665in"}



![． 要 访 问 整 个 结 构 ， 直 接 用 结 构 变 量 的 名 字 ． 对 于 整 个 结 构 ， 可 以 做 赋 值 、 取 地 址 ， 也 可 以 传 递 给 函 数 参 数 = (struct point){5, | 0 } ； / / 相 当 于 p = 5 ； ． 国 = p2 ； // 相 当 于 pl.x=p2.x;pl.y=p2.y; 擞 组 无 法 做 这 两 种 运 算 ！ ](../media/学习笔记-C-Learning-image62.png){width="4.566666666666666in" height="2.2583333333333333in"}



![． 和 数 组 不 同 ， 结 构 变 量 的 名 字 并 不 是 结 构 变 量 的 地 址 ， 必 须 使 用 & 运 算 符 ． struct date pDate = &today; ](../media/学习笔记-C-Learning-image63.png){width="4.391666666666667in" height="1.1in"}



没有直接的方式一次输入一个结构

函数得到的只能是值 结构变量里的值



1.  建立一个临时的结构变量 返回结构

struct Name Func(void) {

struct Name p;

...

return p;

}

x = Func();

x == p



2.  传递指针 性能更好

![struct date { int month; int day; int year; myday; struct date (*P) .month 12 p--->month = 12 = &myday; ](../media/学习笔记-C-Learning-image64.png){width="3.066666666666667in" height="1.8833333333333333in"}

->



结构数组

Name[2] = {{a,b,c},{x,y,z}};

Name[i].first = ...



结构中也可以有结构



![· C 语 言 提 供 了 一 个 叫 做 pedef 的 功 能 来 声 明 一 个 已 有 的 数 据 类 型 的 新 名 字 。 比 如 ， typedef int Length; 使 得 Length 成 为 int 类 型 的 别 名 。 这 样 ， Length 这 个 名 字 就 可 以 代 替 int 出 现 在 变 量 定 义 和 参 数 声 明 的 地 方 了 ， Length a, b,len ， Length •numbers 卩 0] ， ](../media/学习笔记-C-Learning-image65.png){width="5.75in" height="2.691666666666667in"}![Typedef 声 明 新 的 类 型 的 名 字 · 新 的 名 字 是 某 种 类 型 的 别 名 · 改 善 了 程 序 的 可 读 性 ypedef long 攵 nt64 t; ypedef struct ADate { int month ： int day ， int year ， Date; nt64 t 土 = ate d = { 9 ， 重 载 已 有 的 类 型 名 字 新 名 字 的 含 义 更 清 晰 具 有 可 移 植 性 简 化 了 复 杂 的 名 字 100000000000 ； 1 ， 2005 } ； ](../media/学习笔记-C-Learning-image66.png){width="5.791666666666667in" height="4.291666666666667in"}



联合 跟sturct很像 但是里面的元素共用一个内存空间



x86 小端 Little Endian 低位在前



本地变量



全局变量 定义在函数外面

具有全局的生存期和作用域

所有函数都可以使用全局变量

任何函数里都可以更改全局变量



没有初始化的活会得到 0 值

指针的话是NULL值

初始化必须是确定值（常量）



函数里有个同名的变量 全局变量会被隐藏



__func__ 当前函数名的字符串





静态本地变量： static xxx xxx = xxx;

第一次进入时定义变量 退出函数时保留变量值

静态本地变量其实就是全局变量 在内存里是紧挨着的

作用域只在函数中



返回指针的函数

1.  返回本地变量有危险 不受控 但是还存在

2.  返回全局变量或静态本地变量是安全的

3.  最好的是返回指针

不要使用全局变量来传递参数和结果

函数可重入/不可重入



编译预处理指令

#开头

#define 定义一个宏



#define PI 3.14159

其实就是**文本替换**

#define format "%d"



![#define · #define < 名 字 > < 值 > 注 意 没 有 结 尾 的 分 号 ， 因 为 不 是 C 的 语 句 · 名 字 必 须 是 一 个 单 词 ， 值 可 以 是 各 种 东 西 · 在 C 语 言 的 编 译 器 开 始 编 译 之 前 ， 编 译 预 处 理 程 序 (cpp) 会 把 程 序 中 的 名 字 换 成 值 完 全 的 文 本 替 换 gCC ---save-temps ](../media/学习笔记-C-Learning-image67.png){width="4.425in" height="3.2083333333333335in"}

宏里有其他宏的名字 又会进行一次替换



![预 定 义 的 宏 凵 NE_ FILE_ DATE--- TIME_ STDC ](../media/学习笔记-C-Learning-image68.png){width="3.4833333333333334in" height="3.025in"}



带参数的宏

#define cube(x) ((x)*(x)*(x))

![带 参 数 的 宏 的 原 则 一 切 都 要 括 号 · 整 个 值 要 括 号 · 参 数 出 现 的 每 个 地 方 都 要 括 号 · #define RADTODEG(x) （ （ × 厂 57 ． 29578 ） ](../media/学习笔记-C-Learning-image69.png){width="4.125in" height="2.8in"}



![多 个 ℃ 文 件 · main() 里 的 代 码 太 长 了 适 合 分 成 几 个 函 数 一 个 源 代 码 文 件 太 长 了 适 合 分 成 几 个 文 件 · 两 个 独 立 的 源 代 码 文 件 不 能 编 译 形 成 可 执 行 的 程 序 ](../media/学习笔记-C-Learning-image70.png){width="3.8333333333333335in" height="2.325in"}

头文件

#include

把文件所有内容插入那一行

![还 是 < > · # include 有 两 种 形 式 来 指 出 要 插 入 的 文 件 ' 要 求 编 译 器 首 先 在 当 前 目 录 （ ℃ 文 件 所 在 的 目 录 ） 寻 找 这 个 文 件 ， 如 果 没 有 ， 到 编 译 器 指 定 的 目 录 去 找 · < > 让 编 译 器 只 在 指 定 的 目 录 去 找 · 编 译 器 自 己 知 道 自 己 的 标 准 库 的 头 文 件 在 哪 里 · 环 境 变 量 和 编 译 器 命 令 行 参 数 也 可 以 指 定 寻 找 头 文 僻 的 目 录 ](../media/学习笔记-C-Learning-image71.png){width="5.325in" height="3.8666666666666667in"}



![#include 的 误 区 · # inc | ude 不 是 用 来 引 入 库 的 · stdio.h 里 只 有 printf 的 原 型 ， printf 的 代 码 在 另 外 的 地 方 ， 某 个 .1ib(Windows) 或 .a(Unix) 中 · 现 在 的 C 语 言 编 译 器 默 认 会 引 入 所 有 的 标 准 库 · #include < stdio.h > 只 是 为 了 让 编 译 器 知 道 printf 函 数 的 原 型 ， 保 证 你 调 用 时 给 出 的 参 数 值 是 正 确 的 类 型 ](../media/学习笔记-C-Learning-image72.png){width="5.4in" height="3.6416666666666666in"}



![· 在 使 用 和 定 义 这 个 函 数 的 地 方 都 应 该 # include 这 个 头 文 件 一 般 的 做 法 就 是 任 何 ℃ 都 有 对 应 的 同 名 的 上 ， 把 所 有 对 外 公 开 的 函 数 的 原 型 和 全 局 变 量 的 声 明 都 放 进 去 ](../media/学习笔记-C-Learning-image73.png){width="5.8in" height="1.55in"}



![不 对 外 公 开 的 函 数 · 在 函 数 前 面 加 上 static 就 使 得 它 成 为 只 能 在 所 在 的 编 译 单 元 中 被 使 用 的 函 数 · 在 全 局 变 量 前 面 加 上 static 就 使 得 它 成 为 只 能 在 所 在 的 编 译 单 元 中 被 使 用 的 全 局 变 量 ](../media/学习笔记-C-Learning-image74.png){width="5.775in" height="3.55in"}





Clion 调试模式下不能用printf输出

在printf语句前加上 setbuf(stdout, 0);



**extern** int xxx; 注意不能初始化

在头文件中声明全局变量xxx

![inti ； 是 变 量 的 定 义 · extern int i ； 是 变 量 的 声 明 ](../media/学习笔记-C-Learning-image75.png){width="2.975in" height="0.8833333333333333in"}



![· 声 明 是 不 产 生 代 码 的 东 西 · 函 数 原 型 · 变 量 声 明 ． 结 构 声 明 宏 声 明 · 枚 举 声 明 · 类 型 声 明 · inline 函 数 定 义 是 产 生 代 码 的 东 西 ](../media/学习笔记-C-Learning-image76.png){width="3.2in" height="4.116666666666666in"}





条件编译

避免头文件重复引用

标准头文件结构:

#ifndef __LIST_HEAD__

#define __LIST_HEAD__



#include "name.h"



#endif

![· 运 用 条 件 编 译 和 宏 ， 保 证 这 个 头 文 件 在 一 个 编 译 单 元 中 只 会 被 # include 一 次 · #pragma once 也 能 起 到 相 同 的 作 用 ， 但 是 不 是 所 有 的 编 译 器 都 支 持 ](../media/学习笔记-C-Learning-image77.png){width="4.666666666666667in" height="1.4333333333333333in"}



![Flag (space) ](../media/学习笔记-C-Learning-image78.png){width="6.225in" height="4.0in"}![width 或 prec number .number 含 义 最 小 字 符 数 下 一 个 参 数 是 字 符 数 小 数 点 后 的 位 数 下 一 个 参 数 是 小 数 点 后 的 位 数 ](../media/学习笔记-C-Learning-image79.png){width="5.775in" height="3.675in"}![Il short long long long long double ](../media/学习笔记-C-Learning-image80.png){width="5.116666666666666in" height="4.541666666666667in"}![type i 或 d ， 0 X f 或 F e 或 E 用 于 int unsigned int 丿 进 制 十 六 进 制 字 母 大 写 的 十 六 进 制 float, 6 指 数 type G a 或 A C S P 用 于 float float 十 六 进 制 浮 点 Char 字 符 串 指 针 读 入 / 写 出 的 个 数 ](../media/学习笔记-C-Learning-image81.png){width="6.683333333333334in" height="4.1in"}

%n 输出到这里时有多少个字符 后面可以跟&变量名 填充到变量里面



![flag hh scanf: char short %[flag]type flag Il long, double long long long double ](../media/学习笔记-C-Learning-image82.png){width="6.108333333333333in" height="4.466666666666667in"}![scanf. 用 于 int %[flag]type type d 0 C 整 数 ， 可 能 为 十 六 进 制 或 八 进 制 unsigned int 丿 进 制 十 六 进 制 float Char type S P 用 于 字 符 串 （ 单 词 ） 所 允 许 的 字 符 指 针 ](../media/学习笔记-C-Learning-image83.png){width="5.966666666666667in" height="4.125in"}



![printf 和 scanf 的 返 回 值 · 读 入 的 项 目 数 · 输 出 的 字 符 数 在 要 求 严 格 的 程 序 中 ， 应 该 判 断 每 次 调 用 scanf 或 printf 的 返 回 值 ， 从 而 了 解 程 序 运 行 中 是 否 存 在 问 题 ](../media/学习笔记-C-Learning-image84.png){width="6.341666666666667in" height="4.075in"}



![W W+ fopen 打 开 只 读 打 开 读 写 ， 从 文 件 头 开 始 打 开 只 写 。 如 果 不 存 在 则 新 建 ， 如 果 存 在 则 清 空 打 开 读 写 。 如 果 不 存 在 则 新 建 ， 如 果 存 在 则 清 空 打 开 追 加 。 如 果 不 存 在 则 新 建 ， 如 果 存 在 则 从 文 件 尾 开 始 只 新 建 ， 如 果 文 件 已 存 在 则 不 能 打 开 ](../media/学习笔记-C-Learning-image85.png){width="7.091666666666667in" height="3.775in"}



![程 序 为 什 么 要 文 件 · 配 置 · Uni × 用 文 本 ， Windows 用 注 册 表 · 数 据 ． 稍 微 有 点 量 的 数 据 都 放 数 据 库 了 · 媒 体 · 这 个 只 能 是 二 进 制 的 · 现 实 是 ， 程 序 通 过 第 三 方 库 来 读 写 文 件 ， 很 少 直 接 读 写 二 进 制 文 件 了 ](../media/学习笔记-C-Learning-image86.png){width="6.591666666666667in" height="4.825in"}

位运算 to be learned



可变数组

1.  可增大

2.  可获取大小

3.  可访问

自定义一个函数



![Array array_create(int init_size); void array_free(Array *a); int array_size(const Array *a); int* array_at(Array *a, int index); void array_infIate(Array *a, int more_size); ](../media/学习笔记-C-Learning-image87.png){width="4.366666666666666in" height="2.225in"}



![typedef struct { int *array; int size; Array; ](../media/学习笔记-C-Learning-image88.png){width="1.8in" height="0.9083333333333333in"}

struct array not *array

最好不要定义出一个指针类型



链表

数据+指针------节点Node

![typedef struct _ node { int value; struct _ node *next; } Node h #endif ](../media/学习笔记-C-Learning-image89.png){width="2.5833333333333335in" height="1.375in"}

可变数组的替代





用一个list来代表链表 方便日后对list中的指针做修改

链表函数



链表搜索 遍历

![](../media/学习笔记-C-Learning-image90.png){width="3.6in" height="0.24166666666666667in"}

非常常见的写法 p=p->next



当用到->时 指针不能为NULL

边界条件

要有足够的代码保证指针安全







qsort函数

void qsort(void *arr, size_t n, size_t size, int (*cmp)(const void *, const void *))

arr表示要排序的序列，可以是数组名或指针

n表示arr中有多少待排序的元素

size表示arr中单个元素的大小（如果是动态分配内存的序列要注意）

cmp表示自定义的比较函数（核心！！！）



cmp的框架



int cmp(const void *a, const void *b) {

return;

}



升序

int cmp(const void *a, const void *b) {

return *(int*)a - *(int*)b; /* a、b分别指向arr1/arr2中某个元素 */

}






























































































