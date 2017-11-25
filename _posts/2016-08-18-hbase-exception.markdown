异常描述：<br>
HBase Shell 执行list的时候报如下异常：
```
ERROR: org.apache.hadoop.hbase.PleaseHoldException: Master is initializing
	at org.apache.hadoop.hbase.master.HMaster.checkInitialized(HMaster.java:2268)
	at org.apache.hadoop.hbase.master.MasterRpcServices.getTableNames(MasterRpcServices.java:899)
	at org.apache.hadoop.hbase.protobuf.generated.MasterProtos$MasterService$2.callBlockingMethod(MasterProtos.java:55650)
	at org.apache.hadoop.hbase.ipc.RpcServer.call(RpcServer.java:2170)
	at org.apache.hadoop.hbase.ipc.CallRunner.run(CallRunner.java:109)
	at org.apache.hadoop.hbase.ipc.RpcExecutor.consumerLoop(RpcExecutor.java:133)
	at org.apache.hadoop.hbase.ipc.RpcExecutor$1.run(RpcExecutor.java:108)
	at java.lang.Thread.run(Thread.java:748)
```
解决办法：<br>
如果是单机版的，最简单的是删除hbase-datas数据，重新启动即可。<br>
主要原因还是因为Hbase的当前节点的时间不通步导致的。<br>
