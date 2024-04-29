---
title: "Understanding String Functions in MySQL with Example"
datePublished: Mon Apr 29 2024 12:55:59 GMT+0000 (Coordinated Universal Time)
cuid: clvkyqdii001h0al9162x19y9
slug: understanding-string-functions-in-mysql-with-example
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1714395260881/c9803d9c-5fe6-4b73-8f02-1c271a97569d.jpeg
tags: mysql, databases, devops, dbms, databasemanagement

---

MySQL is like a toolbox for storing and managing data, and one of its most useful tools is its string functions. These functions help us do cool stuff with text, like combining words, cutting out parts we don't need, and changing letters to uppercase or lowercase.

In this blog, we'll take a journey into the world of MySQL's string functions. We'll learn what they do and how they work by looking at examples that show us real-life situations where they come in handy. Whether you're new to databases or already know a bit, this guide will help you understand how to use MySQL's string functions to make your data work better for you.

So, let's get started and see how these string functions can help us get the most out of our data!

String functions in MySQL are powerful tools for manipulating and transforming character data stored in tables. In this comprehensive guide, we'll explore various string functions along with common SQL queries, using examples from the `employees` table.

```sql
MariaDB [bank_db]> SELECT * FROM employees;
+----+------------+-----------+-------------+
| id | first_name | last_name | city        |
+----+------------+-----------+-------------+
|  1 | John       | Doe       | New York    |
|  2 | Alice      | Smith     | Los Angeles |
|  3 | Michael    | Johnson   | Chicago     |
+----+------------+-----------+-------------+
3 rows in set (0.001 sec)

MariaDB [bank_db]>
```

**String Functions:**

**1\. CONCAT**

The `CONCAT` function concatenates two or more strings together. It's useful for combining string values or literals.

Example:

```sql
MariaDB [bank_db]> SELECT CONCAT(first_name, ' ', last_name) AS full_name FROM employees;
+-----------------+
| full_name       |
+-----------------+
| John Doe        |
| Alice Smith     |
| Michael Johnson |
+-----------------+
3 rows in set (0.001 sec)

MariaDB [bank_db]>
```

This query combines the `first_name` and `last_name` columns from the `employees` table, separated by a space, to create a full name.

**2\. CONCAT\_WS**

The `CONCAT_WS` function is similar to `CONCAT`, but it allows specifying a separator between strings. It's helpful for joining strings with a specified separator.

Example:

```sql
MariaDB [bank_db]> SELECT CONCAT_WS(', ', first_name, last_name) AS full_name FROM employees;
+------------------+
| full_name        |
+------------------+
| John, Doe        |
| Alice, Smith     |
| Michael, Johnson |
+------------------+
3 rows in set (0.002 sec)

MariaDB [bank_db]>
```

This query combines the `first_name` and `last_name` columns from the `employees` table, separated by a comma and space.

**3\. SUBSTRING**

The `SUBSTRING` function extracts a substring from a given string. It's helpful for retrieving a portion of a string based on specified starting position and length.

Example:

```sql
MariaDB [bank_db]> SELECT SUBSTRING(city, 1, 3) AS city_code FROM employees;
+-----------+
| city_code |
+-----------+
| New       |
| Los       |
| Chi       |
+-----------+
3 rows in set (0.003 sec)

MariaDB [bank_db]>
```

This query extracts the first three characters from the `city` column in the `employees` table, representing city codes.

**4\. REPLACE**

The `REPLACE` function replaces occurrences of a substring within a string. It's commonly used for search-and-replace operations.

Example:

```sql
MariaDB [bank_db]> SELECT REPLACE(city, 'New', 'San') AS updated_city FROM employees;
+--------------+
| updated_city |
+--------------+
| San York     |
| Los Angeles  |
| Chicago      |
+--------------+
3 rows in set (0.001 sec)

MariaDB [bank_db]>
```

This query replaces occurrences of 'New' with 'San' in the `city` column of the `employees` table, resulting in updated city names.

**5\. UPPER and LOWER**

The `UPPER` and `LOWER` functions convert strings to uppercase and lowercase, respectively. They're useful for standardizing case in string data.

Example:

```sql
MariaDB [bank_db]> SELECT UPPER(first_name) AS upper_first_name, LOWER(last_name) AS lower_last_name FROM employees;
+------------------+-----------------+
| upper_first_name | lower_last_name |
+------------------+-----------------+
| JOHN             | doe             |
| ALICE            | smith           |
| MICHAEL          | johnson         |
+------------------+-----------------+
3 rows in set (0.001 sec)

MariaDB [bank_db]>
```

This query converts the `first_name` column to uppercase and the `last_name` column to lowercase in the `employees` table.

**6\. CHAR\_LENGTH**

The `CHAR_LENGTH` function returns the number of characters in a string. It's used to determine the length of string data.

Example:

```sql
MariaDB [bank_db]> SELECT CHAR_LENGTH(first_name) AS name_length FROM employees;
+-------------+
| name_length |
+-------------+
|           4 |
|           5 |
|           7 |
+-------------+
3 rows in set (0.001 sec)

MariaDB [bank_db]>
```

This query calculates the length of each `first_name` in the `employees` table, providing the number of characters in each name.

**7\. LEFT and RIGHT**

The `LEFT` and `RIGHT` functions extract a specified number of characters from the left or right of a string, respectively. They're helpful for retrieving substrings.

Example:

```sql
MariaDB [bank_db]> SELECT LEFT(city, 3) AS left_city, RIGHT(city, 3) AS right_city FROM employees;
+-----------+------------+
| left_city | right_city |
+-----------+------------+
| New       | ork        |
| Los       | les        |
| Chi       | ago        |
+-----------+------------+
3 rows in set (0.001 sec)

MariaDB [bank_db]>
```

This query extracts the first three characters and last three characters from the `city` column in the `employees` table.

**8\. TRIM**

The `TRIM` function removes leading and trailing spaces from a string. It's used to clean up whitespace in string data.

Example:

```sql
MariaDB [bank_db]> SELECT TRIM(city) AS trimmed_city FROM employees;
+--------------+
| trimmed_city |
+--------------+
| New York     |
| Los Angeles  |
| Chicago      |
+--------------+
3 rows in set (0.001 sec)

MariaDB [bank_db]>
```

This query removes leading and trailing spaces from the `city` column in the `employees` table, resulting in trimmed city names.

**Conclusion**

In this blog, we've explored a variety of string functions available in MySQL and learned how to use them effectively with practical examples. These string functions provide powerful tools for manipulating and transforming character data stored in MySQL tables, allowing us to perform a wide range of operations such as concatenation, substring extraction, replacement, case conversion, and more.

By mastering these string functions, MySQL users can efficiently manage and analyze their data, ensuring its accuracy and consistency. Whether it's cleaning up whitespace, standardizing case, extracting substrings, or performing complex transformations, understanding these functions is essential for database professionals and developers working with MySQL databases.