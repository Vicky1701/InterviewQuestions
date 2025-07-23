<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

# Apache Kafka Interview Questions and Answers

*Backend Java Developer – Interview Style*

## Core Concepts

### Q1: What is Apache Kafka and what problem does it solve?

**Answer:**
Apache Kafka is a distributed streaming platform used for building real-time data pipelines and streaming applications. It allows you to publish, subscribe to, store, and process streams of records in a fault-tolerant way. Kafka solves issues related to high-throughput, low-latency handling of real-time data feeds and decouples data producers from data consumers, enabling scalable and durable messaging between distributed systems[^1][^2][^3].

### Q2: Explain the main components of Kafka architecture (Broker, Topic, Partition, Producer, Consumer).

**Answer:**

- **Broker:** Kafka runs as a cluster of servers, each called a broker, that receives, stores, and serves messages.
- **Topic:** A category or feed name to which records are stored and published.
- **Partition:** Each topic is split into partitions for scalability and performance. Partitions are distributed across brokers.
- **Producer:** An application that publishes data (messages) into Kafka topics.
- **Consumer:** An application that subscribes to Kafka topics and processes the published messages[^1][^2][^3].


### Q3: What is a Kafka topic and what are partitions?

**Answer:**
A *topic* is a stream of messages of a similar type. Each topic is divided into *partitions*, which are logs that allow Kafka to parallelize data processing and improve scalability and fault tolerance[^1][^2][^3].

### Q4: What is a Kafka broker and what role does it play?

**Answer:**
A broker is a Kafka server responsible for storing data and serving client requests. In a Kafka cluster, brokers manage partitions, replicate data for fault tolerance, and ensure high availability[^1][^3].

### Q5: Explain Producer and Consumer in Kafka.

**Answer:**

- **Producer:** Sends or publishes messages to Kafka topics. Can write data to specific partitions if required.
- **Consumer:** Reads or subscribes to messages from one or more topics, handling data processing or storage. Consumers can form groups for load balancing and parallel processing[^3].


### Q6: What is a Consumer Group and how does it work?

**Answer:**
A consumer group is a set of consumers that coordinate to read data from a set of partitions of a topic. Each partition is consumed by a single consumer in the group at a time, ensuring that the processing load is shared and that each message is processed only once by the group[^1][^3].

### Q7: What is offset in Kafka? How is it managed?

**Answer:**
Offset is a unique identifier for each message within a partition. It tracks which messages have been consumed. Consumers periodically commit offsets to Kafka or external systems, enabling at-least-once or exactly-once delivery guarantees and recovery from failures[^1][^3].

### Q8: What is Zookeeper's role in Kafka?

**Answer:**
Zookeeper helps manage and coordinate Kafka brokers, handle leader elections for partitions, maintain metadata, and keep track of broker states. Kafka uses Zookeeper for cluster management tasks, but it is being phased out in newer versions in favor of Kafka’s own quorum controller[^3].

## Key Features

### Q9: How does Kafka ensure message ordering?

**Answer:**
Kafka guarantees ordering within a partition. Messages sent to the same partition are stored and read in order. However, across partitions, ordering is not guaranteed[^1][^3].

### Q10: What are the delivery semantics in Kafka?

**Answer:**

- **At least once:** Every message will be delivered at least once, but duplicates are possible.
- **At most once:** Each message is delivered no more than once, possible data loss.
- **Exactly once:** Guarantees each message is delivered once and only once (supported since Kafka 0.11+)[^1][^3].


### Q11: How does Kafka achieve high availability and fault tolerance?

**Answer:**
Kafka achieves high availability through replication: each partition has one leader and one or more follower replicas. If a leader fails, a follower is promoted. Data is replicated, and failures can be tolerated without data loss, provided a minimum number of brokers are available[^1][^3].

### Q12: What is replication in Kafka? What are leader and follower replicas?

**Answer:**
Replication ensures data durability and availability. Each partition elects a leader (handles all reads and writes). Other replicas are followers that replicate the leader’s data. If the leader fails, a new leader is chosen from in-sync replicas (ISRs)[^1][^3].

## Technical Details

### Q13: What is the difference between Kafka and traditional message queues (JMS, RabbitMQ)?

**Answer:**
Kafka is designed for distributed, high-throughput, low-latency applications and supports log-based persistence and stream processing natively. Traditional queues like JMS and RabbitMQ focus more on point-to-point and publish-subscribe messaging but may lack Kafka’s built-in scalability and partitioning features[^1][^2].

### Q14: How do you configure the number of partitions for a topic?

**Answer:**
The number of partitions for a topic can be set at topic creation and increased later using Kafka commands or APIs. Increasing partitions improves parallelism but may affect ordering guarantees[^1][^3].

### Q15: What is retention policy in Kafka?

**Answer:**
Retention policy determines how long Kafka retains messages in a topic, regardless of whether they have been consumed. It can be time-based (e.g., 7 days) or size-based (e.g., 100GB), configurable per topic[^2].

### Q16: Explain different types of Kafka consumers (High-level vs Low-level consumer).

**Answer:**

- **High-level consumer:** Manages group coordination, offset tracking, and partition assignments automatically.
- **Low-level consumer:** Offers more control, enabling manual offset management and partition assignment, suitable for custom or advanced use cases[^1][^2].


### Q17: What is commit strategy in Kafka consumers?

**Answer:**
Consumers commit offsets to indicate which messages have been processed. Strategies include:

- **Auto-commit:** Offset is committed periodically automatically.
- **Manual commit:** Application commits offset after processing, offering better control over processing and error handling[^1].


### Q18: How does load balancing work in Kafka?

**Answer:**
Load balancing is achieved through consumer groups: partitions are distributed among consumers in the group, and the Kafka coordinator ensures a balanced assignment. Producers distribute data to partitions using round-robin, keys, or custom partitioners[^1][^3].

## Performance \& Configuration

### Q19: What factors affect Kafka performance?

**Answer:**

- Number and size of partitions
- Broker hardware (CPU, RAM, Disk I/O)
- Network throughput
- Message size and batch size
- Replication factor and in-sync replicas
- Producer and consumer configurations[^1][^3]


### Q20: What is batch processing in Kafka producers?

**Answer:**
Producers can send messages in batches to reduce network overhead, improve throughput, and optimize performance. Batch size is configurable; larger batches mean higher latency but better throughput[^1].

### Q21: Explain different acknowledgment modes in Kafka (acks=0, acks=1, acks=all).

**Answer:**

- **acks=0:** Producer does not wait for acknowledgment.
- **acks=1:** Producer waits for the leader to acknowledge receipt.
- **acks=all:** Producer waits for all in-sync replicas to acknowledge, ensuring highest durability[^1].


### Q22: What is the role of ISR (In-Sync Replicas)?

**Answer:**
ISR is the set of replicas that are fully caught up with the leader. Only messages acknowledged by all ISRs are considered committed, ensuring durability and consistency if the leader fails[^1][^3].

### Q23: How do you monitor Kafka cluster health?

**Answer:**
By monitoring:

- Broker status
- Partition leader/follower assignments
- Consumer lag
- Under-replicated partitions
- Disk usage
- JVM metrics
- Custom alerts[^3]


## Scenario-Based Questions

### Q24: How would you handle message ordering in a multi-partition topic?

**Answer:**
By ensuring that all messages needing strict order use the same key, as Kafka guarantees order within a partition but not across partitions[^1][^2].

### Q25: What happens if a consumer in a consumer group fails?

**Answer:**
The broker reassigns that consumer’s partitions to other consumers in the group, ensuring continued processing and fault tolerance[^1][^3].

### Q26: How would you scale Kafka producers and consumers?

**Answer:**

- **Producers:** Can be scaled horizontally; more producers can publish in parallel.
- **Consumers:** Can add more consumers to a consumer group for parallel processing, limited by the number of partitions per topic[^1][^2][^3].


### Q27: How do you handle duplicate message processing?

**Answer:**
By designing consumers to be idempotent (handle reprocessing gracefully) and/or using Kafka’s *exactly-once semantics* to avoid duplicates[^1][^3].

### Q28: What would you do if Kafka brokers run out of disk space?

**Answer:**

- Increase disk space or add more brokers.
- Adjust retention policy to delete older data.
- Monitor and alert for disk usage trends[^3].


### Q29: How would you migrate from one Kafka cluster to another?

**Answer:**

- Use tools like MirrorMaker to replicate data.
- Ensure all topics/partitions/configurations are replicated.
- Switch producers and consumers to the new cluster once data is consistent[^2].


### Q30: How do you handle schema evolution in Kafka messages?

**Answer:**
Use schema registries (e.g., Confluent Schema Registry) to manage changes and ensure backward/forward compatibility between producers and consumers[^2].

## Configuration Questions

### Q31: Show basic Kafka producer configuration in Java.

```java
Properties props = new Properties();
props.put("bootstrap.servers", "localhost:9092");
props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");
KafkaProducer<String, String> producer = new KafkaProducer<>(props);
```


### Q32: Show basic Kafka consumer configuration in Java.

```java
Properties props = new Properties();
props.put("bootstrap.servers", "localhost:9092");
props.put("group.id", "test");
props.put("enable.auto.commit", "true");
props.put("key.serializer", "org.apache.kafka.common.serialization.StringDeserializer");
props.put("value.serializer", "org.apache.kafka.common.serialization.StringDeserializer");
KafkaConsumer<String, String> consumer = new KafkaConsumer<>(props);
```


### Q33: How do you create a topic using command line?

```bash
kafka-topics.sh --create --topic mytopic --bootstrap-server localhost:9092 --partitions 3 --replication-factor 2
```


### Q34: What are important broker-level configurations?

**Answer:**

- `broker.id`: Unique ID for each broker
- `log.dirs`: Directories for log storage
- `zookeeper.connect`: Zookeeper connection string
- `num.partitions`: Default number of partitions per topic
- `log.retention.hours`: Message retention time
- `default.replication.factor`: Default replication factor[^2].


## Advanced/Ecosystem Questions

### Q35: What is Kafka Connect and when would you use it?

**Answer:**
Kafka Connect is a tool for integrating Kafka with external systems like databases or files, using source and sink connectors for scalable and fault-tolerant data ingestion or export[^1][^3].

### Q36: What is Kafka Streams and how is it different from regular consumers?

**Answer:**
Kafka Streams is a client library for building applications that process and analyze data stored in Kafka. Unlike regular consumers, it provides higher-level abstractions for stream processing, stateful computations, and windowing[^2].

### Q37: Explain Kafka's log compaction feature.

**Answer:**
Log compaction retains the latest value for each key within a topic, allowing compacted topics to become efficient changelogs or caches while reducing storage needs[^3].

### Q38: What is KSQL and what problems does it solve?

**Answer:**
KSQL is a streaming SQL engine for Kafka enabling real-time, interactive queries over streams and tables without writing Java code[^2].

### Q39: How does Kafka integrate with big data tools like Spark, Hadoop?

**Answer:**
Kafka integrates via connectors and APIs, enabling Spark, Hadoop, and other tools to consume or produce data streams, supporting ETL and analytics scenarios[^1][^2].

### Q40: What are Kafka transactions?

**Answer:**
Kafka transactions ensure that groups of operations (produce/consume) are atomic and consistent across multiple partitions and topics, enabling exactly-once processing semantics in complex pipelines[^3].

## Quick-Fire \& Troubleshooting Questions

| Question | Answer |
| :-- | :-- |
| What is the default port for Kafka? | 9092 |
| Can you change the number of partitions for an existing topic? | Yes, can increase but not decrease |
| What happens to messages when all replicas are down? | Data loss can occur |
| What is the minimum number of brokers recommended for production? | At least 3 for fault tolerance |
| What is the role of controller broker in Kafka? | Manages partition leader elections and cluster metadata |

### Selected Troubleshooting Topics

- **Debugging slow consumer performance:** Monitor lag, thread pool, GC, network, and topic partitioning[^3].
- **UnderReplicatedPartitions alert:** Caused by broker outages or network issues where followers fall behind.
- **Handling OutOfMemoryError:** Adjust JVM heap, monitor memory leaks, and control message size/batch.
- **High consumer lag causes:** Slow processing, network latency, or misconfigured offsets.
- **Data corruption recovery:** Restore from backups or replicate from a healthy cluster[^3].

These answers are tailored for a backend Java developer with scenarios and sample Java code, following an interview Q\&A format to help you prepare for real-world Kafka interview panels[^1][^2][^3].

<div style="text-align: center">⁂</div>

[^1]: https://www.projectpro.io/article/kafka-interview-questions-and-answers/438

[^2]: https://gist.github.com/bansalankit92/9414ef3614229cdca6053464fedf5038

[^3]: https://www.simplilearn.com/kafka-interview-questions-and-answers-article

[^4]: https://github.com/Vicky1701/InterviewQuestions/blob/main/Kafka/MainQuestions.md

[^5]: https://www.geeksforgeeks.org/kafka-interview-questions/

[^6]: https://www.interviewbit.com/kafka-interview-questions/

[^7]: https://www.datacamp.com/blog/kafka-interview-questions

[^8]: https://www.reddit.com/r/Kafka/comments/1d311f1/25_apache_kafka_interview_questions_answers_for/

[^9]: https://www.youtube.com/watch?v=Q7tU0B1bnSE

