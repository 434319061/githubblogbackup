---
title: 'CTFshow web入门 信息收集'
date: 2020-11-11 20:36:57
tags: web入门
---

其实还都是看大周师傅的wp才会做的 :)

https://www.d1a0.cn/2020/09/03/ctfshow-web%E5%85%A5%E9%97%A8%E5%88%B7%E9%A2%98%E8%AE%B0%E5%BD%95/

## **web1**

题目：开发注释未及时删除

解：右键查看源代码

## **web2**

题目：js前台拦截

解：禁止Javascript 右键查看源代码 或者 ctrl+u 快捷键

## **web3**

题目： 没思路的时候抓个包看看，可能会有意外收获

解题思路： burp抓包，查看响应头，得到flag。

## **web4**

访问robots.txt

得到提示

/flagishere.txt

## **web5**

题目： phps源码泄露有时候能帮上忙

解题思路： 根据提示，访问index.phps将文件下载下来，打开即可看到flag。

## **web6**

题目： 解压源码到当前目录，测试正常，收工

解题思路： 访问www.zip ，下载源代码，查看index.php内容，发现flag在fl000g.txt中，这里注意压缩包里fl000g.txt中的flag不正确，应该访问题目环境中的fl000g.txt 

## **web7**

题目： 版本控制很重要，但不要部署到生产环境更重要。

解题思路： 根据提示版本控制，想到常用的版本控制工具git，svn，尝试访问.git和.svn，在.git中发现flag。

## **web8**

题目： 版本控制很重要，但不要部署到生产环境更重要。

解题思路： 上一题访问.git得到flag，这一题首先想到.svn，果然得到flag。

## **web9**

题目： 发现网页有个错别字？赶紧在生产环境vim改下，不好，死机了

解题思路： 提示vim异常关闭，想到linux下vi/vim异常关闭是会存留.swp文件，尝试访问index.php.swp，得到flag。

## **web10**

题目： cookie 只是一块饼干，不能存放任何隐私数据

解题思路： 查看网页的cookie发现flag。

## **web11**

题目： 域名其实也可以隐藏信息，比如ctfshow.com 就隐藏了一条信息

解题思路： 在域名解析查询网站查询，http://dbcha.com/ ，逐个尝试，在Txt中发现flag。

![](\1.png)

## **web12**

题目： 有时候网站上的公开信息，就是管理员常用密码

解题思路： 访问后台，/admin ，提示登录，猜想用户名为admin，密码应该在网站中，观察到页面底部有“Help Line Number : 372619038”，尝试输入数字，成功登录，得到flag。

## **web13**

题目： 技术文档里面不要出现敏感信息，部署到生产环境后及时修改默认密码

解题思路： 在网站中寻找技术文档（查看源代码寻找较为方便）,在底部找到document，点击即可查看到默认用户名，密码，访问http://.chall.ctf.show/system1103/login.php 。输入用户名和密码，即可得到flag。

## **web14**

题目： 有时候源码里面就能不经意间泄露重要(editor)的信息,默认配置害死

解题思路： 根据提示访问editor，出现文本编辑器，点击图片，

可以看到文件目录，/var/www/html/nothinghere 中有一个fl000g.txt，访问 http://6ce4a2ea-ac6a-4c12-84fa-40a347055991.chall.ctf.show/nothinghere/fl000g.txt 得到flag。

## **web15**

题目： 公开的信息比如邮箱，可能造成信息泄露，产生严重后果

解题思路： 在网站底部发现一个qq邮箱，访问后台，发现可以有忘记密码选项，点击，密保问题是所在地城市，查找qq所在地为西安，输入，返回修改后的密码，登录即可得到flag。

## **web16**

题目： 对于测试用的探针，使用完毕后要及时删除，可能会造成信息泄露

解题思路： 提到探针，就想到雅黑探针，访问/tz.php，点击PHP参数，

点击下图红框中文字

![](2.png)

 

跳转到网站的phpinfo页面，在页面搜索flag，即可找到flag。

（关于php探针的内容可参考https://blog.csdn.net/weixin_43790779/article/details/108834213

）

## **web17**

题目： 透过重重缓存，查找到ctfer.com的真实IP，提交flag{IP地址}

解题思路： [https://icplishi.com](https://icplishi.com/) 查询www.ctfer.com 的IP地址，得到IP地址即为flag。

## **web18**

题目： 不要着急，休息，休息一会儿，玩101分给你flag

解题思路： 查看网页源代码，发现Flappy_js.js文件，访问可看到

将\u4f60\u8d62\u4e86\uff0c\u53bb\u5e7a\u5e7a\u96f6\u70b9\u76ae\u7231\u5403\u76ae\u770b\u770b

进行Unicode解码得到 “你赢了，去幺幺零点皮爱吃皮看看”。访问110.php得到flag。

## **web19**

题目： 密钥什么的，就不要放在前端了

解题思路： 查看页面源代码，发现一段注释代码，代码中已经给出了用户名和密码，但是若有表单提交密码就会被加密，所以用hackbar工具，POST提交username=admin&pazzword=a599ac85a73384ee3219fa684296eaa62667238d608efa81837030bd1ce1bf04 ，得到flag。

## **web20**

题目：

mdb文件是早期asp+access构架的数据库文件，文件泄露相当于数据库被脱裤了。

我是asp程序，我用的access数据库

 

直接查看url路径添加/db/db.mdb

下载之后，txt打开

control+f找flag

这里好像是需要用 sqlmap or 御剑来获取accesss数据库

 



## 小结

- 源代码 前端
- js前台拦截
- 抓包：响应头
- robots.txt
- phps源码泄露     index.phps
- 网站源码 www.zip
- 版本控制工具git     svn  .git .svn
- Linux     vim异常关闭 保留临时文件.swp index.php.swp
- cookie
- 查看域名 http://dbcha.com/
- /admin 后台 
- 网站中找到技术文件document     查看默认账号密码
- 源码泄露editor信息
- 公开信息qq信息 - 密保
- 网站测试探针 雅黑探针 /tz.php     查看php参数 - phpinfo
- 前端出现密钥 hackbar     post提交
- asp+access框架数据库   /db/db.mdb

 

 