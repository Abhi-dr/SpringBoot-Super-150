# SPRING BOOT – MODULE 1  
# PHASE 5  
## Java 8 – Optional, Default Methods and Method References  

---

## 1. Introduction  

So far in Java 8, we have studied:

- Lambda Expressions  
- Functional Interfaces  
- Stream API  

In this phase, we focus on:

- Optional Class  
- Default Methods in Interfaces  
- Static Methods in Interfaces  
- Method References (detailed)  

These concepts are frequently used in modern frameworks and production-level code.

---

## 2. Optional Class  

### 2.1 What is Optional?  

`Optional` is a container object introduced in Java 8 to handle null values safely.

It helps avoid `NullPointerException`.

Instead of returning null, methods can return an Optional object.

---

### 2.2 Why Optional is Important  

Before Java 8:

```java
String name = null;
System.out.println(name.length());   // NullPointerException
```

With Optional:

```java
Optional<String> name = Optional.ofNullable(null);
System.out.println(name.orElse("Default Name"));
```

Optional forces developers to handle null cases properly.

---

### 2.3 Creating Optional  

```java
Optional<String> opt1 = Optional.of("Hello");
Optional<String> opt2 = Optional.ofNullable(null);
Optional<String> opt3 = Optional.empty();
```

---

### 2.4 Important Methods of Optional  

- isPresent()  
- ifPresent()  
- orElse()  
- orElseGet()  
- orElseThrow()  
- get()  

---

### Example:

```java
Optional<String> name = Optional.ofNullable("Amit");

if(name.isPresent()) {
    System.out.println(name.get());
}
```

Better approach:

```java
name.ifPresent(System.out::println);
```

---

### orElse vs orElseGet  

```java
name.orElse("Default");
name.orElseGet(() -> "Generated Default");
```

`orElseGet()` is preferred when default value generation is expensive.

---

## 3. Default Methods in Interface  

### 3.1 Why Default Methods Were Introduced  

Before Java 8, adding a new method to an interface would break all implementing classes.

Java 8 introduced default methods to allow method implementation inside interfaces.

---

### 3.2 Example  

```java
interface Vehicle {

    void start();

    default void stop() {
        System.out.println("Vehicle Stopped");
    }
}
```

The implementing class does not need to override stop().

---

## 4. Static Methods in Interface  

Interfaces can also contain static methods.

```java
interface Calculator {

    static int add(int a, int b) {
        return a + b;
    }
}
```

Called using:

```java
Calculator.add(5, 10);
```

---

## 5. Method References (Detailed)

Method reference is a shorthand notation of lambda expression.

### Types of Method References:

1. Reference to static method  
2. Reference to instance method  
3. Reference to constructor  

---

### 5.1 Static Method Reference  

```java
Function<String, Integer> func = Integer::parseInt;
```

---

### 5.2 Instance Method Reference  

```java
List<String> names = Arrays.asList("Amit", "Raj");
names.forEach(System.out::println);
```

---

### 5.3 Constructor Reference  

```java
Supplier<ArrayList<String>> list = ArrayList::new;
```

---

## 6. Why These Concepts Matter in Spring  

- Optional is widely used in repository layers.  
- Default methods are used in interface-based programming.  
- Method references improve readability in Streams and Lambdas.  

These are heavily used in Spring Boot development.

---

## 7. Viva Questions (Detailed Answers)

### 1. What problem does Optional solve?  
Optional helps avoid NullPointerException by forcing developers to handle null cases explicitly.

### 2. Difference between orElse() and orElseGet()?  
orElse() always evaluates its argument.  
orElseGet() evaluates only when Optional is empty.

### 3. Why were default methods introduced?  
To allow adding new methods in interfaces without breaking existing implementations.

### 4. Can interfaces have static methods?  
Yes, from Java 8 onward, interfaces can contain static methods.

### 5. What are method references?  
Method references are shorthand versions of lambda expressions that refer directly to existing methods.

---

## 8. MCQs (With Answers)

### 1. Optional was introduced in:  
A) Java 6  
B) Java 7  
C) Java 8  
D) Java 11  
Answer: C  

### 2. Which method returns default value if Optional is empty?  
A) get()  
B) orElse()  
C) map()  
D) reduce()  
Answer: B  

### 3. Default methods are defined using:  
A) abstract  
B) static  
C) default  
D) final  
Answer: C  

### 4. Constructor reference uses:  
A) ->  
B) ::  
C) &&  
D) ||  
Answer: B  

---

## 9. Interview Questions (Detailed Answers)

### 1. Why is Optional not recommended for fields?  
Optional is designed primarily for return types, not for class fields or method parameters.

### 2. What happens if get() is called on empty Optional?  
It throws NoSuchElementException.

### 3. Can a class override default method?  
Yes, implementing class can override default method.

### 4. Difference between lambda and method reference?  
Lambda provides inline implementation.  
Method reference points directly to an existing method.

---

## 10. Lab Assignment  

1. Create a method that returns Optional<String>.  
2. Use orElse() and orElseGet() in examples.  
3. Create an interface with one default method.  
4. Demonstrate static method in interface.  
5. Use constructor reference to create objects.  
6. Convert a list of strings to uppercase using method reference.
