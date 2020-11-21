---
title: '[BJDCTF 2nd] Misc'
tags: Misc
---

BJDCTF 2nd Misc

虽然有官方wp 但是这次认真做了一会 还是自己写写东西好了

## 最简单的misc

解压下来 发现没有文件头 扔进winhex

发现是张图片但是没有文件头 这里添加文件头 发现是png

![1585019879009](C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585019879009.png)

十六进制转字符串得到flag (Hex转Str)

tips：回看wp才发现我使用的360压缩 可以跳过压缩包的伪加密

![1585020147484](C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585020147484.png)

## A_Beautiful_Picture

​	使用TweakPNG 查询crc![1585020316481](C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585020316481.png)

修改过后还是无效 

跑脚本查询原来crc的高度宽度

```python
import struct
import binascii
import os
 
m = open("1.png","rb").read()
for i in range(1024):
    c = m[12:16] + struct.pack('>i', i) + m[20:29]
    crc = binascii.crc32(c) & 0xffffffff
    if crc == 0xbdf78c92:
        print(i)

```

修改图片高度为1000 就得到flag了

## 小姐姐

图片有**明显错位** 搜索hex 中BJD得到flag	

## EasyBaBa

看Hex

![1585021124329](C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585021124329.png)

里面有文件 用压缩软件打开

![1585021176219](C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585021176219.png)

使用视频播放软件打开

发现视频内有东西一闪而过 逐帧播放 发现四张二维码

使用ps或QR Research 修复扫描二维码

## Real_EasyBaBa

![1585022235393](C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585022235393.png)

查看hex 提示是数字 发现flag

## 圣火昭昭

![1585022309313](C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585022309313.png)

新佛？http://hi.pcmoe.net/buddha.html

 解得 `gemlove`(实际上解出来 gemlovecom 上提时候忘了改了 后来题目告知去掉后三位 com ) 

 由题目描述：“flag 全靠猜”，”**猜**“还被特意加粗，得知是 outguess 隐写 

进linux 使用outguess

` outguess -k gemlove -r a.jpg -t flag.txt `



得到flag

## 