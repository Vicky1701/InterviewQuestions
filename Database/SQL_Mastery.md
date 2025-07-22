# SQL Mastery - Advanced Techniques

## Complex Queries with JOINs

**JOINs** combine data from multiple tables based on related columns. Understanding when and how to use each type is crucial for complex data retrieval.

### INNER JOIN
- Returns **only matching records** from both tables
- Most commonly used JOIN type
- Excludes records that don't have matches in either table

```sql
-- Get customers with their orders
SELECT c.customer_name, o.order_date, o.total_amount
FROM customers c
INNER JOIN orders o ON c.customer_id = o.customer_id;
```

### LEFT JOIN (LEFT OUTER JOIN)
- Returns **all records from left table** + matching records from right table
- NULL values for right table when no match exists
- Useful for finding records that may or may not have related data

```sql
-- Get all customers, including those without orders
SELECT c.customer_name, o.order_date, o.total_amount
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id;
```

### RIGHT JOIN (RIGHT OUTER JOIN)
- Returns **all records from right table** + matching records from left table
- Less commonly used (can usually be rewritten as LEFT JOIN)
- NULL values for left table when no match exists

```sql
-- Get all orders, including orphaned orders (rare scenario)
SELECT c.customer_name, o.order_date, o.total_amount
FROM customers c
RIGHT JOIN orders o ON c.customer_id = o.customer_id;
```

### FULL OUTER JOIN
- Returns **all records from both tables**
- Shows matches where they exist, NULL where they don't
- Useful for data reconciliation and finding mismatches

```sql
-- Get all customers and orders, showing mismatches
SELECT c.customer_name, o.order_date, o.total_amount
FROM customers c
FULL OUTER JOIN orders o ON c.customer_id = o.customer_id;
```

### Complex Multi-Table JOINs
```sql
-- Join multiple tables with different JOIN types
SELECT 
    c.customer_name,
    o.order_date,
    p.product_name,
    oi.quantity,
    oi.price
FROM customers c
INNER JOIN orders o ON c.customer_id = o.customer_id
INNER JOIN order_items oi ON o.order_id = oi.order_id
LEFT JOIN products p ON oi.product_id = p.product_id
WHERE o.order_date >= '2024-01-01';
```

---

## Subqueries and CTEs

### Subqueries
**Subqueries** are queries nested inside other queries, useful for complex filtering and calculations.

#### WHERE Clause Subqueries
```sql
-- Find customers who placed orders above average order value
SELECT customer_name
FROM customers
WHERE customer_id IN (
    SELECT customer_id
    FROM orders
    WHERE total_amount > (
        SELECT AVG(total_amount)
        FROM orders
    )
);
```

#### SELECT Clause Subqueries
```sql
-- Add calculated columns using subqueries
SELECT 
    customer_name,
    (SELECT COUNT(*) FROM orders o WHERE o.customer_id = c.customer_id) as order_count,
    (SELECT MAX(total_amount) FROM orders o WHERE o.customer_id = c.customer_id) as max_order
FROM customers c;
```

#### FROM Clause Subqueries
```sql
-- Use subquery results as a virtual table
SELECT avg_by_month.order_month, avg_by_month.avg_amount
FROM (
    SELECT 
        DATE_FORMAT(order_date, '%Y-%m') as order_month,
        AVG(total_amount) as avg_amount
    FROM orders
    GROUP BY DATE_FORMAT(order_date, '%Y-%m')
) avg_by_month
WHERE avg_by_month.avg_amount > 1000;
```

### Common Table Expressions (CTEs)
**CTEs** provide a more readable way to write complex queries with temporary named result sets.

#### Basic CTE
```sql
-- Same query as above, but more readable with CTE
WITH monthly_averages AS (
    SELECT 
        DATE_FORMAT(order_date, '%Y-%m') as order_month,
        AVG(total_amount) as avg_amount
    FROM orders
    GROUP BY DATE_FORMAT(order_date, '%Y-%m')
)
SELECT order_month, avg_amount
FROM monthly_averages
WHERE avg_amount > 1000;
```

#### Multiple CTEs
```sql
-- Multiple CTEs for complex analysis
WITH customer_stats AS (
    SELECT 
        customer_id,
        COUNT(*) as order_count,
        SUM(total_amount) as total_spent
    FROM orders
    GROUP BY customer_id
),
high_value_customers AS (
    SELECT customer_id
    FROM customer_stats
    WHERE total_spent > 5000
)
SELECT c.customer_name, cs.order_count, cs.total_spent
FROM customers c
INNER JOIN customer_stats cs ON c.customer_id = cs.customer_id
INNER JOIN high_value_customers hvc ON c.customer_id = hvc.customer_id;
```

#### Recursive CTEs
```sql
-- Recursive CTE for hierarchical data (employee management structure)
WITH RECURSIVE employee_hierarchy AS (
    -- Base case: top-level managers
    SELECT employee_id, name, manager_id, 0 as level
    FROM employees
    WHERE manager_id IS NULL
    
    UNION ALL
    
    -- Recursive case: employees with managers
    SELECT e.employee_id, e.name, e.manager_id, eh.level + 1
    FROM employees e
    INNER JOIN employee_hierarchy eh ON e.manager_id = eh.employee_id
)
SELECT * FROM employee_hierarchy ORDER BY level, name;
```

---

## Window Functions

**Window functions** perform calculations across rows related to the current row without collapsing results like GROUP BY.

### ROW_NUMBER()
- Assigns **unique sequential numbers** to rows
- Useful for pagination and removing duplicates

```sql
-- Assign row numbers within each customer's orders
SELECT 
    customer_id,
    order_date,
    total_amount,
    ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date) as order_sequence
FROM orders;

-- Remove duplicates using ROW_NUMBER
WITH numbered_rows AS (
    SELECT *,
        ROW_NUMBER() OVER (PARTITION BY customer_email ORDER BY created_date) as rn
    FROM customers
)
SELECT * FROM numbered_rows WHERE rn = 1;
```

### RANK() and DENSE_RANK()
- **RANK()**: Assigns ranks with gaps for ties
- **DENSE_RANK()**: Assigns ranks without gaps for ties

```sql
-- Rank customers by total spending
SELECT 
    customer_name,
    total_spent,
    RANK() OVER (ORDER BY total_spent DESC) as spending_rank,
    DENSE_RANK() OVER (ORDER BY total_spent DESC) as dense_spending_rank
FROM (
    SELECT c.customer_name, SUM(o.total_amount) as total_spent
    FROM customers c
    INNER JOIN orders o ON c.customer_id = o.customer_id
    GROUP BY c.customer_id, c.customer_name
) customer_totals;
```

### LAG() and LEAD()
- **LAG()**: Access previous row's value
- **LEAD()**: Access next row's value
- Useful for comparing with previous/next records

```sql
-- Calculate month-over-month sales growth
WITH monthly_sales AS (
    SELECT 
        DATE_FORMAT(order_date, '%Y-%m') as order_month,
        SUM(total_amount) as monthly_total
    FROM orders
    GROUP BY DATE_FORMAT(order_date, '%Y-%m')
)
SELECT 
    order_month,
    monthly_total,
    LAG(monthly_total) OVER (ORDER BY order_month) as previous_month,
    ROUND(
        ((monthly_total - LAG(monthly_total) OVER (ORDER BY order_month)) / 
         LAG(monthly_total) OVER (ORDER BY order_month)) * 100, 2
    ) as growth_percentage
FROM monthly_sales;
```

### Other Useful Window Functions
```sql
-- FIRST_VALUE and LAST_VALUE
SELECT 
    customer_id,
    order_date,
    total_amount,
    FIRST_VALUE(total_amount) OVER (PARTITION BY customer_id ORDER BY order_date) as first_order_amount,
    LAST_VALUE(total_amount) OVER (
        PARTITION BY customer_id 
        ORDER BY order_date 
        ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
    ) as last_order_amount
FROM orders;

-- NTILE for creating buckets
SELECT 
    customer_name,
    total_spent,
    NTILE(4) OVER (ORDER BY total_spent) as spending_quartile
FROM customer_spending_summary;
```

---

## Aggregate Functions and GROUP BY Operations

### Basic Aggregate Functions
```sql
-- Standard aggregates
SELECT 
    customer_id,
    COUNT(*) as order_count,
    SUM(total_amount) as total_spent,
    AVG(total_amount) as avg_order_value,
    MIN(order_date) as first_order,
    MAX(order_date) as last_order
FROM orders
GROUP BY customer_id;
```

### Advanced Grouping
```sql
-- GROUP BY with multiple columns
SELECT 
    YEAR(order_date) as order_year,
    MONTH(order_date) as order_month,
    COUNT(*) as order_count,
    SUM(total_amount) as monthly_revenue
FROM orders
GROUP BY YEAR(order_date), MONTH(order_date)
ORDER BY order_year, order_month;
```

### HAVING Clause
```sql
-- Filter groups using HAVING
SELECT 
    customer_id,
    COUNT(*) as order_count,
    SUM(total_amount) as total_spent
FROM orders
GROUP BY customer_id
HAVING COUNT(*) >= 5 AND SUM(total_amount) > 1000;
```

### ROLLUP and CUBE (Advanced Grouping)
```sql
-- ROLLUP for subtotals and grand totals
SELECT 
    YEAR(order_date) as order_year,
    MONTH(order_date) as order_month,
    SUM(total_amount) as revenue
FROM orders
GROUP BY ROLLUP(YEAR(order_date), MONTH(order_date));

-- CUBE for all possible combinations
SELECT 
    customer_type,
    region,
    SUM(total_amount) as revenue
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
GROUP BY CUBE(customer_type, region);
```

### Conditional Aggregation
```sql
-- Use CASE with aggregates for conditional calculations
SELECT 
    customer_id,
    COUNT(*) as total_orders,
    COUNT(CASE WHEN total_amount > 500 THEN 1 END) as high_value_orders,
    SUM(CASE WHEN YEAR(order_date) = 2024 THEN total_amount ELSE 0 END) as revenue_2024,
    AVG(CASE WHEN status = 'completed' THEN total_amount END) as avg_completed_order
FROM orders
GROUP BY customer_id;
```

---

## Query Optimization and Execution Plans

### Understanding Execution Plans
**Execution plans** show how the database engine processes your query, helping identify bottlenecks.

#### Reading Execution Plans
- **Table Scans**: Reading entire table (slow for large tables)
- **Index Seeks**: Using indexes to find specific rows (fast)
- **Nested Loops**: Joining small result sets
- **Hash Joins**: Joining larger result sets
- **Sort Operations**: Ordering data (can be expensive)

### Common Optimization Techniques

#### 1. Proper Indexing
```sql
-- Create indexes on frequently queried columns
CREATE INDEX idx_customer_email ON customers(email);
CREATE INDEX idx_order_date ON orders(order_date);
CREATE INDEX idx_composite ON orders(customer_id, order_date);

-- Covering indexes include all needed columns
CREATE INDEX idx_covering ON orders(customer_id, order_date) INCLUDE (total_amount);
```

#### 2. Query Structure Optimization
```sql
-- BAD: Functions on columns prevent index usage
SELECT * FROM orders WHERE YEAR(order_date) = 2024;

-- GOOD: Use range conditions instead
SELECT * FROM orders WHERE order_date >= '2024-01-01' AND order_date < '2025-01-01';

-- BAD: Leading wildcards prevent index usage
SELECT * FROM customers WHERE customer_name LIKE '%john%';

-- GOOD: Leading characters allow index usage
SELECT * FROM customers WHERE customer_name LIKE 'john%';
```

#### 3. JOIN Optimization
```sql
-- Use appropriate JOIN types and order
-- Smaller tables should typically be on the left in nested loop joins
SELECT c.customer_name, o.total_amount
FROM customers c
INNER JOIN orders o ON c.customer_id = o.customer_id
WHERE c.customer_type = 'premium'  -- Filter early
    AND o.order_date >= '2024-01-01';
```

#### 4. Subquery vs JOIN Performance
```sql
-- Often JOINs perform better than subqueries
-- BAD: Correlated subquery
SELECT customer_name
FROM customers c
WHERE EXISTS (
    SELECT 1 FROM orders o 
    WHERE o.customer_id = c.customer_id 
    AND o.total_amount > 1000
);

-- GOOD: Use JOIN instead
SELECT DISTINCT c.customer_name
FROM customers c
INNER JOIN orders o ON c.customer_id = o.customer_id
WHERE o.total_amount > 1000;
```

#### 5. LIMIT and Pagination
```sql
-- Efficient pagination using OFFSET/LIMIT
SELECT * FROM orders 
ORDER BY order_date DESC 
LIMIT 20 OFFSET 100;

-- Even better: cursor-based pagination
SELECT * FROM orders 
WHERE order_date < '2024-06-01T12:00:00'
ORDER BY order_date DESC 
LIMIT 20;
```

### Query Optimization Checklist
1. **Analyze execution plans** for slow queries
2. **Create appropriate indexes** on WHERE, JOIN, and ORDER BY columns
3. **Avoid functions on columns** in WHERE clauses
4. **Use specific column names** instead of SELECT *
5. **Filter data early** with WHERE clauses
6. **Consider query rewriting** (subqueries to JOINs, etc.)
7. **Update table statistics** regularly
8. **Monitor query performance** over time
9. **Use appropriate data types** to minimize storage and improve comparisons
10. **Consider partitioning** for very large tables

### Performance Monitoring
```sql
-- Check query execution time (varies by database system)
-- PostgreSQL example:
EXPLAIN ANALYZE SELECT * FROM orders WHERE customer_id = 12345;

-- SQL Server example:
SET STATISTICS TIME ON;
SELECT * FROM orders WHERE customer_id = 12345;
SET STATISTICS TIME OFF;
```
