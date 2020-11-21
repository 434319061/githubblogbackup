---
title: '[ASIS 2019]Unicorn shop'
date: 2020-05-19 21:59:25
tags: Web
---

![](image-20200519215130151.png)

发现这里无论输入任何ID和Price都无法购买

当Price 小于10 时 显示Wrong commodity

当Price 大于10 时 显示Only one char（？） allowed！

![](image-20200519215322998.png)

utf8?

https://xz.aliyun.com/t/5402#toc-0

> ### 2.3 数字显示
>
> 一些国家的数字在显示的时候也可能造成问题，例如孟加拉语的0-9是০ ১ ২ ৩ ৪ ৫ ৬ ৭ ৮ ৯，但是这里的৪ (U+09EA) 实际上是数字4。ASIS CTF 2019 的 [Unicorn Shop](https://xz.aliyun.com/t/(https://github.com/hyperreality/ctf-writeups/tree/master/2019-asis)) 也是从Unicode背后的数字角度出发考虑问题。

在这个网站查找Unicode 

https://www.compart.com/en/unicode

![](image-20200519215746992.png)

点开找到大于1337的字符即可

参考链接：

https://www.cnblogs.com/Cl0ud/p/12221360.html

https://shawroot.hatenablog.com/entry/2019/10/29/ASIS_2019-Unicorn_shop