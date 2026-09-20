# Creational Design Patterns — Complete Notes

> **Purpose:** Interview-friendly notes for understanding and implementing Creational Design Patterns in Java.
>
> **Primary references:**  
> - GeeksforGeeks — [Creational Design Patterns](https://www.geeksforgeeks.org/system-design/creational-design-pattern/)  
> - SourceMaking — [Creational Patterns](https://sourcemaking.com/design_patterns/creational_patterns)

---

## 1. What Are Creational Design Patterns?

Creational Design Patterns deal with **object creation**.

Normally, an object is created directly:

```java
PaymentService service = new PaymentService();
```

This is simple, but direct object creation can become a problem when:

- Object creation is complex.
- The exact implementation should be decided at runtime.
- There are many optional constructor parameters.
- We need different families of related objects.
- Creating an object is expensive.
- We need to control the number of instances.

Creational patterns move or encapsulate object-creation logic so that the rest of the application does not need to know the exact creation details.

### Core idea

```text
Without Creational Pattern:

Client
  |
  +----> new ConcreteClass()
  |
  +----> new AnotherConcreteClass()
  |
  +----> complex initialization
```

With a creational pattern:

```text
Client
  |
  +----> Creation Abstraction
             |
             +----> Concrete Object
```

The client focuses on **using the object**, while the pattern handles **how the object is created**.

---

# 2. Why Do We Need Creational Patterns?

Creational patterns mainly help with:

### 2.1 Loose Coupling

The client depends more on an interface/abstraction than on concrete implementations.

```java
Notification notification;
```

instead of:

```java
EmailNotification notification = new EmailNotification();
```

### 2.2 Encapsulation of Creation Logic

Complex construction logic is kept in one place.

### 2.3 Flexibility

The implementation can be changed without changing client code.

### 2.4 Reusability

Creation logic can be reused by multiple clients.

### 2.5 Managing Complex Objects

Builder and related patterns make complex object construction easier to understand and maintain.

---

# 3. Types of Creational Design Patterns

The classic GoF creational patterns are:

| Pattern | Main Idea |
|---|---|
| **Singleton** | Ensure only one instance exists |
| **Factory Method** | Let subclasses decide which object to create |
| **Abstract Factory** | Create families of related objects |
| **Builder** | Construct a complex object step by step |
| **Prototype** | Create objects by copying existing objects |

SourceMaking also lists **Object Pool** as a creational pattern. Object Pool is included in these notes as an additional pattern.

```text
Creational Patterns
│
├── Singleton
├── Factory Method
├── Abstract Factory
├── Builder
├── Prototype
└── Object Pool (additional pattern)
```

---

# 4. Quick Comparison

| Pattern | Main Problem Solved | Key Mechanism |
|---|---|---|
| Singleton | Need exactly one shared instance | Controlled constructor + single instance |
| Factory Method | Object type varies | Factory method / inheritance |
| Abstract Factory | Need related product families | Factory of related products |
| Builder | Complex object construction | Step-by-step construction |
| Prototype | Expensive/complex object creation | Clone existing object |
| Object Pool | Expensive resource creation | Reuse available objects |

### Easy memory trick

```text
Singleton      -> ONE
Factory Method -> WHICH OBJECT?
Abstract Factory -> WHICH FAMILY?
Builder        -> HOW TO BUILD?
Prototype      -> COPY
Object Pool    -> REUSE
```

---

# 5. Singleton Pattern

## 5.1 Definition

**Singleton ensures that a class has only one instance and provides a global access point to that instance.**

Example use cases:

- Configuration manager
- Application-wide registry
- Logging infrastructure
- Certain caches
- A shared resource manager

> Important: Singleton should not automatically be used for every shared object. Global state can make testing and maintenance harder.

---

## 5.2 Problem

Suppose we have:

```java
class DatabaseConnectionManager {
}
```

Any code can do:

```java
DatabaseConnectionManager a = new DatabaseConnectionManager();
DatabaseConnectionManager b = new DatabaseConnectionManager();
```

Now two instances exist.

If the application requires exactly one instance, normal construction is insufficient.

---

## 5.3 Basic Structure

```text
Client
  |
  v
Singleton.getInstance()
  |
  v
+-----------------------+
| Singleton             |
|-----------------------|
| - static instance     |
| - private constructor |
|-----------------------|
| + getInstance()       |
+-----------------------+
```

---

## 5.4 Java Implementation — Lazy Singleton

```java
public class Singleton {

    private static Singleton instance;

    private Singleton() {
    }

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

Usage:

```java
Singleton s1 = Singleton.getInstance();
Singleton s2 = Singleton.getInstance();

System.out.println(s1 == s2); // true
```

---

## 5.5 Thread-Safety Problem

The previous implementation is not thread-safe.

Two threads can execute:

```java
if (instance == null)
```

at the same time and both create an object.

---

## 5.6 Thread-Safe Singleton — Double-Checked Locking

```java
public class Singleton {

    private static volatile Singleton instance;

    private Singleton() {
    }

    public static Singleton getInstance() {

        if (instance == null) {
            synchronized (Singleton.class) {

                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }

        return instance;
    }
}
```

### Why `volatile`?

`volatile` helps ensure visibility between threads and prevents problematic instruction reordering around initialization.

---

## 5.7 Initialization-on-Demand Holder

A clean Java approach is:

```java
public class Singleton {

    private Singleton() {
    }

    private static class Holder {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return Holder.INSTANCE;
    }
}
```

Advantages:

- Lazy initialization
- Thread-safe
- No explicit synchronization
- Simple implementation

---

## 5.8 Enum Singleton

Java also provides a very concise approach:

```java
public enum Singleton {
    INSTANCE;

    public void doSomething() {
        System.out.println("Working...");
    }
}
```

Usage:

```java
Singleton.INSTANCE.doSomething();
```

Enum-based Singleton has strong protection against several serialization/reflection-related issues.

---

## 5.9 Advantages

- Controls the number of instances.
- Provides a single access point.
- Can support lazy initialization.
- Useful for genuinely shared resources.

## 5.10 Disadvantages

- Introduces global state.
- Can make unit testing harder.
- Can hide dependencies.
- May be overused.
- Thread-safety needs consideration.

---

## 5.11 When to Use?

Use Singleton when:

1. Exactly one instance is actually required.
2. That instance needs a well-known access point.
3. The shared state/resource is intentional.

### Interview Question

**Q: Is Singleton always a good design?**

**Answer:** No. It is useful when a single instance is a real business/technical requirement, but unnecessary Singleton usage creates global state and hidden dependencies.

---

# 6. Factory Method Pattern

## 6.1 Definition

**Factory Method defines an interface for creating an object, but allows subclasses to decide which concrete object should be created.**

The important idea is:

> **Creation is delegated to subclasses.**

SourceMaking describes Factory Method as creation through inheritance.

---

## 6.2 Problem

Suppose we have:

```java
class PaymentService {

    void process(String type) {

        if (type.equals("CARD")) {
            new CardPayment();
        } else if (type.equals("UPI")) {
            new UpiPayment();
        }
    }
}
```

As the number of payment types grows, this class becomes difficult to maintain.

---

## 6.3 Structure

```text
             Product
                ^
                |
       +--------+--------+
       |                 |
 CardPayment        UpiPayment


          Creator
             ^
             |
      +------+------+
      |             |
CardPaymentFactory  UpiPaymentFactory
```

---

## 6.4 Java Example

### Product

```java
interface Payment {
    void pay();
}
```

### Concrete Products

```java
class CardPayment implements Payment {

    @Override
    public void pay() {
        System.out.println("Payment using Card");
    }
}
```

```java
class UpiPayment implements Payment {

    @Override
    public void pay() {
        System.out.println("Payment using UPI");
    }
}
```

### Creator

```java
abstract class PaymentFactory {

    public abstract Payment createPayment();

    public void processPayment() {
        Payment payment = createPayment();
        payment.pay();
    }
}
```

### Concrete Creators

```java
class CardPaymentFactory extends PaymentFactory {

    @Override
    public Payment createPayment() {
        return new CardPayment();
    }
}
```

```java
class UpiPaymentFactory extends PaymentFactory {

    @Override
    public Payment createPayment() {
        return new UpiPayment();
    }
}
```

### Client

```java
PaymentFactory factory = new UpiPaymentFactory();
factory.processPayment();
```

The client does not directly create `UpiPayment`.

---

## 6.5 Advantages

- Reduces coupling between client and concrete product.
- Creation logic is localized.
- New products can be introduced with new creators.
- Follows Open/Closed Principle reasonably well when the hierarchy is designed appropriately.

## 6.6 Disadvantages

- Can increase the number of classes.
- Often requires inheritance.
- A simple application may not need this level of abstraction.

---

## 6.7 When to Use?

Use Factory Method when:

- The exact product type is not known in advance.
- Subclasses should control object creation.
- Creation logic needs to be separated from business logic.
- You expect different implementations of the same abstraction.

---

# 7. Abstract Factory Pattern

## 7.1 Definition

**Abstract Factory provides an interface for creating families of related or dependent objects without specifying their concrete classes.**

Think:

```text
Factory Method
    -> creates ONE type of product

Abstract Factory
    -> creates a FAMILY of related products
```

---

## 7.2 Example

Imagine a UI application supporting:

- Windows theme
- Mac theme

Each theme needs:

```text
Button
Checkbox
Textbox
```

We want the whole UI family to be consistent.

---

## 7.3 Structure

```text
                  AbstractFactory
                  /              \
                 /                \
       WindowsFactory          MacFactory
          /       \               /      \
         /         \             /        \
 WindowsButton  WindowsCheckbox  MacButton  MacCheckbox
```

---

## 7.4 Java Example

### Products

```java
interface Button {
    void render();
}
```

```java
interface Checkbox {
    void render();
}
```

### Windows Products

```java
class WindowsButton implements Button {

    @Override
    public void render() {
        System.out.println("Windows Button");
    }
}
```

```java
class WindowsCheckbox implements Checkbox {

    @Override
    public void render() {
        System.out.println("Windows Checkbox");
    }
}
```

### Mac Products

```java
class MacButton implements Button {

    @Override
    public void render() {
        System.out.println("Mac Button");
    }
}
```

```java
class MacCheckbox implements Checkbox {

    @Override
    public void render() {
        System.out.println("Mac Checkbox");
    }
}
```

### Abstract Factory

```java
interface GUIFactory {

    Button createButton();

    Checkbox createCheckbox();
}
```

### Concrete Factories

```java
class WindowsFactory implements GUIFactory {

    @Override
    public Button createButton() {
        return new WindowsButton();
    }

    @Override
    public Checkbox createCheckbox() {
        return new WindowsCheckbox();
    }
}
```

```java
class MacFactory implements GUIFactory {

    @Override
    public Button createButton() {
        return new MacButton();
    }

    @Override
    public Checkbox createCheckbox() {
        return new MacCheckbox();
    }
}
```

### Client

```java
GUIFactory factory = new WindowsFactory();

Button button = factory.createButton();
Checkbox checkbox = factory.createCheckbox();

button.render();
checkbox.render();
```

The client works with interfaces and doesn't need to know the concrete product classes.

---

## 7.5 Key Point

Abstract Factory is especially useful when:

> Multiple objects need to be created together and they must belong to the same compatible family.

Example:

```text
WindowsFactory
    -> WindowsButton
    -> WindowsCheckbox
    -> WindowsTextbox
```

instead of accidentally mixing:

```text
WindowsButton
MacCheckbox
WindowsTextbox
```

---

## 7.6 Factory Method vs Abstract Factory

| Feature | Factory Method | Abstract Factory |
|---|---|---|
| Creates | Usually one product | Family of products |
| Main mechanism | Method/subclass | Factory object |
| Focus | Which product? | Which product family? |
| Typical example | Payment factory | UI component family |
| Complexity | Lower | Higher |

### Memory Trick

```text
Factory Method:
"Which object should I create?"

Abstract Factory:
"Which family of related objects should I create?"
```

---

## 7.7 Advantages

- Keeps related products consistent.
- Hides concrete implementations.
- Makes switching product families easier.
- Reduces client dependency on concrete classes.

## 7.8 Disadvantages

- More interfaces/classes.
- Adding a new product type can require changes across all factories.
- Can be excessive for simple systems.

---

# 8. Builder Pattern

## 8.1 Definition

**Builder separates the construction of a complex object from its representation and allows the object to be constructed step by step.**

This is one of the most commonly used creational patterns in Java.

---

## 8.2 Problem

Consider:

```java
User user = new User(
    "Akash",
    "akash@example.com",
    25,
    "Pune",
    "Java",
    true,
    false
);
```

Problems:

- Too many constructor parameters.
- Hard to understand parameter order.
- Many optional parameters.
- Constructor becomes difficult to maintain.

---

## 8.3 Builder Solution

Instead:

```java
User user = new User.Builder()
        .name("Akash")
        .email("akash@example.com")
        .age(25)
        .city("Pune")
        .skill("Java")
        .build();
```

This is easier to read.

---

## 8.4 Structure

```text
Client
  |
  v
Builder
  |
  +-- setName()
  +-- setEmail()
  +-- setAge()
  +-- setCity()
  |
  +-- build()
        |
        v
      Product
```

---

## 8.5 Java Example

```java
public class User {

    private final String name;
    private final String email;
    private final int age;
    private final String city;

    private User(Builder builder) {
        this.name = builder.name;
        this.email = builder.email;
        this.age = builder.age;
        this.city = builder.city;
    }

    public static class Builder {

        private String name;
        private String email;
        private int age;
        private String city;

        public Builder name(String name) {
            this.name = name;
            return this;
        }

        public Builder email(String email) {
            this.email = email;
            return this;
        }

        public Builder age(int age) {
            this.age = age;
            return this;
        }

        public Builder city(String city) {
            this.city = city;
            return this;
        }

        public User build() {
            return new User(this);
        }
    }
}
```

Usage:

```java
User user = new User.Builder()
        .name("Akash")
        .email("akash@example.com")
        .age(25)
        .city("Pune")
        .build();
```

---

## 8.6 Advantages

- Improves readability.
- Handles many optional parameters.
- Supports immutable objects well.
- Separates construction from the final object.
- Allows validation in `build()`.

Example:

```java
public User build() {

    if (email == null || email.isBlank()) {
        throw new IllegalArgumentException("Email is required");
    }

    return new User(this);
}
```

---

## 8.7 Disadvantages

- More code.
- Extra Builder class.
- Not necessary for objects with only a few simple fields.

---

## 8.8 Builder vs Constructor

### Constructor

```java
new User("Akash", "email", 25, "Pune");
```

### Builder

```java
new User.Builder()
    .name("Akash")
    .email("email")
    .age(25)
    .city("Pune")
    .build();
```

Builder becomes more useful as the number of optional/configurable fields grows.

---

## 8.9 Builder vs Abstract Factory

A common interview question:

**Builder focuses on constructing one complex object step by step.**

**Abstract Factory focuses on producing families of related objects.**

```text
Builder
    -> Complex Product
       -> construct step-by-step

Abstract Factory
    -> Product A
    -> Product B
    -> Product C
       -> related product family
```

---

# 9. Prototype Pattern

## 9.1 Definition

**Prototype creates a new object by copying an existing object instead of constructing the object from scratch.**

SourceMaking describes Prototype as a fully initialized instance that can be copied or cloned.

---

## 9.2 Problem

Suppose creating an object requires expensive operations:

```text
Load large configuration
Read database
Load metadata
Perform calculations
Initialize many fields
```

Creating every object from scratch may be expensive.

Instead:

```text
Existing initialized object
          |
          | clone
          v
      New object
```

---

## 9.3 Structure

```text
             Prototype
                 ^
                 |
        +--------+--------+
        |                 |
  ConcretePrototype 1  ConcretePrototype 2

Client ---> clone()
```

---

## 9.4 Java Example

Using an explicit `copy()` method is often clearer than relying blindly on `Object.clone()`.

```java
public class Employee {

    private String name;
    private String department;

    public Employee(String name, String department) {
        this.name = name;
        this.department = department;
    }

    public Employee copy() {
        return new Employee(this.name, this.department);
    }
}
```

Usage:

```java
Employee original =
        new Employee("Akash", "Engineering");

Employee copy = original.copy();
```

---

## 9.5 Prototype Interface

A more explicit design:

```java
interface Prototype<T> {
    T copy();
}
```

```java
class Document implements Prototype<Document> {

    private String title;
    private String content;

    public Document(String title, String content) {
        this.title = title;
        this.content = content;
    }

    @Override
    public Document copy() {
        return new Document(this.title, this.content);
    }
}
```

---

## 9.6 Shallow Copy vs Deep Copy

This is extremely important for interviews.

### Shallow Copy

Primitive/value fields are copied, but referenced objects may still be shared.

```text
Original
  |
  +---- Address ----+
                   |
Copy --------------+
```

Both objects point to the same `Address`.

### Deep Copy

Referenced objects are copied too.

```text
Original
  |
  +---- Address A

Copy
  |
  +---- Address B
```

Now changing Address B does not change Address A.

---

## 9.7 Example

```java
class Address {
    String city;

    Address(String city) {
        this.city = city;
    }
}
```

Shallow copy:

```java
class Person {

    String name;
    Address address;

    Person(Person other) {
        this.name = other.name;
        this.address = other.address;
    }
}
```

Deep copy:

```java
class Person {

    String name;
    Address address;

    Person(Person other) {
        this.name = other.name;
        this.address = new Address(other.address.city);
    }
}
```

---

## 9.8 Advantages

- Can avoid expensive object creation.
- Can hide complex initialization.
- Useful when object configuration is expensive.
- Can reduce the need for large factory hierarchies.

## 9.9 Disadvantages

- Deep copying can be difficult.
- Circular references can complicate cloning.
- Mutable nested objects require careful handling.
- Clone semantics can become confusing.

---

## 9.10 When to Use?

Use Prototype when:

- Object creation is expensive.
- Objects are mostly configured copies of an existing object.
- The concrete class is determined at runtime.
- You want to avoid a large factory hierarchy.

---

# 10. Object Pool Pattern

> **Note:** Object Pool appears in SourceMaking's creational-pattern list, but it is not one of the five classic GoF creational patterns. It is included here because it is useful in real-world resource management.

## 10.1 Definition

Object Pool maintains a collection of reusable objects.

Instead of:

```text
Create -> Use -> Destroy
Create -> Use -> Destroy
Create -> Use -> Destroy
```

we do:

```text
Create a pool
    |
    +--> Object 1
    +--> Object 2
    +--> Object 3

Borrow -> Use -> Return
Borrow -> Use -> Return
```

The purpose is to avoid repeatedly creating and releasing expensive resources.

---

## 10.2 Examples

Object Pool can be useful for expensive or limited resources such as:

- Database connections
- Thread pools
- HTTP connections
- Expensive reusable resources

For example, JDBC connection pools allow applications to reuse database connections rather than opening a new physical connection for every request.

---

## 10.3 Basic Structure

```text
             ObjectPool
             /        \
            /          \
       borrow()       release()
          |              ^
          v              |
      Available       Used Object
       Objects
```

---

## 10.4 Simplified Java Example

```java
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.LinkedBlockingQueue;

class Worker {

    public void execute() {
        System.out.println("Working...");
    }
}
```

```java
class WorkerPool {

    private final BlockingQueue<Worker> pool;

    public WorkerPool(int size) {
        pool = new LinkedBlockingQueue<>();

        for (int i = 0; i < size; i++) {
            pool.offer(new Worker());
        }
    }

    public Worker borrow() throws InterruptedException {
        return pool.take();
    }

    public void release(Worker worker) {
        pool.offer(worker);
    }
}
```

Usage:

```java
WorkerPool pool = new WorkerPool(5);

Worker worker = pool.borrow();

try {
    worker.execute();
} finally {
    pool.release(worker);
}
```

---

## 10.5 Advantages

- Reuses expensive objects.
- Reduces object creation/destruction overhead.
- Can control the maximum number of resources.
- Useful for resource-constrained systems.

## 10.6 Disadvantages

- Pool management adds complexity.
- Objects must be reset correctly before reuse.
- Idle objects consume resources.
- Pool sizing requires consideration.

---

# 11. Factory Method vs Abstract Factory vs Builder vs Prototype

This is one of the most important interview comparisons.

| Pattern | Question It Answers |
|---|---|
| Factory Method | **Which object should be created?** |
| Abstract Factory | **Which family of objects should be created?** |
| Builder | **How should a complex object be constructed?** |
| Prototype | **Can I copy an existing object instead?** |
| Singleton | **Should there be only one instance?** |
| Object Pool | **Can I reuse an expensive object?** |

---

# 12. Factory Method vs Simple Factory

These are often confused.

## Simple Factory

Simple Factory is generally a technique rather than one of the classic GoF patterns.

```java
class PaymentFactory {

    public static Payment create(String type) {

        if (type.equals("CARD")) {
            return new CardPayment();
        }

        if (type.equals("UPI")) {
            return new UpiPayment();
        }

        throw new IllegalArgumentException("Unknown type");
    }
}
```

Usage:

```java
Payment payment = PaymentFactory.create("UPI");
```

The factory contains the selection logic.

---

## Factory Method

Factory Method generally uses a creator abstraction and lets subclasses provide the concrete creation behavior.

```text
Creator
   |
   +--> createProduct()
          ^
          |
   ConcreteCreator
          |
          v
   ConcreteProduct
```

### Interview shortcut

```text
Simple Factory
    -> one factory + conditional logic

Factory Method
    -> creation delegated through polymorphism/subclasses
```

---

# 13. Abstract Factory vs Factory Method

Think of an e-commerce application.

### Factory Method

Create one type:

```text
PaymentFactory
      |
      +--> CardPayment
```

### Abstract Factory

Create a complete family:

```text
IndiaPaymentFactory
    |
    +--> Payment
    +--> Invoice
    +--> TaxCalculator

USPaymentFactory
    |
    +--> Payment
    +--> Invoice
    +--> TaxCalculator
```

The family must remain compatible.

---

# 14. Builder vs Factory

### Builder

Focus:

```text
How do I construct this complex object?
```

Example:

```java
new User.Builder()
    .name("Akash")
    .age(25)
    .city("Pune")
    .build();
```

### Factory

Focus:

```text
Which implementation should I create?
```

Example:

```java
Payment payment = PaymentFactory.create("UPI");
```

---

# 15. Prototype vs Factory

### Factory

Creates a new object by knowing how to instantiate the concrete type.

```text
Factory
   |
   +--> new Product()
```

### Prototype

Creates a new object by copying an existing configured object.

```text
Prototype
    |
    +--> clone/copy
           |
           v
       New Product
```

Prototype can be useful when construction is expensive or the object configuration already exists.

---

# 16. How These Patterns Can Work Together

Creational patterns are not always isolated.

SourceMaking points out that some creational patterns can be complementary.

For example:

```text
Abstract Factory
       |
       +---- uses Prototype
       |
       +---- returns cloned products
```

Or:

```text
Builder
   |
   +---- uses Factory Method
   |
   +---- decides which component to create
```

A Singleton may also be used to provide a single factory instance when that is genuinely required.

### Important principle

Do not choose a pattern just because it is available.

Choose it because it solves a specific design problem.

---

# 17. Real-World Java Examples

## Singleton

Common examples:

- Configuration manager
- Application-level registry
- Certain shared caches

Be careful with global mutable state.

---

## Factory Method

Examples:

- Payment provider creation
- Notification creation
- File parser creation
- Report generator creation

```text
"PDF" -> PdfReportGenerator
"EXCEL" -> ExcelReportGenerator
```

---

## Abstract Factory

Examples:

- Cross-platform UI components
- Cloud provider-specific components
- Database-family-specific components
- Region-specific product families

```text
AWSFactory
   -> AWSStorage
   -> AWSQueue
   -> AWSDatabase

AzureFactory
   -> AzureStorage
   -> AzureQueue
   -> AzureDatabase
```

---

## Builder

Examples:

- HTTP request construction
- Complex DTO construction
- Configuration objects
- SQL/query builders
- Immutable domain objects

Java-style APIs frequently use fluent builders.

---

## Prototype

Examples:

- Copying configured templates
- Cloning document templates
- Creating similar game objects
- Copying complex configuration objects

---

## Object Pool

Examples:

- Database connection pools
- Thread pools
- Connection/resource reuse

---

# 18. Advantages of Creational Patterns

## 18.1 Reduced Coupling

Clients can depend on abstractions rather than concrete implementations.

## 18.2 Encapsulated Creation

Object creation logic is kept in dedicated components.

## 18.3 Better Maintainability

Changes to construction logic are localized.

## 18.4 Better Reusability

The same creation logic can be used in multiple places.

## 18.5 Flexibility

The system can switch implementations more easily.

## 18.6 Better Handling of Complex Construction

Builder and Prototype can simplify otherwise complicated creation workflows.

---

# 19. Disadvantages

Creational patterns are not free.

Possible disadvantages:

- More classes/interfaces.
- More abstraction.
- More code.
- Increased learning/maintenance overhead.
- Some patterns can be over-engineering for simple applications.
- Poorly designed factories can become large conditional blocks.
- Singleton can introduce global state.
- Object Pool can create lifecycle-management complexity.

### Rule

> **Use the simplest design that solves the actual problem.**

---

# 20. Interview-Oriented Decision Tree

When you see an object-creation problem, ask:

```text
Do I need exactly ONE instance?
        |
       YES
        |
    Singleton


Do I need to choose between implementations?
        |
       YES
        |
    Factory Method


Do I need a family of related objects?
        |
       YES
        |
  Abstract Factory


Does the object have many fields/options
or complex construction steps?
        |
       YES
        |
      Builder


Is creating the object expensive,
and can I copy an existing object?
        |
       YES
        |
    Prototype


Is the resource expensive to create
but safe to reuse?
        |
       YES
        |
   Object Pool
```

---

# 21. One-Page Cheat Sheet

| Pattern | Intent | Keyword | Example |
|---|---|---|---|
| Singleton | One instance | **ONE** | Config Manager |
| Factory Method | Create a product through polymorphism | **WHICH** | Payment |
| Abstract Factory | Create related product families | **FAMILY** | UI components |
| Builder | Build complex object step-by-step | **BUILD** | User/HTTP Request |
| Prototype | Copy an existing object | **COPY** | Document template |
| Object Pool | Reuse expensive resources | **REUSE** | DB connections |

---

# 22. Most Important Interview Questions

## Q1. What are Creational Design Patterns?

They are design patterns that deal with object creation and help separate object creation from object usage.

---

## Q2. Name the five classic Creational Design Patterns.

1. Singleton
2. Factory Method
3. Abstract Factory
4. Builder
5. Prototype

---

## Q3. Factory Method vs Abstract Factory?

**Factory Method** generally creates a product through a creation method and polymorphism.

**Abstract Factory** creates a family of related products.

---

## Q4. Builder vs Factory?

**Builder** controls how a complex object is constructed.

**Factory** decides which concrete object should be created.

---

## Q5. Prototype vs Factory?

Factory creates an object according to creation logic.

Prototype creates an object by copying an existing initialized object.

---

## Q6. Why is Builder useful in Java?

It is useful for objects with many optional parameters and supports readable, controlled construction.

---

## Q7. Why is Singleton controversial?

Because it introduces global state and can hide dependencies, making testing and maintenance more difficult if used unnecessarily.

---

## Q8. What is shallow copy vs deep copy?

**Shallow copy:** referenced objects may still be shared.

**Deep copy:** referenced objects are copied as well.

---

## Q9. Is Object Pool one of the GoF five creational patterns?

No. The classic GoF list has five creational patterns. SourceMaking additionally lists Object Pool as a creational pattern.

---

# 23. Final Mental Model

Remember these six words:

```text
Singleton       -> ONE
Factory Method  -> WHICH
Abstract Factory-> FAMILY
Builder         -> BUILD
Prototype       -> COPY
Object Pool     -> REUSE
```

If you can explain these six ideas with a Java example and explain **why** you would choose one over another, you have a strong foundation for Creational Design Patterns in LLD interviews.

---

# 24. Reference Material

1. **GeeksforGeeks — Creational Design Patterns**  
   https://www.geeksforgeeks.org/system-design/creational-design-pattern/

2. **SourceMaking — Creational Patterns**  
   https://sourcemaking.com/design_patterns/creational_patterns

---

## Study Recommendation

For LLD interview preparation, study the patterns in this order:

```text
1. Factory Method
       ↓
2. Abstract Factory
       ↓
3. Builder
       ↓
4. Singleton
       ↓
5. Prototype
       ↓
6. Object Pool
```

For each pattern, practice answering these four questions:

1. **What problem does it solve?**
2. **What is the structure?**
3. **Can I implement it in Java?**
4. **When should I NOT use it?**
