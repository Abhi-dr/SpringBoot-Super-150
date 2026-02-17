# SPRING BOOT – MODULE 2  
# PHASE 10  
## Spring Boot – JPA, Spring Data JPA and MongoDB Integration  

---

# 1. Introduction  

In this phase, we move into **database integration in Spring Boot**.

We will cover:

- What is JPA?  
- What is Hibernate?  
- Spring Data JPA (Relational Database – MySQL)  
- Entity Mapping  
- Repository Layer  
- CRUD Operations  
- Introduction to MongoDB  
- Spring Data MongoDB  
- JPA vs MongoDB Comparison  

This phase is extremely important for interviews and real-world backend development.

---

# 2. What is JPA?  

JPA (Java Persistence API) is a **specification** that defines how Java objects should be mapped to relational databases.

Important Points:

- JPA is NOT an implementation  
- It provides annotations and interfaces  
- Hibernate is the most popular implementation of JPA  

JPA helps in:

- Mapping Java classes to database tables  
- Managing object lifecycle  
- Reducing boilerplate JDBC code  

---

# 3. What is Hibernate?  

Hibernate is an ORM (Object Relational Mapping) tool.

ORM means:

- Java Object ↔ Database Table  
- Java Fields ↔ Table Columns  

Example:

Java Object:

```java
Student student = new Student("Amit", "amit@gmail.com");
```

Database Table:

```
id | name  | email
1  | Amit  | amit@gmail.com
```

Hibernate automatically handles SQL generation and data mapping.

---

# 4. What is Spring Data JPA?  

Spring Data JPA is a module of Spring that simplifies JPA usage.

Instead of writing:

- SQL queries  
- JDBC code  
- Manual connection handling  

We only define repository interfaces.

Spring automatically provides implementation.

---

# 5. Spring Data JPA – MySQL Version  

---

## 5.1 Dependencies  

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>

<dependency>
    <groupId>com.mysql</groupId>
    <artifactId>mysql-connector-j</artifactId>
</dependency>
```

---

## 5.2 application.properties (MySQL)

```
spring.datasource.url=jdbc:mysql://localhost:3306/testdb
spring.datasource.username=root
spring.datasource.password=root

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
```

Important:

`ddl-auto` options:

- create → Drops and recreates table  
- update → Updates schema  
- validate → Validates schema  
- none → No changes  

---

## 5.3 Entity Class (Relational DB)

```java
import jakarta.persistence.*;

@Entity
@Table(name = "students")
public class Student {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String email;

    // getters and setters
}
```

Annotations Explained:

- @Entity → Marks class as DB entity  
- @Table → Table mapping  
- @Id → Primary key  
- @GeneratedValue → Auto increment  

---

## 5.4 Repository Interface  

```java
import org.springframework.data.jpa.repository.JpaRepository;

public interface StudentRepository
        extends JpaRepository<Student, Long> {

    List<Student> findByName(String name);
}
```

Built-in methods:

- save()  
- findAll()  
- findById()  
- deleteById()  
- count()  

---

## 5.5 Service Example  

```java
@Service
public class StudentService {

    private final StudentRepository repository;

    public StudentService(StudentRepository repository) {
        this.repository = repository;
    }

    public Student createStudent(Student student) {
        return repository.save(student);
    }

    public List<Student> getAllStudents() {
        return repository.findAll();
    }
}
```

---

# 6. MongoDB Integration (NoSQL Version)

MongoDB is a NoSQL document-based database.

Instead of tables and rows, it uses:

- Collections  
- Documents (JSON-like structure)

Example Mongo Document:

```json
{
  "_id": "123",
  "name": "Amit",
  "email": "amit@gmail.com"
}
```

---

## 6.1 Dependency for MongoDB  

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>
```

---

## 6.2 application.properties (MongoDB)

```
spring.data.mongodb.uri=mongodb://localhost:27017/testdb
```

---

## 6.3 Mongo Entity (Document)

```java
import org.springframework.data.annotation.Id;
import org.springframework.data.mongodb.core.mapping.Document;

@Document(collection = "students")
public class Student {

    @Id
    private String id;

    private String name;
    private String email;

    // getters and setters
}
```

Annotations Explained:

- @Document → Maps class to Mongo collection  
- @Id → Primary key  

No @GeneratedValue needed.

---

## 6.4 Mongo Repository  

```java
import org.springframework.data.mongodb.repository.MongoRepository;

public interface StudentRepository
        extends MongoRepository<Student, String> {

    List<Student> findByName(String name);
}
```

Works similar to JPA repository.

---

# 7. JPA vs MongoDB  

| Feature | JPA (MySQL) | MongoDB |
|----------|-------------|----------|
| Database Type | Relational | NoSQL |
| Structure | Tables & Rows | Collections & Documents |
| Schema | Fixed schema | Flexible schema |
| Relationships | Strong (Foreign keys) | Weak / Embedded documents |
| Best For | Structured data | Dynamic data |

---

# 8. When to Use What?  

Use JPA (Relational DB) when:

- Strong relationships required  
- Transactions are critical  
- Structured data  

Use MongoDB when:

- Flexible schema needed  
- Large volume of JSON data  
- High scalability  

---

# 9. Viva Questions (Detailed Answers)

### 1. What is JPA?  
JPA is a specification that defines how Java objects are mapped to relational databases.

### 2. What is Hibernate?  
Hibernate is an ORM tool and implementation of JPA.

### 3. What is Spring Data JPA?  
It simplifies JPA usage by providing repository interfaces with built-in CRUD methods.

### 4. What is MongoDB?  
MongoDB is a NoSQL document-based database that stores data in JSON-like format.

### 5. Difference between @Entity and @Document?  
@Entity is used for relational databases.  
@Document is used for MongoDB.

### 6. What is ORM?  
ORM maps Java objects to database tables.

---

# 10. MCQs (With Answers)

1. JPA is:  
A) Implementation  
B) Specification  
C) Database  
D) Framework  
Answer: B  

2. Hibernate is:  
A) Specification  
B) Database  
C) ORM implementation  
D) HTTP tool  
Answer: C  

3. MongoDB stores data as:  
A) Tables  
B) Rows  
C) Documents  
D) XML  
Answer: C  

4. JpaRepository belongs to:  
A) Mongo  
B) JDBC  
C) Spring Data JPA  
D) Servlet  
Answer: C  

5. @Document is used in:  
A) MySQL  
B) Oracle  
C) MongoDB  
D) PostgreSQL  
Answer: C  

6. ddl-auto=update means:  
A) Delete tables  
B) Update schema automatically  
C) Disable schema  
D) Freeze schema  
Answer: B  

7. MongoRepository extends:  
A) CrudRepository  
B) ListRepository  
C) MongoRepository  
D) JpaRepository  
Answer: C  

8. Relational database uses:  
A) JSON  
B) Documents  
C) Tables  
D) Objects  
Answer: C  

9. NoSQL database example:  
A) MySQL  
B) Oracle  
C) MongoDB  
D) PostgreSQL  
Answer: C  

10. ORM stands for:  
A) Object Relational Mapping  
B) Object Resource Model  
C) Online Resource Manager  
D) Open Relational Module  
Answer: A  

---

# 11. Interview Questions (Detailed Answers)

### 1. Difference between JPA and Hibernate?  
JPA is a specification. Hibernate is an implementation of JPA.

### 2. Difference between JpaRepository and CrudRepository?  
JpaRepository provides additional features like pagination and sorting.

### 3. When would you choose MongoDB over MySQL?  
When flexible schema and horizontal scaling are required.

### 4. What is entity lifecycle in JPA?  
Transient → Persistent → Detached → Removed.

### 5. What is lazy loading?  
Data is fetched only when accessed.

### 6. What is eager loading?  
Data is fetched immediately with parent entity.

---

# 12. Lab Assignment  

1. Create Spring Boot project with MySQL and JPA.  
2. Create Student entity and repository.  
3. Implement CRUD APIs.  
4. Enable show-sql and observe generated SQL.  
5. Create another project using MongoDB.  
6. Implement CRUD using MongoRepository.  
7. Compare relational and Mongo responses.
