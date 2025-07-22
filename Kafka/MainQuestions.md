# Apache Kafka Interview Questions - Complete Study Guide

## 🔥 MUST-KNOW Questions (High Priority)

### Core Concepts
**Q1: What is Apache Kafka and what problem does it solve?**

**Q2: Explain the main components of Kafka architecture (Broker, Topic, Partition, Producer, Consumer).**

**Q3: What is a Kafka topic and what are partitions?**

**Q4: What is a Kafka broker and what role does it play?**

**Q5: Explain Producer and Consumer in Kafka.**

**Q6: What is a Consumer Group and how does it work?**

**Q7: What is offset in Kafka? How is it managed?**

**Q8: What is Zookeeper's role in Kafka?**

### Key Features
**Q9: How does Kafka ensure message ordering?**

**Q10: What are the delivery semantics in Kafka? (At least once, At most once, Exactly once)**

**Q11: How does Kafka achieve high availability and fault tolerance?**

**Q12: What is replication in Kafka? What are leader and follower replicas?**

---

## 💡 LIKELY Questions (Medium Priority)

### Technical Details
**Q13: What is the difference between Kafka and traditional message queues (JMS, RabbitMQ)?**

**Q14: How do you configure the number of partitions for a topic?**

**Q15: What is retention policy in Kafka?**

**Q16: Explain different types of Kafka consumers (High-level vs Low-level consumer).**

**Q17: What is commit strategy in Kafka consumers?**

**Q18: How does load balancing work in Kafka?**

### Performance & Configuration
**Q19: What factors affect Kafka performance?**

**Q20: What is batch processing in Kafka producers?**

**Q21: Explain different acknowledgment modes in Kafka (acks=0, acks=1, acks=all).**

**Q22: What is the role of ISR (In-Sync Replicas)?**

**Q23: How do you monitor Kafka cluster health?**

---

## 🎯 SCENARIO-BASED Questions (Medium Priority)

**Q24: How would you handle message ordering in a multi-partition topic?**

**Q25: What happens if a consumer in a consumer group fails?**

**Q26: How would you scale Kafka producers and consumers?**

**Q27: How do you handle duplicate message processing?**

**Q28: What would you do if Kafka brokers run out of disk space?**

**Q29: How would you migrate from one Kafka cluster to another?**

**Q30: How do you handle schema evolution in Kafka messages?**

---

## 📝 CONFIGURATION Questions (Be Ready)

**Q31: Show basic Kafka producer configuration in Java.**

**Q32: Show basic Kafka consumer configuration in Java.**

**Q33: How do you create a topic using command line?**

**Q34: What are important broker-level configurations?**

---

## 🔧 ADVANCED/ECOSYSTEM Questions (Lower Priority)

**Q35: What is Kafka Connect and when would you use it?**

**Q36: What is Kafka Streams and how is it different from regular consumers?**

**Q37: Explain Kafka's log compaction feature.**

**Q38: What is KSQL and what problems does it solve?**

**Q39: How does Kafka integrate with big data tools like Spark, Hadoop?**

**Q40: What are Kafka transactions?**

---

## ⚡ QUICK-FIRE Questions (Know the Answers)

**Q41: What is the default port for Kafka?**
*Answer: 9092*

**Q42: Can you change the number of partitions for an existing topic?**
*Answer: Yes, you can increase but not decrease*

**Q43: What happens to messages when all replicas are down?**
*Answer: Data loss can occur*

**Q44: What is the minimum number of brokers recommended for production?**
*Answer: At least 3 for fault tolerance*

**Q45: What is the role of controller broker in Kafka?**

---

## 🎪 TROUBLESHOOTING Questions (Important for Experience)

**Q46: How would you debug slow consumer performance?**

**Q47: What causes UnderReplicatedPartitions alert?**

**Q48: How do you handle OutOfMemoryError in Kafka?**

**Q49: What are common reasons for high consumer lag?**

**Q50: How do you recover from data corruption in Kafka?**

---

## 📋 PRIORITY STUDY ORDER

### Week 1 (Essential - Focus Here First)
- Q1-Q12 (Must-know core concepts)
- Understand: Topics, Partitions, Producers, Consumers, Consumer Groups
- Learn basic architecture and why Kafka is used

### Week 2 (If You Have More Time)
- Q13-Q23 (Technical details and configuration)
- Q24-Q30 (Scenario-based questions)
- Practice basic producer/consumer code

### Additional Time
- Q31-Q50 (Configuration, advanced topics, troubleshooting)

---

## 🚀 INTERVIEW SURVIVAL TIPS

### Core Concepts You Must Explain Clearly:
1. **What Kafka is**: Distributed streaming platform for real-time data
2. **Topics & Partitions**: How data is organized and distributed
3. **Producer/Consumer**: How applications interact with Kafka
4. **Consumer Groups**: How consumers work together for scalability
5. **Replication**: How Kafka ensures fault tolerance

### Safe Answers to Have Ready:
- "Kafka is a distributed streaming platform used for building real-time data pipelines"
- "I understand Kafka's pub-sub model and how it differs from traditional message queues"
- "I know the importance of partitioning for scalability and parallelism"
- "I'm familiar with producer-consumer patterns and consumer group concepts"

### When Asked About Experience:
- "I understand Kafka architecture and have studied producer-consumer implementations"
- "I'm familiar with Kafka concepts and ready to work with it in a production environment"
- "I've been learning Kafka fundamentals and understand its use cases in microservices"

---

## 🎯 MINIMUM VIABLE PREPARATION

**If you have LIMITED time, focus ONLY on these 12 questions:**
Q1, Q2, Q3, Q4, Q5, Q6, Q7, Q9, Q10, Q11, Q12, Q13

**Master these core concepts and you can handle 70% of Kafka interviews!**

---

## 📚 QUICK REFERENCE - KEY DEFINITIONS

**Topic**: Category/feed name where records are published
**Partition**: Ordered, immutable sequence of records within a topic
**Broker**: Kafka server that stores data and serves client requests
**Producer**: Application that publishes records to topics
**Consumer**: Application that subscribes to topics and processes records
**Consumer Group**: Set of consumers that work together to consume a topic
**Offset**: Unique identifier of a record within a partition
**Replication Factor**: Number of copies of data across brokers
**Leader**: Primary replica that handles all reads and writes
**Follower**: Backup replica that replicates data from leader

---

## 🔑 ESSENTIAL USE CASES TO MENTION

1. **Real-time Analytics**: Processing streaming data for insights
2. **Event Sourcing**: Storing state changes as events
3. **Log Aggregation**: Collecting logs from multiple services
4. **Microservices Communication**: Decoupling services via events
5. **Data Pipeline**: Moving data between different systems
6. **Activity Tracking**: User behavior tracking and analytics

---

## ⚠️ COMMON PITFALLS TO AVOID

- Don't confuse Kafka with traditional message queues
- Remember that increasing partitions is possible, but decreasing is not
- Understand that message order is guaranteed only within a partition
- Know that Zookeeper is being phased out in favor of KRaft
- Remember that consumers in same group cannot read same partition simultaneously
