# SPRING BOOT – MODULE 1  
# PHASE 1  
## JDBC – Introduction and Core Concepts  

---

## 1. Introduction to JDBC  

### 1.1 What is JDBC?  

JDBC (Java Database Connectivity) is a standard Java API used to connect Java applications with relational databases. It provides a set of interfaces and classes under the `java.sql` package that allow execution of SQL queries, retrieval of data, and management of database transactions.

JDBC enables database-independent programming. This means the structure of Java code remains almost the same even if the database changes (e.g., MySQL, PostgreSQL, Oracle). Only the driver and connection URL need modification.

---

### 1.2 Why JDBC is Required?  

Modern applications require permanent storage of data. For example:

- Student Management Systems  
- Banking Applications  
- E-commerce Platforms  
- Hospital Management Systems  

Without JDBC:

- Java applications cannot communicate with relational databases.  
- Data cannot be stored permanently.  
- CRUD operations (Create, Read, Update, Delete) cannot be performed.  

JDBC acts as a bridge between Java applications and database systems.

---

## 2. JDBC Architecture  

The JDBC architecture consists of the following layers:

Java Application  
↓  
JDBC API (`java.sql`)  
↓  
DriverManager  
↓  
JDBC Driver  
↓  
Database Server  

---

### 2.1 Components Explanation  

#### JDBC API  
Provides interfaces such as:

- `Connection`  
- `Statement`  
- `PreparedStatement`  
- `ResultSet`  

These interfaces define how Java applications interact with databases.

#### DriverManager  
The DriverManager class manages database drivers and establishes connections using the appropriate driver.

#### JDBC Driver  
The driver converts JDBC calls into database-specific protocol commands.

#### Database  
The database stores structured data in tables and executes SQL commands.

---

## 3. JDBC Driver Types  

There are four types of JDBC drivers:

### Type 1 – JDBC-ODBC Bridge Driver  
- Converts JDBC calls into ODBC calls.  
- Requires ODBC installation.  
- Not recommended for modern applications.

### Type 2 – Native API Driver  
- Uses database-specific native libraries.  
- Platform dependent.  

### Type 3 – Network Protocol Driver  
- Converts JDBC calls into database-independent protocol.  
- Requires middleware server.  

### Type 4 – Thin Driver  
- Pure Java driver.  
- Direct communication with database.  
- Most widely used in modern applications.

---

## 4. Steps to Connect Java with Database  

The basic workflow of JDBC is:

1. Establish Connection  
2. Create Statement or PreparedStatement  
3. Execute SQL Query  
4. Process ResultSet  
5. Close Resources  

Proper resource management is essential to avoid memory leaks and performance issues.

---

## 5. Core JDBC Interfaces  

### 5.1 Connection  

The `Connection` interface represents a session between the Java application and the database.

Important methods:

- `setAutoCommit(boolean status)`  
- `commit()`  
- `rollback()`  
- `close()`  

---

### 5.2 Statement  

The `Statement` interface is used to execute static SQL queries.

Example:

```java
Statement stmt = conn.createStatement();
ResultSet rs = stmt.executeQuery("SELECT * FROM students");
```

Limitations:

- Vulnerable to SQL Injection  
- Query compiled every time it executes  

---

### 5.3 PreparedStatement  

The `PreparedStatement` interface is used for parameterized queries.

Advantages:

- Prevents SQL Injection  
- Better performance (precompiled query)  
- Safely handles dynamic input  

Example:

```java
PreparedStatement ps =
conn.prepareStatement("INSERT INTO students(name, roll) VALUES (?, ?)");
ps.setString(1, "Amit");
ps.setInt(2, 101);
ps.executeUpdate();
```

---

### 5.4 ResultSet  

The `ResultSet` interface stores data returned by a SELECT query.

Key points:

- Cursor initially points before first row.  
- `next()` moves to the next row.  
- Use getter methods like `getInt()`, `getString()` to retrieve data.

Example:

```java
while(rs.next()) {
    System.out.println(rs.getString("name"));
}
```

---

## 6. SQL Injection  

SQL Injection is a security vulnerability where malicious SQL code is inserted through user input.

Unsafe example:

```java
String query = "SELECT * FROM users WHERE name = '" + input + "'";
```

If input is:

```
' OR '1'='1
```

The condition becomes true for all records, potentially exposing sensitive data.

PreparedStatement prevents SQL Injection by separating SQL logic from user input.

---

## 7. Complete Working Example  

```java
import java.sql.*;

public class JdbcExample {

    public static void main(String[] args) {

        String url = "jdbc:mysql://localhost:3306/springdb";
        String user = "root";
        String pass = "rootpassword";

        try (Connection conn = DriverManager.getConnection(url, user, pass);
             Statement stmt = conn.createStatement()) {

            String create = "CREATE TABLE IF NOT EXISTS students(" +
                    "id INT AUTO_INCREMENT PRIMARY KEY," +
                    "name VARCHAR(100)," +
                    "roll INT)";

            stmt.execute(create);

            PreparedStatement ps =
            conn.prepareStatement("INSERT INTO students(name, roll) VALUES (?, ?)");

            ps.setString(1, "Neha");
            ps.setInt(2, 102);
            ps.executeUpdate();

            ResultSet rs =
            stmt.executeQuery("SELECT * FROM students");

            while(rs.next()) {
                System.out.println(
                        rs.getInt("id") + " " +
                        rs.getString("name") + " " +
                        rs.getInt("roll"));
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

---

## 8. Viva Questions (Detailed Answers)

### 1. What is JDBC and how does it work?  
JDBC is a Java API that allows Java applications to interact with relational databases. It works by using database drivers that translate Java method calls into database-specific commands.

### 2. Explain JDBC architecture.  
The Java application communicates with the JDBC API. DriverManager selects the correct driver. The driver communicates with the database server and sends results back to the application.

### 3. Why is PreparedStatement preferred over Statement?  
PreparedStatement prevents SQL injection, improves performance due to precompilation, and safely handles dynamic input parameters.

### 4. What is auto-commit mode?  
Auto-commit mode automatically commits each SQL statement immediately after execution. It is enabled by default in JDBC.

### 5. What happens if a Connection is not closed?  
Not closing a connection may cause memory leaks, reduced performance, and database connection exhaustion.

---

## 9. MCQs (With Answers)

### 1. Which package contains JDBC classes?  
A) java.util  
B) java.sql  
C) java.io  
D) javax.servlet  
Answer: B  

### 2. Which method retrieves records?  
A) executeUpdate()  
B) executeQuery()  
C) insertQuery()  
D) run()  
Answer: B  

### 3. Which driver type is most commonly used?  
A) Type 1  
B) Type 2  
C) Type 3  
D) Type 4  
Answer: D  

### 4. Which object stores retrieved data?  
A) Statement  
B) Connection  
C) ResultSet  
D) Driver  
Answer: C  

---

## 10. Interview Questions (Detailed Answers)

### 1. Explain the lifecycle of a JDBC connection.  
The lifecycle includes establishing a connection, creating a statement, executing SQL queries, processing the results, and closing resources properly to avoid memory leaks.

### 2. What are JDBC driver types and which one is preferred?  
There are four types of JDBC drivers. Type 4 (Thin Driver) is preferred because it is pure Java and platform independent.

### 3. How does PreparedStatement improve security?  
PreparedStatement separates SQL structure from user data, ensuring that input values are treated strictly as data, preventing SQL injection attacks.

### 4. Difference between executeQuery() and executeUpdate()?  
executeQuery() is used for SELECT statements and returns a ResultSet. executeUpdate() is used for INSERT, UPDATE, and DELETE statements and returns the number of rows affected.

---

## 11. Lab Assignment

1. Create database `labdb`.  
2. Create table `employee(id, name, salary)`.  
3. Insert 5 records.  
4. Retrieve and display all records.  
5. Update one record.  
6. Submit source code in the next class.
