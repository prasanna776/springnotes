# Spring MVC with Database (JDBC) — Complete Project Guide

A Spring MVC web application that accepts student registration from a form and saves it to a MySQL database using `JdbcTemplate`.

---

## 📁 Project Structure

```
mvcwithdb/
├── src/
│   └── main/
│       ├── java/
│       │   ├── com.demo.dao/
│       │   │   └── StudentDAO.java
│       │   └── com.demo.student/
│       │       └── StudentController.java
│       └── webapp/
│           └── WEB-INF/
│               ├── pages/
│               │   ├── index.jsp
│               │   └── welcome.jsp
│               ├── SpringServlet.xml
│               └── web.xml
├── target/
└── pom.xml
```

---

## 🔧 Tech Stack

| Layer | Technology |
|-------|------------|
| Web Framework | Spring MVC 5.3.32 |
| Database Access | Spring JDBC (JdbcTemplate) |
| Database | MySQL 8 |
| View | JSP + JSTL |
| Server | Apache Tomcat (WAR) |
| Build Tool | Maven |

---

## 📄 File-by-File Explanation

### 1. `pom.xml` — Maven Dependencies

Defines all the libraries needed for the project.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>mvc</groupId>
  <artifactId>mvcwithdb</artifactId>
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
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.3.32</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.33</version>
        </dependency>

        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>

    </dependencies>

    <build>
        <finalName>mvcwithdb</finalName>
    </build>

</project>
```

**Key dependencies explained:**

- `spring-webmvc` — Core Spring MVC framework (DispatcherServlet, Controllers, View Resolvers)
- `javax.servlet-api` — Servlet API (scope `provided` means Tomcat already has it at runtime)
- `spring-jdbc` — Provides `JdbcTemplate` for database operations
- `mysql-connector-java` — JDBC driver to connect Java with MySQL
- `jstl` — JSP Standard Tag Library for rendering dynamic HTML in JSP pages

---

### 2. `web.xml` — Front Controller Setup

Registers the Spring `DispatcherServlet` as the single entry point for all HTTP requests.

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
        <param-value>/WEB-INF/SpringServlet.xml</param-value>
    </init-param>
        

        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>Spring</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

</web-app>
```

**How it works:**

- `DispatcherServlet` is the **Front Controller** — it intercepts every HTTP request (`/`)
- `contextConfigLocation` tells Spring where to find the Spring configuration file (`SpringServlet.xml`)
- `load-on-startup=1` ensures the servlet is initialized when the server starts, not on the first request

---

### 3. `SpringServlet.xml` — Spring Application Context

Configures beans: component scanning, view resolver, data source, and JdbcTemplate.

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

<context:component-scan base-package="com.demo.dao,com.demo.student"/>
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">

        <property name="prefix" value="/WEB-INF/pages/"/>
        <property name="suffix" value=".jsp"/>

    </bean>
    
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
       <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
       <property name="url" value="jdbc:mysql://localhost:3306/mvcdb"/>
       <property name="username" value="root"/>
       <property name="password" value="Prasanna@2007"/>
    </bean>
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate"  autowire="byType">
  
    </bean>
    

    <mvc:annotation-driven/>

</beans>
```

**Bean-by-bean explanation:**

| Bean | Purpose |
|------|---------|
| `component-scan` | Scans `com.demo.dao` and `com.demo.student` packages to auto-detect `@Controller`, `@Repository`, etc. |
| `InternalResourceViewResolver` | Maps view names like `"index"` → `/WEB-INF/pages/index.jsp` |
| `dataSource` | Configures the MySQL connection (URL, credentials, driver) |
| `jdbcTemplate` | Spring's database helper — `autowire="byType"` auto-injects the `dataSource` bean |
| `mvc:annotation-driven` | Enables `@RequestMapping`, `@GetMapping`, `@PostMapping` etc. |

---

### 4. `StudentDAO.java` — Data Access Layer

Handles all database operations. Uses `JdbcTemplate` to run SQL queries.

```java
package com.demo.dao;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Repository;

@Repository 

public class StudentDAO 
{
		@Autowired
		private JdbcTemplate jdbcTemplate;
		
		public void saveStudent(String fullname,String email) 
		{
			System.out.println("SaveStudent Called");
			String sql = "INSERT INTO STUDENTS(FULLNAME,EMAIL) VALUES(?,?)";
			
			jdbcTemplate.update(sql,fullname,email);
			System.out.println("Data Inserted Successfully");

		}
		
		
		
	}
```

**Key points:**

- `@Repository` — Marks this class as a DAO (Data Access Object); Spring auto-detects and registers it as a bean
- `@Autowired JdbcTemplate` — Spring injects the `jdbcTemplate` bean configured in `SpringServlet.xml`
- `jdbcTemplate.update(sql, fullname, email)` — Executes an `INSERT` statement safely using parameterized queries (prevents SQL injection)

---

### 5. `StudentController.java` — Web Layer (Controller)

Handles HTTP requests, calls the DAO, and returns views.

```java
package com.demo.student;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;

import com.demo.dao.*;

 

 

@Controller
public class StudentController {
	
	@Autowired
	private StudentDAO StudentDao;

    // @GetMapping("/") — handles GET request to root URL
    // When browser opens http://localhost:8080/SpringMVCApp/
    // This method runs and returns "index" view
    @GetMapping("/")
    public String home() {
        return "index";
        // View Resolver converts "index" to /WEB-INF/pages/index.jsp
    }

    // @PostMapping("/save") — handles POST request to /save URL
    // When user submits form with action="save" this method runs
    @PostMapping("/save")
    public String save(
            @RequestParam String fullname,
            @RequestParam String email) 	
    {

        System.out.println("Full Name: " + fullname);
        System.out.println("Email: " + email);
        StudentDao.saveStudent(fullname,email);
        return "redirect:/welcomejsp";
    }

    // @GetMapping("/welcome") — handles GET request to /welcome
    @GetMapping("/welcomejsp")
    public String welcome() {
        return "welcome";
        // 	WEB-INF/pages/welcome.jsp
    }
}
```

**Method-by-method breakdown:**

| Method | Annotation | URL | What it does |
|--------|-----------|-----|-------------|
| `home()` | `@GetMapping("/")` | `GET /` | Returns `index.jsp` (shows the registration form) |
| `save()` | `@PostMapping("/save")` | `POST /save` | Reads form data, saves to DB, redirects to welcome page |
| `welcome()` | `@GetMapping("/welcomejsp")` | `GET /welcomejsp` | Returns `welcome.jsp` (success message) |

- `@RequestParam` extracts form field values (`fullname`, `email`) from the POST request body
- `return "redirect:/welcomejsp"` prevents form re-submission on browser refresh (Post/Redirect/Get pattern)

---

### 6. `index.jsp` — Student Registration Form

```jsp
<%@ page language="java"
    contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Student Regestration</title>
</head>
<body>
 <h2> Student Regestration from</h2>
 <form action="save" method="post">
    Full Name: 
    <input type="text" name="fullname" required>
    Email: 
    <input type="email" name="email" required>
    <br>
    <br>
    <button type="submit">Save Student</button>
    
 
 </form>
    
</body>
</html>
```

**Key points:**

- `action="save"` — Form submits a `POST` request to `/mvcwithdb/save`
- `name="fullname"` and `name="email"` — These names must exactly match the `@RequestParam` names in `StudentController`
- `required` attribute provides basic client-side validation

---

### 7. `welcome.jsp` — Success Page

```jsp
<%@ page language="java"
    contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Student Regestration</title>
</head>
<body>
 <h2> Student Regestration Successfull</h2>
 <a href="/mvcwithdb/">add Another Student</a>
 
    
</body>
</html>
```

Shown after a successful database insert. The link navigates back to the registration form.

---

## 🔄 Request Flow Diagram

```
Browser
  │
  ▼
GET /               →  DispatcherServlet  →  StudentController.home()  →  index.jsp (form)
  │
  │ (User fills form and clicks Submit)
  ▼
POST /save          →  DispatcherServlet  →  StudentController.save()
                                                    │
                                                    ▼
                                              StudentDAO.saveStudent()
                                                    │
                                                    ▼
                                              MySQL Database (INSERT)
                                                    │
                                                    ▼
                                          redirect:/welcomejsp
                                                    │
                                                    ▼
GET /welcomejsp     →  DispatcherServlet  →  StudentController.welcome()  →  welcome.jsp
```

---

## 🗄️ Database Setup

Create the database and table in MySQL before running the app:

```sql
CREATE DATABASE mvcdb;

USE mvcdb;

CREATE TABLE STUDENTS (
    ID INT AUTO_INCREMENT PRIMARY KEY,
    FULLNAME VARCHAR(100),
    EMAIL VARCHAR(100)
);
```

---

## ▶️ How to Run

1. Clone or import the project into Eclipse/IntelliJ as a **Maven project**
2. Set up MySQL and run the SQL script above
3. Update credentials in `SpringServlet.xml` if needed
4. Deploy the WAR to Apache Tomcat
5. Open browser: `http://localhost:8080/mvcwithdb/`

---

## 💡 Key Concepts Used

| Concept | Where Used |
|--------|-----------|
| Front Controller Pattern | `DispatcherServlet` in `web.xml` |
| Dependency Injection | `@Autowired` in Controller and DAO |
| Component Scanning | `<context:component-scan>` in `SpringServlet.xml` |
| View Resolution | `InternalResourceViewResolver` maps names to JSP paths |
| JdbcTemplate | Simplifies JDBC boilerplate in `StudentDAO` |
| Post/Redirect/Get | `return "redirect:/welcomejsp"` prevents duplicate form submissions |
