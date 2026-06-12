# Spring AOP Example Using @Before and @After Advice

## Introduction

Spring AOP (Aspect-Oriented Programming) helps separate cross-cutting concerns such as logging, security, auditing, transaction management, and exception handling from the main business logic.

Instead of writing logging code inside every method, Spring AOP allows us to write it once in an Aspect class and apply it automatically wherever needed.

In this example, we demonstrate:

* `@Before` Advice
* `@After` Advice
* Pointcut Expressions
* JoinPoint Usage

---

# Project Structure

```text
springAOPLoginExample
│
├── src
│   └── com.spring.aop
│       ├── LoginService.java
│       ├── LoggingAspect.java
│       └── Main.java
│
├── ApplicationContext.xml
│
└── lib
    ├── spring-context.jar
    ├── spring-aop.jar
    ├── spring-beans.jar
    ├── spring-core.jar
    ├── spring-expression.jar
    ├── aspectjrt.jar
    └── aspectjweaver.jar
```

---

# LoginService.java

```java
package com.spring.aop;

public class LoginService {

    public void login(String username) {
        System.out.println(username + " logged in successfully.");
    }
}
```

### Explanation

This is the target class.

The `login()` method contains the actual business logic.

```java
public void login(String username)
```

When this method is called, Spring AOP intercepts the execution and executes the configured advice methods.

---

# LoggingAspect.java

```java
package com.spring.aop;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;

@Aspect
public class LoggingAspect {

    @Before("execution(* com.spring.aop.LoginService.*(..))")
    public void beforeLogin(JoinPoint jp) {

        System.out.println("=== Before Advice ===");
        System.out.println("Method Name: " + jp.getSignature().getName());
        System.out.println("User is attempting to login...");
    }

    @After("execution(* com.spring.aop.LoginService.*(..))")
    public void afterLogin(JoinPoint jp) {

        System.out.println("=== After Advice ===");
        System.out.println("Method Name: " + jp.getSignature().getName());
        System.out.println("Login process completed.");
    }
}
```

---

# Explanation of @Aspect

```java
@Aspect
```

Marks the class as an Aspect.

An Aspect contains:

* Pointcuts
* Advice Methods

Spring scans this class and applies AOP functionality.

---

# Explanation of @Before

```java
@Before("execution(* com.spring.aop.LoginService.*(..))")
```

Runs before the target method executes.

### Meaning of Expression

```java
execution(* com.spring.aop.LoginService.*(..))
```

| Part         | Meaning                 |
| ------------ | ----------------------- |
| execution    | Match method execution  |
| *            | Any return type         |
| LoginService | Target class            |
| .*           | Any method name         |
| (..)         | Any number of arguments |

This means:

Execute the advice before any method inside LoginService.

---

# Explanation of JoinPoint

```java
JoinPoint jp
```

Provides information about the currently executing method.

Example:

```java
jp.getSignature().getName()
```

Returns:

```text
login
```

This allows us to log method details dynamically.

---

# Explanation of @After

```java
@After("execution(* com.spring.aop.LoginService.*(..))")
```

Runs after the target method finishes execution.

It behaves similarly to a Java `finally` block.

Whether the method:

* Executes successfully
* Throws an exception

The `@After` advice still executes.

---

# Main.java

```java
package com.spring.aop;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Main {

    public static void main(String[] args) {

        ApplicationContext context =
                new ClassPathXmlApplicationContext("ApplicationContext.xml");

        LoginService service =
                (LoginService) context.getBean("loginService");

        service.login("Prasanna");
    }
}
```

### Explanation

1. Spring Container starts.
2. Beans are loaded.
3. A proxy object is created for LoginService.
4. Method call goes through the proxy.
5. Proxy executes advice methods.
6. Actual method executes.

---

# ApplicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans
       https://www.springframework.org/schema/beans/spring-beans.xsd

       http://www.springframework.org/schema/aop
       https://www.springframework.org/schema/aop/spring-aop.xsd">

    <bean id="loginService"
          class="com.spring.aop.LoginService"/>

    <bean id="loggingAspect"
          class="com.spring.aop.LoggingAspect"/>

    <aop:aspectj-autoproxy/>

</beans>
```

### Explanation

#### loginService Bean

```xml
<bean id="loginService"
      class="com.spring.aop.LoginService"/>
```

Creates the target object.

#### loggingAspect Bean

```xml
<bean id="loggingAspect"
      class="com.spring.aop.LoggingAspect"/>
```

Creates the aspect object.

#### aspectj-autoproxy

```xml
<aop:aspectj-autoproxy/>
```

Enables Spring AOP.

Spring automatically creates proxy objects and applies advice methods.

---

# Execution Flow

```text
Client
  |
  v
@Before Advice
  |
  v
login()
  |
  v
@After Advice
```

---

# Output

```text
=== Before Advice ===
Method Name: login
User is attempting to login...

Prasanna logged in successfully.

=== After Advice ===
Method Name: login
Login process completed.
```

---

# Real-World Analogy

Consider an ATM transaction:

```text
Insert Card
     |
     v
@Before
Verify Card

     |
     v
Withdraw Money

     |
     v
@After
Print Receipt
```

The withdrawal operation is the business logic.

Card verification and receipt generation are cross-cutting concerns.

Spring AOP helps keep these concerns separate from the business code.

---

# Advantages of Spring AOP

1. Code Reusability
2. Better Separation of Concerns
3. Reduced Code Duplication
4. Easier Maintenance
5. Centralized Logging
6. Centralized Security
7. Simplified Transaction Management

---

# Common Real-World Uses

* Logging
* Authentication
* Authorization
* Auditing
* Transaction Management
* Exception Handling
* Performance Monitoring
* Caching

Spring AOP is widely used in enterprise applications to manage cross-cutting concerns without modifying the actual business logic.
