# Kafka Terminology
1. Definition: open source distributed event streaming platform. used for real-time data pipelines and streaming applications.
	1. high throughput(amount of work in given time frame, mean system can process, handle large volumes of data), 
	2. fault-tolerance: system continues to work in the event of partial system failures. in Kafka, fault tolerance achieved by replication.each event is stored in multiple brokers. if one is fail message is not lost.
	3. scalability. Kafka can horizontally scaled by adding nodes. so system can demand the requests.
2. Main components of Kafka
	1. **broker**: Kafka servers that store data and serve client requests.
		1. each brokers stores subset of topic partitions.
		2. for each partition one broker servers as a master which replicating data
		3. adding more brokers can increase the capacity of Kafka.
	2. **topic**: is a logical channel that producers write and consumers read from the topics 
		1. data retain configuration made on topic
		2. immutable: once data written in the topic(partition) it cannot be changed 
	3. **partition**: basic unit of paralelism in Kafka. topics are divided into partitions. each partition can be host in the different broker
		1. order: records in a partition maintain the order in which they were written(inserting)
		2. replication: partitions are replicated for the fault tolerance.
		3. offset:each record in a partition has a unique offset.
		4. partition is set while creating the Kafka topics 
			1. kafka-topics --create --zookeeper localhost:2181 --replication-factor 1 --partitions 5 --topic my-topic
		5. important to note partition number can be increase not but decrease support. it may data loss.
		6. even if concurrent listeners working in an instance, only one consumer can consume from one partition.
	4. **producer**: are clients that sends messages(event) to Kafka topics.
		1. producers gets acknowledgement from topic to ensure durability and messages written in to the topics
		2. producers automatically sends retries in case of failure. 
			1. default kafka.producer.retries: Int.MAX_VALUE: we use it as 5.
			2. it will continue to retry.
			3. 100ms time.
		3. producers use partitioners to determine which partition it should send messages
		4. producers can send the message with a key, so it can hashed to same partition if ordering is important.
		5. if no key provided from producer Kafka will run with round robin algorithm.
		6. kafkaTemplate.send('topic-name', key, message); for example if we want to a user related events we can send user as a key so all events goes to same partition in order for that user.
	5. **consumer**: are clients read(consume) messages from Kafka topics
		1. offset: consumers keep tracks of the records they've have consumed by using offset. offset stored in kafka built in consumer__offset_group. so consumer always know which offset. ConsumerRecord class has offset in metadata of the message or record. maintain by partition level
		2. consumer groups: ensuring each messages consumed by only one consumer. divides the load by this approach this prevents dublicate message processing.
			1. for example if we have 3 instances of a consumer and 3 concurrent thread we can say 9 consumer groups.
			2. and if we have 18 partition. kafka will divide per 2 for consumers and assign 2 partition for those listeners(consumers)
		3. poll model: consumer send requests when they ready to consume
		4. poll loop model only used for one consumer. it may result inconsistency.
	6. **offset**: 
		1. unique id for each message, maintain by the partition level.
		2. it increase by 1, consumer fetch the message as batch record [default batch size 500].
		3. so in kafka one consumer per one partition and also offset unique id maintain in partition level. so after batch messaging completed, every consumer that listen that partition will know which records didn't proceed yet.
		4. also if we manually try to commit offset this may cause re-processing the events if we don't caution.
		5. auto-offsett-reset strategy: earliest, latest, none default is earliest.
	7. **zookeeper**:
		1. distributed sync.
		2. master election: defines which broker should act as a master
		3. Kafka raft wants to replace dependency on zookeeper