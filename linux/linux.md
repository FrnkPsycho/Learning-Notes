# linux

Created: 2021-09-19 23:23:27 +0800

Modified: 2022-04-13 19:38:22 +0800

---


通常服务器使用 LAMP（Linux + Apache + MySQL + PHP）或 LNMP（Linux + Nginx+ MySQL + PHP）

开机流程

![](../media/学习笔记-linux-image1.png){width="5.0in" height="1.9583333333333333in"}

init 进程是系统所有进程的起点，你可以把它比拟成系统所有进程的老祖宗，没有这个进程，系统中任何进程都不会启动。

init 程序首先是需要读取配置文件 /etc/inittab



在Windows叫做"服务"（service），在Linux就叫做"守护进程"（daemon）

Linux允许为不同的场合，分配不同的开机启动程序，这就叫做"运行级别"（runlevel）。也就是说，启动时根据"运行级别"，确定要运行哪些程序。

Linux系统有7个运行级别(runlevel)：
-   运行级别0：系统停机状态，系统默认运行级别不能设为0，否则不能正常启动
-   运行级别1：单用户工作状态，root权限，用于系统维护，禁止远程登陆
-   运行级别2：多用户状态(没有NFS)
-   运行级别3：完全的多用户状态(有NFS)，登陆后进入控制台命令行模式
-   运行级别4：系统未使用，保留
-   运行级别5：X11控制台，登陆后进入图形GUI模式
-   运行级别6：系统正常关闭并重启，默认运行级别不能设为6，否则不能正常启动



关机的命令有 shutdown --h now halt poweroff 和 init 0 , 重启系统的命令有 shutdown --r now reboot init 6。



/usr：

usr 是 unix shared resources(共享资源) 的缩写，这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似于 windows 下的 program files 目录。



使用 ll 或者 ls --l 命令来显示一个文件的属性以及文件所属的用户和组





在 Linux 中第一个字符代表这个文件是目录、文件或链接文件等等。

当为 d 则是目录

当为 - 则是文件

若是 l 则表示为链接文档(link file)；

若是 b 则表示为装置文件里面的可供储存的接口设备(可随机存取装置)；

若是 c 则表示为装置文件里面的串行端口设备，例如键盘、鼠标(一次性读取装置)。



接下来的字符中，以三个为一组，且均为 rwx 的三个参数的组合。

r 代表可读(read)、 w 代表可写(write)、 x 代表可执行(execute)。 要注意的是，这三个权限的位置不会改变，如果没有权限，就会出现减号 - 而已。

![](../media/学习笔记-linux-image2.jpg){width="6.425in" height="5.075in"}

在Linux系统中，用户是按组分类的，一个用户属于一个或多个组。

文件所有者以外的用户又可以分为文件所有者的同组用户和其他用户。



更改文件属性

<table>
<colgroup>
<col style="width: 17%" />
<col style="width: 64%" />
<col style="width: 18%" />
</colgroup>
<thead>
<tr class="header">
<th>chgrp</th>
<th>chgrp [-R] 属组名 文件名</th>
<th>更改属组</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>chown</td>
<td><p>chown [–R] 属主名 文件名</p>
<p>chown [-R] 属主名：属组名 文件名</p></td>
<td>更改属主</td>
</tr>
<tr class="even">
<td>chmod</td>
<td>chmod [-R] xyz 文件或目录</td>
<td>更改属性</td>
</tr>
</tbody>
</table>

-R 递归 所有子文件夹全部应用



各权限的分数对照表如下：

r:4

w:2

x:1

每种身份(owner/group/others)各自的三个权限(r/w/x)分数是需要累加的，例如当权限为： -rwxrwx--- 分数则是：

owner = rwx = 4+2+1 = 7

group = rwx = 4+2+1 = 7

others= --- = 0+0+0 = 0



也可以用符号

u user：用户

g group：组

o others：其他

如 chmod u=rwx,g=rx,o=r test1





文件管理 树状结构

顶级目录：根目录 /

绝对路径 从/写起

相对路径

./ 该目录

../ 同级目录or上级目录



ls（英文全拼：list files）: 列出目录及文件名

-a ：全部的文件，连同隐藏文件( 开头为 . 的文件) 一起列出来(常用)

-d ：仅列出目录本身，而不是列出目录内的文件数据(常用)

-l ：长数据串列出，包含文件的属性与权限等等数据；(常用)



cd（英文全拼：change directory）：切换目录

pwd（英文全拼：print work directory）：显示目前的目录

-P 显示出确实的路径，而非使用连结 (link) 路径。

mkdir（英文全拼：make directory）：创建一个新的目录

-m ：配置文件的权限喔！直接配置，不需要看默认权限 (umask) 的脸色～

-p ：帮助你直接将所需要的目录(包含上一级目录)递归创建起来！

mkdir -m 711 test2

mkdir -p test1/test2/test3/test4

rmdir（英文全拼：remove directory）：删除一个**空**的目录

-p ：连同上一级『空的』目录也一起删除

cp（英文全拼：copy file）: 复制文件或目录

**-a：相当於 -pdr 的意思，至於 pdr 请参考下列说明；(常用)**

-d：若来源档为连结档的属性(link file)，则复制连结档属性而非文件本身；

-f：为强制(force)的意思，若目标文件已经存在且无法开启，则移除后再尝试一次；

**-i：若目标档(destination)已经存在时，在覆盖时会先询问动作的进行(常用)**

-l：进行硬式连结(hard link)的连结档创建，而非复制文件本身；

-p：连同文件的属性一起复制过去，而非使用默认属性(备份常用)；

**-r：递归持续复制，用於目录的复制行为；(常用)**

-s：复制成为符号连结档 (symbolic link)，亦即『捷径』文件；

-u：若 destination 比 source 旧才升级 destination ！



rm（英文全拼：remove）: 删除文件或目录

-f ：就是 force 的意思，忽略不存在的文件，不会出现警告信息；

-i ：互动模式，在删除前会询问使用者是否动作

-r ：递归删除啊！最常用在目录的删除了！这是非常危险的选项！！！

mv（英文全拼：move file）: 移动文件与目录，或修改文件与目录的名称

-f ：force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖；

-i ：若目标文件 (destination) 已经存在时，就会询问是否覆盖！

-u ：若目标文件已经存在，且 source 比较新，才会升级 (update)



文件查看

cat 由第一行开始显示文件内容

tac 从最后一行开始显示，可以看出 tac 是 cat 的倒着写！

nl 显示的时候，顺道输出行号！

more 一页一页的显示文件内容

space 下一页

enter 下一行

/搜索

f 显示信息

q 退出

less 与 more 类似，但是比 more 更好的是，他可以往前翻页！

空白键 ：向下翻动一页；

[pagedown]：向下翻动一页；

[pageup] ：向上翻动一页；

/字串 ：向下搜寻『字串』的功能；

?字串 ：向上搜寻『字串』的功能；

n ：重复前一个搜寻 (与 / 或 ? 有关！)

N ：反向的重复前一个搜寻 (与 / 或 ? 有关！)

q ：离开 less 这个程序；



head 只看头几行

tail 只看尾巴几行



tree 路径 -L 级数

tree 路劲 -L 级数 > xxx.txt 保存树状目录



touch 命令有两个功能：一是用于把已存在文件的时间标签更新为系统当前的时间（默认方式），它们的数据将原封不动地保留下来；二是用来创建空文件。



ln 命令用来为文件创建连接，连接类型分为硬连接和符号连接两种，默认的连接类型是硬连接。如果要创建符号连接必须使用"-s"选项。



🔔 注意：符号链接文件不是一个独立的文件，它的许多属性依赖于源文件，所以给符号链接文件设置存取权限是没有意义的。





Linux 用户和用户组管理



Linux系统用户账号的管理

useradd 选项 用户名

-c comment 指定一段注释性描述。

-d 目录 指定用户主目录，如果此目录不存在，则同时使用-m选项，可以创建主目录。

-g 用户组 指定用户所属的用户组。

-G 用户组，用户组 指定用户所属的附加组。

-s Shell文件 指定用户的登录Shell。

-u 用户号 指定用户的用户号，如果同时有-o选项，则可以重复使用其他用户的标识号。

userdel 选项 用户名

常用的选项是 -r，它的作用是把用户的主目录一起删除。

usermod 选项 用户名

选项与useradd相同



指定和修改用户口令的Shell命令是passwd

passwd 选项 用户名

-l 锁定口令，即禁用账号。

-u 口令解锁。

-d 使账号无口令。

-f 强迫用户下次登录时修改口令。

不加用户名默认当前账户





Linux系统用户组的管理

增加一个新的用户组使用groupadd命令

groupadd 选项 用户组

-g GID 指定新用户组的组标识号（GID）。

-o 一般与-g选项同时使用，表示新用户组的GID可以与系统已有用户组的GID相同。



如果要删除一个已有的用户组，使用groupdel命令

groupdel 用户组



修改用户组的属性使用groupmod命令

groupmod 选项 用户组





/etc/passwd文件是用户管理工作涉及的最重要的一个文件

用户名:口令:用户标识号:组标识号:注释性描述:主目录:登录Shell



系统中有一类用户称为伪用户（pseudo users）。

这些用户在/etc/passwd文件中也占有一条记录，但是不能登录，因为它们的登录Shell为空。它们的存在主要是方便系统管理，满足相应的系统进程对文件属主的要求。

常见的伪用户如下所示：



/etc/shadow

登录名:加密口令:最后一次修改时间:最小时间间隔:最大时间间隔:警告时间:不活动时间:失效时间:标志



用户组的所有信息都存放在/etc/group文件中。

组名:口令:组标识号:组内用户列表



批量创建 <https://www.runoob.com/linux/linux-user-manage.html>



Linux 磁盘管理



df [-ahikHTm] [目录或文件名]

-**a ：列出所有的文件系统，包括系统特有的 /proc 等文件系统；**

-k ：以 KBytes 的容量显示各文件系统；

-m ：以 MBytes 的容量显示各文件系统；

**-h ：以人们较易阅读的 GBytes, MBytes, KBytes 等格式自行显示；**

-H ：以 M=1000K 取代 M=1024K 的进位方式；

**-T ：显示文件系统类型, 连同该 partition 的 filesystem 名称 (例如 ext3) 也列出；**

-i ：不用硬盘容量，而以 inode 的数量来显示



du [-ahskm] 文件或目录名称

与 df 命令不同的是 Linux du 命令是对文件和目录磁盘使用的空间的查看

**-a ：列出所有的文件与目录容量，因为默认仅统计目录底下的文件量而已。**

**-h ：以人们较易读的容量格式 (G/M) 显示；**

**-s ：列出总量而已，而不列出每个各别的目录占用容量；**

-S ：不包括子目录下的总计，与 -s 有点差别。

-k ：以 KBytes 列出容量显示；

-m ：以 MBytes 列出容量显示；







fdisk



mkfs 格式化文件系统



fsck（file system check）用来检查和维护不一致的文件系统。

-t : 给定档案系统的型式，若在 /etc/fstab 中已有定义或 kernel 本身已支援的则不需加上此参数

-s : 依序一个一个地执行 fsck 的指令来检查

-A : 对/etc/fstab 中所有列出来的 分区（partition）做检查

-C : 显示完整的检查进度

-d : 打印出 e2fsck 的 debug 结果

-p : 同时有 -A 条件时，同时有多个 fsck 的检查一起执行

-R : 同时有 -A 条件时，省略 / 不检查

-V : 详细显示模式

-a : 如果检查有错则自动修复

-r : 如果检查有错则由使用者回答是否修复

-y : 选项指定检测每个文件是自动输入yes，在不确定那些是不正常的时候，可以执行 # fsck -y 全部检查修复。





磁盘挂载与卸除

Linux 的磁盘挂载使用 mount 命令，卸载使用 umount 命令。

mount [-t 文件系统] [-L Label名] [-o 额外选项] [-n] 装置文件名 挂载点

-f ：强制卸除！可用在类似网络文件系统 (NFS) 无法读取到的情况下；

-n ：不升级 /etc/mtab 情况下卸除。



列出所有可更新的软件清单命令：sudo apt update



升级软件包：sudo apt upgrade



列出可更新的软件包及版本信息：apt list --upgradeable



升级软件包，升级前先删除需要更新软件包：sudo apt full-upgrade



安装指定的软件命令：sudo apt install <package_name>



安装多个软件包：sudo apt install <package_1> <package_2> <package_3>



更新指定的软件命令：sudo apt update <package_name>



显示软件包具体信息,例如：版本号，安装大小，依赖关系等等：sudo apt show <package_name>



删除软件包命令：sudo apt remove <package_name>



清理不再使用的依赖和库文件: sudo apt autoremove



移除软件包及配置文件: sudo apt purge <package_name>



查找软件包命令： sudo apt search <keyword>



列出所有已安装的包：apt list --installed



列出所有已安装的包的版本信息：apt list --all-versions

运行 Shell 脚本有两种方法：

1、作为可执行程序

chmod +x ./test.sh #使脚本具有执行权限

./test.sh #执行脚本



注意，一定要写成 ./test.sh，而不是 test.sh，运行其它二进制的程序也一样，直接写 test.sh，linux 系统会去 PATH 里寻找有没有叫 test.sh 的，而只有 /bin, /sbin, /usr/bin，/usr/sbin 等在 PATH 里，你的当前目录通常不在 PATH 里，所以写成 test.sh 是会找不到命令的，要用 ./test.sh 告诉系统说，就在当前目录找。



2、作为解释器参数

这种运行方式是，直接运行解释器，其参数就是 shell 脚本的文件名，如：

/bin/sh test.sh

/bin/php test.php

这种方式运行的脚本，不需要在第一行指定解释器信息，写了也没用。



Shell 变量

1.  直接赋值 name="ass"

2.  语句赋值

for file in `ls /etc` ;do

echo "${file}"

done



使用变量

${变量名}



只读变量

readonly 变量名



删除变量

unset 变量名







cd - 上一次在的目录

目录权限解释：

r 查看目录中的文件

w是对目录的重命名 删除 创建文件

x 搜索 进入目录（父目录都要有x权限）

文件的w权限不能删除文件本身 最多只能空文件



ctrl+L 清屏



流

IO流

xxx > xx.txt 将前面命令的输出传入后面的文件中

xxx < xx.txt 将后面文件的内容作为输入给前面的命令



xxx < xx.txt > xx2.txt

将xx.txt传给xxx 再将输出写入xx2.txt



>> 追加不覆盖



| pipe符号

ls -l | tail -n1 输出ls-l的最后一行

左边命令的输出给右边 最后输出



curl --head --silent baidu.com | grep -i content-length | cut --delimiter=" " -f2

输出content-length数值



$ 表示非root用户

#表示root用户



sudo xxx | xxx sudo无法对后面的命令



shell 变量赋值 name=value 不能加空格 注意引号

单引号不会替换变量

双引号遇到$会替换

$0 脚本名

$1 - $9 参数argv

$@ 所有参数

$# 参数个数

$? 上一个命令是否错误 1错误 0无错误

$_ 上一条命令的参数

$$ 进程码

$(command) 命令输出作为变量

!! 完整的上一条命令 sudo !!



shebang <https://zh.wikipedia.org/wiki/Shebang>



source mcd.sh 将shell脚本定义到shell里面 之后可以直接使用里面定义的函数



x=$PWD 将pwd命令的输出传入x 注意大写

或者x=$(pwd)



`expr 计算式`



cat <(command) 将后面命令的输出传到stdin



通配符 ? *



touch {a,b,c}.txt 创建a.txt b.txt c.txt

结合通配符 mv *{.py,.c,.sh} folder

touch {foo,bar}/{a..h} 在foo和bar两个文件夹里依次创建a b c ... g h 这些文件



shebang env



查找目录

find <path> -name/path/mtime/size

名称 路径 修改时间 大小



fd find替代

locate



history 历史命令

ctrl+R 回溯搜索命令



chsh 更改默认shell



xargs -d 'delimiter'

j

tar -cvf -czf





移动

**基本移动: hjkl （左， 下， 上， 右）**

**词： w （下一个词）， b （词初）， e （词尾）**

行： 0 （行初）， ^ （第一个非空格字符）， $ （行尾）

屏幕： H （屏幕首行）， M （屏幕中间）， L （屏幕底部）

翻页： Ctrl-u （上翻）， Ctrl-d （下翻）

**文件： gg （文件头）， G （文件尾）**

行数： :{行数}<CR> 或者 {行数}G ({行数}为行数)

杂项： % （找到配对，比如括号或者 /* */ 之类的注释对）

查找： f{字符}， t{字符}， F{字符}， T{字符}

查找/到 向前/向后 在本行的{字符}

, / ; 用于导航匹配

**搜索: /{正则表达式}, n / N 用于导航匹配**

**? 相反方向搜索 n与N也是反方向**



**v 可视化模式**

V 可视化行

ctrl+v 可视化块



i 插入

O向上插入一行 o向下插入一行

d{移动命令} 删除

dw 删除词, d$ 删除到行尾, d0 删除到行头。

x 删除字符

s 删除并插入（替换）

y 复制

p 粘贴

c 变化 （可以带动作的 删除并插入）

u 撤销 U整行撤销 ctrl+r 重做

vim中的删除都是剪切 按p就可以粘贴

dd剪切一整行 p粘贴在光标下一行

r 替换单个字符

ctrl+r 覆盖模式



数字+命令 执行若干次

5o 向下5行 可以输入文字 复制5行



修饰语

i内部 在内

a周围

ci( 改变当前括号内的内容

ci[ 改变当前方括号内的内容

da' 删除一个单引号字符串， 包括周围的单引号



python自带httpserver

python3 -m http.server



Operator [number] motion

ctrl+g 信息

G 文件尾 gg 文件头



[数字]G 跳转到哪一行 （或者:[行数]）

ctrl+o 跳转前的位置 ctrl+I 跳转后的位置

放在某个括号[({上 按% 找到它对应的括号})]



:[开始行],[结束行]s/old/new/g

old 查找串

new 替换串

g globally 整行替换

没有指定行就是光标行



:%s/old/new/g 全文件替换 gc 询问要不要



:r 插入文件

:r !ls 将ls的输出插入

:w 保存文件 可以保存文件部分内容



a 在光标后插入

ctrl + a 在行尾插入

i在光标前插入



set 命令 设置

:set ic(noic) 忽略大小写 （暂时忽略可以在/xxx后加上c）

:set hls 搜索高亮



ctrl+w 在窗口之间跳转

crtl+d 代码补全







easymotion

<leader><leader>s<char> 搜索字符





surround 括号操作

ds<char>

cs<char><char>



:%!xxd vim hex edit mode

xxd 将二进制转为十六进制输出



objdump -d {filename}

反汇编文件输出









tar -tf xxx.zip/tar

预览压缩文件内容（不解压）

rar



SIGINT Ctrl+C

SIGQUIT Ctrl+

SIGTERM `kill -TERM <pid>`



SIGSTOP pause

SIGTSTP suspend Ctrl+Z

recover by `fg/bg`

SIGHUP close all when exiting terminal

`nohup <command>`



`jobs` show all paused jobs



`bg %num` recover [num] job







tmux 终端多路复用

Ctrl+B 再输入指令

tmux new -s NAME

tmux ls



`<C-b> c` create

`<C-d>` close

`<C-b> p/n` prev/next

`<C-b> "/%` horizonal/vertical





alias short_name="FULL COMMAND"

