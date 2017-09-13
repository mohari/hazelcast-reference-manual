Hazelcast provides an HTTP based health check implementation and a Health Monitoring utility explained as below.


### Health Check



hazelcast.http.healthcheck.enabled		Enable/disable Hazelcast's HTTP based health check implementation. When it is enabled, you can retrieve information about your cluster's health status (member state, cluster state, cluster size, etc.) by launching http://<your member's host IP>:5701/hazelcast/health

An example output is given below:

```
Hazelcast::NodeState=ACTIVE
Hazelcast::ClusterState=ACTIVE
Hazelcast::ClusterSafe=TRUE
Hazelcast::MigrationQueueSize=0
Hazelcast::ClusterSize=2
```


### Health Monitor

Health monitor periodically prints logs about some metrics to provide information about your Hazelcast's state. By default, it is enabled when you start your cluster.


hazelcast.health.monitoring.delay.seconds		Health monitoring logging interval in seconds.

hazelcast.health.monitoring.level		Health monitoring log level. When SILENT, logs are printed only when values exceed some predefined threshold. When NOISY, logs are always printed periodically. Set OFF to turn off completely.


```
Sep 08, 2017 5:02:28 PM com.hazelcast.internal.diagnostics.HealthMonitor
INFO: [192.168.2.44]:5701 [host-name] [3.8.1] processors=4, physical.memory.total=16.0G, physical.memory.free=5.5G, swap.space.total=0, swap.space.free=0, heap.memory.used=102.4M, heap.memory.free=249.1M, heap.memory.total=351.5M, heap.memory.max=3.6G, heap.memory.used/total=29.14%, heap.memory.used/max=2.81%, minor.gc.count=4, minor.gc.time=68ms, major.gc.count=1, major.gc.time=41ms, load.process=0.44%, load.system=1.00%, load.systemAverage=315.48%, thread.count=97, thread.peakCount=98, cluster.timeDiff=0, event.q.size=0, executor.q.async.size=0, executor.q.client.size=0, executor.q.query.size=0, executor.q.scheduled.size=0, executor.q.io.size=0, executor.q.system.size=0, executor.q.operations.size=0, executor.q.priorityOperation.size=0, operations.completed.count=226, executor.q.mapLoad.size=0, executor.q.mapLoadAllKeys.size=0, executor.q.cluster.size=0, executor.q.response.size=0, operations.running.count=0, operations.pending.invocations.percentage=0.00%, operations.pending.invocations.count=0, proxy.count=0, clientEndpoint.count=1, connection.active.count=2, client.connection.count=1, connection.count=1
```