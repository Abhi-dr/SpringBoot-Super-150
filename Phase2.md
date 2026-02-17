# SPRING BOOT – MODULE 1  
# PHASE 2  
## JDBC – Transactions, Batch Processing and Advanced Result Handling  

---

## 1. Introduction  

In the previous phase, we learned how to connect Java with a database and perform basic CRUD operations.  

In this phase, we will move towards more advanced and practical database concepts that are essential in real-world enterprise applications.

---

## 2. What We Are Learning  

- Transaction Management in JDBC  
- Auto-commit behavior  
- Commit and Rollback operations  
- ACID properties  
- Batch Processing  
- CallableStatement (Stored Procedures)  
- ResultSet Types and Concurrency Modes  

---

## 3. Transaction Management  

### 3.1 What is a Transaction?  

A transaction is a group of SQL operations executed as a single logical unit of work.  

If all operations succeed → changes are committed.  
If any operation fails → changes are rolled back.  

Transactions ensure data integrity and consistency.

---

### 3.2 Why Transactions Are Important  

Consider a banking example:

- ₹1000 deducted from Account A  
- ₹1000 added to Account B  

If deduction happens but addition fails, the system becomes inconsistent.  

Transactions ensure that both operations succeed together or fail together.

---

### 3.3 Auto-Commit Mode  

By default, JDBC runs in auto-commit mode.

This means:
Every SQL statement is automatically committed after execution.

To manage transactions manually:

```java
conn.setAutoCommit(false);
```

---

### 3.4 Commit and Rollback  

- `commit()` permanently saves changes.  
- `rollback()` cancels changes made in the current transaction.

---

### 3.5 Transaction Example  

```java
import java.sql.*;

public class TransactionExample {

    public static void main(String[] args) {

        String url = "jdbc:mysql://localhost:3306/springdb";
        String user = "root";
        String pass = "rootpassword";

        try (Connection conn = DriverManager.getConnection(url, user, pass)) {

            conn.setAutoCommit(false);

            Statement stmt = conn.createStatement();

            stmt.executeUpdate(
                "UPDATE accounts SET balance = balance - 1000 WHERE id = 1");

            stmt.executeUpdate(
                "UPDATE accounts SET balance = balance + 1000 WHERE id = 2");

            conn.commit();
            System.out.println("Transaction Successful");

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

If an exception occurs before commit:

```java
conn.rollback();
```

---

## 4. ACID Properties  

Transactions follow ACID properties:

### Atomicity  
All operations complete successfully or none are applied.

### Consistency  
Database remains in a valid state before and after transaction.

### Isolation  
Transactions do not interfere with each other.

### Durability  
Committed data remains permanently stored even after system failure.

---

## 5. Batch Processing  

### 5.1 What is Batch Processing?  

Batch processing allows multiple SQL statements to be grouped and executed together.

It reduces database round-trips and improves performance.

---

### 5.2 When to Use Batch Processing  

- Inserting large datasets  
- Bulk updates  
- Data migration  

---

### 5.3 Batch Example  

```java
String insert = "INSERT INTO employee(name, salary) VALUES (?, ?)";
PreparedStatement ps = conn.prepareStatement(insert);

for(int i = 1; i <= 5; i++) {
    ps.setString(1, "Employee" + i);
    ps.setInt(2, 30000 + (i * 1000));
    ps.addBatch();
}

ps.executeBatch();
System.out.println("Batch Insert Completed");
```

---

## 6. CallableStatement  

### 6.1 What is CallableStatement?  

CallableStatement is used to execute stored procedures defined inside the database.

Stored procedures are precompiled SQL code stored inside the database.

---

### 6.2 Why Use Stored Procedures?  

- Improved performance  
- Better security  
- Code reuse  
- Centralized business logic  

---

### 6.3 Example Stored Procedure  

```sql
CREATE PROCEDURE getEmployeeCount()
BEGIN
   SELECT COUNT(*) FROM employee;
END;
```

### Java Code  

```java
CallableStatement cs = conn.prepareCall("{call getEmployeeCount()}");
ResultSet rs = cs.executeQuery();

if(rs.next()) {
    System.out.println("Total Employees: " + rs.getInt(1));
}
```

---

## 7. ResultSet Types  

ResultSet behavior can be controlled using types and concurrency modes.

```java
Statement stmt = conn.createStatement(
    ResultSet.TYPE_SCROLL_INSENSITIVE,
    ResultSet.CONCUR_READ_ONLY
);
```

### ResultSet Types  

- TYPE_FORWARD_ONLY  
- TYPE_SCROLL_INSENSITIVE  
- TYPE_SCROLL_SENSITIVE  

### Concurrency Modes  

- CONCUR_READ_ONLY  
- CONCUR_UPDATABLE  

---

## 8. Viva Questions (Detailed Answers)

### 1. What is a transaction?  
A transaction is a sequence of SQL operations executed as a single logical unit. It ensures that either all operations succeed or none are applied.

### 2. Why disable auto-commit?  
To group multiple SQL operations into a single transaction and control when changes are permanently saved.

### 3. What are ACID properties?  
Atomicity, Consistency, Isolation, and Durability ensure reliable database transactions.

### 4. What is batch processing?  
Batch processing executes multiple SQL statements together to improve performance and reduce database calls.

### 5. What is CallableStatement?  
CallableStatement is used to execute stored procedures inside the database.

---

## 9. MCQs (With Answers)

### 1. Default auto-commit mode is:  
A) TRUE  
B) FALSE  
C) Manual  
D) Disabled  
Answer: A  

### 2. Which method permanently saves transaction?  
A) rollback()  
B) commit()  
C) execute()  
D) save()  
Answer: B  

### 3. Which method executes batch statements?  
A) executeQuery()  
B) executeBatch()  
C) runBatch()  
D) batchExecute()  
Answer: B  

### 4. Stored procedures are executed using:  
A) Statement  
B) PreparedStatement  
C) CallableStatement  
D) ResultSet  
Answer: C  

---

## 10. Interview Questions (Detailed Answers)

### 1. Explain ACID properties with real-world example.  
In banking transactions, money transfer must be atomic. If debit occurs but credit fails, rollback ensures consistency.

### 2. Difference between commit() and rollback()?  
commit() permanently saves changes. rollback() cancels changes made during a transaction.

### 3. Why is batch processing important in enterprise systems?  
It reduces network overhead and improves performance when handling large datasets.

### 4. Difference between Statement, PreparedStatement, and CallableStatement?  
Statement executes static SQL. PreparedStatement executes parameterized SQL. CallableStatement executes stored procedures.

---

## 11. Lab Assignment  

1. Create table `accounts(id, balance)`.  
2. Insert two accounts.  
3. Perform money transfer using transaction.  
4. Demonstrate rollback on error.  
5. Insert 20 records using batch processing.  
6. Create one stored procedure and call it from Java.
