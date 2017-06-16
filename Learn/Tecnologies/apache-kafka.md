

Apache Kafka was originated at LinkedIn and later became an open sourced Apache project in 2011, then First-class Apache project in 2012. Kafka is written in Scala and Java. Apache Kafka is publish-subscribe based fault tolerant messaging system. It is fast, scalable and distributed by design.

Kafka is designed for distributed high throughput systems. Kafka tends to work very well as a replacement for a more traditional message broker. In comparison to other messaging systems, Kafka has better throughput, built-in partitioning, replication and inherent fault-tolerance, which makes it a good fit for large-scale message processing applications.

Kafka is suitable for both offline and online message consumption. Kafka messages are persisted on the disk and replicated within the cluster to prevent data loss. Kafka is built on top of the ZooKeeper synchronization service. It integrates very well with Apache Storm and Spark for real-time streaming data analysis.

 It has the ability to handle a large number of diverse consumers.

## benefits of Kafka

**Reliability** − Kafka is distributed, partitioned, replicated and fault tolerance.

**Scalability** − Kafka messaging system scales easily without down time..

**Durability** − Kafka uses Distributed commit log which means messages persists on disk as fast as possible, hence it is durable..

**Performance** − Kafka has high throughput for both publishing and subscribing messages. It maintains stable performance even many TB of messages are stored. Kafka is very fast, performs 2 million writes/sec.
Kafka persists all data to the disk, which essentially means that all the writes go to the page cache of the OS (RAM). This makes it very efficient to transfer data from page cache to a network socket.

Kafka is very fast and guarantees zero downtime and zero data loss.

## components 

1	
Topics

A stream of messages belonging to a particular category is called a topic. Data is stored in topics.

Topics are split into partitions. For each topic, Kafka keeps a mini-mum of one partition. Each such partition contains messages in an immutable ordered sequence. A partition is implemented as a set of segment files of equal sizes.

2	
Partition

Topics may have many partitions, so it can handle an arbitrary amount of data.

3	
Partition offset

Each partitioned message has a unique sequence id called as offset.

4	
Replicas of partition

Replicas are nothing but backups of a partition. Replicas are never read or write data. They are used to prevent data loss.

5	
Brokers

Brokers are simple system responsible for maintaining the pub-lished data. Each broker may have zero or more partitions per topic. Assume, if there are N partitions in a topic and N number of brokers, each broker will have one partition.

Assume if there are N partitions in a topic and more than N brokers (n + m), the first N broker will have one partition and the next M broker will not have any partition for that particular topic.

Assume if there are N partitions in a topic and less than N brokers (n-m), each broker will have one or more partition sharing among them. This scenario is not recommended due to unequal load distri-bution among the broker.

6	
Kafka Cluster

Kafka’s having more than one broker are called as Kafka cluster. A Kafka cluster can be expanded without downtime. These clusters are used to manage the persistence and replication of message data.

7	
Producers

Producers are the publisher of messages to one or more Kafka topics. Producers send data to Kafka brokers. Every time a producer pub-lishes a message to a broker, the broker simply appends the message to the last segment file. Actually, the message will be appended to a partition. Producer can also send messages to a partition of their choice.

8	
Consumers

Consumers read data from brokers. Consumers subscribes to one or more topics and consume published messages by pulling data from the brokers.

9	
Leader

Leader is the node responsible for all reads and writes for the given partition. Every partition has one server acting as a leader.

10	
Follower

Node which follows leader instructions are called as follower. If the leader fails, one of the follower will automatically become the new leader. A follower acts as normal consumer, pulls messages and up-dates its own data store.


# Resources

* [https://www.tutorialspoint.com/apache_kafka/apache_kafka_fundamentals.htm](https://www.tutorialspoint.com/apache_kafka/apache_kafka_fundamentals.htm)

