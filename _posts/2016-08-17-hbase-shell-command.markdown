---
title:      "HBase Shell 常用指令"
subtitle:   "HBase Shell Command "
author:     "Deqiang Qin"
featimg: 4.jpg
tags: [Bigdata]
published: true
categories: [HBase]
header-img: "img/1.jpg"
---
# HBase Shell 常用的指令


### 1、对表操作

#### 1.1、创建表
```create 'en_datas',{NAME => 'i', COMPRESSION => 'SNAPPY'}```

+ NAME：指定列族的名称，列族的名称最好简单，另外，HBase表中列族的个数最好不要超过3个；
+ COMPRESSION：指定该列族的压缩格式,常用的是snappy格式；

<br>在HbaseShell中，其对大小写区分非常严格，尤其是Shell自带的关键字；

#### 1.2、统计一个表中的条数
统计Hbase的表的条数，主要是统计rowkey的个数，可以用INTERVAL指定统计的间隔，如果不指定默认是按照每1000条统计一次。但是该方法非常Low，效率非常低。尤其是当表的条数如果达到上千万条的时候，效率下降的就更为明显了。建议通过Hive关联Hbase，在Hive中执行sql实现查询。
```
hbase> count ‘t1′  
hbase> count ‘t1′, INTERVAL => 100000  
hbase> count ‘t1′, CACHE => 1000  
hbase> count ‘t1′, INTERVAL => 10, CACHE => 1000  
```

#### 1.3、清空HBase表中的内容
`truncate 'en_datas'`
<p>该方法的执行流程是先disable该表，然后删除数据，指令和mysql的相同</p>

#### 1.4、描述表
`desc 'tablename'`


### 2、操作表中的内容
查询表中的数据,还是以'en_datas'这张表为例：<br>
####    2.1、浏览表的全部的数据<br>
```scan 'en_datas'```<br>
##### 2.1.1、浏览表的指定条数
```
查询前10条数据
scan 'en_datas',{'LIMIT'=> 10}
```

该方法不是很好，因为在HBase Shell中，它会在控制台上将每一个rowkey下的每一个列族的每一列都是按照一行显示的；对用户来说，当表的数据少的时候还可以看，如果表的数据比较多的时候，就头疼了。
#### 2.2、按照rowkey查询

使用<br>`get htablename,'rowkey'`<br>的格式；
```
hbase(main):009:0> get 'en_datas','10_2006-02-28_1'
COLUMN                                     CELL                                                                                                                        
 i:data                                    timestamp=1509358817028, value=13.8                                                                                         
 i:datetime                                timestamp=1509358817028, value=2006-02-28                                                                                   
 i:idxtype                                 timestamp=1509358817028, value=10                                                                                           
 i:typeid                                  timestamp=1509358817028, value=1                                                                                            
4 row(s) in 1.1550 seconds
```
<br>上述查询语句只能获取一条记录；如果rowkey是模糊的。使用模糊查询，即对rowkey进行模糊查询。
<br>按照rowkey的模糊查询,参考文章：http://blog.csdn.net/qq_27078095/article/details/56482010
```

```

参考文章：
http://debugo.com/hbase-shell-cmds/ <br>
http://blog.csdn.net/andie_guo/article/details/44086389
