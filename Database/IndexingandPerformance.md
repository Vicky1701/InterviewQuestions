# Indexing and Performance - Database Optimization

## Index Types

**Indexes** are data structures that improve query performance by providing faster access paths to table data. Understanding different index types helps you choose the right tool for each scenario.

### B-Tree Indexes
**Most common index type** - balanced tree structure that maintains sorted data.

#### Characteristics:
- **Balanced tree structure** with root, intermediate, and leaf nodes
- **Maintains sort order** - excellent for range queries
- **Logarithmic search time** O(log n)
- **Supports equality and range operations**

#### Best Use Cases:
```sql
-- Excellent for range queries
SELECT * FROM orders WHERE order_date BETWEEN '2024-01-01' AND '2024-12-31';

-- Great for sorting operations
SELECT * FROM customers ORDER BY last_name, first_name;

-- Efficient for prefix matching
SELECT * FROM products WHERE product_name LIKE 'Widget%';

-- Creating B-tree indexes (default type in most databases)
CREATE INDEX idx_order_date ON orders(order_date);
CREATE INDEX idx_customer_name ON customers(last_name, first_name);
```

#### When B-Tree Works Best:
- High cardinality columns (many unique values)
- Range queries and sorting
- Prefix searches
- Inequality comparisons (>, <, >=, <=)

### Hash Indexes
**Hash-based structure** optimized for exact equality matches.

#### Characteristics:
- **Constant-time lookups** O(1) for equality
- **Cannot handle range queries** or sorting
- **Memory-efficient** for equality operations
- **Not all databases support** hash indexes on disk

#### Best Use Cases:
```sql
-- Perfect for exact matches
SELECT * FROM users WHERE user_id = 12345;
SELECT * FROM sessions WHERE session_token = 'abc123def456';

-- Creating hash indexes (PostgreSQL example)
CREATE INDEX USING HASH idx_user_id ON users(user_id);
```

#### When Hash Works Best:
- Exact equality searches only
- High-volume point lookups
- Join operations on equality
- Unique identifier searches

### Bitmap Indexes
**Bitmap structure** using bit arrays to represent data presence.

#### Characteristics:
- **Excellent for low cardinality** data (few unique values)
- **Very efficient storage** for sparse data
- **Fast boolean operations** (AND, OR, NOT)
- **Poor performance** for high cardinality data

#### Best Use Cases:
```sql
-- Low cardinality columns
CREATE BITMAP INDEX idx_gender ON customers(gender);
CREATE BITMAP INDEX idx_status ON orders(status);

-- Excellent for complex WHERE clauses with multiple conditions
SELECT * FROM customers 
WHERE gender = 'F' 
  AND age_group = '25-34' 
  AND customer_type = 'premium';
```

#### When Bitmap Works Best:
- Low cardinality columns (gender, status, categories)
- Data warehousing and OLAP workloads
- Complex queries with multiple WHERE conditions
- Read-heavy environments

### Clustered vs Non-Clustered Indexes

#### Clustered Indexes
- **Physical storage order** matches index order
- **Table data stored in index leaf nodes**
- **Only one per table** (data can only be physically ordered one way)
- **Automatically created** for primary keys in many databases

```sql
-- Creating clustered index (SQL Server example)
CREATE CLUSTERED INDEX idx_clustered_order_date ON orders(order_date);

-- Primary key automatically creates clustered index
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,  -- Clustered index
    customer_name VARCHAR(100)
);
```

**Benefits:**
- Fastest access for range queries on clustered column
- No additional lookup required (data is in the index)
- Efficient for queries that return multiple rows

**Drawbacks:**
- Expensive inserts if not inserted in clustered order
- Page splits can occur with random inserts
- Only one per table

#### Non-Clustered Indexes
- **Separate structure** pointing to table rows
- **Multiple indexes allowed** per table
- **Additional lookup required** to get full row data
- **More flexible** for different query patterns

```sql
-- Creating non-clustered indexes
CREATE NONCLUSTERED INDEX idx_customer_email ON customers(email);
CREATE INDEX idx_order_customer ON orders(customer_id);  -- Usually non-clustered
```

**Benefits:**
- Multiple indexes per table
- Less impact on insert performance
- Can be optimized for specific queries

**Drawbacks:**
- Requires key lookup for full row data
- Slightly slower than clustered for range queries
- Additional storage overhead

---

## When and How to Use Indexes Effectively

### Index Selection Strategy

#### 1. Analyze Query Patterns
```sql
-- Identify frequently used WHERE clauses
SELECT * FROM orders WHERE customer_id = ?;           -- Index on customer_id
SELECT * FROM orders WHERE order_date >= ?;          -- Index on order_date
SELECT * FROM customers WHERE email = ?;             -- Index on email
```

#### 2. Consider Column Characteristics
```sql
-- High cardinality columns (good for B-tree)
CREATE INDEX idx_customer_email ON customers(email);

-- Low cardinality columns (consider bitmap if supported)
CREATE INDEX idx_order_status ON orders(status);

-- Composite indexes for multi-column queries
CREATE INDEX idx_customer_date ON orders(customer_id, order_date);
```

### Composite Index Guidelines

#### Column Order Matters
```sql
-- Index: (customer_id, order_date, status)
-- ✅ Can use index
SELECT * FROM orders WHERE customer_id = 123;
SELECT * FROM orders WHERE customer_id = 123 AND order_date >= '2024-01-01';
SELECT * FROM orders WHERE customer_id = 123 AND order_date >= '2024-01-01' AND status = 'pending';

-- ❌ Cannot use index efficiently
SELECT * FROM orders WHERE order_date >= '2024-01-01';  -- Skips first column
SELECT * FROM orders WHERE status = 'pending';          -- Skips first columns
```

#### Index Column Order Rules:
1. **Most selective columns first** (highest cardinality)
2. **Equality conditions before range conditions**
3. **Consider query frequency** - most common queries first

```sql
-- Good composite index design
CREATE INDEX idx_orders_comprehensive ON orders(
    customer_id,     -- High selectivity, equality
    status,          -- Medium selectivity, equality  
    order_date       -- Range queries, put last
);
```

### Covering Indexes
**Include all columns** needed by a query to avoid table lookups.

```sql
-- Query needs customer_id, order_date, and total_amount
SELECT customer_id, order_date, total_amount 
FROM orders 
WHERE customer_id = 123;

-- Covering index includes all needed columns
CREATE INDEX idx_orders_covering ON orders(customer_id) 
INCLUDE (order_date, total_amount);

-- Or composite index with all columns
CREATE INDEX idx_orders_all ON orders(customer_id, order_date, total_amount);
```

### Partial Indexes
**Index only subset** of rows to save space and improve performance.

```sql
-- Only index active customers
CREATE INDEX idx_active_customers ON customers(customer_id) 
WHERE status = 'active';

-- Only index recent orders
CREATE INDEX idx_recent_orders ON orders(order_date) 
WHERE order_date >= '2024-01-01';
```

### Index Maintenance Considerations

#### When NOT to Create Indexes:
- **Small tables** (< 1000 rows) - table scan is faster
- **Frequently updated columns** - index maintenance overhead
- **Very low selectivity** queries - won't improve performance
- **Columns with many NULLs** (unless specifically needed)

#### Index Overhead:
- **Storage space** - indexes require additional disk space
- **Insert/Update/Delete performance** - indexes must be maintained
- **Memory usage** - indexes compete for buffer cache space

---

## Query Performance Tuning

### Systematic Tuning Approach

#### 1. Identify Slow Queries
```sql
-- Find slow queries (varies by database)
-- PostgreSQL example:
SELECT query, mean_time, calls, total_time
FROM pg_stat_statements 
ORDER BY mean_time DESC 
LIMIT 10;

-- SQL Server example:
SELECT 
    qs.total_elapsed_time / qs.execution_count AS avg_time,
    qs.execution_count,
    SUBSTRING(qt.text, qs.statement_start_offset/2+1,
        (CASE WHEN qs.statement_end_offset = -1 
         THEN LEN(CONVERT(nvarchar(max), qt.text)) * 2 
         ELSE qs.statement_end_offset END - qs.statement_start_offset)/2
    ) AS query_text
FROM sys.dm_exec_query_stats qs
CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) qt
ORDER BY avg_time DESC;
```

#### 2. Analyze Query Structure
```sql
-- Inefficient query patterns to avoid
-- ❌ Function on column prevents index usage
SELECT * FROM orders WHERE YEAR(order_date) = 2024;

-- ✅ Range condition allows index usage  
SELECT * FROM orders WHERE order_date >= '2024-01-01' AND order_date < '2025-01-01';

-- ❌ Leading wildcard prevents index usage
SELECT * FROM customers WHERE name LIKE '%john%';

-- ✅ Prefix allows index usage
SELECT * FROM customers WHERE name LIKE 'john%';
```

#### 3. Optimize JOIN Operations
```sql
-- JOIN order optimization
-- ✅ Filter early and join smaller result sets
SELECT c.name, o.total_amount
FROM customers c
INNER JOIN (
    SELECT customer_id, total_amount 
    FROM orders 
    WHERE order_date >= '2024-01-01'
) o ON c.customer_id = o.customer_id
WHERE c.status = 'active';

-- ✅ Use appropriate JOIN algorithms
-- Consider index hints if needed (database-specific)
```

### Common Performance Patterns

#### Efficient Pagination
```sql
-- ❌ OFFSET becomes slower with higher values
SELECT * FROM orders ORDER BY order_id LIMIT 20 OFFSET 10000;

-- ✅ Cursor-based pagination
SELECT * FROM orders 
WHERE order_id > 10000 
ORDER BY order_id 
LIMIT 20;
```

#### Optimized Aggregation
```sql
-- ✅ Use indexes for GROUP BY operations
CREATE INDEX idx_orders_customer_date ON orders(customer_id, order_date);

SELECT customer_id, COUNT(*), SUM(total_amount)
FROM orders
GROUP BY customer_id;  -- Uses index for grouping
```

#### Subquery vs JOIN Performance
```sql
-- ❌ Correlated subquery (often slower)
SELECT c.name
FROM customers c
WHERE EXISTS (
    SELECT 1 FROM orders o 
    WHERE o.customer_id = c.customer_id 
    AND o.total_amount > 1000
);

-- ✅ JOIN (often faster)
SELECT DISTINCT c.name
FROM customers c
INNER JOIN orders o ON c.customer_id = o.customer_id
WHERE o.total_amount > 1000;
```

---

## Understanding Execution Plans

### Reading Execution Plans

**Execution plans** show the database engine's strategy for executing your query. Learning to read them is crucial for performance tuning.

#### Key Components:
- **Operators**: Table scans, index seeks, joins, sorts
- **Cost estimates**: Relative expense of each operation
- **Row estimates**: Expected number of rows processed
- **Actual vs estimated**: Compare predicted vs actual performance

#### Common Plan Operators:

##### Table Access Methods:
```sql
-- Table Scan: Reads entire table (expensive for large tables)
-- Index Scan: Reads entire index (better than table scan)
-- Index Seek: Uses index to find specific rows (fastest)

EXPLAIN (ANALYZE, BUFFERS) 
SELECT * FROM orders WHERE customer_id = 123;
```

**Interpretation:**
- **Index Seek** on customer_id = Good performance
- **Table Scan** = Missing index opportunity
- **Index Scan** = Possibly missing WHERE clause optimization

##### JOIN Algorithms:
```sql
-- Nested Loop: Good for small result sets
-- Hash Join: Good for larger result sets
-- Merge Join: Good when both inputs are sorted

EXPLAIN (ANALYZE, BUFFERS)
SELECT c.name, o.total_amount
FROM customers c
INNER JOIN orders o ON c.customer_id = o.customer_id;
```

### Plan Analysis Tools

#### PostgreSQL:
```sql
-- Basic plan
EXPLAIN SELECT * FROM orders WHERE customer_id = 123;

-- Detailed analysis with actual execution stats
EXPLAIN (ANALYZE, BUFFERS, VERBOSE) 
SELECT * FROM orders WHERE customer_id = 123;

-- Visual plan (if using pgAdmin or similar)
EXPLAIN (ANALYZE, BUFFERS, FORMAT JSON) 
SELECT * FROM orders WHERE customer_id = 123;
```

#### SQL Server:
```sql
-- Show estimated plan
SET SHOWPLAN_ALL ON;
SELECT * FROM orders WHERE customer_id = 123;
SET SHOWPLAN_ALL OFF;

-- Show actual plan with execution stats
SET STATISTICS IO ON;
SET STATISTICS TIME ON;
SELECT * FROM orders WHERE customer_id = 123;
SET STATISTICS IO OFF;
SET STATISTICS TIME OFF;
```

### Plan Optimization Strategies

#### 1. Index Optimization
- **Look for table scans** → Create appropriate indexes
- **Check index usage** → Ensure queries can use existing indexes
- **Identify missing covering indexes** → Reduce key lookups

#### 2. JOIN Optimization
- **Monitor JOIN algorithms** → Ensure appropriate algorithm selection
- **Check JOIN order** → Smaller tables should typically be accessed first
- **Verify JOIN predicates** → Ensure proper JOIN conditions

#### 3. Sorting and Grouping
- **Expensive sorts** → Create indexes to provide pre-sorted data
- **GROUP BY optimization** → Indexes on GROUP BY columns
- **ORDER BY elimination** → Use clustered indexes for natural ordering

---

## Statistics and Cardinality Estimation

### Database Statistics

**Statistics** help the query optimizer make informed decisions about the best execution plan.

#### What Statistics Track:
- **Row counts** in tables
- **Data distribution** in columns
- **Index usage patterns**
- **Column value histograms**
- **Multi-column correlation**

#### Types of Statistics:
```sql
-- Table statistics
-- - Number of rows
-- - Number of pages
-- - Average row size

-- Column statistics  
-- - Number of distinct values (cardinality)
-- - Most frequent values
-- - Data distribution histogram
-- - NULL percentage

-- Index statistics
-- - Index usage frequency
-- - Index effectiveness
-- - Key distribution
```

### Managing Statistics

#### Updating Statistics:
```sql
-- PostgreSQL: Analyze tables to update statistics
ANALYZE orders;
ANALYZE customers;
ANALYZE;  -- All tables

-- SQL Server: Update statistics
UPDATE STATISTICS orders;
UPDATE STATISTICS customers WITH FULLSCAN;

-- Oracle: Gather statistics
EXEC DBMS_STATS.GATHER_TABLE_STATS('schema', 'orders');
```

#### Viewing Statistics:
```sql
-- PostgreSQL: View table statistics
SELECT schemaname, tablename, n_live_tup, n_dead_tup, last_analyze
FROM pg_stat_user_tables;

-- View column statistics
SELECT tablename, attname, n_distinct, correlation
FROM pg_stats 
WHERE tablename = 'orders';

-- SQL Server: View statistics
SELECT 
    s.name as stats_name,
    c.name as column_name,
    stats_date(s.object_id, s.stats_id) as last_updated
FROM sys.stats s
JOIN sys.stats_columns sc ON s.object_id = sc.object_id AND s.stats_id = sc.stats_id
JOIN sys.columns c ON sc.object_id = c.object_id AND sc.column_id = c.column_id
WHERE s.object_id = object_id('orders');
```

### Cardinality Estimation

**Cardinality estimation** predicts how many rows operations will process, crucial for choosing optimal execution plans.

#### Factors Affecting Estimation:
- **Data distribution** - skewed data can cause poor estimates
- **Correlation between columns** - related columns affect selectivity
- **Statistics freshness** - outdated statistics lead to poor estimates
- **Complex predicates** - functions and expressions reduce accuracy

#### Common Estimation Problems:
```sql
-- Skewed data distribution
SELECT * FROM orders WHERE customer_id = 1;  -- VIP customer with many orders
-- Optimizer may underestimate rows if most customers have few orders

-- Correlated columns
SELECT * FROM orders WHERE customer_id = 123 AND order_date = '2024-01-01';
-- If customer 123 always orders on specific dates, estimates may be wrong

-- Parameter sniffing (SQL Server)
-- Plan cached for one parameter value may be suboptimal for others
CREATE PROC GetCustomerOrders @customer_id INT
AS
SELECT * FROM orders WHERE customer_id = @customer_id;
```

### Improving Cardinality Estimation

#### 1. Keep Statistics Current:
```sql
-- Automate statistics updates
-- PostgreSQL: Enable autovacuum
-- SQL Server: Enable auto update statistics
-- Oracle: Schedule regular DBMS_STATS jobs
```

#### 2. Create Multi-Column Statistics:
```sql
-- PostgreSQL: Extended statistics for correlated columns
CREATE STATISTICS stat_customer_date ON customer_id, order_date FROM orders;
ANALYZE orders;

-- SQL Server: Multi-column statistics
CREATE STATISTICS stat_customer_date ON orders(customer_id, order_date);
```

#### 3. Handle Parameter Sniffing:
```sql
-- SQL Server: Use OPTION(RECOMPILE) for varying parameters
SELECT * FROM orders WHERE customer_id = @customer_id
OPTION(RECOMPILE);

-- Or use plan guides for specific scenarios
-- PostgreSQL: Use prepared statements carefully
```

### Monitoring Statistics Health

#### Statistics Maintenance Checklist:
1. **Regular statistics updates** - schedule during low-activity periods
2. **Monitor estimation accuracy** - compare estimated vs actual rows in plans
3. **Identify statistics causing issues** - look for queries with poor estimates
4. **Update after bulk data changes** - large inserts/updates/deletes
5. **Create targeted statistics** - for problematic query patterns
6. **Monitor auto-update thresholds** - ensure they trigger appropriately

#### Performance Monitoring Queries:
```sql
-- Find tables with outdated statistics (PostgreSQL)
SELECT schemaname, tablename, 
       n_live_tup, 
       last_analyze,
       now() - last_analyze as time_since_analyze
FROM pg_stat_user_tables
WHERE last_analyze < now() - interval '1 week'
ORDER BY n_live_tup DESC;

-- Find queries with poor cardinality estimates (look for large differences)
-- This requires analyzing execution plans programmatically
```
