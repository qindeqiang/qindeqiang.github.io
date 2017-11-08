---
author: Deqiang Qin
title: CentOS7下Scrapy的安装流程
featimg: 2.jpg
category: [standard]
---

### 写在前面
>关于Scrapy爬虫框架，官网给出了在Windows下和在Ubuntu下的安装方法，但是没有具体给出在CentOS下的安装流程。具体参考地址如下：
`https://scrapy.org/`
![](/img/centos7/scrapy.png "Scrapy Logo")

#### 步骤一
> 安装最新的工具，指定版本号，具体命令如下：
`sudo pip install setuptools==33.1.1`


#### 步骤二
> 安装Doc工具，可以到官网去下载最新的docutils工具
>下载docutils-0.13.1.tar.gz，解压后进入目录下，安装命令如下：
```sudo python setup.py install```


#### 步骤三
>下载Twisted-13.1.0.tar.bz2，解压后安装；安装命令同步骤二；


#### 步骤四
> 开始安装scrapy，此时的scrapy是最新版本的了。具体命令如下：
```sudo pip install scrapy```

#### 步骤五
> 另外：中间有可能需要的安装组件如下：
```yum install -y python-devel openssl-devel libxslt-devel libxml2-devel ```
