# SPRING BOOT – MODULE 2  
# PHASE 13  
## Spring Boot – JWT Authentication and Stateless Security  

---

# 1. Introduction  

In the previous phase, we learned:

- Basic Authentication  
- Role-based authorization  
- Spring Security configuration  

However, Basic Authentication is not suitable for modern production systems.

In this phase, we move to:

- JWT (JSON Web Token) Authentication  
- Stateless Security  
- Token-based authentication  
- Securing REST APIs using JWT  

JWT is widely used in:

- Microservices  
- Mobile apps  
- Frontend-backend architectures  
- Production-grade REST APIs  

---

# 2. What is JWT?  

JWT (JSON Web Token) is a compact, URL-safe token used for authentication.

Instead of sending username and password with every request:

- User logs in once  
- Server generates token  
- Client stores token  
- Client sends token in header for future requests  

---

# 3. Structure of JWT  

JWT consists of three parts:

```
Header.Payload.Signature
```

Example:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

---

### 3.1 Header  

Contains:

- Algorithm (HS256)  
- Token type  

---

### 3.2 Payload  

Contains claims like:

- Username  
- Roles  
- Expiration time  

---

### 3.3 Signature  

Ensures token integrity.

If payload is modified, signature becomes invalid.

---

# 4. Why JWT is Important  

- Stateless authentication  
- No session storage required  
- Scalable  
- Secure and compact  

Used heavily in production systems.

---

# 5. How JWT Authentication Works  

Step 1 → User sends login request  
Step 2 → Server validates credentials  
Step 3 → Server generates JWT  
Step 4 → Client stores token  
Step 5 → Client sends token in Authorization header  

Header format:

```
Authorization: Bearer <token>
```

---

# 6. Add JWT Dependency  

```xml
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-api</artifactId>
    <version>0.11.5</version>
</dependency>
```

(Plus required implementation dependencies)

---

# 7. Creating JWT Utility Class  

```java
import io.jsonwebtoken.*;
import io.jsonwebtoken.security.Keys;
import java.security.Key;
import java.util.Date;

public class JwtUtil {

    private static final Key key =
            Keys.secretKeyFor(SignatureAlgorithm.HS256);

    public static String generateToken(String username) {

        return Jwts.builder()
                .setSubject(username)
                .setIssuedAt(new Date())
                .setExpiration(new Date(System.currentTimeMillis() + 3600000))
                .signWith(key)
                .compact();
    }

    public static String extractUsername(String token) {

        return Jwts.parserBuilder()
                .setSigningKey(key)
                .build()
                .parseClaimsJws(token)
                .getBody()
                .getSubject();
    }
}
```

---

# 8. Creating Authentication Endpoint  

```java
@RestController
@RequestMapping("/auth")
public class AuthController {

    @PostMapping("/login")
    public String login(@RequestParam String username) {

        return JwtUtil.generateToken(username);
    }
}
```

User receives token after login.

---

# 9. JWT Filter  

Create filter to validate token on every request.

```java
@Component
public class JwtFilter extends OncePerRequestFilter {

    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain)
            throws ServletException, IOException {

        String header = request.getHeader("Authorization");

        if(header != null && header.startsWith("Bearer ")) {

            String token = header.substring(7);
            String username = JwtUtil.extractUsername(token);

            UsernamePasswordAuthenticationToken auth =
                    new UsernamePasswordAuthenticationToken(
                            username, null, List.of());

            SecurityContextHolder.getContext()
                    .setAuthentication(auth);
        }

        filterChain.doFilter(request, response);
    }
}
```

---

# 10. Configure Security for JWT  

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http)
        throws Exception {

    http
        .csrf(csrf -> csrf.disable())
        .authorizeHttpRequests(auth -> auth
            .requestMatchers("/auth/**").permitAll()
            .anyRequest().authenticated()
        );

    return http.build();
}
```

JWT filter must be added before UsernamePasswordAuthenticationFilter.

---

# 11. Stateless Session Configuration  

```java
http.sessionManagement(session ->
    session.sessionCreationPolicy(
        SessionCreationPolicy.STATELESS
    )
);
```

This ensures no session is stored on server.

---

# 12. Basic Auth vs JWT  

| Feature | Basic Auth | JWT |
|----------|------------|------|
| Session | Stateful | Stateless |
| Security | Moderate | High |
| Scalability | Low | High |
| Used In | Small apps | Production apps |

---

# 13. Why JWT is Industry Standard  

- Used in microservices  
