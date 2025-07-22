# Kafka Spring Boot Interview Answers - MobiLytix Loyalty Platform

## Beginner Level Questions & Answers

### 1. What is Apache Kafka and what are its main use cases?

**Answer:** 
Apache Kafka is a distributed streaming platform that handles real-time data feeds. In our MobiLytix Loyalty Platform, we use Kafka for several critical use cases:

- **Event-driven architecture**: When a member earns points through purchases, we publish events to Kafka
- **Microservice communication**: Our Wallet Service publishes point balance updates that the Notification Service consumes to send SMS/email alerts
- **Real-time analytics**: Transaction events from the Transaction Layer are streamed to our Analytics Layer for real-time dashboard updates
- **Audit logging**: All loyalty transactions are streamed to ensure complete audit trails

The main benefits we get are high throughput, fault tolerance, and the ability to handle millions of loyalty transactions per day.

### 2. Explain the key components of Kafka architecture

**Answer:**
Let me explain using our loyalty platform:

- **Producer**: Our Wallet Service acts as a producer when it publishes "points-earned" or "points-redeemed" events
- **Consumer**: Our Notification Service consumes these events to send member notifications
- **Broker**: The Kafka servers that store our event data - we run a 3-node cluster for high availability
- **Topic**: We have topics like "member-transactions", "referral-events", "gamification-updates"
- **Partition**: Each topic is partitioned - for example, "member-transactions" is partitioned by member-id to ensure order per member

### 3. What is the difference between a topic and a partition in Kafka?

**Answer:**
Think of it like this in our loyalty system:
- **Topic** is like a category - "member-transactions" topic contains all loyalty transactions
- **Partition** is like a subcategory within that topic - we partition by member-id so all transactions for member "12345" go to the same partition

This ensures that for each member, their transactions are processed in order. We typically use 6 partitions for our main topics to balance load across our consumer instances.

### 4. What is a consumer group and how does it work?

**Answer:**
In our platform, we use consumer groups for scalability. For example:
- Consumer Group "notification-processors" has 3 instances of our Notification Service
- Each instance processes different partitions of the "member-transactions" topic
- If one instance fails, Kafka automatically redistributes its partitions to the remaining instances

This gives us both scalability and fault tolerance. Each message is processed by only one consumer in the group, preventing duplicate notifications to members.

### 5. What is offset in Kafka and how is it managed?

**Answer:**
Offset is like a bookmark that tells us which message was last processed. In our Wallet Service consumer:
- When we process a "points-earned" event, Kafka tracks that we've processed up to offset 1000
- If our service restarts, it continues from offset 1001, not from the beginning
- We use auto-commit every 5 seconds, but for critical transactions, we manually commit after successful database updates

### 6. What are the different delivery semantics in Kafka?

**Answer:**
In our loyalty platform, we use different semantics for different services:

- **At-least-once**: For notifications - it's okay if a member gets a duplicate SMS about points earned
- **At-most-once**: For promotional messages - we don't want to spam members
- **Exactly-once**: For wallet transactions - critical that points are credited exactly once

We achieve exactly-once for wallet operations using Kafka transactions with our database commits.

### 7. How do you configure Kafka in a Spring Boot application?

**Answer:**
In our application.yml:

```yaml
spring:
  kafka:
    bootstrap-servers: kafka1:9092,kafka2:9092,kafka3:9092
    producer:
      key-serializer: org.apache.kafka.common.serialization.StringSerializer
      value-serializer: org.springframework.kafka.support.serializer.JsonSerializer
    consumer:
      group-id: wallet-service
      key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
      value-deserializer: org.springframework.kafka.support.serializer.JsonDeserializer
      properties:
        spring.json.trusted.packages: com.mobilytix.events
```

We also have separate configurations for different environments (dev, staging, prod).

### 8. What annotations are used for Kafka consumers in Spring Boot?

**Answer:**
In our services, we primarily use:

```java
@KafkaListener(topics = "member-transactions", groupId = "notification-service")
public void handleTransaction(MemberTransaction transaction) {
    // Send notification logic
}

@RetryableTopic(attempts = "3", 
                backoff = @Backoff(delay = 1000, multiplier = 2.0))
@KafkaListener(topics = "wallet-updates")
public void processWalletUpdate(WalletEvent event) {
    // Critical wallet processing with retry
}
```

### 9. How do you create a Kafka producer in Spring Boot?

**Answer:**
In our Wallet Service:

```java
@Service
public class TransactionEventPublisher {
    
    @Autowired
    private KafkaTemplate<String, Object> kafkaTemplate;
    
    public void publishPointsEarned(String memberId, int points) {
        PointsEarnedEvent event = new PointsEarnedEvent(memberId, points, LocalDateTime.now());
        kafkaTemplate.send("member-transactions", memberId, event);
    }
}
```

We use the member ID as the key to ensure all events for a member go to the same partition.

### 10. What is KafkaTemplate and how do you use it?

**Answer:**
KafkaTemplate is Spring's abstraction for sending messages. In our loyalty platform:

```java
// Synchronous send for critical operations
kafkaTemplate.send("wallet-updates", walletEvent).get();

// Asynchronous send for notifications
kafkaTemplate.send("notifications", notification)
    .addCallback(
        result -> log.info("Notification sent successfully"),
        failure -> log.error("Failed to send notification", failure)
    );
```

We use sync for wallet operations and async for notifications to balance performance and reliability.

## Intermediate Level Questions & Answers

### 11. Explain Kafka's log compaction feature and when you would use it

**Answer:**
We use log compaction for our "member-profile-updates" topic. Instead of keeping all profile changes, Kafka keeps only the latest state for each member.

For example, if member "12345" updates their email 5 times, we only need the latest email address, not all 5 changes. This is perfect for maintaining current member state across services while saving storage.

### 12. What is the difference between acks=0, acks=1, and acks=all?

**Answer:**
In our platform, we use different ack settings:

- **acks=0**: For analytics events - we can afford to lose some data for better performance
- **acks=1**: For notification events - good balance of performance and reliability
- **acks=all**: For wallet transactions - we need guarantee that all replicas have the data before considering it successful

For our core wallet operations, we use acks=all with min.insync.replicas=2 to ensure data safety.

### 13. How does Kafka ensure fault tolerance and high availability?

**Answer:**
In our MobiLytix setup:

- **Replication Factor 3**: Each message is stored on 3 brokers
- **Multiple Broker Cluster**: If one broker fails, others continue serving
- **Leader Election**: If a partition leader fails, one of the followers becomes the new leader
- **Consumer Group Rebalancing**: If a consumer instance fails, others pick up its partitions

We monitor this using Prometheus metrics and have alerts for broker failures.

### 14. How do you implement custom serializers in Spring Kafka?

**Answer:**
For our loyalty events, we created custom serializers:

```java
public class LoyaltyEventSerializer implements Serializer<LoyaltyEvent> {
    private ObjectMapper objectMapper = new ObjectMapper();
    
    @Override
    public byte[] serialize(String topic, LoyaltyEvent data) {
        try {
            return objectMapper.writeValueAsBytes(data);
        } catch (JsonProcessingException e) {
            throw new SerializationException("Error serializing LoyaltyEvent", e);
        }
    }
}
```

This gives us control over the serialization format and allows us to add custom headers for tracking.

### 15. Explain error handling strategies in Spring Kafka consumers

**Answer:**
In our platform, we have a multi-layered approach:

```java
@RetryableTopic(
    attempts = "3",
    backoff = @Backoff(delay = 1000, multiplier = 2.0),
    dltStrategy = DltStrategy.FAIL_ON_ERROR
)
@KafkaListener(topics = "wallet-transactions")
public void processWalletTransaction(WalletTransaction transaction) {
    try {
        walletService.processTransaction(transaction);
    } catch (BusinessException e) {
        // Log and send to DLT
        throw e;
    }
}

@DltHandler
public void handleDlt(WalletTransaction transaction, Exception exception) {
    // Send alert to operations team
    alertService.sendCriticalAlert("Failed wallet transaction", transaction);
}
```

### 16. How do you handle duplicate message processing?

**Answer:**
We use idempotency keys in our loyalty system:

```java
@KafkaListener(topics = "points-transactions")
public void processPointsTransaction(PointsTransaction transaction) {
    String idempotencyKey = transaction.getTransactionId();
    
    if (transactionRepository.existsByIdempotencyKey(idempotencyKey)) {
        log.info("Duplicate transaction detected: {}", idempotencyKey);
        return; // Skip processing
    }
    
    // Process transaction and save with idempotency key
    walletService.creditPoints(transaction);
    transactionRepository.save(transaction);
}
```

### 17. How do you implement exactly-once processing?

**Answer:**
For our wallet operations, we use Kafka transactions:

```java
@Transactional
@KafkaListener(topics = "wallet-credits")
public void creditWalletPoints(WalletCreditEvent event) {
    // Database transaction + Kafka transaction
    walletRepository.creditPoints(event.getMemberId(), event.getPoints());
    
    // Publish success event in same transaction
    kafkaTemplate.send("wallet-confirmations", new WalletCreditedEvent(event));
    
    // Both DB and Kafka operations succeed or fail together
}
```

We enable transactions in our producer configuration with `spring.kafka.producer.transaction-id-prefix=wallet-tx-`.

### 18. How do you monitor Kafka performance in Spring Boot?

**Answer:**
We use multiple monitoring approaches:

```java
@Component
public class KafkaMetrics {
    private final MeterRegistry meterRegistry;
    
    @EventListener
    public void onConsumerLag(ConsumerRecordLag lagEvent) {
        meterRegistry.gauge("kafka.consumer.lag", 
                          Tags.of("topic", lagEvent.getTopic()), 
                          lagEvent.getLag());
    }
}
```

We also use:
- **Micrometer metrics**: Exposed to Prometheus for alerting
- **Spring Boot Actuator**: Health checks for Kafka connectivity
- **Custom dashboards**: Grafana dashboards showing consumer lag, throughput, error rates

### 19. How would you handle consumer lag?

**Answer:**
In our loyalty platform, we've faced this with notification processing:

**Diagnosis steps:**
1. Check consumer group lag using `kafka-consumer-groups.sh`
2. Monitor consumer metrics in Grafana
3. Check if consumers are stuck on poison messages

**Solutions we implemented:**
- **Increased consumer instances**: Scaled from 2 to 4 notification service instances
- **Parallel processing**: Used `@KafkaListener(concurrency = "3")` for CPU-intensive tasks
- **Batch processing**: Process notifications in batches during off-peak hours
- **Topic partitioning**: Increased partitions from 3 to 6 for better parallelism

### 20. How do you handle ordering guarantees in Kafka?

**Answer:**
In our system, we ensure order per member:

```java
// Producer side - use member ID as key
public void publishMemberEvent(String memberId, MemberEvent event) {
    kafkaTemplate.send("member-events", memberId, event);
    // All events for same member go to same partition
}

// Consumer side - single threaded processing per partition
@KafkaListener(topics = "member-events", concurrency = "1")
public void processMemberEvent(MemberEvent event) {
    // Processes events in order per partition (per member)
}
```

This ensures that for member "12345", events like "points-earned" → "level-upgraded" → "notification-sent" happen in order.

### 21. Real-world Scenario: High-throughput with message ordering

**Answer:**
In our loyalty platform, during flash sales, we need to:

1. **Partition Strategy**: Partition by member-id to ensure per-member ordering
2. **Producer Configuration**: 
   - `batch.size=65536` for throughput
   - `linger.ms=10` to batch messages
   - `compression.type=snappy` to reduce network load
3. **Consumer Configuration**:
   - `max.poll.records=1000` to process in batches
   - `fetch.min.bytes=1024` to reduce network calls
4. **Scaling**: Auto-scale consumer instances based on lag metrics

This handles 50K+ transactions per minute while maintaining order per member.

### 22. How would you debug a consumer that's not receiving messages?

**Answer:**
When our Gamification Service stopped receiving badge updates, I followed this process:

1. **Check Consumer Group Status**:
   ```bash
   kafka-consumer-groups.sh --describe --group gamification-service
   ```

2. **Verify Topic and Partitions**:
   ```bash
   kafka-topics.sh --describe --topic badge-updates
   ```

3. **Check Consumer Configuration**:
   - Verified `auto.offset.reset=earliest`
   - Confirmed consumer group ID matches

4. **Application Logs**:
   - Found deserializer errors due to schema changes
   - Fixed by updating trusted packages configuration

5. **Network Connectivity**: Verified broker connectivity from consumer

The issue was a schema change that broke deserialization - we fixed it by updating the JsonDeserializer configuration.

### 23. How do you implement circuit breaker pattern with Kafka consumers?

**Answer:**
In our system, when external APIs (like SMS service) are down:

```java
@Component
public class NotificationConsumer {
    
    @Retryable(value = {ExternalServiceException.class}, maxAttempts = 3)
    @KafkaListener(topics = "notifications")
    public void sendNotification(NotificationEvent event) {
        try {
            externalSmsService.sendSms(event);
        } catch (ServiceUnavailableException e) {
            // Circuit breaker opens, messages go to fallback
            throw new ExternalServiceException(e);
        }
    }
    
    @Recover
    public void fallbackNotification(ExternalServiceException ex, NotificationEvent event) {
        // Store in database for later retry
        failedNotificationRepository.save(event);
    }
}
```

We also use Resilience4j circuit breaker for external service calls within consumers.

### 24. How would you migrate from one Kafka cluster to another?

**Answer:**
We did this when moving to a new data center:

**Phase 1 - Setup**:
- Set up new Kafka cluster
- Configure MirrorMaker 2.0 to replicate topics

**Phase 2 - Dual Write**:
- Updated producers to write to both clusters
- Consumers still read from old cluster

**Phase 3 - Consumer Migration**:
- Gradually moved consumer groups to new cluster
- Monitored lag and performance

**Phase 4 - Cleanup**:
- Stopped producers writing to old cluster
- Verified all consumers on new cluster
- Decommissioned old cluster

This gave us zero-downtime migration for our loyalty platform.

### 25. How do you handle schema evolution in Kafka messages?

**Answer:**
In our loyalty platform, we use a versioning strategy:

```java
@JsonTypeInfo(use = JsonTypeInfo.Id.NAME, property = "eventVersion")
@JsonSubTypes({
    @JsonSubTypes.Type(value = MemberEventV1.class, name = "v1"),
    @JsonSubTypes.Type(value = MemberEventV2.class, name = "v2")
})
public abstract class MemberEvent {
    public abstract String getVersion();
}
```

**Migration Strategy**:
1. **Backward Compatibility**: New consumers handle both v1 and v2
2. **Forward Compatibility**: Old consumers ignore unknown fields
3. **Gradual Rollout**: Deploy consumers first, then producers
4. **Version Monitoring**: Track which versions are being produced/consumed

This allows us to evolve our loyalty events without breaking existing consumers.

## Pro Tips for Interview Success

### 1. Always relate to your project
When answering, always connect back to MobiLytix platform with concrete examples.

### 2. Mention monitoring and operations
Show you think about production concerns: "We monitor this using Prometheus and have alerts set up."

### 3. Discuss trade-offs
"We chose acks=all for wallet transactions but acks=1 for notifications because..."

### 4. Show problem-solving skills
"When we faced consumer lag during flash sales, we diagnosed it by... and solved it by..."

### 5. Demonstrate understanding of business impact
"This ensures our members never see duplicate loyalty points, maintaining trust in our platform."

Remember: Interviewers want to see that you can apply Kafka knowledge to solve real business problems, not just recite theory!
