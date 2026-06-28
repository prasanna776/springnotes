# 🌱 Spring MVC First Project

A simple Spring MVC project using:

- Java 8
- Spring MVC 5.3.32
- Maven
- JSP
- Apache Tomcat
- JSTL

---

# 📁 Project Structure

```text
mvcspringfirst/
│
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── spring/
│   │   │           └── demo/
│   │   │               └── Helloworld.java
│   │   │
│   │   ├── resources/
│   │   │
│   │   └── webapp/
│   │       ├── index.jsp
│   │       ├── Springservlet.xml
│   │       └── web.xml
│   │
│   └── test/
│
├── target/
│
└── pom.xml
```

---

# 📄 pom.xml

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"

    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <groupId>mvc</groupId>

    <artifactId>mvcspringfirst</artifactId>

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

        <finalName>mvcspringfirst</finalName>

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

            <param-value>/Springservlet.xml</param-value>

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

    <context:component-scan
        base-package="com.spring.demo" />

    <bean
        class="org.springframework.web.servlet.view.InternalResourceViewResolver">

        <property name="prefix" value="/" />

        <property name="suffix" value=".jsp" />

    </bean>

    <mvc:annotation-driven />

</beans>
```

---

# 📄 index.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<!DOCTYPE html>

<html>

<head>

<meta charset="UTF-8">

<title>Insert title here</title>

</head>

<body>

hello world

</body>

</html>
```

---

# 📄 Helloworld.java

```java
package com.spring.demo;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class Helloworld
{
    // local8080:/mvcspringfirst/

    @GetMapping("/")
    public String hello()
    {
        return "index";
    }
}
```

---

# ▶️ Run the Project

1. Import the project into Eclipse.
2. Update Maven Project.
3. Configure Apache Tomcat.
4. Run the project on the server.
5. Open the browser.

```
http://localhost:8080/mvcspringfirst/
```

---

# ✅ Output

```
hello world
```

---

# 📚 Technologies Used

- Java 8
- Spring MVC 5.3.32
- Maven
- JSP
- JSTL
- Apache Tomcat
