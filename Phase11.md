# SPRING BOOT – MODULE 2  
# PHASE 11  
## Spring Boot – Relationships, Fetch Types and Advanced JPA Concepts  

---

# 1. Introduction  

In the previous phase, we learned:

- JPA basics  
- Spring Data JPA  
- MongoDB integration  
- CRUD operations  

Now we move into **advanced JPA concepts** that are very important in real-world backend systems and interviews.

This phase covers:

- Entity Relationships (One-to-One, One-to-Many, Many-to-One, Many-to-Many)  
- Fetch Types (Lazy vs Eager)  
- Cascade Types  
- JPQL  
- Pagination and Sorting  
- Transaction Management  

---

# 2. Entity Relationships in JPA  

Relational databases are based on relationships.

JPA supports:

- One-to-One  
- One-to-Many  
- Many-to-One  
- Many-to-Many  

---

# 3. One-to-One Relationship  

Example:

Each student has one profile.

### Student Entity

```java
@Entity
public class Student {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @OneToOne(cascade = CascadeType.ALL)
    @JoinColumn(name = "profile_id")
    private Profile profile;
}
```

### Profile Entity

```java
@Entity
public class Profile {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String address;
}
```

---

# 4. One-to-Many and Many-to-One  

Example:

One student can have many courses.

---

### Student Entity

```java
@Entity
public class Student {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @OneToMany(mappedBy = "student",
               cascade = CascadeType.ALL,
               fetch = FetchType.LAZY)
    private List<Course> courses;
}
```

---

### Course Entity

```java
@Entity
public class Course {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String title;

    @ManyToOne
    @JoinColumn(name = "student_id")
    private Student student;
}
```

---

# 5. Many-to-Many Relationship  

Example:

Students and Subjects.

```java
@ManyToMany
@JoinTable(
    name = "student_subject",
    joinColumns = @JoinColumn(name = "student_id"),
    inverseJoinColumns = @JoinColumn(name = "subject_id")
)
private Set<Subject> subjects;
```

This creates a join table automatically.

---

# 6. Fetch Types  

Fetch type defines when related data is loaded.

### FetchType.EAGER  

- Loads related data immediately  
- May reduce performance  

### FetchType.LAZY  

- Loads related data only when accessed  
- Improves performance  

Example:

```java
@OneToMany(fetch = FetchType.LAZY)
```

---

# 7. Cascade Types  

Cascade defines what happens to child entities when parent changes.

Common types:

- CascadeType.ALL  
- CascadeType.PERSIST  
- CascadeType.MERGE  
- CascadeType.REMOVE  

Example:

```java
@OneToMany(cascade = CascadeType.ALL)
```

If parent is deleted, child also deleted.

---

# 8. JPQL (Java Persistence Query Language)  

JPQL is similar to SQL but works with entity names, not table names.

Example:

```java
@Query("SELECT s FROM Student s WHERE s.name = :name")
List<Student> findByStudentName(@Param("name") String name);
```

---

# 9. Pagination and Sorting  

Spring Data JPA provides built-in pagination.

Example:

```java
Page<Student> students =
repository.findAll(PageRequest.of(0, 5));
```

Sorting:

```java
repository.findAll(Sort.by("name").ascending());
```

---

# 10. Transaction Management  

Transactions ensure data consistency.

Spring uses:

```java
@Transactional
```

Example:

```java
@Transactional
public void updateStudent(Long id, String name) {
    Student student = repository.findById(id).orElseThrow();
    student.setName(name);
}
```

Spring automatically commits transaction.

---

# 11. Why These Concepts Are Critical  

- Real-world applications use relationships  
- Lazy loading improves performance  
- Cascade ensures data consistency  
- Pagination required for large datasets  
- Transactions ensure atomic operations  

These are frequently asked in interviews.

---

# 12. Viva Questions (Detailed Answers)

### 1. Difference between OneToMany and ManyToOne?  
OneToMany defines one parent with multiple children.  
ManyToOne defines multiple children belonging to one parent.

### 2. What is fetch type?  
It defines when related data should be loaded.

### 3. Difference between LAZY and EAGER?  
LAZY loads data only when needed.  
EAGER loads immediately.

### 4. What is JPQL?  
JPQL is query language that works on entities instead of tables.

### 5. What is CascadeType.ALL?  
It applies all cascade operations like persist, merge, remove.

### 6. What is @Transactional?  
It ensures method executes within a transaction.

---

# 13. MCQs (With Answers)

1. OneToMany relationship uses:  
A) @OneToMany  
B) @ManyToOne  
C) @JoinTable  
D) @Fetch  
Answer: A  

2. Default fetch type for ManyToOne is:  
A) LAZY  
B) EAGER  
C) MANUAL  
D) NONE  
Answer: B  

3. JPQL works on:  
A) Tables  
B) Columns  
C) Entities  
D) Database  
Answer: C  

4. Pagination class is:  
A) Pageable  
B) PageRequest  
C) Both  
D) None  
Answer: C  

5. @Transactional ensures:  
A) Logging  
B) Security  
C) Atomic operations  
D) Serialization  
Answer: C  

6. Many-to-Many creates:  
A) One table  
B) Two tables  
C) Join table  
D) No table  
Answer: C  

7. CascadeType.REMOVE does:  
A) Updates  
B) Deletes child  
C) Fetches data  
D) None  
Answer: B  

8. LAZY loading improves:  
A) Security  
B) Performance  
C) UI  
D) Logging  
Answer: B  

9. @JoinColumn defines:  
A) Table  
B) Foreign key  
C) Primary key  
D) Query  
Answer: B  

10. JPQL query starts with:  
A) SELECT *  
B) SELECT entity  
C) FETCH  
D) JOIN  
Answer: B  

---

# 14. Interview Questions (Detailed Answers)

### 1. What is difference between LAZY and EAGER loading?  
LAZY loads associated entities only when accessed.  
EAGER loads immediately with parent entity.

### 2. What is N+1 problem?  
When fetching parent entities triggers multiple additional queries for children.

### 3. How to solve N+1 problem?  
Using JOIN FETCH or EntityGraph.

### 4. What is owning side in relationship?  
Owning side contains foreign key and controls mapping.

### 5. What is difference between JPQL and native query?  
JPQL works on entities. Native query works on actual SQL tables.

### 6. Why use pagination?  
To limit result size and improve performance.

---

# 15. Lab Assignment  

1. Create Student and Course entities with OneToMany relationship.  
2. Insert sample data.  
3. Test LAZY vs EAGER behavior.  
4. Implement JPQL query.  
5. Implement pagination and sorting.  
6. Add @Transactional to update method.
```
