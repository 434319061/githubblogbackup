---
title: '[BJDCTF2020]Misc'
date: 2020-11-21 14:36:55
tags: Misc
---

## **认真你就输了**

execl文件打开乱码 16进制查看为压缩包

解压下来查看html文件

发现提示xl/charts/flag.txt访问

## **一叶障目**

![](yyzm.png) 

crc修复脚本

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

## **藏藏藏**

binwalk分离解压出一个二维码

## **你猜我是个啥**

文件头png 扫二维码 flag不在这

结果是在末尾 服了

![](ncwsgs.png)

## **纳尼**

添加个gif文件头

逐帧查看

Q1RGe3dhbmdfYmFvX3FpYW5nX2lzX3NhZH0=

CTF{wang_bao_qiang_is_sad}

## **鸡你太美**

补上文件头 gif逐帧看

## **just_a_rar**

rar 四位数密码爆破 2016

16进制看到flag