# cpp learning

Created: 2021-10-30 17:50:39 +0800

Modified: 2021-12-13 15:31:15 +0800

---


using namespace std; 名称空间



想要用c的头文件 就在名称前加个c 比如

string.h --> cstring



cin cout



getline(cin,string) 获取一整行内容

跟gets类似



string.length();



字串：

s.substr(n,m) n:从哪个index开始

m: 取几个字符



s.substr(n) 从n开始 后面全部



结构体：

不需要typedef了 直接用结构体名称定义



引用：

cpp里面函数参数可以加上& （跟取地址不同）

例如：

int pow(int &num, int exp)

{

int t = num;

for ( int i=0 ; i<exp ; i++ )

{

num *= t;

}

}



类似指针改变变量的值 省去&变量



STL：



vector（向量/可变数组）：

vector 头文件

vector 可以当作可变数组

v.begin() --> head ; v.end() --> tail后面

这两返回的都是指针

size(大小,初始化值) 数组大小 没有设定初始化值默认为

v.resize(大小) 重新分配大小 会将数组初始化

v.push_back(数据) 追加新数据

访问也是[]

迭代器（类似python）:
for ( auto 迭代器 = 初始 ; 结束 ; 迭代器++)

迭代器相当于指针 后面要用 *迭代器 才能用

或者直接 for ( int 迭代器 : 数组 ) 从头开始遍历



set（集合）：

set 头文件

元素不重复，自动升序排序

集合不能预分配空间 也不能初始化

set<int> a;

a.insert(data) 插入

a.find(data) 查找有无这个元素 **返回的是指向该元素的指针**

a.erase(data) 若存在该值 进行删除

一个用法： a.find() != a.end() 表示如果遍历完之前找到该元素 返回true 到了end()还没找到返回0



map（键值对）:

map 头文件

map<string,int>m 键是string 值是int

自动按照键升序排序 实质是结构体数组

添加： m["key"] = value;

访问： m["key"] 存在返回值 不存在返回0

遍历： 迭代器 auto用法一样 后面有点像指针访问结构体

p->first p->second



stack（栈）：

栈**不能遍历**

push(data) 压栈

pop() 出栈

top() 访问栈顶

size() 大小



queue（队列）：

只能访问队首和队尾

push(data) 入队（队尾）

pop() 出队（队首）

front()

back()



unordered_map/unordered_set 无序键值对/集合

省去排序过程 节省时间



sort 排序

algorithm 头文件

sort(v.begin(),v.end(),cmp)没有cmp默认升序

一定注意第二个参数是最后一个元素的下一个位置的指针



cmp函数

bool cmp(int x, int y)

{

return x > y; //降序

}



若返回true x放在y前面

false 交换位置

不能加上等于号



cctype

字符操作

isalpha()字母

islower()

isupper()

isalnum()字母或数字

isspace()

tolower()转换为小写

toupper()转换为大写



auto 根据赋值内容决定数据类型 但是不能cin进去

可用迭代器：数组 集合 map



for循环

for (int i:a) 用i来访问a 但是a不会被修改

for (int &i:a) 这样相当于用指针i修改a



to_string(number) 把数字转换为字符变量

#include <string>



stoi() 字符串变整数

stod() 字符串变double



容器共有函数：

begin()：返回指向开头元素的迭代器。

end()：返回指向末尾的下一个元素的迭代器。end() 不指向某个元素，但它是末尾元素的后继。

size()：返回容器内的元素个数。

max_size()：返回容器 理论上 能存储的最大元素个数。依容器类型和所存储变量的类型而变。

empty()：返回容器是否为空。

swap()：交换两个容器。

clear()：清空容器。



next(iterator,n) 将迭代器往后n次

pre





map映射 对于如 字符串对应数字或布尔值 的题来说很好用 比如是否出现过该字符串或数字

map<string,int>xxx;

map<int,bool>xxx;
