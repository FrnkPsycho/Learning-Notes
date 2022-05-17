## 运行 Shell 脚本

1. 作为可执行程序

    ```shell
    chmod +x ./test.sh #使脚本具有执行权限
    ./test.sh #执行脚本
    ```
	注意，一定要写成 ./test.sh，而不是 test.sh，运行其它二进制的程序也一样，直接写 test.sh，linux 系统会去 PATH 里寻找有没有叫 test.sh 的，而只有 /bin, /sbin, /usr/bin，/usr/sbin 等在 PATH 里，你的当前目录通常不在 PATH 里，所以写成 test.sh 是会找不到命令的，要用 ./test.sh 告诉系统说，就在当前目录找。
    



2. 作为解释器参数

    这种运行方式是，直接运行解释器，其参数就是 shell 脚本的文件名，如：
	`/bin/sh test.sh`
	`/bin/php test.php`
	这种方式运行的脚本，不需要在第一行指定解释器信息，写了也没用。



## Shell 变量

1.  直接赋值 name="ass"

2.  语句赋值

```shell
for file in `ls /etc` ; do
	echo "${file}"
done
```

使用变量 `${变量名}`

只读变量 `readonly 变量名`

删除变量 `unset 变量名`



Shell 变量赋值 `name=value` 不能加空格 注意引号

单引号不会替换变量

双引号遇到$会替换

`$0` 脚本名

`$1 - $9` 参数argv

`$@` 所有参数

`$#` 参数个数

`$?` 上一个命令是否错误 1错误 0无错误

`$_` 上一条命令的参数

`$$` 进程码

`$(command)` 命令输出作为变量

`!!` 完整的上一条命令 `sudo !!`

shebang <https://zh.wikipedia.org/wiki/Shebang>

`source mcd.sh` 将shell脚本定义到shell里面 之后可以直接使用里面定义的函数

`x=$PWD` 将pwd命令的输出传入x 注意大写

或者`x=$(pwd)`

`expr 计算式` shell中数学计算

`cat <(command)` 将后面命令的输出传到stdin