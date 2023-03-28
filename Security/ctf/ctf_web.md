# 简介

SQL 注入
XSS 跨站脚本攻击
RCE 远程命令执行
File Include 文件包含
CSRF 跨站请求伪造
SSRF 服务端请求伪造
File Upload 文件上传
条件竞争
XXE
越权
敏感信息
备份泄露
WAF Web Application Firewall Web应用防护系统

# SQL 注入

-   `user()`：当前数据库用户
-   `database()`：当前数据库名
-   `version()`：当前使用的数据库版本
-   `@@datadir`：数据库存储数据路径
-   `concat()`：联合数据，用于联合两条数据结果。如 `concat(username,0x3a,password)`
-   `group_concat()`：和 `concat()` 类似，如 `group_concat(DISTINCT+user,0x3a,password)`，用于把多条数据一次注入出来
-   `concat_ws()`：用法类似
-   `hex()` 和 `unhex()`：用于 hex 编码解码
-   `load_file()`：以文本方式读取文件，在 Windows 中，路径设置为 `\\`
-   `select xxoo into outfile '路径'`：权限较高时可直接写文件

## Cheat Sheet

### 注释

`--`
`#`
`/**/`
