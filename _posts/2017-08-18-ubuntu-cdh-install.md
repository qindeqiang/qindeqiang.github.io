---
author: Deqiang Qin
title: Ubuntu下离线安装CDH
featimg: 4.jpg
---

## 裸机离线安装CDH

### 操作系统准备工作
```
1 安装操作系统
2 配置主机名及IP地址
3 配置/etc/hosts
4 建立一个集群的用户
5 配置所有主机的集群用户免密码sudo权限
6 指定namenode主机
7 配置namenode间，namenode到node主机免密码ssh信任关系
8 配置/etc/apt/sources
```
### 软件准备
```
1 所有主机安装jdk
2 所有主机安装ntp服务，指定一个ntp server,其余主机作为ntp client 同步到ntp server
3 安装mysql
4 安装基础软件
```

### cdh准备

```

cloudera-manager-trusty-cm5.6.0_amd64.tar.gz
CDH-5.6.0-1.cdh5.6.0.p0.45-trusty.parcel     
CDH-5.6.0-1.cdh5.6.0.p0.45-trusty.parcel.sha1 ->CDH-5.6.0-1.cdh5.6.0.p0.45-trusty.parcel.sha  
manifest.json
```

### 安装

#### 软件安装（所有主机）

```shell
sudo apt-get install mysql-client libmysqlclient-dev libmysql-java
```

#### mysql服务器（单独mysql节点或namenode节点）

```shell
sudo apt-get install mysql-server-5.6

sed -i "s/bind-address/#bind-address/g" /etc/mysql/my.cnf

sudo service mysql restart
```

#### 执行sql

```sql
--hive数据库
create database hive DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
--集群监控数据库
create database amon DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
--hue数据库
create database hue DEFAULT CHARSET utf8 COLLATE utf8_general_ci;

grant all on hive.* to 'hive'@"%" Identified by "hivepassword";

grant all on amon.* to 'amon'@"%" Identified by "amonpassword";

grant all on hue.* to 'hue'@"%" Identified by "huepassword";

grant all on cmf.* to 'cmf'@'%' identified by 'cmfpassword';

#sql end
```

#### manager节点安装

```shell

sudo mkdir -p /opt/cloudera-manager
cp cloudera-manager-trusty-cm5.6.0_amd64.tar.gz /opt/cloudera-manager
cd /opt/cloudera-manager
tar -zxvf cloudera-manager-trusty-cm5.6.0_amd64.tar.gz

sudo mkdir -p /opt/cloudera/parcel-repo
cp manifest.json /opt/cloudera/parcel-repo
cp CDH-5.6.0-1.cdh5.6.0.p0.45-trusty.parcel  /opt/cloudera/parcel-repo
cp CDH-5.6.0-1.cdh5.6.0.p0.45-trusty.parcel.sha1 /opt/cloudera/parcel-repo/CDH-5.6.0-1.cdh5.6.0.p0.45-trusty.parcel.sha  

#在脚本第一行后添加export JAVA_HOME=/usr/local/java

sed -i '1aexport JAVA_HOME=/usr/local/java' /opt/cloudera-manager/cm-5.6.0/share/cmf/schema/scm_prepare_database.sh
sed -i '1aexport JAVA_HOME=/usr/local/java'  /opt/cloudera-manager/cm-5.6.0/etc/init.d/cloudera-scm-server
sed -i '1aexport JAVA_HOME=/usr/local/java'  /opt/cloudera-manager/cm-5.6.0/etc/init.d/cloudera-scm-agent

#修改client的server指向,如：指向namenode3则修改为namenode3
sed -i 's/server_host=localhost/server_host=namenode3/g'  /opt/cloudera-manager/cm-5.6.0/etc/cloudera-scm-agent/config.ini

#创建cmf库，注意scm-host，如：制定scm-host为namenode3
sudo /opt/cloudera-manager/cm-5.6.0/share/cmf/schema/scm_prepare_database.sh mysql -uroot -ppassword --scm-host namenode3 cmf cmf cmfpassword

#用户及权限
sudo useradd --system --home=/opt/cloudera-manager/cm-5.6.0/run/cloudera-scm-server --no-create-home --shell=/bin/false --comment "Cloudera SCM User" cloudera-scm
sudo chown -R cloudera-scm:cloudera-scm /opt/cloudera-manager
sudo chown -R cloudera-scm:cloudera-scm /opt/cloudera


#开机启动

sed -i "/exit/isudo /opt/cloudera-manager/cm-5.6.0/etc/init.d/cloudera-scm-server start" /etc/rc.local
sed -i "/exit/isudo /opt/cloudera-manager/cm-5.6.0/etc/init.d/cloudera-scm-agent start" /etc/rc.local
```

#### 分发到其他节点

```shell

#每个需要分发的节点执行
sudo chmod  777 /opt

#manager上执行
scp -P 51668 -r /opt/cloudera-manager nodeX:/opt

#添加到开机启动
sed -i "/exit/asudo /opt/cloudera-manager/cm-5.6.0/etc/init.d/cloudera-scm-agent start" /etc/rc.local

#用户及权限
sudo useradd --system --home=/opt/cloudera-manager/cm-5.6.0/run/cloudera-scm-server --no-create-home --shell=/bin/false --comment "Cloudera SCM User" cloudera-scm
sudo chown -R cloudera-scm:cloudera-scm /opt/cloudera-manager

```

#### 启动进程

```shell

#manager

sudo /opt/cloudera-manager/cm-5.6.0/etc/init.d/cloudera-scm-server start
sudo /opt/cloudera-manager/cm-5.6.0/etc/init.d/cloudera-scm-agent start

#其他节点
sudo /opt/cloudera-manager/cm-5.6.0/etc/init.d/cloudera-scm-agent start

```

### 图形界面操作

```
http://manager:7180/cmf
```


---

### 安装HIVE

copyMySQL驱动到`/opt/cloudera/parcels/CDH-5.6.0-1.cdh5.6.0.p0.45/lib/hive/lib`目录。

```
sudo cp /opt/cloudera/parcels/CDH-5.6.0-1.cdh5.6.0.p0.45/lib/hive/lib/mysql-connector-java-5.1.31.jar /var/lib/impala
```
$$E=mc^2$$ is a inline formula

$$ 
\begin{aligned} \dot{x} &= \sigma(y-x) \\ 
\dot{y} &= \rho x - y - xz \\ 
\dot{z} &= -\beta z + xy \end{aligned} 
$$
