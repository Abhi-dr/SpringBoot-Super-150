# SPRING BOOT – MODULE 2  
# PHASE 7  
## Introduction to Spring Framework and Spring Boot  

---

## 1. Introduction  

So far, we have completed the foundational Java and JDBC concepts from Module 1.  

Now we begin Module 2, which focuses on the Spring Framework and Spring Boot.

Spring Boot is built on top of the Spring Framework and simplifies enterprise application development.

---

## 2. What is Spring Framework?  

Spring is a lightweight, open-source Java framework used to build enterprise-level applications.

It provides:

- Dependency Injection (DI)  
- Inversion of Control (IoC)  
- Aspect-Oriented Programming (AOP)  
- Transaction Management  
- Web MVC framework  

Spring helps in building loosely coupled and maintainable applications.

---

## 3. Problems Before Spring  

Before Spring, Java Enterprise applications used:

- Heavy configuration (XML-based)  
- Tight coupling between classes  
- Complex deployment using EJB  

Spring simplified enterprise development by introducing IoC and DI.

---

## 4. What is Spring Boot?  

Spring Boot is an extension of the Spring Framework that simplifies setup and development.

Spring Boot provides:

- Auto-configuration  
- Embedded servers (Tomcat, Jetty)  
- Starter dependencies  
- Minimal configuration  
- Production-ready features  

Spring Boot reduces boilerplate configuration.

---

## 5. Difference Between Spring and Spring Boot  

| Spring Framework | Spring Boot |
|------------------|------------|
| Requires manual configuration | Provides auto-configuration |
| Needs external server | Embedded server |
| More setup required | Minimal setup |
| XML-based configuration common | Annotation-based configuration |

---

## 6. Inversion of Control (IoC)  

### 6.1 What is IoC?  

In traditional programming, objects create their own dependencies.

In IoC, the framework creates and manages objects.

This reduces tight coupling.

Example (Without IoC):

```java
class Engine {}

class Car {
    Engine engine = new Engine();
}
```

Example (With IoC concept):

```java
class Car {
    Engine engine;
    Car(Engine engine) {
        this.engine = engine;
    }
}
```

Spring manages object creation and injection.

---

## 7. Dependency Injection (DI)  

Dependency Injection is a design pattern where dependencies are injected into a class rather than created inside the class.

Types of DI:

1. Constructor Injection  
2. Setter Injection  
3. Field Injection  

Example (Constructor Injection):

```java
@Component
class Engine {}

@Component
class Car {

    private Engine engine;

    @Autowired
    public Car(Engine engine) {
        this.engine = engine;
    }
}
```

Spring automatically injects Engine into Car.

---

## 8. Spring Container  

The Spring Container:

- Creates objects (beans)  
- Manages lifecycle  
- Injects dependencies  

Two main containers:

- BeanFactory  
- ApplicationContext (more commonly used)

---

## 9. Creating First Spring Boot Application  

Steps:

1. Go to https://start.spring.io  
2. Select:
   - Project: Maven  
   - Language: Java  
   - Spring Boot Version  
3. Add dependency: Spring Web  
4. Download project  
5. Import in IDE  
6. Run main class  

---

## 10. Basic Spring Boot Application Example  

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

`@SpringBootApplication` includes:

- @Configuration  
- @EnableAutoConfiguration  
- @ComponentScan  

---

## 11. Creating Simple REST Endpoint  

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello Spring Boot";
    }
}
```

Run application and access:

```
http://localhost:8080/hello
```

---

## 12. Viva Questions (Detailed Answers)

### 1. What is Spring Framework?  
Spring is a lightweight framework used for developing enterprise-level Java applications. It provides IoC, DI, and many other features.

### 2. What is Spring Boot?  
Spring Boot is an extension of Spring that simplifies configuration and setup of Spring applications.

### 3. What is IoC?  
Inversion of Control is a design principle where object creation and management are handled by the framework instead of the programmer.

### 4. What is Dependency Injection?  
Dependency Injection is a design pattern where dependencies are provided externally rather than created inside a class.

### 5. What is Spring Container?  
Spring Container manages the lifecycle of beans and performs dependency injection.

---

## 13. MCQs (With Answers)

### 1. Spring Boot provides:  
A) Manual configuration  
B) Auto-configuration  
C) No configuration  
D) XML only  
Answer: B  

### 2. Default embedded server in Spring Boot is:  
A) Jetty  
B) Tomcat  
C) JBoss  
D) GlassFish  
Answer: B  

### 3. Which annotation starts Spring Boot application?  
A) @Controller  
B) @SpringBootApplication  
C) @Bean  
D) @Component  
Answer: B  

### 4. IoC stands for:  
A) Injection of Code  
B) Internet of Control  
C) Inversion of Control  
D) Integration of Components  
Answer: C  

---

## 14. Interview Questions (Detailed Answers)

### 1. Difference between Spring and Spring Boot?  
Spring requires manual configuration and external server. Spring Boot provides auto-configuration and embedded server.

### 2. What is the role of @SpringBootApplication?  
It combines configuration, auto-configuration, and component scanning.

### 3. Why is Spring Boot popular?  
Because it reduces setup time, provides embedded servers, and simplifies development.

### 4. What is loose coupling?  
Loose coupling means components depend on abstractions rather than concrete implementations.

---

## 15. Lab Assignment  

1. Create a Spring Boot project using Spring Initializr.  
2. Add Spring Web dependency.  
3. Create a controller with one GET endpoint.  
4. Return a simple message.  
5. Run application and test in browser or Postman.  
