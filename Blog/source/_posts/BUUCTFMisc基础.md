---
title: BUUCTFMisc基础
date: 2020-09-27 19:38:20
tags: Misc
---

## **金三胖**

gif 使用fireworks 或其他 图像浏览软件 逐帧查看 

## **二维码**

qr码扫下 没有意义 在十六进制编辑器中发现有文件

使用 binwalk分离 4number.txt 和一个压缩包

压缩包 4位数字爆破 获得

## **N种方法解决**

base64图片 是一个二维码 还原 data:image/jpg base64

## **大白**

图片明显修改过 宽高度被篡改

使用TweakPNG

> Warning：
>
> Incorrect crc for IHDR chunk (is 6d7c7135, should be 8e14dfcf)

使用十六进制编辑器修改

回到图片 发现

## **基础破解**

> 你一个压缩包，你并不能获得什么，因为他是四位数字加密的哈哈哈哈哈哈哈。。。不对==我说了什么了不得的东
>  西。。

四位数字 直接爆破 得到base64解密

## **你竟然赶我走**

十六进制的最后发现了

## **LSB**

stegsolve解析

![](LSB.png)

上面感觉有东西

使用 zsteg 解密一下 无

stegsolve 发现红黄蓝有字

![](LSB2.png)

生成图片 扫描获得

## **乌镇峰会种图**

十六进制的最后

## **ningen**

binwalk分离

压缩包爆破四位数字 工具ziperello

获得

## **rar**

爆破4位

## **qr**

扫码

## **文件中的秘密**

十六进制的中间 发现

## **镜子里面的世界**

用stegsloves 分析

![](镜子里面的世界.png)

## **小明的保险箱**

binwalk分离 四位密码爆破

## **爱因斯坦**

binwalk 图片备注中有 压缩包密码

## **假如给我三天光明**

解盲文  :(   kmdonowg

听摩斯电码 网上有摩斯电码的音频解析网站 当时是一点一点看的:(

## **wireshark**

使用wireshark 找到那条http login流量中的数据包

## **easycap**

追踪 TCP流

## **被嗅探的流量**

追踪 TCP流 流量包中发现

## **FLAG**

stegsloves 

![](FLAG.png)

文件头是 zip

里面一个二进制文件

使用kali strings

![](FLAG2.png)

## **另一个世界**

十六进制最后一串二进制 转ascii码 得到 koekj3s

## **荷兰宽带数据泄露**

下载附件得到一个conf.bin文件，路由器信息数据，一般包含账号密码。题目并没有提示flag是什么，猜测是账号或者密码加上格式为最终flag。

用RouterPassView查看后，搜索username或者password，最后发现是用户名

## **来首歌吧**

右耳道摩斯密码

## **隐藏的钥匙**

十六进制内

## **后门查杀**

include下发现include.php为webshell文件

![](后门查杀.png)

![](后门查杀2.png)

## **梅花香自苦寒来**

## **snake**

binwalk 分离 将key中字符串解密

![](snake.png)

Serpent 有蛇的意思在西方神话是一条巨大的蛇型恶魔

anaconda

## **神秘龙卷风**

压缩包 四位数字爆破 Brainfuck和Ook加解密

https://tool.bugku.com/brainfuck/

## **面具下的 flag**

Brainfuck和Ook加解密

## **webshell后门**

![](webshell后门.png)

## **被劫持的神秘礼物**

小明收到了一件很特别的礼物，有奇怪的后缀，奇怪的名字和格式。小明找到了知心姐姐度娘，度娘好像知道这是啥，但是度娘也不知道里面是啥。。。你帮帮小明？找到帐号密码，串在一起，用32位小写MD5哈希一下得到的就是答案。

wireshark http数据

![](被劫持的神秘礼物.png)

拼接用户名密码得到adminaadminb

使用md5加密后得到1d240aafe21a86afc11f38a45b541a49

## **刷新过的图片**

F5隐写

![](刷新过的图片.png)

kali中 java Extract Misc.jpg

解密出一个伪加密的压缩包

## **穿越时空的思念**

摩斯电码 

## **秘密文件**

binwalk分离文件 爆破密码

## **我吃三明治**

一张图片 

使用foremost 解出两张图片

看wp说是 在两张图片连接处 有类似与base32的字符串

![](我吃三明治.png)

