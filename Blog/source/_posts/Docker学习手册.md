---
title: Docker学习手册
date: 2020-10-11 16:35:54
tags: "Docker"
---

# Docker的出现

## 问题出现

产品：

​	开发	->	上线 

 应用环境	应用配置

​		  两套环境

开发----运维

问题：

我的电脑上可以运行，版本更新，导致服务不可用！对于运维来说考验大，导致**开发即运维**。

每个机器都要部署环境(集群Readis、Es、Hadoop)费时费力

发布一个项目 （Redis mysql jdk es）项目

不能都带上环境安装打包

服务器配置环境麻烦 不能跨平台

windows 最后发布到 linux！

> 传统：开发 jar 运维来做
>
> 现在：**开发打包部署上线，一套流程做完**

## 解决问题

> java--apk--发布（应用商店）--使用apk--安装即可
>
> java--jar（环境）--打包项目带上环境（镜像）--Docker仓库：商店--下载我们发布的镜像--直接运行即可

Docker以上问题提出了解决方案

Docker思想就来源于集装箱

JRE--多个应用（端口冲突）--原来都是交叉的

Docker思想

> 打包 互相隔离

# Docker的历史

2010年 Dotcloud做一些 pass的云计算技术，LXC有关的容器技术 Docker 但是刚刚诞生没有引起注意 

## **开源**

2013年 将Docker开源 然后大家发现了docker的优点

每个月都会更新一个版本

2014年4月9日 1.0发布

Docker 为什么这么火？

## **轻巧**

在容器技术出来之前 都是使用虚拟机技术

在window中装一个vmware 通过这个软件我们可以虚拟出一台或者多台电脑 

> 虚拟机也是属于虚拟化技术
>
> Docker容器技术 本质都是 虚拟化技术

> Vm：Linux Centos 原生镜像 一个电脑  隔离需要开启多个虚拟机
>
> Docker：隔离 镜像最核心的环境 + jdk+mysql+.....  运行镜像即可

docker 是基于go语言开发的 开源项目

官方文档Dockerdocs地址：https://docs.docker.com/get-started/

官方仓库Dockerhub地址：https://www.docker.com/products/docker-hub

# Docker的功能

### 之前的虚拟机技术

![](image-20201011164456507.png)

缺点：

- 资源占用多
- 冗余步骤多
- 启动很慢

### 容器化技术

不是模拟一个完整的操作系统

![](image-20201011164555477.png)

不同处：

- 传统虚拟机，虚拟出硬件，运行一个完整的操作系统，然后再这个系统上安装和运行软件
- 容器内的应用直接运行在宿主机的内容，容器是没有自己的内核的，也没有虚拟我们的硬件所以就轻便了
- 每个容器的都是相互隔离，每个容器内都有一个属于自己的文件系统，互不影响

 

**应用更快速的交付和部署**

> 传统：一堆帮助文档，安装程序
>
> Docker：打包镜像发布测试，一键运行

**更便捷的升级和扩容缩容**

> 使用docker后 部署应用像搭积木

# Docker的名称概念

![Docker的基本组成](image-20201011164717715.png)

**镜像 image**： 

> docker镜像好比模板，可以通过模板来创建容器服务
>
> tomcat 镜像 ->run->tomcat01 容器（提供服务器）
>
> 通过这个镜像可以创建多个容器（最终服务运行或项目运行就是在容器中）

**容器 container**：

> 利用容器技术，独立运行一个或者一组应用，通过镜像来创建：
>
> 启动 停止 删除 基本命令
>
> **可以理解成一个简单的linux系统**

**仓库repository**：

> 存放镜像的地方
>
> 分为 公有 私有 
>
> Dockerhub 阿里云 等 

# 购买配置阿里云服务器

### 1.开放端口

配置安全组规则

![](image-20201011164938165.png)

### 2.配置服务器

获取服务器的公网ip地址 修改实例名称和密码 第一次修改需要重启 使用xshell连接

连接服务器之后 需要搭建环境

1.傻瓜式：宝塔

安装教学 

https://www.bt.cn/download/linux.html

ssh工具连接 直接一键安装脚本

下载之后 就获得宝塔管理地址

开启端口 

放入网站进行访问

2.命令式：手动

# 安装Docker

## 环境准备：

1.linux基础

2.cetos7 

3.远程服务器连接

环境查看：

内核版本

```shell
[root@izbp1jcqwirs6gl7qxh61fz ~]# uname -r
3.10.0-514.26.2.el7.x86_64
```

保证系统内核3.0以上

系统版本

```shell
[root@izbp1jcqwirs6gl7qxh61fz ~]# cat /etc/os-release 
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"s
```

安装帮助文档 https://docs.docker.com/get-started/overview/

### **卸载 旧版本** 

https://docs.docker.com/engine/install/centos/

## **安装**

```shell
yum install -y yum-utils
```

**设置镜像仓库**  https://download.docker.com/linux/centos/docker-ce.repo

```
yum-config-manager \
   --add-repo \
```

```
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

**安装docker 相关内容**

```
yum install docker-ce docker-ce-cli containerd.io
```

更新yum软件包索引

```
yum makecache fast
```

## **启动docker**

```
sudo systemctl start docker
```

使用 **docker version** 查看是否安装成果

```
docker run hello-world
```

```
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker run hello-world

Unable to find image 'hello-world:latest' locally

latest: Pulling from library/hello-world

0e03bdcc26d7: Pull complete 

Digest: sha256:4cf9c47f86df71d48364001ede3a4fcd85ae80ce02ebad74156906caff5378bc

Status: Downloaded newer image for hello-world:latest 

Hello from Docker!

This message shows that your installation appears to be working correctly.


To generate this message, Docker took the following steps:

 \1. The Docker client contacted the Docker daemon.

 \2. The Docker daemon pulled the "hello-world" image from the Docker Hub.

  (amd64)

 \3. The Docker daemon created a new container from that image which runs the

  executable that produces the output you are currently reading.

 \4. The Docker daemon streamed that output to the Docker client, which sent it

  to your terminal.
To try something more ambitious, you can run an Ubuntu container with:

 $ docker run -it ubuntu bash
Share images, automate workflows, and more with a free Docker ID:

 https://hub.docker.com/
For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

没有寻找到镜像 然后拉取官方镜像

查看一下这个的hello-world 镜像

```
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker images

REPOSITORY     TAG         IMAGE ID      CREATED       SIZE

hello-world     latest       bf756fb1ae65    9 months ago    13.3kB
```

以下了解：

## 卸载docker

### 卸载依赖

```
sudo yum remove docker-ce docker-ce-cli containerd.io
```

### 卸载资源

```
sudo rm -rf /var/lib/docker
```

/var/lib/docker  为默认工作路径

 

# 配置阿里云镜像加速

![](456 .png)

然后找到自己的镜像加速地址

![](image-20201011165909580.png)

```shell
#配置使用
[root@izbp1jcqwirs6gl7qxh61fz ~]# sudo mkdir -p /etc/docker
[root@izbp1jcqwirs6gl7qxh61fz ~]# sudo tee /etc/docker/daemon.json <<-'EOF'
> {
>   "registry-mirrors": ["https://自己的id.mirror.aliyuncs.com"]
> }
> EOF
{
  "registry-mirrors": ["https://自己的id.mirror.aliyuncs.com"]
}
[root@izbp1jcqwirs6gl7qxh61fz ~]# sudo systemctl daemon-reload.
Unknown operation 'daemon-reload.'.
[root@izbp1jcqwirs6gl7qxh61fz ~]# sudo systemctl daemon-reload
[root@izbp1jcqwirs6gl7qxh61fz ~]# sudo systemctl restart docker
```

# 回顾hello-world流程

![](456123.png)

底层原理

Docker 是什么工作的？ 

> client-sercer 结构 docker 的守护进程在主机上 使用通过 socket 从客户端访问
>
> docker server 接收到 docker-client 的指令 就会执行这个命令

docker 为什么比vm快？

> docker 有着比虚拟机更少的抽象层
>
> docker 利用的是宿主机的内核 vm需要是 Duest os

![](image-20201011170127198.png)

所以说 新建一个容器的时候

docker 不要像虚拟机一样重新加载一个操作系统内核，避免引导

虚拟机是加载guest os ，分钟级别

docker 是利用 宿主机的操作系统吗，省略了这个复杂的过程，秒级

# Docker的常用命令

## 帮助命令

```shell
docker version //显示 docker 的版本信息

docker info //显示docker 的系统信息 包括镜像和容器的数量

docker 命令 --help //帮助命令
```

帮助文档 官网 https://docs.docker.com/compose/reference/overview/

# 镜像命令

```
docker image 
```

```
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker images
REPOSITORY     TAG         IMAGE ID      CREATED       SIZE
hello-world     latest       bf756fb1ae65    9 months ago    13.3kB
镜像的仓库源   	镜像的标签       id          创建时间    大小
```

```
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker images --help
Usage:    docker images [OPTIONS] [REPOSITORY[:TAG]]
List images
Options:
 -a, --all       Show all images (default hides
​            intermediate images)
   --digests     Show digests
 -f, --filter filter  Filter output based on conditions provided
   --format string  Pretty-print images using a Go template
   --no-trunc    Don't truncate output
 -q, --quiet      Only show numeric IDs
-a, --all  列出所有镜像
-q, --quiet  只显示id 
```

## 搜索镜像

```
docker search
```

或者 

 dockerhub搜索

```
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker search mysql
NAME                              DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   10031               [OK]                
mariadb                           MariaDB is a community-developed fork of MyS…   3677                [OK]                
mysql/mysql-server                Optimized MySQL Server Docker images. Create…   735                                     [OK]
percona                           Percona Server is a fork of the MySQL relati…   511                 [OK]                
centos/mysql-57-centos7           MySQL 5.7 SQL database server                   83                                      
mysql/mysql-cluster               Experimental MySQL Cluster Docker images. Cr…   77                                      
centurylink/mysql                 Image containing mysql. Optimized to be link…   61                                      [OK]
bitnami/mysql                     Bitnami MySQL Docker Image                      45                                      [OK]
deitch/mysql-backup               REPLACED! Please use http://hub.docker.com/r…   41                                      [OK]
tutum/mysql                       Base docker image to run a MySQL database se…   35                                      
prom/mysqld-exporter                                                              31                                      [OK]
schickling/mysql-backup-s3        Backup MySQL to S3 (supports periodic backup…   30                                      [OK]
databack/mysql-backup             Back up mysql databases to... anywhere!         30                                      
linuxserver/mysql                 A Mysql container, brought to you by LinuxSe…   26                                      
centos/mysql-56-centos7           MySQL 5.6 SQL database server                   20                                      
circleci/mysql                    MySQL is a widely used, open-source relation…   19                                      
mysql/mysql-router                MySQL Router provides transparent routing be…   16                                      
arey/mysql-client                 Run a MySQL client from a docker container      15                                      [OK]
fradelg/mysql-cron-backup         MySQL/MariaDB database backup using cron tas…   9                                       [OK]
openshift/mysql-55-centos7        DEPRECATED: A Centos7 based MySQL v5.5 image…   6                                       
devilbox/mysql                    Retagged MySQL, MariaDB and PerconaDB offici…   3                                       
ansibleplaybookbundle/mysql-apb   An APB which deploys RHSCL MySQL                2                                       [OK]
jelastic/mysql                    An image of the MySQL database server mainta…   1                                       
widdpim/mysql-client              Dockerized MySQL Client (5.7) including Curl…   1                                       [OK]
monasca/mysql-init                A minimal decoupled init container for mysql    0                                       
```

可选项 通过收藏来过滤 
--filter = STARS=3000       //搜索出来的镜像都是stars 大于3000的

## 下载镜像

```
docker pull
```

```
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker pull mysql
Using default tag: latest #不写tag 默认是latest
latest: Pulling from library/mysql
d121f8d1c412: Pull complete    #分层下载 docker image 联合文件系统
f3cebc0b4691: Pull complete 
1862755a0b37: Pull complete 
489b44f3dbb4: Pull complete 
690874f836db: Pull complete 
baa8be383ffb: Pull complete 
55356608b4ac: Pull complete 
dd35ceccb6eb: Pull complete 
429b35712b19: Pull complete 
162d8291095c: Pull complete 
5e500ef7181b: Pull complete 
af7528e958b6: Pull complete 
Digest: sha256:e1bfe11693ed2052cb3b4e5fa356c65381129e87e38551c6cd6ec532ebe0e808 #签名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest #真实地址

docker pull mysql = docker pull docker.io/library/mysql:latest
```

### 指定版本下载

```
docker pull mysql:5.7
```

前提 要确保官方镜像仓库有

![](image-20201011170838886.png)

```
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker pull mysql:5.7
5.7: Pulling from library/mysql
d121f8d1c412: Already exists  #之前有的 docker 是不会下载的 节约内存
f3cebc0b4691: Already exists  
1862755a0b37: Already exists 
489b44f3dbb4: Already exists 
690874f836db: Already exists 
baa8be383ffb: Already exists 
55356608b4ac: Already exists 
277d8f888368: Pull complete 
21f2da6feb67: Pull complete 
2c98f818bcb9: Pull complete 
031b0a770162: Pull complete 
Digest: sha256:14fd47ec8724954b63d1a236d2299b8da25c9bbb8eacc739bb88038d82da4919
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7
```

![](213132.png)

## 删除镜像

```
docker rmi 
```

```
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
mysql               5.7                 ef08065b0a30        4 weeks ago         448MB
mysql               latest              e1d7dc9731da        4 weeks ago         544MB
hello-world         latest              bf756fb1ae65        9 months ago        13.3kB
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker rmi -f ef08065b0a30
Untagged: mysql:5.7
Untagged: mysql@sha256:14fd47ec8724954b63d1a236d2299b8da25c9bbb8eacc739bb88038d82da4919
Deleted: sha256:ef08065b0a302111b56966aa92c89fa0bacdfc537741cbca88a15b10f14332ca
Deleted: sha256:c8c81ac92392c394197759ca3d50f5f843d85ac1550d8c0bb2b21adc7334100d
Deleted: sha256:2dc86a1b9b92e7c946c684bd349e448d7c4fbb3236686e1a48ddfe5adb86a425
Deleted: sha256:97b541df82456d38e987b630870fcd4e39f05f016717652466b3466841f4162e
Deleted: sha256:aded9a11fc54761c770a9075cfc2d0bb72c72b59171a56cfa4322ab2b2d416e7
```

只删除了5.7

### 删除所有镜像

> docker rmi -f 容器id 		#删除指定镜像
> docker rmi -f 容器id 容器id ..		#删除多个镜像
> docker rmi -f $(docker images -aq) 		#删除全部镜像

![](021121.png)

# 容器命令

说明：我们有了镜像才能创建容器，下载一个centos 镜像来测试学习

```
docker pull centos
```

```
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker pull centos
Using default tag: latest
latest: Pulling from library/centos
3c72a8ed6814: Pull complete 
Digest: sha256:76d24f3ba3317fa945743bb3746fbaf3a0b752f10b10376960de01da70685fbd
Status: Downloaded newer image for centos:latest
docker.io/library/centos:latest
```

## 新建容器并启动

docker run [可选参数] image

> 参数说明：
>
> --name="Name" 容器名字 用来区分容器
>
> -d           后台方式运行
>
> -it              使用交互方式运行，进入容器查看内容
>
> -p              指定容器的端口 -p 8080：8080
>
> -p ip：
>
> -p 主机端口：容器端口（常用）
>
> -p 容器端口
>
> 容器端口

```
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker run -it centos /bin/bash
```

## 启动并进入容器

```
[root@09d1160b9016 /]# ls
bin  etc   lib	  lost+found  mnt  proc  run   srv  tmp  var
dev  home  lib64  media       opt  root  sbin  sys  usr
```

但是镜像容器内部很多基础命令不存在

## 列出所有运行的容器

```
docker ps
```

> -a #列出当前正在运行的容器，并带出历史运行过的容器
> -n=？ #显示最近创建的容器
> -q    #静默模式，只显示容器的编号

```
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                        PORTS               NAMES
09d1160b9016        centos              "/bin/bash"         2 minutes ago       Exited (130) 19 seconds ago                       recursing_snyder
018691cc819b        centos              "/bin/bash"         3 minutes ago       Exited (0) 3 minutes ago                          quirky_bohr
cf3753d48bb7        bf756fb1ae65        "/hello"            2 days ago          Exited (0) 2 days ago                             vibrant_leakey
```

## 退出容器

> exit 			#直接容器停止并退出
> ctrl+p+q 			#容器不停止并退出

## 删除容器

> docker rm 容器id        #删除指定 容器 ，不能删除正在运行的
> docker rm -f  $(docker ps -aq) 	#删除所有容器

## 启动和停止容器操作

> docker start 容器id  	 #启动容器
> docker restart  容器id 	#重启容器
> docker stop 容器id 	#停止当前正在运行的容器
> docker kill 容器id 	#强制停止

# 常用的其他命令

```
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker run -d centos
6c925620b5a7185bc249a7e8f582156f6dd555072b08c9a647aa62c80c6c17b4
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
497de629275f        centos              "/bin/bash"         3 hours ago         Up 3 hours                              vigorous_yalow
```

发现 centos 停止了 

**常见问题：**

**docker 容器使用后台运行 就必须要有一个前台进程**

**docker 发现没有应用 就会自动停止**

容器启动后，发现自己没有提供服务，就会立刻停止 就是没有程序了

## 查看日志

```
docker logs
```

```
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker logs --help
Usage:	docker logs [OPTIONS] CONTAINER
Fetch the logs of a container
Options:
      --details        Show extra details provided to logs
  -f, --follow         Follow log output
      --since string   Show logs since timestamp (e.g. 2013-01-02T13:23:37) or relative (e.g. 42m for 42
                       minutes)
      --tail string    Number of lines to show from the end of the logs (default "all")
  -t, --timestamps     Show timestamps
      --until string   Show logs before a timestamp (e.g. 2013-01-02T13:23:37) or relative (e.g. 42m for 42
                       minutes)
```

```
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker logs -f -t --tail 10 f746f0150673
[root@f746f0150673 /]# 
```

发现**容器里面没有日志**

自己编写一段shell脚本

while ture:do echo 123;sleep 1;done;

```
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker logs -f -t --tail 10  e47b50d3e04a
2020-10-11T07:30:04.979506693Z /bin/sh: ture: command not found
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker run -d centos /bin/sh -c "while true;do echo qwe;sleep 1;done"
7c395ab86e1ec6011e3c932b11f5dea14d1491860442550715a27a8dfba58a7f
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
7c395ab86e1e        centos              "/bin/sh -c 'while t…"   4 seconds ago       Up 3 seconds                            eloquent_lalande
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker logs -f -t --tail 10 7c395ab86e1e 或者docker logs -tf  --tail  10 7c395ab86e1e
2020-10-11T07:33:44.419265878Z qwe
2020-10-11T07:33:45.421200298Z qwe
```

> \#显示日志
>
> -tf 显示日志
>
> --tail number 要显示的日志条数

## 查看容器中的进程信息

```
docker top 容器id
```

```
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
7c395ab86e1e        centos              "/bin/sh -c 'while t…"   3 minutes ago       Up 3 minutes                            eloquent_lalande
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker top 7c395ab86e1e
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                21553               21528               0                   15:33               ?                   00:00:00            /bin/sh -c while true;do echo qwe;sleep 1;done
root                22097               21553               0                   15:37               ?                   00:00:00            /usr/bin/coreutils --coreutils-prog-shebang=sleep /usr/bin/sleep 1
```

pid 当前进程id

```
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker inspect 7c395ab86e1e
[
    {
        "Id": "7c395ab86e1ec6011e3c932b11f5dea14d1491860442550715a27a8dfba58a7f",
        "Created": "2020-10-11T07:33:21.988535877Z",
        "Path": "/bin/sh",
        "Args": [
            "-c",
            "while true;do echo qwe;sleep 1;done"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 21553,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2020-10-11T07:33:22.377254638Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:0d120b6ccaa8c5e149176798b3501d4dd1885f961922497cd0abef155c869566",
        "ResolvConfPath": "/var/lib/docker/containers/7c395ab86e1ec6011e3c932b11f5dea14d1491860442550715a27a8dfba58a7f/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/7c395ab86e1ec6011e3c932b11f5dea14d1491860442550715a27a8dfba58a7f/hostname",
        "HostsPath": "/var/lib/docker/containers/7c395ab86e1ec6011e3c932b11f5dea14d1491860442550715a27a8dfba58a7f/hosts",
        "LogPath": "/var/lib/docker/containers/7c395ab86e1ec6011e3c932b11f5dea14d1491860442550715a27a8dfba58a7f/7c395ab86e1ec6011e3c932b11f5dea14d1491860442550715a27a8dfba58a7f-json.log",
        "Name": "/eloquent_lalande",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "Capabilities": null,
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/0028bf4e077878cffcbb723eaf632fa499a62e3bcd9a3118e50b3bc980df08df-init/diff:/var/lib/docker/overlay2/f1ea0ba31bfdf0bb951b4e5016c72d5648bc4dc92348e2d6353c96f20716b38e/diff",
                "MergedDir": "/var/lib/docker/overlay2/0028bf4e077878cffcbb723eaf632fa499a62e3bcd9a3118e50b3bc980df08df/merged",
                "UpperDir": "/var/lib/docker/overlay2/0028bf4e077878cffcbb723eaf632fa499a62e3bcd9a3118e50b3bc980df08df/diff",
                "WorkDir": "/var/lib/docker/overlay2/0028bf4e077878cffcbb723eaf632fa499a62e3bcd9a3118e50b3bc980df08df/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "7c395ab86e1e",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "while true;do echo qwe;sleep 1;done"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20200809",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "e5f501113e8502f618f6f73d544d314984c34a1caada1cd09a534a740074cc6c",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/e5f501113e85",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "3ca3d7f3644f8d9a05e03d2705e062fdb15aa19e990c27605e0b59f2586db36a",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "97b2ebc629b345fa1387e54a959b09355e5f2ed4282c420163d7fd185be81794",
                    "EndpointID": "3ca3d7f3644f8d9a05e03d2705e062fdb15aa19e990c27605e0b59f2586db36a",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
[root@izbp1jcqwirs6gl7qxh61fz ~]# 

[root@izbp1jcqwirs6gl7qxh61fz ~]# 
[root@izbp1jcqwirs6gl7qxh61fz ~]# 
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker inspect 7c395ab86e1e
[
    {
        "Id": "7c395ab86e1ec6011e3c932b11f5dea14d1491860442550715a27a8dfba58a7f",
        "Created": "2020-10-11T07:33:21.988535877Z",
        "Path": "/bin/sh",
        "Args": [
            "-c",
            "while true;do echo qwe;sleep 1;done"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 21553,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2020-10-11T07:33:22.377254638Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:0d120b6ccaa8c5e149176798b3501d4dd1885f961922497cd0abef155c869566",
        "ResolvConfPath": "/var/lib/docker/containers/7c395ab86e1ec6011e3c932b11f5dea14d1491860442550715a27a8dfba58a7f/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/7c395ab86e1ec6011e3c932b11f5dea14d1491860442550715a27a8dfba58a7f/hostname",
        "HostsPath": "/var/lib/docker/containers/7c395ab86e1ec6011e3c932b11f5dea14d1491860442550715a27a8dfba58a7f/hosts",
        "LogPath": "/var/lib/docker/containers/7c395ab86e1ec6011e3c932b11f5dea14d1491860442550715a27a8dfba58a7f/7c395ab86e1ec6011e3c932b11f5dea14d1491860442550715a27a8dfba58a7f-json.log",
        "Name": "/eloquent_lalande",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "Capabilities": null,
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/0028bf4e077878cffcbb723eaf632fa499a62e3bcd9a3118e50b3bc980df08df-init/diff:/var/lib/docker/overlay2/f1ea0ba31bfdf0bb951b4e5016c72d5648bc4dc92348e2d6353c96f20716b38e/diff",
                "MergedDir": "/var/lib/docker/overlay2/0028bf4e077878cffcbb723eaf632fa499a62e3bcd9a3118e50b3bc980df08df/merged",
                "UpperDir": "/var/lib/docker/overlay2/0028bf4e077878cffcbb723eaf632fa499a62e3bcd9a3118e50b3bc980df08df/diff",
                "WorkDir": "/var/lib/docker/overlay2/0028bf4e077878cffcbb723eaf632fa499a62e3bcd9a3118e50b3bc980df08df/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "7c395ab86e1e",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "while true;do echo qwe;sleep 1;done"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20200809",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "e5f501113e8502f618f6f73d544d314984c34a1caada1cd09a534a740074cc6c",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/e5f501113e85",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "3ca3d7f3644f8d9a05e03d2705e062fdb15aa19e990c27605e0b59f2586db36a",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "97b2ebc629b345fa1387e54a959b09355e5f2ed4282c420163d7fd185be81794",
                    "EndpointID": "3ca3d7f3644f8d9a05e03d2705e062fdb15aa19e990c27605e0b59f2586db36a",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
```

# 进入容器的命令和拷贝命令

**通常容器使用后台方式运行 需要进入容器 修改配置**

```
docker exec -it 容器id bashshell
```

```
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
7c395ab86e1e        centos              "/bin/sh -c 'while t…"   11 minutes ago      Up 11 minutes                           eloquent_lalande
[root@izbp1jcqwirs6gl7qxh61fz ~]# docker exec -it 7c395ab86e1e /bin/bash
[root@7c395ab86e1e /]# 
```

或者

```
docker attach 容器id
```

[root@izbp1jcqwirs6gl7qxh61fz ~]# docker attach 7c395ab86e1e

qwe

qwe #正在执行当前代码

> exec ->  进入容器开启新的终端，可以在里面操作（常用）
> attach -> 进入容器正在执行的终端，不会启动新的进程

## 从容器内拷贝到主机上

```
docker cp 容器id:容器内路径 目的主机
```

```
#容器内新建文件
[root@42bce5d815bd /]# cd /home 
[root@42bce5d815bd home]# ls
[root@42bce5d815bd home]# ls 
[root@42bce5d815bd home]# touch fdc
[root@42bce5d815bd home]# ls
fdc
#将其拷贝到主机上来
[root@izbp1jcqwirs6gl7qxh61fz home]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
bb984feb0c25        centos              "/bin/bash"         2 minutes ago       Up 2 minutes                            vigorous_poincare
[root@izbp1jcqwirs6gl7qxh61fz home]# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
42bce5d815bd        centos              "/bin/bash"         3 minutes ago       Exited (0) 27 seconds ago                       quizzical_curie
bb984feb0c25        centos              "/bin/bash"         3 minutes ago       Up 3 minutes                                    vigorous_poincare
[root@izbp1jcqwirs6gl7qxh61fz home]# docker cp 42bce5d815bd:/home/fdc /home
[root@izbp1jcqwirs6gl7qxh61fz home]# ls
fdc  www
```

\#拷贝是以一个手动过程，未来我们使用 -v卷的技术 可以实现

# 小结

![](image-20201011172216960.png)