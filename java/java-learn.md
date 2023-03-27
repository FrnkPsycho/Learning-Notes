# Java 学习笔记

## 基础

### 变量

### var 关键字

省略变量类型

```java
StringBuilder sb = new StringBuilder();

var sb = new StringBuilder();
```

### 数组

```java
int[] arr = new int[] {1,2,3};
int[] arr = new int[3];
int[] arr = {1,2,3};
```

数组大小不可变，与字符串一样引用类型，赋新值会改变指向，原本的数据仍然存在但是不可达。

```java
String[] names = { "Alice", "Bob", "Claire" };  
String alice = names[0];  
names[0] = "David";  
System.out.println(alice);  // Alice
System.out.println(names[0]); // David
```

### 格式化输出

```java
int bignumber = 17892038;  
double bigfloat = 12301083.849234;  
System.out.printf("%d\n", bignumber);  
System.out.printf("%.2f\n", bigfloat);
```

`%d %x %f %e %s`

### 输入

需要 `java.util.Scanner` 库

```java
import java.util.Scanner
...
Scanner scanner = new Scanner(System.in);  
String name = scanner.nextLine(); // 读入一行  
int id = scanner.nextInt();  // 读入整数
System.out.println(name + " " + id);
```

### 判断

#### if

浮点数最好不要直接比较，使用差值比较：

```java
if ( a == 0.1 ) {}

if ( Math.abs(a - 0.1) < 0.000001 ) {}
```

字符串相等使用 equals() 方法

```java
String s1 = "hello";  
String s2 = "HELLO".toLowerCase();  
System.out.println(s1);  
System.out.println(s2);  
if (s1.equals(s2)) {  
    System.out.println("s1 == s2");  
} else {  
    System.out.println("s1 != s2");  
}
```

#### switch

##### 模式匹配

```java
switch (arg) {
	case value1 -> expr1;
	case value2 -> {
		expressions....
	};
}
```

yield

复杂情况下返回值

```java
int option = switch (fruit) {  
    case "apple" -> 1;  
    case "banana", "orange" -> 2;  
    default -> {  
        int code = fruit.hashCode();  
        yield code;  
    }
};
```

### 循环

while dowhile for

#### for each

遍历可迭代的数据类型，如数组、List、Map

```java
int[] array = {1, 3, 5, 7, 9};  
for ( int n : array ) System.out.println(n + " ");
```

`Arrays.toString()`

```java
int[] ns = { 1, 1, 2, 3, 5, 8 };
System.out.println(Arrays.toString(ns));
```

### 排序

`Array.sort()`

```java
var scanner = new Scanner(System.in);  
int[] arr = new int[5];  
for ( int i=0 ; i<5; i++ ) {  
    arr[i] = scanner.nextInt();  
}  
Arrays.sort(arr);  
System.out.println(Arrays.toString(arr));
```

整数数组排序后内存中位置变化；
字符串数组排序后只是引用发生了变化

### 二维数组

`int[][] biarray = {{...},{...},{...}};`

#### 遍历

```java
for ( int[] arr : biarray ) {
	for ( int n : arr ) {
		...
	}
}

Arrays.deepToString(biarray);
```

### 命令行参数

```java
public class Main {
    public static void main(String[] args) {
        for (String arg : args) {
            if ("-version".equals(arg)) {
                System.out.println("v 1.0");
                break;
            }
        }
    }
}
```


## 面向对象

### 定义

```java
class Animal {
	private String species;
	private int age;
	private String gender;
}
```

定义方法

```java
public void setAge(int age) {
	this.age = age;
}
```

设置属性进行检查

```java
public void setName(String name) {
    if (name == null || name.isBlank()) {
        throw new IllegalArgumentException("invalid name");
    }
    this.name = name.strip(); // 去掉首尾空格
}
```

### 可变参数

`类型... ` 相当于传入一个数组

```java
class Group {
	private String[] names;
	public void setNames(String... names) {
		this.names = names
	}
}
```

### 引用类型参数

```java
class Person {
    private String[] name;
    public String getName() {
        return this.name[0] + " " + this.name[1];
    }
    public void setName(String[] name) {
        this.name = name;
    }
}


Person p = new Person();
String[] fullname = new String[] { "Homer","Simpson" };
p.setName(fullname); // 传入fullname数组
System.out.println(p.getName()); // "Homer Simpson"
fullname[0] = "Bart"; // fullname数组的第一个元素修改为"Bart"
System.out.println(p.getName()); // "Homer Simpson"还是"Bart Simpson"?

// Bart Simpson
```

引用类型传入仍然指向同一个地址

`fullname[0] = "Bart" ` 其实是将指针指向新的内存，内存数据为 `"Bart"`， p中的`name[0]`跟着一起变化

引用类型参数的传递，调用方的变量，和接收方的参数变量，指向的是同一个对象。双方任意一方对这个对象的修改，都会影响对方（因为指向同一个对象嘛）。

### 构造方法

```java
class Person {
	public Person(...) {
		this(name, 18) // 调用其他构造方法
	}
}
```

没有在构造方法中初始化字段时，引用类型的字段默认是`null`，数值类型的字段用默认值，`int`类型默认值是`0`，布尔类型默认值是`false`

### 方法重载

参数不同的方法

例如： `String.indexOf()`

```java
String s = "Test string";
int n1 = s.indexOf('t');
int n2 = s.indexOf("st");
int n3 = s.indexOf("st", 4);
```

### 继承

```java
class Student extends Person {
	private int grade;  
	public Student(String name,int age, int grade) {  
	    super(name, age);  // super调用超类的构造方法
	    this.grade = grade;  
	}
}
```

#### protected

子类可以访问超类的 `protected` 成员

#### super

指代父类，`super.fieldName`

如果父类没有默认的构造方法，子类就必须显式调用`super()`并给出参数以便让编译器定位到父类的一个合适的构造方法。

这里还顺带引出了另一个问题：即子类**不会继承**任何父类的构造方法。子类默认的构造方法是编译器自动生成的，不是继承的。

#### permit

只允许permit的子类继承

```java
public sealed abstract class AngledShape permits Rectangle  {  
    public AngledShape() {};  
    public abstract double getArea();  
    public abstract void printShapeInfo();  
}

public final class Rectangle extends AngledShape {
	...
} 
```

#### casting

把一个子类类型安全地变为父类类型的赋值，被称为向上转型（upcasting）。

向上转型实际上是把一个子类型安全地变为更加抽象的父类型：

```java
Student s = new Student();
Person p = s; // upcasting, ok
Object o1 = p; // upcasting, ok
Object o2 = s; // upcasting, ok
```

不能把父类变为子类，因为子类功能比父类多，多的功能无法凭空变出来。

如果把一个父类类型强制转型为子类类型，就是向下转型（downcasting）。不能把父类变为子类，因为子类功能比父类多，多的功能无法凭空变出来。

##### instanceof

```java
Person p = new Person();
System.out.println(p instanceof Person); // true
System.out.println(p instanceof Student); // false

Student s = new Student();
System.out.println(s instanceof Person); // true
System.out.println(s instanceof Student); // true

Student n = null;
System.out.println(n instanceof Student); // false
```

### 多态

#### @Overide

Override和Overload不同的是，如果方法签名不同，就是Overload，Overload方法是一个新方法；如果方法签名相同，并且返回值也相同，就是`Override`。

```java
class Rectangle extends AngledShape {  
    private double width, height;  
    public Rectangle(double w, double h) {  
        super();  
        this.width = w;  
        this.height = h;  
    }    
    @Override  
    public double getArea() {  
        return width * height;  
    }    
    @Override  
    public void printShapeInfo() {  
        System.out.println("Rectangle Width: " + width + " Height: " + height);  
    }}  
  
class Square extends Rectangle {  
    private double length;  
    public Square(double length) {  
        super(length, length);  
        this.length = length;  
    }   
    @Override  
    public double getArea() {  
        return length * length;  
    }    
    @Override  
    public void printShapeInfo() {  
        System.out.println("Square Width: " + length);  
    }}
```

非必需，只是为了让编译器强制检查

#### Polymorphic

多态是指，针对某个类型的方法调用，其真正执行的方法取决于运行时期实际类型的方法。允许添加更多类型的子类实现功能扩展，却不需要修改基于父类的代码。

#### 重写Object方法

-   `toString()`：把instance输出为`String`；
-   `equals()`：判断两个instance是否逻辑相等；
-   `hashCode()`：计算一个instance的哈希值。

```java
    // 比较是否相等:
    @Override
    public boolean equals(Object o) {
        // 当且仅当o为Person类型:
        if (o instanceof Person) {
            Person p = (Person) o;
            // 并且name字段相同时，返回true:
            return this.name.equals(p.name);
        }
        return false;
    }
```

#### 调用super方法

```java
class Person {
    protected String name;
    public String hello() {
        return "Hello, " + name;
    }
}

class Student extends Person {
    @Override
    public String hello() {
        // 调用父类的hello()方法:
        return super.hello() + "!";
    }
}
```

#### final

不允许子类对其的方法进行覆写

```java
final class ClassName {}
```

可以在构造方法中初始化final字段：

```java
class Person {
    public final String name;
    public Person(String name) {
        this.name = name;
    }
}
```

这种方法更为常用，因为可以保证实例一旦创建，其`final`字段就不可修改。

### 抽象

如果父类的方法本身不需要实现任何功能，仅仅是为了定义方法签名，目的是让子类去覆写它，那么，可以把父类的方法声明为抽象方法：

```java
abstract class Person {
    public abstract void run();
}
```

抽象化无法被实例化

### 接口

没有字段（变量）的抽象类可以声明为接口 `interface`

```java
interface Person {
    void run();
    String getName();
}
```

当一个具体的`class`去实现一个`interface`时，需要使用`implements`关键字。

```java
class Student implements Person {
    private String name;

    public Student(String name) {
        this.name = name;
    }

    @Override
    public void run() {
        System.out.println(this.name + " run");
    }

    @Override
    public String getName() {
        return this.name;
    }
}
```

在Java中，一个类只能继承自另一个类，不能从多个类继承。但是，一个类可以实现多个`interface`，例如：

```java
class Student implements Person, Hello { // 实现了两个interface
    ...
}
```

### 继承关系

![[Pasted image 20230303170409.png]]

在使用的时候，实例化的对象永远只能是某个具体的子类，但总是通过接口去引用它，因为接口比抽象类更抽象：

```java
List list = new ArrayList(); // 用List接口引用具体子类的实例
Collection coll = list; // 向上转型为Collection接口
Iterable it = coll; // 向上转型为Iterable接口
```

### default 方法

interface 中的方法

`default`方法和抽象类的普通方法是有所不同的。因为`interface`没有字段，`default`方法无法访问字段，而抽象类的普通方法可以访问实例字段。

```java
interface Attack {  
    default void attackInfo() {  
        System.out.println("Attacked: " + getEntity().toString());  
    }    
    Entity getEntity();  
}
```

### 静态

静态变量所有实例共享

```java
class Person {
    public String name;
    public int age;
    // 定义静态字段number:
    public static int number;
}


class Ladder {  
    public static int position; // global shared variable  
    public Ladder() { position = 0; }  
    public Ladder(int startPosition) { position = startPosition; }  
    public static void climbUp() { position++; }  
    public static void climbDown() { position--; }  
    public static void printPosition() { System.out.println(position); }  
}
}
```

访问static变量使用：`className.staticVar`

```java
int a = Ladder.position;
```

static方法同理

静态方法经常用于工具类。例如：

-   Arrays.sort()
-   Math.random()

静态方法也经常用于辅助方法。注意到Java程序的入口`main()`也是静态方法。

#### 接口的静态字段

```java
public interface Person {
    public static final int MALE = 1;
    public static final int FEMALE = 2;

public interface Person {
    // 编译器会自动加上public statc final:
    int MALE = 1;
    int FEMALE = 2;
}
```

## 包

java中的namespace：package

包没有父子关系 java.util与java.util.zip不同

### package

```java
package test;
```

```
package_sample
└─ src
    ├─ hong
    │  └─ Person.java // hong.Person
    │  ming
    │  └─ Person.java
    └─ mr
       └─ jun
          └─ Arrays.java
```


位于同一个包的类，可以访问包作用域的字段和方法。不用`public`、`protected`、`private`修饰的字段和方法就是包作用域。

### import

引用包

```java
import mr.jun.Arrays
// import mr.jun.*
...
Arrays arr = new Arrays();
```

### 命名

为了避免名字冲突，我们需要确定唯一的包名。推荐的做法是使用倒置的域名来确保唯一性。例如：

-   org.apache
-   org.apache.commons.log
-   com.liaoxuefeng.sample

## 作用域

### public

能访问该类/变量的都可以访问

### private

无法被其他类访问，推荐放在public方法后面

嵌套类拥有访问 private 的权限

```java
public class Main {
    public static void main(String[] args) {
        Inner i = new Inner();
        i.hi();
    }

    // private方法:
    private static void hello() {
        System.out.println("private hello!");
    }

    // 静态内部类:
    static class Inner {
        public void hi() {
            Main.hello();
        }
    }
}

```

### protected

`protected`作用于继承关系。定义为`protected`的字段和方法可以被子类访问，以及子类的子类

### package

包作用域是指一个类允许访问同一个`package`的没有`public`、`private`修饰的`class`，以及没有`public`、`protected`、`private`修饰的字段和方法。

```java 
package abc;
// package权限的类:
class Hello {
    // package权限的方法:
    void hi() {
    }
}

class Main {
    void foo() {
        // 可以访问package权限的类:
        Hello h = new Hello();
        // 可以调用package权限的方法:
        h.hi();
    }
}
```

只要在同一个包，就可以访问`package`权限的`class`、`field`和`method`

## 内部类

nested class

#### Inner Class

定义在类中的类

```java
public class Ladder {  
    public static int position; // global shared variable  
    public Ladder() { position = 0; }  
    public Ladder(int startPosition) { position = startPosition; }  
    public static void climbUp() { position++; }  
    public static void climbDown() { position--; }  
    public static void printPosition() { System.out.println(position); }  
  
    public class InnerLadder {  
        public void printLadderEmoji() {  
            System.out.println("🪜");  
        }    
    }
}
```

如何创建：依附于一个Outer Class实例才能new

```java
var ladder = new Ladder();
var innerLadder = ladder.new InnerLadder();
```

#### Anonymous Class

```java
Runnable r = new Runnable() {
    // 实现必要的抽象方法...
};
```

匿名类和Inner Class一样，可以访问Outer Class的`private`字段和方法。之所以我们要定义匿名类，是因为在这里我们通常不关心类名，比直接定义Inner Class可以少写很多代码。

### Static Inner Class

TODO

## JAR

IDEA：

项目结构-工件-添加-JAR-来自具有依赖项的模块-选择模块和主类

构建-构建工件

## 模块

自带依赖关系的class容器 `.jmod`

依赖关系：`module-info.java`

```java
module hello.world {
	requires java.xml;
}
```


## 核心库

### 字符串 String

```java
equals()
contains()
indexOf()
lastIndexOf()
startsWith()
endsWith()

substring(int n); // 从索引n开始到末尾
substring(int n, int l); // 从索引n到索引l

trim() // 去除首尾空白字符
strip() // 跟trim类似，全角空格也会去除

isEmpty() // 是否为空字符串""
isBlank() // 是否为空白字符串

replace(char a, char b) // 将字符a全部替换成b
replace(String a, String b) // ...
replaceAll(String regex, String a) // 正则

// 分割
String s = "A,B,C,D"
String[] ss = s.split(",");

// 拼接
String[] arr = {"A", "B", "C"};
String s = String.join("***", arr); // "A***B***C"

// 格式化
str.formatted(...)
String.format(String str, ...)

// 类型转换
String.valueOf(...); // Any -> String
Interger.parseInt(String); // String -> int
Boolean.parseBoolean(String); // String -> Boolean

char[] cs = "Hello".toCharArray(); // String -> char[]
String s = new String(cs); // char[] -> String
```

编码转换：

```java
// String -> byte[]
byte[] b1 = "Hello".getBytes("UTF-8");

// byte[] -> String
String s1 = new String(b1, "UTF-8");
```

### StringBuilder

```java
var sb = new StringBuilder(1024);
sb.append("Mr ")
    .append("Bob")
    .append("!")
    .insert(0, "Hello, ");
System.out.println(sb.toString());
```

##### 链式调用

`return this;``

```java
...
        Adder adder = new Adder();
        adder.add(3)
             .add(5)
             .inc()
             .add(10);
...

class Adder {
    private int sum = 0;

    public Adder add(int n) {
        sum += n;
        return this;
    }

    public Adder inc() {
        sum ++;
        return this;
    }

    public int value() {
        return sum;
    }
}

```

#### StringJoiner

```java
StringJoiner(String delim, String prefix, String suffix)


var sj = new StringJoiner(", ", "(", ")");
String[] names = { "A", "B", "C"};
for ( String n : names ) sj.add(n);
System.out.println(sj.toString());
// ==> (A, B, C)
```

#### 包装类型

![[Pasted image 20230306102046.png]]

```java
public class Main {
    public static void main(String[] args) {
        int i = 100;
        // 通过new操作符创建Integer实例(不推荐使用,会有编译警告):
        Integer n1 = new Integer(i);
        // 通过静态方法valueOf(int)创建Integer实例:
        Integer n2 = Integer.valueOf(i);
        // 通过静态方法valueOf(String)创建Integer实例:
        Integer n3 = Integer.valueOf("100");
        System.out.println(n3.intValue());
    }
}
```

##### 自动装箱/拆箱

直接把`int`变为`Integer`的赋值写法，称为自动装箱（Auto Boxing），反过来，把`Integer`变为`int`的赋值写法，称为自动拆箱（Auto Unboxing）。

```java
Integer n = 100; // Auto Boxing: int->Integer
// Integer.valueOf(int)

int x = n; // Auto Unboxing: Integer->int
// Integer.intValue()
```

包装类型不可变，只能用 `equals()` 比较

为了节省内存，`Integer.valueOf()`对于较小的数，始终返回相同的实例

创建Integer时建议使用 `Integer n = Integer.valueOf(100);`

```java
Integer.toString(int, int)
```

```java
// boolean只有两个值true/false，其包装类型只需要引用Boolean提供的静态字段:
Boolean t = Boolean.TRUE;
Boolean f = Boolean.FALSE;
// int可表示的最大/最小值:
int max = Integer.MAX_VALUE; // 2147483647
int min = Integer.MIN_VALUE; // -2147483648
// long类型占用的bit和byte数量:
int sizeOfLong = Long.SIZE; // 64 (bits)
int bytesOfLong = Long.BYTES; // 8 (bytes)
```

```java
// 向上转型为Number:
Number num = new Integer(999);
// 获取byte, int, long, float, double:
byte b = num.byteValue();
int n = num.intValue();
long ln = num.longValue();
float f = num.floatValue();
double d = num.doubleValue();
```

### 枚举类

```java
enum Weekday {
	SUN, MON, ...
}

Weekday.MON
```

enum也是一种class

```java
Weekday.SUN.name() // --> "SUN"
Weekday.SUN.ordinal() // --> 0 根据定义顺序变化
```

可以定义一些构造方法以免ordinal变化

```java
enum Weekday {
    MON(1), TUE(2), WED(3), THU(4), FRI(5), SAT(6), SUN(0);

    public final int dayValue;

    private Weekday(int dayValue) {
        this.dayValue = dayValue;
    }
}
```

适合用在 switch 语句中

### 记录类

仔细观察`Point`的定义：

```java
public record Point(int x, int y) {}
```

把上述定义改写为class，相当于以下代码：

```java
public final class Point extends Record {
    private final int x;
    private final int y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int x() {
        return this.x;
    }

    public int y() {
        return this.y;
    }

    public String toString() {
        return String.format("Point[x=%s, y=%s]", x, y);
    }

    public boolean equals(Object o) {
        ...
    }
    public int hashCode() {
        ...
    }
}
```

除了用`final`修饰class以及每个字段外，编译器还自动为我们创建了构造方法，和字段名同名的方法，以及覆写`toString()`、`equals()`和`hashCode()`方法。

换句话说，使用`record`关键字，可以一行写出一个不变类。

Compact Constructor:

```java
public record Point(int x, int y) {
    public Point {
        if (x < 0 || y < 0) {
            throw new IllegalArgumentException();
        }
    }
}
```

### BigInteger

```java
var bi = new BigInteger("1234345234523452345")

bi.pow()
bi.multiply()
bi.add()
```

### BigDecimal

精度完全准确的浮点数

```java
var bd = new BigDecimal("1.232323232323232323");

bd.scale(); // 返回小数位数
bd.stripTrailingZeros()
bd.setScale(int scale, RoundingMode.HALF_UP);

// 比较
d1.compareTo(d2) // - 0 +
```

### 常用

#### Math

```java
Math.abs();
Math.max();
Math.min();
pow sqrt exp 
log // e为底
log10 // 10为底
sin cos tan asin acos

Math.PI
Math.E

Math.random()
```

#### HexFormat

```java
var hf = new HexFormat.of();
String hexStr = hf.formatHex(byte[]);
byte[] bs = hf.parseHex(String);
```

#### Random

```java
Random r = new Random();
r.nextInt(); // 2071575453,每次都不一样
r.nextInt(10); // 5,生成一个[0,10)之间的int
r.nextLong(); // 8811649292570369305,每次都不一样
r.nextFloat(); // 0.54335...生成一个[0,1)之间的float
r.nextDouble(); // 0.3716...生成一个[0,1)之间的double
```

#### SecureRandom

```java
import java.util.Arrays;
import java.security.SecureRandom;
import java.security.NoSuchAlgorithmException;

public class Main {
    public static void main(String[] args) {
        SecureRandom sr = null;
        try {
            sr = SecureRandom.getInstanceStrong(); // 获取高强度安全随机数生成器
        } catch (NoSuchAlgorithmException e) {
            sr = new SecureRandom(); // 获取普通的安全随机数生成器
        }
        byte[] buffer = new byte[16];
        sr.nextBytes(buffer); // 用安全随机数填充buffer
        System.out.println(Arrays.toString(buffer));
    }
}

```

## 异常

Java使用异常来表示错误，并通过`try ... catch`捕获异常；

Java的异常是`class`，并且从`Throwable`继承；

`Error`是无需捕获的严重错误，`Exception`是应该捕获的可处理的错误；

`RuntimeException`无需强制捕获，非`RuntimeException`（Checked Exception）需强制捕获，或者用`throws`声明；

### 异常捕获

```java
try {
...
} catch ( Exception e ) {
...
}
```

可以在方法签名后跟上可能会抛出的异常：

```java
public class Main {
    public static void main(String[] args) {
        try {
            byte[] bs = toGBK("中文");
            System.out.println(Arrays.toString(bs));
        } catch (UnsupportedEncodingException e) {
            System.out.println(e);
        }
    }

    static byte[] toGBK(String s) throws UnsupportedEncodingException {
        // 用指定编码转换String为byte[]:
        return s.getBytes("GBK");
    }
}
```

或者全部交给main()来抛出 （不建议）：

```java
public class Main {
    public static void main(String[] args) throws Exception {
        byte[] bs = toGBK("中文");
        System.out.println(Arrays.toString(bs));
    }

    static byte[] toGBK(String s) throws UnsupportedEncodingException {
        // 用指定编码转换String为byte[]:
        return s.getBytes("GBK");
    }
}
```

#### 多catch

子类异常在前，不然永远捕捉不到。

#### finally

保证有无错误都会执行

```java
try {}
catch () {}
catch () {}
...
finally {
...
}
```

#### 捕获多种异常

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (IOException | NumberFormatException e) { // IOException或NumberFormatException
        System.out.println("Bad input");
    } catch (Exception e) {
        System.out.println("Unknown error");
    }
}
```

### 异常抛出

`printStackTrace()` 打印异常调用栈帧

完整异常栈可以通过构造异常时传入原本的异常，一层层传上去

```java
public class Main {
    public static void main(String[] args) {
        try {
            process1();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    static void process1() {
        try {
            process2();
        } catch (NullPointerException e) {
            throw new IllegalArgumentException(e);
        }
    }

    static void process2() {
        throw new NullPointerException();
    }
}

```

### NullPointerException

遇到`NullPointerException`，遵循原则是早暴露，早修复，严禁使用`catch`来隐藏这种编码错误

返回空字符串`""`、空数组而不是`null`

### 断言

`assert`

```java
assert x < 0;
assert x < 0 : "x must < 0";
```

对于可恢复的程序错误，不应该使用断言。例如：

```java
void sort(int[] arr) {
    assert arr != null;
}
```

应该抛出异常并在上层捕获：

```java
void sort(int[] arr) {
    if (arr == null) {
        throw new IllegalArgumentException("array cannot be null");
    }
}
```

断言很少被使用，更好的方法是编写**单元测试**。

### 日志

#### JDK logging

```java
import java.util.logging.Level;
import java.util.logging.Logger;

public class Hello {
    public static void main(String[] args) {
        Logger logger = Logger.getGlobal();
        logger.info("start process...");
        logger.warning("memory is running out...");
        logger.fine("ignored.");
        logger.severe("process will be terminated...");
    }
}
```

#### Commons Logging

Commons Logging的特色是，它可以挂接不同的日志系统，并通过配置文件指定挂接的日志系统。默认情况下，Commons Loggin自动搜索并使用Log4j（Log4j是另一个流行的日志系统），如果没有找到Log4j，再使用JDK Logging。

```java
import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;

// 静态方法中引用Log
public class Main {
    static final Log log = LogFactory.getLog(Main.class);

    static void foo() {
        log.info("foo");
    }
}

// 实例方法中引用Log
public class Person {
    protected final Log log = LogFactory.getLog(getClass()); // getClass 子类也可以直接使用该Log

    void foo() {
        log.info("foo");
    }
}

// 在子类中使用父类实例化的log:
public class Student extends Person {
    void bar() {
        log.info("bar");
    }
}
```

很方便的重载方法：`info(String, Throwable)` 

将异常栈human-readable地打印出

```java
try {
    ...
} catch (Exception e) {
    log.error("got exception!", e);
}
```

#### Log4j

![[Pasted image 20230306221513.png]]

Appender: 同一条日志输出到不同的目标
Filter: 根据日志等级过滤
Layout: 按照样式格式化日志信息

Layout由`classpath`下的`log4j2.xml`定义

## 反射

由于JVM为每个加载的`class`创建了对应的`Class`实例，并在实例中保存了该`class`的所有信息，包括类名、包名、父类、实现的接口、所有方法、字段等，因此，如果获取了某个`Class`实例，我们就可以通过这个`Class`实例获取到该实例对应的`class`的所有信息。

通过 `Class` 实例获取 `class` 信息的方法称为**反射**

```java
Class cls = String.class;

String s = "hello";
Class cls = s.getClass();

Class cls = Class.forName("java.lang.String");
```

Class实例是唯一的

获取Class信息参考代码：

```java
public class Main {
    public static void main(String[] args) {
        printClassInfo("".getClass());
        printClassInfo(Runnable.class);
        printClassInfo(java.time.Month.class);
        printClassInfo(String[].class);
        printClassInfo(int.class);
    }

    static void printClassInfo(Class cls) {
        System.out.println("Class name: " + cls.getName());
        System.out.println("Simple name: " + cls.getSimpleName());
        if (cls.getPackage() != null) {
            System.out.println("Package name: " + cls.getPackage().getName());
        }
        System.out.println("is interface: " + cls.isInterface());
        System.out.println("is enum: " + cls.isEnum());
        System.out.println("is array: " + cls.isArray());
        System.out.println("is primitive: " + cls.isPrimitive());
    }
}

```

#### 动态加载

利用JVM动态加载`class`的特性，我们才能在运行期根据条件加载不同的实现类。

#### 访问字段

-   `Field getField(name)`：根据字段名获取某个public的field（包括父类）
-   `Field getDeclaredField(name)`：根据字段名获取当前类的某个field（不包括父类）
-   `Field[] getFields()`：获取所有public的field（包括父类）
-   `Field[] getDeclaredFields()`：获取当前类的所有field（不包括父类）

```java
Class stdClass = Student.class;
        // 获取public字段"score":
System.out.println(stdClass.getField("score"));
        // 获取继承的public字段"name":
System.out.println(stdClass.getField("name"));
        // 获取private字段"grade":
System.out.println(stdClass.getDeclaredField("grade"));

```

```
public int Student.score
public java.lang.String Person.name
private int Student.grade
```

`Field` 对象有各种方法：

-   `getName()`：返回字段名称，例如，`"name"`；
-   `getType()`：返回字段类型，也是一个`Class`实例，例如，`String.class`；
-   `getModifiers()`：返回字段的修饰符，它是一个`int`，不同的bit表示不同的含义。

```java
Field f = String.class.getDeclaredField("value");
f.getName(); // "value"
f.getType(); // class [B 表示byte[]类型
int m = f.getModifiers();
Modifier.isFinal(m); // true
Modifier.isPublic(m); // false
Modifier.isProtected(m); // false
Modifier.isPrivate(m); // true
Modifier.isStatic(m); // false
```

##### 获取字段值

如何获取实例的字段值：可以先拿到字段对应的Field，然后再获取该字段的值：

```java
Object p = new Person("Xiao Ming");
Class c = p.getClass();
Field f = c.getDeclaredField("name");

f.setAccessible(true); // if name is private
Object value = f.get(p);
System.out.println(value); // "Xiao Ming"
```

使用反射，首先代码非常繁琐，其次，它更多地是给工具或者底层框架来使用，目的是在不知道目标实例任何信息的情况下，获取特定字段的值。

##### 设置字段值

```java
Field.set(Object, value)
```

#### 调用方法

`Class`类提供了以下几个方法来获取`Method`：

-   `Method getMethod(name, Class...)`：获取某个`public`的`Method`（包括父类）
-   `Method getDeclaredMethod(name, Class...)`：获取当前类的某个`Method`（不包括父类）
-   `Method[] getMethods()`：获取所有`public`的`Method`（包括父类）
-   `Method[] getDeclaredMethods()`：获取当前类的所有`Method`（不包括父类）

一个`Method`对象包含一个方法的所有信息：

-   `getName()`：返回方法名称，例如：`"getScore"`；
-   `getReturnType()`：返回方法返回值类型，也是一个Class实例，例如：`String.class`；
-   `getParameterTypes()`：返回方法的参数类型，是一个Class数组，例如：`{String.class, int.class}`；
-   `getModifiers()`：返回方法的修饰符，它是一个`int`，不同的bit表示不同的含义。

```java
Method.invoke(Object, params...)

// example:
String s = "Hello!!!";  
var c2 = s.getClass();  
c2.setAccessible(true)
var m = c2.getMethod("substring", int.class, int.class);  
try {  
    var s2 = (String) m.invoke(s, 1, 3);  
    System.out.println(s2);  
} catch (InvocationTargetException e ) {  
    e.printStackTrace();  
}
```

##### 静态方法

```java
Method.invode(null, params...)
```

##### 多态

使用反射调用方法时，仍然遵循多态原则：即总是调用实际类型的覆写方法（如果存在）。

#### 调用构造函数

```java
Class.getConstructor(classParams...)


// 获取构造方法Integer(int):
Constructor cons1 =
Integer.class.getConstructor(int.class);
Integer n1 = (Integer) cons1.newInstance(123);

// 获取构造方法Integer(String)
Constructor cons2 = Integer.class.getConstructor(String.class);
Integer n2 = (Integer) cons2.newInstance("456");
```

通过Class实例获取Constructor的方法如下：

-   `getConstructor(Class...)`：获取某个`public`的`Constructor`；
-   `getDeclaredConstructor(Class...)`：获取某个`Constructor`；
-   `getConstructors()`：获取所有`public`的`Constructor`；
-   `getDeclaredConstructors()`：获取所有`Constructor`。
- 
调用非`public`的`Constructor`时，必须首先通过`setAccessible(true)`设置允许访问。`setAccessible(true)`可能会失败。

#### 获取继承关系

##### 父类Class

```java
Class.getSuperclass();
```

##### 接口

```java
Class.getInterfaces() -> Class[] 
```

##### 继承关系

如果是两个`Class`实例，要判断一个向上转型是否成立，可以调用`isAssignableFrom()`：

```java
// Integer i = ?
Integer.class.isAssignableFrom(Integer.class); // true，因为Integer可以赋值给Integer
// Number n = ?
Number.class.isAssignableFrom(Integer.class); // true，因为Integer可以赋值给Number
// Object o = ?
Object.class.isAssignableFrom(Integer.class); // true，因为Integer可以赋值给Object
// Integer i = ?
Integer.class.isAssignableFrom(Number.class); // false，因为Number不能赋值给Integer
```

#### 动态代理

Java标准库提供了一种动态代理（Dynamic Proxy）的机制：可以在运行期动态创建某个`interface`的实例。

动态代理，是和静态相对应的。静态代码怎么写：

```java
public interface Hello {
    void morning(String name);
}

public class HelloWorld implements Hello {
    public void morning(String name) {
        System.out.println("Good morning, " + name);
    }
}

Hello hello = new HelloWorld();
hello.morning("Bob");
```

动态代码，我们仍然先定义了接口`Hello`，但是我们并不去编写实现类，而是直接通过JDK提供的一个`Proxy.newProxyInstance()`创建了一个`Hello`接口对象。
这种没有实现类但是在运行期动态创建了一个接口对象的方式，我们称为动态代码。JDK提供的动态创建接口对象的方式，就叫动态代理。

```java
InvocationHandler handler = new InvocationHandler() {  
    @Override  
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {  
        System.out.println(method);  
        String methodName = (String) method.getName();  
        switch ( methodName ) {  
            case "morning" -> System.out.println("Good morning, " + args[0]);  
            case "afternoon" -> System.out.println("Good afternoon, " + args[0]);  
            case "evening" -> System.out.println("Good evening, " + args[0]);  
  
        }        return null;  
    }};  
  
Hello hello = (Hello) Proxy.newProxyInstance(  
        Hello.class.getClassLoader(),  
        new Class[] { Hello.class },  
        handler  
);  
  
hello.afternoon("Alice");
```

在运行期动态创建一个`interface`实例的方法如下：

1.  定义一个`InvocationHandler`实例，它负责实现接口的方法调用；
2.  通过`Proxy.newProxyInstance()`创建`interface`实例，它需要3个参数：
    1.  使用的`ClassLoader`，通常就是接口类的`ClassLoader`；
    2.  需要实现的接口数组，至少需要传入一个接口进去；
    3.  用来处理接口方法调用的`InvocationHandler`实例。
3.  将返回的`Object`强制转型为接口。

动态代理实际上是JVM在运行期动态创建class字节码并加载的过程

## 注解

注解是放在Java源码的类、方法、字段、参数前的一种特殊“注释”：

```java
// this is a component:
@Resource("hello")
public class Hello {
    @Inject
    int n;

    @PostConstruct
    public void hello(@Param String name) {
        System.out.println(name);
    }

    @Override
    public String toString() {
        return "Hello";
    }
}
```

注释会被编译器直接忽略，注解则可以被编译器打包进入class文件，因此，注解是一种用作标注的“元数据”。

类似Python的Wrapper

Java的注解可以分为三类：

第一类是由编译器使用的注解，例如：

-   `@Override`：让编译器检查该方法是否正确地实现了覆写；
-   `@SuppressWarnings`：告诉编译器忽略此处代码产生的警告。

这类注解不会被编译进入`.class`文件，它们在编译后就被编译器扔掉了。

第二类是由工具处理`.class`文件使用的注解，比如有些工具会在加载class的时候，对class做动态修改，实现一些特殊的功能。这类注解会被编译进入`.class`文件，但加载结束后并不会存在于内存中。这类注解只被一些底层库使用，一般我们不必自己处理。

第三类是在程序运行期能够读取的注解，它们在加载后一直存在于JVM中，这也是最常用的注解。例如，一个配置了`@PostConstruct`的方法会在调用构造方法后自动被调用（这是Java代码读取该注解实现的功能，JVM并不会识别该注解）。

定义一个注解时，还可以定义配置参数。配置参数可以包括：

-   所有基本类型；
-   String；
-   枚举类型；
-   基本类型、String、Class以及枚举的数组。

```java
@Check(100)
int n;
```

### 定义注解

```java
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

#### 元注解

修饰其他注解的注解

##### @Target

最常用的元注解是`@Target`。使用`@Target`可以定义`Annotation`能够被应用于源码的哪些位置：

-   类或接口：`ElementType.TYPE`；
-   字段：`ElementType.FIELD`；
-   方法：`ElementType.METHOD`；
-   构造方法：`ElementType.CONSTRUCTOR`；
-   方法参数：`ElementType.PARAMETER`。

```java
@Target({
    ElementType.METHOD,
    ElementType.FIELD
})
public @interface Report {
    ...
}
```

##### @Retention

另一个重要的元注解`@Retention`定义了`Annotation`的生命周期：

-   仅编译期：`RetentionPolicy.SOURCE`；
-   仅class文件：`RetentionPolicy.CLASS`；
-   运行期：`RetentionPolicy.RUNTIME`。

因为通常我们自定义的`Annotation`都是`RUNTIME`，所以，务必要加上`@Retention(RetentionPolicy.RUNTIME)`这个元注解

#### 如何定义Annotation

我们总结一下定义`Annotation`的步骤：

第一步，用`@interface`定义注解：

```java
public @interface Report {
}
```

第二步，添加参数、默认值：

```java
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

把最常用的参数定义为`value()`，推荐所有参数都尽量设置默认值。

第三步，用元注解配置注解：

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

其中，必须设置`@Target`和`@Retention`，`@Retention`一般设置为`RUNTIME`，因为我们自定义的注解通常要求在运行期读取。一般情况下，不必写`@Inherited`和`@Repeatable`。

### 处理注解

Java提供的使用反射API读取`Annotation`的方法包括：

判断某个注解是否存在于`Class`、`Field`、`Method`或`Constructor`：

-   `Class.isAnnotationPresent(Class)`
-   `Field.isAnnotationPresent(Class)`
-   `Method.isAnnotationPresent(Class)`
-   `Constructor.isAnnotationPresent(Class)`

使用反射API读取Annotation：

-   `Class.getAnnotation(Class)`
-   `Field.getAnnotation(Class)`
-   `Method.getAnnotation(Class)`
-   `Constructor.getAnnotation(Class)`


## 泛型

Generics

```java
public class ArrayList<T> {
    private T[] array;
    private int size;
    public void add(T e) {...}
    public void remove(int index) {...}
    public T get(int index) {...}
}

// 创建可以存储String的ArrayList:
ArrayList<String> strList = new ArrayList<String>();
// 创建可以存储Float的ArrayList:
ArrayList<Float> floatList = new ArrayList<Float>();
// 创建可以存储Person的ArrayList:
ArrayList<Person> personList = new ArrayList<Person>();
```

### 向上转型

在Java标准库中的`ArrayList<T>`实现了`List<T>`接口，它可以向上转型为`List<T>`：

```java
public class ArrayList<T> implements List<T> {
    ...
}

List<String> list = new ArrayList<String>();
```

### 泛型接口

```java
public interface Comparable<T> {
    /**
     * 返回负数: 当前实例比参数o小
     * 返回0: 当前实例与参数o相等
     * 返回正数: 当前实例比参数o大
     */
    int compareTo(T o);
}
```

类实现接口：

```java
class Person implements Comparable<Person> {  
    private String name;  
    private int age;  
  
    public Person(String name, int age) {  
        this.name = name;  
        this.age = age;  
    }  
...
    @Override  
    public int compareTo(Person o) {  
        return this.name.compareTo(o.name);  
    }  
    @Override  
    public String toString() {  
        return this.name + ": " + this.age;  
    }
}
```

### 编写泛型

```java
public class Pair<T> {
    private T first;
    private T last;
    public Pair(T first, T last) {
        this.first = first;
        this.last = last;
    }
    public T getFirst() {
        return first;
    }
    public T getLast() {
        return last;
    }
}
```

#### 静态方法泛型

```java
    // 静态泛型方法应该使用其他类型区分:
    public static <K> Pair<K> create(K first, K last) {
        return new Pair<K>(first, last);
    }
```

#### 多个泛型

```java
public class Pair<T, K> {
    private T first;
    private K last;
    public Pair(T first, K last) {
        this.first = first;
        this.last = last;
    }
    public T getFirst() { ... }
    public K getLast() { ... }
}

Pair<String, Integer> p = new Pair<>("a", 1);
```

### 擦拭法

Java语言的泛型实现方式是擦拭法（Type Erasure）。

所谓擦拭法是指，虚拟机对泛型其实一无所知，所有的工作都是编译器做的。

Java的泛型是由编译器在编译时实行的，编译器内部永远把所有类型`T`视为`Object`处理，但是，在需要转型的时候，编译器会根据`T`的类型自动为我们实行安全地强制转型。

局限：
1. `<T>`不能是基本类型，必须是Object
2. 带泛型的Class，无法使用getClass()获得，如只存在唯一的 `Pair` 的 `Class`
3. 无法判断带泛型的类型，如上
4. 不能实例化 `T` 类型

#### 小心覆写犯法

泛型方法要防止重复定义方法，例如：`public boolean equals(T obj)`；

#### 泛型继承

一个类可以继承自一个泛型类。例如：父类的类型是`Pair<Integer>`，子类的类型是`IntPair`，可以这么继承：

```java
public class IntPair extends Pair<Integer> {
}
```

### extends

```java
public static int add(Pair<? extends Number> p) {  
    Number x = p.getX();  
    Number y = p.getY();  
    return x.intValue() + y.intValue();  
}
```

给方法传入`Pair<Integer>`类型时，它符合参数`Pair<? extends Number>`类型。

这种使用`<? extends Number>`的泛型定义称之为**上界通配符（Upper Bounds Wildcards）**，即把泛型类型`T`的上界限定在`Number`了。

除了可以传入`Pair<Integer>`类型，我们还可以传入`Pair<Double>`类型，`Pair<BigDecimal>`类型等等，因为`Double`和`BigDecimal`都是`Number`的子类。

#### 限定泛型类型

```java
public class Pair<T extends Number> {...}
```

### super

```java
void set(Pair<? super Integer> p, Integer first, Integer last) {
    p.setFirst(first);
    p.setLast(last);
}
```

`Pair<? super Integer>`表示，方法参数接受所有泛型类型为`Integer`或`Integer`父类的`Pair`类型。

### 对比

作为方法参数，`<? extends T>`类型和`<? super T>`类型的区别在于：

-   `<? extends T>`允许调用读方法`T get()`获取`T`的引用，但不允许调用写方法`set(T)`传入`T`的引用（传入`null`除外）；
    
-   `<? super T>`允许调用写方法`set(T)`传入`T`的引用，但不允许调用读方法`T get()`获取`T`的引用（获取`Object`除外）。

```java
public class Collections {
    // 把src的每个元素复制到dest中:
    public static <T> void copy(List<? super T> dest, List<? extends T> src) {
        for (int i=0; i<src.size(); i++) {
            T t = src.get(i);
            dest.add(t);
        }
    }
}
```

-   `copy()`方法内部不会读取`dest`，因为不能调用`dest.get()`来获取`T`的引用；
    
-   `copy()`方法内部也不会修改`src`，因为不能调用`src.add(T)`。

获取和赋值时能够**安全的向上转型**

### PECS

PECS原则：Producer Extends Consumer Super。

如果需要返回`T`，它是生产者（Producer），要使用`extends`通配符；如果需要写入`T`，它是消费者（Consumer），要使用`super`通配符。


## 集合 Collection

`List` / `Set` / `Map`

迭代器访问

### List

`ArrayList` / `LinkedList`

```java
add(index, element)
remove(index)
get(index)
size()
```

创建ArrayList：

```java
var al = new ArrayList<Integer>();
var al = new ArrayList<Integer>(5);
var al = new ArrayList<Integer>(List.of(1,2,3,4,5));
```

迭代器访问：

```java
List<Integer> list = List.of(1,3,5,7,9);  
// 原始的Iterator访问
for (Iterator<Integer> it = list.iterator(); it.hasNext();) {  
    int s = it.next();  
    System.out.println(s);  
}

// 自动的foreach访问
for (int s : list) {  
    System.out.println(s);  
}
```

List与Array转换：

```java
// List -> Array

List<Integer> list = List.of(12, 34, 56);
Integer[] array = list.toArray(new Integer[0]);
// 通过List接口定义的 T[] toArray(IntFunction<T[]> generator) 方法
Integer[] array2 = list.toArray(Integer[]::new);
```

```java
// Array -> List
List<Integer> list = List.of(array);
List<Integer> list2 = Arrays.asList(array);
```

注意返回的 `List` 不是ArrayList/LinkedList，只读，无法进行增删操作

#### equals

`List.contains()` 方法使用的是`equals`方法而非`==`

`equals()`方法的正确编写方法：

1.  先确定实例“相等”的逻辑，即哪些字段相等，就认为实例相等；
2.  用`instanceof`判断传入的待比较的`Object`是不是当前类型，如果是，继续比较，否则，返回`false`；
3.  对引用类型用`Objects.equals()`比较，对基本类型直接用`==`比较。

```java
public boolean equals(Object o) {  
    if ( o instanceof Person ) {  
        return Objects.equals(this.firstName, ((Person) o).firstName) &&  
                Objects.equals(this.lastName, ((Person) o).lastName) &&  
                this.age == ((Person) o).age;  
    }    return false;  
}
```

### Map

`Map<K, V>`

```java
get(key)
put(key, value)
containsKey(key)
```

创建HashMap：

```java
Map<String, Integer> map = new HashMap<>();
```

key不重复，key相等value不相等会更新

遍历Map：

- 遍历key：keySet()
- 遍历key和value：entrySet()

```java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 123);
map.put("pear", 456);
map.put("banana", 789);
for (String key : map.keySet()) {
	Integer value = map.get(key);
	System.out.println(key + " = " + value);
}
```

```java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 123);
map.put("pear", 456);
map.put("banana", 789);
for (Map.Entry<String, Integer> entry : map.entrySet()) {
	String key = entry.getKey();
	Integer value = entry.getValue();
	System.out.println(key + " = " + value);
}
```

#### equals & hashCode

在`Map`的内部，对`key`做比较是通过`equals()`实现的，这一点和`List`查找元素需要正确覆写`equals()`是一样的，即正确使用`Map`必须保证：**作为`key`的对象必须正确覆写`equals()`方法。**

通过`key`计算索引的方式就是**调用`key`对象的`hashCode()`方法**，它返回一个`int`整数。`HashMap`正是通过这个方法直接定位`key`对应的`value`的索引，继而直接返回`value`。

-   如果`a`和`b`相等，那么`a.equals(b)`一定为`true`，则`a.hashCode()`必须等于`b.hashCode()`；
-   如果`a`和`b`不相等，那么`a.equals(b)`一定为`false`，则`a.hashCode()`和`b.hashCode()`尽量不要相等。

```java
public int hashCode() {
    return Objects.hash(firstName, lastName, age);
}
```

### EnumMap

使用枚举类型作为key，效率高，空间不浪费

```java
Map<DayOfWeek, String> map = new EnumMap<>(DayOfWeek.class);
```

### TreeMap

能够排序key的Map

```java
Map<String, Integer> map = new TreeMap<>();
```

key必须实现 `Comparable` 接口，或者定义一个排序算法

```java
Map<Person, Integer> map = new TreeMap<>(new Comparator<Person>() {
    public int compare(Person p1, Person p2) {
        return p1.name.compareTo(p2.name);
    }
});
```

TreeMap不使用equals和hashCode

compare方法需要返回 `-1 0 1`

```java
Map<Person, Integer> personMap = new TreeMap<>(new Comparator<Person>() {  
    @Override  
    public int compare(Person o1, Person o2) {  
        return Objects.compare(o1.age, o2.age, Integer::compareTo);  
    }});
```


### Properties

1.  创建`Properties`实例；
2.  调用`load()`读取文件；
3.  调用`getProperty()`获取配置。

```java
String propertiesFile = "settings.properties";  
Properties props = new Properties();  
props.load(new FileInputStream(propertiesFile));  
for ( var prop : props.entrySet() ) {  
    System.out.println(prop);  
}
String serverIp = props.getProperty("server_ip");
```

写入Properties

```java
props.setProperty("gamemode", "0");  
props.store(new FileOutputStream(propertiesFile), "Generated by Program");
```

### Set

不重复元素，实现类有：HashSet

```java
Set<String> set = new HashSet<>();
add
remove
contains
```

SortedSet保证有序，实现类有：TreeSet

```java
TreeSet<Integer> tset = new TreeSet<>();
```

跟TreeMap一样，类要不实现了Comparable接口，要不就是创建时传入一个Comparator对象。

```java
TreeSet<Person> pset = new TreeSet<>((o1, o2) -> Objects.compare(o1.age, o2.age, Integer::compareTo));
```

### Queue

```java
add / offer // add
remove / poll // return and remove
element / peek // return don't remove
// 失败时前者返回异常，后者返回null/false
```

实现类有：LinkedList

### PriortyQueue

优先队列

元素必须实现 Comparable 接口，根据排序顺序决定优先级。没有实现可以传入一个Comparator

```java
PriorityQueue<Animal> pq = new PriorityQueue<>(new AnimalComparator());

  
class AnimalComparator implements Comparator<Animal> {  
    public int compare(Animal a1, Animal a2) {  
        return Objects.compare(a1.age, a2.age, Integer::compareTo);  
    }
}
```

### Deque

双向队列， 是Queue接口的子类

```java
addFirst / offerFirst
addLast / offerLast
...

```

使用`Deque`，推荐总是明确调用`offerLast()`/`offerFirst()`或者`pollFirst()`/`pollLast()`方法。

实现类有：ArrayQueue, LinkedList

### Stack

```java
push
pop
```

### Iterator

// TODO

### Collections

提供了一些静态方法

```java
Collections.emptyList() // 创建空List
emptyMap()
emptySet()

singletonList
singletonMap
singletonSet

Collections.sort() // 排序，传入可变List
Collections.shuffle() // 随机打乱
```


## I/O

`InputStream`代表输入**字节流**，`OuputStream`代表输出字节流，这是最基本的两种IO流。

Java提供了`Reader`和`Writer`表示**字符流**，字符流传输的最小数据单位是`char`。

### File

```java
File f = new File(String pathname);
File.separator // 当前平台的路径分隔符
f.getPath()
f.getAbsolutePath() // 绝对路径，但是允许有..
f.getCanonicalPath() // 纯绝对路径，规范路径


f.isFile()
f.isDirectory()
f.canRead()
f.canWrite()
f.canExecute()
f.length() // 文件字节大小

f.createNewFile()
f.delete()

// 临时文件
File f = File.createTempFile("tmp-", ".txt") // 临时文件的前后缀
f.deleteOnExit();

// 遍历文件和目录
File[] fs1 = f.listFiles(); // 列出所有文件和子目录
File[] fs2 = f.listFiles(new FilenameFilter {
	public boolean accept(File dir, String name) }
		return name.endsWith(".txt");
}); // 提供了许多重载方法，包括传入FilenameFilter对象

// 目录创建与删除
f.mkdir()
f.mkdirs()
f.delete()
```

`java.nio.file.Path`

```java
Path p = Paths.get(".", "Projects", "Study");
p.toAbsolutePath()
p.normalize() // 规范路径
p.toFile()

// 遍历Path层级
for ( Path p: Paths.get("..").toAbsolutePath())
```


### InputStream

```java
InputStream input = new FileInputStream(pathname);
input.read(); // 单个字符，返回int
input.close();
```

保证输入流正确关闭：

```java
InputStream input = null;
try {
	input = new FileInputStream("src/readme.txt");
	int n;
	while ((n = input.read()) != -1) { // 利用while同时读取并判断
		System.out.println(n);
	}
} finally {
	if (input != null) { input.close(); }
}

// try(resource)
try ( InputStream input = new FileInputStream("src/readme.txt")) {  
    int n;  
    while ((n = input.read()) != -1) {  
        System.out.println(n);  
    }
} // 自动调用close()
```

#### 缓冲

一次性写入byte[]

```java
input.read(byte[] buffer, int offset, int length)

try ( InputStream input = new FileInputStream("src/readme.txt")) {  
    byte[] buffer = new byte[1024];  
    int n;  
    while ( (n = input.read(buffer)) != -1 ) {  
        System.out.println("read " + n + " bytes.");  
    }}
```

read方法是阻塞的，必须要等待其返回

#### 实现类

FileInputStream

ByteArrayInputStream 在内存中模拟一个InputStream


### OutputStream

```java
OutputStream output = new FileOutputStream(pathname);
output.write(int); // 只写入int最低8位
output.close();
output.flush(); // 将缓冲区内容写入目的地
```

向磁盘、网络写入数据的时候，出于效率的考虑，操作系统并不是输出一个字节就立刻写入到文件或者发送到网络，而是把输出的字节先放到内存的一个缓冲区里（本质上就是一个`byte[]`数组），等到缓冲区写满了，再一次性写入文件或者网络。`flush()`方法，能强制把缓冲区内容输出。

```java
try ( var output = new FileOutputStream("src/readme.txt")) {  
	output.write("Hello!".getBytes(StandardCharsets.UTF_8));  
}
```

#### 实现类

FileOutputStream

ByteArrayOutputStream


### Filter

将Stream分为两种，一种是基础功能Stream：
-   FileInputStream
-   ByteArrayInputStream
-   ServletInputStream
-   ...

另一种是叠加附加功能的FilterStream:
-   BufferedInputStream
-   DigestInputStream
-   CipherInputStream
-   ...

```java
public class Main {
    public static void main(String[] args) throws IOException {
        byte[] data = "hello, world!".getBytes("UTF-8");
        try (CountInputStream input = new CountInputStream(new ByteArrayInputStream(data))) {
            int n;
            while ((n = input.read()) != -1) {
                System.out.println((char)n);
            }
            System.out.println("Total read " + input.getBytesRead() + " bytes");
        }
    }
}

class CountInputStream extends FilterInputStream {
    private int count = 0;

    CountInputStream(InputStream in) {
        super(in);
    }

    public int getBytesRead() {
        return this.count;
    }

    public int read() throws IOException {
        int n = in.read();
        if (n != -1) {
            this.count ++;
        }
        return n;
    }

    public int read(byte[] b, int off, int len) throws IOException {
        int n = in.read(b, off, len);
        if (n != -1) {
            this.count += n;
        }
        return n;
    }
}
```

### Zip

```java
try ( var zip = new ZipInputStream(new FileInputStream("zip.zip"))) {  
    ZipEntry entry = null;  
    while ( (entry = zip.getNextEntry()) != null) {  
        String name = entry.getName();  
        if ( !(entry.isDirectory()) ) {  
            int n;  
            while ( (n = zip.read()) != -1 ) {  
                System.out.print((char) n);  
            }        
        }    
    }
}
```

### 读取配置

```java
InputStream input = getClass().getResourceAsStream("/default.properties")

Properties props = new Properties();
props.load(inputStreamFromClassPath("/default.properties"));
props.load(inputStreamFromFile("./conf.properties"));
```


### 序列化

将对象转换为二进制`byte[]`数组

#### 序列化 ObjectOutputStream

```java
var buffer = new ByteArrayOutputStream();  
try ( var output = new ObjectOutputStream(buffer)) {  
    output.writeInt(1);  
    output.writeUTF("Hellllloooo!");  
    output.writeObject(new int[] {1, 2, 3, 4});  
}  
System.out.println(Arrays.toString(buffer.toByteArray()));
```

#### 反序列化 ObjectInputStream

```java
try (ObjectInputStream input = new ObjectInputStream(...)) {
    int n = input.readInt();
    String s = input.readUTF();
    Double d = (Double) input.readObject();
}
```

除了能读取基本类型和`String`类型外，调用`readObject()`可以直接返回一个`Object`对象。要把它变成一个特定类型，必须强制转型。

`readObject()`可能抛出的异常有：

-   `ClassNotFoundException`：没有找到对应的Class；
-   `InvalidClassException`：Class不匹配。


### Reader

以 `char` 为单位读取字符流

```java
public int read() throws IOException; // 最主要方法
```

#### FileReader

将文件按照 char 读取而非 byte

```java
public void readFile() throws IOException {
    try (Reader reader = new FileReader("src/readme.txt", StandardCharsets.UTF_8)) {
        char[] buffer = new char[1000];
        int n;
        while ((n = reader.read(buffer)) != -1) {
            System.out.println("read " + n + " chars.");
        }
    }
}
```

#### CharArrayReader

把一个`char[]`数组变成一个`Reader`，这和`ByteArrayInputStream`非常类似：

```java
try (Reader reader = new CharArrayReader("Hello".toCharArray())) {
}
```

#### StringReader

`StringReader`可以直接把`String`作为数据源，它和`CharArrayReader`几乎一样：

```java
try (Reader reader = new StringReader("Hello")) {
}
```

#### InputStreamReader

已有InputStream的情况下，将其转换为Reader

```java
// 持有InputStream:
InputStream input = new FileInputStream("src/readme.txt");
// 变换为Reader:
Reader reader = new InputStreamReader(input, "UTF-8");
```

### Writer

与 Reader 一样，按照`char`输出

```java
try ( var writer = new FileWriter("src/readme.txt")) {  
    writer.write('H');  
    writer.write("ello");  
    writer.write("!!!".toCharArray());  
}
```

其他派生类与Reader类似


### Files

提供了很多实用方法

```java
var f = Path.of("src/readme.txt");  
byte[] data = Files.readAllBytes(f);    
String content = Files.readString(f);  
List<String> lines = Files.readAllLines(f);  

Files.write(f, "Helo".getBytes(StandardCharsets.UTF_8));  
Files.writeString(f, "WWWOOORRRLLLDDD!");
```


## 时间

系统时间戳：
`System.currentTimeMillis()`

### Date

```java
Date date = new Date();

System.out.println(date);  
System.out.println(date.toGMTString());  
System.out.println(date.toLocaleString());

var sdf = new SimpleDateFormat("E MMM dd, yyyy");
System.out.println(sdf.format(date));
```

### Calendar

比Date多了简单的日期和时间计算功能

```java
var c = Calendar.getInstance();  // 只有这一种方式获取
int y = c.get(Calendar.YEAR);  
int m = 1 + c.get(Calendar.MONTH);  
int d = c.get(Calendar.DAY_OF_MONTH);  
int w = c.get(Calendar.DAY_OF_WEEK);  // 1为周日
int hh = c.get(Calendar.HOUR_OF_DAY);  
int mm = c.get(Calendar.MINUTE);  
int ss = c.get(Calendar.SECOND);  
int ms = c.get(Calendar.MILLISECOND);  
System.out.println(y + "-" + m + "-" + d + " " + w + " " + hh + ":" + mm + ":" + ss + "." + ms);

c.clear()
c.set(...)

c.getTime() // 将 Calendar 转换成 Date 对象
c.add(field, int)
```

### TimeZone

```java
TimeZone tzDefault = TimeZone.getDefault(); // 当前TimeZone tzGMT9 = TimeZone.getTimeZone("GMT+09:00"); // GMT+9:00时区
TimeZone tzNY = TimeZone.getTimeZone("America/New_York"); // 纽约时区

System.out.println(tzDefault.getID()); // Asia/Shanghai
System.out.println(tzGMT9.getID()); // GMT+09:00
System.out.println(tzNY.getID()); // America/New_York

TimeZone.getAvailableIDs();
```

时区的唯一标识是以字符串表示的ID，我们获取指定`TimeZone`对象也是以这个ID为参数获取，`GMT+09:00`、`Asia/Shanghai`都是有效的时区ID。要列出系统支持的所有ID，请使用`TimeZone.getAvailableIDs()`。

### LocalDateTime

更好更新的时间API `java.time`

```java
LocalDate d = LocalDate.now();  
LocalTime t = LocalTime.now();  
LocalDateTime dt = LocalDateTime.now();



LocalDateTime dt = LocalDateTime.now(); // 当前日期和时间
LocalDate d = dt.toLocalDate(); // 转换到当前日期
LocalTime t = dt.toLocalTime(); // 转换到当前时间



// 指定日期和时间:
LocalDate d2 = LocalDate.of(2019, 11, 30); // 2019-11-30, 注意11=11月
LocalTime t2 = LocalTime.of(15, 16, 17); // 15:16:17
LocalDateTime dt2 = LocalDateTime.of(2019, 11, 30, 15, 16, 17);
LocalDateTime dt3 = LocalDateTime.of(d2, t2);


// 字符串解析出时间
LocalDateTime dt = LocalDateTime.parse("2019-11-19T15:16:17");
LocalDate d = LocalDate.parse("2019-11-19");
LocalTime t = LocalTime.parse("15:16:17");


// 自定义格式化：
DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyMMdd-HHmmss");  
System.out.println(dtf.format(LocalDateTime.now()));
// 用自定义格式解析:
LocalDateTime dt2 = LocalDateTime.parse("2019/11/30 15:16:17", dtf);
System.out.println(dt2);
```

-   日期：yyyy-MM-dd
-   时间：HH:mm:ss
-   带毫秒的时间：HH:mm:ss.SSS
-   日期和时间：yyyy-MM-dd'T'HH:mm:ss
-   带毫秒的日期和时间：yyyy-MM-dd'T'HH:mm:ss.SSS

```java
LocalDateTime dt = ......
LocalDateTime dt2 = dt.plusDays(1).minusHours(3);
```

对日期和时间进行调整则使用`withXxx()`方法，例如：`withHour(15)`会把`10:11:12`变为`15:11:12`：

-   调整年：withYear()
-   调整月：withMonth()
-   调整日：withDayOfMonth()
-   调整时：withHour()
-   调整分：withMinute()
-   调整秒：withSecond()

要判断两个`LocalDateTime`的先后，可以使用`isBefore()`、`isAfter()`方法，对于`LocalDate`和`LocalTime`类似

注意到`LocalDateTime`无法与时间戳进行转换，因为`LocalDateTime`没有时区，无法确定某一时刻。后面我们要介绍的`ZonedDateTime`相当于`LocalDateTime`加时区的组合，它具有时区，可以与`long`表示的时间戳进行转换。

### Duration / Period

`Duration`表示两个时刻之间的时间间隔。另一个类似的`Period`表示两个日期之间的天数：

```java
Duration d = Duration.between(dt1, dt2);
```


### ZonedDateTime

```java
ZonedDateTime zbj = ZonedDateTime.now(); // 默认时区
ZonedDateTime zny = ZonedDateTime.now(ZoneId.of("America/New_York")); // 用指定时区获取当前时间
System.out.println(zbj);
System.out.println(zny);
```

### Instant

```java
Instant now = Instant.now();
now.getEpochSecond()
now.toEpochMilli()
```


## 多线程

### 创建线程

```java
Thread t1 = new Thread( () -> {  
   System.out.println("t1 Hello!");  
});  


Thread t2 = new Thread() {  
    @Override  
    public void run() {  
        System.out.println("t2 Hello!");  
    }
};
```

直接调用 `Thread.run()` 没有用

线程优先级：
`Thread.setPriority(int n)` n为1到10的整数 10为最高

### 线程状态

Java线程的状态有以下几种：

-   New：新创建的线程，尚未执行；
-   Runnable：运行中的线程，正在执行`run()`方法的Java代码；
-   Blocked：运行中的线程，因为某些操作被阻塞而挂起；
-   Waiting：运行中的线程，因为某些操作在等待中；
-   Timed Waiting：运行中的线程，因为执行`sleep()`方法正在计时等待；
-   Terminated：线程已终止，因为`run()`方法执行完毕。

```java
Thread.join() // 主线程等待子线程执行完毕
```

### 中断线程

- 使用interrupt方法中断，此方法结束线程的 `join` 等待并抛出 `InterruptedException`
线程通过循环检查 `isInterrupted()`
```java
Thread.interrupt()

// example:
public class Main {
    public static void main(String[] args) throws InterruptedException {
        Thread t = new MyThread();
        t.start();
        Thread.sleep(1); // 暂停1毫秒
        t.interrupt(); // 中断t线程
        t.join(); // 等待t线程结束
        System.out.println("end");
    }
}

class MyThread extends Thread {
    public void run() {
        int n = 0;
        while (! isInterrupted()) {
            n ++;
            System.out.println(n + " hello!");
        }
    }
}
```

- 设置标志位，如 `running`

```java
public class Main {
    public static void main(String[] args)  throws InterruptedException {
        HelloThread t = new HelloThread();
        t.start();
        Thread.sleep(1);
        t.running = false; // 标志位置为false
    }
}

class HelloThread extends Thread {
    public volatile boolean running = true;
    public void run() {
        int n = 0;
        while (running) {
            n ++;
            System.out.println(n + " hello!");
        }
        System.out.println("end!");
    }
}

```

注意标志位 `running` 是一个线程间共享的变量，需要用 `volatile` 标记

`volatile`关键字的目的是告诉虚拟机：

-   每次访问变量时，总是获取主内存的最新值；
-   每次修改变量后，立刻回写到主内存。

`volatile`关键字解决的是可见性问题：当一个线程修改了某个共享变量的值，其他线程能够立刻看到修改后的值。

### 守护线程

Daemon

守护线程是指为其他线程服务的线程。在JVM中，所有非守护线程都执行完毕后，无论有没有守护线程，虚拟机都会自动退出。

```java
Thread daemon = new Thread();
t.setDaemon(true);
t.start();
```

在守护线程中，编写代码要注意：守护线程不能持有任何需要关闭的资源，例如打开文件等，因为虚拟机退出时，守护线程没有任何机会来关闭文件，这会导致数据丢失。

### 线程同步

同步锁 `synchorized()`

```java

class Counter {
    public static final Object lock = new Object();
    public static int count = 0;
}

class AdderThread extends Thread {  
    public void run() {  
        for ( int i=0; i<=10000; i++ ) {  
            synchronized (Counter.lock) {  
                Counter.count += 1;  
            }        
        }    
    }
}  
  
class SuberThread extends Thread {  
    public void run() {  
        for ( int i=0 ; i<=10000; i++ ) {  
            synchronized (Counter.lock) {  
                Counter.count -= 1;  
            }        
        }    
    }
}
```

如何使用`synchronized`：

1.  找出修改共享变量的线程代码块；
2.  选择一个共享实例作为锁；
3.  使用`synchronized(lockObject) { ... }`。

确定好要线程保护的对象，每个对象一把锁

#### 不需要同步锁

JVM规定了以下原子操作：

- 基本类型的赋值
注意 `long` 和 `double` 的赋值在x64平台上实现为原子操作
- 引用类型的赋值

比如以下两种都不需要：
```java
public void set(int m) {
    synchronized(lock) {
        this.value = m;
    }
}
public void set(String s) {
    this.value = s;
}
```

如果是多行赋值就必须要同步锁！

### 同步方法

将 `synchronized` 逻辑封装起来，对外表现为线程安全的类/方法：

```java
class Counter {
    public static int count = 0;

    public void add(int n) {
        synchronized (this) {
            count += n;
        }
    }

	// 如果要锁住的this的话，可以直接将方法标记为同步
    public synchronized void sub(int n) {
        count -= n;
    }
    
    // 只读一个变量的话不需要同步
    // 若有多行赋值并返回的情况的话需要同步！
    public int get() {
        return count;
    }
}


  
var c1 = new Counter();  
var c2 = new Counter();  
  
new Thread( () -> c1.add(1)).start();  
new Thread( () -> c1.sub(1)).start();  
  
new Thread( () -> c2.add(1)).start();  
new Thread( () -> c2.sub(1)).start();  
  
Thread.sleep(100);  
System.out.println("" + c1.get() + c2.get()); // 00
```

### 死锁

Java的线程锁是可重进的：

JVM允许同一个线程重复获取同一个锁，这种能被同一个线程反复获取的锁，就叫做可重入锁。

由于Java的线程锁是可重入锁，所以，获取锁的时候，不但要判断是否是第一次获取，还要记录这是第几次获取。每获取一次锁，记录+1，每退出`synchronized`块，记录-1，减到0的时候，才会真正释放锁。

如何避免死锁：**线程获取锁的顺序要一致。**

### wait & notify

```java
class TaskQueue {
    Queue<String> queue = new LinkedList<>();

    public synchronized void addTask(String s) {
        this.queue.add(s);
        this.notify(); // 唤醒在this锁等待的进程
    }

    public synchronized String getTask() {
        while (queue.isEmpty()) {
            // release lock
            this.wait();
            // get lock again
        }
        return queue.remove();
    }
}
```

- wait必须在当前获取的锁上使用，如 `this`
- 调用wait之后，线程进入等待状态（不占用CPU），直到线程被其他线程唤醒之后才会返回
- wait方法调用时会释放线程获得的锁，返回后又试图重新获得锁
- `notify` 方法可以唤醒在该锁等待的线程（其中之一）
- `notifyAll` 方法一次唤醒所有在该锁等待的线程

### ReentrantLock

`java.util.concurrent` 并发功能，提供了`ReentrantLock`

```java
class BetterCounter {
    private final Lock lock = new ReentrantLock();
    public static int count = 0;

    public void add(int n) throws InterruptedException {
        if ( lock.tryLock(1, TimeUnit.SECONDS)) {
            try {
                count += n;
            } finally {
                lock.unlock();
            }
        }
        // continues...
    }
}
```

注意需要处理异常

`ReentrantLock` 顾名思义可重进，但是多了尝试获取锁的功能，设置超时时间，以便没有获取到锁时可以做别的操作

### Condition

类似于 `wait` 和 `notify` 的功能

```java
class TaskQueue {
    private final Lock lock = new ReentrantLock();
    private final Condition condition = lock.newCondition();
    private Queue<String> queue = new LinkedList<>();

    public void addTask(String s) {
        lock.lock();
        try {
            queue.add(s);
            condition.signalAll();
        } finally {
            lock.unlock();
        }
    }

    public String getTask() {
        lock.lock();
        try {
            while (queue.isEmpty()) {
                condition.await();
            }
            return queue.remove();
        } finally {
            lock.unlock();
        }
    }
}
```

-   `await()`会释放当前锁，进入等待状态；
    
-   `signal()`会唤醒某个等待线程；
    
-   `signalAll()`会唤醒所有等待线程；
    
-   唤醒线程从`await()`返回后需要重新获得锁。
