# 刷题总结

## 一些C++的高级函数用法

```cpp
// set_intersection() 可以取两个vector的交集(集合!)
// 使用前要将两个vector排序
vector<int>arr1;
vector<int>arr2;
vector<int>inter;
set_intersection(arr1.begin(), arr1.end(), arr2.begin(), arr2.end(), back_inserter(inter));
transform(str.begin(),str.end(),str.begin(),::toupper);

```

## find()用法
```cpp
// 字符串查找
string a;
int pos = a.find('a');
if ( pos != a.npos ) found = true;
else found = false;
// .npos是字符串的最后一位的后一位指针

//vector等 查找
v.find(1) != v.end() //表示找到

//返回第一次出现位置
s.find_fist_of(a);
//返回在第n位之后第一次出现位置
s.find(a,n);

```
## GCD/LCM

$$
a\cdot b = gcd(a,b) \times lcm(a,b)

$$

### 辗转相除法求gcd

```cpp
int gcd(int a,int b)
{
    int temp;
    while ( b!=0 )
    {
        temp = a%b;
        a=b;
        b=temp;
    }
    return a;
}
```

### 通过gcd求lcm

两数相乘除以gcd数

### 最简分数

原理是通分再相加减

## 数字类型常见错误

1.  当没有明确输入数据大小时，默认使用`long long`
2.  使用`long long`的话，就要注意`scanf()`和`printf()`中的格式化输出如`%lld`

## 日期问题

1.  定义好月份对应天数的数组（闰月单独一个）

```cpp
int normalm[13] = {0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
int runm[13] = {0, 31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
```

2.  遍历1~x月，day每次加上m[i]
3.  TODO

## 数组定位赋值（某些情况下较方便）

```cpp
int arr[10] = {
    [0]=2, [2]=3, 6, 7
};
// 没有定位的数据接在前面位置之后
// 其他位置补零
```

## 对char字符的操作/判断

`isalpha()` 判断字母
`islower()` 判断是否小写
`isupper()` 判断是否大写
`isalnum()` 判断是否为字母或数字
`isspace()` 判断是否为空格
`tolower()` 转换为小写
`toupper()` 转换为大写

## sort函数

```cpp
sort(v.begin(),v.end(),cmp);
// 没有自定义cmp函数默认升序

//cmp函数
bool cmp(int a, int b)
{
    return a > b; // 降序
}
```

## vector 删除重复元素

删除重复元素，首先将vector排序。
`sort( vecSrc.begin(), vecSrc.end() );`
然后使用unique方法。
`vecSrc.erase( unique( vecSrc.begin(), vecSrc.end() ), vecSrc.end() );`

## substr() 用法

`substr()`在字符串拼接上很有用

```cpp
string str;
str.substr(index,length);
```

## 去掉字符串开头0

遍历字符串，为’0’的时候使用erase方法
不为’0’的时候break

```cpp
while ( str[0] == '0')
{
    str.erase(0,1);
}
// str.erase(index,length);
```

`erase()` 在处理字符串时也很有用

## 反转字符串

`reverse(str.begin(),str.end())`

## 四舍五入

可以用`round()`
涉及到小数点的四舍五入可以先乘上10的倍数四舍五入后再除回去.

## 将数字转化成数组（还可以改变进制）

```cpp
sprintf(arr,"%",var);
// %d %x etc
```

## 数字转换为string

`to_string(number)`

## string转换为数字

```cpp
stoi(); // 字符串转整数
stod(); // 字符串转double
```

## 截取字符串

`str.substr(index,length)`

# C++输入总结

```cpp
scanf("%s",&str); //忽略空格换行EOF
cin >> str; //忽略空格换行EOF

scanf("%c",&ch); //只读取一个字符 对于输入数据有奇怪的字符时很有用
scanf("%i",&num); //读入八/十/十六进制整数 化为十进制

getline(cin,string); //c++中读取一整行到string类型（非字符串数组）

scanf("%7s"); // 最多读入7个字符到字符串
```

# 易错总结

- 大数组放在main()函数外面以免超限
    
- 注意初始化变量
    
- 注意闰年判断
    
    ```cpp
    y % 4 == 0 && (y % 100 != 0 || y % 400 == 0)
    ```
    

# STL总结

## 共有函数

**`begin()`：返回指向开头元素的迭代器。**
**`end()`：返回指向末尾的下一个元素的迭代器。end() 不指向某个元素，但它是末尾元素的后继。**
**`size()`：返回容器内的元素个数。**
`max_size()`：返回容器 理论上 能存储的最大元素个数。依容器类型和所存储变量的类型而变。
`empty()`：返回容器是否为空。
`swap()`：交换两个容器。
**`clear()`：清空容器。**

## vector

```cpp
vector<int>s;
vector<int,int>ss; // 二维数组
s.size(); // 返回数量
s.push_back();
s.resize(); // 重新分配大小
for ( auto p:s ) ...; // 遍历
```

## map

自动按照key升序排序

```cpp
map<string,bool>m;
m[str] = true; // 添加键值对
cout << m[str]; // 访问值
// 遍历
for ( auto p:m )
{
    cout << p.first << " " << p.second;
}
```

## stack

栈不可以遍历

```cpp
stack<int> s;
s.push(1); // 压栈
s.top(); // 访问栈顶
s.pop(); // 出栈
s.size(); // 获取栈大小
```

## queue

只能访问队首和队尾

```cpp
queue<int> q;
q.push(data); // 从队尾入队
q.pop(); //队首出队
q.front(); // 队首
q.back(); // 队尾
```

## set

元素不重复，自动升序排序
不能预分配空间，也不能初始化

```cpp
set<int> a;
a.insert(data); // 插入
a.find(data); // 查找有无这个元素 返回的是指向该元素的指针
a.erase(data); // 若存在这个值，进行删除
```

`find()`怎么使用：
`a.find() != a.end()`
表示如果遍历完之前找到该元素 返回true 到了end()还没找到返回false

## priority_queue
```c++
priority_queue <int,vector<int>,greater<int> > q;
priority_queue <int,vector<int>,less<int> >q;

top();
empty();
size();

```