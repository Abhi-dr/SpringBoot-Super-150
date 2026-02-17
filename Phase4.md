# SPRING BOOT – MODULE 1  
# PHASE 4  
## Java 8 – Stream API  

---

## 1. Introduction  

In the previous phase, we learned about Lambda Expressions and Functional Interfaces.  

In this phase, we move to **Stream API**, one of the most powerful features introduced in Java 8.

Stream API allows processing collections of data in a functional style.  
It simplifies filtering, transforming, and aggregating data.

---

## 2. What is Stream API?  

Stream API is used to process collections of objects in a declarative way.

It does NOT store data.  
It processes data from a source such as:

- List  
- Set  
- Map  
- Arrays  

Streams allow operations like:

- Filtering  
- Sorting  
- Mapping  
- Counting  
- Collecting  

---

## 3. Why Stream API is Important  

Before Java 8, processing collections required loops.

Example (Before Java 8):

```java
List<Integer> list = Arrays.asList(1,2,3,4,5);
for(Integer num : list) {
    if(num % 2 == 0) {
        System.out.println(num);
    }
}
```

With Stream API:

```java
list.stream()
    .filter(num -> num % 2 == 0)
    .forEach(System.out::println);
```

Stream API:
- Reduces boilerplate code  
- Improves readability  
- Supports functional programming  
- Supports parallel processing  

---

## 4. Creating Streams  

### From Collection

```java
List<String> names = Arrays.asList("Amit", "Neha", "Raj");
Stream<String> stream = names.stream();
```

### From Array

```java
int[] arr = {1,2,3,4};
IntStream stream = Arrays.stream(arr);
```

---

## 5. Types of Stream Operations  

Stream operations are divided into:

1. Intermediate Operations  
2. Terminal Operations  

---

## 6. Intermediate Operations  

Intermediate operations return a new stream.

Examples:

- filter()  
- map()  
- sorted()  
- distinct()  
- limit()  

### Example – filter()

```java
List<Integer> numbers = Arrays.asList(1,2,3,4,5,6);

numbers.stream()
       .filter(n -> n % 2 == 0)
       .forEach(System.out::println);
```

---

### Example – map()

map() transforms elements.

```java
numbers.stream()
       .map(n -> n * n)
       .forEach(System.out::println);
```

---

## 7. Terminal Operations  

Terminal operations produce a result or side effect.

Examples:

- forEach()  
- collect()  
- count()  
- reduce()  

---

### Example – collect()

```java
List<Integer> evenList = numbers.stream()
        .filter(n -> n % 2 == 0)
        .collect(Collectors.toList());
```

---

### Example – count()

```java
long count = numbers.stream()
        .filter(n -> n > 3)
        .count();
```

---

### Example – reduce()

```java
int sum = numbers.stream()
        .reduce(0, (a, b) -> a + b);
```

---

## 8. Method Reference  

Method reference is a shorter way to write lambda expressions.

Instead of:

```java
numbers.forEach(n -> System.out.println(n));
```

We can write:

```java
numbers.forEach(System.out::println);
```

---

## 9. Parallel Stream  

Streams can run in parallel for better performance.

```java
numbers.parallelStream()
       .forEach(System.out::println);
```

Parallel streams divide tasks among multiple threads.

---

## 10. Viva Questions (Detailed Answers)

### 1. What is Stream API?  
Stream API is a feature introduced in Java 8 that allows processing of collections in a functional style using operations like filter, map, and reduce.

### 2. Difference between intermediate and terminal operations?  
Intermediate operations return a stream and are lazy. Terminal operations produce a result or side effect.

### 3. What is lazy evaluation?  
Intermediate operations are not executed until a terminal operation is called.

### 4. What is reduce()?  
reduce() combines elements of a stream into a single result, such as sum or multiplication.

### 5. What is parallel stream?  
Parallel stream processes elements using multiple threads to improve performance.

---

## 11. MCQs (With Answers)

### 1. Stream API was introduced in:  
A) Java 6  
B) Java 7  
C) Java 8  
D) Java 11  
Answer: C  

### 2. Which operation is terminal?  
A) filter()  
B) map()  
C) sorted()  
D) count()  
Answer: D  

### 3. Which method converts stream to list?  
A) filter()  
B) collect()  
C) reduce()  
D) count()  
Answer: B  

### 4. Which operation transforms elements?  
A) filter()  
B) map()  
C) count()  
D) limit()  
Answer: B  

---

## 12. Interview Questions (Detailed Answers)

### 1. Difference between Collection and Stream?  
Collection stores data, while Stream processes data. Stream does not store elements.

### 2. What is lazy evaluation in Stream API?  
Intermediate operations are executed only when a terminal operation is invoked.

### 3. When should parallel streams be used?  
When dealing with large datasets where operations are independent and can be processed concurrently.

### 4. What is the benefit of Stream API over loops?  
Stream API provides cleaner, more readable code and supports functional-style programming.

---

## 13. Lab Assignment  

1. Create a list of integers.  
2. Print only odd numbers using Stream.  
3. Find square of each number using map().  
4. Count numbers greater than 5.  
5. Find sum of all numbers using reduce().  
6. Convert a list of names to uppercase using Stream API.
