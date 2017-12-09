---
title:      "Apache Phoenix 常用指令"
subtitle:   "Apache Phoenix Shell Commands "
author:     "Deqiang Qin"
featimg: 4.jpg
tags: [Bigdata]
published: true
categories: [Phoenix]
header-img: "img/post-bg-05.jpg"
---
通过SqlLine.py的方式可以启动SQLine。Apache Phoenix就是内嵌SQLine作为命令行的交互工具。<br>
SQLine 是一个Java编写的控制台工具，主要用于链接关系型数据库并执行SQL命令。和Mysql的和Mysql的mysql,Oracle的sqlplus类似。<br>
以下就是ApachePhoenix下SqlLine的具体使用。<br>

### 1.启动
`./sqline.py <hbase.zookeeper.quorum>`<br>
其中，hbase.zookeeper.quorum是HBase使用的集群地址，以逗号分隔开。如下：
```
[bigdata@localhost bin]$ ./sqlline.py 192.168.122.1:2181
Setting property: [incremental, false]
Setting property: [isolation, TRANSACTION_READ_COMMITTED]
issuing: !connect jdbc:phoenix:192.168.122.1:2181 none none org.apache.phoenix.jdbc.PhoenixDriver
Connecting to jdbc:phoenix:192.168.122.1:2181
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/home/bigdata/apps/apache-phoenix-4.13.0-HBase-1.3-bin/phoenix-4.13.0-HBase-1.3-client.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/usr/local/hadoop-2.6.5/share/hadoop/common/lib/slf4j-log4j12-1.7.5.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
17/12/09 02:52:29 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Connected to: Phoenix (version 4.13)
Driver: PhoenixEmbeddedDriver (version 4.13)
Autocommit status: true
Transaction isolation: TRANSACTION_READ_COMMITTED
Building list of tables and columns for tab-completion (set fastconnect to true to skip)...
95/95 (100%) Done
Done
sqlline version 1.2.0
0: jdbc:phoenix:192.168.122.1:2181>
```
### 2.显示所有表
使用!tables 命令能够显示所有表。<br>
注意，此时只显示通过Phoenix客户端创建的表，通过其他方式创建的表不能显示。

```
0: jdbc:phoenix:192.168.122.1:2181> !tables
+------------+--------------+---------------+---------------+----------+------------+----------------------------+-----------------+--------------+-----------------+---------------+---------------+-------------+
| TABLE_CAT  | TABLE_SCHEM  |  TABLE_NAME   |  TABLE_TYPE   | REMARKS  | TYPE_NAME  | SELF_REFERENCING_COL_NAME  | REF_GENERATION  | INDEX_STATE  | IMMUTABLE_ROWS  | SALT_BUCKETS  | MULTI_TENANT  | VIEW_STATEM |
+------------+--------------+---------------+---------------+----------+------------+----------------------------+-----------------+--------------+-----------------+---------------+---------------+-------------+
|            | SYSTEM       | CATALOG       | SYSTEM TABLE  |          |            |                            |                 |              | false           | null          | false         |             |
|            | SYSTEM       | FUNCTION      | SYSTEM TABLE  |          |            |                            |                 |              | false           | null          | false         |             |
|            | SYSTEM       | SEQUENCE      | SYSTEM TABLE  |          |            |                            |                 |              | false           | null          | false         |             |
|            | SYSTEM       | STATS         | SYSTEM TABLE  |          |            |                            |                 |              | false           | null          | false         |             |
|            |              | USER_TEMP_CF  | TABLE         |          |            |                            |                 |              | false           | null          | false         |             |
+------------+--------------+---------------+---------------+----------+------------+----------------------------+-----------------+--------------+-----------------+---------------+---------------+-------------+
0: jdbc:phoenix:192.168.122.1:2181>
```

### 3.创建表
```
0: jdbc:phoenix:192.168.122.1> CREATE TABLE user_temp (id varchar PRIMARY KEY,account varchar ,passwd varchar);
No rows affected (1.542 seconds)
```
### 4.使用命令行加载数据
Apache Phoenix提供了官方的样本数据，可以通过命令行加载。<br>
如下所是：
```
[bigdata@localhost bin]$ ./psql.py 192.168.122.1:2181 ../examples/WEB_STAT.sql ../examples/WEB_STAT.csv ../examples/WEB_STAT_QUERIES.sql
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/home/bigdata/apps/apache-phoenix-4.13.0-HBase-1.3-bin/phoenix-4.13.0-HBase-1.3-client.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/usr/local/hadoop-2.6.5/share/hadoop/common/lib/slf4j-log4j12-1.7.5.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
17/12/09 04:12:51 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
no rows upserted
Time: 1.584 sec(s)

csv columns from database.
CSV Upsert complete. 39 rows upserted
Time: 0.368 sec(s)

DOMAIN                                                          AVERAGE_CPU_USAGE                         AVERAGE_DB_USAGE
---------------------------------------- ---------------------------------------- ----------------------------------------
Salesforce.com                                                            260.727                                  257.636
Google.com                                                                212.875                                   213.75
Apple.com                                                                 114.111                                  119.556
Time: 0.173 sec(s)

DAY                                              TOTAL_CPU_USAGE                            MIN_CPU_USAGE                            MAX_CPU_USAGE
----------------------- ---------------------------------------- ---------------------------------------- ----------------------------------------
2013-01-01 00:00:00.000                                       35                                       35                                       35
2013-01-02 00:00:00.000                                      150                                       25                                      125
2013-01-03 00:00:00.000                                       88                                       88                                       88
2013-01-04 00:00:00.000                                       26                                        3                                       23
2013-01-05 00:00:00.000                                      550                                       75                                      475
2013-01-06 00:00:00.000                                       12                                       12                                       12
2013-01-08 00:00:00.000                                      345                                      345                                      345
2013-01-09 00:00:00.000                                      390                                       35                                      355
2013-01-10 00:00:00.000                                      345                                      345                                      345
2013-01-11 00:00:00.000                                      335                                      335                                      335
2013-01-12 00:00:00.000                                        5                                        5                                        5
2013-01-13 00:00:00.000                                      355                                      355                                      355
2013-01-14 00:00:00.000                                        5                                        5                                        5
2013-01-15 00:00:00.000                                      720                                       65                                      655
2013-01-16 00:00:00.000                                      785                                      785                                      785
2013-01-17 00:00:00.000                                     1590                                      355                                     1235
Time: 0.12 sec(s)

HO                    TOTAL_ACTIVE_VISITORS
-- ----------------------------------------
EU                                      150
NA                                        1
Time: 0.156 sec(s)
```
其中，<br>
WEB_STAT.sql  是建表语句；<br>
WEB_STAT.csv 是数据文件；<br>
WEB_STAT_QUERIES.sql 是查询语句；<br>
```
-- WEB_STAT_QUERIES.sql
SELECT DOMAIN, AVG(CORE) Average_CPU_Usage, AVG(DB) Average_DB_Usage
FROM WEB_STAT
GROUP BY DOMAIN
ORDER BY DOMAIN DESC;

-- Sum, Min and Max CPU usage by Salesforce grouped by day
SELECT TRUNC(DATE,'DAY') DAY, SUM(CORE) TOTAL_CPU_Usage, MIN(CORE) MIN_CPU_Usage, MAX(CORE) MAX_CPU_Usage
FROM WEB_STAT
WHERE DOMAIN LIKE 'Salesforce%'
GROUP BY TRUNC(DATE,'DAY');

-- list host and total active users when core CPU usage is 10X greater than DB usage
SELECT HOST, SUM(ACTIVE_VISITOR) TOTAL_ACTIVE_VISITORS
FROM WEB_STAT
WHERE DB > (CORE * 10)
GROUP BY HOST;
```
