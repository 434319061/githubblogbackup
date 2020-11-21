---
title: Win10下 Java环境变量配置
tags: Http
---

# **Win10下 Java环境变量配置**

之前win10更新系统 开机无限月读了 干脆重新配置下 备份自用

首先先下载安装JAVA JDK8

![](1.png)

打开**属性**

![](2.png)

高级系统设置——环境变量

在系统变量这里选择**新建**

![](3.png)

如图添加系统变量

![](4.png)

这里的路径应该为本机路径

继续添加

![](5.png)

> .;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;

在系统变量中修改path

![](6.png)

![](7.png)

添加这两行

> %JAVA_HOME%\bin

> %JAVA_HOME%\jre\bin

win7系统的输入%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;





C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;C:\Windows\System32\OpenSSH\;C:\Program Files\Intel\WiFi\bin\;C:\Program Files\Common Fil
es\Intel\WirelessCommon\;C:\Program Files (x86)\NVIDIA Corporation\PhysX\Common;C:\Program Files\NVIDIA Corporation\NVIDIA NvDLISR;C:\Users\weisu\AppData\Local\Microsoft\WindowsApps