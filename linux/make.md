# Build system

对于大多数系统来说，不论其是否包含代码，都会包含一个“构建过程”。

构建系统：定义*依赖*、*目标*、*规则*以及具体的构建目标，系统的任务则是找到构建这些目标所需要的依赖，并根据规则构建所需的中间产物，直到最终目标被构建出来。

# make

## makefile

```makefile
<output_file>: <dependence>
	<command>
			
// example:
paper.pdf: paper.tex plot-data.png
	pdflatex paper.tex

plot-%.png: %.dat plot.py
	./plot.py -i $*.dat -o $@
```

使用`:` 右侧的依赖，执行命令，得到左侧文件

`%` 变量匹配pattern

执行后再次执行`make`会提示`up to date`，make会检查依赖是否更改而判断是否需要更新。