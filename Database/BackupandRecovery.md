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
