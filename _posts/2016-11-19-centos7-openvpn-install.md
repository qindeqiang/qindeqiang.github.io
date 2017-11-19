---
title:      "基于CentOS7的OpenVPN客户端的安装流程"
subtitle:   "The way to install openvpn in Centos7"
author:     "Deqiang Qin"
featimg: 4.jpg
tags: [OpenVPN]
published: true
categories: [Linux]
header-img: "img/Linux-bg-00.jpg"
---

本案例是结合谢公羁VPN服务说明的。<br>
#### 步骤一：安装
```
sudo yum install -y openvpn
```

#### 步骤二：配置
```
将服务器上的配置文件*.ovpn的文件拷贝到任意位置
```

#### 步骤三：前台启动
```
执行指令：openvpn --config *.ovpn
所以，此时会提示输入用户名和密码。

```

#### 步骤四：后台启动
```
为了避免输入用户名和密码，可以新建一个文件，auth_info.conf，写成两行。
第一行是用户名，第二行是密码。
这样就可以在启动的时候不用输入密码和用户名。可以变成后台服务一样的启动。
具体启动指令如下：<br>
nohup sudo openvpn --config client.ovpn --auth-user-pass auth_info.conf &
```
<br>
