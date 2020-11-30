# Docker

> 前置知识：Linux SpringBoot

- Docker概述
- Docker安装
- Docker命令
  - 镜像命令
  - 容器命令
  - 操作命令
  - ...
- **Docker镜像**
- 容器数据卷
- DockerFile 难点多
- Docker网络原理（计算机网络原理基础）
- IDEA整合Docker
- 集群 Docker Compose
- Docker Swarm 集群管理
- CI/CD Jenkins 持续集成



## Docker概述

#### Docker为什么出现？

- 开发，线上两套环境！应用环境，应用配置～
- 开发，运维，环境差异化，为什么我的电脑上可以运行，对运维考验十分大
- 环境配置是十分麻烦的，每个机器都要部署环境（集群Redis，Es，Hadoop）费时费力
- 发布项目 jar （搭配环境需要很长时间）所以 （jar包+环境）进行打包！docker就出现了
- 在服务器配置环境 ，配置超麻烦，不能跨平台
- 传统：开发jar，运维配置环境！
- 现在：开发打包部署上线，一套流程做完～

- java - jar（环境）---打包项目带上环境（镜像）----（Docker仓库）----下载发布的镜像 --直接运行即可

- Docker给以上问题，提出了解决方案

![image-20200514192028694](/Users/kidjaya/Library/Application Support/typora-user-images/image-20200514192028694.png)

- Docker的是想来源于集装箱
- JRE 多个应用（端口冲突）---原来都是交叉的
- 隔离：每个箱子直接互相隔离
- Docker通过隔离机制，可以将服务器利用到极致

### Docker的历史

- 2010年，几个年轻人，成立了公司，美国成立了一家公司 **dotCloud**，做一些pass的云计算服务，LXC有关的容器技术，他们将自己的技术（容器化技术），命名就是Docker！
- 开源：开放源代码～

- 2013年，Docker开源！越来越多人发现Docker的优点，每个月更新一个版本！

- 2014年4月9日，Docker1.0发布！

- Docker为什么火 原因：十分轻巧

- 在容器技术出来之前，我们都是使用虚拟机技术

- 虚拟机：在windows中安装一个Vmware，通过这个软件可以虚拟出来一台或多台电脑，十分笨重！

- 虚拟机也是属于虚拟化技术，Docker容器技术，也是一种虚拟化技术

- ```shell
  vm, linux centos原生镜像（一个电脑） 隔离需要开启多个虚拟机 几个G
  docker, 隔离，镜像（最核心的环境）十分的小巧，运行镜像即可 几个M 最小KB级别  秒级启动
  ```

- 成为了开发人员必备的技能～

### 关于Docker

- Docker是基于Go语言开发的～开源项目

- 文档<https://docs.docker.com/>

- 仓库<https://hub.docker.com/>

- 官网<https://www.docker.com/>

  

### Docker能干嘛

- 之前的虚拟机技术
  ![image-20200514194239547](/Users/kidjaya/Library/Application Support/typora-user-images/image-20200514194239547.png)
  -  缺点

    1. 资源占用十分多

    2. 冗余步骤多

    3. 启动很慢

       

- 容器技术

  **容器技术并不是一个完整的系统**
  ![image-20200514194507643](/Users/kidjaya/Library/Application Support/typora-user-images/image-20200514194507643.png)

- 比较Docker和虚拟机技术的不同
  - 传统虚拟机：虚拟出一条硬件，运行一个完整的操作系统，然后在这个系统安装和运行软件
  - 容器内的应用直接运行在 宿主机的内核，容器没有自己的内核，也没有虚拟硬件，所以就轻便了
  - 每个容器间是互相隔离，每个容器都有一个属于自己的文件系统，互不影响



### DevOps（开发运维）

- **应用更快速的交付和部署**
- 传统：一堆帮助文档，安装程序
- Docker：打包镜像发布测试，一键运行
- **更快捷的升级和扩容**
- 使用Docker之后，我们的部署和搭积木一样
- 项目打包为一个镜像，扩展，服务器A，服务器B
- **更简单的系统运维**
- 在容器化后，我们的开发，测试环境都是高度一致的
- **更高效的计算机资源利用**
- Docker是内核级别的虚拟化，可以在一个物理机上运行很多的容器实例，服务器的性能可以被压榨到极致

##  Docker安装

![image-20200514200227460](/Users/kidjaya/Library/Application Support/typora-user-images/image-20200514200227460.png)

### 仓库（repository）

- 仓库就是存放镜像的地方
- 仓库分为共有仓库和私有仓库
- Docker Hub 默认是国外的
- 阿里云...都有容器服务器（CDN加速）

### 安装Docker

环境准备













