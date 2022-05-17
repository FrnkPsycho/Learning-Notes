# CTF Forensic

## Python处理二进制数据

### struct module

[https://docs.python.org/2/library/struct.html](https://docs.python.org/2/library/struct.html)

`pack()`, `unpack()`, `calcsize()`

`struct.pack('>I',16)`

`>` Big-Endian
`I` Unsigned Integer

BMP Example:

```python
>>> import struct
>>> bmp = '\x42\x4d\x38\x8c\x0a\x00\x00\x00\x00\x00\x36\x00\x00\x00\x28\x00\x00\x00\x80\x02\x00\x00\x68\x01\x00\x00\x01\x00\x18\x00'
>>> struct.unpack('<ccIIIIIIHH',bmp)
('B', 'M', 691256, 0, 54, 40, 640, 360, 1, 24)
```

### bytearray

将文件以二进制数组形式读取

```python
data = bytearray(open('xxx.png','rb').read())
data[0] = '\x89'
```

## 常用工具

`file`, `strings`, `binwalk`

## 图片分析

`strings`, `identify`

### indentify

[https://www.imagemagick.org/script/escape.php](https://www.imagemagick.org/script/escape.php)

### 像素值转化

像素块组成的图片为主

## 图片文件

### PNG

由chunk组成

文件头Signature `89 50 4E 47 0D 0A 1A 0A` + IHDR Chunk + Other Chunks

CRC循环冗余检测 检测是否有错误/篡改

#### IHDR隐写

例如通过IHDR头更改图片的高度或者宽度
然而长宽度不能任意更改，否则图片会显示错误（出现类似扫描线的东西）
需要通过CRC值爆破得到宽度

```python
import os
import binascii
import struct

misc = open("misc4.png","rb").read()

for i in range(1024):
    data = misc[12:16] + struct.pack('>i',i)+ misc[20:29] # 宽度在第17(16)到第19(20)字节
    crc32 = binascii.crc32(data) & 0xffffffff
    if crc32 == 0x932f8a6b:
        print i
```

#### LSB隐写

修改颜色分量最低位，每个像素可以有3bits信息（RGB）

`stegsolve`

### JPG

`0xff` 填充字节 忽略
`0xffd8` `0xffd9` 文件开始结束的标志

`Stegdetect` 通过统计分析技术评估 JPEG 文件的 DCT 频率系数的隐写工具

### GIF

`convert` 将帧分离出来

## 音频

### MP3

`MP3Stego`

### 波形/频谱

### LSB音频隐写

`slienteye`

## 流量包分析

