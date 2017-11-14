---
title: Elasticsearch的数据存储模型
subtitle: The Model of Elasticsearch uses to save datas
author: Deqiang Qin
tags: Elasticsearch
categories: Elasticsearch
---

# Elasticsearch的数据存储模型
在使用ES之前需要弄清楚ES的数据存储模型，简单说数据到底是如何在ES保存的，按照一个什么样的模型架构保存的。
<div>如下图是我绘制的一个架构图，并就图中的关键术语进行解释:</div>
![](/img/post-es-data-model.jpg "Elasticsearch Data Model")

## Cluster-集群

1、ES的集群不依赖zookeeper。集群搭建起来非常容易。如果需要提高ES集群的效率，需要单独的优化。<br>

2、需要为每一个ES集群取一个名字，ES的集群的区别就是按照集群的名称进行区分的。

## Node-节点
1、Node是ES集群的节点，一般常规的情况下，ES的节点要求最少是3个。
<br>
2、节点也是按照名称来区分的。
## Index-索引
1、数据保存在ES集群中都是按照索引保存的。<br/>
2、索引对mysql，就相当于一个数据库；<br/>
3、ES集群中可以建立任意多的索引；<br/>
4、索引的名字必须是小写字母；<br/>
## Type-类型
索引中可以定义任意多个类型，类型相当于mysql中的隶属于某个数据库的数据表；<br>
## Document-文档
1、文档是ES中的基础信息单元；
<br>
2、文档是按照JSON的格式存储的；<br/>
3、每一个文档必须指定type；<br/>
## Shards-分片
在创建索引的时候，如果数据量比较大，需要指定分片数。假设单个节点的空间为1TB，如果该数据也是1TB，那么指定一个分片的时候，就会把节点弄爆了。<br>
为了解决上述的问题，ES提供了将索引划分为多个分片的功能。分成多份保存，保存在不同的节点上。<br>
<div>
分片带来的好处：<br>
1、有了分片的功能后，ES就能够支持水平扩展了，当集群的空间不够的时候，直接上机器。<br>
2、分片支持了分布式的并行操作，提高了ES集群的IO；
</div>

## Replicas-复制
为了防止集群的集群中的某个节点突发宕机，ES对每一个索引的每一个分片都做了复制。<br>

一个分片或者说索引可以有多个复制；<br>
分片与复制不会放在一个节点上；<br>
多个复制不会防在同一个节点上；<br>

<div>
复制带来的好处有：<br>
1、提高了集群的高可用性；
<br>
2、复制的分片也是分片，所以同样能够提高ES集群的IO；
</div>

### original/primary-主分片
当ES把一个分片复制了指定份数后，会随机选取一个分片作为主分片-primary shards,也称作原始分片-original shards，其余的就是复制，也称为副分片。<br>


关于ES中的又是如何指定Shards和Replicas,参考官方文档：<br>
[https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-create-index.html](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-create-index.html)

<div align="right"><p></p><p></p><strong>未完，待优化......</strong></div>
