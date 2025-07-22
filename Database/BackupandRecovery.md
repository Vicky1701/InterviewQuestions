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

## Backup and Recovery

### Backup Strategies

#### Full Backup
- **Definition**: Complete copy of all data at specific point in time
- **Process**: Copies every file, database, or system component
- **Advantages**:
  - Simple restore process - single backup contains everything
  - Fastest recovery time
  - Independent - doesn't rely on other backups
  - Easy to verify and validate
- **Disadvantages**:
  - Largest storage requirement
  - Longest backup time
  - High network/IO impact during backup
  - Most expensive in terms of resources
- **Best for**: Critical systems, weekly/monthly schedules, compliance requirements
- **Example**: Complete database dump every Sunday night

#### Incremental Backup
- **Definition**: Only backs up data changed since last backup (full or incremental)
- **Process**: 
  - Day 1: Full backup
  - Day 2: Changes since Day 1
  - Day 3: Changes since Day 2
- **Advantages**:
  - Fastest backup time after initial full
  - Minimal storage space per backup
  - Low impact on system resources
  - Efficient use of bandwidth
- **Disadvantages**:
  - Complex restore - needs full + all incrementals
  - Chain dependency - if one fails, later backups unusable
  - Longer recovery time
  - Higher risk of backup corruption
- **Best for**: Daily backups, large datasets, limited backup windows
- **Example**: Daily incremental backups with weekly full backup

#### Differential Backup
- **Definition**: Backs up all changes since last full backup
- **Process**:
  - Day 1: Full backup
  - Day 2: All changes since Day 1
  - Day 3: All changes since Day 1 (cumulative)
- **Advantages**:
  - Simpler restore than incremental (full + latest differential)
  - Faster backup than full (except right before next full)
  - Good balance of speed and simplicity
  - Less dependency chain risk
- **Disadvantages**:
  - Grows larger over time until next full backup
  - Slower than incremental backup
  - More storage than incremental
- **Best for**: Medium-sized systems, balance between speed and simplicity
- **Example**: Weekly full backup with daily differential backups

### Backup Strategy Comparison

| Strategy | Backup Speed | Storage Space | Restore Speed | Complexity |
|----------|--------------|---------------|---------------|------------|
| Full | Slowest | Highest | Fastest | Lowest |
| Incremental | Fastest | Lowest | Slowest | Highest |
| Differential | Medium | Medium | Medium | Medium |

### Point-in-Time Recovery (PITR)

#### Definition
Ability to restore database to any specific moment in time, not just backup points

#### How It Works
- **Transaction logs**: Continuous recording of all database changes
- **Log shipping**: Regular transfer of transaction logs to backup location
- **Recovery process**: Restore full backup + apply transaction logs up to desired time
- **Granularity**: Can recover to exact second or transaction

#### Components
1. **Full backup**: Starting point for recovery
2. **Transaction logs**: Record of all changes since backup
3. **Log sequence numbers**: Ensure proper ordering of changes
4. **Recovery tools**: Software to apply logs to backup

#### Benefits
- **Minimal data loss**: Can recover to within seconds of failure
- **Flexibility**: Choose exact recovery point
- **Compliance**: Meet regulatory requirements for data retention
- **Human error recovery**: Undo accidental changes

#### Challenges
- **Storage overhead**: Transaction logs require significant space
- **Performance impact**: Continuous logging affects system performance
- **Complexity**: More complex backup and recovery procedures
- **Network bandwidth**: Log shipping requires network resources

#### Implementation Examples
- **MySQL**: Binary log files with mysqlbinlog utility
- **PostgreSQL**: Write-Ahead Logging (WAL) with pg_basebackup
- **SQL Server**: Transaction log backups with T-SQL RESTORE commands
- **Oracle**: Archive logs with RMAN recovery

### Disaster Recovery Planning

#### Recovery Objectives

##### Recovery Time Objective (RTO)
- **Definition**: Maximum acceptable time to restore service after failure
- **Examples**: 4 hours, 24 hours, 1 week
- **Impact**: Determines infrastructure investment and strategy
- **Considerations**: Business impact, customer expectations, compliance

##### Recovery Point Objective (RPO)
- **Definition**: Maximum acceptable data loss measured in time
- **Examples**: 15 minutes, 1 hour, 1 day of data loss
- **Impact**: Determines backup frequency and strategy
- **Considerations**: Data criticality, business processes, regulations

#### Disaster Recovery Strategies

##### Cold Site
- **Definition**: Backup facility with basic infrastructure but no active systems
- **Setup time**: Days to weeks
- **Cost**: Lowest
- **Use case**: Non-critical systems, long RTO acceptable

##### Warm Site
- **Definition**: Backup facility with infrastructure and some systems pre-configured
- **Setup time**: Hours to days
- **Cost**: Medium
- **Use case**: Moderate criticality, balanced cost/recovery time

##### Hot Site
- **Definition**: Fully operational backup facility with real-time data replication
- **Setup time**: Minutes to hours
- **Cost**: Highest
- **Use case**: Mission-critical systems, minimal RTO/RPO

##### Cloud DR
- **Definition**: Disaster recovery using cloud infrastructure
- **Advantages**: Scalable, cost-effective, geographically distributed
- **Models**: Backup to cloud, pilot light, warm standby, multi-site

#### DR Planning Components
1. **Risk assessment**: Identify potential disasters and impacts
2. **Business impact analysis**: Prioritize systems and data criticality
3. **Recovery strategies**: Define approaches for different scenarios
4. **Documentation**: Detailed procedures and contact information
5. **Testing**: Regular drills and plan validation
6. **Communication**: Stakeholder notification and coordination plans

### High Availability Concepts

#### Availability Metrics

##### Uptime Percentages
- **99%**: 3.65 days downtime per year
- **99.9% (3 nines)**: 8.77 hours downtime per year
- **99.99% (4 nines)**: 52.6 minutes downtime per year
- **99.999% (5 nines)**: 5.26 minutes downtime per year

#### High Availability Strategies

##### Redundancy
- **Hardware**: Multiple servers, storage, network paths
- **Software**: Clustering, load balancing
- **Geographic**: Multiple data centers, regions
- **N+1**: One extra component beyond minimum required
- **2N**: Complete duplicate of entire system

##### Failover Mechanisms
- **Active-Passive**: One system active, backup on standby
- **Active-Active**: Multiple systems handling load simultaneously
- **Automatic failover**: System detects failure and switches automatically
- **Manual failover**: Human intervention required to switch systems

##### Load Balancing
- **Purpose**: Distribute traffic across multiple servers
- **Methods**: Round-robin, least connections, weighted, geographic
- **Health checks**: Monitor server status and remove failed nodes
- **Session affinity**: Route user sessions to same server

#### Eliminating Single Points of Failure

##### Common Single Points of Failure
- Database servers
- Load balancers
- Network switches
- Power supplies
- Internet connections
- DNS servers

##### Mitigation Strategies
- **Database clustering**: Master-slave, master-master configurations
- **Load balancer redundancy**: Multiple load balancers with failover
- **Network redundancy**: Multiple ISPs, diverse routing paths
- **Power redundancy**: UPS, generators, dual power supplies
- **Geographic distribution**: Multi-region deployments

#### Monitoring and Alerting
- **Health checks**: Regular system status verification
- **Performance monitoring**: Track response times, throughput
- **Capacity monitoring**: Resource utilization trends
- **Automated alerts**: Immediate notification of issues
- **Escalation procedures**: Progressive notification of failures

### Interview Focus Areas

#### Common Questions
1. "How would you design a backup strategy for a 24/7 e-commerce site?"
2. "Explain the trade-offs between different backup types"
3. "How do you achieve 99.99% availability?"
4. "Design a disaster recovery plan for a financial application"

#### Key Points to Emphasize
- **Business requirements drive technical decisions**: Always start with RTO/RPO
- **Cost vs. availability trade-offs**: Higher availability costs more
- **Testing is critical**: Untested backups/DR plans often fail when needed
- **Automation reduces human error**: Automate failover and recovery processes
- **Documentation and training**: Ensure team knows procedures

#### Real-world Scenarios to Discuss
- Database corruption during business hours
- Data center outage affecting primary systems
- Accidental data deletion by application bug
- Regional disaster affecting entire geographic area
- Gradual system degradation vs. sudden failure
