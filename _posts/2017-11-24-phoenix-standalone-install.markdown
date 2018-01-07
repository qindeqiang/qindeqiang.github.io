---
title:      "Apache Phoenix 单机版的安装"
subtitle:   "Apache Phoenix Install Standalone "
author:     "Deqiang Qin"
featimg: 4.jpg
tags: [Bigdata]
published: true
categories: [Phoenix]
header-img: "img/post-bg-05.jpg"
---
# Apache Phoenix 简介
Apache Phoenix 是由Salesforce.com开源，是用在ApacheHbase上的一个SQL中间件，目的是为了让开发者能够在HBase上执行SQL查询。
<br>
Phoenix之后的版本支持Cloudera和MapR的商业发行版。

如下图所示使用Phoenix的公司，还是有很多的。这在Phoenix的官网上也能查到。<br>

<div>
<img src="/img/phoenix/post-phoenix-company.png" align="center">
</div>



# Apache Phoenix 的安装步骤：<br>
> 原来Phoenix还有一个翻译就是不死鸟。也是最近看美剧《电脑狂人(Halt and Catch Fire)》中给出的翻译.<br>

## 驱动拷贝
将对应版本的jar包拷贝到HBase的lib目录下面；
## 启动HBase

## 运行Phoenix
```
[bigdata@localhost bin]$ ./sqlline.py localhost
```
启动的时候报如下异常信息：<br>
```
[bigdata@localhost bin]$ ./sqlline.py localhost
Setting property: [incremental, false]
Setting property: [isolation, TRANSACTION_READ_COMMITTED]
issuing: !connect jdbc:phoenix:localhost none none org.apache.phoenix.jdbc.PhoenixDriver
Connecting to jdbc:phoenix:localhost
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/home/bigdata/apps/apache-phoenix-4.13.0-HBase-1.3-bin/phoenix-4.13.0-HBase-1.3-client.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/usr/local/hadoop-2.6.5/share/hadoop/common/lib/slf4j-log4j12-1.7.5.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
17/11/23 21:11:02 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Error: org.apache.hadoop.hbase.client.RetriesExhaustedException: Failed after attempts=36, exceptions:
Thu Nov 23 21:11:52 EST 2017, null, java.net.SocketTimeoutException: callTimeout=60000, callDuration=68700: Connection refused row 'SYSTEM:CATALOG,,' on table 'hbase:meta' at region=hbase:meta,,1.1588230740, hostname=localhost,43678,1511489383195, seqNum=0 (state=,code=0)
java.sql.SQLException: org.apache.hadoop.hbase.client.RetriesExhaustedException: Failed after attempts=36, exceptions:
Thu Nov 23 21:11:52 EST 2017, null, java.net.SocketTimeoutException: callTimeout=60000, callDuration=68700: Connection refused row 'SYSTEM:CATALOG,,' on table 'hbase:meta' at region=hbase:meta,,1.1588230740, hostname=localhost,43678,1511489383195, seqNum=0

	at org.apache.phoenix.query.ConnectionQueryServicesImpl$12.call(ConnectionQueryServicesImpl.java:2492)
	at org.apache.phoenix.query.ConnectionQueryServicesImpl$12.call(ConnectionQueryServicesImpl.java:2384)
	at org.apache.phoenix.util.PhoenixContextExecutor.call(PhoenixContextExecutor.java:76)
	at org.apache.phoenix.query.ConnectionQueryServicesImpl.init(ConnectionQueryServicesImpl.java:2384)
	at org.apache.phoenix.jdbc.PhoenixDriver.getConnectionQueryServices(PhoenixDriver.java:255)
	at org.apache.phoenix.jdbc.PhoenixEmbeddedDriver.createConnection(PhoenixEmbeddedDriver.java:150)
	at org.apache.phoenix.jdbc.PhoenixDriver.connect(PhoenixDriver.java:221)
	at sqlline.DatabaseConnection.connect(DatabaseConnection.java:157)
	at sqlline.DatabaseConnection.getConnection(DatabaseConnection.java:203)
	at sqlline.Commands.connect(Commands.java:1064)
	at sqlline.Commands.connect(Commands.java:996)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at sqlline.ReflectiveCommandHandler.execute(ReflectiveCommandHandler.java:38)
	at sqlline.SqlLine.dispatch(SqlLine.java:809)
	at sqlline.SqlLine.initArgs(SqlLine.java:588)
	at sqlline.SqlLine.begin(SqlLine.java:661)
	at sqlline.SqlLine.start(SqlLine.java:398)
	at sqlline.SqlLine.main(SqlLine.java:291)
Caused by: org.apache.hadoop.hbase.client.RetriesExhaustedException: Failed after attempts=36, exceptions:
Thu Nov 23 21:11:52 EST 2017, null, java.net.SocketTimeoutException: callTimeout=60000, callDuration=68700: Connection refused row 'SYSTEM:CATALOG,,' on table 'hbase:meta' at region=hbase:meta,,1.1588230740, hostname=localhost,43678,1511489383195, seqNum=0

	at org.apache.hadoop.hbase.client.RpcRetryingCallerWithReadReplicas.throwEnrichedException(RpcRetryingCallerWithReadReplicas.java:276)
	at org.apache.hadoop.hbase.client.ScannerCallableWithReplicas.call(ScannerCallableWithReplicas.java:210)
	at org.apache.hadoop.hbase.client.ScannerCallableWithReplicas.call(ScannerCallableWithReplicas.java:60)
	at org.apache.hadoop.hbase.client.RpcRetryingCaller.callWithoutRetries(RpcRetryingCaller.java:212)
	at org.apache.hadoop.hbase.client.ClientScanner.call(ClientScanner.java:314)
	at org.apache.hadoop.hbase.client.ClientScanner.nextScanner(ClientScanner.java:289)
	at org.apache.hadoop.hbase.client.ClientScanner.initializeScannerInConstruction(ClientScanner.java:164)
	at org.apache.hadoop.hbase.client.ClientScanner.<init>(ClientScanner.java:159)
	at org.apache.hadoop.hbase.client.HTable.getScanner(HTable.java:796)
	at org.apache.hadoop.hbase.MetaTableAccessor.fullScan(MetaTableAccessor.java:602)
	at org.apache.hadoop.hbase.MetaTableAccessor.tableExists(MetaTableAccessor.java:366)
	at org.apache.hadoop.hbase.client.HBaseAdmin.tableExists(HBaseAdmin.java:408)
	at org.apache.phoenix.query.ConnectionQueryServicesImpl$12.call(ConnectionQueryServicesImpl.java:2412)
	... 20 more
Caused by: java.net.SocketTimeoutException: callTimeout=60000, callDuration=68700: Connection refused row 'SYSTEM:CATALOG,,' on table 'hbase:meta' at region=hbase:meta,,1.1588230740, hostname=localhost,43678,1511489383195, seqNum=0
	at org.apache.hadoop.hbase.client.RpcRetryingCaller.callWithRetries(RpcRetryingCaller.java:171)
	at org.apache.hadoop.hbase.client.ResultBoundedCompletionService$QueueingFuture.run(ResultBoundedCompletionService.java:65)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617)
	at java.lang.Thread.run(Thread.java:748)
Caused by: java.net.ConnectException: Connection refused
	at sun.nio.ch.SocketChannelImpl.checkConnect(Native Method)
	at sun.nio.ch.SocketChannelImpl.finishConnect(SocketChannelImpl.java:717)
	at org.apache.hadoop.net.SocketIOWithTimeout.connect(SocketIOWithTimeout.java:206)
	at org.apache.hadoop.net.NetUtils.connect(NetUtils.java:531)
	at org.apache.hadoop.net.NetUtils.connect(NetUtils.java:495)
	at org.apache.hadoop.hbase.ipc.RpcClientImpl$Connection.setupConnection(RpcClientImpl.java:416)
	at org.apache.hadoop.hbase.ipc.RpcClientImpl$Connection.setupIOstreams(RpcClientImpl.java:722)
	at org.apache.hadoop.hbase.ipc.RpcClientImpl$Connection.writeRequest(RpcClientImpl.java:909)
	at org.apache.hadoop.hbase.ipc.RpcClientImpl$Connection.tracedWriteRequest(RpcClientImpl.java:873)
	at org.apache.hadoop.hbase.ipc.RpcClientImpl.call(RpcClientImpl.java:1244)
	at org.apache.hadoop.hbase.ipc.AbstractRpcClient.callBlockingMethod(AbstractRpcClient.java:227)
	at org.apache.hadoop.hbase.ipc.AbstractRpcClient$BlockingRpcChannelImplementation.callBlockingMethod(AbstractRpcClient.java:336)
	at org.apache.hadoop.hbase.protobuf.generated.ClientProtos$ClientService$BlockingStub.scan(ClientProtos.java:35396)
	at org.apache.hadoop.hbase.client.ScannerCallable.openScanner(ScannerCallable.java:404)
	at org.apache.hadoop.hbase.client.ScannerCallable.call(ScannerCallable.java:211)
	at org.apache.hadoop.hbase.client.ScannerCallable.call(ScannerCallable.java:65)
	at org.apache.hadoop.hbase.client.RpcRetryingCaller.callWithoutRetries(RpcRetryingCaller.java:212)
	at org.apache.hadoop.hbase.client.ScannerCallableWithReplicas$RetryingRPC.call(ScannerCallableWithReplicas.java:364)
	at org.apache.hadoop.hbase.client.ScannerCallableWithReplicas$RetryingRPC.call(ScannerCallableWithReplicas.java:338)
	at org.apache.hadoop.hbase.client.RpcRetryingCaller.callWithRetries(RpcRetryingCaller.java:137)
	... 4 more
sqlline version 1.2.0
0: jdbc:phoenix:localhost>
```

也可以通过如下指令查看链接是否异常:<br>
```
0: jdbc:phoenix:localhost> !tables
java.lang.IllegalArgumentException: No current connection
	at sqlline.SqlLine.getConnection(SqlLine.java:416)
	at sqlline.Commands.tables(Commands.java:329)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at sqlline.ReflectiveCommandHandler.execute(ReflectiveCommandHandler.java:38)
	at sqlline.SqlLine.dispatch(SqlLine.java:809)
	at sqlline.SqlLine.begin(SqlLine.java:686)
	at sqlline.SqlLine.start(SqlLine.java:398)
	at sqlline.SqlLine.main(SqlLine.java:291)

```

报如下异常信息：<br>
```
org.apache.phoenix.exception.PhoenixIOException: SYSTEM.CATALOG
	at org.apache.phoenix.util.ServerUtil.parseServerException(ServerUtil.java:120)
	at org.apache.phoenix.query.ConnectionQueryServicesImpl.metaDataCoprocessorExec(ConnectionQueryServicesImpl.java:1293)
	at org.apache.phoenix.query.ConnectionQueryServicesImpl.metaDataCoprocessorExec(ConnectionQueryServicesImpl.java:1257)
	at org.apache.phoenix.query.ConnectionQueryServicesImpl.createTable(ConnectionQueryServicesImpl.java:1442)
	at org.apache.phoenix.schema.MetaDataClient.createTableInternal(MetaDataClient.java:2639)
	at org.apache.phoenix.schema.MetaDataClient.createTable(MetaDataClient.java:1044)
	at org.apache.phoenix.compile.CreateTableCompiler$1.execute(CreateTableCompiler.java:192)
	at org.apache.phoenix.jdbc.PhoenixStatement$2.call(PhoenixStatement.java:394)
	at org.apache.phoenix.jdbc.PhoenixStatement$2.call(PhoenixStatement.java:377)
	at org.apache.phoenix.call.CallRunner.run(CallRunner.java:53)
	at org.apache.phoenix.jdbc.PhoenixStatement.executeMutation(PhoenixStatement.java:375)
	at org.apache.phoenix.jdbc.PhoenixStatement.executeMutation(PhoenixStatement.java:364)
	at org.apache.phoenix.jdbc.PhoenixStatement.executeUpdate(PhoenixStatement.java:1719)
	at org.apache.phoenix.query.ConnectionQueryServicesImpl$12.call(ConnectionQueryServicesImpl.java:2445)
	at org.apache.phoenix.query.ConnectionQueryServicesImpl$12.call(ConnectionQueryServicesImpl.java:2384)
	at org.apache.phoenix.util.PhoenixContextExecutor.call(PhoenixContextExecutor.java:76)
	at org.apache.phoenix.query.ConnectionQueryServicesImpl.init(ConnectionQueryServicesImpl.java:2384)
	at org.apache.phoenix.jdbc.PhoenixDriver.getConnectionQueryServices(PhoenixDriver.java:255)
	at org.apache.phoenix.jdbc.PhoenixEmbeddedDriver.createConnection(PhoenixEmbeddedDriver.java:150)
	at org.apache.phoenix.jdbc.PhoenixDriver.connect(PhoenixDriver.java:221)
	at sqlline.DatabaseConnection.connect(DatabaseConnection.java:157)
	at sqlline.DatabaseConnection.getConnection(DatabaseConnection.java:203)
	at sqlline.Commands.close(Commands.java:906)
	at sqlline.Commands.closeall(Commands.java:880)
	at sqlline.SqlLine.begin(SqlLine.java:714)
	at sqlline.SqlLine.start(SqlLine.java:398)
	at sqlline.SqlLine.main(SqlLine.java:291)
Caused by: org.apache.hadoop.hbase.TableNotFoundException: SYSTEM.CATALOG
	at org.apache.hadoop.hbase.client.ConnectionManager$HConnectionImplementation.locateRegionInMeta(ConnectionManager.java:1285)
	at org.apache.hadoop.hbase.client.ConnectionManager$HConnectionImplementation.locateRegion(ConnectionManager.java:1183)
	at org.apache.hadoop.hbase.client.ConnectionManager$HConnectionImplementation.locateRegion(ConnectionManager.java:1167)
	at org.apache.hadoop.hbase.client.ConnectionManager$HConnectionImplementation.locateRegion(ConnectionManager.java:1124)
	at org.apache.hadoop.hbase.client.ConnectionManager$HConnectionImplementation.getRegionLocation(ConnectionManager.java:959)
	at org.apache.hadoop.hbase.client.HRegionLocator.getRegionLocation(HRegionLocator.java:83)
	at org.apache.hadoop.hbase.client.HTable.getRegionLocation(HTable.java:505)
	at org.apache.hadoop.hbase.client.HTable.getKeysAndRegionsInRange(HTable.java:721)
	at org.apache.hadoop.hbase.client.HTable.getKeysAndRegionsInRange(HTable.java:691)
	at org.apache.hadoop.hbase.client.HTable.getStartKeysInRange(HTable.java:1796)
	at org.apache.hadoop.hbase.client.HTable.coprocessorService(HTable.java:1751)
	at org.apache.hadoop.hbase.client.HTable.coprocessorService(HTable.java:1731)
	at org.apache.phoenix.query.ConnectionQueryServicesImpl.metaDataCoprocessorExec(ConnectionQueryServicesImpl.java:1276)
	... 25 more
```
以上的错误的原因主要是HBase的安装出现了问题，具体的可以查看我的HBase的单机版的安装方法那篇文章。<br>

其中Hbase下也有可以直接访问zk的客户端：<br>
`hbase zkcli`
