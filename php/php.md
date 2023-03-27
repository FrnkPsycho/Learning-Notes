# PHP

## 基础

`.php`文件可以包含html和php代码，但是html不能执行php代码。

函数名/关键词不区分大小写，变量区分

注释：

```
// single line

/* */ inline comment

/*
... multi-line
*/
```

### 变量

`$name = "";`

```php
$txt = "W3Schools.com";
echo "I love " . $txt . "!";
echo "I love $txt!"
```

php区分局部和全局变量

全局变量储存在`$GLOBALS`数组（字典）内，函数内用索引引用可以更改全局变量：

```php
$x = 5;
$y = 10;

function myTest() {
  $GLOBALS['y'] = $GLOBALS['x'] + $GLOBALS['y'];
}

myTest();
echo $y; // outputs 15
```

static变量，函数内可保存的局部变量



### 类型

- String

    `""`

- Integer

- Float (floating point numbers - also called double)

- Boolean

- Array

    `$arr = array(a, b, c)`

    字典like:

    ```php
    $age = array("Peter"=>"35", "Ben"=>"37", "Joe"=>"43");
    $age['Peter'] = "35";
    $age['Ben'] = "37";
    $age['Joe'] = "43";
    
    // 循环
    foreach($age as $x => $x_value) {
      echo "Key=" . $x . ", Value=" . $x_value;
      echo "<br>";
    }
    ```

    排序函数：`sort()`

- Object

    语法类似Java：

    ```php
    class Car {
      public $color;
      public $model;
      public function __construct($color, $model) {
        $this->color = $color;
        $this->model = $model;
      } // 构建器函数
      public function message() {
        return "My car is a " . $this->color . " " . $this->model . "!";
      }
    }
    
    $myCar = new Car("black", "Volvo");
    echo $myCar -> message();
    echo "<br>";
    $myCar = new Car("red", "Toyota");
    echo $myCar -> message();
    ```

    

- NULL

- Resource

`var_dump($var)` 返回变量数据类型和值（`type(value)`）



`strlen()`, `str_word_count()`, `strrev()`

`strpos(string, text)`

`str_replace(string, text)`



`is_int()`

`is_float()`

`is_finite()`

`is_nan()`



`(int)$x` 类型转换



### 常量

`define(name, value, is_case_sensitive)`

value可以为数组

常量是全局的

### 运算符

`+ - * / % **`

`= += -= *= /= %=`

`++` `--`



比较运算符差不多，有些特别的：

`===` 严格相等（类型和值相等）

`!==` 不严格相等

`<=>` Spaceship 等于返回0，小于返回-1，大于返回1



`&& || !` `and or`

`xor`



字符串拼接：

`.` `$text1 . $text2`

`.=` `$text1 .= $text2`



`?:` `??`



### 判断语句

```php
if () {
	// code here...
} elseif {
    // code here...
} else {
    // code here...
}
```

```php
switch (n) {
  case label1:
    break;
  case label2:
    break;
  case label3:
    break;
    ...
  default:
}
```



### 循环

`while` `do while` `for` SAME

`foreach`:

```php
foreach ($array as $value) {
	// code here...
}

$colors = array("red", "green", "blue", "yellow");
foreach ($colors as $value) {
  echo "$value <br>";
}
```

### 函数

`function name() {}`

`declare(strict_types=1)`可以限制php在计算时变量类型需统一

参数默认值跟python一样

#### 返回值声明

`function name() : type {return $var}`

#### 引用变量

`function name(&$var)` 类似用指针修改变量



### 超全局变量

- $GLOBALS 全局变量

- $_SERVER 储存服务器信息，页面信息，Headers等

- $_REQUEST 储存请求信息

    ```php+html
    <form method="post" action="<?php echo $_SERVER['PHP_SELF'];?>">
      Name: <input type="text" name="fname">
      <input type="submit">
    </form>
    
    <?php
    if ($_SERVER["REQUEST_METHOD"] == "POST") {
      // collect value of input field
      $name = $_REQUEST['fname'];
      if (empty($name)) {
        echo "Name is empty";
      } else {
        echo $name;
      }
    }
    ?>
    
    ```

    

- $_POST 储存POST请求信息 来自HTTP POST请求

- $_GET 储存GET请求信息 来自URL末尾给出的参数

- $_FILES

- $_ENV

- $_COOKIE

- $_SESSION

## 表单

HTML `<form action="" method="">`标签负责收集表单内容；

php中的 `$_GET` 数组存有上传的表单内容；

![](../media/php/image-20220521221408963.png)

php内的HTML表单（调用该php自己接收表单）：

`<form method="post" action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]);?>">` 

`htmlspecialchars()` 将特殊字符URLEncode，避免跨站脚本攻击

### 验证表单

- 所有用户输入的变量都要经过 `htmlspecialchars()` 函数！！！

- 使用 `trim()` 函数将不必要的字符（多余空格/制表符/换行符）去掉
- 使用 `stripslashes()` 函数将反斜杠 `\` 去除

```php
$name = $email = $gender = $comment = $website = ""; // 变量记得先设为空

if ($_SERVER["REQUEST_METHOD"] == "POST") {
  $name = test_input($_POST["name"]);
  ...
}

function test_input($data) {
  $data = trim($data);
  $data = stripslashes($data);
  $data = htmlspecialchars($data);
  return $data;
}
```

### 必填字段

- 定义错误信息变量： `$nameErr = ""`

- 使用 `empty()` 检查是否为空，若为空将错误信息变量赋值： `$nameErr = "Name is Required."`，不为空则将变量通过验证赋值
- 将错误信息用 `<span>` 元素放在HTML中： `<span class="error">* <?php echo $nameErr;?></span>`

### 字段验证

英文名：

```php
$name = test_input($_POST["name"]);
if (!preg_match("/^[a-zA-Z-' ]*$/",$name)) {
  $nameErr = "Only letters and white space allowed";
}
```

邮箱：

```php
$email = test_input($_POST["email"]);
if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
  $emailErr = "Invalid email format";
}
```

URL:

```php
$website = test_input($_POST["website"]);
if (!preg_match("/\b(?:(?:https?|ftp):\/\/|www\.)[-a-z0-9+&@#\/%?=~_|!:,.;]*[-a-z0-9+&@#\/%=~_|]/i",$website)) {
  $websiteErr = "Invalid URL";
}
```

## 时间

`date(format, timestamp)`

```php
echo "Today is " . date("Y/m/d") . "<br>";
echo "Today is " . date("l"); // 显示星期几
&copy; 20xx-<?php echo date("Y");?> // 版权所有年份
echo "The time is " . date("h:i:sa"); // 时间
date_default_timezone_set("Asia/Shanghai"); // 设置时区

```

## 文件

### 包含文件

`include` & `require` 

### 文件处理

- `readfile()`

- `fopen(file, mode) or die(string);`

    `r/w/a/x` (`+` 文件开始)

- `fread(fstream, size)` 

- `fclose(fstream)`

- `fgets(fstream)` 读入一行，指针指向下一行

- `fgetc(fstream)` 读入一个字符

- `feof(fstream)`

    ```php
    while(!feof($myfile)) {
      echo fgets($myfile) . "<br>";
    }
    
    while(!feof($myfile)) {
      echo fgetc($myfile);
    }
    ```

### 文件上传
