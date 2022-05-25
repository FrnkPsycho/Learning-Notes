## .htaccess

```php
<FilesMatch "file.name">
Sethandler application/... //将符合该文件名的文件以何种形式解析
</FileMatch>
```

## 大小写绕过

如果只检查文件名与后缀，且为黑名单



## 后缀名末尾空格

文件后缀名末尾加上空格，php若没有trim掉空格，会将其认为是有效字符，不属于黑名单

Windows在重命名时末尾空格会被去掉，要用Burp抓包改文件名

前提是服务器是Windows Server！

## 后缀名末尾点

同上。末尾点要是没有去掉也是不同的

## 后缀名末尾::$

同上

