# Transaction Management - Concurrency and Isolation

## Isolation Levels and Their Trade-offs

**Isolation levels** control how transactions interact with each other, balancing data consistency against performance and concurrency. Each level makes different trade-offs between preventing anomalies and allowing concurrent access.

### Read Uncommitted (Isolation Level 0)
**Lowest isolation** - allows reading uncommitted changes from other transactions.

#### Characteristics:
- **No shared locks** on read operations
- **Allows dirty reads** - can see uncommitted changes
- **Highest concurrency** but lowest consistency
- **Rarely used** in production systems

```sql
-- Setting isolation level (varies by database)
-- SQL Server/PostgreSQL
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

-- Example scenario
-- Transaction 1:
BEGIN TRANSACTION;
UPDATE accounts SET balance = balance - 1000 WHERE account_id = 1;
-- (Not yet committed)

-- Transaction 2 (Read Uncommitted):
SELECT balance FROM accounts WHERE account_id = 1;  
-- Returns the reduced balance even though T1 might rollback
```

#### Anomalies Allowed:
- **Dirty Read**: Reading uncommitted changes that might be rolled back
- **Non-repeatable Read**: Same query returns different results within transaction
- **Phantom Read**: New rows appear between identical queries

#### When to Use:
- **Data warehousing** where approximate results are acceptable
- **Reporting queries** that don't require perfect consistency
- **Performance-critical** scenarios where dirty reads are acceptable

### Read Committed (Isolation Level 1)
**Default in most databases** - prevents dirty reads but allows other anomalies.

#### Characteristics:
- **Shared locks held only during read**
- **Prevents dirty reads** - only sees committed changes
- **Good balance** of consistency and performance
- **Most commonly used** isolation level

```sql
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

-- Example: Prevents dirty reads
-- Transaction 1:
BEGIN TRANSACTION;
UPDATE accounts SET balance = balance - 1000 WHERE account_id = 1;
-- (Not yet committed)

-- Transaction 2 (Read Committed):
SELECT balance FROM accounts WHERE account_id = 1;  
-- Returns original balance, waits for T1 to commit/rollback
```

#### Anomalies Prevented:
- ✅ **Dirty Read**: Cannot read uncommitted changes

#### Anomalies Allowed:
- ❌ **Non-repeatable Read**: Same row read twice may return different values
- ❌ **Phantom Read**: Range queries may return different row sets

#### Real-world Example:
```sql
-- Banking application
BEGIN TRANSACTION;
    SELECT balance FROM accounts WHERE account_id = 1;  -- Returns 5000
    -- Another transaction updates this account
    SELECT balance FROM accounts WHERE account_id = 1;  -- Might return 4000
    -- Non-repeatable read occurred
COMMIT;
```

### Repeatable Read (Isolation Level 2)
**Stronger isolation** - prevents non-repeatable reads by holding shared locks longer.

#### Characteristics:
- **Shared locks held until transaction ends**
- **Same row always returns same value** within transaction
- **Prevents non-repeatable reads**
- **Higher locking overhead** than Read Committed

```sql
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;

-- Example: Consistent row reads
-- Transaction 1:
BEGIN TRANSACTION;
SELECT balance FROM accounts WHERE account_id = 1;  -- Returns 5000

-- Transaction 2 tries to update:
UPDATE accounts SET balance = 4000 WHERE account_id = 1;  
-- Blocked until T1 commits

-- Back to Transaction 1:
SELECT balance FROM accounts WHERE account_id = 1;  -- Still returns 5000
COMMIT;  -- Now T2 can proceed
```

#### Anomalies Prevented:
- ✅ **Dirty Read**: Cannot read uncommitted changes
- ✅ **Non-repeatable Read**: Same row reads return consistent values

#### Anomalies Allowed:
- ❌ **Phantom Read**: Range queries may still return different row sets

#### Phantom Read Example:
```sql
-- Transaction 1 (Repeatable Read):
BEGIN TRANSACTION;
SELECT COUNT(*) FROM orders WHERE customer_id = 123;  -- Returns 5

-- Transaction 2 inserts new order:
INSERT INTO orders (customer_id, amount) VALUES (123, 500);
COMMIT;

-- Back to Transaction 1:
SELECT COUNT(*) FROM orders WHERE customer_id = 123;  -- Returns 6 (phantom!)
COMMIT;
```

### Serializable (Isolation Level 3)
**Highest isolation** - transactions appear to execute serially.

#### Characteristics:
- **Complete isolation** from other transactions
- **Prevents all anomalies** including phantom reads
- **Highest consistency** but lowest concurrency
- **Performance overhead** from extensive locking/validation

```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;

-- Example: Complete isolation
-- Transaction 1:
BEGIN TRANSACTION;
SELECT COUNT(*) FROM orders WHERE customer_id = 123;  -- Returns 5

-- Transaction 2 tries to insert:
INSERT INTO orders (customer_id, amount) VALUES (123, 500);
-- Blocked or causes serialization failure

-- Transaction 1 continues:
SELECT COUNT(*) FROM orders WHERE customer_id = 123;  -- Still returns 5
COMMIT;
```

#### Implementation Approaches:
- **Lock-based**: Extensive range locking
- **Snapshot-based**: Compare read/write sets at commit time
- **Optimistic**: Validate at commit time, rollback if conflicts

#### Anomalies Prevented:
- ✅ **Dirty Read**: Cannot read uncommitted changes
- ✅ **Non-repeatable Read**: Same row reads return consistent values  
- ✅ **Phantom Read**: Range queries return consistent results

### Snapshot Isolation (Non-standard)
**Time-travel approach** - each transaction sees database snapshot from its start time.

#### Characteristics:
- **Each transaction sees consistent snapshot**
- **No blocking on reads**
- **Optimistic concurrency control**
- **Write conflicts detected at commit time**

```sql
-- PostgreSQL example
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;  -- Uses snapshot isolation

-- SQL Server example
SET TRANSACTION ISOLATION LEVEL SNAPSHOT;

-- Example: No read blocking
-- Transaction 1:
BEGIN TRANSACTION;
SELECT * FROM accounts;  -- Sees snapshot at T1 start time

-- Transaction 2 updates and commits:
UPDATE accounts SET balance = balance + 100 WHERE account_id = 1;

-- Transaction 1 continues:
SELECT * FROM accounts;  -- Still sees original snapshot
COMMIT;
```

### Isolation Level Comparison Table

| Isolation Level | Dirty Read | Non-repeatable Read | Phantom Read | Performance | Concurrency |
|----------------|------------|--------------------|--------------| ------------|-------------|
| Read Uncommitted | ❌ Allowed | ❌ Allowed | ❌ Allowed | Highest | Highest |
| Read Committed | ✅ Prevented | ❌ Allowed | ❌ Allowed | High | High |
| Repeatable Read | ✅ Prevented | ✅ Prevented | ❌ Allowed | Medium | Medium |
| Serializable | ✅ Prevented | ✅ Prevented | ✅ Prevented | Lowest | Lowest |
| Snapshot | ✅ Prevented | ✅ Prevented | ✅ Prevented | High | High |

---

## Deadlock Detection and Prevention

**Deadlocks** occur when two or more transactions wait indefinitely for each other to release resources. Understanding detection and prevention is crucial for robust applications.

### Understanding Deadlocks

#### Classic Deadlock Scenario:
```sql
-- Transaction 1:
BEGIN TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;  -- Locks account 1
-- Transaction 1 waits here...
UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;  -- Wants account 2
COMMIT;

-- Transaction 2 (running concurrently):
BEGIN TRANSACTION;
UPDATE accounts SET balance = balance - 50 WHERE account_id = 2;   -- Locks account 2  
-- Transaction 2 waits here...
UPDATE accounts SET balance = balance + 50 WHERE account_id = 1;   -- Wants account 1
COMMIT;

-- Result: Deadlock! Each transaction holds what the other needs
```

### Deadlock Detection

#### Database Detection Mechanisms:
```sql
-- Most databases use wait-for graphs to detect cycles

-- PostgreSQL: View current locks and blocking
SELECT 
    blocked_locks.pid AS blocked_pid,
    blocked_activity.usename AS blocked_user,
    blocking_locks.pid AS blocking_pid,
    blocking_activity.usename AS blocking_user,
    blocked_activity.query AS blocked_statement,
    blocking_activity.query AS current_statement_in_blocking_process
FROM pg_catalog.pg_locks blocked_locks
JOIN pg_catalog.pg_stat_activity blocked_activity ON blocked_activity.pid = blocked_locks.pid
JOIN pg_catalog.pg_locks blocking_locks ON blocking_locks.locktype = blocked_locks.locktype
JOIN pg_catalog.pg_stat_activity blocking_activity ON blocking_activity.pid = blocking_locks.pid
WHERE NOT blocked_locks.granted;

-- SQL Server: Deadlock detection and victim selection
-- Automatically detects deadlocks and rolls back "cheapest" transaction
-- Enable deadlock monitoring:
DBCC TRACEON(1204);  -- Basic deadlock information
DBCC TRACEON(1222);  -- Detailed deadlock information
```

#### Deadlock Detection Algorithm:
1. **Build wait-for graph** - track which transactions wait for which resources
2. **Detect cycles** - cycles indicate deadlocks
3. **Choose victim** - select transaction to abort (usually least expensive)
4. **Rollback victim** - abort chosen transaction and release its locks
5. **Resume other transactions** - allow remaining transactions to proceed

### Deadlock Prevention Strategies

#### 1. Consistent Ordering
**Always acquire resources in the same order** across all transactions.

```sql
-- ❌ Bad: Inconsistent ordering can cause deadlocks
-- Transaction 1: Lock account 1, then account 2
-- Transaction 2: Lock account 2, then account 1

-- ✅ Good: Consistent ordering prevents deadlocks
-- Both transactions lock accounts in ID order
CREATE OR REPLACE FUNCTION transfer_funds(from_id INT, to_id INT, amount DECIMAL)
RETURNS VOID AS $$
DECLARE
    first_id INT := LEAST(from_id, to_id);
    second_id INT := GREATEST(from_id, to_id);
BEGIN
    -- Always lock accounts in ID order
    IF first_id = from_id THEN
        UPDATE accounts SET balance = balance - amount WHERE account_id = first_id;
        UPDATE accounts SET balance = balance + amount WHERE account_id = second_id;
    ELSE
        UPDATE accounts SET balance = balance + amount WHERE account_id = first_id;
        UPDATE accounts SET balance = balance - amount WHERE account_id = second_id;
    END IF;
END;
$$ LANGUAGE plpgsql;
```

#### 2. Timeout-based Prevention
**Set maximum wait times** for lock acquisition.

```sql
-- PostgreSQL: Set lock timeout
SET lock_timeout = '5s';

-- SQL Server: Set lock timeout (milliseconds)
SET LOCK_TIMEOUT 5000;

-- Application-level timeout handling
BEGIN TRANSACTION;
TRY
    -- Attempt operations with timeout
    UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
    UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;
    COMMIT;
CATCH
    -- Handle timeout or deadlock
    ROLLBACK;
    -- Retry with exponential backoff
END TRY;
```

#### 3. Minimize Transaction Scope
**Keep transactions short** and release locks quickly.

```sql
-- ❌ Bad: Long-running transaction holds locks
BEGIN TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
-- Expensive operations, network calls, user input...
WAITFOR DELAY '00:01:00';  -- Holding locks for 1 minute!
UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;
COMMIT;

-- ✅ Good: Minimize lock duration
-- Prepare data first
DECLARE @transfer_amount DECIMAL = CalculateTransferAmount();
DECLARE @from_account INT = GetFromAccount();
DECLARE @to_account INT = GetToAccount();

-- Quick transaction
BEGIN TRANSACTION;
UPDATE accounts SET balance = balance - @transfer_amount WHERE account_id = @from_account;
UPDATE accounts SET balance = balance + @transfer_amount WHERE account_id = @to_account;
COMMIT;
```

#### 4. Use Lower Isolation Levels
**Reduce locking requirements** when consistency permits.

```sql
-- Use Read Committed instead of Repeatable Read when possible
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

-- Use snapshot isolation to avoid blocking reads
SET TRANSACTION ISOLATION LEVEL SNAPSHOT;  -- SQL Server
```

### Deadlock Recovery Strategies

#### 1. Automatic Retry with Backoff
```sql
-- Application-level retry logic
CREATE OR REPLACE FUNCTION execute_with_retry(operation TEXT, max_retries INT)
RETURNS VOID AS $$
DECLARE
    retry_count INT := 0;
    wait_time INT;
BEGIN
    LOOP
        BEGIN
            EXECUTE operation;
            RETURN;  -- Success, exit
        EXCEPTION
            WHEN deadlock_detected THEN
                retry_count := retry_count + 1;
                IF retry_count >= max_retries THEN
                    RAISE;  -- Give up after max retries
                END IF;
                
                -- Exponential backoff with jitter
                wait_time := POWER(2, retry_count) + RANDOM() * 1000;
                PERFORM pg_sleep(wait_time / 1000.0);
        END;
    END LOOP;
END;
$$ LANGUAGE plpgsql;
```

#### 2. Graceful Degradation
```sql
-- Handle deadlocks gracefully in application
public class TransferService {
    public boolean transferFunds(int fromId, int toId, decimal amount, int maxRetries) {
        for (int attempt = 0; attempt < maxRetries; attempt++) {
            try {
                executeTransfer(fromId, toId, amount);
                return true;
            } catch (DeadlockException e) {
                if (attempt == maxRetries - 1) {
                    log.error("Transfer failed after {} attempts: {}", maxRetries, e.getMessage());
                    return false;
                }
                
                // Wait with exponential backoff
                Thread.sleep(Math.pow(2, attempt) * 100 + random.nextInt(100));
            }
        }
        return false;
    }
}
```

---

## Locking Mechanisms

**Locks** control concurrent access to database resources, ensuring data integrity while allowing maximum possible concurrency.

### Lock Types

#### Shared Locks (S)
- **Multiple transactions can hold** shared locks on same resource
- **Prevents modifications** but allows other reads
- **Acquired automatically** by SELECT statements
- **Released when no longer needed** (depends on isolation level)

```sql
-- Explicit shared lock acquisition
-- SQL Server
SELECT * FROM accounts WITH (HOLDLOCK) WHERE account_id = 1;

-- PostgreSQL  
SELECT * FROM accounts WHERE account_id = 1 FOR SHARE;

-- Lock compatibility: S + S = Compatible, S + X = Incompatible
```

#### Exclusive Locks (X)
- **Only one transaction can hold** exclusive lock
- **Prevents all other access** (reads and writes)
- **Acquired by INSERT, UPDATE, DELETE**
- **Held until transaction completes**

```sql
-- Explicit exclusive lock
-- PostgreSQL
SELECT * FROM accounts WHERE account_id = 1 FOR UPDATE;

-- SQL Server
SELECT * FROM accounts WITH (XLOCK) WHERE account_id = 1;

-- Automatic exclusive locks
UPDATE accounts SET balance = balance + 100 WHERE account_id = 1;  -- X lock
DELETE FROM accounts WHERE account_id = 1;                        -- X lock
```

#### Intent Locks
- **Signal intention** to acquire locks at lower levels
- **Improve lock manager performance**
- **Prevent conflicts** at higher granularity levels

```sql
-- Lock hierarchy with intent locks:
-- Database Level
--   Table Level (Intent Shared/Exclusive)
--     Page Level (Intent Shared/Exclusive)  
--       Row Level (Shared/Exclusive)

-- When updating a row:
-- 1. Intent Exclusive lock on table
-- 2. Intent Exclusive lock on page  
-- 3. Exclusive lock on row
```

### Lock Granularity

#### Row-Level Locking
- **Finest granularity** - locks individual rows
- **Maximum concurrency** but higher overhead
- **Default in modern databases**

```sql
-- PostgreSQL: Row-level locking by default
UPDATE accounts SET balance = balance + 100 WHERE account_id = 1;
-- Only locks the specific row with account_id = 1

-- Explicit row locking
SELECT * FROM accounts WHERE account_id = 1 FOR UPDATE;  -- Lock specific row
```

#### Page-Level Locking  
- **Locks entire data page** (usually 4KB-8KB)
- **Balance between concurrency and overhead**
- **Used when many rows on same page are accessed**

```sql
-- SQL Server can escalate to page locks
UPDATE accounts SET balance = balance + 100 
WHERE account_id BETWEEN 1 AND 100;  -- May use page locks
```

#### Table-Level Locking
- **Locks entire table**
- **Lowest overhead** but minimal concurrency
- **Used for DDL operations** and bulk operations

```sql
-- Explicit table locking
-- PostgreSQL
LOCK TABLE accounts IN EXCLUSIVE MODE;

-- SQL Server  
SELECT * FROM accounts WITH (TABLOCKX);

-- DDL operations automatically use table locks
ALTER TABLE accounts ADD COLUMN credit_limit DECIMAL(10,2);
```

### Lock Escalation

**Lock escalation** converts many fine-grained locks to fewer coarse-grained locks to reduce memory overhead.

#### Escalation Triggers:
- **Too many row locks** (typically 5000+ in SQL Server)
- **Memory pressure** in lock manager
- **Cost-based decision** by query optimizer

```sql
-- SQL Server: Monitor lock escalation
SELECT 
    object_name(object_id) as table_name,
    lock_escalation_desc,
    lock_escalation_attempt_count,
    lock_escalation_count
FROM sys.dm_db_index_operational_stats(DB_ID(), NULL, NULL, NULL)
WHERE lock_escalation_attempt_count > 0;

-- Disable lock escalation for specific table
ALTER TABLE accounts SET (LOCK_ESCALATION = DISABLE);
```

#### Controlling Escalation:
```sql
-- PostgreSQL: Generally doesn't escalate, but can tune
-- shared_preload_libraries = 'pg_stat_statements'

-- SQL Server: Control escalation behavior
-- Use ROWLOCK hint to prevent escalation
UPDATE accounts WITH (ROWLOCK) 
SET balance = balance + 100 
WHERE region = 'US';

-- Use PAGLOCK hint to allow page locks
UPDATE accounts WITH (PAGLOCK) 
SET balance = balance + 100 
WHERE region = 'US';
```

---

## Concurrency Control Strategies

**Concurrency control** ensures database consistency while maximizing transaction throughput. Different strategies make different trade-offs between consistency, performance, and complexity.

### Pessimistic Concurrency Control

**Assumes conflicts will occur** and prevents them by acquiring locks before accessing data.

#### Two-Phase Locking (2PL)
**Standard locking protocol** ensuring serializability through two phases.

##### Growing Phase:
- **Acquire locks** as needed
- **Cannot release any locks**
- **Shared locks for reads, exclusive for writes**

##### Shrinking Phase:
- **Release locks** as operations complete
- **Cannot acquire new locks**
- **All locks released at transaction end**

```sql
-- Example of 2PL in practice
BEGIN TRANSACTION;

-- Growing phase: Acquire locks
SELECT balance FROM accounts WHERE account_id = 1;     -- Acquire S lock
SELECT balance FROM accounts WHERE account_id = 2;     -- Acquire S lock  
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;  -- Upgrade to X lock

-- Shrinking phase begins when first lock is released
-- (Usually at COMMIT/ROLLBACK)
COMMIT;  -- All locks released
```

#### Strict Two-Phase Locking
**Holds all locks until transaction commits** - prevents cascading rollbacks.

```sql
-- All locks held until transaction ends
BEGIN TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;  -- X lock acquired
UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;  -- X lock acquired
-- Both locks held here until commit
COMMIT;  -- Both locks released simultaneously
```

### Optimistic Concurrency Control

**Assumes conflicts are rare** and detects them at commit time rather than preventing them.

#### Version-Based Optimistic Control
**Use version numbers** to detect conflicting updates.

```sql
-- Table with version column
CREATE TABLE accounts (
    account_id INT PRIMARY KEY,
    balance DECIMAL(10,2),
    version_number INT DEFAULT 1
);

-- Optimistic update process
-- 1. Read data with version
SELECT account_id, balance, version_number 
FROM accounts 
WHERE account_id = 1;
-- Returns: account_id=1, balance=1000, version_number=5

-- 2. Update with version check
UPDATE accounts 
SET balance = 900, version_number = version_number + 1
WHERE account_id = 1 AND version_number = 5;

-- 3. Check if update succeeded
GET @@ROWCOUNT;  -- SQL Server
GET ROW_COUNT(); -- PostgreSQL
-- If 0 rows affected, another transaction modified the record
```

#### Timestamp-Based Optimistic Control
**Use timestamps** to order transactions and detect conflicts.

```sql
-- Table with timestamp column
CREATE TABLE accounts (
    account_id INT PRIMARY KEY,
    balance DECIMAL(10,2),
    last_modified TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Application-level optimistic control
public class OptimisticTransfer {
    public boolean transferFunds(int fromId, int toId, decimal amount) {
        // Read with timestamps
        Account fromAccount = getAccountWithTimestamp(fromId);
        Account toAccount = getAccountWithTimestamp(toId);
        
        // Perform business logic
        fromAccount.balance -= amount;
        toAccount.balance += amount;
        
        // Attempt update with timestamp validation
        boolean success = updateAccountIfUnchanged(fromAccount) && 
                         updateAccountIfUnchanged(toAccount);
        
        if (!success) {
            // Conflict detected, retry
            return transferFunds(fromId, toId, amount);
        }
        
        return true;
    }
}
```

### Multiversion Concurrency Control (MVCC)

**Maintains multiple versions** of data to provide consistent snapshots without blocking.

#### How MVCC Works:
1. **Each transaction sees consistent snapshot** from its start time
2. **Reads never block writes** and vice versa
3. **Old versions maintained** until no longer needed
4. **Write conflicts resolved** at commit time

```sql
-- PostgreSQL MVCC example
-- Transaction 1:
BEGIN;
SELECT balance FROM accounts WHERE account_id = 1;  -- Sees version at T1 start

-- Transaction 2 (concurrent):
BEGIN;
UPDATE accounts SET balance = balance + 100 WHERE account_id = 1;  -- Creates new version
COMMIT;  -- New version visible to future transactions

-- Back to Transaction 1:
SELECT balance FROM accounts WHERE account_id = 1;  -- Still sees old version
COMMIT;
```

#### MVCC Benefits:
- **Readers don't block writers**
- **Writers don't block readers**  
- **Consistent snapshots** without locking
- **Improved concurrency** especially for read-heavy workloads

#### MVCC Challenges:
- **Storage overhead** for multiple versions
- **Cleanup required** for old versions (VACUUM in PostgreSQL)
- **Write conflicts** still need resolution
- **Snapshot isolation anomalies** possible

### Hybrid Strategies

#### Snapshot Isolation with Conflict Detection
**Combine MVCC reads with conflict detection** at commit time.

```sql
-- Read-write conflict detection
CREATE TABLE accounts (
    account_id INT PRIMARY KEY,
    balance DECIMAL(10,2),
    read_timestamp TIMESTAMP,
    write_timestamp TIMESTAMP
);

-- Transaction maintains read and write sets
-- At commit time, check for conflicts:
-- - No other transaction wrote to our read set after our start time
-- - No other transaction read from our write set before our commit
```

#### Lock-Free Data Structures
**Use atomic operations** for simple concurrent operations.

```sql
-- Atomic increment operations
-- PostgreSQL: Use atomic functions where possible
UPDATE counters SET value = value + 1 WHERE counter_name = 'page_views';

-- Application level: Use compare-and-swap
public class AtomicAccount {
    private volatile long balance;
    
    public boolean withdraw(long amount) {
        long currentBalance;
        long newBalance;
        
        do {
            currentBalance = balance;
            if (currentBalance < amount) {
                return false;  // Insufficient funds
            }
            newBalance = currentBalance - amount;
        } while (!compareAndSet(currentBalance, newBalance));
        
        return true;
    }
}
```

### Strategy Selection Guidelines

#### Choose Pessimistic When:
- **High conflict probability**
- **Short transaction duration**  
- **Consistency critical**
- **Simple conflict resolution**

#### Choose Optimistic When:
- **Low conflict probability**
- **Read-heavy workloads**
- **Long transaction duration**
- **Can handle retry logic**

#### Choose MVCC When:
- **Many concurrent readers**
- **Mix of read and write transactions**
- **Snapshot consistency required**
- **Can handle storage overhead**

### Performance Monitoring

#### Lock Monitoring Queries:
```sql
-- PostgreSQL: Monitor lock contention
SELECT 
    schemaname,
    tablename,
    attname,
    n_tup_ins + n_tup_upd + n_tup_del as total_writes,
    n_tup_hot_upd,
    n_dead_tup,
    last_vacuum,
    last_autovacuum
FROM pg_stat_user_tables 
ORDER BY total_writes DESC;

-- SQL Server: Monitor blocking
SELECT 
    r.session_id,
    r.wait_type,
    r.wait_time,
    r.blocking_session_id,
    s.program_name,
    t.text
FROM sys.dm_exec_requests r
JOIN sys.dm_exec_sessions s ON r.session_id = s.session_id
CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) t
WHERE r.blocking_session_id > 0;
```
