# Spring Framework Introduction

## What is Spring?

Spring is a lightweight, open-source Java framework used to develop enterprise-level applications. It provides infrastructure support for developing Java applications and makes development easier, faster, and more maintainable.

## Why Spring?

Before Spring, Java developers had to write a lot of boilerplate code and manually manage object creation and dependencies.

Spring solves these problems by providing:

- Dependency Injection (DI)
- Inversion of Control (IoC)
- Aspect-Oriented Programming (AOP)
- Transaction Management
- Security
- Integration with databases and web technologies

## Features of Spring

### 1. Lightweight
Spring is lightweight because it uses POJO (Plain Old Java Object) classes.

### 2. Dependency Injection (DI)
Spring injects required objects instead of creating them manually.

Example:
```java
Student s = new Student();
```

Spring can create and inject the object automatically.

### 3. Inversion of Control (IoC)
The Spring container manages object creation and lifecycle.

### 4. Aspect-Oriented Programming (AOP)
Used to separate cross-cutting concerns such as:
- Logging
- Security
- Transaction management

### 5. Modular Framework
Spring consists of multiple modules:
- Spring Core
- Spring Context
- Spring AOP
- Spring JDBC
- Spring ORM
- Spring MVC
- Spring Boot

## Architecture of Spring

```text
Application
      |
      v
Spring Container
      |
      v
Beans (Objects)
```

The Spring Container creates, configures, and manages bean objects.

## Spring Container

The Spring Container is the core of the Spring Framework.

Responsibilities:
- Creates objects (Beans)
- Injects dependencies
- Manages bean lifecycle

Examples:
- BeanFactory
- ApplicationContext

## What is a Bean?

A Bean is an object created and managed by the Spring Container.

Example:

```java
public class Student {
    public void display() {
        System.out.println("Student Bean");
    }
}
```

## Advantages of Spring

- Reduces code complexity
- Promotes loose coupling
- Easy testing
- Better maintainability
- Reusable code
- Enterprise application support

## Real-World Example

Without Spring:

```java
Engine engine = new Engine();
Car car = new Car(engine);
```

Developer creates objects manually.

With Spring:

```java
Car car = context.getBean(Car.class);
```

Spring creates and injects dependencies automatically.

## Summary

- Spring is a Java framework.
- It simplifies enterprise application development.
- Core concepts are IoC and Dependency Injection.
- Spring manages objects using the Spring Container.
- Spring promotes loose coupling and maintainable code.

## Interview Question

### What is Spring?

Spring is a lightweight, open-source Java framework that provides IoC and Dependency Injection features to simplify enterprise application development.

### What is IoC?

Inversion of Control is a principle where the Spring Container manages object creation and dependencies.

### What is Dependency Injection?

Dependency Injection is the process of providing required objects to a class instead of creating them inside the class.

### What is a Bean?

A Bean is an object created and managed by the Spring Container.
