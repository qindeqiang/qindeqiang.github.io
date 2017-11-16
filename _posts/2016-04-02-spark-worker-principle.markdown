---
title:      "Spark的运行原理"
subtitle:   "The principles of Spark"
author:     "Deqiang Qin"
featimg: 4.jpg
tags: [Spark,Bigdata]
published: true
categories: [Spark]
header-img: "img/1.jpg"
---
![](/img/post-spark-principles.png)
使用spark-submit提交一个Spark作业之后，这个作业就会启动一个对应的Driver进程。根据你使用的部署模式（deploy-mode）不同，Driver进程可能在本地启动，也可能在集群中某个工作节点上启动。Driver进程本身会根据我们设置的参数，占有一定数量的内存和CPU core。而Driver进程要做的第一件事情，就是向集群管理器如Yarn申请运行Spark作业需要使用的资源，这里的资源指的就是Executor进程。YARN集群管理器会根据我们为Spark作业设置的资源参数，在各个工作节点上，启动一定数量的Executor进程，每个Executor进程都占有一定数量的内存和CPU core。此时可以把每一个Executor看作一个个的机器，但是又不是真正的机器，是在真实机上由Yarn虚拟出来的，倒是很类似Docker，这种类比方便理解，后面的对Executor的操作就相当于对单独的机器操作。<br>
在申请到了作业执行所需的资源之后，Driver进程就会开始调度和执行我们编写的作业代码了。Driver进程会将我们编写的Spark作业代码分拆为多个stage，stage就相当于阶段的意思，spark是按照阶段来执行程序的，具体的stage的划分原理不细说，每个stage执行一部分代码片段，Driver为每个stage创建一批task，然后将这些task分配到各个Executor进程中执行。task是最小的计算单元，负责执行一模一样的计算逻辑，只是每个task处理的数据不同而已。一个stage的所有task都执行完毕之后，会将计算结果写入到当前的Executor中。
<font color='red'>（内存or硬盘？）</font><br>
然后Driver就会调度运行下一个stage。下一个stage的task的输入数据就是上一个stage输出的结果。如此循环往复，直到将我们自己编写的代码逻辑全部执行完，执行完整个程序。<br>

#### Shuffle

Spark是根据shuffle类算子来进行stage的划分。如果我们的代码中执行了某个shuffle类算子（比如reduceByKey、join等），那么就会在该算子处，划分出一个stage界限来。可以大致理解为，shuffle算子执行之前的代码会被划分为一个stage，shuffle算子执行以及之后的代码会被划分为下一个stage。因此一个stage刚开始执行的时候，它的每个task可能都会从上一个stage的task所在的节点，去通过网络传输拉取需要自己处理的所有key，然后对拉取到的所有相同的key使用我们自己编写的算子函数执行聚合操作（比如reduceByKey()算子接收的函数）。这个过程就是shuffle。

#### 持久化
当我们在代码中执行了cache/persist等持久化操作时，根据我们选择的持久化级别的不同，每个task计算出来的数据也会保存到Executor进程的内存或者所在节点的磁盘文件中。<br>
如何设置持久化？

#### Executor内存分块
<div style="text-align:center" markdown="1">
![](/img/post-spark-executor-model.png)
</div>
Executor的内存主要分为三块，如上图所示：第一块是让task执行我们自己编写的代码时使用，默认是占Executor总内存的20%；第二块是让task通过shuffle过程拉取了上一个stage的task的输出后，进行聚合等操作时使用，默认也是占Executor总内存的20%；第三块是让RDD持久化时使用，默认占Executor总内存的60%。
<font color='red'>放置一张图片</font><br>

#### Task
Task是在Executor中执行的，TaskSechuderManager将Task分配给Executor来执行，每一个Task的执行速度和当前的Executor的Core以及内存有关的。一个Core一次只能执行一个Task，如果当前的
Executor的Core多的话，那么就能有多个Task同时被执行。从整个任务上来看，那么速度就快很多。<br>
Task的多少是如何划分的？<br>
Task和Partition的关系以及Partition与Executor的关系？<br>

#### 参考文章：
[http://tech.meituan.com/spark-tuning-basic.html](http://tech.meituan.com/spark-tuning-basic.html)
<br>
[https://www.iteblog.com/archives/1659.html](https://www.iteblog.com/archives/1659.html)
