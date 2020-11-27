---
title: Docker Introduction
date: 2020-10-30 17:54:08
tags: tools
categories: tools 
---

### Docker 简介

Docker 是一个开源，轻量级的应用容器引擎，基于GO语言开发，用于创建、管理和编排容器。Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。**Docker 属于 Linux 容器的一种封装，提供简单易用的容器使用接口。**它是目前最流行的 Linux 容器解决方案。Docker 将应用程序与该程序的依赖，打包在一个文件里面。运行这个文件，就会生成一个虚拟容器。程序在这个虚拟容器里运行，就好像在真实的物理机上运行一样。有了 Docker，就不用担心环境问题。Docker 的接口相当简单，用户可以方便地创建和使用容器，把自己的应用放入容器。容器还可以进行版本管理、复制、分享、修改，就像管理普通的代码一样。

<!-- more -->

1. **虚拟机和容器的区别与联系**

   * 虚拟机需要有额外的虚拟机管理应用和虚拟机操作系统层，操作系统层不仅占用空间而且运行速度也相对慢

   * Docker容器是在本机操作系统层面上实现虚拟化，因此很轻量，速度接近原生系统速度

   * 传统的虚拟机需要模拟整台机器包括硬件，每台虚拟机都需要有自己的操作系统，虚拟机一旦被开启，预分配给他的资源将全部被占用。每一个虚拟机包括应用，必要的二进制和库，以及一个完整的用户操作系统

   * 容器技术和我们的宿主机共享硬件资源及操作系统，可以实现资源的动态分配。容器包含应用和其所有的依赖包，但是与其他容器共享内核。容器在宿主机操作系统中，在用户空间以分离的进程运行。容器内没有自己的内核，也没有进行硬件虚拟。

     ![](https://raw.githubusercontent.com/ichbinhandsome/images/main/68edf099da1ec2b442759b136d97d95.png)

     ![](https://raw.githubusercontent.com/ichbinhandsome/images/main/245f0b186036d5814ad0ca0a599ead4.png)

     ![](https://raw.githubusercontent.com/ichbinhandsome/images/main/67d348ec911d8f6ccab45f3183a76c7.png)

     ![](https://raw.githubusercontent.com/ichbinhandsome/images/main/62eed5796896de9224f843390e64784.jpg)

     ![](https://raw.githubusercontent.com/ichbinhandsome/images/main/bf26e3f822828ad852ddb96857d84b6.jpg)

     

2. **Docker 的应用场景和特点**

   * Build, Ship and Run
   * Build once，Run anywhere

3. **Docker 的基本概念**

   - **镜像**：Docker 镜像是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。镜像不包含任何动态数据，其内容在构建之后也不会被改变；	

     镜像（类）= 文件系统+数据，可以用开发语言中的类比作镜像，对象比作容器

   - **容器**：容器的实质是进程，但与直接在宿主执行的进程不同，容器进程运行于属于自己的独立的命名空间容器可以被。创建、启动、停止、删除和暂停等等，说到镜像与容器之间的关系，可以类比面向对象程序设计中的类和实例；

     容器是镜像的运行实例，可以使用同一个镜像运行多个实例。

     *从读写角度来说，镜像是只读的，容器是在镜像上添加了一层可读写的文件系统*

   - **仓库**：镜像构建完成后，可以很容易的在当前宿主机上运行，但是，如果需要在其它服务器上使用这个镜像，我们就需要一个集中的存储、分发镜像的服务，Docker Registry 就是这样的服务。一个 Docker Registry 中可以包含多个仓库；每个仓库可以包含多个标签；每个标签对应一个镜像，其中标签可以理解为镜像的版本号

4. **Docker 的常用命令**

   * `docker pull 【镜像名】` 从docker-hub 中拉取镜像

   * `docker search image_name` - 从 Docker Hub仓库中检索镜像

   * `docker run 【镜像名】` 运行镜像

   * `docker image ls `, `docker container ls`  获取镜像， 容器列表

   * `exit` 从正在运行的容器中退出

   * `docker ps ` 查看所有正在运行的容器

   * `docker container rm [container ID]` 删除容器

   * docker 端口映射，开启 bash 进程 `-it `

   * 制作自己的 docker 容器， 使用 dockerfile ：

     1. 编写 Dockerfile 文件

     2. `docker image build -t [文件名]` 创建image 文件

     3. `docker container run`命令从image 文件生成容器

     4. 镜像文件的发布 参考：http://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html

     5. [commit 命令](https://hijiangtao.github.io/2018/04/17/Docker-in-Action/)

        [从 docker 到 K8S](https://www.qikqiak.com/k8s-book/)

