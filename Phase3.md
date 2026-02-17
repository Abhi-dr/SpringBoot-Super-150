# SPRING BOOT – MODULE 1  
# PHASE 3  
## Java 8 Features – Lambda Expressions and Functional Interfaces  

---

## 1. Introduction  

After understanding JDBC concepts, the next important foundation topic in Module 1 is **Java 8 features**.  

Java 8 introduced major improvements that changed the way modern Java applications are written.  
Most modern frameworks (including Spring Boot) heavily rely on Java 8 concepts.

In this phase, we focus on:

- Lambda Expressions  
- Functional Interfaces  
- Built-in Functional Interfaces  

---

## 2. What is a Lambda Expression?  

A Lambda Expression is a short block of code that takes input parameters and returns a value.  

It provides a clear and concise way to represent a method without writing a full class.

Before Java 8, to implement an interface method, we had to create:

- A separate class  
OR  
- An anonymous inner class  

Java 8 allows writing this in a shorter form using lambda expressions.

---

## 3. Syntax of Lambda Expression  

Basic Syntax:

```java
(parameters) -> expression
```

OR

```java
(parameters) -> {
    // multiple statements
}
```

---

## 4. Example Without Lambda (Before Java 8)

```java
interface MyInterface {
    void show();
}

public class Test {
    public static void main(String[] args) {

        MyInterface obj = new MyInterface() {
            public void show() {
                System.out.println("Hello World");
            }
        };

        obj.show();
    }
}
```

---

## 5. Same Example Using Lambda (Java 8)

```java
interface MyInterface {
    void show();
}

public class Test {
    public static void main(String[] args) {

        MyInterface obj = () -> {
            System.out.println("Hello World");
        };

        obj.show();
    }
}
```

Lambda reduces boilerplate code and improves readability.

---

## 6. Functional Interface  

### 6.1 What is a Functional Interface?  

A Functional Interface is an interface that contains **only one abstract method**.

It can have:
- One abstract method  
- Multiple default methods  
- Multiple static methods  

But only one abstract method.

Example:

```java
@FunctionalInterface
interface MyFunctionalInterface {
    void display();
}
```

The `@FunctionalInterface` annotation ensures that the interface has only one abstract method.

---

## 7. Built-in Functional Interfaces (java.util.function package)

Java 8 provides many predefined functional interfaces.

### 7.1 Predicate  

Represents a condition (returns boolean).

```java
import java.util.function.Predicate;

Predicate<Integer> checkEven = num -> num % 2 == 0;
System.out.println(checkEven.test(10));
```

---

### 7.2 Function  

Takes one input and returns one output.

```java
import java.util.function.Function;

Function<Integer, Integer> square = x -> x * x;
System.out.println(square.apply(5));
```

---

### 7.3 Consumer  

Takes input but returns nothing.

```java
import java.util.function.Consumer;

Consumer<String> print = str -> System.out.println(str);
print.accept("Hello");
```

---

### 7.4 Supplier  

Takes no input but returns a value.

```java
import java.util.function.Supplier;

Supplier<Double> randomValue = () -> Math.random();
System.out.println(randomValue.get());
```

---

## 8. Why Lambda is Important in Spring Boot  

- Used in Stream API  
- Used in event handling  
- Used in functional programming style  
- Reduces boilerplate code  
- Makes code more readable  

Modern Spring applications heavily depend on Java 8 features.

---

## 9. Viva Questions (Detailed Answers)

### 1. What is a Lambda Expression?  
A lambda expression is a short representation of a method that can be passed as an argument. It is used to implement functional interfaces.

### 2. What is a Functional Interface?  
A functional interface is an interface with exactly one abstract method. It can contain multiple default and static methods.

### 3. Why was Lambda introduced in Java 8?  
To reduce boilerplate code, enable functional programming style, and improve readability.

### 4. Can a functional interface have multiple methods?  
It can have multiple default and static methods, but only one abstract method.

### 5. Name some built-in functional interfaces.  
Predicate, Function, Consumer, Supplier.

---

## 10. MCQs (With Answers)

### 1. Lambda expressions were introduced in:  
A) Java 6  
B) Java 7  
C) Java 8  
D) Java 11  
Answer: C  

### 2. A functional interface contains:  
A) Multiple abstract methods  
B) One abstract method  
C) No methods  
D) Only static methods  
Answer: B  

### 3. Which functional interface returns boolean?  
A) Function  
B) Consumer  
C) Supplier  
D) Predicate  
Answer: D  

### 4. Which method is used in Predicate?  
A) apply()  
B) accept()  
C) test()  
D) get()  
Answer: C  

---

## 11. Interview Questions (Detailed Answers)

### 1. What problem does Lambda solve?  
It eliminates the need for anonymous inner classes when implementing single-method interfaces.

### 2. Difference between anonymous class and lambda?  
Anonymous class creates a new class instance. Lambda provides a concise way to represent functional interface implementation.

### 3. What is @FunctionalInterface annotation?  
It ensures that an interface contains exactly one abstract method at compile time.

### 4. How does Lambda improve performance?  
It reduces class creation overhead and improves readability, though performance gains depend on use case.

---

## 12. Lab Assignment  

1. Create a functional interface with one abstract method.  
2. Implement it using lambda expression.  
3. Write examples using Predicate, Function, Consumer, and Supplier.  
4. Create a program that checks whether a number is prime using Predicate.
