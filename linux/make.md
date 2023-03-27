# Build system

对于大多数系统来说，不论其是否包含代码，都会包含一个“构建过程”。

构建系统：定义*依赖*、*目标*、*规则*以及具体的构建目标，系统的任务则是找到构建这些目标所需要的依赖，并根据规则构建所需的中间产物，直到最终目标被构建出来。

# make

## makefile

```makefile
<target>: <prerequisites>
[tab]<commands>
			
// example:
paper.pdf: paper.tex plot-data.png
	pdflatex paper.tex

plot-%.png: %.dat plot.py
	./plot.py -i $*.dat -o $@
```

使用`:` 右侧的依赖，执行命令，得到左侧文件

`%` 变量匹配pattern

执行后再次执行`make`会提示`up to date`，make会检查依赖是否更改而判断是否需要更新。

```shell
> make -f rule.txt # 自定义makefile文件
```

### 伪目标

```shell
clean:
	rm *.o *.s temp
```

```shell
> make clean # 会执行这个伪目标的命令
```

如果目录下有同名文件，可以指定伪指令：

```shell
.PHONY: clean
clean:
	rm *.o *.s temp
```

### 命令

每行命令在单独的shell中执行，shell之间没有继承关系
解决：反斜杠转义换行符

```shell
var-kept:
    export foo=bar; \
    echo "foo=[$$foo]"
```

# 语法

注释 #

echo @
正常情况下make会打印每条命令再执行，加上@可以关闭该命令的echo
```shell
test:
	@# TESTING...
	# What
```

通配符 
与Bash一样的通配符 * ? \[...\]

模式匹配 %
```shell
%.o: %.c
```

# 变量

```
text = Hello!
test:
	@echo $(text)
```

变量 `$()`

调用shell变量：`$$VAR`

内置变量：
```shell
output:
	$(CC) -o output input.c # $(CC) 指向当前使用的编译器
```

自动变量：
$@
```shell
a.txt b.txt:
	touch $@ # 指代当前目标
```
$<
指代第一个前置
$?
指代比目标更新的所有前置
$^
指代所有前置
$*
指代匹配符%匹配的部分
$(@D) 和 $(@F)
目录名 文件名
$(<D) 和 $(<F)
第一个前置的目录名和文件名
https://www.gnu.org/software/make/manual/html_node/Automatic-Variables.html