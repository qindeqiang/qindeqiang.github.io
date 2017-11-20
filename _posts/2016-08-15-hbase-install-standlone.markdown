---
title:      "HBase单机版的安装"
subtitle:   "HBase Install Standalone "
author:     "Deqiang Qin"
featimg: 4.jpg
tags: [Bigdata]
published: true
categories: [HBase]
header-img: "img/post-bg-05.jpg"
---
### HBase 的安装
#### HBase 下载地址
[http://mirrors.gigenet.com/apache/hbase/](http://mirrors.gigenet.com/apache/hbase/)

#### 解压
`sudo tar -zxvf hbase-1.2.0-bin.tar.gz -C /usr/local/`

#### 创建软链接
`sudo ln -s hbase-1.2.0/  hbase`

#### 赋予HBase的安装目录下所有文件bigdata用户权限
`sudo chown -R bigdata:bigdata hbase-1.2.0/`

#### 全局环境变量的配置
```
export HBASE_HOME=/usr/local/hbase
export PATH=$SPARK_HOME/bin:$SPARK_HOME/sbin:$HBASE_HOME/bin:$PATH
source /etc/profile
```


#### 验证是否安装成功：
```
[bigdata@localhost local]$ hbase version
HBase 1.2.0
Source code repository git://asf-dev/home/busbey/projects/hbase revision=25b281972df2f5b15c426c8963cbf77dd853a5ad
Compiled by busbey on Thu Feb 18 23:01:49 CST 2016
From source with checksum bcb25b7506ecf5d62c79d8f7193c829b
[bigdata@localhost local]$
```
### 配置单击运行模式
#### 配置HBase依赖的环境参数
```
[bigdata@localhost conf]$ vi hbase-env.sh
#对hbase-env.sh文件做如下修改：
export JAVA_HOME=/usr/local/jdk     #配置本机的java安装根目录
export HBASE_MANAGES_ZK=true        #配置由hbase自己管理zookeeper，不需要单独的zookeeper。
```
#### 配置HBase运行时的参数
&emsp;&emsp;HBase在启动的时候需要指定数据的存储位置,设置属性HBase.rootdir.<br>
创建数据目录datas.
```
[bigdata@localhost hbase]$ mkdir datas
[bigdata@localhost hbase]$ ll
total 292
drwxr-xr-x.  4 bigdata bigdata   4096 Jan 29  2016 bin
-rw-r--r--.  1 bigdata bigdata 105820 Feb 18  2016 CHANGES.txt
drwxr-xr-x.  2 bigdata bigdata    178 Nov 20 02:10 conf
drwxrwxr-x.  2 bigdata bigdata      6 Nov 20 02:18 datas
drwxr-xr-x. 12 bigdata bigdata   4096 Feb 18  2016 docs
drwxr-xr-x.  7 bigdata bigdata     80 Feb 18  2016 hbase-webapps
-rw-rw-r--.  1 bigdata bigdata    261 Feb 19  2016 LEGAL
drwxr-xr-x.  3 bigdata bigdata   8192 Nov 20 01:50 lib
-rw-rw-r--.  1 bigdata bigdata 131265 Feb 19  2016 LICENSE.txt
-rw-rw-r--.  1 bigdata bigdata  27636 Feb 19  2016 NOTICE.txt
-rw-r--r--.  1 bigdata bigdata   1477 Dec 26  2015 README.txt
```
配置/conf/hbase-site.xml
```
<configuration>
    <property>
        <name>hbase.rootdir</name>
        <value>file:///usr/local/hbase/datas/</value>
    </property>
</configuration>
```
### 启动/停止HBase
```
[bigdata@localhost ~]$ start-hbase.sh
starting master, logging to /usr/local/hbase/logs/hbase-bigdata-master-localhost.localdomain.out
Java HotSpot(TM) 64-Bit Server VM warning: ignoring option PermSize=128m; support was removed in 8.0
Java HotSpot(TM) 64-Bit Server VM warning: ignoring option MaxPermSize=128m; support was removed in 8.0
[bigdata@localhost ~]$ jps
11591 Jps
6424 NailgunRunner
6347 RemoteMavenServer
6220 Main
11277 HMaster
[bigdata@localhost ~]$ stop-hbase.sh

```

### 进入Hbase Shell的模式
```
[bigdata@localhost ~]$ hbase shell
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/usr/local/hbase-1.2.0/lib/slf4j-log4j12-1.7.5.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/usr/local/hadoop-2.6.5/share/hadoop/common/lib/slf4j-log4j12-1.7.5.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
HBase Shell; enter 'help<RETURN>' for list of supported commands.
Type "exit<RETURN>" to leave the HBase Shell
Version 1.2.0, r25b281972df2f5b15c426c8963cbf77dd853a5ad, Thu Feb 18 23:01:49 CST 2016

1.8.7-p357 :001 > list
TABLE                                                                                                                                                                                                              
0 row(s) in 0.3590 seconds

 => []
1.8.7-p357 :002 >
```
