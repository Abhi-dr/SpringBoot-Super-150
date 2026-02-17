# SPRING BOOT – MODULE 1  
# PHASE 6  
## Java 8 – Date and Time API  

---

## 1. Introduction  

Before Java 8, date and time handling in Java was done using:

- `java.util.Date`  
- `java.util.Calendar`  
- `SimpleDateFormat`  

These classes had several problems:

- Mutable (not thread-safe)  
- Complex to use  
- Poor design  
- Confusing API  

Java 8 introduced a new Date and Time API under the `java.time` package.  
This API is:

- Immutable  
- Thread-safe  
- More readable  
- More powerful  

---

## 2. Important Classes in java.time Package  

- `LocalDate`  
- `LocalTime`  
- `LocalDateTime`  
- `ZonedDateTime`  
- `Period`  
- `Duration`  
- `DateTimeFormatter`  

---

## 3. LocalDate  

Represents only date (year, month, day).

Example:

```java
import java.time.LocalDate;

LocalDate today = LocalDate.now();
System.out.println(today);
```

Create specific date:

```java
LocalDate date = LocalDate.of(2024, 5, 20);
System.out.println(date);
```

Get values:

```java
System.out.println(date.getYear());
System.out.println(date.getMonth());
System.out.println(date.getDayOfMonth());
```

---

## 4. LocalTime  

Represents only time (hour, minute, second).

```java
import java.time.LocalTime;

LocalTime time = LocalTime.now();
System.out.println(time);
```

Create specific time:

```java
LocalTime customTime = LocalTime.of(10, 30, 45);
System.out.println(customTime);
```

---

## 5. LocalDateTime  

Represents both date and time.

```java
import java.time.LocalDateTime;

LocalDateTime dateTime = LocalDateTime.now();
System.out.println(dateTime);
```

---

## 6. ZonedDateTime  

Used when working with time zones.

```java
import java.time.ZonedDateTime;

ZonedDateTime zoned = ZonedDateTime.now();
System.out.println(zoned);
```

Specify zone:

```java
ZonedDateTime indiaTime =
ZonedDateTime.now(java.time.ZoneId.of("Asia/Kolkata"));
System.out.println(indiaTime);
```

---

## 7. Period  

Used to calculate difference between two dates.

```java
import java.time.Period;

LocalDate start = LocalDate.of(2020, 1, 1);
LocalDate end = LocalDate.now();

Period period = Period.between(start, end);

System.out.println(period.getYears());
System.out.println(period.getMonths());
System.out.println(period.getDays());
```

---

## 8. Duration  

Used to calculate time difference.

```java
import java.time.Duration;

LocalTime t1 = LocalTime.of(10, 0);
LocalTime t2 = LocalTime.of(12, 30);

Duration duration = Duration.between(t1, t2);

System.out.println(duration.toHours());
```

---

## 9. DateTimeFormatter  

Used for formatting date and time.

```java
import java.time.format.DateTimeFormatter;

LocalDate today = LocalDate.now();

DateTimeFormatter formatter =
DateTimeFormatter.ofPattern("dd-MM-yyyy");

String formattedDate = today.format(formatter);
System.out.println(formattedDate);
```

---

## 10. Why New Date API is Better  

- Immutable → safer in multi-threaded environments  
- Clear class separation (Date, Time, DateTime)  
- Better formatting options  
- Cleaner syntax  

This API is heavily used in enterprise applications including Spring Boot.

---

## 11. Viva Questions (Detailed Answers)

### 1. Why was new Date and Time API introduced?  
The old Date and Calendar classes were mutable, not thread-safe, and difficult to use. Java 8 introduced a cleaner, immutable, and thread-safe API.

### 2. Difference between LocalDate and LocalDateTime?  
LocalDate represents only date.  
LocalDateTime represents both date and time.

### 3. What is Period?  
Period calculates difference between two dates in years, months, and days.

### 4. What is Duration?  
Duration calculates time difference in hours, minutes, or seconds.

### 5. Is java.time API thread-safe?  
Yes, classes in java.time package are immutable and thread-safe.

---

## 12. MCQs (With Answers)

### 1. New Date API was introduced in:  
A) Java 6  
B) Java 7  
C) Java 8  
D) Java 11  
Answer: C  

### 2. Which class represents only date?  
A) LocalTime  
B) LocalDate  
C) ZonedDateTime  
D) Duration  
Answer: B  

### 3. Which class handles time zone?  
A) LocalDateTime  
B) LocalTime  
C) ZonedDateTime  
D) Period  
Answer: C  

### 4. Which class calculates date difference?  
A) Duration  
B) Period  
C) LocalDate  
D) ZoneId  
Answer: B  

---

## 13. Interview Questions (Detailed Answers)

### 1. Difference between Date and LocalDate?  
Date is mutable and not thread-safe. LocalDate is immutable and thread-safe.

### 2. Why is immutability important in Date API?  
Immutability ensures thread safety and avoids unexpected modifications.

### 3. How do you format date in Java 8?  
Using DateTimeFormatter with a pattern.

### 4. How to get current time in specific time zone?  
Using ZonedDateTime.now(ZoneId.of("ZoneName")).
