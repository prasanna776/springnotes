# Spring Framework - Constructor Injection Example

## Project Structure

```text
constructorInjectionUsingSpring
│
├── JRE System Library [JavaSE-21]
│
├── src
│   └── com.Spring.Practice
│       ├── Course.java
│       ├── Student.java
│       └── Main.java
│
├── lib
│   ├── commons-logging-1.2.jar
│   ├── spring-aop-5.3.39.jar
│   ├── spring-beans-5.3.39.jar
│   ├── spring-context-5.3.39.jar
│   ├── spring-core-5.3.39.jar
│   └── spring-expression-5.3.39.jar
│
├── ApplicationContext.xml
│
└── Referenced Libraries
    ├── commons-logging-1.2.jar
    ├── spring-aop-5.3.39.jar
    ├── spring-beans-5.3.39.jar
    ├── spring-context-5.3.39.jar
    ├── spring-core-5.3.39.jar
    └── spring-expression-5.3.39.jar
```

---

# 1. Introduction

This project demonstrates **Constructor Injection** using the Spring Framework.

In this example:

* Spring creates a `Course` object.
* Spring creates a `Student` object.
* Spring injects the `Course` object into the `Student` object through the constructor.
* The Spring IoC Container manages object creation and dependency injection.

---

# 2. Course.java

```java
package com.Spring.Practice;

public class Course {

    private String courseName;

    public Course(String courseName) {
        this.courseName = courseName;
    }

    public String getCourseName() {
        return courseName;
    }
}
```

---

# 3. Student.java

```java
package com.Spring.Practice;

public class Student {

    private int id;
    private String name;
    private Course course;

    public Student(int id, String name, Course course) {
        this.id = id;
        this.name = name;
        this.course = course;
    }

    public void display() {

        System.out.println("Student Id      : " + id);
        System.out.println("Student Name    : " + name);
        System.out.println("Course Name     : " + course.getCourseName());
    }
}
```

---

# 4. Main.java

```java
package com.Spring.Practice;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Main {

    public static void main(String[] args) {

        ApplicationContext context =
                new ClassPathXmlApplicationContext("ApplicationContext.xml");

        Student student =
                (Student) context.getBean("student");

        student.display();
    }
}
```

---

# 5. ApplicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans
       https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- Course Bean -->

    <bean id="course"
          class="com.Spring.Practice.Course">

        <constructor-arg value="Java Full Stack"/>

    </bean>

    <!-- Student Bean -->

    <bean id="student"
          class="com.Spring.Practice.Student">

        <constructor-arg value="101"/>
        <constructor-arg value="Prasanna"/>
        <constructor-arg ref="course"/>

    </bean>

</beans>
```

---

# 6. Output

```text
Student Id      : 101
Student Name    : Prasanna
Course Name     : Java Full Stack
```

---

# 7. How Constructor Injection Works

Spring first creates the Course object:

```xml
<bean id="course"
      class="com.Spring.Practice.Course">

    <constructor-arg value="Java Full Stack"/>

</bean>
```

Then Spring creates the Student object:

```xml
<bean id="student"
      class="com.Spring.Practice.Student">

    <constructor-arg value="101"/>
    <constructor-arg value="Prasanna"/>
    <constructor-arg ref="course"/>

</bean>
```

Internally Spring performs:

```java
Course course = new Course("Java Full Stack");

Student student =
        new Student(
                101,
                "Prasanna",
                course
        );
```

Since dependencies are supplied through the constructor while creating the object, this is called **Constructor Injection**.

---

# Key Difference

### Setter Injection

```java
student.setCourse(course);
```

Dependency is injected after object creation.

### Constructor Injection

```java
new Student(101, "Prasanna", course);
```

Dependency is injected during object creation.
