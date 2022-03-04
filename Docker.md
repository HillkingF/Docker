# Docker

> 学前准备

- linux（常用命令如：cd  mkdir）
- SpringBoot

> 学习内容

基础内容：

- Docker概述
- Docker安装
- Docker命令
  - 镜像命令
  - 容器命令
  - 操作命令
  - ...
- Docker镜像
- 容器数据卷
- DockerFile
- Docker网络原理
- IDEA整合Docker

企业级内容：

- Docker Compose
- Docker Swarm
- CI\CD Jenkins

------

## 一、Docker概述

### 1、Docker为什么出现？

常规来说，一款产品从开发到上线，一般需要两套应用环境。在不同的环境下需要进行不同的应用配置。

开发和运维工作过程中常遇到问题：

- 软件在自己的电脑上可以运行但在别的电脑环境下不能运行。
- 版本更新，导致服务不可用。

这对运维工作来说考验很大。

此外：

- 环境配置十分麻烦，每一台机器都要部署环境（集群、ES、Hadoop），费时费力！
- 例如在服务器配置一个应用环境：Redis MySQL jdk ES Hadoop，配置超麻烦，而且不能跨平台

**因此，能否有一种方案使得项目在发布的时候带上环境安装打包？**

传统开发模式：开发人员开发jar，运维人员来进行部署

现在开发模式：开发人员直接打包部署上线，一套流程做完！

例如：

原来：Java -- apk -- 发布（应用商店） --- 张三使用apk -- 安装即可用

现在：Java -- jar(环境) ---- 打包项目带上环境（镜像） -- （Docker仓库：商店） -- 下载我们发布的镜像，直接运行即可！



**Docker给以上问题，提出了解决方案！**

Docker 的思想就来源于集装箱！他是一种隔离的思想：

将各种应用的环境进行隔离，然后打包装箱，因此每个箱子都是隔离的！

Docker通过隔离机制，可以将服务器利用到极致！



**本质：所有的技术都是因为出现了一些问题需要解决，才去学习！**



### 2、Docker的历史

> 历史介绍

2020年，几个搞IT的年轻人在美国成立了一家公司`dotCloud`。主要做pass的云计算服务！LXC有关的容器技术！他们将自己的容器化技术命名为Docker！Docker刚诞生的时候，没有引起行业的注意！dotCloud就无法生存！



2013年，Docker开源。越来越多的人发现了 docker的优点。docker火了，每个月都会更新一个版本！



2014年4月9日，Docker1.0版本发布！



Docker为什么这么火？因为它十分轻巧！

在容器技术出现之前，开发者都用的虚拟机技术！



对比：

虚拟机：在window中装一个Vmvare，通过这个软件我们可以虚拟一台或者多台电脑。但这样十分笨重！

虚拟机也属于虚拟化技术，Docker容器技术，也是一种虚拟化技术！

```
vm： linux cenntos 原生镜像（一个电脑！） 隔离，需要开启多个 虚拟！通常需要几个G，消耗几分钟开启！
docker：隔离 ，镜像（只包含最核心的环境：4m+jdk+mysql）,十分小巧，运行镜像就可以了！可能只需要几M，KB！秒级启动！
```



到现在，所有的开发人员都需要docker！



> 聊聊Docker

Docker是基于 Go语言开发的！是一个开源项目！

官网：https://www.docker.com/

Docker的文档在官网的最下方：

![docker_docs_place](img/docker_docs_place.png)

文档地址：https://docs.docker.com/

Docker的文档超级详细，好好研读！

Docker远程仓库：https://hub.docker.com/





### 3、Docker的功能

> 之前的虚拟机技术

![虚拟机原理图](img/%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%8E%9F%E7%90%86%E5%9B%BE.png) 

虚拟机技术的缺点：

- 资源占用十分的多
- 冗余步骤多
- 启动很慢



> 容器化技术

**容器化技术不是模拟一个完整的操作系统**

![容器技术原理](img/%E5%AE%B9%E5%99%A8%E6%8A%80%E6%9C%AF%E5%8E%9F%E7%90%86.png)

> 比较虚拟机技术和Docker技术的不同：

- 传统虚拟机，虚拟出一条硬件，运行一个完整的操作系统，然后在这个系统上安装和运行软件
- 同期内的应用直接运行在宿主机内，容器没有自己的内核，也没有虚拟我们的硬件，所以就轻便了
- 每个容器间是相互隔离的，每个容器都有一个属于自己的文件系统，互不影响



> DevOps（开发和运维）

**应用更快速的交付和部署**

传统：一堆帮助文档，安装程序

Docker：打包镜像发布测试，一键运行

**更便捷的升级和扩容缩容**

使用了Docker之后，我们部署应用就和搭积木一样

项目打包为一个镜像，扩展服务器A，服务器B！

**更便捷的运维**

在容器化之后，我们的开发、测试环境都是高度一致的。

**更高效的计算资源利用**

Docker是内核级别的虚拟化，可以在一个物理机上运行很多的容器实例！服务器的性能可以被压榨到极致。











------

## 二、Docker安装

### 1、Docker的基本组成

架构图如下：

![架构图](img/Docker%E6%9E%B6%E6%9E%84.png)

下面先来简单了解以下几个名词。



**镜像(Image):**

docker镜像就好比是一个模板，可以通过这个模板来创建容器服务，例如一个Tomcat镜像===》run===》tomcat01容器（提供服务器），通过这个镜像可以创建多个容器（最终服务运行或者项目运行就是在容器中的）。

**容器(container):**

Docker利用容器技术，独立运行一个或者一个组应用，通过镜像来创建的。

启动，停止，删除，基本命令！

目前就可以把这个容器理解为一个简易的linux系统。

**仓库(repository):**

仓库就是存放镜像的地方！

仓库分为公有仓库和私有仓库！

Docker hub这个仓库默认是国外的。

阿里云...都有容器服务器（配置镜像加速！）





### 2、安装Docker

> 环境准备

1. 需要会一些Linux基础
2. CentOS 7：将Docker安装到云服务器中
3. 使用远程控制软件，连接远程服务器进行操作!  (Xshell在macos不能使用，因此下载FinallShell)

> 环境查看

查看云服务器系统内核版本信息：

```
3.10.0-1160.11.1.el7.x86_64   #系统内核是3.10以上的
```

系统版本：

```
[root@VM-24-12-centos ~]# cat /etc/os-release
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
REDHAT_SUPPORT_PRODUCT_VERSION="7"
```

> 具体安装

将Docker安装到自己的云服务器中，帮助文档中有详细的安装步骤。

![Docker安装文档入口](img/Docker%E5%AE%89%E8%A3%85%E6%96%87%E6%A1%A3%E5%85%A5%E5%8F%A3.png)

![选择对应的系统版本](img/%E9%80%89%E6%8B%A9%E5%AF%B9%E5%BA%94%E7%9A%84%E7%B3%BB%E7%BB%9F%E7%89%88%E6%9C%AC.png)

上面这张图像开始就是正式的安装过程了。根据其说明，步骤总结如下：

1. 首先卸载旧的版本 Uninstall old versions。下面的命令中我们实际操作时不加sudo：

   ```
   sudo yum remove docker \
                     docker-client \
                     docker-client-latest \
                     docker-common \
                     docker-latest \
                     docker-latest-logrotate \
                     docker-logrotat
   ```

2. 然后选择一种方法进行安装。文档中目前有三种安装方法，我们选择其中通过仓库安装的方法：Install using the repository：

   - 首先安装基本环境——仓库：

     ```cmake
     安装仓库需要的安装包：
     (sudo) yum install -y yum-utils
     
     设置仓库的镜像：
     yum-config-manager \
         --add-repo \ https://download.docker.com/linux/centos/docker-ce.repo    # [这个地址是国外的，不推荐]
     
      yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo  #[这个镜像是国内的可以使用，这我们使用这个]
     ```

     ![镜像命令](img/%E9%95%9C%E5%83%8F%E5%91%BD%E4%BB%A4.png)

   - 然后更新yum软件包索引

     ```
     yum makecache fast
     ```

   - 接下来安装docker—— Install Docker Engine：

     ```cmake
     # PS：docker-ce是社区版，ee是企业版。这里我们安装社区版
     yum install docker-ce docker-ce-cli containerd.io
     ```

3. 安装完毕，下面启动docker并测试：

   启动：

   ```cmake
   # 启动docker
   systemctl start docker
   # 查看版本信息
   docker version
   ```

   测试“hello world”：

   ```cmake
   docker run hello-world
   ```

   ![运行效果](img/%E8%BF%90%E8%A1%8C%E6%95%88%E6%9E%9C.png)

   在上面的运行结果图中：

   - 红色的表示运行命令
   - 蓝色的表示没有在本地找到图像，所以去拉取镜像
   - 黄色的表示成功执行的结果（此时就表示docker安装成功了）

   下面验证镜像是否被下载成功？即查看一下下载的hello-world镜像：

   ```
   docker images
   ```

   ![查看结果](img/%E4%B8%8B%E8%BD%BD%E7%9A%84hello-world%E9%95%9C%E5%83%8F.png)



### 3、卸载docker

完全卸载docker需要以下手动操作。

```
# 第一步：卸载依赖
yum remove docker-ce docker-ce-cli containerd.io

# 第二步：卸载资源环境
rm -rf /var/lib/docker
rm -rf /var/lib/containerd
```

上面的命令中：`/var/lib/docker` 是docker的默认工作路径。

