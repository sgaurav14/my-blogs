---
title: "Demystifying MySQL: A Beginner's Exploration with Real-world Examples"
datePublished: Mon May 06 2024 13:15:04 GMT+0000 (Coordinated Universal Time)
cuid: clvuzhvp2000009mfd9vkg7yo
slug: demystifying-mysql-a-beginners-exploration-with-real-world-examples
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1715001193524/4751d575-dcb9-492e-9b31-c6389e3f8eb3.webp
tags: data, mysql, data-science, databases, devops, dbms

---

Are you new to the world of MySQL databases? Does the terminology seem like a foreign language? Fear not! In this beginner's guide, we'll embark on a journey through MySQL's essential concepts using everyday examples to make them crystal clear. Let's jump right in!

We're delving into a bank employee database table to explore its contents. Below are the details of the table:

```sql
MariaDB [bank_db]> SELECT * FROM employee;
+--------+---------+---------+------------+---------+--------+
| emp_id | fname   | lname   | desig      | dept    | salary |
+--------+---------+---------+------------+---------+--------+
|    101 | Raju    | Rastogi | Manager    | Loan    |  37000 |
|    102 | Sham    | Mohan   | Cashier    | Cash    |  32000 |
|    103 | baburao | Apte    | Associate  | Loan    |  25000 |
|    104 | Paul    | Phillip | Accountant | Account |  25000 |
|    105 | Alex    | Watt    | Associate  | Deposit |  35000 |
+--------+---------+---------+------------+---------+--------+
5 rows in set (0.001 sec)

MariaDB [bank_db]>
```

### **1\. DISTINCT: Identifying Unique Positions**

In our bank's employee database, there might be multiple employees with the same designation, such as "Associate" or "Manager." Using `DISTINCT`, we can retrieve a list of unique job titles without repetition.

**Example:** Suppose we have an `employee` table with a column `dept`, and we want to see all unique job titles:

```sql
MariaDB [bank_db]> SELECT DISTINCT dept FROM employee;
+---------+
| dept    |
+---------+
| Loan    |
| Cash    |
| Account |
| Deposit |
+---------+
4 rows in set (0.002 sec)

MariaDB [bank_db]>
```

This query will give us a list of distinct job titles across all bank employees.

### **2\. ORDER BY: Organizing Employee Data**

Imagine we want to view a list of employees sorted alphabetically by their last names. We can use `ORDER BY` to arrange the results accordingly.

**Example:** To sort the employees' data by their last names:

```sql
MariaDB [bank_db]> SELECT * FROM employee ORDER BY lname;
+--------+---------+---------+------------+---------+--------+
| emp_id | fname   | lname   | desig      | dept    | salary |
+--------+---------+---------+------------+---------+--------+
|    103 | baburao | Apte    | Associate  | Loan    |  25000 |
|    102 | Sham    | Mohan   | Cashier    | Cash    |  32000 |
|    104 | Paul    | Phillip | Accountant | Account |  25000 |
|    101 | Raju    | Rastogi | Manager    | Loan    |  37000 |
|    105 | Alex    | Watt    | Associate  | Deposit |  35000 |
+--------+---------+---------+------------+---------+--------+
5 rows in set (0.002 sec)

MariaDB [bank_db]>
```

This query will retrieve all employee records from the database, sorted alphabetically by their last names.

### **3\. LIKE Keyword: Searching for Specific Employees**

Suppose we want to find all employees whose job titles contain the word "Manager." We can use the `LIKE` keyword to perform a pattern search.

**Example:** To find all employees with designation containing "Manager":

```sql
MariaDB [bank_db]> SELECT * FROM employee WHERE desig LIKE "%Manager%";
+--------+-------+---------+---------+------+--------+
| emp_id | fname | lname   | desig   | dept | salary |
+--------+-------+---------+---------+------+--------+
|    101 | Raju  | Rastogi | Manager | Loan |  37000 |
+--------+-------+---------+---------+------+--------+
1 row in set (0.001 sec)

MariaDB [bank_db]>
```

This query will return all employees whose job titles include the word "Manager," such as "Senior Manager" or "Assistant Manager."

### **4\. LIMIT Keyword: Limiting the Number of Results**

In some cases, we may only want to retrieve a certain number of records, such as the top five highest-paid employees. We can use the `LIMIT` keyword for this purpose.

**Example:** To retrieve the top five highest-paid employees:

```plaintext
MariaDB [bank_db]> SELECT * FROM employee ORDER BY salary DESC LIMIT 5;
+--------+---------+---------+------------+---------+--------+
| emp_id | fname   | lname   | desig      | dept    | salary |
+--------+---------+---------+------------+---------+--------+
|    101 | Raju    | Rastogi | Manager    | Loan    |  37000 |
|    105 | Alex    | Watt    | Associate  | Deposit |  35000 |
|    102 | Sham    | Mohan   | Cashier    | Cash    |  32000 |
|    103 | baburao | Apte    | Associate  | Loan    |  25000 |
|    104 | Paul    | Phillip | Accountant | Account |  25000 |
+--------+---------+---------+------------+---------+--------+
5 rows in set (0.002 sec)

MariaDB [bank_db]>
```

This query will fetch the first five records from the employees table, sorted by salary in descending order.

### **5\. Function COUNT: Counting Employees by Job Title**

If we want to know how many employees working in the bank, we can use the `COUNT` function.

**Example:** To count the number of employees :

```sql
MariaDB [bank_db]> SELECT COUNT(emp_id) FROM employee;
+---------------+
| COUNT(emp_id) |
+---------------+
|             5 |
+---------------+
1 row in set (0.001 sec)

MariaDB [bank_db]>
```

This query will give us a count of employees present in the table.

### **6\. GROUP BY Function: Grouping Data**

The `GROUP BY` function in MySQL is used to group rows that have the same values into summary rows. It's commonly used with aggregate functions like `COUNT`, `SUM`, `AVG`, etc., to perform operations on groups of data.

Suppose we have a table named `employee` with columns `dept` and `salary`. We want to find the number of employee in each department.

Example: To calculate the total employee present on each department:

```sql
MariaDB [bank_db]> SELECT dept, COUNT(emp_id) FROM employee GROUP BY dept;
+---------+---------------+
| dept    | COUNT(emp_id) |
+---------+---------------+
| Account |             1 |
| Cash    |             1 |
| Deposit |             1 |
| Loan    |             2 |
+---------+---------------+
4 rows in set (0.005 sec)

MariaDB [bank_db]>
```

### **7\. MIN and MAX Functions: Finding Extremes**

Suppose we want to find the employee with the lowest and highest salaries in the bank. We can use the `MIN` and `MAX` functions for this purpose.

**Example:** To find the employee with the lowest and highest salaries:

```sql
MariaDB [bank_db]> SELECT MIN(salary) as min_salary, MAX(salary) as max_salary FROM employee;
+------------+------------+
| min_salary | max_salary |
+------------+------------+
|      25000 |      37000 |
+------------+------------+
1 row in set (0.002 sec)

MariaDB [bank_db]>
```

This query will return the minimum and maximum salaries among all employees.

### **8\. Subqueries: Nested Queries**

A subquery is a query nested within another query. Let's say we want to find employees who has the maximum salary with all his details

**Example:** To find employee having highest salary with all his details:

```sql
MariaDB [bank_db]> SELECT * FROM employee WHERE salary = (SELECT MAX(salary) FROM employee);
+--------+-------+---------+---------+------+--------+
| emp_id | fname | lname   | desig   | dept | salary |
+--------+-------+---------+---------+------+--------+
|    101 | Raju  | Rastogi | Manager | Loan |  37000 |
+--------+-------+---------+---------+------+--------+
1 row in set (0.002 sec)

MariaDB [bank_db]>
```

This query will return employee whose salary is highest.

### **8\. SUM and AVERAGE Functions: Calculating Totals and Averages**

If we want to calculate the total and average salary of all employees, we can use the `SUM` and `AVG` functions.

**Example:** To calculate the total and average salary of all employees:

```sql
MariaDB [bank_db]> SELECT SUM(salary) as total_salary, AVG(salary) as average_salary FROM employee;
+--------------+----------------+
| total_salary | average_salary |
+--------------+----------------+
|       154000 |     30800.0000 |
+--------------+----------------+
1 row in set (0.002 sec)

MariaDB [bank_db]>
```

This query will provide the total and average salary across all employees in the bank.

With these examples, understanding MySQL concepts in the context of a bank's employee database should be much clearer!

### **Conclusion**

In this beginner's guide, we've navigated through the essential concepts of MySQL databases using examples from a bank's employee database. Let's recap what we've learned:

* **DISTINCT**: We can use the `DISTINCT` keyword to retrieve unique values from a column, eliminating duplicates and gaining insights into distinct positions or categories within our dataset.
    
* **ORDER BY**: Sorting data with the `ORDER BY` clause enables us to organize results in ascending or descending order based on specified columns, providing clarity and ease of interpretation.
    
* **LIKE**: The `LIKE` keyword allows us to perform pattern searches within text columns, facilitating the identification of specific records based on partial or exact matches.
    
* **LIMIT**: By using the `LIMIT` keyword, we can restrict the number of results returned by a query, focusing on a subset of data and improving efficiency when dealing with large datasets.
    
* **COUNT**: The `COUNT` function enables us to tally the number of occurrences of a particular value or column, providing valuable insights into the size or distribution of our dataset.
    
* **GROUP BY**: With the `GROUP BY` function, we can group rows based on common values in one or more columns, facilitating the analysis of data subsets and the application of aggregate functions to each group.
    
* **MIN and MAX**: Utilizing the `MIN` and `MAX` functions allows us to identify the smallest and largest values within a dataset, aiding in the discovery of extremes and outliers.
    
* **Subqueries**: Nested queries, or subqueries, provide a powerful tool for performing complex analyses by embedding one query within another, enabling dynamic filtering and data retrieval based on intermediate results.
    
* **SUM and AVERAGE**: The `SUM` and `AVERAGE` functions enable us to calculate total and average values respectively, offering insights into cumulative totals and typical values within our dataset.
    

By applying these fundamental MySQL concepts to scenarios within a bank's employee database, we've gained practical knowledge that can be extended to a wide range of real-world applications. Whether you're a budding data analyst, software developer, or database administrator, mastering these concepts lays a solid foundation for navigating the world of relational databases with confidence.

So, armed with these newfound skills, go forth and explore the vast realm of MySQL databases with clarity and purpose!