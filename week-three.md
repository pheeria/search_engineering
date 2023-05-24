# Week 3

## Level 1

### Q1: Which node was elected as cluster manager?
opensearch-node2

### Q2: After stopping the previous cluster manager, which node was elected the new cluster manager?
opensearch-node3

[opensearch-node3] elected-as-cluster-manager ([2] nodes joined)
        [{opensearch-node3}{8taOttegRjyqIKz6oqxdXg}{EQxwgIJTTfeWXeFt-kgmww}{172.23.0.5}{172.23.0.5:9300}{dimr}{shard_indexing_pressure_enabled=true} elect leader,
         {opensearch-node1}{qM9NB4WSTp2YTbdV9FaCUA}{7zyZ8PQlRiCyh4LPB1tapA}{172.23.0.7}{172.23.0.7:9300}{dimr}{shard_indexing_pressure_enabled=true} elect leader, _BECOME_CLUSTER_MANAGER_TASK_, _FINISH_ELECTION_],
         term: 4,
         version: 70,
         delta: cluster-manager node changed
         {previous [],
          current [{opensearch-node3}{8taOttegRjyqIKz6oqxdXg}{EQxwgIJTTfeWXeFt-kgmww}{172.23.0.5}{172.23.0.5:9300}{dimr}{shard_indexing_pressure_enabled=true}]}

### Q3: Did the cluster manager node change again? (was a different node elected as cluster manager when you started the node back up?)
No, it did not change again.

## Level 2

### Q4: How much faster was it to index the dataset with 0 replicas versus the previous time with 2 replica shards?
I mistakenly used 16 workers instead of 8 (didn't save the file 😅) and decided to keep the setting, so that the comparison makes sense.

With "number_of_shards": 3, "number_of_replicas": 2
INFO:Done. 1275077 were indexed in 6.0053447875000225 minutes.
Total accumulated time spent in `bulk` indexing: 83.85183604332464 minutes

With "number_of_shards": 3, "number_of_replicas": 0
INFO:Done. 1275077 were indexed in 2.556026663183002 minutes.
Total accumulated time spent in `bulk` indexing: 24.846760695094904 minutes

### Q5: Why was it faster?
Without replicas primary shards don't have to send the data to replicas and wait for the replication.
This is done in parallel, however it's still a work that needs to be done and involves network calls.

### Q6: How long did it take to create the new replica shards?  This will be the difference in time between those two log messages.
44 seconds.

[2023-05-21T17:20:51,754][INFO ][o.o.c.m.MetadataUpdateSettingsService] [opensearch-node3] updating number_of_replicas to [2] for indices [bbuy_products]
[2023-05-21T17:21:35,636][INFO ][o.o.c.r.a.AllocationService] [opensearch-node3] Cluster health status changed from [YELLOW] to [GREEN] (reason: [shards started [[bbuy_products][2]]]).


### Q7: Those two messages were both logged by the cluster_manager.  Why do you think the cluster manager is the node that logs these actions (versus non-manager nodes)?
The cluster is responsible for managing the cluster. Things like creating new replicas belong to it.


## Level 3

### Q8: Looking at the metrics dashboard, what queries/sec rate are you getting?
205 queries per second per node at maximum.

### Q9: How does that compare to the max queries/sec rate you saw in week 2?
Last week, when using 1 CPU, 2GB RAM the QPS was 115.
So a bit less than twice of an improvement.
