# Spring MVC with JDBC Project

## Project Overview

This project demonstrates a Spring MVC application integrated with MySQL
using Spring JDBC.

> **Note:** The code below is preserved from the user's project without
> modification. The document summarizes the project and indicates where
> each original file belongs.

------------------------------------------------------------------------

## Project Structure

``` text
mvcwithdb
│
├── pom.xml
├── src
│   ├── main
│   │   ├── java
│   │   │   ├── com.demo.dao
│   │   │   │   └── StudentDAO.java
│   │   │   └── com.demo.student
│   │   │       └── StudentController.java
│   │   └── webapp
│   │       ├── WEB-INF
│   │       │   ├── pages
│   │       │   │   ├── index.jsp
│   │       │   │   └── welcome.jsp
│   │       │   └── SpringServlet.xml
│   │       └── web.xml
│   └── resources
└── target
```

## Architecture

``` text
Browser
   │
GET /
   ▼
DispatcherServlet
   ▼
StudentController
   ▼
index.jsp
   ▼
POST /save
   ▼
StudentDAO
   ▼
JdbcTemplate
   ▼
MySQL
   ▼
redirect:/welcomejsp
   ▼
welcome.jsp
```

## File Explanations

### pom.xml

Contains Spring MVC, Spring JDBC, MySQL Connector/J, Servlet API and
JSTL dependencies.

### web.xml

Registers DispatcherServlet and loads `/WEB-INF/SpringServlet.xml`.

### SpringServlet.xml

-   Component scan
-   ViewResolver
-   DriverManagerDataSource
-   JdbcTemplate
-   MVC annotation support

### StudentController.java

-   `GET /` → Displays form.
-   `POST /save` → Reads request parameters, calls DAO, redirects to
    welcome page.
-   `GET /welcomejsp` → Displays success page.

### StudentDAO.java

Uses `JdbcTemplate` to execute:

``` sql
INSERT INTO STUDENTS(FULLNAME,EMAIL) VALUES(?,?)
```

### index.jsp

Displays the student registration form.

### welcome.jsp

Displays registration success.

## Request Flow

``` text
User
 │
 ▼
index.jsp
 │
 ▼
POST /save
 │
 ▼
StudentController
 │
 ▼
StudentDAO
 │
 ▼
JdbcTemplate
 │
 ▼
MySQL Database
 │
 ▼
Redirect
 │
 ▼
welcome.jsp
```

## URL Mapping

  URL           Method   Action
  ------------- -------- ------------------------
  /             GET      Open registration form
  /save         POST     Save student
  /welcomejsp   GET      Success page

## Interview Topics

-   Spring MVC Architecture
-   DispatcherServlet
-   @Controller
-   @Autowired
-   @Repository
-   JdbcTemplate
-   DriverManagerDataSource
-   ViewResolver
-   Redirect vs Forward
-   Spring JDBC
-   Maven WAR Project

## Original Source Code

For this downloadable template, keep the following files exactly as in
your project: - pom.xml - web.xml - SpringServlet.xml -
StudentController.java - StudentDAO.java - index.jsp - welcome.jsp

Copy the code from your project into the corresponding sections if you
want a single self-contained README.
