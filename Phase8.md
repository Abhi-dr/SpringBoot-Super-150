# SPRING BOOT – MODULE 2  
# PHASE 8  
## Spring Boot – Layered Architecture, Bean Lifecycle and Advanced Dependency Injection  

---

# 1. Introduction  

In this phase, we move deeper into Spring Boot core concepts that are heavily asked in interviews and widely used in real-world projects.

This phase focuses on:

- Layered Architecture (Detailed)  
- Bean Lifecycle  
- Types of Dependency Injection  
- @Qualifier and @Primary  
- @Configuration and @Bean  
- application.properties deep understanding  

These concepts form the backbone of enterprise Spring Boot applications.

---

# 2. Layered Architecture in Spring Boot  

A standard Spring Boot application follows a layered architecture:

```
Controller → Service → Repository → Database
```

Each layer has a clear responsibility.

---

## 2.1 Controller Layer  

- Handles HTTP requests  
- Accepts user input  
- Returns HTTP responses  
- Should NOT contain business logic  

Example:

```java
@RestController
@RequestMapping("/students")
public class StudentController {

    private final StudentService service;

    public StudentController(StudentService service) {
        this.service = service;
    }

    @GetMapping
    public List<String> getStudents() {
        return service.getAllStudents();
    }
}
```

---

## 2.2 Service Layer  

- Contains business logic  
- Acts as mediator between controller and repository  
- Performs validations and calculations  

```java
@Service
public class StudentService {

    public List<String> getAllStudents() {
        return List.of("Amit", "Neha", "Raj");
    }
}
```

---

## 2.3 Repository Layer  

- Handles database operations  
- Communicates with database  
- Contains CRUD logic  

```java
@Repository
public class StudentRepository {
}
```

---

# 3. Bean Lifecycle in Spring  

A Spring Bean goes through several stages:

1. Bean Instantiation  
2. Dependency Injection  
3. Initialization  
4. Ready for use  
5. Destruction  

---

## 3.1 Bean Lifecycle Example  

```java
@Component
public class DemoBean {

    public DemoBean() {
        System.out.println("Bean Created");
    }

    @PostConstruct
    public void init() {
        System.out.println("Bean Initialized");
    }

    @PreDestroy
    public void destroy() {
        System.out.println("Bean Destroyed");
    }
}
```

`@PostConstruct` runs after dependency injection.  
`@PreDestroy` runs before bean destruction.

---

# 4. Types of Dependency Injection  

---

## 4.1 Constructor Injection (Recommended)

```java
@Service
public class UserService {

    private final UserRepository repository;

    public UserService(UserRepository repository) {
        this.repository = repository;
    }
}
```

Advantages:
- Immutable dependencies  
- Easier testing  
- Recommended by Spring  

---

## 4.2 Setter Injection  

```java
@Service
public class UserService {

    private UserRepository repository;

    @Autowired
    public void setRepository(UserRepository repository) {
        this.repository = repository;
    }
}
```

---

## 4.3 Field Injection (Not Recommended)

```java
@Autowired
private UserRepository repository;
```

Problems:
- Harder to test  
- Breaks immutability  

---

# 5. @Qualifier and @Primary  

When multiple beans of same type exist, Spring throws:

```
NoUniqueBeanDefinitionException
```

---

## 5.1 Using @Primary  

```java
@Service
@Primary
public class MySQLUserService implements UserService {
}
```

Spring gives preference to this bean.

---

## 5.2 Using @Qualifier  

```java
@Autowired
public UserController(@Qualifier("oracleUserService") UserService service) {
    this.service = service;
}
```

Specifies exact bean to inject.

---

# 6. @Configuration and @Bean  

Used to define custom beans manually.

```java
@Configuration
public class AppConfig {

    @Bean
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }
}
```

Spring registers this as a bean in container.

---

# 7. application.properties Deep Understanding  

application.properties controls configuration.

Examples:

```
server.port=9090
spring.application.name=my-app
spring.datasource.url=jdbc:mysql://localhost:3306/testdb
spring.datasource.username=root
spring.datasource.password=root
```

This configures:
- Server port  
- Application name  
- Database connection  

---

# 8. Why These Concepts Matter  

- Layered architecture ensures maintainability  
- Constructor injection ensures clean design  
- Bean lifecycle is important in production  
- Qualifier resolves multiple bean conflicts  
- Properties file controls environment configuration  

These are common real-world and interview topics.

---

# 9. Viva Questions (Detailed Answers)

### 1. What is layered architecture?  
It separates application into Controller, Service, and Repository layers for clean separation of concerns.

### 2. Which dependency injection type is recommended?  
Constructor injection is recommended because it ensures immutability and better testability.

### 3. What is Bean lifecycle?  
It is the sequence of steps a bean goes through from creation to destruction inside Spring container.

### 4. Why use @Qualifier?  
To resolve ambiguity when multiple beans of same type exist.

### 5. What is @Configuration?  
It indicates that a class contains bean definitions.

### 6. What is difference between @Component and @Bean?  
@Component is used for auto-detected classes.  
@Bean is used for manual bean creation inside configuration class.

---

# 10. MCQs (With Answers)

1. Which injection type is recommended?  
A) Field  
B) Constructor  
C) Setter  
D) XML  
Answer: B  

2. Which annotation defines custom bean?  
A) @Component  
B) @Bean  
C) @Autowired  
D) @Service  
Answer: B  

3. If two beans of same type exist, Spring throws:  
A) NullPointerException  
B) BeanNotFoundException  
C) NoUniqueBeanDefinitionException  
D) InjectionException  
Answer: C  

4. Which annotation runs after bean creation?  
A) @PreDestroy  
B) @Autowired  
C) @PostConstruct  
D) @Component  
Answer: C  

5. application.properties is located in:  
A) src/main/java  
B) src/main/resources  
C) target  
D) root  
Answer: B  

6. Which layer contains business logic?  
A) Controller  
B) Repository  
C) Service  
D) Entity  
Answer: C  

7. @Primary is used to:  
A) Create bean  
B) Inject dependency  
C) Give preference to bean  
D) Configure database  
Answer: C  

8. Bean destruction method uses:  
A) @Destroy  
B) @PreDestroy  
C) @End  
D) @Close  
Answer: B  

---

# 11. Interview Questions (Detailed Answers)

### 1. Why is constructor injection preferred over field injection?  
Constructor injection ensures immutability, easier unit testing, and better design.

### 2. What happens during bean lifecycle?  
Bean is instantiated, dependencies injected, initialization method executed, bean used, and finally destroyed.

### 3. Difference between @Component, @Service, @Repository?  
All are stereotypes of @Component.  
@Service indicates business logic.  
@Repository indicates data access layer.

### 4. How does Spring resolve multiple bean conflicts?  
Using @Primary or @Qualifier.

### 5. What is difference between @Bean and @Component?  
@Component is automatically detected.  
@Bean is manually declared inside configuration class.

### 6. What is dependency injection?  
It is a design pattern where dependencies are provided externally rather than created inside the class.

### 7. How does Spring Boot read properties file?  
It automatically loads application.properties from resources folder during startup.

---

# 12. Lab Assignment  

1. Create layered architecture (Controller, Service, Repository).  
2. Use constructor injection.  
3. Create two service implementations and resolve using @Qualifier.  
4. Create custom bean using @Configuration and @Bean.  
5. Add custom port in application.properties.  
6. Observe bean lifecycle using @PostConstruct and @PreDestroy.
