# SPRING BOOT – MODULE 2  
# PHASE 12  
## Spring Boot – Spring Security (Authentication and Authorization Basics)  

---

# 1. Introduction  

So far, we have built:

- REST APIs  
- Database integration using JPA and MongoDB  
- Entity relationships  
- Validation and exception handling  

Now we move to one of the most critical parts of backend development:

🔐 **Security**

In this phase, we will cover:

- What is Spring Security?  
- Authentication vs Authorization  
- Basic Authentication  
- In-memory authentication  
- Securing REST APIs  
- Role-based access control  

This is a highly important topic for interviews and real-world applications.

---

# 2. What is Spring Security?  

Spring Security is a powerful framework that provides:

- Authentication  
- Authorization  
- Protection against attacks (CSRF, XSS, etc.)  
- Password encoding  
- Session management  

It integrates seamlessly with Spring Boot.

---

# 3. Authentication vs Authorization  

Authentication → Who are you?  
Authorization → What are you allowed to do?  

Example:

- Login using username and password → Authentication  
- Access admin dashboard → Authorization  

---

# 4. Adding Spring Security Dependency  

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

After adding this dependency:

- All endpoints become secured by default  
- Default username: `user`  
- Random password printed in console  

---

# 5. Basic Security Configuration  

Spring Boot 3 uses SecurityFilterChain instead of WebSecurityConfigurerAdapter.

---

## 5.1 Basic Configuration Example  

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http)
            throws Exception {

        http
            .csrf(csrf -> csrf.disable())
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/public/**").permitAll()
                .anyRequest().authenticated()
            )
            .httpBasic();

        return http.build();
    }
}
```

Explanation:

- `/public/**` is accessible without login  
- Other endpoints require authentication  
- httpBasic() enables basic authentication  

---

# 6. In-Memory Authentication  

We can define users inside application.

```java
import org.springframework.context.annotation.Bean;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;

@Bean
public UserDetailsService userDetailsService() {

    return new InMemoryUserDetailsManager(
        User.withUsername("admin")
            .password("{noop}admin123")
            .roles("ADMIN")
            .build(),
        User.withUsername("user")
            .password("{noop}user123")
            .roles("USER")
            .build()
    );
}
```

`{noop}` means no password encoding (for demo only).

---

# 7. Role-Based Authorization  

We can restrict endpoints based on roles.

```java
.authorizeHttpRequests(auth -> auth
    .requestMatchers("/admin/**").hasRole("ADMIN")
    .requestMatchers("/user/**").hasRole("USER")
    .anyRequest().authenticated()
)
```

Example:

```
/admin/dashboard → Only ADMIN
/user/profile → Only USER
```

---

# 8. Password Encoding  

Storing plain passwords is insecure.

Use PasswordEncoder:

```java
@Bean
public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}
```

BCrypt automatically hashes passwords.

---

# 9. Securing REST APIs  

When accessing secured endpoint:

- Browser shows login popup (Basic Auth)
- Postman requires Authorization header

Header Example:

```
Authorization: Basic base64(username:password)
```

---

# 10. Disabling Security for Development  

To allow all endpoints (not recommended in production):

```java
.authorizeHttpRequests(auth -> auth
    .anyRequest().permitAll()
)
```

---

# 11. Why Security is Important  

- Prevent unauthorized access  
- Protect user data  
- Secure APIs  
- Prevent data breaches  

Every production application must implement security.

---

# 12. Viva Questions (Detailed Answers)

### 1. What is Spring Security?  
Spring Security is a framework that provides authentication and authorization for Spring applications.

### 2. Difference between authentication and authorization?  
Authentication verifies identity.  
Authorization determines permissions.

### 3. What is Basic Authentication?  
A simple authentication mechanism where credentials are sent in HTTP header.

### 4. What is PasswordEncoder?  
It encrypts passwords before storing in database.

### 5. What is role-based access control?  
Access control mechanism based on user roles.

### 6. What is CSRF?  
Cross-Site Request Forgery – an attack where unauthorized commands are transmitted.

---

# 13. MCQs (With Answers)

1. Spring Security dependency is:  
A) spring-boot-starter-web  
B) spring-boot-starter-security  
C) spring-security-core  
D) security-starter  
Answer: B  

2. Authentication means:  
A) Permission checking  
B) Identity verification  
C) Data validation  
D) Logging  
Answer: B  

3. Authorization means:  
A) Login  
B) Encryption  
C) Access control  
D) Validation  
Answer: C  

4. Default Spring Security username is:  
A) admin  
B) root  
C) user  
D) spring  
Answer: C  

5. BCrypt is used for:  
A) Logging  
B) Password hashing  
C) Session handling  
D) Query execution  
Answer: B  

6. Which method defines HTTP security rules?  
A) configure()  
B) securityFilterChain()  
C) enableSecurity()  
D) setAuth()  
Answer: B  

7. Basic authentication sends credentials in:  
A) Body  
B) URL  
C) Header  
D) Cookie  
Answer: C  

8. Which annotation marks configuration class?  
A) @Component  
B) @Configuration  
C) @Security  
D) @Bean  
Answer: B  

9. Roles in Spring start with prefix:  
A) ROLE_  
B) AUTH_  
C) USER_  
D) ADMIN_  
Answer: A  

10. CSRF stands for:  
A) Cross Security Request Format  
B) Cross Site Request Forgery  
C) Central Secure Resource Function  
D) Cross Server Random Failure  
Answer: B  

---

# 14. Interview Questions (Detailed Answers)

### 1. How does Spring Security work internally?  
It uses filter chains to intercept requests and apply authentication and authorization logic.

### 2. What is SecurityFilterChain?  
It defines security configuration and request authorization rules.

### 3. Difference between Basic Auth and JWT?  
Basic Auth sends credentials with every request.  
JWT uses token-based authentication.

### 4. Why is password encoding important?  
To prevent plain text password storage and improve security.

### 5. What is role hierarchy?  
It allows one role to inherit permissions of another role.

### 6. How to secure specific endpoints only?  
Using requestMatchers() with role-based restrictions.

---

# 15. Lab Assignment  

1. Add Spring Security dependency.  
2. Secure all endpoints.  
3. Create in-memory users with roles.  
4. Restrict one endpoint to ADMIN only.  
5. Encode password using BCrypt.  
6. Test APIs using Postman with authentication.
