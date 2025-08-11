# SQL Interview Questions - Quick Reference List

## 📚 **4-Week Study Plan: 52 Questions Total**

---

## **🟢 WEEK 1: FOUNDATION LEVEL (16 Questions)**

### **Day 1-2: Basic SELECT & Filtering (4 Questions)**
1. **Find Customer Referee** ⭐ - Return customers who were not referred by ID 2
2. **Big Countries** ⭐ - List countries with population >= 25M or area >= 3M km²
3. **Duplicate Emails** - Identify all email addresses that appear more than once
4. **Classes More Than 5 Students** - Get classes with at least 5 unique students

### **Day 3-4: GROUP BY & Aggregations (4 Questions)**
5. **Combine Two Tables** - Use LEFT JOIN to get complete person info, even if address is missing
6. **Count Student Number in Departments** - Count number of students per department
7. **Customers Who Never Order** - List all customers who never placed any orders
8. **Customer Placing the Largest Number of Orders** - Find the customer who placed the most orders

### **Day 5-6: Basic JOINs (4 Questions)**
9. **Employees Earning More Than Their Managers** - Return names of employees whose salary is greater than their manager's salary
10. **Rising Temperature** - Find dates where the temperature was higher than the previous day
11. **Employee Bonus** - Select employee names and bonus amounts, show NULL if no bonus exists
12. **Sales Person** - Find all salespersons who did not have any orders related to company 'RED'

### **Day 7: Subqueries Introduction (4 Questions)**
13. **Second Highest Salary** - Find the second highest unique salary from the Employee table
14. **Game Play Analysis I** - Find the first login date for each player
15. **Tree Node** - Classify each node as Root, Leaf, or Inner node in a tree structure
16. **Not Boring Movies** - Find movies with odd ID and description not equal to 'boring', ordered by rating

---

## **🟡 WEEK 2: INTERMEDIATE LEVEL (16 Questions)**

### **Day 8-9: Advanced Subqueries (4 Questions)**
17. **Game Play Analysis II** - Get player ID and device ID of first login for each player
18. **Department Highest Salary** - Return the highest salary paid in each department along with the employee name
19. **Exchange Seats** - Swap seats for students sitting next to each other (odd with even)
20. **Product Sales Analysis I** - Get product name, year, and price for each sale_id

### **Day 10-11: Functions & CASE (4 Questions)**
21. **Nth Highest Salary** - Create a function to find the Nth highest unique salary in the Employee table
22. **Swap Gender Values** - Swap 'm' and 'f' values in a gender column using CASE
23. **Fix Names in a Table** - Fix names so only the first character is uppercase and the rest lowercase
24. **Group Sold Products by Date** - Group products sold on each date and count unique products

### **Day 12-13: Window Functions Introduction (4 Questions)**
25. **Rank Scores** - Rank the scores using DENSE_RANK function so that same scores have the same rank
26. **Find Cumulative Salary of an Employee** - Calculate running salary totals using window functions
27. **Duplicate Emails with Row Numbers** - Find duplicate emails and assign row numbers to identify which to keep
28. **Percentage of Users Attended Contest** - Calculate the percentage of users who attended each contest

### **Day 14: Advanced Window Functions (4 Questions)**
29. **Department Top Three Salaries** - Find top 3 highest salaries in each department
30. **Median Employee Salary** - Get median salary for each company using window functions
31. **Market Analysis I** - Find buyers who made at least one order in 2019 and count their orders
32. **Bank Account Summary II** - Find users with total balance over 10000 after all transactions

---

## **🟠 WEEK 3: ADVANCED LEVEL (8 Questions)**

### **Day 15-16: Complex Pattern Recognition (4 Questions)**
33. **Consecutive Numbers** - Find numbers that appear at least three times consecutively in the Logs table
34. **Human Traffic of Stadium** - Return records where people count was >= 100 for 3+ consecutive days
35. **Monthly Transactions I** - Find monthly transactions count and total amount for each country
36. **Immediate Food Delivery I** - Find percentage of immediate orders (order_date = customer_pref_delivery_date)

### **Day 17-18: Advanced Analytics (4 Questions)**
37. **Game Play Analysis III** - Compute average sessions per user by counting distinct event dates
38. **Game Play Analysis IV** - Calculate the fraction of players who logged in on consecutive days
39. **Student Report by Geography** - Pivot student names by continent (America, Asia, Europe)
40. **Weather Type in Each Country** - Determine if weather is Cold, Warm, or Hot based on average temperature

---

## **🔴 WEEK 4: EXPERT LEVEL (12 Questions)**

### **Day 19-20: Complex Business Logic (4 Questions)**
41. **Trips and Users** - Calculate the cancellation rate for trips on specific dates
42. **Get Highest Answer Rate Question** - Identify the question with the highest answer-to-view ratio
43. **New Users Daily Count** - Find the number of users who logged in for the first time each day
44. **Active Businesses** - Find businesses with above-average activity for their event type

### **Day 21-22: Performance & Advanced Queries (4 Questions)**
45. **Managers with at Least 5 Direct Reports** - Find managers who have at least five direct reports
46. **Find Median Given Frequency** - Calculate median from a table where numbers have frequencies
47. **Investments in 2016** - Find total investment values where TIV_2015 is not unique but (LAT, LON) is unique
48. **Friend Requests II** - Find the person with the most friends and their friend count

### **Day 23-24: Data Modification & Advanced Concepts (4 Questions)**
49. **Delete Duplicate Emails** - Remove duplicate emails, keeping only the record with the smallest Id
50. **Friend Requests I** - Find the user who sent the most friend requests
51. **Department Top Salary Alternative** - Find employees with the highest salary in each department using EXISTS
52. **Customers with Maximum Orders** - Find all customers who placed orders on consecutive days and count their orders

---

## 🎯 **Quick Practice Guide**

### **Daily Schedule (60-75 minutes):**
- **Question 1** (15 min) - Read, understand, code
- **Question 2** (15 min) - Practice, optimize  
- **Question 3** (15 min) - Apply different approach
- **Question 4** (15 min) - Explain solution aloud
- **Review** (15 min) - Document learnings

### **Difficulty Legend:**
- ⭐ **Easy** - Basic SQL concepts
- 🟡 **Medium** - Intermediate concepts with JOINs/subqueries
- 🟠 **Hard** - Advanced window functions and complex logic
- 🔴 **Expert** - Performance optimization and complex business logic

### **Progress Tracking:**
- [ ] Week 1 Complete (Questions 1-16)
- [ ] Week 2 Complete (Questions 17-32) 
- [ ] Week 3 Complete (Questions 33-40)
- [ ] Week 4 Complete (Questions 41-52)

### **Key Skills Covered:**
✅ Basic SQL (SELECT, WHERE, ORDER BY)  
✅ Aggregations (GROUP BY, HAVING)  
✅ JOINs (INNER, LEFT, RIGHT, FULL, SELF)  
✅ Subqueries & CTEs  
✅ Window Functions (RANK, ROW_NUMBER, LEAD, LAG)  
✅ String & Date Functions  
✅ Performance Optimization  
✅ Complex Business Logic  

**Start with Question 1 and build your SQL expertise systematically!** 🚀