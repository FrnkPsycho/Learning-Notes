# ISA

Instruction Set Architecture 指令集架构

接口规范 比如基本类型/寄存器/指令/寻址模式/异常中断处理

应用-操作系统-ISA-微架构

CISC 复杂指令集
RISC 精简指令集

## 宽度

CPU中通用寄存器的宽度（二进制位数）：8/16/32/64-bit

## 命名规范

`RV[###][abc...xyz]`

`[###]` 字宽

`[abc...xyz]` 模块

### 模块化

RISC：1个基本整数指令集 + 多个可选的扩展指令集

`RV32I` `I`: 整数
`RV32E` `E`: 嵌入式整数集
`RV64I` 兼容`RV32I`

扩展指令集：
M 乘除指令集
A 原子指令集
F 单精度浮点
D 双精度浮点
C 压缩指令集

G 通用：IMAFD

## 通用寄存器

32个寄存器 `x0-x31` + PC寄存器（不暴露）

## HART

**HAR**dware **T**hread

一个处理器可以有多个指令执行流，core、cpu、thread可能会产生歧义，定义了一种叫做HART的抽象概念

一个hart是能够独立地取指执行RISCV指令的资源

## 特权级

三种态：`User` / `Supervisor` / `Machine` 

x86 -> RISC-V
用户态->User
内核态+虚拟地址->Supervisor
内核态+物理地址（实模式）->Machine 

只有三种模式都支持才能实现进程（虚拟地址）

### CSR

不同特权级分别对应不同的  **控制和状态寄存器 Control and Status Registers**

![[Pasted image 20230325155005.png]]

高级别特权级可以访问低级别的CSR

Zicsr 扩展指令集 操作CSR指令

特定指令可以切换特权级

## 内存管理与保护

### 物理内存保护 

Physical Memory Protection

允许M模式指定U模式可以访问的内存地址
R/W/X Lock

### 虚拟内存

## 异常和中断

Exception Interrupt

异常：处理后返回同一条指令
中断：处理后返回到下一条指令

# 编译和链接

### GCC

```shell
gcc
-E 预处理
-c 只编译不链接 .o
-S 生成汇编代码 .s
-o 生成ELF程序
-g 加入调试信息
-v verbose
```

预处理cc1->编译cc1->汇编as->链接ld

### ELF

Executable Linkable Format

- 可重定位文件 .o
- 可执行文件
- 共享目标文件 .so
- 核心转储文件Core Dump core

![[Pasted image 20230325162614.png]]

Segment: 为了节省内存（因为有对齐）将同类型的Section合并

### Binutils

```
ar / tar 归档
as 汇编器
ld 链接器

objcopy 文件格式转换
objdump 显示ELF信息 -S 反汇编
readelf 更多ELF信息
```

# 嵌入式简介

## 交叉编译

`build` 构建系统：生成编译器程序
`host` 主机系统： 运行编译器程序，编译并链接应用程序
`target` 目标系统： 运行应用程序

![[Pasted image 20230325165518.png]]

### 工具链

Toolchain

命名： `arch-vendor-os1-[os2-]xxx`

```
x86_64-linux-gnu-gcc
riscv64-unknown-elf-gcc
```

## GDB

gdbserver

![[Pasted image 20230325170540.png]]

## QEMU

User Mode 直接运行应用程序
System Mode 模拟整个系统


# RISC-V 汇编

## 语法

由多条语句 `statement` 组成

`[label:] [operation] [comment]`

```asm
.macro do_nothing	# directive
	nop		# pseudo-instruction
	nop		# pseudo-instruction
.endm			# directive

	.text		# directive
	.global _start	# directive
_start: 		# Label
	li x6, 5	# pseudo-instruction
	li x7, 4	# pseudo-instruction
	add x5, x6, x7	# instruction
	do_nothing	# Calling macro
stop:	j stop		# statement in one line

	.end		# End of file

```

operation:

- instruction
- pseudo-instruction 
  指示汇编器产生多条实际的指令
- directive （指示/伪操作） 
  `.` 开头，指示汇编器如何生成代码
- macro `.macro/.endm` 

comment: 一般以 # 开头


## 操作对象 Operand

### 寄存器

32个通用寄存器（RV32I），
`XLEN` 寄存器宽度（RV32I为32位）

x0 零寄存器
pc 计数寄存器（不可读）

Hart进行算术逻辑运算时只能来自于寄存器

### 内存

读写操作按字节为基本单位进行寻址

## 编码格式

![[Pasted image 20230326144836.png]]

- 指令长度 `ILEN1`：32bits
- 指令对齐 `IALIGN`：32bits
- 32bits划分为不同的域 field
- funct3/funct7 与 opcode 决定指令类型
- 指令按照小端序排列


![[Pasted image 20230327102501.png]]

