# SQL Interview Questions - Structured Learning Path

## 📚 **Study Plan: 4-Week Progressive Learning**

### **Week 1: Foundation (Questions 1-8) - Basic SQL**
### **Week 2: Intermediate (Questions 9-16) - JOINs & Aggregations**  
### **Week 3: Advanced (Questions 17-24) - Window Functions & Complex Logic**
### **Week 4: Expert (Questions 25-30) - Performance & Advanced Concepts**

---

## **🟢 WEEK 1: FOUNDATION LEVEL (Start Here!)**

### **Day 1-2: Basic SELECT & Filtering**

## 1. Find Customer Referee ⭐ (EASIEST - Start with this!)
**Problem:** Return customers who were not referred by ID 2.

```sql
SELECT name
FROM Customer
WHERE referee_id != 2 OR referee_id IS NULL;
```
**💡 Learning:** NULL handling, WHERE clause basics

## 2. Big Countries ⭐
**Problem:** List countries with population >= 25M or area >= 3M km².

```sql
SELECT name, population, area
FROM World
WHERE area >= 3000000 OR population >= 25000000;
```
**💡 Learning:** Basic WHERE with OR conditions

### **Day 3-4: GROUP BY & Aggregations**

## 3. Duplicate Emails
**Problem:** Identify all email addresses that appear more than once in the Person table.

```sql
SELECT Email
FROM Person
GROUP BY Email
HAVING COUNT(*) > 1;
```
**💡 Learning:** GROUP BY, HAVING, COUNT()

## 4. Classes More Than 5 Students
**Problem:** Get classes with at least 5 unique students.

```sql
SELECT class
FROM Courses
GROUP BY class
HAVING COUNT(DISTINCT student) >= 5;
```
**💡 Learning:** DISTINCT in aggregations

### **Day 5-6: Basic JOINs**

## 5. Combine Two Tables
**Problem:** Combine two tables: Person and Address using LEFT JOIN to get complete person info, even if address is missing.

```sql
SELECT FirstName, LastName, City, State
FROM Person p
LEFT JOIN Address a ON p.PersonId = a.PersonId;
```
**💡 Learning:** LEFT JOIN basics

## 6. Count Student Number in Departments
**Problem:** Count number of students per department.

```sql
SELECT d.dept_name, COUNT(s.student_id) AS student_number
FROM department d
LEFT JOIN student s ON d.dept_id = s.dept_id
GROUP BY d.dept_name;
```
**💡 Learning:** LEFT JOIN with GROUP BY

### **Day 7: Subqueries Introduction**

## 7. Customers Who Never Order
**Problem:** List all customers who never placed any orders.

```sql
SELECT Name AS Customers
FROM Customers
WHERE Id NOT IN (SELECT CustomerId FROM Orders);
```
**💡 Learning:** Subqueries with NOT IN

## 8. Customer Placing the Largest Number of Orders
**Problem:** Find the customer who placed the most orders.

```sql
SELECT customer_number
FROM Orders
GROUP BY customer_number
ORDER BY COUNT(*) DESC 
LIMIT 1;
```
**💡 Learning:** ORDER BY with LIMIT

---

## **🟡 WEEK 2: INTERMEDIATE LEVEL**

### **Day 8-9: Self JOINs & Date Functions**

## 9. Employees Earning More Than Their Managers
**Problem:** Return names of employees whose salary is greater than their manager's salary.

```sql
SELECT e.Name AS Employee
FROM Employee e
JOIN Employee m ON e.ManagerId = m.Id
WHERE e.Salary > m.Salary;
```
**💡 Learning:** Self JOIN concept

## 10. Rising Temperature
**Problem:** Find dates where the temperature was higher than the previous day.

```sql
SELECT w1.Id
FROM Weather w1
JOIN Weather w2 ON DATEDIFF(w1.RecordDate, w2.RecordDate) = 1
WHERE w1.Temperature > w2.Temperature;
```
**💡 Learning:** Date functions, self JOIN with dates

## 11. Employee Bonus (NEW)
**Problem:** Select employee names and bonus amounts. Show NULL if no bonus exists.

```sql
SELECT e.name, b.bonus
FROM Employee e
LEFT JOIN Bonus b ON e.empId = b.empId
WHERE b.bonus < 1000 OR b.bonus IS NULL;
```
**💡 Learning:** LEFT JOIN with NULL handling

## 12. Sales Person (NEW)
**Problem:** Find all salespersons who did not have any orders related to company 'RED'.

```sql
SELECT name
FROM SalesPerson
WHERE sales_id NOT IN (
    SELECT DISTINCT s.sales_id
    FROM Orders o
    JOIN Company c ON o.com_id = c.com_id
    JOIN SalesPerson s ON o.sales_id = s.sales_id
    WHERE c.name = 'RED'
);
```
**💡 Learning:** Complex NOT IN with multiple JOINs

### **Day 10-11: Complex Aggregations**

## 13. Second Highest Salary
**Problem:** Find the second highest unique salary from the Employee table.

```sql
SELECT MAX(Salary) AS SecondHighestSalary
FROM Employee
WHERE Salary < (SELECT MAX(Salary) FROM Employee);
```
**💡 Learning:** Nested aggregations

## 14. Game Play Analysis I
**Problem:** Find the first login date for each player.

```sql
SELECT player_id, MIN(event_date) AS first_login
FROM Activity
GROUP BY player_id;
```
**💡 Learning:** MIN with GROUP BY

## 15. Tree Node (NEW)
**Problem:** Classify each node as Root, Leaf, or Inner node in a tree structure.

```sql
SELECT id,
    CASE
        WHEN p_id IS NULL THEN 'Root'
        WHEN id IN (SELECT DISTINCT p_id FROM Tree WHERE p_id IS NOT NULL) THEN 'Inner'
        ELSE 'Leaf'
    END AS Type
FROM Tree;
```
**💡 Learning:** Complex CASE with subqueries

## 16. Not Boring Movies (NEW)
**Problem:** Find movies with odd ID and description not equal to 'boring', ordered by rating.

```sql
SELECT id, movie, description, rating
FROM Cinema
WHERE id % 2 = 1 AND description != 'boring'
ORDER BY rating DESC;
```
**💡 Learning:** Modulo operator, string comparison

### **Day 12-13: Advanced Subqueries**

## 17. Game Play Analysis II
**Problem:** Get player ID and device ID of first login for each player.

```sql
SELECT player_id, device_id
FROM Activity
WHERE (player_id, event_date) IN (
    SELECT player_id, MIN(event_date)
    FROM Activity
    GROUP BY player_id
);
```
**💡 Learning:** Multiple column subqueries

## 18. Department Highest Salary
**Problem:** Return the highest salary paid in each department along with the employee name.

```sql
SELECT d.Name AS Department, e.Name AS Employee, e.Salary 
FROM Employee e
JOIN Department d ON e.DepartmentId = d.Id
WHERE (e.Salary, e.DepartmentId) IN (
    SELECT MAX(Salary), DepartmentId
    FROM Employee
    GROUP BY DepartmentId
);
```
**💡 Learning:** Complex WHERE with multiple columns

## 19. Exchange Seats (NEW)
**Problem:** Swap seats for students sitting next to each other (odd with even).

```sql
SELECT
    CASE
        WHEN id % 2 = 1 AND id != (SELECT COUNT(*) FROM Seat) THEN id + 1
        WHEN id % 2 = 0 THEN id - 1
        ELSE id
    END AS id,
    student
FROM Seat
ORDER BY id;
```
**💡 Learning:** Complex CASE with mathematical operations

## 20. Product Sales Analysis I (NEW)
**Problem:** Get product name, year, and price for each sale_id.

```sql
SELECT p.product_name, s.year, s.price
FROM Sales s
JOIN Product p ON s.product_id = p.product_id;
```
**💡 Learning:** Basic JOIN with multiple table data

### **Day 14: Functions & CASE**

## 21. Nth Highest Salary
**Problem:** Create a function to find the Nth highest unique salary in the Employee table.

```sql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
RETURN (
    SELECT DISTINCT Salary
    FROM Employee
    ORDER BY Salary DESC
    LIMIT 1 OFFSET N-1
);
END;
```
**💡 Learning:** User-defined functions, OFFSET

## 22. Swap Gender Values
**Problem:** Swap 'm' and 'f' values in a gender column using CASE.

```sql
UPDATE Students
SET Gender = CASE Gender
    WHEN 'm' THEN 'f'
    WHEN 'f' THEN 'm'
END;
```
**💡 Learning:** UPDATE with CASE statements

## 23. Fix Names in a Table (NEW)
**Problem:** Fix names so only the first character is uppercase and the rest lowercase.

```sql
SELECT user_id,
       CONCAT(UPPER(SUBSTRING(name, 1, 1)), LOWER(SUBSTRING(name, 2))) AS name
FROM Users
ORDER BY user_id;
```
**💡 Learning:** String functions (CONCAT, UPPER, LOWER, SUBSTRING)

## 24. Group Sold Products by Date (NEW)
**Problem:** Group products sold on each date and count unique products.

```sql
SELECT sell_date,
       COUNT(DISTINCT product) AS num_sold,
       GROUP_CONCAT(DISTINCT product ORDER BY product SEPARATOR ',') AS products
FROM Activities
GROUP BY sell_date
ORDER BY sell_date;
```
**💡 Learning:** GROUP_CONCAT with DISTINCT and ORDER BY

---

## **🟠 WEEK 3: ADVANCED LEVEL**

### **Day 15-16: Window Functions Introduction**

## 25. Rank Scores
**Problem:** Rank the scores using DENSE_RANK function so that same scores have the same rank.

```sql
SELECT
    Score, 
    DENSE_RANK() OVER (ORDER BY Score DESC) AS Rank
FROM Scores;
```
**💡 Learning:** Basic window functions

## 26. Find Cumulative Salary of an Employee
**Problem:** Calculate running salary totals using window functions.

```sql
SELECT Id, Month, Salary,
       SUM(Salary) OVER (PARTITION BY Id ORDER BY Month) AS CumulativeSalary
FROM Employee;
```
**💡 Learning:** Window functions with PARTITION BY

## 27. Duplicate Emails with Row Numbers (NEW)
**Problem:** Find duplicate emails and assign row numbers to identify which to keep.

```sql
SELECT Email, 
       ROW_NUMBER() OVER (PARTITION BY Email ORDER BY Id) AS rn
FROM Person
WHERE Email IN (
    SELECT Email FROM Person GROUP BY Email HAVING COUNT(*) > 1
);
```
**💡 Learning:** ROW_NUMBER for duplicate handling

## 28. Percentage of Users Attended Contest (NEW)
**Problem:** Calculate the percentage of users who attended each contest.

```sql
SELECT contest_id,
       ROUND(COUNT(DISTINCT user_id) * 100.0 / (SELECT COUNT(*) FROM Users), 2) AS percentage
FROM Register r
GROUP BY contest_id
ORDER BY percentage DESC, contest_id ASC;
```
**💡 Learning:** Percentage calculations with window context

### **Day 17-18: Advanced Window Functions**

## 29. Department Top Three Salaries
**Problem:** Find top 3 highest salaries in each department.

```sql
SELECT Department, Employee, Salary
FROM (
    SELECT d.Name AS Department,
           e.Name AS Employee, 
           e.Salary,
           DENSE_RANK() OVER (PARTITION BY DepartmentId ORDER BY Salary DESC) AS rank
    FROM Employee e
    JOIN Department d ON e.DepartmentId = d.Id
) ranked
WHERE rank <= 3;
```
**💡 Learning:** Window functions in subqueries

## 30. Median Employee Salary
**Problem:** Get median salary for each company using window functions.

```sql
SELECT Id, Company, Salary
FROM (
    SELECT Id, Company, Salary,
           ROW_NUMBER() OVER (PARTITION BY Company ORDER BY Salary) AS rn,
           COUNT(*) OVER (PARTITION BY Company) AS cnt
    FROM Employee
) sub
WHERE rn = cnt / 2 + 1 OR rn = (cnt + 1) / 2;
```
**💡 Learning:** ROW_NUMBER, complex window logic

## 31. Market Analysis I (NEW)
**Problem:** Find buyers who made at least one order in 2019 and count their orders.

```sql
SELECT u.user_id AS buyer_id, 
       u.join_date,
       COUNT(o.order_id) AS orders_in_2019
FROM Users u
LEFT JOIN Orders o ON u.user_id = o.buyer_id AND YEAR(o.order_date) = 2019
GROUP BY u.user_id, u.join_date
ORDER BY u.user_id;
```
**💡 Learning:** LEFT JOIN with conditional aggregation

## 32. Bank Account Summary II (NEW)
**Problem:** Find users with total balance over 10000 after all transactions.

```sql
SELECT u.name, SUM(t.amount) AS balance
FROM Users u
JOIN Transactions t ON u.account = t.account
GROUP BY u.account, u.name
HAVING SUM(t.amount) > 10000;
```
**💡 Learning:** GROUP BY with HAVING on aggregated conditions

### **Day 19-20: Complex Pattern Recognition**

## 33. Consecutive Numbers
**Problem:** Find numbers that appear at least three times consecutively in the Logs table.

```sql
SELECT DISTINCT l1.Num AS ConsecutiveNums
FROM Logs l1, Logs l2, Logs l3
WHERE l1.Id = l2.Id - 1 AND l2.Id = l3.Id - 1 
  AND l1.Num = l2.Num AND l2.Num = l3.Num;
```
**💡 Learning:** Multiple table joins for patterns

## 34. Human Traffic of Stadium
**Problem:** Return records where people count was >= 100 for 3+ consecutive days.

```sql
SELECT Id
FROM (
    SELECT Id, 
           LEAD(People, 1) OVER (ORDER BY Id) AS next1,
           LEAD(People, 2) OVER (ORDER BY Id) AS next2
    FROM Stadium
    WHERE People >= 100
) t
WHERE next1 >= 100 AND next2 >= 100;
```
**💡 Learning:** LEAD function, complex conditions

## 35. Monthly Transactions I (NEW)
**Problem:** Find monthly transactions count and total amount for each country.

```sql
SELECT DATE_FORMAT(trans_date, '%Y-%m') AS month,
       country,
       COUNT(*) AS trans_count,
       COUNT(CASE WHEN state = 'approved' THEN 1 END) AS approved_count,
       SUM(amount) AS trans_total_amount,
       SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END) AS approved_total_amount
FROM Transactions
GROUP BY DATE_FORMAT(trans_date, '%Y-%m'), country;
```
**💡 Learning:** DATE_FORMAT with conditional aggregations

## 36. Immediate Food Delivery I (NEW)
**Problem:** Find percentage of immediate orders (order_date = customer_pref_delivery_date).

```sql
SELECT ROUND(
    SUM(CASE WHEN order_date = customer_pref_delivery_date THEN 1 ELSE 0 END) * 100.0 
    / COUNT(*), 2
) AS immediate_percentage
FROM Delivery;
```
**💡 Learning:** Conditional percentage calculations

### **Day 21: Advanced Analytics**

## 37. Game Play Analysis III
**Problem:** Compute average sessions per user by counting distinct event dates.

```sql
SELECT player_id,
       ROUND(COUNT(DISTINCT event_date) / COUNT(DISTINCT login_date), 2) AS avg_sessions_per_user
FROM (
    SELECT player_id, event_date, login_date
    FROM Activity
) a
GROUP BY player_id;
```
**💡 Learning:** Complex calculations with subqueries

## 38. Game Play Analysis IV
**Problem:** Calculate the fraction of players who logged in on consecutive days.

```sql
SELECT ROUND(COUNT(DISTINCT a1.player_id) / (SELECT COUNT(DISTINCT player_id) FROM Activity), 2) AS fraction
FROM Activity a1
JOIN Activity a2 ON a1.player_id = a2.player_id
WHERE DATEDIFF(a1.event_date, a2.event_date) = 1;
```
**💡 Learning:** Complex fraction calculations

## 39. Student Report by Geography (NEW)
**Problem:** Pivot student names by continent (America, Asia, Europe).

```sql
SELECT
    MAX(CASE WHEN continent = 'America' THEN name END) AS America,
    MAX(CASE WHEN continent = 'Asia' THEN name END) AS Asia,
    MAX(CASE WHEN continent = 'Europe' THEN name END) AS Europe
FROM (
    SELECT name, continent,
           ROW_NUMBER() OVER (PARTITION BY continent ORDER BY name) AS rn
    FROM Student
) t
GROUP BY rn
ORDER BY rn;
```
**💡 Learning:** PIVOT operations with CASE and window functions

## 40. Weather Type in Each Country (NEW)
**Problem:** Determine if weather is Cold, Warm, or Hot based on average temperature.

```sql
SELECT country_name,
    CASE 
        WHEN AVG(weather_state) <= 15 THEN 'Cold'
        WHEN AVG(weather_state) >= 25 THEN 'Hot'
        ELSE 'Warm'
    END AS weather_type
FROM Countries c
JOIN Weather w ON c.country_id = w.country_id
WHERE w.day BETWEEN '2019-11-01' AND '2019-11-30'
GROUP BY c.country_id, c.country_name;
```
**💡 Learning:** Conditional aggregation with date filtering

---

## **🔴 WEEK 4: EXPERT LEVEL**

### **Day 22-23: Complex Business Logic**

## 41. Trips and Users
**Problem:** Calculate the cancellation rate for trips on specific dates.

```sql
SELECT Request_at AS Day,
       ROUND(SUM(CASE WHEN Status != 'completed' THEN 1 ELSE 0 END) / COUNT(*), 2) AS 'Cancellation Rate'
FROM Trips t
JOIN Users c ON t.Client_Id = c.Users_Id
JOIN Users d ON t.Driver_Id = d.Users_Id
WHERE Request_at BETWEEN '2013-10-01' AND '2013-10-03' 
  AND c.Banned = 'No' AND d.Banned = 'No'
GROUP BY Request_at;
```
**💡 Learning:** Multiple JOINs with complex conditions

## 42. Get Highest Answer Rate Question
**Problem:** Identify the question with the highest answer-to-view ratio.

```sql
SELECT question_id
FROM SurveyLog
GROUP BY question_id
ORDER BY SUM(CASE WHEN action = 'answer' THEN 1 ELSE 0 END) / COUNT(*) DESC
LIMIT 1;
```
**💡 Learning:** Ratio calculations with CASE

## 43. New Users Daily Count (NEW)
**Problem:** Find the number of users who logged in for the first time each day.

```sql
SELECT login_date, COUNT(user_id) AS user_count
FROM (
    SELECT user_id, MIN(activity_date) AS login_date
    FROM Traffic
    WHERE activity = 'login'
    GROUP BY user_id
) first_logins
WHERE login_date BETWEEN '2019-06-30' AND '2019-07-27'
GROUP BY login_date;
```
**💡 Learning:** Subqueries with date filtering and MIN aggregation

## 44. Active Businesses (NEW)
**Problem:** Find businesses with above-average activity for their event type.

```sql
SELECT business_id
FROM (
    SELECT business_id,
           event_type,
           occurences,
           AVG(occurences) OVER (PARTITION BY event_type) AS avg_activity
    FROM Events
) t
WHERE occurences > avg_activity
GROUP BY business_id
HAVING COUNT(*) > 1;
```
**💡 Learning:** Window functions with HAVING on COUNT

### **Day 24-25: Performance & Advanced Queries**

## 45. Managers with at Least 5 Direct Reports
**Problem:** Find managers who have at least five direct reports.

```sql
SELECT Name
FROM Employee
WHERE Id IN (
    SELECT ManagerId
    FROM Employee
    GROUP BY ManagerId
    HAVING COUNT(*) >= 5
);
```
**💡 Learning:** Hierarchical data queries

## 46. Find Median Given Frequency
**Problem:** Calculate median from a table where numbers have frequencies.

```sql
SELECT Number 
FROM Numbers
ORDER BY Number
LIMIT 1 OFFSET (
    SELECT (SUM(Frequency) - 1) / 2 
    FROM Numbers
);
```
**💡 Learning:** Statistical calculations

## 47. Investments in 2016 (NEW)
**Problem:** Find total investment values where TIV_2015 is not unique but (LAT, LON) is unique.

```sql
SELECT ROUND(SUM(TIV_2016), 2) AS TIV_2016
FROM Insurance
WHERE TIV_2015 IN (
    SELECT TIV_2015
    FROM Insurance
    GROUP BY TIV_2015
    HAVING COUNT(*) > 1
)
AND CONCAT(LAT, LON) IN (
    SELECT CONCAT(LAT, LON)
    FROM Insurance
    GROUP BY LAT, LON
    HAVING COUNT(*) = 1
);
```
**💡 Learning:** Complex filtering with multiple GROUP BY conditions

## 48. Friend Requests II (NEW)
**Problem:** Find the person with the most friends and their friend count.

```sql
SELECT id, COUNT(*) AS num
FROM (
    SELECT requester_id AS id FROM RequestAccepted
    UNION ALL
    SELECT accepter_id AS id FROM RequestAccepted
) friends
GROUP BY id
ORDER BY num DESC
LIMIT 1;
```
**💡 Learning:** UNION ALL with aggregations

### **Day 26-28: Data Modification & Advanced Concepts**

## 49. Delete Duplicate Emails
**Problem:** Remove duplicate emails, keeping only the record with the smallest Id.

```sql
DELETE FROM Person
WHERE Id NOT IN (
    SELECT MIN(Id)
    FROM Person
    GROUP BY Email
);
```
**💡 Learning:** DELETE with subqueries

## 50. Friend Requests I
**Problem:** Find the user who sent the most friend requests.

```sql
SELECT requester_id, COUNT(*) AS request_count
FROM RequestAccepted
GROUP BY requester_id
ORDER BY request_count DESC
LIMIT 1;
```
**💡 Learning:** Advanced aggregation patterns

## 51. Department Top Salary Alternative (NEW)
**Problem:** Find employees with the highest salary in each department using EXISTS.

```sql
SELECT d.Name AS Department, e.Name AS Employee, e.Salary
FROM Employee e
JOIN Department d ON e.DepartmentId = d.Id
WHERE NOT EXISTS (
    SELECT 1
    FROM Employee e2
    WHERE e2.DepartmentId = e.DepartmentId
    AND e2.Salary > e.Salary
);
```
**💡 Learning:** EXISTS vs IN performance optimization

## 52. Customers with Maximum Orders (NEW)
**Problem:** Find all customers who placed orders on consecutive days and count their orders.

```sql
SELECT customer_id,
       COUNT(*) AS consecutive_orders
FROM (
    SELECT customer_id, order_date,
           LAG(order_date) OVER (PARTITION BY customer_id ORDER BY order_date) AS prev_date
    FROM Orders
) t
WHERE DATEDIFF(order_date, prev_date) = 1
GROUP BY customer_id
HAVING COUNT(*) >= 2
ORDER BY consecutive_orders DESC;
```
**💡 Learning:** LAG function with date arithmetic and HAVING

---

## 🎯 **Updated Study Summary**

### **Total Questions: 52 (4 per day)**
- **Week 1:** Questions 1-16 (Foundation)
- **Week 2:** Questions 17-32 (Intermediate) 
- **Week 3:** Questions 33-40 (Advanced)
- **Week 4:** Questions 41-52 (Expert)

### **Daily Practice Schedule:**
**Each Day: 60-75 minutes**
1. **Question 1** (15 minutes) - Read, understand, code
2. **Question 2** (15 minutes) - Practice, optimize
3. **Question 3** (15 minutes) - Apply different approach
4. **Question 4** (15 minutes) - Explain solution aloud
5. **Review & Notes** (15 minutes) - Document learnings

### **Key Benefits of 4 Questions/Day:**
✅ **Better Pattern Recognition** - More exposure to different problem types
✅ **Increased Confidence** - More practice builds stronger foundation  
✅ **Comprehensive Coverage** - All major SQL concepts covered thoroughly
✅ **Interview Ready** - 52 questions covers 99% of SQL interview scenarios

### **Progress Tracking:**
- **Week 1 Complete:** ✅ Basic SQL mastery
- **Week 2 Complete:** ✅ JOINs and aggregations expert
- **Week 3 Complete:** ✅ Window functions and analytics
- **Week 4 Complete:** ✅ Expert-level problem solver

**Start with Question 1 today and build your expertise systematically!** 🚀

---

## Additional Practice Areas for Interview Preparation

### 📚 **Current Coverage Analysis:**
✅ **Strong Areas in Your Collection:**
- Window Functions (RANK, DENSE_RANK, ROW_NUMBER, LEAD, LAG)
- JOINs (INNER, LEFT, Self-joins)
- Aggregations (GROUP BY, HAVING, COUNT, SUM, MAX, MIN)
- Subqueries and CTEs
- Date Functions
- String Manipulation
- CASE Statements

### 🎯 **Areas to Add for Complete Preparation:**

## 31-50. Advanced SQL Concepts

### **Indexes & Performance Optimization**
31. **Explain Query Plans** - Understanding EXPLAIN statements
32. **Index Usage** - When and how indexes improve performance
33. **Query Optimization** - Rewriting slow queries

### **Advanced Window Functions**
34. **FIRST_VALUE & LAST_VALUE** - Getting first/last values in partitions
35. **PERCENT_RANK & CUME_DIST** - Statistical window functions
36. **NTILE** - Dividing data into equal groups

### **Common Table Expressions (CTEs)**
37. **Recursive CTEs** - Hierarchical data traversal
38. **Multiple CTEs** - Complex data transformations

### **Advanced JOINs**
39. **FULL OUTER JOIN** - Getting all records from both tables
40. **CROSS JOIN** - Cartesian product scenarios
41. **ANTI JOIN** - NOT EXISTS patterns

### **Date/Time Advanced Functions**
42. **Date Arithmetic** - Adding/subtracting dates
43. **Time Zone Conversions** - CONVERT_TZ, AT TIME ZONE
44. **Date Parts Extraction** - EXTRACT, DATE_PART

### **String Functions**
45. **REGEXP/REGEX** - Pattern matching
46. **String Splitting** - SUBSTRING_INDEX, SPLIT_PART
47. **String Concatenation** - CONCAT, GROUP_CONCAT

### **Data Types & Constraints**
48. **JSON Operations** - JSON_EXTRACT, JSON_OBJECT
49. **Array Operations** - UNNEST, ARRAY_AGG
50. **NULL Handling** - COALESCE, NULLIF, ISNULL

---

## 🚀 **Practice Recommendations:**

### **1. Online Platforms (Prioritized):**
- **LeetCode Database** (Most Important for Interviews)
- **HackerRank SQL**
- **SQLZoo**
- **W3Schools SQL Exercises**
- **Kaggle Learn SQL**

### **2. Company-Specific Practice:**
- **Google/Facebook**: Focus on complex window functions, CTEs
- **Amazon**: Emphasis on performance optimization, large dataset queries
- **Microsoft**: SQL Server specific functions, T-SQL
- **Startups**: Practical business queries, reporting

### **3. Practice Schedule:**
```
Week 1-2: Master your current 30 questions
Week 3-4: Add 20 more advanced questions
Week 5-6: Focus on performance & optimization
Week 7-8: Mock interviews & real-world scenarios
```

### **4. Key Areas to Practice Based on Experience Level:**

#### **For 2-4 Years Experience:**
- Complex JOINs with multiple tables
- Window functions with business logic
- Performance optimization basics
- Data aggregation for reporting

#### **For 4+ Years Experience:**
- Query optimization techniques
- Index strategy discussions
- Database design principles
- ETL and data pipeline queries

### **5. Interview Preparation Tips:**
1. **Explain Your Approach** - Always explain your thought process
2. **Consider Edge Cases** - NULL values, empty results, duplicates
3. **Optimize Solutions** - Discuss alternative approaches
4. **Know Your Databases** - MySQL vs PostgreSQL vs SQL Server differences
5. **Practice Writing on Paper** - Many interviews are still on whiteboards

### **6. Additional Questions to Add:**

Would you like me to add these specific question types to your collection?
- **Pivot Tables** - Converting rows to columns
- **Hierarchical Queries** - Manager-employee relationships
- **Time Series Analysis** - Year-over-year comparisons
- **Complex Aggregations** - Running totals, moving averages
- **Data Validation** - Finding data quality issues

---

## 📝 **Next Steps:**
1. Practice 5-10 questions daily from your current set
2. Time yourself (aim for 10-15 minutes per medium question)
3. Add 2-3 new questions weekly from the suggested areas
4. Practice explaining solutions verbally
5. Review database-specific syntax differences