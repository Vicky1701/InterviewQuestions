# SQL Interview Problems

## 1. Joins (INNER, LEFT, RIGHT)

1. Write a query to get employee details along with department name (**INNER JOIN**).
2. Find employees who don’t have a department assigned (**LEFT JOIN**).
3. Find departments with no employees (**RIGHT JOIN**).
4. List all employees and their managers (**self-join**).

---

## 2. Aggregations with GROUP BY & HAVING

1. Find the total salary per department.
2. Find departments with more than 5 employees (**HAVING**).
3. Find average salary by job role.

---

## 3. Top N Queries

1. Query to find the 2nd highest salary in an employee table.
2. Find top 3 highest paid employees in each department.
3. Find top 3 recent orders from the orders table.

---

## 4. Subqueries vs CTE

1. Write a query to find employees earning more than the average salary (**subquery**).
2. Rewrite the same using a **CTE**.

---

## 5. Handling Duplicates

1. Find duplicate rows in a table (by name/email).
2. Write a query to delete duplicates but keep the first occurrence.

---

## 6. Date Filtering

1. Find all orders placed today.
2. Find orders placed in the last 7 days.
3. Find orders placed in the current month.

---

## 7. Window Functions

1. Use **ROW_NUMBER** to find duplicate records in a table.
2. Use **RANK** to get top 3 employees by salary (with ties).
3. Use **DENSE_RANK** to assign rank to employees based on salary.
