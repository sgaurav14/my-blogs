---
title: "Title: A Beginner's Guide to SQL and MySQL"
datePublished: Tue Apr 09 2024 15:09:05 GMT+0000 (Coordinated Universal Time)
cuid: clusioi7l000508lcg4392h1g
slug: title-a-beginners-guide-to-sql-and-mysql
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1712675085569/a282ee58-5bcf-46af-af54-f24a00f8a813.webp
tags: data, mysql, databases, sql

---

**Introduction:** Welcome to the world of databases! Whether you're a budding software developer or just curious about how data is managed in modern applications, understanding SQL (Structured Query Language) and MySQL can open up a world of possibilities. In this beginner's guide, we'll cover everything you need to know to get started with SQL and MySQL, from basic concepts to practical examples.

### **Understanding SQL and MySQL**

* **SQL (Structured Query Language):** SQL is a standard programming language used for managing and manipulating relational databases. It provides a set of commands and syntax for performing various operations such as querying data, inserting new records, updating existing data, and deleting records.
    
* **MySQL:** MySQL is an open-source relational database management system (RDBMS) that uses SQL as its language for interacting with databases. It is widely used in web development, enterprise applications, and various other domains due to its performance, scalability, and ease of use.
    

### **Basic Operations in MySQL:**

* **Listing Databases:** To view a list of all databases on a MySQL server, you can use the `SHOW DATABASES;` command.
    
    ```sql
    MariaDB [(none)]> show databases;
    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | mysql              |
    | performance_schema |
    +--------------------+
    3 rows in set (0.002 sec)
    
    MariaDB [(none)]>
    ```
    
* **Creating a New Database:** To create a new database, use the `CREATE DATABASE <database_name>;` command.
    
    ```sql
    MariaDB [(none)]> create database student_info;
    Query OK, 1 row affected (0.001 sec)
    
    MariaDB [(none)]> show databases;
    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | mysql              |
    | performance_schema |
    | student_info       |
    +--------------------+
    4 rows in set (0.001 sec)
    
    MariaDB [(none)]>
    ```
    
* **Using a Database:** To switch to a specific database, use the `USE <database_name>;` command.
    
    ```sql
    MariaDB [(none)]> show databases;
    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | mysql              |
    | performance_schema |
    | student_info       |
    +--------------------+
    4 rows in set (0.001 sec)
    
    MariaDB [(none)]>
    MariaDB [(none)]> use student_info;
    Database changed
    MariaDB [student_info]>
    MariaDB [student_info]>
    ```
    
* **Checking Current Database:** You can verify the currently selected database using the `SELECT DATABASE();` command.
    
    ```sql
    MariaDB [student_info]> select database();
    +--------------+
    | database()   |
    +--------------+
    | student_info |
    +--------------+
    1 row in set (0.000 sec)
    
    MariaDB [student_info]>
    MariaDB [student_info]>
    ```
    
* **Deleting a Database:** To delete a database, use the `DROP DATABASE <database_name>;` command. Be careful, as this will permanently delete the database and its contents.
    
    ```sql
    MariaDB [(none)]> show databases;
    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | mysql              |
    | performance_schema |
    | student_info       |
    +--------------------+
    4 rows in set (0.001 sec)
    
    MariaDB [(none)]> drop database student_info;
    Query OK, 0 rows affected (0.003 sec)
    
    MariaDB [(none)]> show databases;
    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | mysql              |
    | performance_schema |
    +--------------------+
    3 rows in set (0.001 sec)
    
    MariaDB [(none)]>
    ```
    

### **Working with Tables:**

**What is a Table?**

A table is a fundamental structure in a relational database that stores data in rows and columns.

* **Creating a Table:** To create a new table, use the `CREATE TABLE <table_name> (...)` command, specifying the column names, data types, and any constraints.
    
    ```sql
    MariaDB [(none)]> show databases;
    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | mysql              |
    | performance_schema |
    | students_info      |
    +--------------------+
    4 rows in set (0.002 sec)
    
    MariaDB [(none)]>
    MariaDB [(none)]> use students_info;
    Database changed
    MariaDB [students_info]>
    MariaDB [students_info]> CREATE TABLE students (
        -> id INT,
        -> name VARCHAR (50)
        -> );
    Query OK, 0 rows affected (0.093 sec)
    
    MariaDB [students_info]>
    ```
    
* **Checking a Table:** To view the structure of a table, you can use the `DESCRIBE <table_name>;` or `DESC <table_name>;` command.
    
    ```sql
    MariaDB [students_info]> DESC students;
    +-------+-------------+------+-----+---------+-------+
    | Field | Type        | Null | Key | Default | Extra |
    +-------+-------------+------+-----+---------+-------+
    | id    | int(11)     | YES  |     | NULL    |       |
    | name  | varchar(50) | YES  |     | NULL    |       |
    +-------+-------------+------+-----+---------+-------+
    2 rows in set (0.003 sec)
    
    MariaDB [students_info]>
    ```
    
* **Listing the Table:** To view the list of tables in the current database, you can use the `SHOW TABLES;` command.
    
    ```sql
    MariaDB [(none)]> SHOW DATABASES;
    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | mysql              |
    | performance_schema |
    | students_info      |
    +--------------------+
    4 rows in set (0.002 sec)
    
    MariaDB [(none)]>
    MariaDB [(none)]> USE students_info;
    Reading table information for completion of table and column names
    You can turn off this feature to get a quicker startup with -A
    
    Database changed
    MariaDB [students_info]>
    MariaDB [students_info]> SHOW TABLES;
    +-------------------------+
    | Tables_in_students_info |
    +-------------------------+
    | students                |
    +-------------------------+
    1 row in set (0.001 sec)
    
    MariaDB [students_info]>
    ```
    
* **Inserting Data into a Table:** To add new records to a table, use the `INSERT INTO <table_name> (...) VALUES (...);` command.
    
    ```sql
    MariaDB [students_info]> INSERT INTO students (id, name)
        -> VALUES (101, 'Siddhartha');
    Query OK, 1 row affected (0.018 sec)
    
    MariaDB [students_info]>
    ```
    
* **Inserting Multiple Data at a Time:** You can insert multiple rows of data into a table using a single `INSERT INTO` statement with multiple value sets.
    
    ```sql
    MariaDB [students_info]>
    MariaDB [students_info]> INSERT INTO students
        -> VALUES (102, 'Gaurav') , (103, 'Paul');
    Query OK, 2 rows affected (0.022 sec)
    Records: 2  Duplicates: 0  Warnings: 0
    
    MariaDB [students_info]>
    ```
    

### **Reading Data from a Table:**

* **Reading All Data:** To retrieve all records from a table, use the `SELECT * FROM <table_name>;` command.
    
    ```sql
    MariaDB [students_info]> SELECT * FROM students;
    +------+------------+
    | id   | name       |
    +------+------------+
    |  101 | Siddhartha |
    |  102 | Gaurav     |
    |  103 | Paul       |
    +------+------------+
    3 rows in set (0.001 sec)
    
    MariaDB [students_info]>
    ```
    
* **Reading Selected Columns:** You can specify which columns to retrieve using the `SELECT <column1>, <column2>, ... FROM <table_name>;` command.
    
    ```sql
    MariaDB [students_info]> SELECT name FROM students;
    +------------+
    | name       |
    +------------+
    | Siddhartha |
    | Gaurav     |
    | Paul       |
    +------------+
    3 rows in set (0.001 sec)
    
    MariaDB [students_info]>
    MariaDB [students_info]> SELECT id,name FROM students;
    +------+------------+
    | id   | name       |
    +------+------------+
    |  101 | Siddhartha |
    |  102 | Gaurav     |
    |  103 | Paul       |
    +------+------------+
    3 rows in set (0.001 sec)
    
    MariaDB [students_info]>
    ```
    
* **Printing Data Based on Conditions:** Use the `SELECT * FROM <table_name> WHERE <condition>;` command to filter records based on specific conditions.
    
    ```sql
    MariaDB [students_info]> SELECT * FROM students WHERE id=103;
    +------+------+
    | id   | name |
    +------+------+
    |  103 | Paul |
    +------+------+
    1 row in set (0.001 sec)
    
    MariaDB [students_info]>
    ```
    

### **Modifying Data in a Table:**

* **Updating Data:** To modify existing data in a table, use the `UPDATE <table_name> SET <column_name> = <new_value> WHERE <condition>;` command.
    
    ```sql
    MariaDB [students_info]> UPDATE students SET name='John' WHERE id=103;
    Query OK, 1 row affected (0.021 sec)
    Rows matched: 1  Changed: 1  Warnings: 0
    
    MariaDB [students_info]> SELECT * FROM students;
    +------+------------+
    | id   | name       |
    +------+------------+
    |  101 | Siddhartha |
    |  102 | Gaurav     |
    |  103 | John       |
    +------+------------+
    3 rows in set (0.001 sec)
    
    MariaDB [students_info]>
    ```
    
* **Deleting Data:** To remove records from a table, use the `DELETE FROM <table_name> WHERE <condition>;` command.
    
    ```sql
    MariaDB [students_info]> DELETE FROM students WHERE id=103;
    Query OK, 1 row affected (0.022 sec)
    
    MariaDB [students_info]>
    MariaDB [students_info]>
    MariaDB [students_info]> SELECT * FROM students;
    +------+------------+
    | id   | name       |
    +------+------------+
    |  101 | Siddhartha |
    |  102 | Gaurav     |
    +------+------------+
    2 rows in set (0.001 sec)
    
    MariaDB [students_info]>
    ```
    

### **Understanding Table Constraints:**

### NULL

* In databases, NULL represents the absence of a value. It indicates that the value for a particular data item is unknown, missing, or undefined. NULL is distinct from an empty string, zero, or any other value—it signifies the absence of any value.
    
    **Key Points about NULL:**
    
    1. **No Value:** NULL indicates that no value has been provided for a particular column in a record.
        
    2. **Data Integrity:** NULL can affect data integrity if not handled properly. Columns that allow NULL values may not enforce data constraints, leading to inconsistencies or unexpected behavior.
        
    3. **Handling NULL:** When querying data from a database, it's essential to handle NULL values appropriately to avoid errors or incorrect results. SQL provides functions and operators for working with NULL values, such as `IS NULL`, `IS NOT NULL`, and the `COALESCE` function.
        
    4. **Column Definitions:** When creating tables in MySQL, you can specify whether a column allows NULL values or not. By default, most columns allow NULL values unless explicitly defined otherwise.
        
    5. **Implications for Querying Data:** When querying data from a table, NULL values may need to be considered in WHERE clauses or other conditions. Failure to account for NULL values can result in incomplete or inaccurate query results.
        
    6. **Use Cases:** NULL values are commonly used in scenarios where the absence of a value is meaningful. For example, a database might use NULL to represent an optional field that hasn't been filled out by a user.
        
    
    ```sql
    MariaDB [students_info]> DESC students;
    +---------+-------------+------+-----+---------+-------+
    | Field   | Type        | Null | Key | Default | Extra |
    +---------+-------------+------+-----+---------+-------+
    | id      | int(11)     | YES  |     | NULL    |       |
    | name    | varchar(50) | YES  |     | NULL    |       |
    | address | varchar(50) | YES  |     | NULL    |       |
    +---------+-------------+------+-----+---------+-------+
    3 rows in set (0.003 sec)
    
    MariaDB [students_info]>
    ```
    
    ### NOT NULL Constraint
    
* The `NOT NULL` constraint in MySQL ensures that a column cannot contain null values. Null represents the absence of a value, and it can cause issues in database operations if not handled properly. By applying the `NOT NULL` constraint to a column, you enforce the rule that every entry in that column must contain a value, thereby preventing null values from being inserted.
    
    For example, consider a 'students' table `email` column should always contain a valid email address. You can enforce this requirement by defining the `email` column with the `NOT NULL` constraint:
    
    ```sql
    MariaDB [students_info]> CREATE TABLE students
        -> (id INT, name VARCHAR(50), email VARCHAR(50) NOT NULL);
    Query OK, 0 rows affected (0.089 sec)
    
    MariaDB [students_info]>
    MariaDB [students_info]> DESC students;
    +-------+-------------+------+-----+---------+-------+
    | Field | Type        | Null | Key | Default | Extra |
    +-------+-------------+------+-----+---------+-------+
    | id    | int(11)     | YES  |     | NULL    |       |
    | name  | varchar(50) | YES  |     | NULL    |       |
    | email | varchar(50) | NO   |     | NULL    |       |
    +-------+-------------+------+-----+---------+-------+
    3 rows in set (0.004 sec)
    
    MariaDB [students_info]>
    ```
    

We can see in the above example that 'email' field Null value is set to NO. It means we can't leave the field empty.

```sql
MariaDB [students_info]> INSERT INTO students (id, name)
    -> VALUES (101, 'Siddhartha');
ERROR 1364 (HY000): Field 'email' doesn't have a default value
MariaDB [students_info]>
MariaDB [students_info]>
MariaDB [students_info]> INSERT INTO students
    -> VALUES (101, 'Siddhartha', 'abc@xyz.com');
Query OK, 1 row affected (0.019 sec)

MariaDB [students_info]> SELECT * FROM students;
+------+------------+-------------+
| id   | name       | email       |
+------+------------+-------------+
|  101 | Siddhartha | abc@xyz.com |
+------+------------+-------------+
1 row in set (0.001 sec)

MariaDB [students_info]>
```

### Default Values

In MySQL, you can specify default values for columns using the `DEFAULT` keyword. Default values are used when no explicit value is provided for a column during insertion. This feature is useful for columns where a default value is appropriate or necessary.

* For example, let's suppose if you want to set the default class as X for the 'students' table:
    
    ```sql
    MariaDB [students_info]> CREATE TABLE students
        -> (id INT NOT NULL, name VARCHAR(50) NOT NULL, email VARCHAR(50), class INT NOT NULL DEFAULT 10);
    Query OK, 0 rows affected (0.098 sec)
    
    MariaDB [students_info]>
    MariaDB [students_info]> DESC students;
    +-------+-------------+------+-----+---------+-------+
    | Field | Type        | Null | Key | Default | Extra |
    +-------+-------------+------+-----+---------+-------+
    | id    | int(11)     | NO   |     | NULL    |       |
    | name  | varchar(50) | NO   |     | NULL    |       |
    | email | varchar(50) | YES  |     | NULL    |       |
    | class | int(11)     | NO   |     | 10      |       |
    +-------+-------------+------+-----+---------+-------+
    4 rows in set (0.004 sec)
    
    MariaDB [students_info]>
    ```
    

Let's do some insertion of data and check if it is taking the default value or not.

```sql
MariaDB [students_info]> INSERT INTO students (id, name, email) VALUES (101, 'Siddhartha', 'xyz@abc.com'), (102, 'Gaurav', 'abc@ab.com');
Query OK, 2 rows affected (0.024 sec)
Records: 2  Duplicates: 0  Warnings: 0

MariaDB [students_info]> SELECT * FROM students;
+-----+------------+-------------+-------+
| id  | name       | email       | class |
+-----+------------+-------------+-------+
| 101 | Siddhartha | xyz@abc.com |    10 |
| 102 | Gaurav     | abc@ab.com  |    10 |
+-----+------------+-------------+-------+
2 rows in set (0.002 sec)

MariaDB [students_info]>
```

### Primary Key

A primary key in MySQL is a unique identifier for each record in a table. It serves as a unique identifier and ensures data integrity by enforcing uniqueness. Additionally, a primary key facilitates efficient data retrieval, as it provides a fast and reliable way to locate specific records.

* A table can have only one primary key.  
    For example, in the `students`table, the `id` column is often used as the primary key:
    
    ```sql
    MariaDB [students_info]> CREATE TABLE students (id INT PRIMARY KEY, name VARCHAR(50) NOT NULL, class INT NOT NULL DEFAULT 10);
    Query OK, 0 rows affected (0.087 sec)
    
    MariaDB [students_info]> SHOW TABLES;
    +-------------------------+
    | Tables_in_students_info |
    +-------------------------+
    | students                |
    +-------------------------+
    1 row in set (0.001 sec)
    
    MariaDB [students_info]>
    MariaDB [students_info]> DESC students;
    +-------+-------------+------+-----+---------+-------+
    | Field | Type        | Null | Key | Default | Extra |
    +-------+-------------+------+-----+---------+-------+
    | id    | int(11)     | NO   | PRI | NULL    |       |
    | name  | varchar(50) | NO   |     | NULL    |       |
    | class | int(11)     | NO   |     | 10      |       |
    +-------+-------------+------+-----+---------+-------+
    3 rows in set (0.005 sec)
    
    MariaDB [students_info]>
    ```
    

Let's so some insertion in the table -

```sql
MariaDB [students_info]> INSERT INTO students (id, name) VALUES (101, 'Sid'), (102, 'Gaurav'), (103, 'Tom');
Query OK, 3 rows affected (0.036 sec)
Records: 3  Duplicates: 0  Warnings: 0

MariaDB [students_info]> SELECT * FROM students;
+-----+--------+-------+
| id  | name   | class |
+-----+--------+-------+
| 101 | Sid    |    10 |
| 102 | Gaurav |    10 |
| 103 | Tom    |    10 |
+-----+--------+-------+
3 rows in set (0.002 sec)

MariaDB [students_info]>
```

The NULL values and duplicate entries are not allow for the primary key column.

### **Auto Increment:**

The `AUTO_INCREMENT` attribute in MySQL automatically generates unique values for a column, typically used for primary keys. It simplifies the process of assigning unique identifiers to records by automatically incrementing the column value for each new record inserted into the table.  
For example, in the `students`table, the `id` column is often defined with the `AUTO_INCREMENT` attribute:

* ```sql
      MariaDB [students_info]> CREATE TABLE students (id INT PRIMARY KEY AUTO_INCREMENT, name VARCHAR(50) NOT NULL, class INT NOT NULL DEFAULT 10);
      Query OK, 0 rows affected (0.076 sec)
      
      MariaDB [students_info]> DESC students;
      +-------+-------------+------+-----+---------+----------------+
      | Field | Type        | Null | Key | Default | Extra          |
      +-------+-------------+------+-----+---------+----------------+
      | id    | int(11)     | NO   | PRI | NULL    | auto_increment |
      | name  | varchar(50) | NO   |     | NULL    |                |
      | class | int(11)     | NO   |     | 10      |                |
      +-------+-------------+------+-----+---------+----------------+
      3 rows in set (0.004 sec)
      
      MariaDB [students_info]>
    ```
    

Let's do some insertion and check -

```sql
MariaDB [students_info]> INSERT INTO students (id, name)
    -> VALUES (101, 'sid');
Query OK, 1 row affected (0.026 sec)
MariaDB [students_info]> SELECT * FROM students;
+-----+------+-------+
| id  | name | class |
+-----+------+-------+
| 101 | sid  |    10 |
+-----+------+-------+
1 row in set (0.001 sec)

MariaDB [students_info]>
MariaDB [students_info]> INSERT INTO students (name)
    -> VALUES ('ram'), ('shyam'), ('baburao');
Query OK, 3 rows affected (0.019 sec)
Records: 3  Duplicates: 0  Warnings: 0

MariaDB [students_info]> SELECT * FROM students;
+-----+---------+-------+
| id  | name    | class |
+-----+---------+-------+
| 101 | sid     |    10 |
| 102 | ram     |    10 |
| 103 | shyam   |    10 |
| 104 | baburao |    10 |
+-----+---------+-------+
4 rows in set (0.001 sec)

MariaDB [students_info]>
```

We can see that the auto increment for the id is working fine.

### Alias

In SQL, an alias is a temporary name assigned to a table or a column in a query result. Aliases are often used to make column names more readable or to simplify complex queries.

* **Table Aliases:** Short names assigned to tables in a query to simplify referencing multiple tables.
    
    * Syntax: `FROM table_name AS alias_name`
        
* **Column Aliases:** Renames a column in the query result for clarity or calculation readability.
    
    * Syntax: `SELECT column_name AS alias_name`
        
        ```sql
        MariaDB [students_info]> SELECT * FROM students;
        +-----+---------+-------+
        | id  | name    | class |
        +-----+---------+-------+
        | 101 | sid     |    10 |
        | 102 | ram     |    10 |
        | 103 | shyam   |    10 |
        | 104 | baburao |    10 |
        +-----+---------+-------+
        4 rows in set (0.001 sec)
        
        MariaDB [students_info]> SELECT name AS 'Students Name' FROM students;
        +---------------+
        | Students Name |
        +---------------+
        | sid           |
        | ram           |
        | shyam         |
        | baburao       |
        +---------------+
        4 rows in set (0.002 sec)
        ```
        
* **Derived Table Aliases:** Assigns a name to a subquery in the `FROM` clause.
    
    * Syntax: `(SELECT column_name FROM table_name) AS alias_name`
        

Aliases make SQL queries more concise and readable, especially in complex queries involving multiple tables or calculations.

### **Conclusion**

Understanding SQL and MySQL is essential for anyone working with databases, whether you're a beginner or an experienced developer. In this guide, we've covered the fundamental concepts of SQL and MySQL, including basic operations, table manipulation, and key features like constraints and aliases.

By mastering these concepts, you'll be well-equipped to create, query, and manage databases effectively. Remember to practice writing SQL queries and experimenting with MySQL to reinforce your understanding. As you gain more experience, you'll discover new ways to leverage SQL and MySQL to solve complex problems and build powerful applications.

Whether you're building a small personal project or working on enterprise-level applications, SQL and MySQL provide the tools you need to store, retrieve, and manipulate data efficiently. Keep exploring, learning, and honing your skills, and you'll unlock the full potential of SQL and MySQL in your projects.

Happy coding!