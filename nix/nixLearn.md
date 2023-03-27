 # nix 学习笔记

## nix 运行程序

`nix run`

```shell
echo "Hello Nix" | nix run "nixpkgs#ponysay"
```

## nix 开发环境

`nix develop`

`type <command>` 解释命令所指向的程序

`shell hook` 环境启动时自动运行的shell脚本

`nix develop --command <command>`

### flake

```shell
nix flake init --template "github:DeterminateSystems/zero-to-nix#python-dev"
nix develop
```

`devShells`

flake.nix

## nix 包构建

`nix build <flake>`

```shell
nix build "nixpkgs#bat"
```

`readlink`

### home-manager

```shell
nix build "github:nix-community/home-manager"
```

## nixpkgs

`nix search nixpkgs`

`nixpkgs` [flake reference](https://zero-to-nix.com/concepts/flakes#references) is shorthand for `github:NixOS/nixpkgs`

`nix flake show <flake>` 

## nix language

### 基础

`nix repl`

惰性，只有需要时才被求值

#### 依赖关系

```
let 
  赋值语句
in
  表达式
```

```nix
let
  foo = { bar.qux = 1; };
  lax = foo.bar.qux;
in
  lax  # 我们需要 lax，lax 需要 foo.bar.qux
```

求值
```shell
nix-instantiate --eval file.nix
```

详尽求值
```shell
nix-instantiate --eval --strict file.nix
```

#### 属性集 attribute set

```
{
  string = "hello";
  integer = 1;
  float = 3.141;
  bool = true;
  null = null;
  list = [ 1 "two" false ];
  attribute-set = {
    a = "hello";
    b = 2;
    c = 2.718;
    d = false;
  };  # 标准 json 不支持注释
}
```

```
list = [ a b c ];
# 空格分隔
```

#### 递归属性集

属性集内访问自己的属性

```
rec {
...
}
```

#### let in

let 赋值
in 一个表达式

有作用域！

#### 属性访问

`.`

#### with 表达式

少写属性集名称

```
let
  a = {
    x = 1;
    y = 2;
    z = 3;
  };
in
  with a; [ x y z ]  # 等价 [ a.x a.y a.z ]
```

```nix
let
  alice = {
    name = "Alice";
    age = 18;
    gender = "Non-binary";
  };
in
  {
    withoutGender = with alice; [ name age ];
    withoutAge = with alice; [ name gender ];
    name = alice.name;
  }

```

#### inherit 表达式

完成一对命名相同的名称和属性之间的赋值

```
let
  alice = {
    name = "Alice";
    age = 18;
    gender = "Non-binary";
  };
  a = 1;
in
  {
    withoutGender = with alice; [ name age ];
    withoutAge = with alice; [ name gender ];
    inherit (alice) name age gender;
    inheritName = name; # "Alice"
    inherit a; # 1
  }
```

#### 字符串插入

`"${Name}"`

#### 路径

路径是独立的类型

```
./sub
/a/b/c
../
../../
./
```

#### 检索路径

系统变量获取路径

```
<nixpkgs>
```

#### 多行字符串

```nix
''
mkdir ass
cd ass
ls -alh
''
```

#### 函数

##### 匿名函数 Lambda

`:` 左边是参数，右边是表达式
```
x: x + 1
x: y: x + y
{ a, b } : a + b
{ a, b ? 0 } : a + b
args@{ a, b, ...}: a + b + args.c
```

函数声明（？）
```
concat = { a, b }: a + b  # 等价于 concat = x: x.a + x.b
concat { a = "Hello"; b = "NixOS"; }
```

##### 柯里化函数

将 `f (a,b,c)` 转换为 `f (a)(b)(c)` 的过程就是柯里化

```
f = x: ( y: x + y )

> f 1
«lambda @ «string»:1:7»
# f 1 --> y: 1 + y

> f 1 1
2
# f 1 1 --> 1 + 1
```

##### 属性集参数

关键字参数（

```
> f = { a, b ? 0 } : a + b
> f { a = 1; b = 2; }
3
> f { a = 1; }
1
```

#### 内建方法

`builtins.`

```
with builtins; [
	(getEnv "PATH")
]
```

#### 导入

`import ./file.nix` 相对路径不能是字符串！

`import <nixpkgs>`

```
let
  pkgs = import <nixpkgs> {};
in
  pkgs.lib.strings.toUpper "search paths considered harmful"
```

https://learnxinyminutes.com/docs/nix/

#### 列表

```
[] ++ []
builtins.length [ 1 2 3 ]
builtins.concatLists [ [1] [2] [3] ]
builtins.head [ 1 2 3 ]
builtins.tail [ 1 2 3 ]
builtins.elemAt [ 1 2 3 4 ] 3 ==> 4
builtins.elem [ 1 2 3 4 ] 0 ==> false
builtins.filter (n: n < 3) [1 2 3 4]
```

#### 集合

```
set = { key = "value" ;};
set.key ==> "value"
set?foo ==> false
newSet = set // { foo = "bar";};

```

#### 集合Pattern

```nix
({x, y}: x + y) { x = "a"; y = "b"; }
({x, y, ...}) { x = "a"; y = "b"; z = "c";}
```

#### 异常

```nix
throw "error!"
抛出错误

builtins.tryEval ( throw "oops!" )
返回Set：表达式是否成功，成功的值为多少

builtins.abort "fatal error!"
致命错误，无法被catch

assert 1 < 2; value
真则返回value 假则throw异常（tryEval来Catch）

```

#### 污染

```
builtins.getEnv "PATH"
```

##### fetchers

```
with builtins;
fetchurl
fetchTarball ==> HASH-source
fetchGit
fetchClosure

```

#### 文件

```
let
  filename = toFile ""
```

### 推导 derivation

build task

example:

```nix
{ lib, stdenv }:

stdenv.mkDerivation rec {

  pname = "hello";

  version = "2.12";

  src = builtins.fetchTarball {
    url = "mirror://gnu/${pname}/${pname}-${version}.tar.gz";
    sha256 = "1ayhp9v4m4rdhjmnl2bq3cibrbqqkgjbl3s7yk2nhlh8vj3ay16g";
  };

  meta = with lib; {
    license = licenses.gpl3Plus;
  };

}
```

### Shell 环境

```
{ pkgs ? import <nixpkgs> {} }:
let
  message = "MAGIC!";
in
pkgs.mkShell {
  buildInputs = with pkgs; [ ponysay ];
  shellHook = ''
    ponysay ${message}
  '';
}
```