# Spring AOP - With and Without AOP Example

## Introduction

AOP (Aspect-Oriented Programming) helps separate cross-cutting concerns such as logging, security, auditing, and transaction management from business logic.

To understand why AOP is useful, let's compare a Login System implemented:

1. Without AOP
2. With AOP

---

# Scenario

Suppose we want to log every login operation.

Before login:

```text
User is attempting to login...
```

After login:

```text
Login completed.
```

---

# Without AOP

In a normal Java application, logging code is written directly inside business methods.

## LoginService.java

```java
package com.spring.withoutaop;

public class LoginService {

    public void login(String username) {

        // Logging Code
        System.out.println("User is attempting to login...");

        // Business Logic
        System.out.println(username + " logged in successfully.");

        // Logging Code
        System.out.println("Login completed.");
    }
}
```

---

## Main.java

```java
package com.spring.withoutaop;

public class Main {

    public static void main(String[] args) {

        LoginService service = new LoginService();

        service.login("Prasanna");
    }
}
```

---

## Output

```text
User is attempting to login...

Prasanna logged in successfully.

Login completed.
```

---

# Problems Without AOP

Suppose there are many methods:

```java
login()
logout()
register()
changePassword()
forgotPassword()
```

We must write:

```java
System.out.println("Logging...");
```

inside every method.

Example:

```java
public void login() {
    System.out.println("Logging...");
}

public void logout() {
    System.out.println("Logging...");
}

public void register() {
    System.out.println("Logging...");
}
```

### Issues

* Code Duplication
* Difficult Maintenance
* Business Logic mixed with Logging Logic
* Poor Separation of Concerns

---

# With Spring AOP

Using AOP, logging code is moved into a separate Aspect class.

Business class contains only business logic.

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

Notice:

No logging code is present.

Only business logic exists.

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
        System.out.println("Method Name: " +
                jp.getSignature().getName());

        System.out.println("User is attempting to login...");
    }

    @After("execution(* com.spring.aop.LoginService.*(..))")
    public void afterLogin(JoinPoint jp) {

        System.out.println("=== After Advice ===");
        System.out.println("Method Name: " +
                jp.getSignature().getName());

        System.out.println("Login completed.");
    }
}
```

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

---

# Main.java

```java
package com.spring.aop;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Main {

    public static void main(String[] args) {

        ApplicationContext context =
                new ClassPathXmlApplicationContext(
                        "ApplicationContext.xml");

        LoginService service =
                (LoginService) context.getBean("loginService");

        service.login("Prasanna");
    }
}
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
Login completed.
```

---

# Execution Flow Without AOP

```text
Client
   |
   v
login()
   |
   +--> Logging
   |
   +--> Business Logic
   |
   +--> Logging
```

Everything is inside the same method.

---

# Execution Flow With AOP

```text
Client
   |
   v
Spring Proxy
   |
   +--> @Before Advice
   |
   +--> login()
   |
   +--> @After Advice
```

Spring automatically injects logging behavior.

---

# Comparison

| Without AOP                       | With AOP                  |
| --------------------------------- | ------------------------- |
| Logging code inside every method  | Logging code written once |
| Code duplication                  | Reusable                  |
| Difficult maintenance             | Easy maintenance          |
| Business logic mixed with logging | Clean separation          |
| Hard to manage large applications | Easy to manage            |

---

# Real-World Example

Imagine a Bank Application.

Methods:

```java
deposit()
withdraw()
transfer()
checkBalance()
```

Without AOP:

```java
Logging
Security Check
Transaction Code
```

must be written in every method.

With AOP:

```java
SecurityAspect
LoggingAspect
TransactionAspect
```

handle these concerns automatically.

Business methods remain clean and focused only on business operations.

---

# Conclusion

Without AOP:

```text
Business Logic + Logging + Security + Auditing
```

are mixed together.

With AOP:

```text
Business Logic
       +
Aspect Classes
```

are separated.

This makes enterprise applications cleaner, reusable, maintainable, and easier to develop.
