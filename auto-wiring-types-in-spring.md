# Spring Framework – Autowiring Complete Guide

---

# 1. What is Autowiring?

Autowiring is a feature of the Spring IoC (Inversion of Control) Container that automatically injects dependent beans into another bean.

Instead of manually specifying dependencies using:

```xml
<property>
<constructor-arg>
```

Spring automatically finds and injects the required bean.

---

# Real Life Example

Consider:

```text
Car → depends on → Engine
```

A Car cannot run without an Engine.

Similarly:

```text
Bean A → depends on → Bean B
```

Spring automatically injects Bean B into Bean A.

---

# 2. Without Autowiring (Manual Injection)

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
<bean id="engine"
      class="com.spring.autowire.Engine"/>

<bean id="car"
      class="com.spring.autowire.Car">

    <property name="engine"
              ref="engine"/>

</bean>
```

---

# Internal Working

### Step 1

Spring creates Engine Bean.

```java
new Engine();
```

### Step 2

Spring creates Car Bean.

```java
new Car();
```

### Step 3

Reads:

```xml
<property name="engine"
          ref="engine"/>
```

### Step 4

Finds Engine bean.

### Step 5

Calls:

```java
car.setEngine(engineBean);
```

using Reflection.

### Output

```text
Engine Started...
Car is Running...
```

---

# 3. With Autowiring

No need to specify:

```xml
<property name="engine"
          ref="engine"/>
```

Spring injects automatically.

```xml
<bean id="engine"
      class="com.spring.autowire.Engine"/>

<bean id="car"
      class="com.spring.autowire.Car"
      autowire="byType"/>
```

---

# Internal Flow

```text
Create Engine Bean
        |
Create Car Bean
        |
Check Autowire Mode
        |
Find Dependency
        |
Inject Dependency
        |
Ready Bean
```

---

# 4. Types of Autowiring

Spring supports:

1. no
2. byName
3. byType
4. constructor
5. autodetect (Deprecated)

---

# Type 1: no

Default mode.

No automatic dependency injection.

```xml
<bean id="car"
      class="com.spring.autowire.Car"
      autowire="no"/>
```

Dependency must be configured manually.

```xml
<property name="engine"
          ref="engine"/>
```

---

## Internal Working

```text
Create Engine Bean
      |
Create Car Bean
      |
Read <property>
      |
Find Bean
      |
Call Setter
```

---

## Advantages

✔ Full control

✔ Easy debugging

---

## Disadvantages

✘ More XML

✘ More configuration

---

# Type 2: byName

Spring matches:

```text
Property Name == Bean ID
```

---

## Car.java

```java
private Engine engine;
```

Property Name:

```text
engine
```

---

## XML

```xml
<bean id="engine"
      class="com.spring.autowire.Engine"/>

<bean id="car"
      class="com.spring.autowire.Car"
      autowire="byName"/>
```

---

## Internal Working

### Step 1

Spring creates Car object.

### Step 2

Uses Reflection.

Finds:

```java
private Engine engine;
```

Property name:

```text
engine
```

### Step 3

Searches BeanFactory.

```java
containsBean("engine")
```

### Step 4

Bean found.

### Step 5

Calls:

```java
setEngine(engineBean);
```

---

## Flow

```text
Property Name
      |
      V
   engine
      |
Search Bean ID
      |
      V
   engine
      |
Inject
```

---

# Type 3: byType

Spring matches:

```text
Property Type == Bean Type
```

---

## XML

```xml
<bean id="engineBean"
      class="com.spring.autowire.Engine"/>

<bean id="car"
      class="com.spring.autowire.Car"
      autowire="byType"/>
```

---

## Internal Working

### Step 1

Find property.

```java
private Engine engine;
```

### Step 2

Determine type.

```text
Engine
```

### Step 3

Search all beans of type Engine.

```java
getBeansOfType(Engine.class)
```

### Step 4

Inject matching bean.

---

## Flow

```text
Property Type
      |
      V
    Engine
      |
Search Beans
      |
      V
Inject Bean
```

---

## Problem

```xml
<bean id="e1"
      class="Engine"/>

<bean id="e2"
      class="Engine"/>
```

Spring finds:

```text
2 Beans Found
```

Exception:

```text
NoUniqueBeanDefinitionException
```

---

# Type 4: constructor

Injection happens through constructor.

---

## Car.java

```java
public class Car {

    private Engine engine;

    public Car(Engine engine) {
        this.engine = engine;
    }
}
```

---

## XML

```xml
<bean id="engine"
      class="com.spring.autowire.Engine"/>

<bean id="car"
      class="com.spring.autowire.Car"
      autowire="constructor"/>
```

---

## Internal Working

### Step 1

Find constructor.

```java
Car(Engine engine)
```

### Step 2

Determine parameter type.

```text
Engine
```

### Step 3

Search matching bean.

### Step 4

Instantiate:

```java
new Car(engineBean);
```

---

## Flow

```text
Find Constructor
        |
Analyze Parameters
        |
Find Matching Bean
        |
Pass To Constructor
        |
Object Created
```

---

## Advantages

✔ Dependencies mandatory

✔ Better testing

✔ Immutable objects

✔ Most used today

---

# Type 5: autodetect (Deprecated)

Old Spring feature.

```xml
autowire="autodetect"
```

Spring automatically chooses:

```text
constructor
OR
byType
```

---

## Internal Logic

```text
Constructor Exists?
        |
      Yes
        |
Use Constructor
```

Otherwise:

```text
Use byType
```

---

## Why Deprecated?

✘ Unpredictable

✘ Hard to debug

✘ Spring had to guess

Removed from modern Spring.

---

# 5. Modern Annotation-Based Autowiring

Today we use:

```java
@Autowired
```

---

## Engine.java

```java
@Component
public class Engine {

}
```

---

## Car.java

```java
@Component
public class Car {

    @Autowired
    private Engine engine;
}
```

---

# Internal Working of @Autowired

### Step 1

Spring creates Car Bean.

### Step 2

AutowiredAnnotationBeanPostProcessor scans class.

### Step 3

Finds:

```java
@Autowired
private Engine engine;
```

### Step 4

Determines type.

```text
Engine
```

### Step 5

Searches BeanFactory.

### Step 6

Injects using Reflection.

```java
field.set(carObject, engineBean);
```

---

# Internal Classes Responsible

```text
BeanFactory
DefaultListableBeanFactory
BeanDefinition
DependencyDescriptor
ReflectionUtils
AutowiredAnnotationBeanPostProcessor
```

---

# Advantages of Autowiring

✔ Less XML

✔ Less coding

✔ Faster development

✔ Automatic dependency injection

✔ Easier maintenance

✔ Better readability

---

# Disadvantages of Autowiring

✘ Hidden dependencies

✘ Ambiguity with multiple beans

✘ Debugging can be harder

---

# Comparison

| Mode        | Matching Rule                 | Injection Type       |
| ----------- | ----------------------------- | -------------------- |
| no          | Manual                        | Setter / Constructor |
| byName      | Property Name = Bean ID       | Setter               |
| byType      | Property Type = Bean Type     | Setter               |
| constructor | Constructor Parameter Type    | Constructor          |
| autodetect  | Chooses constructor or byType | Automatic            |

---

# Interview Definition

Autowiring is a Spring Framework feature that automatically injects one bean into another bean by matching bean name, bean type, or constructor dependency, reducing the need for explicit configuration and simplifying dependency management.
