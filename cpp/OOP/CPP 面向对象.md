# Program Structure

1. 预处理部分
2. 全局声明
3. 函数


# 函数

## IO控制符

```cpp
#include <iostream>
#include <iomanip>

cout << hex << b;
cout << dec << b;
cout << oct << b;

cout << setiosflags(ios::xxxx);
```

右对齐，保留两位小数，小数点对齐
```cpp
cout << setiosflags(ios::fixed) << setiosflags(ios::right) << setprecision(2);
```

## Inline

内置函数类似于文本替换，不能有复杂的控制语句，只是为了方便调用函数但又不产生堆栈

## 重载

同一个名字，若参数个数/参数类型不同，可以多用

```cpp

int max(int a, int b) {
    return a > b ? a : b;
}
double max(double a, double b) {
    return a > b ? a : b;
}
long max(long a, long b) {
    return a > b ? a : b;
}

int main()
{
    int a = 1, b = 2;
    double c = 1.1, d = 2.2;
    long e = 1000000000000000000, f = 2000000000000000000;
    cout << max(a, b) << endl;
    cout << max(c, d) << endl;
    cout << max(e, f) << endl;
    
    return 0;
}
```

参数个数/参数类型/参数顺序 必须至少一种不同，返回值可以相同也可以不同。

## 模板

通用函数，虚拟类型代替：
```cpp
template <typename T>
T max1(T a, T b) {
    return a > b ? a : b;
}

template <class T>
T max2(T a, T b) {
    return a > b ? a : b;
}

template<typename T1, typename T2>
double max2(T1 a, T2 b) {
    return a > b ? a : b;
}
```

只适用于参数个数相同且函数体相同的情况

## 默认参数

```cpp
double area(double r, double pi=3.14159)
```

从左至右：指定默认值的参数必须放在最右端

# String

TBD

# const

## const变量

```cpp
const double PI = 3.14159;
```

## const 指针

### 指向常量

`const T *name`

```cpp
int a = 1, b = 2;
const int *pb = &b;
*pb = 3; // Error: Read-only variable is not assignable
*pb = &a; // No Problem
```

const指针禁止通过解引用修改其指向的内容，但是可以修改指针本身

### 常指针

`T *const name`

```cpp
int a = 1, b = 2;
int *const pa = &a;

*pa = 3; // No Problem
*pa = &b; // Error: Cannot assign to variable 'pa' with const-qualified type 'int *const'
```

常指针禁止修改指针，但是可以修改间接引用

### 指向常量的常指针

`const T *const name`

```cpp
int a = 1, b = 2;
const int *const pa = &a;

*pa = 3; // Error: Read-only variable is not assignable
*pa = &b; // Error: Cannot assign to variable 'pa' with const-qualified type 'int *const'
```

上面两种操作都不行

## 引用

reference
是C语言的一个重要扩充

```cpp
int a, b;
int &ra = a;
int &ra = b; // Error 不能修改引用
```

b不是指针，而是对a的引用（别名），都是一个变量

不能修改引用，不能创建引用数组

一般用于函数参数：
![[Pasted image 20221030190614.png]]

不需要指针，可以通过引用修改变量值：

```cpp
void swap(int &a, int &b) {
    int t = a;
    a = b;
    b = t;
}

int main()
{
    int a = 1, b = 2;
    swap(a, b);
    cout << a << " " << b << endl;
    // 2 1
    return 0;
}
```

返回值的引用：

```cpp
int a = 5;
int &acc(int x) {
    a += x;
    return a;
}
```

返回的变量必须是全局变量或者静态局部变量

![[Pasted image 20221030191539.png]]


# 类


example：
```cpp

class Worker {
    private:
        string id;
        string name;
        string address;
        string phone;
    public:
        Worker() {
            id = "";
            name = "";
            address = "";
            phone = "";
        }
        Worker(string id, string name, string address, string phone) {
            this->id = id;
            this->name = name;
            this->address = address;
            this->phone = phone;
        }
        void set_data(string id, string name, string address, string phone) {
            this->id = id;
            this->name = name;
            this->address = address;
            this->phone = phone;
        }
        void print_info() {
            cout << "ID: " << id << endl;
            cout << "Name: " << name << endl;
            cout << "Address: " << address << endl;
            cout << "Phone: " << phone << endl;
        }
};
```

类外定义函数，应指明函数作用域（如 `bool Date::is_valid()`）

面向对象特点：**抽象、封装、继承、多态性**
抽象：表示同一类事物的本质
封装：对外隐蔽，提供接口
继承：重用
多态性：不同类对同一种信息作出不同的响应

## 声明/定义

```cpp
class ClassName {
	private: // 只能被本类成员调用
	public: // 可以被本类成员和类的作用域内其他函数调用
	protected: //不能被类外成员调用，可以被派生类成员调用
}
```

![[Pasted image 20221030201359.png]]

![[Pasted image 20221030201722.png]]

## 存储方式

不同对象数据位置不同，函数代码是公用的（位于对象空间之外）

## 访问成员

1. 对象名.成员
2. 指向对象的指针->成员
3. 对象的引用.成员

## 类的封装

数据私有，接口公有：private implementation

类声明与函数定义分离：
类的声明放在头文件中，实现放在单独的cpp文件中：
![[Pasted image 20221030202645.png]]

## 构造函数

`constructor`

```cpp
class Date {
    private:
        int year, month, day;
    public:
	    Date() {
            year = 1997;
            month = 1;
            day = 1;
        } // default constructor
        
        Date(int y, int m, int d) {
            year = y;
            month = m;
            day = d;
        } // constructor with parameters
        
        Date(int y, int m, int d): year(y), month(m), day(d) {} // constructor with init list
        
        Date(int y=1997, int m=1, int d=1) {
            year = y;
            month = m;
            day = d;
        } // constructor with default parameters
}
```

构造函数可以重载：无参数、有参数、参数类型不一
构造函数可以有默认参数

## 析构函数

`deconstructor`

```cpp
class Date {
    private:
        int year, month, day;
    public:
	    Date() {
            year = 1997;
            month = 1;
            day = 1;
            cout << "constructed Date." << endl;
        } // default constructor
        ~Date() {
            cout << "deleted Date." << endl;
        }
}
```

生命期结束自动执行析构函数：
![[Pasted image 20221030205543.png]]

作用：撤销对象占用的内存之前进行清理工作

![[Pasted image 20221030205807.png]]
![[Pasted image 20221030205843.png]]

## 对象数组

```cpp
Date dates[50];
// constructor with one parameter:
Date dates[3] = {1, 2, 3};
// constructor with multiple parameter:
Date dates[3] = {
	Date(2022, 10, 30),
	Date(2022, 10, 31),
	Date(2022, 11, 1),
}
```

## 对象指针

### 指向对象的指针

```cpp
Date *pdate;
Date date1;
pdate = &date1;

*pdate // date1
(*pdate).year // date1.year
pdate->year // date1.year
```

### 指向对象成员的指针

#### 数据成员

```cpp
int *pd1; 
pd1=&d1.hour;
```

#### 成员函数
指向函数的指针变量：
`typeName (*pointerName) (parameters)`
`int (*p) (int, int)`

指向对象成员函数的指针：
`typeName (className::*pointerName) (parameters)`

### this 指针

```cpp
int Box::volume() {
	return (this->h * this->w * this->l);
}
```

## 共用数据保护

### 常对象

```cpp
Time const t1(9,44,10);
const Time t2(1,1,1);
```
生命周期中任何成员都不能修改
不能调用该对象**非const型的成员函数**
```cpp
void get_time() const; // 常成员函数
t1.get_time(); // valid
```

```cpp
mutable int count; // 可以用声明为const的成员函数修改值
```

### 常对象成员

```cpp
const int hour;
Time::Time(int h) {
	hour = h; // INVALID
}
Time::Time(int h): hour(h) {} // VALID!
```

### 常成员函数

```cpp
void get_time() const; // 常成员函数
t1.get_time(); // valid
```

![[Pasted image 20221031095333.png]]

![[Pasted image 20221031095421.png]]

### 指向对象的常指针

```cpp
Time *const ptr1;
ptr1 = &t1; // yes
ptr1 = &t2; // nope
```

blahblah

### 对象的常引用

若不希望在函数中修改对象成员的值，可以把引用变量声明为const：`void func(const Time &t);`

### const 小结
![[Pasted image 20221031100306.png]]


## 对象赋值/复制

### 赋值

`t1 = t2` 只赋值成员的值
```cpp
    Box box1(15, 30, 25), box2;
    box2 = box1; //将box1的值赋给box2
```

### 复制

自定义复制构造函数：
```cpp
Box::Box(const Box &box) {
	height = box.height;
	...
}

Box box2(box1); // 调用复制构造函数
```

![[Pasted image 20221031102033.png]]

## 静态成员

### 静态数据成员

同类的多个对象之间实现数据共享：静态数据成员

`static int a`

```cpp
class Box {
	public:
		int volume();
	private:
		//把height定义为静态数据成员
		static int height; 
		int width;
		int length;
};
```

静态数据成员：编译时分配空间，程序结束释放空间

初始化：只能在类体之外初始化
```cpp
int Box::height = 10;

Box(int h, int w, int l):height(h) {}
// 错误
```

引用静态数据成员：
```cpp
cout<<a.height<<endl; //通过对象名a引用静态数据成员
cout<<b.height<<endl; //通过对象名b引用静态数据成员
cout<<Box::height<<endl; //通过类名引用静态数据成员
```


### 静态成员函数

```cpp
static int volume();

Box::volume();
box1.volume();
```

```cpp
cout<<height<<endl; // static成员height 合法
cout<<width<<endl; // 非static成员width 指向不明
```

**只用静态成员函数引用静态数据，而不引用非静态数据成员**


## 友元

friend 可以访问与其有好友关系的类中的私有成员，友元包括**友元函数**和**友元类**

类以外的地方定义了一个函数（非成员函数或者其他类的成员函数），类体中用friend声明，即为该类的友元函数。

### 友元函数

```cpp
class Date {
    private:
        int year, month, day;
    public:
        Date(int y, int m, int d) {
            year = y;
            month = m;
            day = d;
        }
        ~Date() {
            cout << "Date " << year << "-" << month << "-" << day << " is deleted." << endl;
        }
        friend void print(Date d);
};


void print(Date d) {
    cout << d.year << "-" << d.month << "-" << d.day << endl;

int main() {
	Date d1(2002, 10, 31);
	print(d1); // 友元函数
	return 0;
}
```

### 友元成员函数

友元函数可以是其他类的成员函数

```cpp
class Date() {
	...
	void print_time(Time &);
}

class Time() {
	public:
		Date(int, int, int);
		friend void Date::print_time(Time &);
}
```

注意对象需要先声明才能使用

一个函数可以被多个类声明为友元函数，可以引用多个类中的私有数据。

## 类模板

```cpp
template <class T>
class className {
	private:
	public:
}

template <class T>
typeName className<typeName> objectName();

// 调用需要指明泛型T具体的类型
Compare<int> c1(1, 3);
```

## 运算符重载

```cpp
class Complex {
    private:
        double real, imag;
    public:
        Complex() { real = 0 ; imag = 0; }
        Complex(double r, double i): real(r), imag(i) {}
        Complex operator+ (Complex &c);
        Complex operator- (Complex &c);
        Complex operator* (Complex &c);
};

void Complex::display() {
    cout << real << setiosflags(ios::showpos) << imag << "i" << endl;
}

Complex Complex::operator+(Complex &c) {
    Complex temp;
    temp.real = real + c.real;
    temp.imag = imag + c.imag;
    return temp;
}

Complex Complex::operator-(Complex &c) {
    Complex temp;
    temp.real = real - c.real;
    temp.imag = imag - c.imag;
    return temp;
}

Complex Complex::operator*(Complex &c) {
    Complex temp;
    temp.real = real * c.real - imag * c.imag;
    temp.imag = real * c.imag + imag * c.real;
    return temp;
}

```

- 只能对已有的运算符重载
- 绝大多数运算符可以重载，5个不能：
	- `.` 成员访问符
	- `*` 成员指针访问符
	- `::` 域运算符
	- `sizeof` 长度运算符
	- `?:` 条件运算符
- 操作数个数/优先级/结合性不能改
- 不能有默认参数

### 重载运算符 友元函数

```cpp
friend Complex operator+ (Complex &c1, Complex &c2);
...
Complex operator+ (Complex &c1, Complex &c2) {
	...
}
```

### 重载双目运算符

`== > < >= <= += -= *= /= %=`

```cpp
// += -= *= /= %= 返回this指针
Complex Complex::operator+=(Complex &c) {
    real += c.real;
    imag += c.imag;
    return *this;
}

// > < >= <= == 返回布尔值
bool Complex::operator==(Complex &c) {
    return real == c.real && imag == c.imag;
}
```

双目运算符可以有两个参数，第一个是左值；
也可以只有一个参数，this自动传入，参数为右值；

### 重载单目运算符

`- ! ~ ++ --`

```cpp
Time operator++(); // 前置++
Time operator++(int); // 后置++

Time Time::operator++() {
    if ( ++second >= 60 ) {
        second -= 60;
        ++minute;
    }
    return *this; // 返回second已经自加过的对象本身
}

Time Time::operator++(int) {
    Time temp(*this);
    second++;
    if ( second >= 60 ) {
        second -= 60;
        ++minute;
    }
    return temp; // 返回还未自加过的对象的克隆，但是原本的对象已经自加了，即先用后加
}
```

### 重载流运算符

```cpp
friend istream &operator>> (istream &, Complex &);
friend ostream &operator<< (ostream &, Complex &);

istream &operator>> (istream &input, Complex &c) {
    input >> c.real >> c.imag;
    return input;
}

ostream &operator<< (ostream &output, Complex &c) {
    output << c.real;
    if ( c.imag > 0 ) output << "+" << c.imag << "i";
    else if ( c.imag < 0 ) output << "-" << -c.imag << "i";
    output << endl;
    return output;
}

```

注意最后要return对应的流，保留输出流状态

## 数据类型转换

### 转换构造函数

将其他类型的数据转换为某类对象

`className(targetType a) {...}`

```cpp
Complex(double r) { real=r; imag=0; }
// 将double类型参数转换为Complex类型对象

Complex c1 = Complex(2.33);
```

### 类型转换函数

将某类对象转换为其他类型的数据

`operator typeName() {...}`

```cpp
class Complex() {
	...
	operator double() { return real; }
}
```

只能作为成员函数

# 派生类

基类 -> 派生类（父类 -> 子类）

继承：从父类获得已有特性
派生：从父类产生新的子类

单继承：一个派生类只从一个基类派生
多重继承：可以从多个基类派生

![[Pasted image 20221101140503.png]]

### 派生类的产生

`class Child: <inheritanceType> Father { newMembers }`

`inheritanceType` : `public/private/protected`

![[Pasted image 20221101141818.png]]

构造派生类：
1. 从基类接收成员：把基类全部的成员（不包括构造函数和析构函数）接收过来；
2. 调整从基类接收的成员：
	- 指定继承方式，改变基类成员在派生类中的访问属性
	- 派生类中声明一个与基类成员同名的成员，覆盖基类的同名成员
3. 在声明派生类时增加成员

派生类需要定义构造函数和析构函数
基类可以定义最基本的功能，具体功能分配到派生类实现（抽象基类）

## 派生类成员的访问属性

1. 公用继承：基类public/protected成员保持原有访问属性，**基类的私有成员仍为基类私有**；
2. 私有继承：基类的public/protected成员变为派生类的私有成员，**基类的私有成员仍为基类私有**；
3. 受保护继承：类的public/protected成员变为派生类的protected成员，**基类的私有成员仍为基类私有**；

保护成员：不能被外界访问，但可以被派生类成员访问

![[Pasted image 20221101143536.png]]

![[Pasted image 20221101143929.png]]

实际中常用**公用继承**

## 派生类的构造函数

基类的数据成员也要同时被初始化： 在执行派生类的构造函数是，**调用基类的构造函数**

`childConstructor(...): FatherConstructor(...) {...}`

```cpp
class Animal {
    public:
        Animal(string species, int age, Gender gender ): species(species), age(age), gender(gender) {}
        ~Animal() {}
    private:
    protected:
        string species;
        int age;
        Gender gender;
};

class Dog:protected Animal {
    private:
        string name;
    public:
        // Dog(string name, int age, Gender gender): Animal("Dog", age, gender) {
        //   this->name = name;
        // }
        Dog(string name, int age, Gender gender): Animal("Dog", age, gender), name(name) {}
        ~Dog() {}
        void bark();
        
};
```

派生类构造函数体只需要初始化新增加的成员就行了，基类的成员通过基类构造函数初始化：`Dog(string name, int age, Gender gender): Animal("Dog", age, gender) {this->name = name;}`

shorter: `Dog(string name, int age, Gender gender): Animal("Dog", age, gender), name(name) {}`

![[Pasted image 20221101150121.png]]

## 多重继承

```cpp
class D: public A, private B, protected C {
	newMembersOfD...
}
```

## 虚基类

在继承间接共同基类时只保留一份成员
```cpp
class A//声明基类A
{...};
class B :virtual public A //声明类B是类A的公用派生类，A是B的虚基类
{...};
class C :virtual public A //声明类C是类A的公用派生类，A是C的虚基类
{...};
```

经过这样的声明后，当基类通过多条派生路径被一个派生类继承时，该派生类只继承该基类一次。
![[Pasted image 20221101152418.png]]

虚基类中若有带参数的构造函数且没有默认参数，在所有派生类中都需要通过构造函数初始化表对虚基类进行初始化。

## 组合

composition: 一个类中将另一个类的对象作为数据成员

```cpp
class Professor:public Teacher {
	public:
		...
	private:
		BirthDate birthday; 
		//BirthDate类的对象作为数据成员
};
```

### 继承意义

对类库中类的声明一般放在头文件中，类的实现(函数的定义部分)是单独编译的，以目标代码形式存放在系统某一目录下。

由于基类是单独编译的，在程序编译时只需对派生类新增的
功能进行编译。

...

# 多态性

面向对象系统的多态性是指不同的对象收到相同的的消息时会产生不同的行为（即方法）。C++中，多态性体现在用一个名字定义不同的函数。

多态的实现与链接（联编）有关，分为静态链接（编译时）和动态链接（运行时）。

编译时多态性：函数重载/运算符重载
运行时多态性：**虚函数**

## 基类与派生类之间的赋值兼容

```cpp
Base b; //定义基类Base的对象b
Derived d; //定义公有派生类Derived的对象d
b=d;
```

这样赋值的效果是：对象b中数据成员将具有对象d中对应数据成员的值。
![[Pasted image 20221101192146.png]]

- 可以用派生类对象给基类对象赋值
- 可以用派生类对象来初始化基类对象的引用
- 派生类对象的地址可以赋给指向基类对象的指针
- 声明为指向基类对象的指针可以指向它的公有派生的对象，但不允许指向它的私有派生的对象
- 允许将一个声明为指向基类的指针指向其公有派生类的对象，但是不能将一个声明为指向派生类对象的指针指向其基类的对象

- 在C++中规定：基类的对象指针可以指向它的公有派生的对象,但是当其指向公有派生类对象时，它**只能访问派生类中从基类继承来的成员**，而**不能访问公有派生类中定义的成员**

## 虚函数

```cpp
class A {
    public:
        void print() { cout << "A print()..." << endl; }
};
class B : public A {
    public:
        void print() { cout << "B print()..." << endl; }
};

int main() {
    A p, *p1;
    p1 = &p; // p1指向p
    p1->print();
    B op2;
    op2.print();
    p1 = &op2;   // p1指向op2
    p1->print(); //输出? ==> A print()...
    return 0;
}
```

静态链接的结果：指针p1所指向的对象所执行的print操作只能绑定从类A中继承到的print函数上

```cpp
void print()
virtual void print() // 虚函数
```

C++动态链接是在虚函数支持下实现的

运用实例：
```cpp
class Point {
    public:
        Point(int i, int j): x(i), y(j) {}
        
        int Area() { return 0; } 
        
    private:
        int x, y;
};

class Rectangle: public Point {
    public:
        Rectangle(int i, int j, int k, int l): Point(i, j), w(k), h(l) {}
        
        int Area() { return w * h; }
        
    private:
        int w, h;
};

void show_area(Point &p) {
    cout << p.Area();
}

int main() {
    Rectangle r1(0, 0, 10, 20);
    show_area(r1);
    // 0
    return 0;
}
```

show_area()函数中，Area()操作被绑定到Point::Area()上，这是静态链接的结果

将类中的Area()函数定义为虚函数即可让p被动态链接：
```cpp
virtual int Area() { return 0; } 
virtual int Area() { return w * h; }

show_area(r1);
// 300
```

1. 派生类必须从基类共有派生
2. 类外定义不需要加`virtual`
3. 虚函数若未被重新定义，直接被继承
4. **只有通过基类指针或者引用访问虚函数时**才有多态性，对象名加点运算符没有利用到虚函数的特性
5. 虚函数必须是成员函数

动态联编的实现需要如下3个条件：
（1）类之间为基类与派生类的关系；
（2）要有虚函数；
（3）指针或者对象引用来操作函数。

### 虚析构函数

```cpp
virtual ~className() {...}
```

如果将基类的析构函数定义为虚函数,由该基类所派生的所有派生类的析构函数也都自动成为虚函数。

声明虚析构函数的目的在于使用delete运算符删除一个对象时，能保证析构函数被正确执行。

```cpp
class A {
    public:
        virtual ~A() {
            cout << "~A()..." << endl;
        }
};

class B: public A {
    char *buf;
    public:
        B(int i) {
            buf = new char[i];
        }
        virtual ~B() {
            delete [] buf;
            cout << "~B()..." << endl;
        }
};

int main()
{
    A *a = new B(10);
    delete a;
    // ~B()...
    // ~A()...
    return 0;
}
```

析构函数未说明virtual时，只删除A类对象`~A()...`

### 纯虚函数

```cpp
class Figure {
    protected:
        double x, y;
    public:
        Figure(double a, double b): x(a), y(b) {}
        virtual void area() = 0 ;
};

class Triangle: public Figure {
    public:
        Triangle(double a, double b): Figure(a, b) {}
        void area() {
            cout << "Triangle height: " << x << " bottom: " << y << endl;
            cout << "Triangle area: " << (x*y)*0.5 << endl;
        }
};

```

基类本身不需要这个虚函数，而是为派生类提供一个公共接口，以便派生类重新定义虚函数

`virtual returnType func(...) = 0;`

### 抽象类

至少有一个纯虚函数的类就叫抽象类
抽象类的作用是作为一个类族的共同基类，相关的派生类是从这个基类派生出来的。

- 抽象类不能建立对象
- 不允许从具体类派生出抽象类
- 抽象类不能作为类型
- 可以声明指向抽象类的指针或引用


# 输入输出流

![[Pasted image 20221103095049.png]]
![[Pasted image 20221103095110.png]]

cin/cout
cerr: 标准错误输出，无缓冲
clog: 标准错误输出，有缓冲

## 标准输出流

`cout` 开辟缓冲区用于存放流数据，遇到endl时立即输出流中所有数据然后插入一个`\n` ，并刷新流（清空缓冲区）

### 格式输出

头文件 `<iomanip>` 定义了一系列控制符
`setiosflags()`
`fixed`
`scientific`
`hex`
`showpos`

`setprecision(n)`

## 标准输入流

`cin`
`>>` 提取数据时跳过空格、制表符、换行符
输入回车后，数据才被送入键盘缓冲区，形成输入流（与终端模式有关）

### 输入函数

```cpp
cin.get()
cin.get(ch)
cin.get(array, n, delimiter)

cin.getline(array, n, delimiter)
```

### istream

```cpp
cin.eof() // 若到达文件末尾（eof），返回真
cin.peek() // 观测输入流的下一个字符
cin.putback(ch) // 将字符返回到输入流当前指针位置
cin.ignore(n, delimiter) // 跳过n个字符或遇到指定字符提前结束跳过（即跳过包括终止符在内的若干字符）
```

## 文件流

1. 建立文件流对象
2. 打开或建立文件
3. 进行读写操作
4. 关闭文件


### 打开磁盘文件

```cpp
fstream file;
file.open(path, mode)
```

文件流模式：

`ios::in` ： 打开一个输入文件
`ios::out` ：打开一个输出文件
`ios::app`：在文件末尾添加数据
`ios::nocreate`：仅打开存在的文件
`ios::in | ios::out`：可读可写
`ios::binary`：二进制模式

### 关闭磁盘文件

```cpp
file.close()
```

### ASCII 文件操作

`<<` `>>` 输入输出标准类型的数据

### 文件指针函数

```cpp
// 指针标记符 / 参照位置
ios::beg
ios::cur
ios::end
```

![[Pasted image 20221103165112.png]]

seekg 输入文件的指针
seekp 输出文件的指针


# 异常处理

C++ 通过 throw 和 catch 来处理异常

```cpp
try {
	...
	throw e;
} catch(exceptionType e) {
	...
}
```

```cpp
enum FileIOError {
    OpenFileError,
    FilePermissionError,
};

fstream file;
try {
    file.open("afasdf", ios::in | ios::out);
    if ( !file ) throw FileIOError::OpenFileError;
} catch ( FileIOError e ) {
    switch (e) {
        case OpenFileError:
		    cerr << "open file error!" << endl;
            break;
        case FilePermissionError:
            cerr << "file permission error!" << endl;
            break;
        default:
            cerr << "unknown error!" << endl;
            break;
    }
} 
```

# 命名空间

```cpp
extern int a;
// extern声明两个文件的同名变量为同一个变量
```

```cpp
namespace ns1 {
	int a;
	double b;
}

ns1::a = 1;
ns1::b = 1.0/3.0;
```

**空间限定** 避免命名冲突

## 简化

```cpp
namespace FileErrorHandler
namespace feh = FileErrorHandler

using ns1::Student
// 有效范围就是using语句的作用域

using namespace std
```
