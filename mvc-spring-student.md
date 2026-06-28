# 🎓 Spring MVC Student Form Project

> A simple Spring MVC project that accepts student details using a JSP form and processes the submitted data using `@RequestParam`.

---

# 📌 Project Information

| Property | Value |
|----------|-------|
| Project Name | mvcspringStudent |
| Build Tool | Maven |
| Framework | Spring MVC |
| Java Version | Java 8 |
| Packaging | WAR |
| Server | Apache Tomcat 9 |
| View Technology | JSP |
| Configuration | XML Based |

---

# 📂 Project Structure

```text
mvcspringStudent
│
├── pom.xml
│
├── src
│   ├── main
│   │
│   ├── java
│   │   └── com
│   │       └── spring
│   │           └── student
│   │               └── Student.java
│   │
│   ├── resources
│   │
│   └── webapp
│       │
│       ├── WEB-INF
│       │   ├── index.jsp
│       │   └── Springservlet.xml
│       │
│       └── web.xml
│
├── src/test
│
└── target
```

---

# 🏗 Spring MVC Architecture

```text
Browser
   │
   ▼
DispatcherServlet
   │
   ▼
Springservlet.xml
   │
   ▼
Component Scan
   │
   ▼
Student Controller
   │
   ▼
@GetMapping("/")
   │
   ▼
index.jsp
   │
   ▼
Student Form
   │
   ▼
@PostMapping("/submit")
   │
   ▼
@RequestParam
   │
   ▼
Console Output
```

---

# 🔄 Request Flow

```text
Client Request
      │
      ▼
DispatcherServlet
      │
      ▼
Student Controller
      │
      ▼
Return View Name
      │
      ▼
View Resolver
      │
      ▼
/WEB-INF/index.jsp
      │
      ▼
User fills Form
      │
      ▼
POST /submit
      │
      ▼
@RequestParam
      │
      ▼
Controller Method
      │
      ▼
Print Student Details
```

---

# 📄 pom.xml

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">

  <modelVersion>4.0.0</modelVersion>

  <groupId>mvc</groupId>
  <artifactId>mvcspringStudent</artifactId>
  <version>0.0.1-SNAPSHOT</version>

  <packaging>war</packaging>

  <dependencies>

      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-webmvc</artifactId>
          <version>5.3.32</version>
      </dependency>

      <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>javax.servlet-api</artifactId>
          <version>4.0.1</version>
          <scope>provided</scope>
      </dependency>

      <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>jstl</artifactId>
          <version>1.2</version>
      </dependency>

  </dependencies>

  <build>
      <finalName>mvcspringStudent</finalName>
  </build>

</project>
```

---

# 📄 web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="
         http://xmlns.jcp.org/xml/ns/javaee
         http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">

    <servlet>

        <servlet-name>Spring</servlet-name>

        <servlet-class>
            org.springframework.web.servlet.DispatcherServlet
        </servlet-class>

        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>/WEB-INF/Springservlet.xml</param-value>
        </init-param>

        <load-on-startup>1</load-on-startup>

    </servlet>

    <servlet-mapping>

        <servlet-name>Spring</servlet-name>

        <url-pattern>/</url-pattern>

    </servlet-mapping>

</web-app>
```

---

# 📄 Springservlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

       xsi:schemaLocation="
       http://www.springframework.org/schema/beans
       https://www.springframework.org/schema/beans/spring-beans.xsd

       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd

       http://www.springframework.org/schema/mvc
       https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <context:component-scan base-package="com.spring.student"/>

    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">

        <property name="prefix" value="/WEB-INF/"/>
        <property name="suffix" value=".jsp"/>

    </bean>

    <mvc:annotation-driven/>

</beans>
```

---

# 📄 Student.java

```java
package com.spring.student;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller
public class Student {

    @GetMapping("/")
    public String studentform() {
        return "index";
    }

    @PostMapping("/submit")
    public void students(@RequestParam String name,
                         @RequestParam int age,
                         @RequestParam String email) {

        System.out.println(name);
        System.out.println(age);
        System.out.println(email);

    }
}
```

---

# 📄 index.jsp

```jsp
<%@ page language="java"
contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8"%>

<!DOCTYPE html>

<html>

<head>

<meta charset="UTF-8">

<title>Student Form</title>

</head>

<body>

<form action="submit" method="post">

Name :
<input name="name" type="text">

<br><br>

Age :
<input name="age" type="number">

<br><br>

Email :
<input name="email" type="email">

<br><br>

<button>Submit</button>

</form>

</body>

</html>
```

---

# ▶️ Running the Project

1. Import the project into Eclipse.
2. Right-click the project → **Maven → Update Project**.
3. Configure Apache Tomcat.
4. Run the project on the server.
5. Open:

```
http://localhost:8080/mvcspringStudent/
```

---

# 📸 Expected Form

```
------------------------------
Name  : ____________

Age   : ____________

Email : ____________

        [ Submit ]
------------------------------
```

---

# 💻 Console Output Example

```
Prasanna
21
prasanna@gmail.com
```

---

# 📚 Spring MVC Concepts Used

- Spring MVC
- Maven
- DispatcherServlet
- Component Scan
- InternalResourceViewResolver
- JSP
- @Controller
- @GetMapping
- @PostMapping
- @RequestParam
- Servlet Configuration
- WAR Deployment

---

# 🎤 Interview Questions

### 1. What is `@Controller`?
Marks the class as a Spring MVC Controller.

### 2. What is `@GetMapping`?
Maps HTTP GET requests to a controller method.

### 3. What is `@PostMapping`?
Maps HTTP POST requests, commonly used for form submission.

### 4. What is `@RequestParam`?
Reads request parameters from the submitted form and binds them to method arguments.

### 5. Why are JSP files kept inside `WEB-INF`?
They cannot be accessed directly by the browser and must be returned through a controller.

### 6. What is the purpose of `InternalResourceViewResolver`?
It converts the logical view name:

```
index
```

into:

```
/WEB-INF/index.jsp
```

---

# ✅ Learning Outcome

After completing this project, you will understand:

- Spring MVC XML Configuration
- DispatcherServlet
- JSP View Resolution
- Form Handling
- GET & POST Request Mapping
- Request Parameter Binding
- Maven Project Structure
- WAR Deployment on Tomcat
