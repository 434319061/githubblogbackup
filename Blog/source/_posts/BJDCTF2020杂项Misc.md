---
title: [BJDCTF2020]杂项Misc
date: 2020-10-02 18:19:54
tags: Misc
---

## 认真你就输了

xls文件打开乱码 看下文件类型

16进制下发现时 压缩包

每个都看看

![](认真你就输了.png)

## 一叶障目

![](一叶障目.png)

```python
import zlib
import struct
#读文件
file = '1.png'  #注意，1.png图片要和脚本在同一个文件夹下哦~
fr = open(file,'rb').read()
data = bytearray(fr[12:29])
crc32key = eval(str(fr[29:33]).replace('\\x','').replace("b'",'0x').replace("'",''))
#crc32key = 0xCBD6DF8A #补上0x，copy hex value
#data = bytearray(b'\x49\x48\x44\x52\x00\x00\x01\xF4\x00\x00\x01\xF1\x08\x06\x00\x00\x00')  #hex下copy grep hex
n = 4095 #理论上0xffffffff,但考虑到屏幕实际，0x0fff就差不多了
for w in range(n):#高和宽一起爆破
    width = bytearray(struct.pack('>i', w))#q为8字节，i为4字节，h为2字节
    for h in range(n):
        height = bytearray(struct.pack('>i', h))
        for x in range(4):
            data[x+4] = width[x]
            data[x+8] = height[x]
            #print(data)
        crc32result = zlib.crc32(data)
        if crc32result == crc32key:
            print(width,height)
            #写文件
            newpic = bytearray(fr)
            for x in range(4):
                newpic[x+16] = width[x]
                newpic[x+20] = height[x]
            fw = open(file+'.png','wb')#保存副本
            fw.write(newpic)
            fw.close
```

crc修复脚本

## 藏藏藏

binwalk分离 解压出word文档 

文档里有个qr码 扫描得到

## 你猜我是个啥

查看文件头 png

图片二维码 扫出来不是

![](你猜我是个啥.png)

佛了

## 鸡你太美

在副本添加文件头

![](鸡你太美.png)

