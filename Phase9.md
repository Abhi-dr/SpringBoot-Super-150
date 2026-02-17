# SPRING BOOT – MODULE 2  
# PHASE 9  
## Spring Boot – REST API Design, Validation and Exception Handling  

---

# 1. Introduction  

In previous phases, we created REST APIs and understood request mapping, dependency injection and layered architecture.

Now we move toward production-level REST API development.

This phase focuses on:

- REST API design best practices  
- DTO (Data Transfer Object)  
- Validation using @Valid  
- Global Exception Handling  
- Custom Exception Creation  
- Proper HTTP Status Codes  

These concepts are extremely important in real-world backend development and interviews.

---

# 2. REST API Design Best Practices  

Good REST APIs should follow:

- Use nouns in URLs (not verbs)
- Use proper HTTP methods
- Return proper HTTP status codes
- Keep APIs stateless
- Use consistent JSON structure

Example (Good Design):

```
GET     /api/students
GET     /api/students/10
POST    /api/students
PUT     /api/students/10
DELETE  /api/students/10
```

Bad Design:

```
GET /getAllStudents
POST /createStudent
```

---

# 3. DTO (Data Transfer Object)

## 3.1 What is DTO?

DTO is used to transfer data between client and server.

Why use DTO?

- Prevent exposing internal entity structure
- Control input and output fields
- Improve security

Example Entity:

```java
public class Student {
    private Long id;
    private String name;
    private String email;
    private String password;
}
```

We should NOT return password in API response.

DTO:

```java
public class StudentDTO {
    private String name;
    private String email;
}
```

---

# 4. Validation in Spring Boot  

Spring Boot supports validation using:

- @Valid
- @NotNull
- @NotBlank
- @Size
- @Email

Add dependency:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

---

## 4.1 Example DTO with Validation

```java
import jakarta.validation.constraints.*;

public class StudentDTO {

    @NotBlank(message = "Name cannot be empty")
    private String name;

    @Email(message = "Invalid email format")
    private String email;

    @Size(min = 6, message = "Password must be at least 6 characters")
    private String password;

    // getters and setters
}
```

---

## 4.2 Using @Valid in Controller

```java
@PostMapping("/students")
public ResponseEntity<String> createStudent(
        @Valid @RequestBody StudentDTO student) {

    return ResponseEntity
            .status(201)
            .body("Student Created Successfully");
}
```

If validation fails, Spring automatically throws an exception.

---

# 5. Exception Handling in Spring Boot  

If something goes wrong, we must return proper error responses.

---

## 5.1 Custom Exception

```java
public class StudentNotFoundException extends RuntimeException {

    public StudentNotFoundException(String message) {
        super(message);
    }
}
```

---

## 5.2 Throwing Custom Exception

```java
@GetMapping("/students/{id}")
public String getStudent(@PathVariable Long id) {

    if(id > 100) {
        throw new StudentNotFoundException("Student Not Found");
    }

    return "Student Found";
}
```

---

## 5.3 Global Exception Handler

```java
import org.springframework.web.bind.annotation.*;

@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(StudentNotFoundException.class)
    public ResponseEntity<String> handleStudentNotFound(
            StudentNotFoundException ex) {

        return ResponseEntity
                .status(404)
                .body(ex.getMessage());
    }
}
```

Now whenever exception occurs, it returns proper HTTP 404.

---

# 6. Handling Validation Errors Globally

```java
@ExceptionHandler(MethodArgumentNotValidException.class)
public ResponseEntity<Map<String, String>> handleValidation(
        MethodArgumentNotValidException ex) {

    Map<String, String> errors = new HashMap<>();

    ex.getBindingResult().getFieldErrors()
            .forEach(error ->
                    errors.put(error.getField(), error.getDefaultMessage()));

    return ResponseEntity.badRequest().body(errors);
}
```

---

# 7. HTTP Status Codes (Important)

| Code | Meaning |
|------|----------|
| 200  | OK |
| 201  | Created |
| 400  | Bad Request |
| 401  | Unauthorized |
| 404  | Not Found |
| 500  | Internal Server Error |

Always return meaningful status codes.

---

# 8. Why These Concepts Are Critical

- Validation ensures clean input
- DTO improves security
- Global exception handling improves maintainability
- Proper HTTP codes improve API quality
- Industry-standard API design

---

# 9. Viva Questions (Detailed Answers)

### 1. What is DTO and why use it?
DTO is used to transfer data between layers or between client and server. It prevents exposing internal entity details and improves security.

### 2. What is @Valid?
@Valid triggers validation rules defined in DTO before method execution.

### 3. What is @RestControllerAdvice?
It is used for global exception handling in REST APIs.

### 4. Why use custom exceptions?
To provide meaningful error messages and better control over error responses.

### 5. What happens if validation fails?
Spring throws MethodArgumentNotValidException.

### 6. Why proper HTTP status codes are important?
They clearly communicate success or failure of request to client.

---

# 10. MCQs (With Answers)

1. Which annotation enables validation?
A) @Validate  
B) @Valid  
C) @Check  
D) @NotNull  
Answer: B  

2. @RestControllerAdvice is used for:
A) Controller creation  
B) Dependency injection  
C) Global exception handling  
D) Database connection  
Answer: C  

3. 201 status code means:
A) OK  
B) Created  
C) Bad Request  
D) Unauthorized  
Answer: B  

4. Which annotation validates email?
A) @NotBlank  
B) @Email  
C) @Size  
D) @CheckEmail  
Answer: B  

5. DTO is mainly used for:
A) Database connection  
B) Security and data transfer  
C) Logging  
D) Configuration  
Answer: B  

6. Which exception occurs on validation failure?
A) NullPointerException  
B) RuntimeException  
C) MethodArgumentNotValidException  
D) SQLException  
Answer: C  

7. 404 status code represents:
A) OK  
B) Not Found  
C) Created  
D) Internal Error  
Answer: B  

8. @ExceptionHandler handles:
A) HTTP request  
B) Errors  
C) Database  
D) Injection  
Answer: B  

9. Validation annotations come from:
A) java.util  
B) jakarta.validation  
C) org.sql  
D) javax.servlet  
Answer: B  

10. Proper REST API should use:
A) Verbs in URL  
B) Nouns in URL  
C) Random names  
D) HTML only  
Answer: B  

---

# 11. Interview Questions (Detailed Answers)

### 1. Difference between @ControllerAdvice and @RestControllerAdvice?
@RestControllerAdvice combines @ControllerAdvice and @ResponseBody.

### 2. How to customize error response structure?
By creating a custom error response class and returning it inside exception handler.

### 3. Why should entity not be exposed directly?
It may contain sensitive fields and internal structure.

### 4. How do you handle validation errors globally?
Using @ExceptionHandler for MethodArgumentNotValidException.

### 5. How to return custom HTTP status?
Using ResponseEntity.status().

### 6. What is stateless API?
Each request is independent and server does not store session data.

---

# 12. Lab Assignment

1. Create Student API with DTO.
2. Add validation annotations.
3. Create custom exception.
4. Implement global exception handler.
5. Return proper HTTP status codes.
6. Test all APIs using Postman.
7. Handle validation errors properly.
