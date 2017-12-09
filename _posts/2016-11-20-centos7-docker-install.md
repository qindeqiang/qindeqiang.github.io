---
title:      "CentOS7.X 中安装Docker"
subtitle:   "Install Docker in CentOS7"
author:     "Deqiang Qin"
featimg: 4.jpg
tags: [CentOS7]
published: true
categories: [Linux]
header-img: "img/post-bg-05.jpg"
---
* 目录
{:toc}

<div align="center">
<img src="/img/post-docker-logo.jpeg" align="center" alt="">
</div>

### 1.安装
```
sudo yum install -y docker
```
### 2.启动服务
```
service docker start
```

### 3.安装CentOS
#### 3.1. 检查已经包含的镜像
```
[bigdata@localhost ~]$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE

```
#### 3.2.下载Centos镜像
```
[bigdata@localhost ~]$ sudo docker pull centos
Using default tag:
Trying to pull repository docker.io/library/centos ...
latest: Pulling from docker.io/library/centos
85432449fd0f: Pull complete
Digest: sha256:3b1a65e9a05f0a77b5e8a698d3359459904c2a354dc3b25ae2e2f5c95f0b3667

```
可以通过3.1的指令查看CentOS的镜像是否安装成功.

#### 3.3.运行一个Docker容器
```
[bigdata@localhost ~]$ sudo docker run -i -t centos /bin/bash
[root@32e3e1fbdd37 /]#
```
可以看到，CentOS 容器已经被启动，并且得到了 bash 提示符。<br>
在 docker 命令中使用 “-i 捕获标准输入输出”和 “-t 分配一个终端或控制台”选项。若要断开与容器的连接，输入 exit。
<br>
查看CentOS的内核：
```
[root@32e3e1fbdd37 /]# cat /etc/redhat-release
CentOS Linux release 7.4.1708 (Core)
[root@32e3e1fbdd37 /]# exit
exit
[bigdata@localhost ~]$
```
