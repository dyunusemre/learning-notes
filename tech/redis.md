# Redis

1. Definition: is an open-source in-memory data structure storage.
	1. used as database
	2. cache(key-value store)
	3. message broker
	4. streaming engine
2. Supported Data Structures
	1. String
	2. JSON
	3. Lists
	4. Set
	5. Sorted Sets
	6. Hashes
3. Redis Persistence: for redis instances.
	1. Persistence refers to writing data to durable disk such as SSD
	2. persistence modes
		1. RDB(Redis Database): point in time snapshots of your data set specified intervals.
			1. is not good for if you worry about not losing data
			2. usually creates a snapshots in 5min or you can set like very 100 writes etc
			3.
		2. AOF(Append Only File): persistence log every write operation received by the server. These operation can be replayed.
			1. much more durable. default policy f sync every second and write performance still good.
			2.
		3. no persistence: no persistence sometimes used for caching.
		4. RDP+AOF: both can be applied on the same instance
4. Redis Sentinel
	1. Redis sentinel provides highly availability when using not Redis Cluster
	2. Monitoring checks master and replicas working as expected
	3. Notification: Redis can send notification when something goes wrong
	4. Automatic failover: if a master not corresponding, Redis sentinel automatically assign a new master and create new replica and other replicas started to use new master for syncing
5. Redis Cluster
	1. Horizontal Scaling with cluster.
	2. Automatically split our data set with running replicas/nodes
	3. continue to work in case of network partitioning
	4. Sharding
		1. Every key in Redis is hashing with hash slot.
		2. same keys stay in same nodes.
		3. there 16384 hash slots in Redis cluster and every node responsible a part of it accordingly number of nodes.
		4. it can be configurable percentage of which nodes can hold hash slot and it can be moved without downtime.
	5. to sustain available redis cluster uses also master-replica model. every master nodes has it own replicas.
		1. Redis cluster not strong consistency.
		2. Redis cluster default uses async replication.
		3. so if a client wants to ride b node
		4. in that point b node unavailable
		5. Redis cluster promoted replica of b1
		6. because replication not waiting acknowledgement
		7. and written data may be lost forever.
		8. it can be sync but tradeoff is performance and latency.
		9. even with sync replication strong consistenc
		10. voy not availably.
6. Keyspace
7. Client side Caching
8. Eviction
	1. time to live TTL
	2. allkey-lru for all keys without checking ttl
	3. allkey- lfu for all keys without checking ttl
	4. volatile-ttl - removes keys with ttl
	5. volatile lfu- least frequently used but ttl set
	6. volatile lru- least recently used but ttl set
	7. allkeys random

9. Distributed Locks
	1. Redisson