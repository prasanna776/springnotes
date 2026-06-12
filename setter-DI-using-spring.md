# Spring Framework - Setter Injection Example

## Project Structure

```text
helloSpring
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

# Course.java

```java
package com.Spring.Practice;

public class Course {

    private String courseName;

    public Course() {
    }

    public String getCourseName() {
        return courseName;
    }

    public void setCourseName(String courseName) {
        this.courseName = courseName;
    }
}
```

---

# Student.java

```java
package com.Spring.Practice;

public class Student {

    private int id;
    private String name;
    private Course course;

    public Student() {
    }

    public void setId(int id) {
        this.id = id;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setCourse(Course course) {
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

# Main.java

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

# ApplicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans
       https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="course"
          class="com.Spring.Practice.Course">

        <property name="courseName"
                  value="Java Full Stack"/>
    </bean>

    <bean id="student"
          class="com.Spring.Practice.Student">

        <property name="id"
                  value="101"/>

        <property name="name"
                  value="Prasanna"/>

        <property name="course"
                  ref="course"/>
    </bean>

</beans>
```

---

# Output

```text
Student Id      : 101
Student Name    : Prasanna
Course Name     : Java Full Stack
```

---

# How Setter Injection Works

1. Spring creates the `course` bean.
2. Spring sets the `courseName` property.
3. Spring creates the `student` bean.
4. Spring injects `id` and `name`.
5. Spring injects the `course` object into `student`.

```xml
<property name="course" ref="course"/>
```

Spring internally calls:

```java
student.setCourse(course);
```

This is called **Setter Injection** because the dependency is injected through the setter method.
