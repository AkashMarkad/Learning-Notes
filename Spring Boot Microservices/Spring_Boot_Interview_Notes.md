# Spring Boot Interview Notes — Detailed Guide

> **Purpose:** Interview-ready notes for Java/Spring Boot backend roles. The examples use modern Spring Boot style (`@SpringBootApplication`, `SecurityFilterChain`, constructor injection, Spring Data JPA repositories, etc.).
>
> **Core mental model:** Spring Boot makes Spring application setup convenient. Spring Framework provides the IoC container, dependency injection, MVC, transaction support, and core infrastructure. Spring Data JPA simplifies repository-based data access, while Hibernate is commonly used as the JPA implementation.

---

## Table of Contents

1. [Actuator and Health Endpoint](#1-actuator-and-health-endpoint)
2. [How a Spring Boot Application Starts](#2-how-a-spring-boot-application-starts)
3. [Spring Bean and Bean Lifecycle](#3-spring-bean-and-bean-lifecycle)
4. [Dependency Injection — All Important Types](#4-dependency-injection--all-important-types)
5. [Spring MVC](#5-spring-mvc)
6. [Spring Data JPA](#6-spring-data-jpa)
7. [Hibernate](#7-hibernate)
8. [JPQL](#8-jpql)
9. [Spring Security](#9-spring-security)
10. [Profiles in Spring Boot](#10-profiles-in-spring-boot)
11. [Exception Handling in Spring Boot](#11-exception-handling-in-spring-boot)
12. [Lazy Loading](#12-lazy-loading)
13. [JSONObject and JSON Handling](#13-jsonobject-and-json-handling)
14. [How These Concepts Fit Together](#14-how-these-concepts-fit-together)
15. [Common Interview Questions and Answers](#15-common-interview-questions-and-answers)
16. [Quick Revision Sheet](#16-quick-revision-sheet)

---

# 1. Actuator and Health Endpoint

## 1.1 What is Spring Boot Actuator?

**Spring Boot Actuator** provides production-oriented endpoints and features for monitoring and managing a Spring Boot application.

It can expose information such as:

- Application health
- Application information
- Metrics
- Environment/configuration information
- Beans
- Mappings
- Logging configuration
- Application startup information
- Thread and JVM information

Actuator is especially useful in production, Kubernetes deployments, monitoring systems, and microservices.

### Why do we need Actuator?

Imagine a microservice deployed on AWS/Kubernetes. The service may be running as a process, but that does not automatically mean the application is healthy.

For example:

```text
Application process -> running
        |
        +--> Database -> DOWN
        +--> Redis -> DOWN
        +--> Kafka -> UP
```

A monitoring system needs a machine-readable way to determine application health. Actuator provides that capability.

Spring Boot's official documentation describes Actuator as providing production-ready features for monitoring and managing applications through HTTP endpoints or JMX, including health and metrics.

---

## 1.2 Add Actuator dependency

For Maven:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

After adding it, restart the application.

---

## 1.3 Actuator endpoints

A common endpoint is:

```text
GET /actuator/health
```

Other commonly used endpoints include:

```text
/actuator/info
/actuator/metrics
/actuator/loggers
/actuator/mappings
/actuator/beans
/actuator/env
/actuator/threaddump
/actuator/startup
```

Which endpoints are exposed over HTTP is configurable. Do not expose sensitive endpoints blindly in production.

Example configuration:

```properties
management.endpoints.web.exposure.include=health,info,metrics
```

YAML:

```yaml
management:
  endpoints:
    web:
      exposure:
        include: health,info,metrics
```

> **Interview point:** Adding the Actuator dependency does not mean every endpoint should be publicly exposed. Security and endpoint exposure should be configured intentionally.

---

## 1.4 Health endpoint

The standard health endpoint is:

```text
GET /actuator/health
```

A basic response may look like:

```json
{
  "status": "UP"
}
```

The official endpoint documentation defines `/actuator/health` as the endpoint for retrieving application health, including an overall status and, depending on configuration, component details.

When health details are enabled, you may see something conceptually like:

```json
{
  "status": "UP",
  "components": {
    "db": {
      "status": "UP",
      "details": {
        "database": "PostgreSQL"
      }
    },
    "diskSpace": {
      "status": "UP"
    }
  }
}
```

---

## 1.5 Health status values

Common statuses include:

```text
UP
DOWN
OUT_OF_SERVICE
UNKNOWN
```

A service may be considered unhealthy when an important dependency such as a database is unavailable.

---

## 1.6 Show health details

Example configuration:

```properties
management.endpoint.health.show-details=when-authorized
```

Possible policies depend on the Spring Boot version/configuration, but conceptually you should choose whether health details are always visible, conditionally visible, or hidden.

### Why should health details be protected?

Suppose the response exposes:

```json
{
  "database": "company-prod-db.internal",
  "redis": "redis-prod.internal"
}
```

That can reveal infrastructure information to unauthorized users.

---

## 1.7 Custom HealthIndicator

You can create a custom health check.

```java
@Component
public class PaymentGatewayHealthIndicator implements HealthIndicator {

    @Override
    public Health health() {
        boolean available = checkPaymentGateway();

        if (available) {
            return Health.up()
                    .withDetail("provider", "Payment Gateway")
                    .build();
        }

        return Health.down()
                .withDetail("provider", "Payment Gateway")
                .build();
    }

    private boolean checkPaymentGateway() {
        // Call/check the dependency here.
        return true;
    }
}
```

Then the health endpoint can incorporate the custom indicator.

---

## 1.8 Readiness and liveness

In cloud/container environments, two important concepts are:

### Liveness

Question:

> Is this application instance fundamentally alive, or should the platform restart it?

### Readiness

Question:

> Is this application currently ready to receive traffic?

Spring Boot also has application availability support for common liveness and readiness states, including integration with Actuator health groups.

### Interview example

Suppose your service starts but database initialization has not completed.

```text
Process alive?        YES
Ready for traffic?    NO
```

That means **liveness may be UP while readiness is not yet READY**.

---

## 1.9 Actuator startup endpoint

The startup endpoint can expose information about the application startup sequence:

```text
GET /actuator/startup
```

This can help diagnose slow startup and identify expensive initialization steps.

---

## 1.10 Actuator interview questions

### Q1. What is Actuator?

Actuator provides production-ready monitoring and management features through endpoints such as health, metrics, info, loggers, and startup.

### Q2. What is `/actuator/health` used for?

It provides the health status of the application and can include health information for dependencies such as the database.

### Q3. Should Actuator endpoints be publicly exposed?

No. Expose only required endpoints and secure sensitive endpoints, especially in production.

### Q4. What is the difference between liveness and readiness?

Liveness determines whether an application instance is alive/recoverable by restart; readiness determines whether it is ready to receive traffic.

---

# 2. How a Spring Boot Application Starts

This is a very common interview question.

Consider:

```java
@SpringBootApplication
public class MyApplication {

    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```

The important thing to understand is that:

```java
SpringApplication.run(...)
```

creates and refreshes a Spring `ApplicationContext`, processes configuration, discovers beans, performs dependency injection, starts the embedded web server when applicable, and publishes lifecycle events.

---

## 2.1 What does `@SpringBootApplication` mean?

`@SpringBootApplication` is a composed annotation. Conceptually, it combines:

```java
@Configuration
@EnableAutoConfiguration
@ComponentScan
```

Therefore:

### `@Configuration`

Indicates that the class can contain bean definitions.

### `@EnableAutoConfiguration`

Tells Spring Boot to configure components based on the classpath, properties, and available beans.

### `@ComponentScan`

Scans packages for components such as:

```java
@Component
@Service
@Repository
@Controller
@RestController
```

Spring Boot recommends placing the application class in a suitable root package so component scanning naturally covers application components.

---

## 2.2 High-level startup flow

Think of startup as:

```text
main()
  |
  v
SpringApplication.run()
  |
  v
Create/prepare application environment
  |
  v
Determine application type
  |
  v
Create ApplicationContext
  |
  v
Load configuration and auto-configuration
  |
  v
Component scan + register bean definitions
  |
  v
Refresh ApplicationContext
  |
  v
Create singleton beans
  |
  v
Dependency Injection
  |
  v
BeanPostProcessors / initialization callbacks
  |
  v
Start embedded server (web app)
  |
  v
Publish startup/ready events
  |
  v
Application is ready
```

This is a simplified interview-level flow; actual startup involves many internal classes, conditions, post-processors, listeners, and application events.

---

## 2.3 Environment and configuration

Spring Boot loads configuration from supported property/YAML sources and prepares the `Environment`.

For example:

```properties
server.port=8081
spring.datasource.url=jdbc:postgresql://localhost:5432/company
```

These properties can influence auto-configuration.

---

## 2.4 Auto-configuration

Suppose your classpath contains Spring MVC and an embedded server.

Spring Boot can automatically configure web infrastructure.

Suppose JPA and a database driver are present:

```text
spring-boot-starter-data-jpa
postgresql driver
```

Boot can configure pieces such as:

```text
DataSource
EntityManagerFactory
Transaction manager
Spring Data repository infrastructure
```

But auto-configuration is conditional. It backs off when application-defined beans/configuration should take precedence.

---

## 2.5 Component scan

Spring scans the package tree for components.

Example:

```text
com.example
   |
   +-- MyApplication.java
   +-- controller
   |     +-- UserController.java
   +-- service
   |     +-- UserService.java
   +-- repository
         +-- UserRepository.java
```

If `MyApplication` is in `com.example`, component scanning can discover these classes.

---

## 2.6 ApplicationContext refresh

The `ApplicationContext` is the central Spring container that manages beans and their dependencies.

Spring's IoC documentation describes `ApplicationContext` as an extended container that adds features such as AOP integration, event publication, and application-layer context support.

During context refresh, Spring performs a large amount of initialization work, including bean creation, post-processing, and lifecycle callbacks.

---

## 2.7 Embedded web server

For a web application, Boot can start an embedded servlet container.

Conceptually:

```text
Spring Boot application
       |
       +---- DispatcherServlet
       |
       +---- Embedded Servlet Server
```

The developer does not have to package the application as a traditional WAR and deploy it to an externally managed server for normal Spring Boot usage.

---

## 2.8 Application ready

When startup completes, Spring Boot publishes lifecycle events and the application becomes ready to service requests.

You can react to startup events:

```java
@Component
public class StartupListener {

    @EventListener(ApplicationReadyEvent.class)
    public void onApplicationReady() {
        System.out.println("Application is ready");
    }
}
```

---

## 2.9 Interview answer — concise version

> `SpringApplication.run()` bootstraps the application. Spring Boot prepares the environment, creates the `ApplicationContext`, applies auto-configuration, scans and registers beans, creates and injects dependencies, runs bean lifecycle callbacks, starts the embedded web server for web applications, and finally publishes lifecycle/readiness events.

---

# 3. Spring Bean and Bean Lifecycle

## 3.1 What is a Spring Bean?

A **Spring Bean** is an object that is instantiated, assembled, and managed by the Spring IoC container.

For example:

```java
@Service
public class UserService {

    public String getUser() {
        return "Akash";
    }
}
```

`UserService` becomes a Spring bean because Spring discovers it through component scanning.

The official Spring documentation defines beans as objects instantiated, assembled, and managed by the Spring IoC container.

---

## 3.2 Why use Spring Beans?

Without Spring:

```java
UserRepository repository = new UserRepository();
UserService service = new UserService(repository);
```

Your code manually controls object creation.

With Spring:

```java
@Service
public class UserService {

    private final UserRepository repository;

    public UserService(UserRepository repository) {
        this.repository = repository;
    }
}
```

Spring manages object creation and dependency wiring.

---

## 3.3 Bean lifecycle

A useful interview-level sequence is:

```text
1. Bean definition discovered/registered
2. Bean instance created
3. Dependencies injected
4. Aware callbacks (when applicable)
5. BeanPostProcessor before initialization
6. @PostConstruct / initialization callback
7. InitializingBean.afterPropertiesSet()
8. Custom init method
9. BeanPostProcessor after initialization
10. Bean is ready for use
11. Application shuts down
12. @PreDestroy / destruction callbacks
```

Not every bean goes through every callback.

---

## 3.4 Example

```java
@Component
public class PaymentService {

    @PostConstruct
    public void init() {
        System.out.println("Bean initialized");
    }

    @PreDestroy
    public void destroy() {
        System.out.println("Bean destroyed");
    }
}
```

Imports:

```java
import jakarta.annotation.PostConstruct;
import jakarta.annotation.PreDestroy;
```

---

## 3.5 `BeanPostProcessor`

Spring can intercept bean initialization using `BeanPostProcessor`.

```java
@Component
public class MyBeanPostProcessor implements BeanPostProcessor {

    @Override
    public Object postProcessBeforeInitialization(
            Object bean, String beanName) {
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(
            Object bean, String beanName) {
        return bean;
    }
}
```

This mechanism is important because Spring itself uses post-processors heavily for framework features.

Examples of infrastructure implemented using post-processing include dependency injection mechanisms and proxy creation.

---

## 3.6 Singleton scope

By default, Spring beans are singleton scoped in a given `ApplicationContext`.

```java
@Component
public class MyService {
}
```

Normally, Spring maintains one bean instance for that context.

Important:

> **Spring singleton != Java `Singleton Design Pattern`.**

Spring controls the lifecycle and scope within the container. It is not necessarily a JVM-wide single instance across all contexts.

---

## 3.7 Other common scopes

```text
singleton
prototype
request
session
application
websocket
```

For example:

```java
@Scope("prototype")
@Component
public class ReportGenerator {
}
```

With prototype scope, the container can create a new instance whenever the bean is requested.

---

## 3.8 Important interview question

### Does Spring create a bean first or inject dependencies first?

Conceptually, the bean instance must be created before field/setter dependencies can be populated. Constructor dependencies are required at creation time because the constructor needs them.

---

# 4. Dependency Injection — All Important Types

## 4.1 What is Dependency Injection?

Dependency Injection (DI) is a design technique in which an object receives the dependencies it needs rather than creating or locating them itself.

Spring describes DI as a specialized form of IoC where dependencies are supplied through constructors, factory method arguments, or properties after object creation. citeturn261818search0turn261818search10

### Without DI

```java
public class OrderService {

    private PaymentService paymentService = new PaymentService();
}
```

Problems:

- Tight coupling
- Difficult unit testing
- Hard to replace implementations
- Object creation is mixed with business logic

### With DI

```java
public class OrderService {

    private final PaymentService paymentService;

    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

Now the dependency is injected from outside.

---

## 4.2 Constructor injection — recommended

```java
@Service
public class OrderService {

    private final PaymentService paymentService;

    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

With a single constructor, modern Spring can inject it without requiring `@Autowired` on the constructor.

Spring Boot documentation generally recommends constructor injection.

### Advantages

- Dependencies are mandatory and explicit
- Supports `final` fields
- Easier unit testing
- Better immutability
- Fails fast if required dependencies are missing

Unit test:

```java
PaymentService paymentService = mock(PaymentService.class);
OrderService service = new OrderService(paymentService);
```

No Spring context is required for this basic unit-test setup.

---

## 4.3 Field injection

```java
@Service
public class OrderService {

    @Autowired
    private PaymentService paymentService;
}
```

### Problems with field injection

- Dependencies are hidden inside the class declaration.
- Fields cannot naturally be `final`.
- Plain unit testing becomes less convenient.
- It encourages mutable dependencies.

It exists and works, but constructor injection is usually preferred.

---

## 4.4 Setter injection

```java
@Service
public class OrderService {

    private PaymentService paymentService;

    @Autowired
    public void setPaymentService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

Setter injection is useful when a dependency is optional or can legitimately be changed after construction.

---

## 4.5 Method injection

A Spring bean can also receive dependencies through methods.

For example:

```java
@Autowired
public void configure(PaymentService paymentService) {
    // dependency injected through method
}
```

This is less common than constructor injection in typical Spring Boot applications.

---

## 4.6 `@Autowired` resolution process

Suppose:

```java
public interface PaymentProcessor {
    void pay();
}
```

Two implementations:

```java
@Component
public class StripePaymentProcessor implements PaymentProcessor {
    @Override
    public void pay() {
    }
}
```

```java
@Component
public class RazorpayPaymentProcessor implements PaymentProcessor {
    @Override
    public void pay() {
    }
}
```

Now:

```java
@Service
public class PaymentService {

    private final PaymentProcessor processor;

    public PaymentService(PaymentProcessor processor) {
        this.processor = processor;
    }
}
```

Spring sees multiple candidates.

You can resolve ambiguity using `@Primary`:

```java
@Primary
@Component
public class StripePaymentProcessor implements PaymentProcessor {
}
```

Or `@Qualifier`:

```java
public PaymentService(
        @Qualifier("razorpayPaymentProcessor")
        PaymentProcessor processor) {
    this.processor = processor;
}
```

---

## 4.7 `@Qualifier`

`@Qualifier` tells Spring which candidate should be injected.

```java
@Component("stripe")
public class StripePaymentProcessor implements PaymentProcessor {
}
```

```java
@Component("razorpay")
public class RazorpayPaymentProcessor implements PaymentProcessor {
}
```

Then:

```java
public PaymentService(
        @Qualifier("stripe") PaymentProcessor processor) {
    this.processor = processor;
}
```

---

## 4.8 DI with `@Bean`

Not every bean has to be declared with `@Component`.

```java
@Configuration
public class AppConfig {

    @Bean
    public ObjectMapper objectMapper() {
        return new ObjectMapper();
    }
}
```

Now another bean can inject it:

```java
@Service
public class JsonService {

    private final ObjectMapper objectMapper;

    public JsonService(ObjectMapper objectMapper) {
        this.objectMapper = objectMapper;
    }
}
```

---

## 4.9 What is IoC?

**Inversion of Control** is the broader concept.

Normally:

```text
Application code controls object creation
```

With Spring:

```text
Spring container controls bean creation and dependency wiring
```

**DI is one of the main techniques used to implement IoC.**

---

# 5. Spring MVC

## 5.1 What is Spring MVC?

Spring MVC is Spring's servlet-based web framework for building web applications and REST APIs.

MVC means:

```text
Model
View
Controller
```

In a REST API, you often focus primarily on the Controller/Service/Repository layers, while JSON is used as the representation returned to clients.

---

## 5.2 Basic architecture

A common application architecture is:

```text
Client
  |
  | HTTP Request
  v
Controller
  |
  v
Service
  |
  v
Repository
  |
  v
Database
```

The response travels back in the opposite direction.

---

## 5.3 Example controller

```java
@RestController
@RequestMapping("/users")
public class UserController {

    private final UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping("/{id}")
    public UserResponse getUser(@PathVariable Long id) {
        return userService.getUser(id);
    }
}
```

---

## 5.4 `@Controller` vs `@RestController`

### `@Controller`

Used for MVC controllers, often with views.

```java
@Controller
public class HomeController {

    @GetMapping("/home")
    public String home() {
        return "home";
    }
}
```

The returned value can represent a view name.

### `@RestController`

Equivalent conceptually to:

```java
@Controller
@ResponseBody
```

It is generally used for REST APIs.

```java
@RestController
public class UserController {

    @GetMapping("/users")
    public List<User> users() {
        return ...;
    }
}
```

The response is serialized into JSON by the HTTP message conversion infrastructure.

---

## 5.5 Request flow in Spring MVC

A simplified flow:

```text
HTTP Request
    |
    v
Servlet Container
    |
    v
Spring Security Filters (when configured)
    |
    v
DispatcherServlet
    |
    v
HandlerMapping
    |
    v
Controller method selected
    |
    v
Argument resolution / validation / conversion
    |
    v
Service
    |
    v
Repository
    |
    v
Controller returns object
    |
    v
HttpMessageConverter / JSON serialization
    |
    v
HTTP Response
```

---

## 5.6 DispatcherServlet

`DispatcherServlet` is the central front controller of Spring MVC.

Its responsibility includes coordinating request processing with MVC infrastructure.

It does not mean that every piece of application logic happens inside `DispatcherServlet`. It delegates work to components such as handler mappings, handler adapters, argument resolvers, and message converters.

---

## 5.7 `@PathVariable`

```java
@GetMapping("/users/{id}")
public UserResponse getUser(@PathVariable Long id) {
    return userService.getUser(id);
}
```

Request:

```text
GET /users/101
```

Then:

```text
id = 101
```

---

## 5.8 `@RequestParam`

```java
@GetMapping("/users")
public List<UserResponse> search(
        @RequestParam String city) {
    return userService.findByCity(city);
}
```

Request:

```text
GET /users?city=Pune
```

---

## 5.9 `@RequestBody`

```java
@PostMapping("/users")
public UserResponse createUser(
        @RequestBody CreateUserRequest request) {
    return userService.create(request);
}
```

JSON request:

```json
{
  "name": "Akash",
  "email": "akash@example.com"
}
```

A JSON converter/deserializer turns the JSON into the Java object.

---

## 5.10 Validation

DTO:

```java
public class CreateUserRequest {

    @NotBlank
    private String name;

    @Email
    @NotBlank
    private String email;

    // getters/setters
}
```

Controller:

```java
@PostMapping("/users")
public UserResponse createUser(
        @Valid @RequestBody CreateUserRequest request) {
    return userService.create(request);
}
```

Validation failures can then be handled centrally using exception handling.

---

# 6. Spring Data JPA

## 6.1 What is Spring Data JPA?

Spring Data JPA is a Spring Data module that simplifies implementation of data access layers using JPA.

Instead of writing a large amount of repetitive CRUD code, you can define repository interfaces and let Spring Data generate the implementation.

---

## 6.2 JPA vs Spring Data JPA vs Hibernate

This is one of the most important interview distinctions.

```text
JPA
 |
 | specification/API
 v
Hibernate
 |
 | implementation
 v
Database

Spring Data JPA
 |
 | repository abstraction on top of JPA
 v
JPA / EntityManager
```

### JPA

JPA is a **specification** for ORM in Java/Jakarta ecosystems.

It defines concepts such as:

- `@Entity`
- `@Id`
- `EntityManager`
- JPQL
- Persistence context

### Hibernate

Hibernate is an ORM framework and a common JPA implementation.

### Spring Data JPA

Spring Data JPA reduces boilerplate around repository/data-access code.

---

## 6.3 Entity example

```java
@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String name;

    @Column(nullable = false, unique = true)
    private String email;

    // getters/setters
}
```

---

## 6.4 Repository

```java
public interface UserRepository
        extends JpaRepository<User, Long> {
}
```

You automatically get methods such as:

```java
save(user)
findById(id)
findAll()
deleteById(id)
count()
existsById(id)
```

This is one of the biggest productivity benefits of Spring Data JPA.

---

## 6.5 Derived query methods

Suppose we need users by email:

```java
Optional<User> findByEmail(String email);
```

By city:

```java
List<User> findByCity(String city);
```

By city and age:

```java
List<User> findByCityAndAge(String city, Integer age);
```

The method name describes the query.

---

## 6.6 `@Query`

You can define custom JPQL:

```java
@Query("select u from User u where u.email = :email")
Optional<User> findUserByEmail(
        @Param("email") String email);
```

---

## 6.7 Pagination

```java
Page<User> findByCity(String city, Pageable pageable);
```

Usage:

```java
Pageable pageable = PageRequest.of(0, 20);
Page<User> page = userRepository.findByCity("Pune", pageable);
```

This is useful for large datasets.

---

## 6.8 `save()` behavior

A common interview trap is:

> Does `save()` always execute an SQL INSERT?

No.

The actual behavior depends on whether the entity is considered new and on the persistence context/JPA implementation.

Conceptually:

```text
New entity   -> persist
Existing     -> merge/appropriate update handling
```

Do not assume every `save()` means exactly one SQL statement at that line.

---

## 6.9 Transactions

For business operations involving multiple database changes:

```java
@Service
public class OrderService {

    @Transactional
    public void placeOrder(OrderRequest request) {
        // create order
        // update inventory
        // create payment record
    }
}
```

The transaction gives the operation a unit-of-work boundary.

Typical ACID expectations:

- Atomicity
- Consistency
- Isolation
- Durability

---

## 6.10 Persistence context

A **persistence context** is a first-level managed context where entity instances are tracked.

For example:

```java
User user = entityManager.find(User.class, 10L);
user.setName("New Name");
```

Within the transaction/persistence context, JPA can detect the change and synchronize it with the database at flush/transaction completion.

This is called **dirty checking**.

---

# 7. Hibernate

## 7.1 What is Hibernate?

Hibernate is an ORM framework that maps Java objects to relational database tables.

ORM = **Object-Relational Mapping**.

Without ORM, you may write:

```java
Connection connection = ...;
PreparedStatement ps = ...;
ResultSet rs = ...;
```

and manually map rows to Java objects.

With Hibernate/JPA:

```java
User user = entityManager.find(User.class, id);
```

The ORM handles much of the repetitive mapping work.

---

## 7.2 Object-relational mapping example

Java:

```java
@Entity
public class Employee {

    @Id
    private Long id;

    private String name;
}
```

Database:

```text
employee
----------------
id       BIGINT
name     VARCHAR
```

Hibernate maps:

```text
Employee.id   <-> employee.id
Employee.name <-> employee.name
```

---

## 7.3 Important Hibernate concepts

### Session / EntityManager

JPA code generally interacts through `EntityManager`, while Hibernate exposes its native concepts such as `Session`.

### Persistence Context

Tracks managed entities.

### First-Level Cache

Associated with the persistence context/session.

### Second-Level Cache

Optional and shared across sessions when configured.

### Dirty Checking

Hibernate/JPA detects changes to managed entities and synchronizes them to the database.

### Lazy Loading

Associations can be loaded when accessed rather than immediately.

### Proxy / enhancement mechanisms

Hibernate can use proxies or bytecode enhancement mechanisms to defer loading.

---

## 7.4 Entity states

Interviewers often ask this.

A simplified lifecycle is:

```text
Transient
   |
   | persist()
   v
Managed
   |
   | detach()
   v
Detached
   |
   | merge()
   v
Managed

Managed -- remove() --> Removed
```

### Transient

Object created with `new`, not managed by the persistence context.

### Managed

Entity is tracked by the persistence context.

### Detached

Was managed, but is no longer attached to the current persistence context.

### Removed

Marked for deletion.

---

## 7.5 First-level cache

Example:

```java
User u1 = entityManager.find(User.class, 1L);
User u2 = entityManager.find(User.class, 1L);
```

Within the same persistence context, JPA can return the same managed entity instance rather than issuing another database lookup for the same identity.

This behavior is often described as the first-level cache.

---

## 7.6 Dirty checking

```java
@Transactional
public void updateUser(Long id) {
    User user = entityManager.find(User.class, id);
    user.setName("New Name");
}
```

You do not necessarily need:

```java
entityManager.update(user);
```

At transaction flush/commit, the ORM can detect the state change and generate an SQL update.

---

# 8. JPQL

## 8.1 What is JPQL?

**JPQL = Java Persistence Query Language**.

JPQL queries **entities and their fields**, not database tables and raw column names.

Example:

```sql
SELECT u FROM User u WHERE u.email = :email
```

Notice:

```text
User       -> entity class name
u.email    -> entity property
```

not:

```sql
SELECT * FROM users WHERE email = ?
```

The ORM provider converts JPQL into SQL appropriate for the database.

---

## 8.2 Entity-based querying

Entity:

```java
@Entity
public class User {
    private Long id;
    private String name;
    private String email;
}
```

JPQL:

```java
@Query("select u from User u where u.name = :name")
List<User> findByName(@Param("name") String name);
```

---

## 8.3 JPQL select query

```java
SELECT u FROM User u
```

Meaning:

```text
select User entities
from User entity
```

---

## 8.4 WHERE

```java
SELECT u
FROM User u
WHERE u.age > :age
```

---

## 8.5 ORDER BY

```java
SELECT u
FROM User u
ORDER BY u.name ASC
```

---

## 8.6 JOIN

Suppose:

```java
@Entity
public class Order {

    @ManyToOne
    private User user;
}
```

JPQL:

```java
SELECT o
FROM Order o
JOIN o.user u
WHERE u.email = :email
```

Notice the join uses the object relationship:

```text
o.user
```

rather than manually writing table join conditions.

---

## 8.7 Projection

You do not always need the entire entity.

You can use DTO projections.

```java
@Query("""
       select new com.example.dto.UserSummary(
           u.id, u.name)
       from User u
       """)
List<UserSummary> findUserSummaries();
```

This can reduce unnecessary data retrieval.

---

## 8.8 JPQL vs SQL

| Feature | JPQL | SQL |
|---|---|---|
| Targets | Entities | Tables |
| Fields | Java entity properties | DB columns |
| Database-independent | Generally yes | Often database-specific |
| ORM-aware | Yes | No |
| Primary use | JPA persistence layer | Direct relational database querying |

### Interview answer

> JPQL is an object-oriented query language defined by JPA. It queries entities and their persistent attributes, while SQL queries database tables and columns.

---

# 9. Spring Security

## 9.1 What is Spring Security?

Spring Security provides authentication, authorization, and protection against common web/security attacks for Spring applications.

The current Spring Security reference describes it as a framework for authentication, authorization, and protection against common attacks.

---

## 9.2 Authentication vs Authorization

### Authentication

> Who are you?

Example:

```text
Username: akash
Password: ********
```

### Authorization

> What are you allowed to do?

Example:

```text
USER  -> GET /orders
ADMIN -> GET /orders
ADMIN -> DELETE /orders
USER  -> DELETE /orders -> denied
```

---

## 9.3 SecurityFilterChain

Modern Spring Security configurations commonly use a `SecurityFilterChain` bean.

Example:

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(
            HttpSecurity http) throws Exception {

        http
            .csrf(csrf -> csrf.disable())
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/public/**").permitAll()
                .requestMatchers("/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            )
            .httpBasic(Customizer.withDefaults());

        return http.build();
    }
}
```

Spring Boot's security auto-configuration uses Spring Security and supports customizing the application by providing a `SecurityFilterChain` bean.

> **Important:** Disabling CSRF blindly is not universally correct. It depends on the application architecture and authentication mechanism. For browser sessions/forms, CSRF protection can be important. For stateless token-based APIs, teams may intentionally configure a different approach.

---

## 9.4 Security filter chain

A request passes through a sequence of security filters before reaching your controller.

Conceptually:

```text
HTTP Request
    |
    v
Security Filters
    |
    +--> Authentication processing
    |
    +--> Authorization checks
    |
    +--> Exception handling
    |
    v
DispatcherServlet / Controller
```

Spring Security's web architecture is built around servlet filters arranged in a filter chain.

---

## 9.5 Basic authentication flow

For HTTP Basic:

```text
Client
  |
  | Authorization: Basic ...
  v
Security filter chain
  |
  v
AuthenticationManager
  |
  v
AuthenticationProvider
  |
  v
UserDetailsService / user store
  |
  v
Password verification
  |
  v
Authenticated SecurityContext
  |
  v
Controller
```

---

## 9.6 UserDetailsService

Example:

```java
@Bean
public UserDetailsService userDetailsService() {

    UserDetails user = User
            .withUsername("akash")
            .password(passwordEncoder().encode("password"))
            .roles("USER")
            .build();

    return new InMemoryUserDetailsManager(user);
}
```

In real applications, users are commonly loaded from a database.

---

## 9.7 PasswordEncoder

Do not store plain-text passwords.

```java
@Bean
public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}
```

Store a secure password hash, not the original password.

---

## 9.8 Role-based authorization

```java
.requestMatchers("/admin/**").hasRole("ADMIN")
```

Method-level:

```java
@PreAuthorize("hasRole('ADMIN')")
public void deleteUser(Long id) {
}
```

Enable method security where appropriate:

```java
@EnableMethodSecurity
```

---

## 9.9 JWT-based authentication — interview overview

In stateless REST APIs, a common architecture is:

```text
Login Request
   |
   v
Authentication
   |
   v
JWT generated
   |
   v
Client stores token appropriately
   |
   v
Future request:
Authorization: Bearer <token>
   |
   v
JWT authentication filter / resource server
   |
   v
Validate token
   |
   v
Create authenticated SecurityContext
   |
   v
Controller
```

The exact implementation can use Spring Security's OAuth2 Resource Server support rather than a custom handwritten filter. Custom filters should be added only when there is a real requirement.

---

## 9.10 401 vs 403

### 401 Unauthorized

Usually means the request is not properly authenticated.

Example:

```text
Missing/invalid credentials
```

### 403 Forbidden

The user is authenticated but does not have sufficient permissions.

Example:

```text
USER attempts ADMIN-only operation
```

Interview rule:

```text
401 -> authentication problem
403 -> authorization/permission problem
```

---

# 10. Profiles in Spring Boot

## 10.1 What is a Spring profile?

A **Spring profile** allows you to activate different beans/configuration for different environments or application modes.

Typical profiles:

```text
dev
qa
staging
prod
```

---

## 10.2 Profile-specific configuration

Example:

```text
application.properties
application-dev.properties
application-prod.properties
```

`application.properties`:

```properties
spring.application.name=user-service
```

`application-dev.properties`:

```properties
server.port=8081
spring.datasource.url=jdbc:h2:mem:testdb
```

`application-prod.properties`:

```properties
server.port=8080
spring.datasource.url=jdbc:postgresql://prod-db:5432/users
```

Activate a profile:

```properties
spring.profiles.active=dev
```

or commonly via the command line/environment at deployment time:

```bash
java -jar app.jar --spring.profiles.active=prod
```

---

## 10.3 `@Profile`

Example:

```java
@Configuration
@Profile("dev")
public class DevConfig {

    @Bean
    public PaymentClient paymentClient() {
        return new MockPaymentClient();
    }
}
```

Production:

```java
@Configuration
@Profile("prod")
public class ProdConfig {

    @Bean
    public PaymentClient paymentClient() {
        return new RealPaymentClient();
    }
}
```

Only beans belonging to active profiles are registered.

---

## 10.4 Profiles vs environment variables

These are related but not identical.

```text
Spring Profile
    -> selects environment-specific beans/configuration

Environment variable
    -> supplies configuration value to the application
```

You can combine them:

```text
PROFILE = prod
DB_URL  = supplied securely by deployment environment
```

---

## 10.5 Good production practice

Do not put secrets like database passwords directly into source-controlled profile files.

Prefer secret management/environment mechanisms appropriate to your deployment platform.

---

## 10.6 Interview questions

### What is `@Profile`?

It conditionally registers a bean/configuration only when the specified profile is active.

### Can multiple profiles be active?

Yes. Spring applications can have multiple active profiles.

Example:

```properties
spring.profiles.active=prod,monitoring
```

---

# 11. Exception Handling in Spring Boot

## 11.1 Why central exception handling?

Without central handling, controllers often become repetitive:

```java
try {
    ...
} catch (UserNotFoundException e) {
    ...
}
```

throughout every method.

A cleaner approach is centralized exception handling.

---

## 11.2 Custom exception

```java
public class UserNotFoundException extends RuntimeException {

    public UserNotFoundException(Long id) {
        super("User not found: " + id);
    }
}
```

Service:

```java
@Service
public class UserService {

    public User getUser(Long id) {
        return userRepository.findById(id)
                .orElseThrow(() ->
                        new UserNotFoundException(id));
    }
}
```

---

## 11.3 `@RestControllerAdvice`

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(UserNotFoundException.class)
    public ResponseEntity<ApiError> handleUserNotFound(
            UserNotFoundException ex) {

        ApiError error = new ApiError(
                404,
                ex.getMessage()
        );

        return ResponseEntity.status(HttpStatus.NOT_FOUND)
                .body(error);
    }
}
```

This lets controllers stay focused on business flow.

---

## 11.4 Error response DTO

```java
public record ApiError(
        int status,
        String message
) {
}
```

Response:

```json
{
  "status": 404,
  "message": "User not found: 10"
}
```

A production API may include additional fields such as:

```text
timestamp
status
error
message
path
traceId/correlationId
validationErrors
```

Be careful not to leak sensitive implementation details or stack traces to clients.

---

## 11.5 Validation exception handling

Suppose:

```java
public record CreateUserRequest(
        @NotBlank String name,
        @Email @NotBlank String email
) {
}
```

Controller:

```java
@PostMapping
public UserResponse create(
        @Valid @RequestBody CreateUserRequest request) {
    return service.create(request);
}
```

Invalid input can result in validation-related exceptions that your global handler can convert into a clean API response.

Example structure:

```java
@ExceptionHandler(MethodArgumentNotValidException.class)
public ResponseEntity<ApiError> handleValidation(
        MethodArgumentNotValidException ex) {

    String message = ex.getBindingResult()
            .getFieldErrors()
            .stream()
            .map(error -> error.getField() + ": " + error.getDefaultMessage())
            .findFirst()
            .orElse("Validation failed");

    return ResponseEntity.badRequest()
            .body(new ApiError(400, message));
}
```

---

## 11.6 `@ControllerAdvice` vs `@RestControllerAdvice`

### `@ControllerAdvice`

Global advice for controllers. If returning response bodies, you may also use `@ResponseBody` as appropriate.

### `@RestControllerAdvice`

Convenience combination suited to REST APIs.

Conceptually:

```text
@ControllerAdvice + @ResponseBody
```

---

## 11.7 Checked vs unchecked exceptions

In Spring service code, domain/business exceptions are often modeled as unchecked exceptions:

```java
extends RuntimeException
```

Reason: callers do not have to add noisy `throws` declarations for every business exception, and Spring applications can centrally translate them into HTTP responses.

This is a design choice, not a universal rule.

---

## 11.8 `ResponseStatusException`

For simple cases:

```java
throw new ResponseStatusException(
        HttpStatus.NOT_FOUND,
        "User not found"
);
```

This is useful but should not replace a consistent error-handling strategy in a larger API.

---

## 11.9 HTTP status mapping

A common API mapping is:

```text
400 -> Bad request / validation
401 -> Authentication required/failed
403 -> Forbidden
404 -> Resource not found
409 -> Conflict
422 -> Unprocessable semantic input (if adopted by API contract)
500 -> Unexpected server error
```

The exact contract depends on the API design.

---

# 12. Lazy Loading

## 12.1 What is lazy loading?

Lazy loading means an associated object/data is loaded only when it is actually needed, rather than eagerly loading it immediately.

Consider:

```java
@Entity
public class Department {

    @OneToMany(mappedBy = "department", fetch = FetchType.LAZY)
    private List<Employee> employees;
}
```

When you load a department:

```java
Department dept = departmentRepository.findById(1L).orElseThrow();
```

Hibernate may not immediately fetch all employees.

Only when code accesses:

```java
dept.getEmployees().size();
```

does Hibernate need to initialize the collection.

---

## 12.2 EAGER vs LAZY

### EAGER

Load association immediately.

```java
@ManyToOne(fetch = FetchType.EAGER)
```

### LAZY

Load association when accessed.

```java
@OneToMany(fetch = FetchType.LAZY)
```

Do not assume that simply declaring LAZY always gives exactly one specific SQL shape; actual SQL can depend on the query, provider, fetch plan, proxies/enhancement, entity graph, joins, and other factors.

---

## 12.3 Why lazy loading exists

Imagine:

```text
Customer
  |
  +-- Orders: 10,000 rows
```

If you only need customer name:

```java
customer.getName()
```

Loading 10,000 orders immediately would be unnecessary.

Lazy loading can reduce:

- Unnecessary DB work
- Memory usage
- Initial query cost
- Large object graphs

---

## 12.4 The LazyInitializationException

A classic problem is:

```java
@Transactional
public Department getDepartment(Long id) {
    return repository.findById(id).orElseThrow();
}
```

Then outside the transaction, another layer tries:

```java
department.getEmployees().size();
```

If the persistence context is already closed and the lazy collection needs initialization, Hibernate may throw:

```text
LazyInitializationException
```

Conceptually:

```text
Session open
   |
   v
Load Department
   |
   v
Session closes
   |
   v
Access lazy employees
   |
   X
Cannot initialize -> LazyInitializationException
```

---

## 12.5 Correct solutions

### Solution 1: Access within transaction

```java
@Transactional
public DepartmentResponse getDepartment(Long id) {
    Department department = repository.findById(id).orElseThrow();

    int count = department.getEmployees().size();

    return new DepartmentResponse(
            department.getId(),
            department.getName(),
            count
    );
}
```

### Solution 2: Fetch join

JPQL:

```java
@Query("""
       select d
       from Department d
       left join fetch d.employees
       where d.id = :id
       """)
Optional<Department> findDepartmentWithEmployees(Long id);
```

### Solution 3: EntityGraph

```java
@EntityGraph(attributePaths = "employees")
Optional<Department> findById(Long id);
```

### Solution 4: DTO projection

Often the cleanest API-facing solution is to query exactly the data required by the response.

---

## 12.6 Why `Open Session in View` should be discussed carefully

Some applications keep the persistence context open through the web request, which can make lazy loading available during view/request processing.

However, relying on this to hide poor fetch planning can lead to unexpected database access during serialization and potentially N+1 query problems.

A stronger design is to define an explicit transaction and fetch plan at the service/data-access boundary.

---

## 12.7 N+1 query problem

Suppose:

```java
List<Department> departments = departmentRepository.findAll();
```

Then:

```java
for (Department d : departments) {
    System.out.println(d.getEmployees().size());
}
```

Potential SQL pattern:

```text
1 query -> fetch all departments
N queries -> fetch employees for each department
```

Total:

```text
1 + N queries
```

This is the **N+1 query problem**.

Potential solutions:

- Join fetch
- Entity graphs
- DTO projections
- Batch fetching
- Query redesign

---

## 12.8 Interview answer

> Lazy loading delays retrieval of an association until it is accessed. It can improve performance by avoiding unnecessary data loading, but the association must be initialized while the persistence context is available, otherwise a `LazyInitializationException` can occur. Fetch joins, entity graphs, DTO projections, and properly scoped transactions are common ways to control fetch behavior.

---

# 13. JSONObject and JSON Handling

## 13.1 What is JSON?

JSON stands for **JavaScript Object Notation**.

Example:

```json
{
  "id": 101,
  "name": "Akash",
  "active": true
}
```

It is a text-based data interchange format commonly used in REST APIs.

---

## 13.2 What is `JSONObject`?

A commonly used `JSONObject` class is from the `org.json` library.

Maven example:

```xml
<dependency>
    <groupId>org.json</groupId>
    <artifactId>json</artifactId>
    <version>${json.version}</version>
</dependency>
```

Choose the version appropriate for your project rather than blindly copying an old version from a tutorial.

---

## 13.3 Creating a JSONObject

```java
JSONObject json = new JSONObject();

json.put("id", 101);
json.put("name", "Akash");
json.put("active", true);

System.out.println(json.toString());
```

Output:

```json
{"id":101,"name":"Akash","active":true}
```

---

## 13.4 Reading values

```java
String name = json.getString("name");
int id = json.getInt("id");
boolean active = json.getBoolean("active");
```

---

## 13.5 Optional values

If a property may not exist, APIs such as `optString`, `optInt`, etc. can avoid a direct exception for missing keys.

```java
String city = json.optString("city", "Unknown");
```

---

## 13.6 Nested JSONObject

```java
JSONObject address = new JSONObject();
address.put("city", "Pune");
address.put("country", "India");

JSONObject user = new JSONObject();
user.put("name", "Akash");
user.put("address", address);
```

Result:

```json
{
  "name": "Akash",
  "address": {
    "city": "Pune",
    "country": "India"
  }
}
```

---

## 13.7 JSONObject in a Spring Boot REST API

You can technically return a `JSONObject`:

```java
@GetMapping("/demo")
public JSONObject demo() {
    JSONObject response = new JSONObject();
    response.put("status", "success");
    return response;
}
```

However, for most Spring Boot APIs, a **DTO/record + Jackson** is cleaner and more type-safe.

---

## 13.8 Preferred Spring Boot approach — DTO + Jackson

DTO:

```java
public record UserResponse(
        Long id,
        String name,
        String email
) {
}
```

Controller:

```java
@GetMapping("/{id}")
public UserResponse getUser(@PathVariable Long id) {
    return userService.getUser(id);
}
```

Spring Boot commonly uses Jackson for JSON serialization/deserialization when using its web stack.

Spring automatically converts:

```text
Java object
    |
    | Jackson
    v
JSON response
```

and:

```text
JSON request
    |
    | Jackson
    v
Java DTO
```

---

## 13.9 ObjectMapper

You may see:

```java
ObjectMapper objectMapper = new ObjectMapper();
```

Convert Java object to JSON string:

```java
String json = objectMapper.writeValueAsString(user);
```

Convert JSON string to Java object:

```java
UserResponse user = objectMapper.readValue(
        json,
        UserResponse.class
);
```

In a Spring Boot application, prefer injecting/configuring the application's `ObjectMapper` rather than repeatedly constructing unmanaged instances when the framework already provides one.

---

## 13.10 JSONObject vs ObjectMapper

| Feature | JSONObject | Jackson ObjectMapper |
|---|---|---|
| Style | Programmatic JSON tree/map-like API | Serialization/deserialization framework |
| Type safety | Lower | Higher with DTOs |
| Spring Boot integration | Manual | Excellent/native ecosystem integration |
| DTO mapping | Manual/limited | Strong support |
| Typical REST API use | Sometimes useful | Very common |

### Interview answer

> `JSONObject` is useful when dynamically creating or reading JSON key-value structures. In a typical Spring Boot REST API, DTOs with Jackson are preferred because serialization/deserialization is integrated with Spring MVC and provides better type safety and maintainability.

---

# 14. How These Concepts Fit Together

A real Spring Boot request may combine almost every topic above.

Consider:

```text
Client
  |
  | GET /users/101
  v
Spring Security Filter Chain
  |
  | authenticate/authorize
  v
DispatcherServlet
  |
  | route request
  v
UserController
  |
  v
UserService
  |
  | @Transactional
  v
UserRepository
  |
  | Spring Data JPA
  v
JPA EntityManager
  |
  v
Hibernate
  |
  v
PostgreSQL
```

Then:

```text
PostgreSQL row
      |
      v
Hibernate entity
      |
      v
Service / DTO mapping
      |
      v
Jackson serialization
      |
      v
JSON response
```

Meanwhile:

```text
Actuator -> monitors health/metrics/startup
Profiles -> choose environment-specific configuration
Global exception handler -> converts exceptions to API responses
Spring bean container -> creates and wires all managed components
```

---

# 15. Common Interview Questions and Answers

## Q1. What is Spring Boot?

Spring Boot is a framework built on Spring that simplifies application setup by providing opinionated defaults, auto-configuration, starter dependencies, embedded server support, externalized configuration, and production-oriented features.

---

## Q2. What is the difference between Spring and Spring Boot?

**Spring Framework** provides the core programming model and infrastructure, such as IoC, DI, MVC, transactions, and AOP.

**Spring Boot** simplifies Spring application setup using auto-configuration, starters, embedded server support, externalized configuration, and operational features.

---

## Q3. What is `@SpringBootApplication`?

It is a composed annotation that combines the semantics of configuration, component scanning, and auto-configuration.

---

## Q4. What is a Spring Bean?

An object managed by the Spring IoC container.

---

## Q5. What is dependency injection?

Providing an object's dependencies from outside rather than having the object create them itself.

---

## Q6. Which injection type is preferred?

Constructor injection is generally preferred because dependencies are explicit, can be immutable, are easier to test, and are mandatory by construction.

---

## Q7. `@Component` vs `@Service` vs `@Repository`?

All are stereotype annotations used for component scanning, but they communicate different roles.

```text
@Component -> generic component
@Service   -> service/business layer
@Repository -> persistence/data access layer
```

`@Repository` also participates in Spring's exception translation mechanisms for supported persistence technologies.

---

## Q8. `@Controller` vs `@RestController`?

`@Controller` is used for MVC controllers; `@RestController` is designed for REST responses and effectively combines controller semantics with response-body behavior.

---

## Q9. What is `DispatcherServlet`?

The central front controller in Spring MVC that coordinates request processing and delegates to MVC infrastructure.

---

## Q10. What is JPA?

JPA is a persistence specification/API defining ORM concepts such as entities, persistence context, entity manager, and JPQL.

---

## Q11. What is Hibernate?

Hibernate is an ORM framework and a commonly used implementation of JPA.

---

## Q12. What is Spring Data JPA?

A Spring Data module that simplifies JPA-based data access through repository abstractions, derived queries, pagination, projections, and custom query support.

---

## Q13. What is JPQL?

JPQL is JPA's object-oriented query language. It queries entity classes and their fields instead of database tables and columns.

---

## Q14. What is lazy loading?

Lazy loading delays loading associated data until the association is accessed.

---

## Q15. What is `LazyInitializationException`?

A Hibernate exception commonly encountered when a lazy association needs initialization after the relevant persistence context/session is no longer available.

---

## Q16. What is dirty checking?

Hibernate/JPA tracks managed entities and detects changes so it can synchronize modified state with the database during flush/transaction completion.

---

## Q17. What is the N+1 problem?

One query retrieves the parent records, then one additional query is executed per parent to retrieve related records, producing `1 + N` queries.

---

## Q18. What is Spring Security?

A framework for authentication, authorization, and protection against common application attacks.

---

## Q19. What is the difference between authentication and authorization?

```text
Authentication -> Who are you?
Authorization  -> What can you access/do?
```

---

## Q20. What is a SecurityFilterChain?

It is the configured sequence of Spring Security filters through which HTTP requests pass before reaching protected application resources.

---

## Q21. What is a Spring profile?

A mechanism for activating environment-specific beans/configuration such as `dev`, `qa`, or `prod`.

---

## Q22. What is `@RestControllerAdvice`?

A convenient mechanism for centralized exception handling across REST controllers.

---

## Q23. Why use DTOs?

DTOs provide an explicit API contract and prevent exposing persistence entities directly. They also support validation, data shaping, versioning, and security boundaries.

---

## Q24. Should we return entities directly from REST controllers?

You can, but DTOs are often preferred in production APIs because entities represent persistence concerns while API responses represent an external contract.

---

## Q25. What happens when a request reaches a secured Spring Boot API?

A simplified flow is:

```text
HTTP Request
   |
   v
Security Filter Chain
   |
   v
Authentication / authorization
   |
   v
DispatcherServlet
   |
   v
Controller
   |
   v
Service
   |
   v
Repository/JPA/Hibernate
   |
   v
Database
   |
   v
JSON serialization
   |
   v
HTTP Response
```

---

# 16. Quick Revision Sheet

## Spring Boot startup

```text
main()
 -> SpringApplication.run()
 -> Environment
 -> ApplicationContext
 -> Auto-configuration
 -> Component scan
 -> Bean definitions
 -> Bean creation
 -> Dependency injection
 -> Bean initialization
 -> Embedded server
 -> Ready
```

## Bean lifecycle

```text
Create
 -> Inject dependencies
 -> BeforeInitialization
 -> @PostConstruct
 -> afterPropertiesSet
 -> Custom init
 -> AfterInitialization
 -> Ready
 -> @PreDestroy
 -> Destroy
```

## Dependency injection

```text
Constructor   -> preferred
Setter        -> optional/changeable dependency
Field         -> works, generally less preferred
Method        -> possible, less common
```

## MVC

```text
Request
 -> Security filters
 -> DispatcherServlet
 -> HandlerMapping
 -> Controller
 -> Service
 -> Repository
 -> Response conversion
 -> JSON
```

## JPA stack

```text
Spring Data JPA
       |
       v
JPA EntityManager
       |
       v
Hibernate
       |
       v
JDBC / Database
```

## JPQL

```text
JPQL -> Entity + property
SQL  -> Table + column
```

## Security

```text
Authentication -> identity
Authorization  -> permission
401            -> authentication issue
403            -> authorization issue
```

## Lazy loading

```text
Load parent
   |
   v
Association not immediately loaded
   |
   v
Access association
   |
   v
Hibernate loads it
```

## Exception handling

```text
Controller / Service throws exception
            |
            v
@RestControllerAdvice
            |
            v
@ExceptionHandler
            |
            v
Consistent API error response
```

## Actuator

```text
/actuator/health  -> health
/actuator/info    -> application info
/actuator/metrics -> metrics
/actuator/startup -> startup information
```

---

# Interview-Level Deep-Dive Summary

For a strong Spring Boot interview answer, connect the concepts rather than memorizing isolated definitions:

1. **Spring Boot starts with `SpringApplication.run()`**, creates the application context, applies configuration and auto-configuration, discovers beans, wires dependencies, and starts the application.
2. **Beans are managed objects** created and controlled by the Spring IoC container.
3. **Dependency Injection** removes direct object-creation coupling; constructor injection is usually the best default.
4. **Spring MVC** handles HTTP requests through the `DispatcherServlet`, maps requests to controllers, invokes application logic, and converts responses to JSON.
5. **Spring Data JPA** gives repository abstractions; **JPA** defines the persistence model; **Hibernate** commonly implements JPA.
6. **JPQL** queries entities rather than raw database tables.
7. **Hibernate** provides ORM capabilities such as entity state tracking, dirty checking, lazy loading, and persistence-context management.
8. **Lazy loading** can improve efficiency but requires deliberate fetch planning to avoid `LazyInitializationException` and N+1 problems.
9. **Spring Security** protects requests through a security filter chain and handles authentication/authorization.
10. **Profiles** separate environment-specific configuration and beans.
11. **Global exception handling** keeps REST APIs consistent and prevents duplicated error-handling logic.
12. **Actuator** exposes operational information such as health, metrics, and startup data for monitoring and production operations.
13. **JSONObject** is useful for dynamic JSON manipulation, while **DTO + Jackson** is usually the cleaner default for Spring Boot REST APIs.

---

# Suggested Interview Practice Flow

For each topic, practice answering three levels:

### Level 1 — Definition

Example:

> What is Spring Data JPA?

Answer in 1–2 sentences.

### Level 2 — Internal working

Example:

> How does Spring Data JPA generate repository implementations?

Explain repository proxy/infrastructure, query derivation, EntityManager, and ORM interaction at a conceptual level.

### Level 3 — Production scenario

Example:

> Your API is slow because fetching departments also loads thousands of employees. What will you check?

Discuss:

```text
Lazy/Eager fetch
N+1 queries
JOIN FETCH / EntityGraph
DTO projection
Pagination
Indexes
SQL logs/metrics
Transaction boundaries
```

That pattern — **definition -> internal flow -> real production problem -> solution** — is especially useful for Java/Spring Boot interviews.

---

# Official References

- Spring Boot — Actuator / Production-ready Features: https://docs.spring.io/spring-boot/reference/actuator/
- Spring Boot — SpringApplication and application startup: https://docs.spring.io/spring-boot/reference/features/spring-application.html
- Spring Framework — IoC Container and Beans: https://docs.spring.io/spring-framework/reference/core/beans/
- Spring Boot — Beans and Dependency Injection: https://docs.spring.io/spring-boot/reference/using/spring-beans-and-dependency-injection.html
- Spring Security Reference: https://docs.spring.io/spring-security/reference/
- Spring Data JPA Reference: https://docs.spring.io/spring-data/jpa/reference/
