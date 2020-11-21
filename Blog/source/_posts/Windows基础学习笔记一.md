---
title: Windows基础学习笔记(一)
date: 2020-05-12 21:05:40
tags: Http
---

# Windows基础学习笔记(一)

## C盘根目录常见文件夹

- **Documents and Settings/用户**： Windows7中的“用户”文件夹其实就是XP中的 Documents andSettings文件夹，这里储存了用户的设置，包括用户文档、上网浏览信息、配置文件等数据。

- **Downloads**：这通常是下载软件的默认下载路径，
  建议修改软件设置改到其他硬盘分区。

- **Drivers**：部分驱动程序的文件夹。在etc中，用文本打开其中的hosts，其中的资料主要用来解析域名。

- Favorites：收藏夹。

- **Program Files**：应用程序文件夹，一般软件默认都安装在这里，也有一些系统自带的应用程序。Windows7系统中，64位用户会多出一个 ProgramFies（X86）文件夹，这是系统中32位软件的安装目录  

  >  Common Files：共用程序文件夹，用于同系列不同程序软件共同使用或调用数据，比如微软和Adob的各种软件。
  >
  >  ComPlus Applications：微软COM+组件使用的文件夹，删除后可能引起COM+组件不能运行。
  >  DFX：DFX是一种高效的XML索引方法，此文件夹与数据索引有关，不可以删除。  

- **Program Data**： Windows7的系统文件夹，放置程序的使用数据、设置等文件，不建议删除。Windows：存放操作系统主要文件，非常重要。

- Temp：存放系统或其他软件临时文件，需经常清理

- hiberfil.sys：系统的休眠功能所占用的硬盘空间的文件，不建议删除。

- pagefile.sys：虛拟内存页面文件，不建议删除。

- PerfLogs： Windows vista或 Windows7系统日志，记录如磁盘扫描错误等信息，没有必要删除。  

- Internet Explorer：系统自带的浏览器，删除后可能导致部分程序不能正常运行。

- Install Shield Installation Information：专门存储安装程序信息的文件夹，用于某些程序的卸载和更新。

- MSECache： MS Office运行时自动创建。

- MSXML60：微软ⅩML解析器程序的文件夹。  

- microsoft frontpage： FrontPage是微软推出的款网页设计软件，此文件夹通常为空。

- Movie maker：系统自带的一款视频编辑软件。

- MSN Gaming Zone：一些系统自带的扑克牌等小游戏，可以删除。

- Netmeeting：系统自带的网上聊天软件。

- Outlook Express： Outlook Express WIndows内置的邮件收发端，不用可删。  

- Online Services：网络服务文件夹，不能删。

- Windows Media Player：系统自带的一款多媒体播放器。

- WinRAR：一款流行的压缩解压缩软件。

- Xerox：用作自带的一些图像处理软件的临时空间，文件夹不能删除，但通常为空。  

### **system32** --保存系统的配置文件

> System\config下有一个SAM文件，这个文件就是用来保存用户的账号和密码的
>
> 对于win10系统来说，进到这个目录，首先就需要管理员权限，其次就算你进来了，这种东西都是一个被锁定的文件，在系统启动的时候他就已经被系统调动了，如果你想打开这个文件，电脑首先就会显示此文件已经被占用，具体的显示内容为“另一个程序正在使用此文件，进程无法访问。假设现在我们需要尝试渗透一个系统的一块软件数据，那么我们首先就要登陆进这个系统，打开软件才能拷贝数据，而且进去还不能靠改他的密码，这种解决方法就是，首先把SAM拷贝出来一份，然后用PE工具把他清掉，再登录系统的时候就可以不需要密码了，然后该做什么做什么，做完了以后重新进入PE，把之前我们备份的SAM再放进去。其实PE本身已经足够我们直接进入系统，但我们要知道，如果没有最终以用户的形式进入那个系统的话，我们就算找到了那个软件，那个软件本身也是不可读的，所以还是要有这种先将用户密码清除在从用户进入的过程。

## Windows常见服务

<u>Web</u>服务，<u>DNS</u>服务，<u>DHCP</u>服务，<u>邮件</u>服务，<u>Telnet</u>服务，<u>SSH</u>服务，<u>FTP</u>服务，<u>SMB</u>服务

- Web服务

  > Web服务是一种[服务导向架构](https://zh.wikipedia.org/wiki/服務導向架構)的技术，通过标准的[Web](https://zh.wikipedia.org/wiki/Web)协议提供服务，目的是保证不同平台的应用服务可以互操作。根据[W3C](https://zh.wikipedia.org/wiki/W3C)的定义，**Web服务**（Web service）应当是一个[软件](https://zh.wikipedia.org/wiki/软件)系统，用以支持[网络](https://zh.wikipedia.org/wiki/网络)间不同机器的互动操作。网络服务通常是许多[应用程序接口](https://zh.wikipedia.org/wiki/应用程序接口)（[API](https://zh.wikipedia.org/wiki/API)）所组成的，它们透过网络，例如国际互联网（[Internet](https://zh.wikipedia.org/wiki/Internet)）的远程[服务器](https://zh.wikipedia.org/wiki/服务器)端，执行客户所提交服务的请求。Web service是一个[平台](https://baike.baidu.com/item/平台/1064049)独立的，低耦合的，自包含的、基于可[编程](https://baike.baidu.com/item/编程)的web的应用程序，可使用开放的[XML](https://baike.baidu.com/item/XML)（[标准通用标记语言](https://baike.baidu.com/item/标准通用标记语言/6805073)下的一个子集）[标准](https://baike.baidu.com/item/标准/219665)来[描述](https://baike.baidu.com/item/描述/8928757)、发布、发现、协调和配置这些应用程序，用于开发分布式的互操作的[应用程序](https://baike.baidu.com/item/应用程序/5985445)。

>https://blog.csdn.net/Z_Grant/article/details/90650111

- DNS服务

  IP地址电脑理解不了URL域名，在这种情况下，使用dns服务。

- DHCP服务

**动态主机设置协议**（英语：**Dynamic Host Configuration Protocol****，DHCP**）是一个[局域网](https://baike.baidu.com/item/局域网)的[网络协议](https://baike.baidu.com/item/网络协议)，使用[UDP](https://baike.baidu.com/item/UDP)协议工作，主要有两个用途：用于内部网或网络服务供应商自动分配[IP](https://baike.baidu.com/item/IP)地址；给用户用于内部网管理员作为对所有计算机作中央管理的手段。

- Telnet服务

Telnet协议是[TCP/IP协议](https://baike.baidu.com/item/TCP%2FIP协议)族中的一员，是Internet远程登录服务的标准协议和主要方式。它为用户提供了在本地计算机上完成远程[主机](https://baike.baidu.com/item/主机/455151)工作的能力。在[终端](https://baike.baidu.com/item/终端/1903878)使用者的电脑上使用telnet程序，用它连接到[服务器](https://baike.baidu.com/item/服务器/100571)。[终端](https://baike.baidu.com/item/终端/1903878)使用者可以在telnet程序中输入命令，这些命令会在[服务器](https://baike.baidu.com/item/服务器/100571)上运行，就像直接在服务器的控制台上输入一样。可以在本地就能控制[服务器](https://baike.baidu.com/item/服务器/100571)。要开始一个telnet会话，必须输入用户名和密码来登录[服务器](https://baike.baidu.com/item/服务器/100571)。Telnet是常用的[远程控制](https://baike.baidu.com/item/远程控制/934368)Web[服务器](https://baike.baidu.com/item/服务器)的方法。

-  SSH服务

> 多用于Linux，与telnet类似，SSH 为 [Secure Shell](https://baike.baidu.com/item/Secure Shell) 的缩写，由 IETF 的网络小组（Network Working Group）所制定；SSH 为建立在应用层基础上的安全协议。SSH 是目前较可靠，专为[远程登录](https://baike.baidu.com/item/远程登录/1071998)会话和其他网络服务提供安全性的协议。利用 SSH 协议可以有效防止远程管理过程中的信息泄露问题。SSH最初是UNIX系统上的一个程序，后来又迅速扩展到其他操作平台。SSH在正确使用时可弥补网络中的漏洞。SSH客户端适用于多种平台。几乎所有UNIX平台—包括[HP-UX](https://baike.baidu.com/item/HP-UX)、[Linux](https://baike.baidu.com/item/Linux)、[AIX](https://baike.baidu.com/item/AIX)、[Solaris](https://baike.baidu.com/item/Solaris/3517)、[Digital](https://baike.baidu.com/item/Digital) [UNIX](https://baike.baidu.com/item/UNIX)、[Irix](https://baike.baidu.com/item/Irix)，以及其他平台，都可运行SSH。传统的网络服务程序，如：[ftp](https://baike.baidu.com/item/ftp)、pop和[telnet](https://baike.baidu.com/item/telnet)在本质上都是不安全的，因为它们在网络上用[明文](https://baike.baidu.com/item/明文)传送口令和数据，别有用心的人非常容易就可以截获这些口令和数据。而且，这些服务程序的[安全验证](https://baike.baidu.com/item/安全验证)方式也是有其弱点的， 就是很容易受到“中间人”（man-in-the-middle）这种方式的攻击。所谓“中间人”的攻击方式， 就是“中间人”冒充真正的[服务器](https://baike.baidu.com/item/服务器)接收你传给服务器的数据，然后再冒充你把数据传给真正的服务器。服务器和你之间的数据传送被“中间人”转手做了手脚之后，就会出现很严重的问题。通过使用SSH，你可以把所有传输的数据进行加密，这样"中间人"这种攻击方式就不可能实现了，而且也能够防止DNS欺骗和IP欺骗。使用SSH，还有一个额外的好处就是传输的数据是经过压缩的，所以可以加快传输的[速度](https://baike.baidu.com/item/速度/5456)。SSH有很多功能，它既可以代替[Telnet](https://baike.baidu.com/item/Telnet)，又可以为[FTP](https://baike.baidu.com/item/FTP)、[PoP](https://baike.baidu.com/item/PoP)、甚至为[PPP](https://baike.baidu.com/item/PPP)提供一个安全的"通道"
>
> 从客户端来看，SSH提供两种级别的安全验证。
>
> 首先是基于口令的安全验证，只要你知道自己的帐号和口令，就可以登录到远程主机。所有传输的数据都会被加密，但是不能保证你正在连接的服务器就是你想连接的[服务器](https://baike.baidu.com/item/服务器)。可能会有别的服务器在冒充真正的服务器，也就是受到“中间人”这种方式的攻击。其次是基于密钥的安全验证，这需要依靠[密匙](https://baike.baidu.com/item/密匙)，也就是你必须为自己创建一对密匙，并把公用密匙放在需要访问的服务器上。如果你要连接到SSH服务器上，客户端软件就会向服务器发出请求，请求用你的密匙进行安全验证。服务器收到请求之后，先在该服务器上你的主目录下寻找你的公用密匙，然后把它和你发送过来的公用密匙进行比较。如果两个密匙一致，服务器就用公用密匙加密“质询”（challenge）并把它发送给客户端软件。客户端软件收到“质询”之后就可以用你的私人密匙解密再把它发送给服务器。用这种方式，你必须知道自己密匙的[口令](https://baike.baidu.com/item/口令)。但是，与第一种级别相比，第二种级别不需要在网络上传送口令。
>
> 第二种级别不仅加密所有传送的数据，而且“中间人”这种攻击方式也是不可能的（因为他没有你的私人密匙）。但是整个登录的过程可能需要10秒。

- FTP服务

> 即文件传输协议（[英文](https://baike.baidu.com/item/英文)：**F**ile **T**ransfer **P**rotocol，[缩写](https://baike.baidu.com/item/缩写)：**FTP**）是用于在[网络](https://baike.baidu.com/item/网络)上进行文件传输的一套标准协议，使用客户/服务器模式。它属于[网络传输协议](https://baike.baidu.com/item/网络传输协议)的[应用层](https://baike.baidu.com/item/应用层)。文件传送（file transfer）和文件访问（file access）之间的区别在于：前者由FTP提供，后者由如NFS等应用系统提供
>
> [TCP/IP协议](https://baike.baidu.com/item/TCP%2FIP协议)中，FTP标准命令TCP[端口](https://baike.baidu.com/item/端口)号为21，Port方式数据端口为20。FTP的任务是从一台计算机将文件传送到另一台计算机，不受[操作系统](https://baike.baidu.com/item/操作系统)的限制。
>
> 需要进行远程文件传输的计算机必须安装和运行ftp客户程序。在windows操作系统的安装过程中，通常都安装了tcp/ip协议软件，其中就包含了ftp客户程序。但是该程序是字符界面而不是图形界面，这就必须以[命令提示符](https://baike.baidu.com/item/命令提示符)的方式进行操作，很不方便。