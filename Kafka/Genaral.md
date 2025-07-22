# Kafka Spring Boot Interview Questions & Answers Guide

## Beginner Level Questions & Answers

### 1. What is Apache Kafka and what are its main use cases?

**Answer:**
Apache Kafka is a distributed streaming platform designed for high-throughput, fault-tolerant, real-time data streaming. Think of it as a distributed commit log where data is stored in an ordered, immutable sequence.

**Key characteristics:**
- **Distributed**: Runs on multiple servers for fault tolerance
- **Persistent**: Data is stored on disk and replicated
- **High-throughput**: Can handle millions of messages per second
- **Low-latency**: Messages can be processed in near real-time

**Main use cases:**
- **Real-time analytics**: Processing streaming data for immediate insights
- **Event sourcing**: Storing all changes as events for audit trails
- **Log aggregation**: Collecting logs from multiple services
- **Microservices communication**: Decoupling services through async messaging
- **Data integration**: Moving data between systems (ETL pipelines)

**Example scenario:** An e-commerce platform uses Kafka to track user clicks, process orders, update inventory, and send notifications - all in real-time.

### 2. Explain the key components of Kafka architecture

**Answer:**
Kafka has several key components that work together:

**Producer:**
- Applications that publish/send messages to Kafka topics
- Responsible for choosing which partition to send data to
- Can send data synchronously or asynchronously

**Consumer:**
- Applications that read/subscribe to messages from topics
- Pull messages from Kafka (not pushed)
- Track their position using offsets

**Broker:**
- Kafka servers that store and serve data
- Handle read/write requests from producers and consumers
- Manage data replication and persistence

**Topic:**
- Named feed or category where messages are published
- Like a table in a database or a folder in a filesystem
- Divided into partitions for scalability

**Partition:**
- Ordered, immutable sequence of messages within a topic
- Each message gets a sequential offset number
- Enables parallel processing and scalability

**Analogy:** Think of Kafka like a newspaper system:
- Topics are different newspaper sections (Sports, Business, etc.)
- Partitions are different printing presses for each section
- Producers are journalists writing articles
- Consumers are readers subscribing to specific sections

### 3. What is the difference between a topic and a partition in Kafka?

**Answer:**
This is a fundamental concept in Kafka architecture:

**Topic:**
- Logical grouping of related messages
- Like a category or channel
- Example: "user-orders", "payment-events", "email-notifications"

**Partition:**
- Physical division of a topic
- Each topic can have multiple partitions (1 to many)
- Messages within a partition are ordered by offset
- Different partitions can be on different brokers

**Key differences:**

| Topic | Partition |
|-------|-----------|
| Logical concept | Physical storage unit |
| Contains related messages | Contains ordered sequence of messages |
| Has multiple partitions | Belongs to one topic |
| Identified by name | Identified by topic + partition number |

**Example:**
```
Topic: "user-orders" (has 3 partitions)
├── Partition 0: [order1, order4, order7, ...]
├── Partition 1: [order2, order5, order8, ...]
└── Partition 2: [order3, order6, order9, ...]
```

**Why partitions matter:**
- **Parallelism**: Multiple consumers can read different partitions simultaneously
- **Scalability**: Distribute load across multiple brokers
- **Ordering**: Messages within a partition maintain order

### 4. What is a consumer group and how does it work?

**Answer:**
A consumer group is a set of consumers that work together to consume messages from a topic. It's Kafka's way of implementing load balancing and fault tolerance.

**Key concepts:**

**Consumer Group ID:**
- All consumers in a group share the same `group.id`
- Each group maintains its own offset for each partition

**Partition Assignment:**
- Each partition is consumed by only ONE consumer within a group
- If you have 3 partitions and 2 consumers, one consumer gets 2 partitions
- If you have 3 partitions and 4 consumers, one consumer will be idle

**Rebalancing:**
- When consumers join/leave, Kafka reassigns partitions
- Ensures all partitions are being consumed

**Example scenario:**
```
Topic: "orders" with 4 partitions
Consumer Group: "order-processors" with 2 consumers

Assignment:
Consumer-1: Partitions 0, 1
Consumer-2: Partitions 2, 3

If Consumer-3 joins:
Consumer-1: Partition 0
Consumer-2: Partition 1  
Consumer-3: Partitions 2, 3
```

**Benefits:**
- **Load balancing**: Work is distributed among consumers
- **Fault tolerance**: If one consumer fails, others take over its partitions
- **Scalability**: Add more consumers to handle more load

### 5. What is offset in Kafka and how is it managed?

**Answer:**
An offset is a unique identifier for each message within a partition. Think of it as a bookmark that tells you where you are in reading a book.

**Key characteristics:**
- **Sequential numbers**: 0, 1, 2, 3, ... for each partition
- **Immutable**: Once assigned, never changes
- **Per partition**: Each partition has its own offset sequence
- **Consumer tracking**: Each consumer group tracks its current offset

**Offset management strategies:**

**1. Automatic (Default):**
- Kafka automatically commits offsets every 5 seconds
- Configuration: `enable.auto.commit=true`
- Risk: May lose messages or process duplicates during failures

**2. Manual:**
- Your application explicitly commits offsets
- Configuration: `enable.auto.commit=false`
- More control but requires careful handling

**Example:**
```java
// Automatic offset management
@KafkaListener(topics = "orders")
public void processOrder(Order order) {
    // Process the order
    // Offset is committed automatically
}

// Manual offset management
@KafkaListener(topics = "orders")
public void processOrder(Order order, Acknowledgment ack) {
    try {
        // Process the order
        processOrderLogic(order);
        // Manually commit offset
        ack.acknowledge();
    } catch (Exception e) {
        // Don't commit on error - message will be reprocessed
    }
}
```

**Offset storage:**
- **Kafka 0.9+**: Stored in special `__consumer_offsets` topic
- **Earlier versions**: Stored in Zookeeper

### 6. How do you configure Kafka in a Spring Boot application?

**Answer:**
There are multiple ways to configure Kafka in Spring Boot, from simple to advanced:

**1. Using application.properties (Simplest):**
```properties
# Basic producer configuration
spring.kafka.producer.bootstrap-servers=localhost:9092
spring.kafka.producer.key-serializer=org.apache.kafka.common.serialization.StringSerializer
spring.kafka.producer.value-serializer=org.springframework.kafka.support.serializer.JsonSerializer

# Basic consumer configuration
spring.kafka.consumer.bootstrap-servers=localhost:9092
spring.kafka.consumer.group-id=my-group
spring.kafka.consumer.key-deserializer=org.apache.kafka.common.serialization.StringDeserializer
spring.kafka.consumer.value-deserializer=org.springframework.kafka.support.serializer.JsonDeserializer
spring.kafka.consumer.properties.spring.json.trusted.packages=com.example.model
```

**2. Using @Configuration class (More control):**
```java
@Configuration
@EnableKafka
public class KafkaConfig {

    @Bean
    public ProducerFactory<String, Object> producerFactory() {
        Map<String, Object> configProps = new HashMap<>();
        configProps.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        configProps.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
        configProps.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, JsonSerializer.class);
        return new DefaultKafkaProducerFactory<>(configProps);
    }

    @Bean
    public KafkaTemplate<String, Object> kafkaTemplate() {
        return new KafkaTemplate<>(producerFactory());
    }

    @Bean
    public ConsumerFactory<String, Object> consumerFactory() {
        Map<String, Object> configProps = new HashMap<>();
        configProps.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        configProps.put(ConsumerConfig.GROUP_ID_CONFIG, "my-group");
        configProps.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
        configProps.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, JsonDeserializer.class);
        configProps.put(JsonDeserializer.TRUSTED_PACKAGES, "com.example.model");
        return new DefaultKafkaConsumerFactory<>(configProps);
    }

    @Bean
    public ConcurrentKafkaListenerContainerFactory<String, Object> kafkaListenerContainerFactory() {
        ConcurrentKafkaListenerContainerFactory<String, Object> factory = new ConcurrentKafkaListenerContainerFactory<>();
        factory.setConsumerFactory(consumerFactory());
        return factory;
    }
}
```

**3. Adding required dependencies:**
```xml
<dependency>
    <groupId>org.springframework.kafka</groupId>
    <artifactId>spring-kafka</artifactId>
</dependency>
```

**Key configuration points:**
- **Bootstrap servers**: List of Kafka broker addresses
- **Serializers/Deserializers**: How to convert objects to/from bytes
- **Group ID**: Consumer group identification
- **Trusted packages**: For security when deserializing JSON

### 7. What is KafkaTemplate and how do you use it?

**Answer:**
KafkaTemplate is Spring Kafka's central class for producing messages. It's similar to JdbcTemplate or RestTemplate - it provides a high-level abstraction for Kafka operations.

**Key features:**
- **Synchronous and Asynchronous sending**
- **Automatic serialization**
- **Transaction support**
- **Error handling**
- **Metrics and monitoring**

**Basic usage examples:**

**1. Simple message sending:**
```java
@Service
public class OrderService {
    
    @Autowired
    private KafkaTemplate<String, Object> kafkaTemplate;
    
    public void sendOrder(Order order) {
        // Send to default topic
        kafkaTemplate.send("orders", order);
        
        // Send with specific key
        kafkaTemplate.send("orders", order.getId(), order);
        
        // Send to specific partition
        kafkaTemplate.send("orders", 0, order.getId(), order);
    }
}
```

**2. Asynchronous sending with callback:**
```java
public void sendOrderAsync(Order order) {
    ListenableFuture<SendResult<String, Object>> future = 
        kafkaTemplate.send("orders", order);
    
    future.addCallback(new ListenableFutureCallback<SendResult<String, Object>>() {
        @Override
        public void onSuccess(SendResult<String, Object> result) {
            System.out.println("Sent message=[" + order + 
                "] with offset=[" + result.getRecordMetadata().offset() + "]");
        }
        
        @Override
        public void onFailure(Throwable ex) {
            System.out.println("Unable to send message=[" + order + "] due to : " + ex.getMessage());
        }
    });
}
```

**3. With headers:**
```java
public void sendWithHeaders(Order order) {
    ProducerRecord<String, Object> record = new ProducerRecord<>("orders", order);
    record.headers().add("source", "order-service".getBytes());
    record.headers().add("timestamp", String.valueOf(System.currentTimeMillis()).getBytes());
    
    kafkaTemplate.send(record);
}
```

**Benefits:**
- **Simplified API**: No need to manage low-level producer details
- **Spring Integration**: Works seamlessly with Spring's transaction management
- **Error handling**: Built-in retry and error handling mechanisms

## Intermediate Level Questions & Answers

### 1. Explain Kafka's log compaction feature and when you would use it

**Answer:**
Log compaction is a cleanup policy that ensures Kafka retains the latest value for each key in a topic. Instead of deleting old messages based on time or size, it keeps only the most recent message for each unique key.

**How it works:**
- Kafka maintains two segments: active and compacted
- During compaction, Kafka scans older segments
- For each key, only the latest value is kept
- Deleted records (null values) eventually get removed

**Configuration:**
```properties
# Enable log compaction
log.cleanup.policy=compact

# Minimum time before a message can be compacted
log.min.cleanable.dirty.ratio=0.5

# How often compaction runs
log.cleaner.min.compaction.lag.ms=0
```

**Use cases:**

**1. User Profile Management:**
```java
// Only the latest profile for each user is needed
Key: "user123" -> Value: {"name": "John", "email": "john@example.com", "age": 30}
Key: "user123" -> Value: {"name": "John", "email": "john.doe@example.com", "age": 31}
// After compaction, only the second message remains
```

**2. Configuration Management:**
```java
// Application configurations where only latest matters
Key: "database.url" -> Value: "jdbc:mysql://old-server:3306/db"
Key: "database.url" -> Value: "jdbc:mysql://new-server:3306/db"
// Only the latest configuration is retained
```

**3. State Snapshots:**
- Account balances (only latest balance matters)
- Inventory levels
- User preferences

**Benefits:**
- **Storage efficiency**: Reduces disk usage
- **Faster recovery**: Less data to replay during startup
- **State management**: Perfect for maintaining current state

**Important considerations:**
- Messages without keys cannot be compacted
- Compaction is not immediate - it's a background process
- Order within a partition is maintained

### 2. What is the difference between acks=0, acks=1, and acks=all in producer configuration?

**Answer:**
The `acks` configuration controls how many broker acknowledgments the producer requires before considering a request complete. This directly impacts durability vs. performance trade-offs.

**acks=0 (Fire and Forget):**
- Producer doesn't wait for any acknowledgment
- Highest performance, lowest durability
- Messages can be lost if broker fails

```java
props.put(ProducerConfig.ACKS_CONFIG, "0");
```

**Use case:** High-volume, non-critical data like metrics or logs where some loss is acceptable

**acks=1 (Leader Acknowledgment):**
- Producer waits for leader broker acknowledgment only
- Balanced performance and durability
- Messages can be lost if leader fails before replication

```java
props.put(ProducerConfig.ACKS_CONFIG, "1");
```

**Use case:** Most common setting for general-purpose messaging

**acks=all or acks=-1 (Full ISR Acknowledgment):**
- Producer waits for acknowledgment from all in-sync replicas (ISR)
- Highest durability, lower performance
- No message loss as long as one ISR remains

```java
props.put(ProducerConfig.ACKS_CONFIG, "all");
// Usually combined with other settings for exactly-once
props.put(ProducerConfig.ENABLE_IDEMPOTENCE_CONFIG, true);
props.put(ProducerConfig.RETRIES_CONFIG, Integer.MAX_VALUE);
```

**Comparison table:**

| Setting | Durability | Performance | Use Case |
|---------|------------|-------------|----------|
| acks=0 | Lowest | Highest | Metrics, logs |
| acks=1 | Medium | Medium | General messaging |
| acks=all | Highest | Lowest | Financial transactions |

**Real-world example:**
```java
@Configuration
public class KafkaProducerConfig {
    
    // High-throughput, loss-tolerant
    @Bean("metricsProducer")
    public ProducerFactory<String, Object> metricsProducerFactory() {
        Map<String, Object> props = new HashMap<>();
        props.put(ProducerConfig.ACKS_CONFIG, "0");
        props.put(ProducerConfig.BATCH_SIZE_CONFIG, 16384);
        return new DefaultKafkaProducerFactory<>(props);
    }
    
    // Critical data, no loss acceptable
    @Bean("transactionProducer")
    public ProducerFactory<String, Object> transactionProducerFactory() {
        Map<String, Object> props = new HashMap<>();
        props.put(ProducerConfig.ACKS_CONFIG, "all");
        props.put(ProducerConfig.ENABLE_IDEMPOTENCE_CONFIG, true);
        props.put(ProducerConfig.RETRIES_CONFIG, Integer.MAX_VALUE);
        return new DefaultKafkaProducerFactory<>(props);
    }
}
```

### 3. How do you implement custom serializers and deserializers in Spring Kafka?

**Answer:**
Custom serializers and deserializers are needed when working with complex objects or when you need specific serialization logic beyond the built-in options.

**Custom Serializer Example:**

```java
public class OrderSerializer implements Serializer<Order> {
    
    private ObjectMapper objectMapper = new ObjectMapper();
    
    @Override
    public void configure(Map<String, ?> configs, boolean isKey) {
        // Configuration logic if needed
    }
    
    @Override
    public byte[] serialize(String topic, Order order) {
        try {
            if (order == null) {
                return null;
            }
            return objectMapper.writeValueAsBytes(order);
        } catch (Exception e) {
            throw new SerializationException("Error when serializing Order to byte[]", e);
        }
    }
    
    @Override
    public void close() {
        // Cleanup resources
    }
}
```

**Custom Deserializer Example:**

```java
public class OrderDeserializer implements Deserializer<Order> {
    
    private ObjectMapper objectMapper = new ObjectMapper();
    
    @Override
    public void configure(Map<String, ?> configs, boolean isKey) {
        // Configuration logic
    }
    
    @Override
    public Order deserialize(String topic, byte[] data) {
        try {
            if (data == null) {
                return null;
            }
            return objectMapper.readValue(data, Order.class);
        } catch (Exception e) {
            throw new SerializationException("Error when deserializing byte[] to Order", e);
        }
    }
    
    @Override
    public void close() {
        // Cleanup resources
    }
}
```

**Configuration in Spring Boot:**

```java
@Configuration
public class KafkaConfig {
    
    @Bean
    public ProducerFactory<String, Order> orderProducerFactory() {
        Map<String, Object> configProps = new HashMap<>();
        configProps.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        configProps.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
        configProps.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, OrderSerializer.class);
        return new DefaultKafkaProducerFactory<>(configProps);
    }
    
    @Bean
    public ConsumerFactory<String, Order> orderConsumerFactory() {
        Map<String, Object> configProps = new HashMap<>();
        configProps.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        configProps.put(ConsumerConfig.GROUP_ID_CONFIG, "order-group");
        configProps.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
        configProps.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, OrderDeserializer.class);
        return new DefaultKafkaConsumerFactory<>(configProps);
    }
}
```

**Advanced example with Avro:**

```java
@Component
public class AvroOrderSerializer implements Serializer<Order> {
    
    private KafkaAvroSerializer avroSerializer;
    
    @Override
    public void configure(Map<String, ?> configs, boolean isKey) {
        avroSerializer = new KafkaAvroSerializer();
        avroSerializer.configure(configs, isKey);
    }
    
    @Override
    public byte[] serialize(String topic, Order order) {
        return avroSerializer.serialize(topic, convertToAvro(order));
    }
    
    private GenericRecord convertToAvro(Order order) {
        // Convert Order POJO to Avro GenericRecord
        Schema schema = getOrderSchema();
        GenericRecord record = new GenericData.Record(schema);
        record.put("orderId", order.getId());
        record.put("customerId", order.getCustomerId());
        record.put("amount", order.getAmount());
        return record;
    }
}
```

**When to use custom serializers:**
- **Schema evolution**: Need to handle backward/forward compatibility
- **Performance optimization**: Custom binary formats
- **Encryption**: Encrypt sensitive data before sending
- **Compression**: Custom compression algorithms
- **Legacy systems**: Integration with existing message formats

### 4. Explain error handling strategies in Spring Kafka consumers

**Answer:**
Error handling in Kafka consumers is crucial for building resilient applications. Spring Kafka provides several strategies to handle failures gracefully.

**1. Retry Mechanisms:**

**Simple Retry with @RetryableTopic:**
```java
@Component
public class OrderProcessor {
    
    @RetryableTopic(
        attempts = "3",
        backoff = @Backoff(delay = 1000, multiplier = 2.0),
        dlt = @DltStrategy(TopicSuffixStrategy.SUFFIX_WITH_INDEX_VALUE)
    )
    @KafkaListener(topics = "orders")
    public void processOrder(Order order) {
        if (order.getAmount() < 0) {
            throw new IllegalArgumentException("Invalid order amount");
        }
        // Process order
    }
    
    // DLT handler
    @DltHandler
    public void handleDlt(Order order, @Header KafkaHeaders headers) {
        log.error("Order sent to DLT: {}", order);
        // Handle poison message - save to database, alert admin, etc.
    }
}
```

**2. Manual Error Handling:**

```java
@Component
public class OrderConsumer {
    
    @KafkaListener(topics = "orders")
    public void processOrder(Order order, 
                           @Header Map<String, Object> headers,
                           Acknowledgment ack) {
        try {
            // Process the order
            processOrderLogic(order);
            // Acknowledge successful processing
            ack.acknowledge();
            
        } catch (RetryableException e) {
            // Don't acknowledge - message will be retried
            log.warn("Retryable error processing order: {}", e.getMessage());
            
        } catch (NonRetryableException e) {
            // Acknowledge to skip this message
            log.error("Non-retryable error: {}", e.getMessage());
            sendToDeadLetterTopic(order, e);
            ack.acknowledge();
        }
    }
}
```

**3. Global Error Handler:**

```java
@Configuration
public class KafkaErrorHandlingConfig {
    
    @Bean
    public ConcurrentKafkaListenerContainerFactory<String, Object> kafkaListenerContainerFactory() {
        ConcurrentKafkaListenerContainerFactory<String, Object> factory = 
            new ConcurrentKafkaListenerContainerFactory<>();
        
        factory.setConsumerFactory(consumerFactory());
        
        // Set error handler
        factory.setCommonErrorHandler(new DefaultErrorHandler(
            new DeadLetterPublishingRecoverer(kafkaTemplate()),
            new FixedBackOff(1000L, 3))
        );
        
        return factory;
    }
}
```

**4. Circuit Breaker Pattern:**

```java
@Component
public class ResilientOrderProcessor {
    
    private final CircuitBreaker circuitBreaker;
    
    public ResilientOrderProcessor() {
        this.circuitBreaker = CircuitBreaker.ofDefaults("orderProcessing");
        circuitBreaker.getEventPublisher()
            .onStateTransition(event -> 
                log.info("Circuit breaker state transition: {}", event));
    }
    
    @KafkaListener(topics = "orders")
    public void processOrder(Order order) {
        Supplier<Void> decoratedSupplier = CircuitBreaker
            .decorateSupplier(circuitBreaker, () -> {
                processOrderLogic(order);
                return null;
            });
            
        Try.ofSupplier(decoratedSupplier)
            .recover(throwable -> {
                log.error("Circuit breaker opened, sending to fallback: {}", throwable.getMessage());
                handleFallback(order);
                return null;
            });
    }
}
```

**5. Poison Message Handling:**

```java
@Component
public class PoisonMessageHandler {
    
    private final KafkaTemplate<String, Object> kafkaTemplate;
    private final OrderRepository orderRepository;
    
    @EventListener
    public void handleListenerException(ListenerExecutionFailedException ex) {
        ConsumerRecord<?, ?> record = ex.getRecord();
        
        // Check if message has been retried too many times
        Integer retryCount = getRetryCount(record);
        if (retryCount > MAX_RETRIES) {
            // Send to poison message topic
            kafkaTemplate.send("poison-messages", record.value());
            
            // Store in database for manual investigation
            PoisonMessage poisonMessage = new PoisonMessage();
            poisonMessage.setTopic(record.topic());
            poisonMessage.setPartition(record.partition());
            poisonMessage.setOffset(record.offset());
            poisonMessage.setPayload(record.value().toString());
            poisonMessage.setError(ex.getMessage());
            
            orderRepository.savePoisonMessage(poisonMessage);
        }
    }
}
```

**Error handling strategies summary:**

| Strategy | Use Case | Pros | Cons |
|----------|----------|------|------|
| @RetryableTopic | Transient failures | Simple configuration | Limited customization |
| Manual ACK | Fine-grained control | Full control | More complex code |
| Global Error Handler | Consistent handling | Centralized | One-size-fits-all |
| Circuit Breaker | Downstream failures | Prevents cascade failures | Additional complexity |
| Dead Letter Topic | Poison messages | Doesn't block processing | Requires monitoring |

### 5. How would you handle duplicate message processing?

**Answer:**
Duplicate message handling is critical in distributed systems. There are several strategies depending on your requirements and constraints.

**1. Idempotent Processing (Recommended):**

Design your processing logic to be naturally idempotent:

```java
@Service
public class IdempotentOrderProcessor {
    
    @Autowired
    private OrderRepository orderRepository;
    
    @KafkaListener(topics = "orders")
    @Transactional
    public void processOrder(Order order) {
        // Check if order already exists
        if (orderRepository.existsById(order.getId())) {
            log.info("Order {} already processed, skipping", order.getId());
            return;
        }
        
        // Process order
        Order processedOrder = processOrderLogic(order);
        orderRepository.save(processedOrder);
        
        log.info("Order {} processed successfully", order.getId());
    }
}
```

**2. Database Constraints for Uniqueness:**

```java
@Entity
@Table(name = "orders")
public class Order {
    @Id
    private String orderId;
    
    @Column(unique = true)
    private String externalOrderId; // Business key
    
    // Other fields
}

@Service
public class OrderProcessor {
    
    @KafkaListener(topics = "orders")
    public void processOrder(Order order) {
        try {
            orderRepository.save(order);
        } catch (DataIntegrityViolationException e) {
            // Duplicate key - order already processed
            log.warn("Duplicate order received: {}", order.getId());
        }
    }
}
```

**3. Redis-based Deduplication:**

```java
@Service
public class RedisDeduplicationProcessor {
    
    @Autowired
    private RedisTemplate<String, String> redisTemplate;
    
    private static final String PROCESSED_KEY_PREFIX = "processed:order:";
    private static final Duration EXPIRY = Duration.ofHours(24);
    
    @KafkaListener(topics = "orders")
    public void processOrder(Order order) {
        String deduplicationKey = PROCESSED_KEY_PREFIX + order.getId();
        
        // Try to set the key if it doesn't exist
        Boolean isNewMessage = redisTemplate.opsForValue()
            .setIfAbsent(deduplicationKey, "processed", EXPIRY);
        
        if (Boolean.TRUE.equals(isNewMessage)) {
            // First time processing this message
            processOrderLogic(order);
            log.info("Order {} processed", order.getId());
        } else {
            log.info("Order {} already processed, skipping", order.getId());
        }
    }
}
```

**4. Exactly-Once Processing with Kafka Transactions:**

```java
@Configuration
public class ExactlyOnceKafkaConfig {
    
    @Bean
    public ProducerFactory<String, Object> producerFactory() {
        Map<String, Object> props = new HashMap<>();
        props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        props.put(ProducerConfig.ENABLE_IDEMPOTENCE_CONFIG, true);
        props.put(ProducerConfig.TRANSACTIONAL_ID_CONFIG, "order-processor-tx");
        props.put(ProducerConfig.ACKS_CONFIG, "all");
        props.put(ProducerConfig.RETRIES_CONFIG, Integer.MAX_VALUE);
        return new DefaultKafkaProducerFactory<>(props);
    }
    
    @Bean
    public ConsumerFactory<String, Object> consumerFactory() {
        Map<String, Object> props = new HashMap<>();
        props.put(ConsumerConfig.ISOLATION_LEVEL_CONFIG, "read_committed");
        props.put(ConsumerConfig.ENABLE_AUTO_COMMIT_CONFIG, false);
        return new DefaultKafkaConsumerFactory<>(props);
    }
}

@Service
public class TransactionalOrderProcessor {
    
    @Autowired
    @Qualifier("kafkaTransactionManager")
    private KafkaTransactionManager transactionManager;
    
    @KafkaListener(topics = "orders")
    @Transactional("kafkaTransactionManager")
    public void processOrder(Order order) {
        // All operations within this method are part of the same transaction
        processOrderLogic(order);
        
        // Send confirmation message
        kafkaTemplate.send("order-confirmations", order.getId(), "processed");
        
        // If any operation fails, entire transaction is rolled back
    }
}
```

**5. Message Headers for Deduplication:**

```java
@Service
public class HeaderBasedDeduplicationProcessor {
    
    @KafkaListener(topics = "orders")
    public void processOrder(Order order, 
                           @Header("message-id") String messageId,
                           @Header(value = "retry-count", required = false) Integer retryCount) {
        
        String deduplicationKey = "msg:" + messageId;
        
        // Check if we've seen this message ID before
        if (isMessageProcessed(deduplicationKey)) {
            log.info("Message {} already processed", messageId);
            return;
        }
        
        try {
            processOrderLogic(order);
            markMessageAsProcessed(deduplicationKey);
        } catch (Exception e) {
            log.error("Error processing message {}: {}", messageId, e.getMessage());
            throw e; // Let retry mechanism handle it
        }
    }
    
    private boolean isMessageProcessed(String key) {
        return redisTemplate.hasKey(key);
    }
    
    private void markMessageAsProcessed(String key) {
        redisTemplate.opsForValue().set(key, "processed", Duration.ofHours(24));
    }
}
```

**Producer-side deduplication:**

```java
@Service
public class DeduplicatingOrderProducer {
    
    @Autowired
    private KafkaTemplate<String, Object> kafkaTemplate;
    
    public void sendOrder(Order order) {
        // Generate idempotent key
        String messageId = generateMessageId(order);
        
        ProducerRecord<String, Object> record = new ProducerRecord<>("orders", order.getId(), order);
        record.headers().add("message-id", messageId.getBytes());
        record.headers().add("timestamp", String.valueOf(System.currentTimeMillis()).getBytes());
        record.headers().add("source", "order-service".getBytes());
        
        kafkaTemplate.send(record);
    }
    
    private String generateMessageId(Order order) {
        // Create deterministic ID based on order content
        String content = order.getId() + order.getCustomerId() + order.getAmount();
        return DigestUtils.sha256Hex(content);
    }
}
```

**Best practices for handling duplicates:**

1. **Design for idempotency first** - Make your business logic naturally handle duplicates
2. **Use database constraints** - Leverage unique indexes where possible  
3. **Implement deduplication windows** - Don't store deduplication keys forever
4. **Monitor duplicate rates** - High duplicate rates might indicate upstream issues
5. **Test failure scenarios** - Ensure your deduplication works during retries and rebalances

**Performance considerations:**
- Redis lookup adds latency but provides distributed deduplication
- Database constraints are reliable but can impact performance
- In-memory deduplication is fast but not distributed
- Choose based on your consistency and performance requirements

### 6. How do you implement exactly-once processing in your application?

**Answer:**
Exactly-once processing is one of the most challenging aspects of distributed messaging. Here's how to implement it properly:

**1. Understanding Exactly-Once Semantics:**

Exactly-once means each message is processed exactly one time, even in the presence of failures. This requires coordination between:
- Producer (idempotent sends)
- Kafka (transactional guarantees) 
- Consumer (transactional processing)

**2. Producer Configuration for Exactly-Once:**

```java
@Configuration
public class ExactlyOnceProducerConfig {
    
    @Bean
    public ProducerFactory<String, Object> exactlyOnceProducerFactory() {
        Map<String, Object> props = new HashMap<>();
        
        // Basic configuration
        props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
        props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, JsonSerializer.class);
        
        // Exactly-once configuration
        props.put(ProducerConfig.ENABLE_IDEMPOTENCE_CONFIG, true);
        props.put(ProducerConfig.TRANSACTIONAL_ID_CONFIG, "payment-processor-" + UUID.randomUUID());
        props.put(ProducerConfig.ACKS_CONFIG, "all");
        props.put(ProducerConfig.RETRIES_CONFIG, Integer.MAX_VALUE);
        props.put(ProducerConfig.MAX_IN_FLIGHT_REQUESTS_PER_CONNECTION, 1);
        
        return new DefaultKafkaProducerFactory<>(props);
    }
    
    @Bean
    public KafkaTemplate<String, Object> exactlyOnceKafkaTemplate() {
        return new KafkaTemplate<>(exactlyOnceProducerFactory());
    }
}
```

**3. Consumer Configuration for Exactly-Once:**

```java
@Configuration
public class ExactlyOnceConsumerConfig {
    
    @Bean
    public ConsumerFactory<String, Object> exactlyOnceConsumerFactory() {
        Map<String, Object> props = new HashMap<>();
        
        props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        props.put(ConsumerConfig.GROUP_ID_CONFIG, "payment-group");
        props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
        props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, JsonDeserializer.class);
        
        // Exactly-once consumer settings
        props.put(ConsumerConfig.ISOLATION_LEVEL_CONFIG, "read_committed");
        props.put(ConsumerConfig.ENABLE_AUTO_COMMIT_CONFIG, false);
        
        return new DefaultKafkaConsumerFactory<>(props);
    }
    
    @Bean
    public KafkaTransactionManager kafkaTransactionManager() {
        KafkaTransactionManager manager = new KafkaTransactionManager(exactlyOnceProducerFactory());
        return manager;
    }
}
```

**4. Transactional Message Processing:**

```java
@Service
public class PaymentProcessor {
    
    @Autowired
    private PaymentRepository paymentRepository;
    
    @Autowired
    private KafkaTemplate<String, Object> kafkaTemplate;
    
    @KafkaListener(topics = "payment-requests")
    @Transactional("kafkaTransactionManager")
    public void processPayment(PaymentRequest request) {
        try {
            // 1. Process the payment
            Payment payment = processPaymentLogic(request);
            
            // 2. Save to database (this will be part of Kafka transaction)
            paymentRepository.save(payment);
            
            // 3. Send confirmation message
            PaymentConfirmation confirmation = new PaymentConfirmation(
                payment.getId(), 
                payment.getStatus(),
                payment.getAmount()
            );
            kafkaTemplate.send("payment-confirmations", confirmation);
            
            // 4. Send notification
            NotificationEvent notification = new NotificationEvent(
                request.getCustomerId(),
                "Payment processed successfully"
            );
            kafkaTemplate.send("notifications", notification);
            
            log.info("Payment {} processed successfully", payment.getId());
            
        } catch (Exception e) {
            log.error("Error processing payment {}: {}", request.getId(), e.getMessage());
            // Transaction will be rolled back automatically
            throw new PaymentProcessingException("Payment processing failed", e);
        }
    }
}
```

**5. Database + Kafka Transactions (Dual Writes Problem):**

```java
@Service
@Transactional
public class OrderProcessorWithDualWrites {
    
    @Autowired
    private OrderRepository orderRepository;
    
    @Autowired
    private KafkaTemplate<String, Object> kafkaTemplate;
    
    // This approach has issues - database and Kafka transactions are separate
    public void processOrderIncorrect(Order order) {
        // Database transaction
        Order savedOrder = orderRepository.save(order);
        
        // Separate Kafka transaction - can fail independently
        kafkaTemplate.send("order-confirmations", savedOrder);
        // If Kafka fails, database change is already committed!
    }
    
    // Better approach: Outbox pattern
    public void processOrderWithOutbox(Order order) {
        // Save both order and outbox event in same database transaction
        Order savedOrder = orderRepository.save(order);
        
        OutboxEvent event = new OutboxEvent(
            "order-confirmations",
            savedOrder.getId(),
            savedOrder
        );
        outboxEventRepository.save(event);
        
        // Separate process publishes outbox events to Kafka
    }
}

// Outbox event publisher (separate component)
@Component
public class OutboxEventPublisher {
    
    @Scheduled(fixedDelay = 1000)
    @Transactional("kafkaTransactionManager")
    public void publishOutboxEvents() {
        List<OutboxEvent> unpublishedEvents = outboxEventRepository.findUnpublished();
        
        for (OutboxEvent event : unpublishedEvents) {
            kafkaTemplate.send(event.getTopic(), event.getPayload());
            event.setPublished(true);
            outboxEventRepository.save(event);
        }
    }
}
```

**6. Saga Pattern for Distributed Transactions:**

```java
@Service
public class OrderSagaProcessor {
    
    @KafkaListener(topics = "order-created")
    @Transactional("kafkaTransactionManager")
    public void handleOrderCreated(OrderCreated event) {
        try {
            // Step 1: Reserve inventory
            InventoryReservation reservation = new InventoryReservation(
                event.getOrderId(), 
                event.getProductId(), 
                event.getQuantity()
            );
            kafkaTemplate.send("inventory-reservations", reservation);
            
        } catch (Exception e) {
            // Send compensation event
            OrderCancellation cancellation = new OrderCancellation(
                event.getOrderId(), 
                "Inventory reservation failed"
            );
            kafkaTemplate.send("order-cancellations", cancellation);
        }
    }
    
    @KafkaListener(topics = "inventory-reserved")
    @Transactional("kafkaTransactionManager")
    public void handleInventoryReserved(InventoryReserved event) {
        try {
            // Step 2: Process payment
            PaymentRequest payment = new PaymentRequest(
                event.getOrderId(),
                event.getAmount()
            );
            kafkaTemplate.send("payment-requests", payment);
            
        } catch (Exception e) {
            // Send compensation events
            kafkaTemplate.send("inventory-releases", new InventoryRelease(event.getOrderId()));
            kafkaTemplate.send("order-cancellations", new OrderCancellation(event.getOrderId(), "Payment failed"));
        }
    }
    
    // Handle compensation events
    @KafkaListener(topics = "payment-failed")
    @Transactional("kafkaTransactionManager")
    public void handlePaymentFailed(PaymentFailed event) {
        // Release inventory
        kafkaTemplate.send("inventory-releases", new InventoryRelease(event.getOrderId()));
        
        // Cancel order
        kafkaTemplate.send("order-cancellations", new OrderCancellation(event.getOrderId(), "Payment failed"));
    }
}
```

**7. Monitoring Exactly-Once Processing:**

```java
@Component
public class ExactlyOnceMonitor {
    
    private final MeterRegistry meterRegistry;
    private final Counter duplicateCounter;
    private final Counter transactionRollbackCounter;
    
    public ExactlyOnceMonitor(MeterRegistry meterRegistry) {
        this.meterRegistry = meterRegistry;
        this.duplicateCounter = Counter.builder("kafka.duplicates")
            .description("Number of duplicate messages detected")
            .register(meterRegistry);
        this.transactionRollbackCounter = Counter.builder("kafka.transaction.rollbacks")
            .description("Number of transaction rollbacks")
            .register(meterRegistry);
    }
    
    @EventListener
    public void handleDuplicateMessage(DuplicateMessageEvent event) {
        duplicateCounter.increment(
            Tags.of("topic", event.getTopic(), "partition", String.valueOf(event.getPartition()))
        );
    }
    
    @EventListener
    public void handleTransactionRollback(TransactionRollbackEvent event) {
        transactionRollbackCounter.increment(
            Tags.of("reason", event.getReason())
        );
    }
}
```

**Key points for exactly-once processing:**

1. **Enable idempotent producers** - Prevents duplicate messages at source
2. **Use transactions** - Coordinate between multiple operations
3. **Read committed isolation** - Only see committed messages
4. **Handle the outbox pattern** - Solve dual writes problem
5. **Monitor and alert** - Track transaction failures and duplicates
6. **Test failure scenarios** - Ensure exactly-once works during failures

**Trade-offs:**
- **Performance**: Exactly-once processing is slower than at-least-once
- **Complexity**: More complex configuration and error handling
- **Operational overhead**: More monitoring and troubleshooting needed

### 7. How do you handle consumer lag and what causes it?

**Answer:**
Consumer lag is the difference between the latest message in a partition and the last message processed by a consumer. It's a critical metric for Kafka applications.

**Understanding Consumer Lag:**

```
Latest Offset in Partition: 1000
Consumer Current Offset: 850
Consumer Lag: 150 messages
```

**Common Causes of Consumer Lag:**

**1. Slow Message Processing:**
```java
// Problematic - blocking operation in consumer
@KafkaListener(topics = "orders")
public void processOrder(Order order) {
    // This takes 2 seconds per message!
    callSlowExternalAPI(order);
    updateDatabase(order);
    sendEmail(order);
}
```

**Solution - Asynchronous Processing:**
```java
@Service
public class AsyncOrderProcessor {
    
    @Async("orderProcessingExecutor")
    @KafkaListener(topics = "orders")
    public CompletableFuture<Void> processOrder(Order order, Acknowledgment ack) {
        return CompletableFuture.runAsync(() -> {
            try {
                processOrderLogic(order);
                ack.acknowledge();
            } catch (Exception e) {
                log.error("Error processing order: {}", e.getMessage());
                // Don't acknowledge on error
            }
        });
    }
}

@Configuration
public class AsyncConfig {
    
    @Bean("orderProcessingExecutor")
    public TaskExecutor orderProcessingExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(10);
        executor.setMaxPoolSize(20);
        executor.setQueueCapacity(100);
        executor.setThreadNamePrefix("order-processor-");
        executor.initialize();
        return executor;
    }
}
```

**2. Insufficient Consumer Instances:**

**Problem**: 4 partitions, 1 consumer → 1 consumer handles all partitions

**Solution**: Scale consumers to match partitions
```java
@KafkaListener(topics = "orders", concurrency = "4")
public void processOrder(Order order) {
    // Now 4 consumer threads running concurrently
    processOrderLogic(order);
}
```

**Or deploy multiple instances:**
```yaml
# docker-compose.yml
version: '3'
services:
  order-processor-1:
    image: order-processor:latest
    environment:
      - SPRING_KAFKA_CONSUMER_GROUP_ID=order-processors
  
  order-processor-2:
    image: order-processor:latest
    environment:
      - SPRING_KAFKA_CONSUMER_GROUP_ID=order-processors
      
  order-processor-3:
    image: order-processor:latest
    environment:
      - SPRING_KAFKA_CONSUMER_GROUP_ID=order-processors
```

**3. Poor Consumer Configuration:**

```java
@Bean
public ConsumerFactory<String, Object> optimizedConsumerFactory() {
    Map<String, Object> props = new HashMap<>();
    
    // Basic config
    props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
    props.put(ConsumerConfig.GROUP_ID_CONFIG, "order-processors");
    
    // Performance optimizations
    props.put(ConsumerConfig.FETCH_MIN_BYTES_CONFIG, 1024); // Wait for more data
    props.put(ConsumerConfig.FETCH_MAX_WAIT_MS_CONFIG, 500); // But don't wait too long
    props.put(ConsumerConfig.MAX_POLL_RECORDS_CONFIG, 100); // Process in batches
    props.put(ConsumerConfig.ENABLE_AUTO_COMMIT_CONFIG, false); // Manual commit for control
    
    return new DefaultKafkaConsumerFactory<>(props);
}
```

**4. Monitoring Consumer Lag:**

```java
@Component
public class ConsumerLagMonitor {
    
    private final MeterRegistry meterRegistry;
    private final AdminClient adminClient;
    
    @Scheduled(fixedDelay = 30000) // Check every 30 seconds
    public void monitorConsumerLag() {
        try {
            Map<TopicPartition, OffsetAndMetadata> consumerOffsets = getConsumerOffsets("order-processors");
            Map<TopicPartition, Long> latestOffsets = getLatestOffsets();
            
            for (Map.Entry<TopicPartition, OffsetAndMetadata> entry : consumerOffsets.entrySet()) {
                TopicPartition tp = entry.getKey();
                long consumerOffset = entry.getValue().offset();
                long latestOffset = latestOffsets.get(tp);
                long lag = latestOffset - consumerOffset;
                
                // Record metric
                Gauge.builder("kafka.consumer.lag")
                    .tags("topic", tp.topic(), "partition", String.valueOf(tp.partition()))
                    .register(meterRegistry, () -> lag);
                
                // Alert if lag is too high
                if (lag > 1000) {
                    log.warn("High consumer lag detected: {} messages for {}", lag, tp);
                    sendAlert("High consumer lag", tp, lag);
                }
            }
        } catch (Exception e) {
            log.error("Error monitoring consumer lag: {}", e.getMessage());
        }
    }
}
```

**5. Implementing Circuit Breaker for External Dependencies:**

```java
@Component
public class ResilientOrderProcessor {
    
    private final CircuitBreaker externalAPICircuitBreaker;
    private final CircuitBreaker databaseCircuitBreaker;
    
    public ResilientOrderProcessor() {
        // Configure circuit breakers
        CircuitBreakerConfig config = CircuitBreakerConfig.custom()
            .failureRateThreshold(50)
            .waitDurationInOpenState(Duration.ofSeconds(30))
            .slidingWindowSize(10)
            .build();
            
        this.externalAPICircuitBreaker = CircuitBreaker.of("externalAPI", config);
        this.databaseCircuitBreaker = CircuitBreaker.of("database", config);
    }
    
    @KafkaListener(topics = "orders")
    public void processOrder(Order order, Acknowledgment ack) {
        try {
            // Process with circuit breaker protection
            Supplier<Void> decoratedSupplier = CircuitBreaker
                .decorateSupplier(externalAPICircuitBreaker, () -> {
                    callExternalAPI(order);
                    return null;
                });
            
            Try.ofSupplier(decoratedSupplier)
                .recover(throwable -> {
                    // Fallback - process without external API
                    log.warn("External API unavailable, processing with fallback");
                    processOrderWithoutAPI(order);
                    return null;
                });
            
            // Database operation with circuit breaker
            Supplier<Void> dbSupplier = CircuitBreaker
                .decorateSupplier(databaseCircuitBreaker, () -> {
                    saveOrder(order);
                    return null;
                });
            
            Try.ofSupplier(dbSupplier)
                .onSuccess(result -> ack.acknowledge())
                .onFailure(throwable -> {
                    // Don't acknowledge - message will be retried
                    log.error("Database operation failed: {}", throwable.getMessage());
                });
                
        } catch (Exception e) {
            log.error("Error processing order: {}", e.getMessage());
            // Don't acknowledge on error
        }
    }
}
```

**6. Batch Processing to Reduce Lag:**

```java
@Component
public class BatchOrderProcessor {
    
    private final List<Order> orderBatch = new ArrayList<>();
    private final Object batchLock = new Object();
    
    @KafkaListener(topics = "orders")
    public void processOrder(Order order, Acknowledgment ack) {
        synchronized (batchLock) {
            orderBatch.add(order);
            
            // Process in batches of 10 or when timeout occurs
            if (orderBatch.size() >= 10) {
                processBatch(new ArrayList<>(orderBatch));
                orderBatch.clear();
                ack.acknowledge();
            }
        }
    }
    
    @Scheduled(fixedDelay = 5000) // Process remaining orders every 5 seconds
    public void processRemainingOrders() {
        synchronized (batchLock) {
            if (!orderBatch.isEmpty()) {
                processBatch(new ArrayList<>(orderBatch));
                orderBatch.clear();
            }
        }
    }
    
    private void processBatch(List<Order> orders) {
        // Batch database operations
        orderRepository.saveAll(orders);
        
        // Batch external API calls
        externalAPIService.processBatch(orders);
        
        log.info("Processed batch of {} orders", orders.size());
    }
}
```

**7. Auto-scaling Based on Consumer Lag:**

```java
@Component
public class ConsumerAutoScaler {
    
    @Autowired
    private KubernetesClient kubernetesClient;
    
    @Scheduled(fixedDelay = 60000) // Check every minute
    public void scaleBasedOnLag() {
        double avgLag = getAverageConsumerLag("order-processors");
        int currentReplicas = getCurrentReplicaCount("order-processor-deployment");
        
        if (avgLag > 1000 && currentReplicas < MAX_REPLICAS) {
            // Scale up
            scaleDeployment("order-processor-deployment", currentReplicas + 1);
            log.info("Scaled up order processors due to high lag: {}", avgLag);
            
        } else if (avgLag < 100 && currentReplicas > MIN_REPLICAS) {
            // Scale down
            scaleDeployment("order-processor-deployment", currentReplicas - 1);
            log.info("Scaled down order processors due to low lag: {}", avgLag);
        }
    }
}
```

**Consumer Lag Troubleshooting Checklist:**

1. **Check processing time per message** - Use metrics to identify slow operations
2. **Monitor consumer thread utilization** - Ensure threads aren't idle
3. **Review external dependencies** - Identify slow downstream services  
4. **Analyze partition distribution** - Ensure even load across partitions
5. **Check consumer configuration** - Optimize fetch sizes and poll records
6. **Monitor GC performance** - Long GC pauses can cause lag
7. **Review error rates** - Failed messages that retry can cause lag

**Best practices:**
- Set up alerting when lag exceeds thresholds
- Use batch processing for better throughput
- Implement circuit breakers for external dependencies
- Scale consumers horizontally when needed
- Monitor and optimize the entire processing pipeline

### 8. Sample Technical Scenarios

**Scenario 1: High-throughput application where message order is critical**

**Question:** "You have a high-throughput application processing financial transactions where message order is critical. How would you design your Kafka setup?"

**Answer:**

For financial transactions requiring strict ordering, here's how I would design the system:

**Topic and Partition Strategy:**
```java
// Single partition per account to guarantee ordering
public class AccountTransactionProducer {
    
    @Autowired
    private KafkaTemplate<String, Transaction> kafkaTemplate;
    
    public void sendTransaction(Transaction transaction) {
        // Use account ID as key to ensure same partition
        String partitionKey = transaction.getAccountId();
        
        ProducerRecord<String, Transaction> record = new ProducerRecord<>(
            "financial-transactions",
            partitionKey, // This ensures all transactions for an account go to same partition
            transaction
        );
        
        // Add headers for tracing
        record.headers().add("transaction-id", transaction.getId().getBytes());
        record.headers().add("timestamp", String.valueOf(System.currentTimeMillis()).getBytes());
        
        kafkaTemplate.send(record);
    }
}
```

**Producer Configuration for Reliability:**
```java
@Bean
public ProducerFactory<String, Transaction> transactionProducerFactory() {
    Map<String, Object> props = new HashMap<>();
    
    // High reliability settings
    props.put(ProducerConfig.ACKS_CONFIG, "all"); // Wait for all replicas
    props.put(ProducerConfig.RETRIES_CONFIG, Integer.MAX_VALUE);
    props.put(ProducerConfig.ENABLE_IDEMPOTENCE_CONFIG, true);
    props.put(ProducerConfig.MAX_IN_FLIGHT_REQUESTS_PER_CONNECTION, 1); // Strict ordering
    
    // Performance settings
    props.put(ProducerConfig.BATCH_SIZE_CONFIG, 16384);
    props.put(ProducerConfig.LINGER_MS_CONFIG, 10); // Small delay for batching
    props.put(ProducerConfig.COMPRESSION_TYPE_CONFIG, "snappy");
    
    return new DefaultKafkaProducerFactory<>(props);
}
```

**Consumer with Ordered Processing:**
```java
@Service
public class OrderedTransactionProcessor {
    
    @KafkaListener(topics = "financial-transactions", concurrency = "1") // Single thread per partition
    public void processTransaction(Transaction transaction, 
                                 @Header("kafka_receivedPartitionId") int partition,
                                 Acknowledgment ack) {
        
        String accountId = transaction.getAccountId();
        log.info("Processing transaction {} for account {} on partition {}", 
                transaction.getId(), accountId, partition);
        
        try {
            // Process transaction in order
            Account account = accountService.getAccount(accountId);
            
            // Validate transaction sequence
            if (!isValidSequence(transaction, account.getLastTransactionSequence())) {
                throw new InvalidSequenceException("Transaction out of sequence");
            }
            
            // Apply transaction
            account = applyTransaction(account, transaction);
            accountService.updateAccount(account);
            
            // Send confirmation
            sendTransactionConfirmation(transaction);
            
            ack.acknowledge();
            
        } catch (Exception e) {
            log.error("Error processing transaction {}: {}", transaction.getId(), e.getMessage());
            // Don't acknowledge - this will pause processing for this partition
            // Consider implementing dead letter queue for poison messages
        }
    }
    
    private boolean isValidSequence(Transaction transaction, long lastSequence) {
        return transaction.getSequenceNumber() == lastSequence + 1;
    }
}
```

**Topic Configuration:**
```bash
# Create topic with multiple partitions but careful key partitioning
kafka-topics.sh --create --topic financial-transactions \
  --bootstrap-server localhost:9092 \
  --partitions 12 \
  --replication-factor 3 \
  --config min.insync.replicas=2 \
  --config cleanup.policy=compact \
  --config retention.ms=2592000000  # 30 days
```

**Monitoring and Alerting:**
```java
@Component
public class TransactionOrderingMonitor {
    
    @EventListener
    public void handleOutOfOrderTransaction(OutOfOrderTransactionEvent event) {
        // Alert immediately - this is critical for financial data
        alertService.sendCriticalAlert(
            "Out of order transaction detected",
            "Account: " + event.getAccountId() + 
            ", Expected: " + event.getExpectedSequence() + 
            ", Actual: " + event.getActualSequence()
        );
    }
    
    @Scheduled(fixedDelay = 30000)
    public void monitorProcessingLag() {
        // Financial transactions should have minimal lag
        double avgLag = getAverageConsumerLag("financial-transaction-processors");
        if (avgLag > 100) { // Alert if more than 100 messages behind
            alertService.sendAlert("High processing lag: " + avgLag + " messages");
        }
    }
}
```

**Key Design Decisions:**
- **Single partition per account** - Guarantees ordering for each account
- **Idempotent producer** - Prevents duplicate transactions
- **Manual acknowledgment** - Ensures no transaction is lost
- **Sequence validation** - Detect out-of-order processing
- **High replication factor** - Ensures durability
- **Monitoring** - Critical for financial applications

---

**Scenario 2: Consumer processing messages slowly, causing lag**

**Question:** "Your Kafka consumer is processing messages slowly, causing lag. What steps would you take to diagnose and fix this?"

**Answer:**

Here's my systematic approach to diagnosing and fixing slow consumer processing:

**Step 1: Identify the Bottleneck**

```java
@Component
public class ConsumerPerformanceProfiler {
    
    private final Timer.Sample processingTimer;
    private final MeterRegistry meterRegistry;
    
    @KafkaListener(topics = "orders")
    public void processOrder(Order order, Acknowledgment ack) {
        Timer.Sample sample = Timer.start(meterRegistry);
        
        try {
            // Measure each operation
            Timer.Sample dbTimer = Timer.start(meterRegistry);
            Order savedOrder = orderRepository.save(order); // Database operation
            dbTimer.stop(Timer.builder("order.database.time").register(meterRegistry));
            
            Timer.Sample apiTimer = Timer.start(meterRegistry);
            externalAPIService.notifyOrder(savedOrder); // External API call
            apiTimer.stop(Timer.builder("order.api.time").register(meterRegistry));
            
            Timer.Sample emailTimer = Timer.start(meterRegistry);
            emailService.sendConfirmation(savedOrder); // Email service
            emailTimer.stop(Timer.builder("order.email.time").register(meterRegistry));
            
            ack.acknowledge();
            
        } catch (Exception e) {
            log.error("Error processing order: {}", e.getMessage());
            sample.stop(Timer.builder("order.processing.time").tag("status", "error").register(meterRegistry));
            throw e;
        }
        
        sample.stop(Timer.builder("order.processing.time").tag("status", "success").register(meterRegistry
