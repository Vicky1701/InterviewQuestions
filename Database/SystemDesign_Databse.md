# System Design & Architecture Interview Notes

## Database Scaling Strategies

### Vertical Scaling (Scale Up)
- **Definition**: Adding more power (CPU, RAM, storage) to existing machine
- **Pros**:
  - Simple to implement - no code changes needed
  - Maintains data consistency easily
  - No complex distributed system issues
- **Cons**:
  - Hardware limits - can't scale infinitely
  - Single point of failure
  - Expensive at high levels
  - Downtime required for upgrades
- **Best for**: Small to medium applications, when simplicity is priority

### Horizontal Scaling (Scale Out)
- **Definition**: Adding more machines to resource pool
- **Pros**:
  - Nearly unlimited scaling potential
  - Better fault tolerance - no single point of failure
  - Cost-effective using commodity hardware
  - Can scale specific components independently
- **Cons**:
  - Complex architecture and code changes
  - Data consistency challenges
  - Network latency between nodes
  - Operational complexity increases
- **Best for**: Large-scale applications, high availability requirements

## Database Replication

### Master-Slave Replication
- **Structure**: One master (write), multiple slaves (read)
- **How it works**:
  - All writes go to master
  - Master replicates changes to slaves
  - Reads distributed across slaves
- **Advantages**:
  - Read scalability
  - Simple to implement
  - Good for read-heavy workloads
- **Disadvantages**:
  - Write bottleneck at master
  - Replication lag - eventual consistency
  - Master is single point of failure
- **Use cases**: Analytics, reporting, content delivery

### Master-Master Replication
- **Structure**: Multiple masters, all can handle reads/writes
- **How it works**:
  - All nodes can accept writes
  - Changes replicated to all other masters
  - Conflict resolution mechanisms needed
- **Advantages**:
  - No write bottleneck
  - Better availability - no single point of failure
  - Geographic distribution possible
- **Disadvantages**:
  - Complex conflict resolution
  - Consistency challenges
  - More complex to manage
- **Use cases**: Global applications, high write throughput needs

## Sharding and Partitioning

### Sharding (Horizontal Partitioning)
- **Definition**: Splitting data across multiple databases/servers
- **Strategies**:
  - **Range-based**: Partition by value ranges (A-M, N-Z)
  - **Hash-based**: Use hash function on key
  - **Directory-based**: Lookup service maps keys to shards
  - **Geographic**: Partition by location

### Partitioning Types
- **Horizontal**: Split rows across multiple tables/databases
- **Vertical**: Split columns into separate tables
- **Functional**: Split by feature/service (users, orders, products)

### Benefits
- Improved performance - smaller datasets per shard
- Better availability - failure affects only one shard
- Scalability - add more shards as needed

### Challenges
- **Hot spots**: Uneven data distribution
- **Cross-shard queries**: Complex and slow
- **Rebalancing**: Moving data when adding/removing shards
- **Consistency**: Maintaining ACID across shards

## CAP Theorem

### Definition
In distributed systems, you can only guarantee 2 of 3 properties:
- **Consistency**: All nodes see same data simultaneously
- **Availability**: System remains operational
- **Partition Tolerance**: System continues despite network failures

### Trade-offs in Practice

#### CP Systems (Consistency + Partition Tolerance)
- **Examples**: MongoDB, Redis, HBase
- **Behavior**: Becomes unavailable during network partitions
- **Use cases**: Financial systems, inventory management
- **Trade-off**: Sacrifice availability for data accuracy

#### AP Systems (Availability + Partition Tolerance)
- **Examples**: Cassandra, DynamoDB, CouchDB
- **Behavior**: Remains available but may serve stale data
- **Use cases**: Social media, content delivery, analytics
- **Trade-off**: Sacrifice immediate consistency for uptime

#### CA Systems (Consistency + Availability)
- **Examples**: Traditional RDBMS (MySQL, PostgreSQL)
- **Reality**: Only works in single-node or perfect network
- **Limitation**: Can't handle network partitions in distributed setup

### Practical Implications
- Network partitions are inevitable in distributed systems
- Must choose between CP or AP based on business requirements
- Often implement "eventual consistency" as compromise

## RDBMS vs NoSQL Trade-offs

### RDBMS (Relational Databases)

#### Strengths
- **ACID compliance**: Strong consistency guarantees
- **Complex queries**: SQL with joins, aggregations, subqueries
- **Data integrity**: Foreign keys, constraints, normalization
- **Mature ecosystem**: Tools, expertise, standards
- **Flexible querying**: Ad-hoc queries without predefined access patterns

#### Weaknesses
- **Scaling limitations**: Vertical scaling primarily
- **Schema rigidity**: Changes require migrations
- **Performance**: Joins expensive at scale
- **Impedance mismatch**: Object-relational mapping complexity

#### Best for
- Financial systems, ERP, CRM
- Complex relationships and transactions
- Strong consistency requirements
- Reporting and analytics

### NoSQL Databases

#### Document Stores (MongoDB, CouchDB)
- **Strengths**: Flexible schema, natural object mapping
- **Weaknesses**: Limited query capabilities, eventual consistency
- **Use cases**: Content management, catalogs, user profiles

#### Key-Value Stores (Redis, DynamoDB)
- **Strengths**: Simple, fast, highly scalable
- **Weaknesses**: Limited querying, no relationships
- **Use cases**: Caching, session storage, real-time features

#### Column Stores (Cassandra, HBase)
- **Strengths**: Great for time-series, analytical queries
- **Weaknesses**: Limited flexibility, eventual consistency
- **Use cases**: Analytics, logging, IoT data

#### Graph Databases (Neo4j, Amazon Neptune)
- **Strengths**: Relationship queries, pattern matching
- **Weaknesses**: Limited scalability, specialized use cases
- **Use cases**: Social networks, recommendation engines, fraud detection

### Decision Framework

#### Choose RDBMS when
- Strong consistency required
- Complex relationships and transactions
- Ad-hoc querying needs
- Team expertise with SQL
- Compliance requirements (ACID)

#### Choose NoSQL when
- Massive scale requirements
- Flexible/evolving schema
- Simple access patterns
- High availability priority
- Specific data model fits (documents, graphs, etc.)

## Interview Tips

### Common Questions
1. "How would you scale a database for 1 million users?"
2. "Explain the trade-offs between SQL and NoSQL"
3. "Design a system that needs both high availability and consistency"
4. "When would you choose sharding over replication?"

### Key Points to Remember
- Always ask about requirements first (consistency vs availability)
- Mention trade-offs explicitly - no perfect solutions
- Consider operational complexity in decisions
- Think about failure scenarios and recovery
- Discuss monitoring and observability needs

### Red Flags to Avoid
- Claiming one approach is always better
- Ignoring consistency/availability trade-offs
- Over-engineering simple problems
- Not considering operational overhead
- Forgetting about data migration challenges
