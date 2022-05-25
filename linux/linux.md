# Linux

## 基础

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



关机的命令有 `shutdown --h now halt poweroff` 和 `init 0` , 重启系统的命令有 `shutdown --r now reboot` `init 6`。

/usr：

usr 是 unix shared resources(共享资源) 的缩写，这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似于 windows 下的 program files 目录。

- `/bin` - 基本命令二进制文件

- `/sbin` - 基本的系统二进制文件，通常是root运行的

- `/dev` - 设备文件，通常是硬件设备接口文件

- `/etc` - 主机特定的系统配置文件

- `/home` - 系统用户的主目录

- `/lib` - 系统软件通用库

- `/opt` - 可选的应用软件

- `/sys` - 包含系统的信息和配置([第一堂课](https://missing-semester-cn.github.io/2020/course-shell/)介绍的)

- `/tmp` - 临时文件( `/var/tmp` ) 通常重启时删除

- `/usr/` - 只读的用户数据
    - `/usr/bin` - 非必须的命令二进制文件
    - `/usr/sbin` - 非必须的系统二进制文件，通常是由root运行的
    - `/usr/local/bin` - 用户编译程序的二进制文件

- `/var` -变量文件 像日志或缓存

$ 表示非root用户

#表示root用户

### 文件属性

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



#### 更改文件属性

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
`-R` 递归 所有子文件夹全部应用

#### 权限

各权限的分数对照表如下：

`r:4` `w:2` `x:1`

每种身份(owner/group/others)各自的三个权限(r/w/x)分数是需要累加的，例如当权限为： -rwxrwx--- 分数则是：

```
owner = rwx = 4+2+1 = 7
group = rwx = 4+2+1 = 7
others= --- = 0+0+0 = 0
```

也可以用符号

`u` user：用户

`g` group：组

`o` others：其他

如 `chmod u=rwx,g=rx,o=r test1`

#### 目录权限

`r` 查看目录中的文件

`w` 是对目录的重命名 删除 创建文件

`x` 搜索 进入目录（父目录都要有x权限）

文件的w权限不能删除文件本身 最多只能空文件

### 文件管理

树状结构

顶级目录：根目录 /

绝对路径 从/写起

相对路径

`./` 该目录

`../` 同级目录or上级目录



`ls`（英文全拼：list files）: 列出目录及文件名

​	-a ：全部的文件，连同隐藏文件( 开头为 . 的文件) 一起列出来(常用)

​	-d ：仅列出目录本身，而不是列出目录内的文件数据(常用)

​	-l ：长数据串列出，包含文件的属性与权限等等数据；(常用)



`cd`（英文全拼：change directory）：切换目录

`cd -` 上一次访问的目录

`cd ~` home目录



`pwd`（英文全拼：print work directory）：显示目前的目录

​	-P 显示出确实的路径，而非使用连结 (link) 路径。



`mkdir`（英文全拼：make directory）：创建一个新的目录

​	-m ：配置文件的权限喔！直接配置，不需要看默认权限 (umask) 的脸色～

​	-p ：帮助你直接将所需要的目录(包含上一级目录)递归创建起来！

```
mkdir -m 711 test2
mkdir -p test1/test2/test3/test4
```



`rmdir`（英文全拼：remove directory）：删除一个**空**的目录

​	-p ：连同上一级『空的』目录也一起删除



`cp`（英文全拼：copy file）: 复制文件或目录

​	**-a：相当於 -pdr 的意思，至於 pdr 请参考下列说明；(常用)**

​	-d：若来源档为连结档的属性(link file)，则复制连结档属性而非文件本身；

​	-f：为强制(force)的意思，若目标文件已经存在且无法开启，则移除后再尝试一次；

​	**-i：若目标档(destination)已经存在时，在覆盖时会先询问动作的进行(常用)**

​	-l：进行硬式连结(hard link)的连结档创建，而非复制文件本身；

​	-p：连同文件的属性一起复制过去，而非使用默认属性(备份常用)；

​	**-r：递归持续复制，用於目录的复制行为；(常用)**

​	-s：复制成为符号连结档 (symbolic link)，亦即『捷径』文件；

​	-u：若 destination 比 source 旧才升级 destination ！



`rm`（英文全拼：remove）: 删除文件或目录

-f ：就是 force 的意思，忽略不存在的文件，不会出现警告信息；

-i ：互动模式，在删除前会询问使用者是否动作

-r ：递归删除啊！最常用在目录的删除了！这是非常危险的选项！！！

mv（英文全拼：move file）: 移动文件与目录，或修改文件与目录的名称

-f ：force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖；

-i ：若目标文件 (destination) 已经存在时，就会询问是否覆盖！

-u ：若目标文件已经存在，且 source 比较新，才会升级 (update)



### 文件查看

`cat` 由第一行开始显示文件内容

`tac` 从最后一行开始显示，可以看出 tac 是 cat 的倒着写！

`nl` 显示的时候，顺道输出行号！



`more` 一页一页的显示文件内容

​	space 下一页

​	enter 下一行

​	/搜索

​	f 显示信息

​	q 退出



`less` 与 `more` 类似，但是比 more 更好的是，他可以往前翻页！

​	空白键 ：向下翻动一页；

​	[pagedown]：向下翻动一页；

​	[pageup] ：向上翻动一页；

​	/字串 ：向下搜寻『字串』的功能；

​	?字串 ：向上搜寻『字串』的功能；

​	n ：重复前一个搜寻 (与 / 或 ? 有关！)

​	N ：反向的重复前一个搜寻 (与 / 或 ? 有关！)

​	q ：离开 less 这个程序；



`head`只看头几行

`tail` 只看尾巴几行



`tree 路径 -L 级数`

`tree 路径 -L 级数 > xxx.txt`



`touch` 命令有两个功能：

一是用于把已存在文件的时间标签更新为系统当前的时间（默认方式），它们的数据将原封不动地保留下来；

二是用来创建空文件。



`ln` 命令用来为文件创建连接，连接类型分为硬连接和符号连接两种，默认的连接类型是硬连接。

如果要创建符号连接必须使用"-s"选项。

🔔 注意：符号链接文件不是一个独立的文件，它的许多属性依赖于源文件，所以给符号链接文件设置存取权限是没有意义的。



### 用户管理

#### 用户账号管理

`useradd 选项 用户名`

​	`-c comment` 指定一段注释性描述。

​	`-d 目录` 指定用户主目录，如果此目录不存在，则同时使用-m选项，可以创建主目录。

​	`-g 用户组` 指定用户所属的用户组。

​	`-G 用户组,用户组 `指定用户所属的附加组。

​	`-s Shell文件` 指定用户的登录Shell。

​	`-u 用户号` 指定用户的用户号，如果同时有-o选项，则可以重复使用其他用户的标识号。



`userdel 选项 用户名`

常用的选项是 -r，它的作用是把用户的主目录一起删除。



`usermod 选项 用户名`

选项与useradd相同



指定和修改用户口令的Shell命令是`passwd`

`passwd 选项 用户名`

​	-l 锁定口令，即禁用账号。

​	-u 口令解锁。

​	-d 使账号无口令。

​	-f 强迫用户下次登录时修改口令。

不加用户名默认当前账户



#### 用户组管理

增加一个新的用户组使用groupadd命令

`groupadd 选项 用户组`

​	-g GID 指定新用户组的组标识号（GID）。

​	-o 一般与-g选项同时使用，表示新用户组的GID可以与系统已有用户组的GID相同。



如果要删除一个已有的用户组，使用groupdel命令

`groupdel 用户组`



修改用户组的属性使用groupmod命令

`groupmod 选项 用户组`



`/etc/passwd`文件是用户管理工作涉及的最重要的一个文件

`用户名:口令:用户标识号:组标识号:注释性描述:主目录:登录Shell`



系统中有一类用户称为伪用户（pseudo users）。

这些用户在/etc/passwd文件中也占有一条记录，但是不能登录，因为它们的登录Shell为空。它们的存在主要是方便系统管理，满足相应的系统进程对文件属主的要求。



`/etc/shadow`

`登录名:加密口令:最后一次修改时间:最小时间间隔:最大时间间隔:警告时间:不活动时间:失效时间:标志`



用户组的所有信息都存放在`/etc/group`文件中。

`组名:口令:组标识号:组内用户列表`



批量创建 <https://www.runoob.com/linux/linux-user-manage.html>



### 磁盘管理



`df [-ahikHTm] [目录或文件名]`

​	-**a ：列出所有的文件系统，包括系统特有的 /proc 等文件系统；**

​	-k ：以 KBytes 的容量显示各文件系统；

​	-m ：以 MBytes 的容量显示各文件系统；

​	**-h ：以人们较易阅读的 GBytes, MBytes, KBytes 等格式自行显示；**

​	-H ：以 M=1000K 取代 M=1024K 的进位方式；

​	**-T ：显示文件系统类型, 连同该 partition 的 filesystem 名称 (例如 ext3) 也列出；**

​	-i ：不用硬盘容量，而以 inode 的数量来显示



`du [-ahskm] 文件或目录名称`

与 df 命令不同的是 Linux du 命令是对文件和目录磁盘使用的空间的查看

​	**-a ：列出所有的文件与目录容量，因为默认仅统计目录底下的文件量而已。**

​	**-h ：以人们较易读的容量格式 (G/M) 显示；**

​	**-s ：列出总量而已，而不列出每个各别的目录占用容量；**

​	-S ：不包括子目录下的总计，与 -s 有点差别。

​	-k ：以 KBytes 列出容量显示；

​	-m ：以 MBytes 列出容量显示；

`fdisk`

`mkfs `格式化文件系统

`fsck`（file system check）用来检查和维护不一致的文件系统。



#### 磁盘挂载与卸除

Linux 的磁盘挂载使用 mount 命令，卸载使用 umount 命令。

`mount [-t 文件系统] [-L Label名] [-o 额外选项] [-n] 装置文件名 挂载点`

​	-f ：强制卸除！可用在类似网络文件系统 (NFS) 无法读取到的情况下；

​	-n ：不升级 /etc/mtab 情况下卸除。



### apt 包管理

列出所有可更新的软件清单命令：`sudo apt update`

升级软件包：`sudo apt upgrade`

列出可更新的软件包及版本信息：`apt list --upgradeable`

升级软件包，升级前先删除需要更新软件包：`sudo apt full-upgrade`

安装指定的软件命令：`sudo apt install <package_name>`

安装多个软件包：`sudo apt install <package_1> <package_2> <package_3>`

更新指定的软件命令：`sudo apt update <package_name>`

显示软件包具体信息,例如：版本号，安装大小，依赖关系等等：`sudo apt show <package_name>`

删除软件包命令：`sudo apt remove <package_name>`

清理不再使用的依赖和库文件: `sudo apt autoremove`

移除软件包及配置文件: `sudo apt purge <package_name>`

查找软件包命令： `sudo apt search <keyword>`

列出所有已安装的包：`apt list --installed`

列出所有已安装的包的版本信息：`apt list --all-versions`



### 流

`xxx > xx.txt` 将前面命令的输出传入后面的文件中

`xxx < xx.txt` 将后面文件的内容作为输入给前面的命令

`xxx < xx.txt > xx2.txt` 将xx.txt传给xxx 再将输出写入xx2.txt

`>>` 追加不覆盖



`|` pipe符号

`ls -l | tail -n1` 输出ls-l的最后一行

左边命令的输出给右边 最后输出

example: `curl --head --silent baidu.com | grep -i content-length | cut --delimiter=" " -f2` 输出content-length数值



### 终端进阶

#### 信号 SIG

SIGINT `Ctrl+C`

SIGQUIT `Ctrl+\`

SIGTERM `kill -TERM <pid>`

SIGSTOP 

SIGTSTP `Ctrl+Z`

​	recover by `fg/bg`

SIGHUP close all when exiting terminal

`nohup <command>` Allows for a process to live when the terminal gets killed.

`jobs` show all paused jobs

`bg %num` recover [num] job



#### 终端多路复用 tmux

先 `Ctrl+B` 再输入指令

`tmux new -s NAME`

`tmux ls`



`<C-b> c` create

`<C-d>` close

`<C-b> p/n` prev/next

`<C-b> "/%` horizonal/vertical



## 依赖管理

### 版本控制

为构建指定项目需要依赖的版本（或者某个范围的版本）

#### 版本号

版本号常用标准：[语义版本号](https://semver.org/)

`主版本号.次版本号.补丁号`

- 如果新的版本没有改变 API，请将补丁号递增；
- 如果您添加了 API 并且该改动是向后兼容的，请将次版本号递增；
- 如果您修改了 API 但是它并不向后兼容，请将主版本号递增。

Python就是很好的例子，Python2和Python3相互不兼容，Python3.10的代码可能3.7不能运行

#### 锁文件 lockfile

列出了当前每个依赖所对应的具体版本号（或者大于某个版本号`>=`之类）

或者也可以将依赖直接拉取到项目中（*vendoring*）

### 持续集成系统 CI

当代码变动时，自动运行的一系列过程。

市面上有如 Travis CI、Azure Pipelines 、 GitHub Actions等工具。

在代码仓库中添加一个文件，描述当前仓库发生任何修改时，应该如何应对，如：如果有人提交代码，执行测试

`Github Pages`为例，每次 `master` 更新时，执行博客应用重新构建渲染。

## Shell命令记录

通配符 ? *

`touch {a,b,c}.txt` 创建a.txt b.txt c.txt

结合通配符 `mv *{.py,.c,.sh} folder`

`touch {foo,bar}/{a..h}` 在foo和bar两个文件夹里依次创建a b c ... g h 这些文件



`fd` find替代

`locate`



`history` 历史命令

​	`ctrl+R` 回溯搜索命令



`chsh` 更改默认shell

`xargs -d 'delimiter'`

`j` autojump

`tar -cvf -czf`



python自带httpserver

`python3 -m http.server`



`objdump -d {filename}`

反汇编文件输出



`tar -tf xxx.zip/tar`

预览压缩文件内容（不解压）

`rar`



`alias short_name="FULL COMMAND"`

`ctrl+L` 清屏
