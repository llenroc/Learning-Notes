# Kafka

> Graphical User Interface has been doing this for a long time. Instead of a TextInput know all your code, it will only generate event for you to handle the logic.

* [Thorough Introduction to Apache Kafka](https://hackernoon.com/thorough-introduction-to-apache-kafka-6fbf2989bbc1)
* [Handling user-initiated actions in an asynchronous, message-based architecture](https://www.oreilly.com/ideas/handling-user-initiated-actions-in-an-asynchronous-message-based-architecture)
* [Turning the database inside-out with Apache Samza](https://www.confluent.io/blog/turning-the-database-inside-out-with-apache-samza/)

> To ensure that the new system worked end-to-end, we enqueued heartbeat canaries for every Kafka partition every minute.

A high-throughput distributed messaging system rethought as a **distributed commit log/write-ahead log/transaction log**.

Kafka does not do data processing or transforming. It basically is a logger to produce and consume messages. It is a distributed commit log.

* Basically a WAL/binlog for application
* Producers push and consumers pull
* No state held by broker
* Data movement
* Decoupled. Multiple data producers and consumers.
* Re-playable history - configurable time-to-live for messages
* Retains offsets
* Enable **Event Sourcing** - an architectural style to maintaining an application's state by capturing all changes as a sequence of time-ordered, immutable events.
* Message stay on disk when consumed, deletes on TTL or compaction. You can store message for days or weeks, it does not get lost after being consumed. You can therefore go back and replay or rerun things if you want.
* Recently added better support for transaction
* Kafka Stream - for building asynchronous, loosely-coupled and stateful applications

---

* [Turning the database inside out with Apache Samza - Martin Kleppmann](https://www.youtube.com/watch?v=fU9hR3kiOK0)

## Change Data Capture


## Data Pipelines

Data pipelines typically start out simply. A single place where all data resides. A single ETL process to move data to that location. However, data pipelines inevitably grow over time. New systems are added. Each new system requires its own ETL procedures. Data storage formats diverge.

* The first is building a data pipeline where Apache Kafka is one of the two end points. For example, getting data from Kafka to S3 or getting data from MongoDB into Kafka.
* The second use case involves building a pipeline between two different systems but using Kafka as an intermediary. An example of this is getting data from Twitter to Elasticsearch by sending the data first from Twitter to Kafka and then from Kafka to Elasticsearch.

The main value Kafka provides to data pipelines is its ability to serve as a very large, reliable buffer between various stages in the pipeline, effectively decoupling producers and consumers of data within the pipeline.

Kafka decouples data source and destination.

## Data Problems

Many source systems:

* Relational Databases
* Log files
* Metrics
* Messaging systems

Many data formats:

* XML
* CSV
* JSON

Constant change:

* New schemas
* New data sources

## Leaders and Followers

* Followers don't serve client requests, their only job is to replicate messages from the leader and stay up to date with the most recent messages the leader has

## Jobline

* Event-driven
* Central nervous system
* Central hub
* True integration
* Real-time processing

## Terms

* Worker
* Work load
* Topic
* Queue
* PubSub
* Route
* Message

## Installing

* Kafka broker is the server

```
▶ sudo apt-get install zookeeperd
▶ netstat -ant | grep :2181

```

```
adduser: Warning: The home directory `/var/lib/zookeeper' does not belong to the user you are currently creating.
```

## Plan your Partitions

* Or else some of your disk will be full and others barely used
* Ordering is also based on partition

## Maintain your Zookeeper

* Periodically clean up transaction log
* Leader data stored in Zookeeper
* Writes to Zookeeper are only performed on changes to the membership of consumer groups or on changes to the kafka cluster itself. This amount of traffic is minimal.

## Broker Considerations

Need at least 3 broker servers to guarantee no loss of data through replication.

* One broker is typically one machine.
* Each broker will have some of the partition of the same topic
* Kafka runs in a cluster. Nodes are called brokers.
* No need any RAID on Brokers
* Kafka is not CPU bound (only for compressing and decompressing messages), it is network hungry
* SSD not required for Kafka disks
* SSD for zookeeper data
* Tune `pdflush` for higher throughput
* `/proc/sys/vm/dirty_ratio` for more caching
* Listen on port 9092
* Running Kafka as root is not recommended
* Good practice to use a chroot path for the Kafka cluster
* Manage topic creation explicitly and disable `auto.create.topics`
* Disable `atime` with `noatime` during mount. Kafka only cares about `ctime` and `mtime`

```
// Using G1 Garbage-First on Java 7

export KAFKA_JVM_PERFORMANCE_OPTS="-server -XX:+UseG1GC
-XX:MaxGCPauseMillis=20 -XX:InitiatingHeapOccupancyPercent=35
-XX:+DisableExplicitGC -Djava.awt.headless=true"
```

## Producer

```
▶ /usr/local/kafka/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
```

## Serializer

## Consumer

Since a topic can have many partitions, each ordered by their own, consumers also need to be inside its own Consumer Group to receive messages from different subset of the partitions.

* Consumer Group is how we scale out consumer-side
* Kafka will do the job of load balancing consumption across Consumer Group

## Database Locking

One of Kafka's main job is to alleviate database locking when you stream too much data too frequently to your main database.

Decouple producers and consumers.

## Offset

* Automatic or manual offset management
* Caught up with your offset
* Allow consumer to maintain isolation and autonomy
* Allow consumer to read message at their own pace
* Retention period is 168 hours or 7 days and is configurable
* A given topic will have the same offset ID across their partitions?

## Log Compaction

* Turn Kafka into a long-term data store

## LeaderNotAvailable

Looping and dropping messages

```
fetch.message.max.bytes
replica.fetch.max.bytes
```

## Topics, Partitions, Segments

* Topic is broken down into partitions (like shards)
* A partition is stored on a single disk. It can't be split into multiple disks.
* Each partition can have multiple replicas, one of which is a designated leader
* Kafka will make sure each replica for a partition is on a separate broker (Node)
* Each partition has their own offset. So be mindful of sequential operations, but really you should design your logic that is independent of ordering and idempotent.

## Duplicates

* Kafka do not do de-duplication, so you need another data store with validation
* [Real-time deduping at scale](http://eng.tapjoy.com/blog-list/real-time-deduping-at-scale)

## Events vs Commands



## Case Studies

* [Scaling Slack's Job Queue](https://slack.engineering/scaling-slacks-job-queue-687222e9d100)

## Ruby

* [Kafka for Ruby](http://teamcoding.com/blog/2015/10/05/kafka/)
* [Avro in Ruby](http://teamcoding.com/blog/2015/12/01/avro-in-ruby/)
* [delivery_boy](https://github.com/zendesk/delivery_boy)
* [racecar](https://github.com/zendesk/racecar)

## Videos

* [Processing Streaming Data at a Large Scale with Kafka](https://www.youtube.com/watch?v=-NMDqqW1uCE)
* [The Many Meanings of Event-Driven Architecture](https://www.youtube.com/watch?v=STKCRSUsyP0)
* [Introduction to Apache Kafka by James Ward](https://www.youtube.com/watch?v=UEg40Te8pnE)

