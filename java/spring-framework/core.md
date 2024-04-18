# Core

### Inversion of Control

For a very long time, I was not able to get a clear idea about these terms and always thought they were interchangeable.

IOC is a programming principle that advocates that an application benefits in terms of pluggability, testability, usability, and loose coupling if the application flow is transferred to a different part of the application (framework). This is the "inversion" aspect of the Inversion of Control design pattern. In layman's terms, the application is no longer in control of its resources or flow of execution instead Spring framework takes control of it. The way Spring framework does it is by using configuration either through `XML` or `java` and creating a `container`. This container is responsible for maintaining all the object creation and supplying them on demand. All object creation is strictly subjected to the rules of the spring framework and the application has no control over it. The `org.springframework.beans` and `org.springframework.context` packages are the basis for Spring Framework’s IoC container. The [`BeanFactory`](https://docs.spring.io/spring-framework/docs/6.1.4/javadoc-api/org/springframework/beans/factory/BeanFactory.html) interface provides an advanced configuration mechanism capable of managing any type of object. [`ApplicationContext`](https://docs.spring.io/spring-framework/docs/6.1.4/javadoc-api/org/springframework/context/ApplicationContext.html) is a sub-interface of `BeanFactory`.&#x20;

In spring the objects that form the backbone of your application and that are maintained by the spring IoC container are called beans.

### Container Overview

`ApplicationContext` interface represents the IoC container and is responsible for instantiating, configuring, and assembling the beans necessary for the application.

<figure><img src="../../.gitbook/assets/image (7).png" alt="" width="563"><figcaption></figcaption></figure>

Configuration metadata required for the IoC container is usually through XML or Java-based configuration.

There are many application-aware contexts available for example to develop standalone apps we can use `ClassPathXmlApplicationContext` that take `xml` files defined as input to create beans, for web applications we have `WebApplicationContext`&#x20;

The basic structure of XML-based configuration

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
		https://www.springframework.org/schema/beans/spring-beans.xsd">

	<bean id="..." class="...">
		<!-- collaborators and configuration for this bean go here -->
	</bean>

	<bean id="..." class="...">
		<!-- collaborators and configuration for this bean go here -->
	</bean>

	<!-- more bean definitions go here -->
</beans>
```

Lately, developers have preferred a Java-based configuration style. The container is initialized with this configuration data either by referring to the XML files or loading all `@Configuration` at the start of the spring application.

### Bean Overview

IoC container manages one or more beans, and beans are defined inside the container using `bean definition` which contain one or more configurations on how the bean should behave inside the container, for example: whether it needs to be created every time there is a request or return the same object, what is the life cycle of that bean, etc...

Beans can be instantiated in multiple ways through constructors, static factory method, and factory instance method.

```java
// Constructor
<bean id="exampleBean" class="examples.ExampleBean"/>

// Static Factory Method
<bean id="clientService" class="examples.ClientService" factory-method="createInstance"/>

public class ClientService {
	private static ClientService clientService = new ClientService();
	private ClientService() {}
	public static ClientService createInstance() {
		return clientService;
	}
}

// Factory Instance Method
<!-- the factory bean, which contains a method called createClientServiceInstance() -->
<bean id="serviceLocator" class="examples.DefaultServiceLocator"></bean>
<!-- the bean to be created via the factory bean -->
<bean id="clientService"
	factory-bean="serviceLocator"
	factory-method="createClientServiceInstance"/>
public class DefaultServiceLocator {
	private static ClientService clientService = new ClientServiceImpl();
	public ClientService createClientServiceInstance() {
		return clientService;
	}
}
```

#### Bean Definition

<table><thead><tr><th width="294">Property</th></tr></thead><tbody><tr><td>Class</td></tr><tr><td>Name</td></tr><tr><td>Scope</td></tr><tr><td>Constructor arguments</td></tr><tr><td>Properties</td></tr><tr><td>Autowire mode</td></tr><tr><td>Lazy initialization mode</td></tr><tr><td>Initialization method</td></tr><tr><td>Destruction method</td></tr></tbody></table>

### Dependency Injection

Dependency Injection is a design pattern that compliments the IoC principle, In dependency injection objects specify their dependencies (other objects to work with) only through constructor arguments, arguments to a factory method, or set through properties once the object is constructed or returned from a factory method.

#### Constructor based DI

As the same suggests, we use constructor arguments to inject dependencies while creating the object.

```java
package x.y;
public class ThingOne {
    public ThingOne(ThingTwo thingTwo, ThingThree thingThree) { }
}
```

```xml
<beans>
  <bean id="thingOne" class="x.y.ThingOne">
    <constructor-arg ref="x.y.ThingTwo" />
    <constructor-arg ref="x.y.ThingThree" />
  </bean>
  <bean id="thingTwo" class="x.y.ThingTwo" />
  <bean id="thingThree" class="x.y.ThingThree" />
</beans>
```

If the constructor arguments are not by `ref` but simply values, there are three ways it tries to resolve

```java
public class ThingOne {
    private String text;
    private int num;
    public ThingOne(String text, int num) {
        this.text = text;
        this.num = num;
    }
}
```

* By Type

```xml
<bean id="thingOne" class="x.y.ThingOne">
  <constructor-arg type="int" value="75" />
  <constructor-arg type="java.lang.String" value="hello" />
</bean>
```

* By Index

<pre class="language-xml"><code class="lang-xml"><strong>&#x3C;bean id="thingOne" class="x.y.ThingOne">
</strong>  &#x3C;constructor-arg index="1" value="75" />
  &#x3C;constructor-arg index="0" value="hello" />
&#x3C;/bean>
</code></pre>

* We could also use parameter names, however, this will not work unless the code is compiled in debug mode, or we should annotate our constructor with `@ConstructorProperties` annotation and should remove the declaration of the properties

```java
public class ThingOne {
	// Fields omitted
	@ConstructorProperties({"num", "text"})
	public ExampleBean(int num, String text) {
		this.num = num;
		this.text = text;
	}
}
```

```xml
<bean id="thingOne" class="x.y.ThingOne">
  <constructor-arg name="num" value="75" />
  <constructor-arg name="text" value="hello" />
</bean>
```

#### Setter based DI

