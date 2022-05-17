## 计算机相关编码

[](https://ctf-wiki.org/misc/encode/computer/)

### ASCII编码

十进制 二进制 十六进制
最常用：

- 0-9, 48-57
- A-Z, 65-90
- a-z, 97-122

### BaseXX编码

Base32 Base64

Base64即用64个字符编码 2^6=64 故每6个比特为一个单元
除不尽则用0不足使其可以被3整除，然后再进行编码

**隐写原理**：填充两个'='隐写四位信息，填充一个'='隐写两位信息，如：

```text
IEEtWn==
IC3=
IGFuZCBfIGFzIGFkZGl0aW9uYWwgY2hhcmFjdGVycy5=
```

`n==,3=,5= 解码n,3,5` ==> `39,55,57` == 39取前四位，其他取前两位 ==> `011111101b` ==> `125` ==> `ASCII('}')`

- Base64结尾可能有=号 最多2个
- Base32最多6个

### 霍夫曼编码

### XXencoding

### URL编码

[https://zh.wikipedia.org/wiki/%E7%99%BE%E5%88%86%E5%8F%B7%E7%BC%96%E7%A0%81](https://zh.wikipedia.org/wiki/%E7%99%BE%E5%88%86%E5%8F%B7%E7%BC%96%E7%A0%81)

百分号巨多

### Unicode编码

四种表现形式：

源文本： The

&#x [Hex]: `&#x0054;&#x0068;&#x0065;`

&# [Decimal]: `&#00084;&#00104;&#00101;`

\U [Hex]: `\U0054\U0068\U0065`

\U+ [Hex]: `\U+0054\U+0068\U+0065`

### HTML实体编码

实体名称/实体数

[https://www.w3schools.com/html/html_entities.asp](https://www.w3schools.com/html/html_entities.asp)

# 现实世界编码

## 条形码

识别网站: [https://online-barcode-reader.inliteresearch.com/](https://online-barcode-reader.inliteresearch.com/)

