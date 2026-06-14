# Spring Framework - Autowiring Example

---

# 1. What is Autowiring?

Autowiring is a feature of the Spring IoC Container that automatically injects dependent beans into another bean.

Instead of manually specifying dependencies using `<property>` or `<constructor-arg>`, Spring automatically finds and injects the required bean.

---

# 2. Without Autowiring

In this example, a `Car` object depends on an `Engine` object.

The dependency must be configured manually in the XML file.

## Project Structure

```text
springAutowiring

├── src
│   └── com.spring.autowire
│       ├── Engine.java
│       ├── Car.java
│       └── Main.java
│
├── ApplicationContext.xml
│
└── lib
    ├── spring-core.jar
    ├── spring-beans.jar
    ├── spring-context.jar
    └── commons-logging.jar
```

---

## Engine.java

```java
package com.spring.autowire;

public class Engine {

    public void start() {
        System.out.println("Engine Started...");
    }
}
```

---

## Car.java

```java
package com.spring.autowire;

public class Car {

    private Engine engine;

    public void setEngine(Engine engine) {
        this.engine = engine;
    }

    public void drive() {
        engine.start();
        System.out.println("Car is Running...");
    }
}
```

---

## ApplicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans
       https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="engine"
          class="com.spring.autowire.Engine"/>

    <bean id="car"
          class="com.spring.autowire.Car">

        <property name="engine"
                  ref="engine"/>

    </bean>

</beans>
```

---

## Main.java

```java
package com.spring.autowire;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Main {

    public static void main(String[] args) {

        ApplicationContext context =
                new ClassPathXmlApplicationContext("ApplicationContext.xml");

        Car car = (Car) context.getBean("car");

        car.drive();
    }
}
```

---

## Output

```text
Engine Started...
Car is Running...
```

---

# 3. With Autowiring

Now Spring automatically injects the Engine bean into Car.

No need to write:

```xml
<property name="engine" ref="engine"/>
```

---

## Engine.java

```java
package com.spring.autowire;

public class Engine {

    public void start() {
        System.out.println("Engine Started...");
    }
}
```

---

## Car.java

```java
package com.spring.autowire;

public class Car {

    private Engine engine;

    public void setEngine(Engine engine) {
        this.engine = engine;
    }

    public void drive() {
        engine.start();
        System.out.println("Car is Running...");
    }
}
```

---

## ApplicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans
       https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="engine"
          class="com.spring.autowire.Engine"/>

    <bean id="car"
          class="com.spring.autowire.Car"
          autowire="byType"/>

</beans>
```

---

## Main.java

```java
package com.spring.autowire;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Main {

    public static void main(String[] args) {

        ApplicationContext context =
                new ClassPathXmlApplicationContext("ApplicationContext.xml");

        Car car = (Car) context.getBean("car");

        car.drive();
    }
}
```

---

## Output

```text
Engine Started...
Car is Running...
```

---

# 4. Types of Autowiring

Spring supports four autowiring modes.

---

## 1. byName

Spring searches for a bean whose id matches the property name.

### Example

Property:

```java
private Engine engine;
```

Bean:

```xml
<bean id="engine"
      class="com.spring.autowire.Engine"/>

<bean id="car"
      class="com.spring.autowire.Car"
      autowire="byName"/>
```

Since bean id = engine and property name = engine, Spring injects the bean.

---

## 2. byType

Spring searches for a bean whose type matches the property type.

```xml
<bean id="engineBean"
      class="com.spring.autowire.Engine"/>

<bean id="car"
      class="com.spring.autowire.Car"
      autowire="byType"/>
```

Spring checks the type `Engine` and injects it.

---

## 3. constructor

Spring injects dependencies through the constructor.

### Car.java

```java
package com.spring.autowire;

public class Car {

    private Engine engine;

    public Car(Engine engine) {
        this.engine = engine;
    }

    public void drive() {
        engine.start();
        System.out.println("Car is Running...");
    }
}
```

### XML

```xml
<bean id="engine"
      class="com.spring.autowire.Engine"/>

<bean id="car"
      class="com.spring.autowire.Car"
      autowire="constructor"/>
```

---

## 4. no

Default mode.

```xml
<bean id="car"
      class="com.spring.autowire.Car"
      autowire="no"/>
```

Dependencies must be injected manually.

---

# 5. Annotation-Based Autowiring

Modern Spring applications commonly use `@Autowired`.

---

## Engine.java

```java
package com.spring.autowire;

import org.springframework.stereotype.Component;

@Component
public class Engine {

    public void start() {
        System.out.println("Engine Started...");
    }
}
```

---

## Car.java

```java
package com.spring.autowire;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class Car {

    @Autowired
    private Engine engine;

    public void drive() {
        engine.start();
        System.out.println("Car is Running...");
    }
}
```

---

## Main.java

```java
package com.spring.autowire;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main {

    public static void main(String[] args) {

        ApplicationContext context =
                new AnnotationConfigApplicationContext("com.spring.autowire");

        Car car = context.getBean(Car.class);

        car.drive();
    }
}
```

---

## Output

```text
Engine Started...
Car is Running...
```

---

# 6. Advantages of Autowiring

- Reduces XML configuration
- Less coding
- Automatic dependency injection
- Easier maintenance
- Better readability
- Faster development

---

# 7. Disadvantages of Autowiring

- Less control over dependency injection
- Ambiguity when multiple beans of the same type exist
- Can be difficult to debug in large projects

---

# 8. Comparison

| Without Autowiring | With Autowiring |
|-------------------|----------------|
| Manual dependency injection | Automatic dependency injection |
| More XML configuration | Less XML configuration |
| More code | Less code |
| Easy to understand dependencies | Dependencies may be hidden |
| Suitable for small projects | Suitable for medium and large projects |

---

# Interview Definition

**Autowiring is a Spring Framework feature that automatically injects one bean into another bean by matching bean name, bean type, or constructor dependency, reducing the need for explicit configuration.**
