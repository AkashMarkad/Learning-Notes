# Table of Contents

- [1. What Are Structural Design Patterns?](#1-what-are-structural-design-patterns)
- [2. Why Do We Need Structural Patterns?](#2-why-do-we-need-structural-patterns)
- [3. Types of Structural Design Patterns](#3-types-of-structural-design-patterns)
- [4. Quick Comparison](#4-quick-comparison)
- [5. Adapter Pattern](#5-adapter-pattern)
  - [5.1 Definition](#51-definition)
  - [5.2 Problem](#52-problem)
  - [5.3 Structure](#53-structure)
  - [5.4 Java Implementation](#54-java-implementation)
  - [5.5 Object Adapter vs Class Adapter](#55-object-adapter-vs-class-adapter)
  - [5.6 Advantages, Disadvantages, and Use Cases](#56-advantages-disadvantages-and-use-cases)
- [6. Bridge Pattern](#6-bridge-pattern)
  - [6.1 Definition](#61-definition)
  - [6.2 Problem](#62-problem)
  - [6.3 Structure](#63-structure)
  - [6.4 Java Implementation](#64-java-implementation)
  - [6.5 Advantages, Disadvantages, and Use Cases](#65-advantages-disadvantages-and-use-cases)
- [7. Composite Pattern](#7-composite-pattern)
  - [7.1 Definition](#71-definition)
  - [7.2 Problem](#72-problem)
  - [7.3 Structure](#73-structure)
  - [7.4 Java Implementation](#74-java-implementation)
  - [7.5 Advantages, Disadvantages, and Use Cases](#75-advantages-disadvantages-and-use-cases)
- [8. Decorator Pattern](#8-decorator-pattern)
  - [8.1 Definition](#81-definition)
  - [8.2 Problem](#82-problem)
  - [8.3 Structure](#83-structure)
  - [8.4 Java Implementation](#84-java-implementation)
  - [8.5 Decorator vs Inheritance](#85-decorator-vs-inheritance)
  - [8.6 Advantages, Disadvantages, and Use Cases](#86-advantages-disadvantages-and-use-cases)
- [9. Facade Pattern](#9-facade-pattern)
  - [9.1 Definition](#91-definition)
  - [9.2 Problem](#92-problem)
  - [9.3 Structure](#93-structure)
  - [9.4 Java Implementation](#94-java-implementation)
  - [9.5 Advantages, Disadvantages, and Use Cases](#95-advantages-disadvantages-and-use-cases)
- [10. Flyweight Pattern](#10-flyweight-pattern)
  - [10.1 Definition](#101-definition)
  - [10.2 Intrinsic vs Extrinsic State](#102-intrinsic-vs-extrinsic-state)
  - [10.3 Problem](#103-problem)
  - [10.4 Structure](#104-structure)
  - [10.5 Java Implementation](#105-java-implementation)
  - [10.6 Advantages, Disadvantages, and Use Cases](#106-advantages-disadvantages-and-use-cases)
- [11. Proxy Pattern](#11-proxy-pattern)
  - [11.1 Definition](#111-definition)
  - [11.2 Problem](#112-problem)
  - [11.3 Structure](#113-structure)
  - [11.4 Java Implementation — Virtual Proxy](#114-java-implementation--virtual-proxy)
  - [11.5 Common Types of Proxy](#115-common-types-of-proxy)
  - [11.6 Advantages, Disadvantages, and Use Cases](#116-advantages-disadvantages-and-use-cases)
- [12. Private Class Data — Additional Pattern](#12-private-class-data--additional-pattern)
- [13. Similar Structural Patterns: How to Distinguish Them](#13-similar-structural-patterns-how-to-distinguish-them)
- [14. Structural Patterns in Java and Spring](#14-structural-patterns-in-java-and-spring)
- [15. Combining Structural Patterns](#15-combining-structural-patterns)
- [16. Advantages of Structural Patterns](#16-advantages-of-structural-patterns)
- [17. Disadvantages and Common Mistakes](#17-disadvantages-and-common-mistakes)
- [18. Interview-Oriented Decision Tree](#18-interview-oriented-decision-tree)
- [19. One-Page Cheat Sheet](#19-one-page-cheat-sheet)
- [20. Most Important Interview Questions](#20-most-important-interview-questions)
- [21. Final Mental Model](#21-final-mental-model)
- [22. Reference Material](#22-reference-material)
- [Study Recommendation](#study-recommendation)

---

# Structural Design Patterns — Complete Notes

> **Purpose:** Interview-friendly notes for understanding and implementing Structural Design Patterns in Java, with practical examples and comparison tables.
>
> **Primary references:**
> - GeeksforGeeks — [Structural Design Patterns](https://www.geeksforgeeks.org/system-design/structural-design-patterns/)
> - SourceMaking — [Structural Patterns](https://sourcemaking.com/design_patterns/structural_patterns)

---

# 1. What Are Structural Design Patterns?

**Structural Design Patterns describe how classes and objects are combined to form larger structures while keeping the system flexible, understandable, and maintainable.**

A Java application usually contains many objects. These objects need to collaborate, but direct dependencies can make the code difficult to extend. Structural patterns provide reusable ways to connect objects, adapt interfaces, simplify subsystems, create object trees, add responsibilities, or control access.

For example, an application may need to:

- Integrate a legacy payment API whose method names do not match the application's interface.
- Support both SMS and email delivery for several kinds of notifications.
- Represent folders containing files and other folders.
- Add logging or compression around an existing service without modifying it.
- Hide several subsystem calls behind one simple operation.
- Render thousands of map markers without duplicating their common visual data.
- Delay the creation of an expensive object until it is actually needed.

Structural patterns focus on **composition and relationships**, rather than primarily on creating objects or defining communication algorithms.

### Core idea

Without a deliberate structure, client code can become tightly coupled to concrete classes and internal details:

```text
Client
  |
  +----> Concrete implementation A
  +----> Concrete implementation B
  +----> Concrete implementation C
```

A structural pattern introduces an intentional relationship or abstraction:

```text
Client
  |
  v
Useful abstraction / wrapper / composition
  |
  +----> Existing or collaborating objects
```

The precise arrangement depends on the problem. An Adapter changes the interface seen by the client, a Facade simplifies subsystem access, and a Decorator adds behavior while preserving an interface.

### Structural patterns in the GoF classification

The classic *Gang of Four (GoF)* catalog has **seven structural patterns**:

1. Adapter
2. Bridge
3. Composite
4. Decorator
5. Facade
6. Flyweight
7. Proxy

SourceMaking also catalogs **Private Class Data** as an additional structural pattern. It is covered separately in these notes and is not one of the classic seven GoF structural patterns.

---

# 2. Why Do We Need Structural Patterns?

Structural patterns help when the arrangement of classes or objects becomes a design problem.

## 2.1 Reduce coupling

The client can depend on a stable interface instead of knowing every concrete implementation.

## 2.2 Make incompatible APIs work together

An Adapter lets new code use a legacy or third-party component without changing that component's source code.

## 2.3 Support independent variation

A Bridge separates two dimensions of change, such as notification type and delivery channel. Adding a new channel does not require creating a subclass for every notification type.

## 2.4 Represent hierarchical data naturally

Composite makes a single item and a group of items usable through the same component abstraction.

## 2.5 Add optional behavior without subclass explosion

Decorator wraps an object with additional behavior, such as compression, logging, retries, or notification channels.

## 2.6 Simplify complex subsystems

Facade provides a clear entry point for callers that should not need to understand every subsystem class.

## 2.7 Improve memory efficiency

Flyweight shares common, intrinsic data when a very large number of similar objects would otherwise duplicate it.

## 2.8 Control access or object lifecycle

Proxy places an intermediary in front of another object to support lazy initialization, access checks, caching, remote access, or monitoring.

> **Important:** Design patterns are tools, not goals. Use one when it makes a real change easier to manage. For a simple problem, direct composition or a small method may be clearer than introducing multiple classes.

---

# 3. Types of Structural Design Patterns

| Pattern | Main Idea | Typical Question |
|---|---|---|
| **Adapter** | Convert one interface into another that the client expects | How can these incompatible APIs work together? |
| **Bridge** | Separate abstraction from implementation so both can vary independently | How do I avoid subclass combinations across two dimensions? |
| **Composite** | Treat individual objects and groups uniformly | How do I represent part-whole trees? |
| **Decorator** | Add responsibilities dynamically by wrapping an object | How can I add optional behavior without changing the original class? |
| **Facade** | Provide a simplified interface to a subsystem | How can the client perform a complex task with one clear entry point? |
| **Flyweight** | Share common state between many fine-grained objects | How can I reduce memory use for many similar objects? |
| **Proxy** | Stand in for another object and control access to it | How can I add indirection before reaching the real object? |
| **Private Class Data** *(additional)* | Encapsulate data to restrict direct mutation/access | How can an object's data be made harder to modify accidentally? |

### High-level classification

```text
Structural Patterns
|
+-- Interface compatibility
|   +-- Adapter
|
+-- Separate variation dimensions
|   +-- Bridge
|
+-- Object composition
|   +-- Composite
|   +-- Decorator
|
+-- Simplified access to a subsystem
|   +-- Facade
|
+-- Sharing and controlled access
|   +-- Flyweight
|   +-- Proxy
|
+-- Additional catalog pattern
    +-- Private Class Data
```

---

# 4. Quick Comparison

| Pattern | Main Problem Solved | Key Mechanism | Main Benefit |
|---|---|---|---|
| Adapter | An existing API has the wrong interface | Wrap and translate calls | Compatibility |
| Bridge | Two dimensions of a design change independently | Composition between abstraction and implementation | Independent extension |
| Composite | Individual objects and groups should be treated alike | Tree of common components | Uniform tree operations |
| Decorator | Optional responsibilities create too many subclasses | Same-interface wrappers | Runtime behavior composition |
| Facade | Callers must coordinate many subsystem classes | One high-level entry point | Simplicity and reduced coupling |
| Flyweight | Many objects duplicate the same state | Share immutable intrinsic state | Lower memory consumption |
| Proxy | Access to the real object needs control or indirection | Surrogate implementing the same interface | Lazy loading, security, caching, or remote access |
| Private Class Data *(additional)* | Data is too easy to mutate or expose | Encapsulated data object and restricted API | Stronger encapsulation |

### Easy memory trick

```text
Adapter    -> TRANSLATE the interface
Bridge     -> SEPARATE two dimensions
Composite  -> TREE of objects
Decorator  -> ADD behavior by wrapping
Facade     -> SIMPLIFY a subsystem
Flyweight  -> SHARE common state
Proxy      -> CONTROL access
Private Class Data -> RESTRICT data access
```

---

# 5. Adapter Pattern

## 5.1 Definition

**Adapter converts the interface of an existing class into another interface that a client expects, allowing otherwise incompatible classes to collaborate.**

Adapter is also commonly called a **wrapper**, though not every wrapper is an Adapter. Its defining purpose is interface compatibility.

### Real-world analogy

A travel plug adapter allows a plug with one physical format to fit a different socket format. The adapter does not change the original plug or socket; it translates the connection between them.

### When the idea appears in software

- Connecting a legacy payment library to a new payment interface.
- Integrating a third-party SDK with application-specific interfaces.
- Normalizing multiple vendor APIs behind one internal contract.
- Converting a legacy response object into a format expected by modern code.

## 5.2 Problem

Suppose application code expects this interface:

```java
interface PaymentProcessor {
    void pay(long amountInPaise);
}
```

But an old library exposes a different API:

```java
class LegacyPaymentApi {
    public void makePayment(long amountInPaise) {
        System.out.println("Legacy API charged: " + amountInPaise + " paise");
    }
}
```

The client cannot use `LegacyPaymentApi` through `PaymentProcessor` directly because the method names and contracts differ. Modifying a third-party or legacy library may be impossible or unsafe.

## 5.3 Structure

```text
Client
  |
  v
PaymentProcessor (Target interface)
  ^
  |
PaymentAdapter (Adapter)
  |
  v
LegacyPaymentApi (Adaptee)
```

**Participants:**

- **Target:** Interface that the client expects.
- **Adaptee:** Existing class with an incompatible interface.
- **Adapter:** Implements the Target and delegates to the Adaptee, translating calls or data as required.
- **Client:** Uses the Target interface without knowing the Adaptee's details.

## 5.4 Java Implementation

### Target interface

```java
public interface PaymentProcessor {
    void pay(long amountInPaise);
}
```

### Existing class (Adaptee)

```java
public class LegacyPaymentApi {

    public void makePayment(long amountInPaise) {
        System.out.println("Legacy API charged: " + amountInPaise + " paise");
    }
}
```

### Adapter

```java
public class PaymentAdapter implements PaymentProcessor {

    private final LegacyPaymentApi legacyPaymentApi;

    public PaymentAdapter(LegacyPaymentApi legacyPaymentApi) {
        this.legacyPaymentApi = legacyPaymentApi;
    }

    @Override
    public void pay(long amountInPaise) {
        // Translate the Target operation to the Adaptee operation.
        legacyPaymentApi.makePayment(amountInPaise);
    }
}
```

### Client

```java
public class PaymentClient {

    public static void main(String[] args) {
        PaymentProcessor processor =
                new PaymentAdapter(new LegacyPaymentApi());

        processor.pay(25_000L);
    }
}
```

The client depends only on `PaymentProcessor`. If the legacy library is replaced, the application-facing contract can remain the same while the adapter implementation changes.

### Important implementation detail

An adapter may do more than rename a method. It can translate units, map request/response objects, convert exceptions, supply defaults, or normalize error codes. Those translations should be explicit and tested; do not hide incompatible semantics behind a superficially compatible method signature.

## 5.5 Object Adapter vs Class Adapter

| Feature | Object Adapter | Class Adapter |
|---|---|---|
| Mechanism | Composition: holds an Adaptee object | Inheritance: inherits from Target/Adaptee roles |
| Coupling | Usually lower | Usually tighter to a particular class |
| Existing subclasses | Can often wrap different Adaptee implementations | Typically tied to the class hierarchy chosen |
| Java suitability | Common and natural | Multiple class inheritance is not supported in Java, so the classic form is constrained |
| Flexibility | Can swap the wrapped object at runtime | Less flexible when the adapted class is fixed by inheritance |

In Java, **object adapters are the usual choice**: implement the Target interface and hold a reference to the Adaptee.

## 5.6 Advantages, Disadvantages, and Use Cases

### Advantages

- Reuses existing or third-party classes without changing their source.
- Separates compatibility logic from application business logic.
- Lets the client depend on a stable target interface.
- Makes it easier to replace one vendor integration with another.

### Disadvantages

- Adds another class and level of indirection.
- A poor adapter can conceal important semantic differences.
- One adapter may become overloaded if it tries to normalize too many unrelated responsibilities.

### When to use

Use Adapter when a useful class already exists, but its interface or data contract does not match what the client needs.

### When not to use

Do not add an Adapter merely to wrap a class without changing or isolating its interface. If the existing interface is already appropriate and under your control, direct use may be simpler.

### Interview memory line

**Adapter makes already-designed, incompatible interfaces work together.**

---

# 6. Bridge Pattern

## 6.1 Definition

**Bridge separates an abstraction from its implementation so that both can vary independently.**

Bridge replaces an inheritance-heavy design with composition between two hierarchies or dimensions.

### Real-world example

Imagine multiple notification types—normal notifications and urgent alerts—and multiple delivery channels—email and SMS. If each combination becomes a subclass, the number of classes grows quickly. Bridge models notification type and delivery mechanism separately.

## 6.2 Problem

A rigid inheritance solution could create classes such as:

```text
EmailNormalNotification
SmsNormalNotification
EmailUrgentNotification
SmsUrgentNotification
PushNormalNotification
PushUrgentNotification
...
```

Adding a new notification type or delivery channel creates additional combinations. This is the **subclass explosion** problem.

Bridge separates the two dimensions:

```text
Notification type  --->  MessageSender implementation
```

Now a new sender can be combined with existing notification types without creating one class for every pair.

## 6.3 Structure

```text
Abstraction --------------------> Implementor
    ^                                  ^
    |                                  |
RefinedAbstraction              ConcreteImplementorA
                                   ConcreteImplementorB
```

**Participants:**

- **Abstraction:** High-level API used by the client; holds a reference to the Implementor.
- **Refined Abstraction:** Extends the high-level abstraction with specialized behavior.
- **Implementor:** Interface for low-level operations.
- **Concrete Implementor:** Provides a concrete low-level implementation.

Do not confuse the words *abstraction* and *interface*. The Abstraction is the high-level role in the pattern; it can itself be an abstract class or an interface.

## 6.4 Java Implementation

### Implementor

```java
public interface MessageSender {
    void sendMessage(String recipient, String message);
}
```

### Concrete Implementors

```java
public class EmailSender implements MessageSender {

    @Override
    public void sendMessage(String recipient, String message) {
        System.out.println("Email to " + recipient + ": " + message);
    }
}
```

```java
public class SmsSender implements MessageSender {

    @Override
    public void sendMessage(String recipient, String message) {
        System.out.println("SMS to " + recipient + ": " + message);
    }
}
```

### Abstraction

```java
public abstract class Notification {

    protected final MessageSender sender;

    protected Notification(MessageSender sender) {
        this.sender = sender;
    }

    public abstract void notifyUser(String recipient, String message);
}
```

### Refined Abstractions

```java
public class BasicNotification extends Notification {

    public BasicNotification(MessageSender sender) {
        super(sender);
    }

    @Override
    public void notifyUser(String recipient, String message) {
        sender.sendMessage(recipient, message);
    }
}
```

```java
public class UrgentNotification extends Notification {

    public UrgentNotification(MessageSender sender) {
        super(sender);
    }

    @Override
    public void notifyUser(String recipient, String message) {
        sender.sendMessage(recipient, "URGENT: " + message);
    }
}
```

### Client

```java
MessageSender emailSender = new EmailSender();
MessageSender smsSender = new SmsSender();

Notification normalEmail = new BasicNotification(emailSender);
Notification urgentSms = new UrgentNotification(smsSender);

normalEmail.notifyUser("user@example.com", "Your order was shipped");
urgentSms.notifyUser("+911234567890", "Account security alert");
```

The notification type and transport can evolve independently. Another sender, such as a push-notification implementation, can be introduced without creating a separate class for every notification type.

## 6.5 Advantages, Disadvantages, and Use Cases

### Advantages

- Prevents subclass explosion across independent dimensions.
- Lets abstractions and implementations evolve separately.
- Supports switching the implementation through composition.
- Can improve testability by injecting a fake implementor.

### Disadvantages

- Introduces additional interfaces and indirection.
- May be unnecessary when there is only one stable dimension.
- Requires the designer to identify the independent variation dimensions correctly.

### When to use

Use Bridge when **two or more dimensions are expected to vary independently**, such as report format and output destination, device and controller, or notification type and delivery channel.

### Adapter vs Bridge: the key distinction

- **Adapter:** Usually introduced after interfaces already exist and do not fit together.
- **Bridge:** Usually designed up front to separate independent dimensions and avoid tight coupling.

### Interview memory line

**Bridge separates what an abstraction does from how its implementation performs it.**

---

# 7. Composite Pattern

## 7.1 Definition

**Composite composes objects into tree structures to represent part-whole hierarchies and allows clients to treat individual objects and groups uniformly.**

Composite is useful when a domain naturally contains objects made up of other objects.

### Common examples

- File systems: files and directories.
- Organization charts: employees and departments.
- Product categories: individual products and nested categories.
- UI components: controls and containers.
- Graphics: shapes and groups of shapes.

## 7.2 Problem

Consider a file system where a file has a size, while a directory contains files and other directories. Without a shared abstraction, client code may need separate logic for files and directories every time it calculates size, displays a tree, or performs a traversal.

Composite gives both individual objects and containers a shared contract. The container delegates operations to its children recursively.

## 7.3 Structure

```text
              Component
              /       \
             /         \
          Leaf       Composite
                       |
                       +---- child Component
                       +---- child Component
                       +---- child Component
```

**Participants:**

- **Component:** Common interface for both leaves and composites.
- **Leaf:** Represents an individual item and has no children.
- **Composite:** Stores child Components and implements operations by delegating to them.
- **Client:** Uses the common Component interface.

## 7.4 Java Implementation

The following simplified example models a file system. It focuses on structure rather than real filesystem I/O, permissions, symbolic links, or error handling.

### Component

```java
public interface FileSystemNode {
    String name();
    long sizeInBytes();
    void print(String indent);
}
```

### Leaf

```java
public class FileNode implements FileSystemNode {

    private final String name;
    private final long sizeInBytes;

    public FileNode(String name, long sizeInBytes) {
        if (sizeInBytes < 0) {
            throw new IllegalArgumentException("File size cannot be negative");
        }
        this.name = name;
        this.sizeInBytes = sizeInBytes;
    }

    @Override
    public String name() {
        return name;
    }

    @Override
    public long sizeInBytes() {
        return sizeInBytes;
    }

    @Override
    public void print(String indent) {
        System.out.println(indent + "File: " + name + " (" + sizeInBytes + " bytes)");
    }
}
```

### Composite

```java
import java.util.ArrayList;
import java.util.List;

public class DirectoryNode implements FileSystemNode {

    private final String name;
    private final List<FileSystemNode> children = new ArrayList<>();

    public DirectoryNode(String name) {
        this.name = name;
    }

    public void add(FileSystemNode child) {
        if (child == null) {
            throw new IllegalArgumentException("Child cannot be null");
        }
        children.add(child);
    }

    public boolean remove(FileSystemNode child) {
        return children.remove(child);
    }

    @Override
    public String name() {
        return name;
    }

    @Override
    public long sizeInBytes() {
        long total = 0;
        for (FileSystemNode child : children) {
            total = Math.addExact(total, child.sizeInBytes());
        }
        return total;
    }

    @Override
    public void print(String indent) {
        System.out.println(indent + "Directory: " + name
                + " (" + sizeInBytes() + " bytes)");
        for (FileSystemNode child : children) {
            child.print(indent + "  ");
        }
    }
}
```

### Client

```java
public class CompositeDemo {

    public static void main(String[] args) {
        FileNode readme = new FileNode("README.md", 1200);
        FileNode notes = new FileNode("notes.txt", 3500);

        DirectoryNode docs = new DirectoryNode("docs");
        docs.add(readme);
        docs.add(notes);

        FileNode app = new FileNode("app.jar", 8500);
        DirectoryNode root = new DirectoryNode("project");
        root.add(docs);
        root.add(app);

        root.print("");
        System.out.println("Total size: " + root.sizeInBytes() + " bytes");
    }
}
```

The client can call `sizeInBytes()` on either a `FileNode` or a `DirectoryNode` using the same `FileSystemNode` type.

### A design choice: transparent vs safe Composite

There are two common API approaches:

- **Transparent Composite:** The common Component declares child-management methods such as `add()` and `remove()`. Client code can use one API, but leaves may need to reject child operations.
- **Safe Composite:** Only Composite exposes child-management methods. The type system prevents clients from adding children to a leaf, but some operations require knowing whether a node is a composite.

Neither choice is universally correct. Choose the API that best balances uniform use and type safety for the domain.

## 7.5 Advantages, Disadvantages, and Use Cases

### Advantages

- Models recursive part-whole structures naturally.
- Lets clients operate on leaves and groups through one abstraction.
- Makes recursive operations—such as traversal and aggregation—straightforward.
- Supports adding new leaf types without rewriting client traversal logic, when they honor the Component contract.

### Disadvantages

- A broad Component interface can be difficult to design without exposing operations that do not make sense for every node.
- Validation rules can become complex for recursive structures.
- Cycles must be prevented if the domain is intended to be a tree.
- Recursive traversal may need depth limits or iterative handling for very deep structures.

### When to use

Use Composite when the domain contains **nested groups and individual elements that should be treated uniformly**.

### Interview memory line

**Composite is the tree pattern: one item and a group of items share the same interface.**

---

# 8. Decorator Pattern

## 8.1 Definition

**Decorator attaches additional responsibilities to an object dynamically by wrapping it with another object that follows the same interface.**

Decorator provides a flexible alternative to creating a subclass for every possible combination of features.

### Everyday analogy

A basic coffee can be combined with milk, then syrup, then another topping. Each addition wraps or extends the behavior/cost of the existing item without requiring a new class for every possible combination.

## 8.2 Problem

Suppose an application has a `Notifier` interface and needs these combinations:

- Email only
- Email plus SMS
- Email plus Slack
- Email plus SMS plus Slack

A subclass for every combination grows rapidly. Adding new delivery channels makes this worse. Decorator builds the combination by composing wrappers at runtime.

## 8.3 Structure

```text
              Component interface
                      ^
                      |
          +-----------+-----------+
          |                       |
    ConcreteComponent        Decorator (wraps Component)
                                  ^
                                  |
                        +---------+---------+
                        |                   |
                   SmsDecorator        SlackDecorator
```

**Participants:**

- **Component:** Shared interface.
- **Concrete Component:** Base object being decorated.
- **Decorator:** Implements the same Component interface and holds another Component.
- **Concrete Decorator:** Adds a responsibility before or after delegating to the wrapped object.

## 8.4 Java Implementation

### Component and base implementation

```java
public interface Notifier {
    void send(String message);
}
```

```java
public class EmailNotifier implements Notifier {

    @Override
    public void send(String message) {
        System.out.println("Sending email: " + message);
    }
}
```

### Base Decorator

```java
public abstract class NotifierDecorator implements Notifier {

    protected final Notifier wrapped;

    protected NotifierDecorator(Notifier wrapped) {
        if (wrapped == null) {
            throw new IllegalArgumentException("Wrapped notifier cannot be null");
        }
        this.wrapped = wrapped;
    }
}
```

### Concrete Decorators

```java
public class SmsDecorator extends NotifierDecorator {

    public SmsDecorator(Notifier wrapped) {
        super(wrapped);
    }

    @Override
    public void send(String message) {
        wrapped.send(message);
        System.out.println("Sending SMS: " + message);
    }
}
```

```java
public class SlackDecorator extends NotifierDecorator {

    public SlackDecorator(Notifier wrapped) {
        super(wrapped);
    }

    @Override
    public void send(String message) {
        wrapped.send(message);
        System.out.println("Sending Slack message: " + message);
    }
}
```

### Client

```java
Notifier notifier = new SlackDecorator(
        new SmsDecorator(
                new EmailNotifier()));

notifier.send("Your order has shipped");
```

Output is conceptually:

```text
Sending email: Your order has shipped
Sending SMS: Your order has shipped
Sending Slack message: Your order has shipped
```

The order of wrappers matters because decorators run in a nesting order. In other designs, a decorator may transform input or output, rather than simply triggering an additional action.

## 8.5 Decorator vs Inheritance

| Feature | Inheritance | Decorator |
|---|---|---|
| Behavior chosen | Usually encoded by subtype | Can be assembled at runtime |
| Feature combinations | May produce many subclasses | Can compose wrappers |
| Add/remove behavior | Usually requires another subtype/object | Can wrap or unwrap components where supported |
| Object identity | New subtype instance | Wrapper and wrapped object are distinct objects |
| Complexity | Simple for a few fixed variants | More flexible but adds layers |

## 8.6 Advantages, Disadvantages, and Use Cases

### Advantages

- Adds responsibilities without modifying the original class.
- Supports runtime composition of independent behaviors.
- Often follows the Open/Closed Principle by extending through wrappers.
- Keeps optional features separate from the core component.

### Disadvantages

- Many nested wrappers can make debugging harder.
- Order may affect the result.
- A client that relies on a concrete implementation rather than the Component interface may not work with a decorated object.
- Inspecting or unwrapping a decorator chain can be awkward if the design exposes no suitable API.

### When to use

Use Decorator when optional, combinable responsibilities should be added around an object and you want to avoid an explosion of subclasses.

### Common examples

- Java I/O stream wrappers, such as `BufferedInputStream` wrapping another `InputStream`.
- Adding metrics, logging, compression, encryption, or retry behavior around a component, provided the wrapping semantics are appropriate.
- Adding extra notification channels around a base notifier.

### Interview memory line

**Decorator keeps the same interface and adds responsibilities through wrapping.**

---

# 9. Facade Pattern

## 9.1 Definition

**Facade provides a unified, simplified interface to a set of interfaces in a subsystem.**

A Facade hides orchestration details from clients that do not need to know every low-level step. It does not necessarily remove or replace the subsystem's existing interfaces; clients that need advanced control can still use those interfaces if the architecture allows it.

## 9.2 Problem

Placing an order might require the client to understand this sequence:

1. Check inventory.
2. Authorize payment.
3. Create a shipment.
4. Return an order confirmation.

If every client coordinates these services independently, orchestration logic is duplicated and callers are tightly coupled to internal subsystem details.

## 9.3 Structure

```text
Client
  |
  v
OrderFacade
  |       |          |
  v       v          v
Inventory Payment   Shipping
 Service  Service   Service
```

**Participants:**

- **Facade:** Presents a high-level entry point and coordinates calls.
- **Subsystem classes:** Perform the detailed work.
- **Client:** Uses the Facade for common workflows rather than coordinating every class itself.

## 9.4 Java Implementation

### Subsystem classes

```java
public class InventoryService {

    public boolean hasStock(String productId, int quantity) {
        System.out.println("Checking inventory for " + productId);
        return quantity > 0;
    }
}
```

```java
public class PaymentService {

    public boolean charge(String customerId, long amountInPaise) {
        System.out.println("Charging customer " + customerId
                + " amount " + amountInPaise + " paise");
        return amountInPaise > 0;
    }
}
```

```java
public class ShippingService {

    public String createShipment(String productId, int quantity) {
        System.out.println("Creating shipment for " + productId);
        return "SHIP-1001";
    }
}
```

### Facade

```java
public class OrderFacade {

    private final InventoryService inventoryService;
    private final PaymentService paymentService;
    private final ShippingService shippingService;

    public OrderFacade(
            InventoryService inventoryService,
            PaymentService paymentService,
            ShippingService shippingService) {
        this.inventoryService = inventoryService;
        this.paymentService = paymentService;
        this.shippingService = shippingService;
    }

    public String placeOrder(
            String customerId,
            String productId,
            int quantity,
            long amountInPaise) {

        if (!inventoryService.hasStock(productId, quantity)) {
            throw new IllegalStateException("Product is unavailable");
        }

        if (!paymentService.charge(customerId, amountInPaise)) {
            throw new IllegalStateException("Payment failed");
        }

        return shippingService.createShipment(productId, quantity);
    }
}
```

### Client

```java
OrderFacade orderFacade = new OrderFacade(
        new InventoryService(),
        new PaymentService(),
        new ShippingService());

String shipmentId = orderFacade.placeOrder(
        "CUST-42", "BOOK-10", 2, 149_900L);

System.out.println("Shipment created: " + shipmentId);
```

The client makes one high-level call instead of directly coordinating three services.

> **Production note:** This small example illustrates structure only. Real order workflows need durable state transitions, idempotency, failure recovery, and usually compensation or a saga when multiple remote systems are involved. A Facade alone does not make a multi-step workflow transactional.

## 9.5 Advantages, Disadvantages, and Use Cases

### Advantages

- Reduces the amount of subsystem knowledge required by the client.
- Provides one clear entry point for a common workflow.
- Reduces dependencies between clients and subsystem implementation classes.
- Helps define architectural layers and module boundaries.

### Disadvantages

- Can become a “God object” if unrelated workflows accumulate in one Facade.
- May hide useful lower-level capabilities if the API is too restrictive.
- Does not automatically solve transaction management, distributed failures, or business consistency.

### When to use

Use Facade when multiple subsystem calls form a common workflow and clients should not be responsible for coordinating the details.

### Facade vs Adapter

- **Facade:** Creates a simpler, higher-level interface to a subsystem.
- **Adapter:** Converts an existing interface so that it matches a target interface expected by a client.

### Facade vs Mediator

Facade simplifies access to a subsystem and typically is not known by subsystem classes. Mediator centralizes communication among colleague objects, which know or refer to the Mediator. Mediator belongs to the behavioral pattern category.

### Interview memory line

**Facade offers one convenient front door to a complex subsystem.**

---

# 10. Flyweight Pattern

## 10.1 Definition

**Flyweight uses sharing to support a large number of fine-grained objects efficiently.**

It is most useful when a large number of objects contain repeated, relatively stable data. Common data is stored in shared Flyweight objects, while per-instance data is supplied externally.

## 10.2 Intrinsic vs Extrinsic State

This distinction is central to Flyweight.

| State | Meaning | Example for a map marker |
|---|---|---|
| **Intrinsic state** | Shared, reusable state stored in the Flyweight | Marker type, icon URL, color |
| **Extrinsic state** | Context-specific state supplied by the client | Latitude, longitude, label for one marker |

The Flyweight should not store per-instance data that differs for every use. Intrinsic state should generally be immutable (or otherwise safely managed) because multiple consumers share the same object.

## 10.3 Problem

A map may contain tens of thousands of markers. If each marker stores its own copy of the same icon and style metadata, memory use increases unnecessarily. Markers of the same type can share that common style while retaining their own coordinates.

Flyweight is most useful when:

- The application creates a large number of similar objects.
- Repeated state accounts for a meaningful portion of memory use.
- Shared state can be separated from per-object context.
- The application does not rely on each equivalent object's unique identity.

Do not apply Flyweight simply because two objects contain equal values. Measure the memory cost first: a cache or factory has its own overhead and complexity.

## 10.4 Structure

```text
Client / Context objects
   |        |
   |        +---- extrinsic state (coordinates, label)
   |
   v
FlyweightFactory
   |
   +---- shared Flyweight A
   +---- shared Flyweight B
   +---- shared Flyweight C
```

**Participants:**

- **Flyweight:** Holds shared intrinsic state and exposes operations that use it.
- **Concrete Flyweight:** Implements the shared behavior.
- **Flyweight Factory:** Reuses existing Flyweights or creates them when needed.
- **Context:** Stores the extrinsic state needed for each occurrence.

## 10.5 Java Implementation

### Shared Flyweight

```java
public final class MarkerStyle {

    private final String type;
    private final String iconUrl;
    private final String color;

    public MarkerStyle(String type, String iconUrl, String color) {
        this.type = type;
        this.iconUrl = iconUrl;
        this.color = color;
    }

    public String type() {
        return type;
    }

    public String iconUrl() {
        return iconUrl;
    }

    public String color() {
        return color;
    }

    public void renderAt(double latitude, double longitude, String label) {
        System.out.println(type + " marker at (" + latitude + ", " + longitude
                + ") label=" + label + ", icon=" + iconUrl + ", color=" + color);
    }
}
```

### Factory

```java
import java.util.concurrent.ConcurrentHashMap;
import java.util.concurrent.ConcurrentMap;

public class MarkerStyleFactory {

    private final ConcurrentMap<String, MarkerStyle> styles =
            new ConcurrentHashMap<>();

    public MarkerStyle getStyle(String type) {
        return styles.computeIfAbsent(type, this::createStyle);
    }

    private MarkerStyle createStyle(String type) {
        return switch (type) {
            case "RESTAURANT" ->
                    new MarkerStyle(type, "restaurant.png", "red");
            case "HOTEL" ->
                    new MarkerStyle(type, "hotel.png", "blue");
            case "HOSPITAL" ->
                    new MarkerStyle(type, "hospital.png", "green");
            default -> throw new IllegalArgumentException(
                    "Unknown marker type: " + type);
        };
    }
}
```

This example uses a switch expression, available in modern Java versions. For older Java versions, replace it with a traditional `switch` statement.

### Context

```java
public class MapMarker {

    private final double latitude;
    private final double longitude;
    private final String label;
    private final MarkerStyle style;

    public MapMarker(
            double latitude,
            double longitude,
            String label,
            MarkerStyle style) {
        this.latitude = latitude;
        this.longitude = longitude;
        this.label = label;
        this.style = style;
    }

    public void render() {
        style.renderAt(latitude, longitude, label);
    }

    // Included here so the demo can verify that the factory reuses styles.
    public MarkerStyle style() {
        return style;
    }
}
```

### Client

```java
MarkerStyleFactory factory = new MarkerStyleFactory();

MapMarker first = new MapMarker(
        18.5204, 73.8567, "Cafe A", factory.getStyle("RESTAURANT"));
MapMarker second = new MapMarker(
        18.5314, 73.8446, "Cafe B", factory.getStyle("RESTAURANT"));
MapMarker hospital = new MapMarker(
        18.5167, 73.8563, "City Hospital", factory.getStyle("HOSPITAL"));

first.render();
second.render();
hospital.render();

System.out.println(first.style() == second.style()); // true
```

Both restaurant markers obtain the same cached `MarkerStyle` object from the factory, while each `MapMarker` retains its own coordinates and label. The accessor is included to make the demo explicit; production code would normally verify sharing in a test or keep Flyweight internals package-private if clients do not need them.

## 10.6 Advantages, Disadvantages, and Use Cases

### Advantages

- Reduces duplicated memory for intrinsic state.
- Centralizes creation and reuse of shared values.
- Can be useful in graphics, maps, games, text rendering, and large domain models.

### Disadvantages

- Splitting intrinsic and extrinsic state adds design complexity.
- Shared mutable state can cause bugs and concurrency problems.
- A cache can grow without bound if keys are uncontrolled.
- Benefits may be small or negative for low object counts.

### When to use

Use Flyweight when profiling or domain characteristics show a large population of objects with substantial repeated state that can safely be shared.

### Interview memory line

**Flyweight saves memory by sharing intrinsic state and keeping extrinsic state outside the shared object.**

---

# 11. Proxy Pattern

## 11.1 Definition

**Proxy provides a surrogate or placeholder for another object to control access to that object.**

A Proxy usually follows the same interface as the real object so that the client can use either one without changing its main calling code. It adds a level of indirection before forwarding a request to the Real Subject.

## 11.2 Problem

Imagine a report viewer that can display large images. Loading every image when a screen opens may be expensive, even when many images are never viewed. A proxy can delay creation of the real image until `display()` is called.

Other situations include:

- Checking permissions before allowing a call.
- Caching the result of expensive reads.
- Representing an object on a remote server.
- Logging access or controlling resource lifecycle.

## 11.3 Structure

```text
               Subject interface
                 ^           ^
                 |           |
          RealSubject       Proxy
                               |
                               +----> RealSubject

Client calls either implementation through Subject.
```

**Participants:**

- **Subject:** Interface shared by the Proxy and Real Subject.
- **Real Subject:** Performs the actual work.
- **Proxy:** Controls access and may perform setup, checks, caching, or delegation.
- **Client:** Depends on the Subject interface.

## 11.4 Java Implementation — Virtual Proxy

### Subject

```java
public interface Image {
    void display();
}
```

### Real Subject

```java
public class RealImage implements Image {

    private final String filename;

    public RealImage(String filename) {
        this.filename = filename;
        loadFromDisk();
    }

    private void loadFromDisk() {
        System.out.println("Loading image from disk: " + filename);
    }

    @Override
    public void display() {
        System.out.println("Displaying image: " + filename);
    }
}
```

### Virtual Proxy

```java
public class ImageProxy implements Image {

    private final String filename;
    private RealImage realImage;

    public ImageProxy(String filename) {
        this.filename = filename;
    }

    @Override
    public void display() {
        if (realImage == null) {
            realImage = new RealImage(filename);
        }
        realImage.display();
    }
}
```

### Client

```java
Image image = new ImageProxy("large-photo.png");

System.out.println("Image object created; file not loaded yet.");
image.display(); // RealImage is created here
image.display(); // Reuses the already loaded RealImage
```

The proxy implements the same `Image` interface as `RealImage`. The client does not need to know whether it is dealing with the proxy or the real object.

> **Thread-safety note:** This minimal lazy-initialization example is not thread-safe if multiple threads call `display()` concurrently. A production implementation should use suitable synchronization or another safe initialization strategy if concurrent access is possible.

## 11.5 Common Types of Proxy

| Proxy Type | Purpose | Example Scenario |
|---|---|---|
| **Virtual Proxy** | Creates or loads an expensive object on demand | Large image loaded only when displayed |
| **Protection Proxy** | Enforces access control | Only an authorized role can call a protected operation |
| **Remote Proxy** | Represents an object in another process or address space | Client-side stub for a remote service |
| **Caching Proxy** | Stores results to avoid repeated expensive work | Cache repeated read requests |
| **Logging / Smart Reference Proxy** | Adds actions when an object is accessed | Audit calls, monitor usage, manage references |

These are common variants of the same broad idea; a proxy can combine more than one responsibility, but it should not become an uncontrolled collection of unrelated features.

## 11.6 Advantages, Disadvantages, and Use Cases

### Advantages

- Controls access without necessarily changing the Real Subject.
- Supports lazy initialization of expensive resources.
- Can add authorization, caching, monitoring, or remote-call behavior.
- Lets the client use a consistent interface.

### Disadvantages

- Adds indirection and may complicate debugging.
- Can introduce latency or stale cached data depending on implementation.
- Remote proxies must account for network failures, timeouts, retries, and partial failure.
- Authorization logic in a proxy must not be the only security boundary if callers can bypass it.

### When to use

Use Proxy when the main objective is to **control access to the real object** or provide a more sophisticated reference to it.

### Decorator vs Proxy

They often have similar structures because both wrap an object. Their intent differs:

- **Decorator:** Adds responsibilities or behavior to an object.
- **Proxy:** Controls access to, or stands in for, the real object.

### Interview memory line

**Proxy stands in front of the real object to manage access to it.**

---

# 12. Private Class Data — Additional Pattern

> **Classification note:** SourceMaking lists Private Class Data under structural patterns. It is not one of the classic seven GoF structural patterns, so treat it as an additional catalog pattern.

## Definition

**Private Class Data encapsulates an object's data in a dedicated data holder and limits the ways that data can be accessed or changed.** It is particularly relevant when a class should expose behavior but should not expose mutators for its internal configuration or state.

### Problem

A class with many mutable fields and public setters allows callers to place it in invalid states. It can also make it difficult to understand which operations are allowed to change its data.

### Basic structure

```text
Public domain object
       |
       +----> Private data holder
                 - private fields
                 - no public mutators
                 - controlled read access
```

### Java example

```java
public final class ServiceConfiguration {

    private final ConfigurationData data;

    public ServiceConfiguration(String endpoint, int timeoutMillis) {
        if (endpoint == null || endpoint.isBlank()) {
            throw new IllegalArgumentException("Endpoint is required");
        }
        if (timeoutMillis <= 0) {
            throw new IllegalArgumentException("Timeout must be positive");
        }
        this.data = new ConfigurationData(endpoint, timeoutMillis);
    }

    public String endpoint() {
        return data.endpoint();
    }

    public int timeoutMillis() {
        return data.timeoutMillis();
    }

    private static final class ConfigurationData {
        private final String endpoint;
        private final int timeoutMillis;

        private ConfigurationData(String endpoint, int timeoutMillis) {
            this.endpoint = endpoint;
            this.timeoutMillis = timeoutMillis;
        }

        private String endpoint() {
            return endpoint;
        }

        private int timeoutMillis() {
            return timeoutMillis;
        }
    }
}
```

The example is intentionally immutable: configuration is validated once, stored in a private holder, and exposed through read-only methods. The holder's separation makes the data boundary explicit, while `private`, `final`, and the lack of mutator methods restrict changes.

### Advantages

- Makes ownership and mutation boundaries clearer.
- Reduces the chance of accidental changes to critical internal data.
- Supports immutable design when data should not change after construction.

### Disadvantages

- Can introduce an extra class and forwarding methods.
- Adds little value if the class is already small, immutable, and well encapsulated.
- `private` fields alone do not automatically make referenced mutable objects immutable; defensive copies may be needed.

### When to use

Use this approach when data isolation and restricted mutation are genuine requirements—not merely to add another layer around ordinary fields.

---

# 13. Similar Structural Patterns: How to Distinguish Them

Several structural patterns use composition and can look similar in a diagram. In interviews, explain the **intent** first, then the structure.

## 13.1 Adapter vs Bridge

| Feature | Adapter | Bridge |
|---|---|---|
| Primary intent | Make incompatible interfaces work together | Separate abstraction from implementation |
| Typical timing | Often introduced after classes/APIs already exist | Usually planned as part of the design |
| Interface to client | Converts an existing interface to the expected Target | Preserves the abstraction while allowing implementation variation |
| Main question | “How can I use this class through my interface?” | “How can these two dimensions evolve separately?” |

**Rule of thumb:** Adapter makes existing pieces work together; Bridge structures a design so two dimensions can vary independently.

## 13.2 Adapter vs Facade

| Feature | Adapter | Facade |
|---|---|---|
| Main goal | Interface compatibility | Simplification of a subsystem |
| API | Adapts one interface to another expected interface | Defines a convenient higher-level interface |
| Typical scope | Often one class/API integration | Often coordinates multiple subsystem classes |
| Example | Wrap a legacy payment API as `PaymentProcessor` | `OrderFacade.placeOrder()` coordinates inventory, payment, and shipping |

An Adapter preserves access to an existing capability through a different interface. A Facade provides a convenient entry point to a larger subsystem.

## 13.3 Decorator vs Proxy

| Feature | Decorator | Proxy |
|---|---|---|
| Main goal | Add responsibilities | Control access to a subject |
| Same interface as wrapped object | Usually yes | Usually yes |
| Typical behavior | Adds logging, compression, or extra notification delivery | Lazy initialization, authorization, remote access, or caching |
| Client view | Intends to enhance behavior transparently | Intends to manage access or stand in for a real object |

Their structures may be nearly identical. Choose the name based on purpose, not merely on the fact that one object holds another.

## 13.4 Composite vs Decorator

| Feature | Composite | Decorator |
|---|---|---|
| Main goal | Represent part-whole hierarchies | Add responsibilities to one component |
| Child/reference structure | Usually holds zero or more child Components | Usually wraps one Component |
| Composition meaning | “Contains these items” | “Enhances this object” |
| Example | Directory containing files and directories | Notifier wrapped by an SMS decorator |

Both use recursive composition. Composite focuses on representing a tree; Decorator focuses on adding behavior. They can also be used together.

## 13.5 Facade vs Mediator

- **Facade (structural):** Simplifies the use of a subsystem for external clients. Subsystem classes generally do not communicate through the Facade.
- **Mediator (behavioral):** Centralizes interaction among colleague objects, which know the Mediator and use it to coordinate their communication.

## 13.6 Flyweight vs Singleton

- **Flyweight:** Shares many equivalent objects or intrinsic state where reuse can reduce memory consumption. There may be many Flyweight instances—one per distinct intrinsic-state key.
- **Singleton:** Ensures one instance of a particular class (with caveats around global state and lifecycle).

A Flyweight factory may cache and return shared objects, but Flyweight is not simply another name for Singleton.

## 13.7 Quick identification questions

```text
Are two interfaces incompatible?                 -> Adapter
Must two design dimensions vary independently?   -> Bridge
Is the domain a tree of items and groups?        -> Composite
Do you need optional behavior around an object?  -> Decorator
Should clients call one entry point for a subsystem? -> Facade
Are many objects duplicating common state?       -> Flyweight
Must access to the real object be controlled?    -> Proxy
Must object data be isolated from mutation?      -> Private Class Data (additional)
```

---

# 14. Structural Patterns in Java and Spring

Patterns are design ideas, not annotations or framework features by themselves. A framework may use a pattern internally, and an application may use a similar design without implementing the full textbook version.

| Pattern | Familiar Java / Spring example | Why it resembles the pattern |
|---|---|---|
| **Adapter** | `InputStreamReader` adapts byte-oriented `InputStream` data to the character-oriented `Reader` API | Bridges between incompatible levels of an API |
| **Decorator** | `BufferedInputStream` wrapping an `InputStream`; `BufferedReader` wrapping a `Reader` | Adds buffering while preserving the relevant stream/reader abstraction |
| **Proxy** | Spring AOP proxies used for features such as method interception and, in common configurations, transaction advice | Calls pass through an intermediary before reaching the target |
| **Facade** | SLF4J offers a logging facade over logging implementations; application service methods can provide facade-like entry points | A simpler API hides implementation/subsystem details |
| **Composite** | UI/container trees and hierarchical component models | Individual components and groups can be traversed or handled uniformly |
| **Flyweight** | Shared immutable symbols, styles, glyph data, or cached value objects in rendering systems | Common intrinsic data can be shared across many contexts |
| **Bridge** | A deliberately separated abstraction/implementation pair in a library or application | The two sides can vary independently through composition |

### Spring-specific note about Proxy

Features such as Spring declarative transactions and method-level advice often rely on proxies, depending on configuration and proxy type. Proxying has practical boundaries: self-invocation may bypass proxy advice, and final/private methods or classes can affect which proxy mechanisms are possible. Consult the relevant Spring version's documentation when relying on a specific proxy behavior.

### Be precise in interviews

A class having a wrapper or using composition does not automatically make it a textbook pattern. Explain the intent and the roles it plays. For example, SLF4J is commonly described as a logging facade; `BufferedInputStream` is a well-known Decorator-style example; and `InputStreamReader` is an Adapter-style example.

---

# 15. Combining Structural Patterns

Patterns can work together when each addresses a different concern.

## 15.1 Facade + Adapter

A Facade coordinates a workflow while Adapters normalize incompatible vendor APIs used inside that workflow.

```text
Client -> OrderFacade -> PaymentProcessor interface -> PaymentAdapter -> Vendor API
```

The Facade simplifies the workflow; the Adapter solves interface incompatibility.

## 15.2 Decorator + Proxy

A service may use a Proxy to enforce access rules and a Decorator to add metrics or formatting. Because their structures overlap, document each layer's purpose clearly and avoid unnecessary wrapping.

## 15.3 Composite + Decorator

A Composite models a tree, and Decorators can add behavior to individual nodes or to a whole composite. Ensure the added behavior makes sense at the level where the wrapper is placed.

## 15.4 Flyweight + Composite

A large tree can refer to shared Flyweight objects for repeated leaf attributes. This can reduce duplication, provided the shared state is separated from context-specific data.

## 15.5 Bridge + Adapter

A Bridge can define independent abstraction and implementation dimensions, while an Adapter can normalize a third-party implementation so it satisfies the Bridge's Implementor interface.

> Pattern combinations should emerge from distinct requirements. Do not add a second pattern merely because it can technically be combined with the first.

---

# 16. Advantages of Structural Patterns

- **Loose coupling:** Clients can depend on stable interfaces rather than concrete implementations.
- **Reuse:** Existing libraries and objects can be integrated or extended rather than rewritten.
- **Flexibility:** Composition allows behavior and implementation choices to change without redesigning every class.
- **Maintainability:** Responsibilities and subsystem boundaries can become clearer.
- **Extensibility:** New implementations, decorators, leaves, or adapters can often be added without modifying all clients.
- **Resource efficiency:** Flyweight can reduce duplicated memory when its assumptions are satisfied.
- **Clearer APIs:** Facade and Adapter can give clients an interface that fits their actual needs.

The benefits depend on choosing the appropriate pattern and keeping responsibilities focused.

---

# 17. Disadvantages and Common Mistakes

## 17.1 Overengineering

Introducing many interfaces and wrappers for a one-class problem can make code harder to understand. Start with the simplest design that meets the requirements.

## 17.2 Confusing similar patterns

Adapter, Facade, Decorator, and Proxy can all involve wrapper objects, but their intents are different. Choose based on the problem being solved.

## 17.3 Hiding business failures behind a Facade

A Facade simplifies access; it does not automatically guarantee atomicity, rollback, or distributed consistency. Model failure handling explicitly.

## 17.4 Leaking mutable state in Flyweight

A shared Flyweight should not retain per-request or per-user mutable data. Keep extrinsic state in the context and make shared intrinsic state immutable whenever possible.

## 17.5 Incorrect Composite contracts

A single interface should not promise unsupported operations without a clear policy. Decide deliberately between transparent and safe Composite APIs.

## 17.6 Overly deep decorator or proxy chains

Too many layers make the call path difficult to follow. Name wrappers clearly, keep them focused, and test order-sensitive behavior.

## 17.7 Assuming a proxy guarantees security

Authorization must be enforced at a trustworthy boundary. If code can bypass the proxy and call the underlying object directly, the proxy alone is not a sufficient security control.

## 17.8 Using Flyweight without measuring

Sharing adds a factory/cache and identity considerations. Confirm that duplicated intrinsic state is significant before introducing this optimization.

---

# 18. Interview-Oriented Decision Tree

Use this as a starting point when an interviewer describes a design problem.

```text
Start: What is difficult about the object structure?
|
+-- Existing API has the wrong interface
|      -> Adapter
|
+-- Two independent dimensions create too many subclasses
|      -> Bridge
|
+-- Data consists of individual elements and nested groups
|      -> Composite
|
+-- Need to add optional behavior by combining wrappers
|      -> Decorator
|
+-- Client must coordinate too many subsystem classes
|      -> Facade
|
+-- A huge number of objects duplicate common data
|      -> Flyweight
|
+-- Need a placeholder that controls access to the real object
|      -> Proxy
|
+-- Need a strict boundary around data mutation/access
       -> Private Class Data (additional)
```

### Four questions to ask before choosing a pattern

1. **What is changing or difficult?** Interface, dimensions, hierarchy, behavior, subsystem access, memory use, object access, or data mutation?
2. **Which role owns the responsibility?** Client, wrapper, factory, abstraction, component, or subsystem?
3. **What should stay stable?** The client-facing interface, the common Component, the high-level abstraction, or the domain contract?
4. **What is the cost?** Extra classes, runtime indirection, cache lifecycle, deeper call stacks, or more complicated tests?

---

# 19. One-Page Cheat Sheet

| Pattern | Remember | Typical Example |
|---|---|---|
| Adapter | Translate interface | Legacy payment API adapted to an internal payment contract |
| Bridge | Separate two changing dimensions | Notification type × delivery channel |
| Composite | Uniform tree | Files and directories |
| Decorator | Wrap to add behavior | Email notifier plus SMS/Slack behavior; Java I/O wrappers |
| Facade | One simpler entry point | Order workflow coordinating inventory, payment, and shipping |
| Flyweight | Share intrinsic state | Many map markers sharing icon/style metadata |
| Proxy | Control access to a subject | Lazy image loading or Spring AOP proxying |
| Private Class Data *(additional)* | Restrict access/mutation | Immutable configuration stored behind a private data holder |

### Memorize the intent, then the structure

```text
Adapter    = Target + Adapter + Adaptee
Bridge     = Abstraction + Implementor
Composite  = Component + Leaf + Composite
Decorator  = Component + Wrapped Component + Added Responsibility
Facade     = Client + Simple Entry Point + Subsystem
Flyweight  = Factory + Shared Intrinsic State + Extrinsic Context
Proxy      = Subject + Proxy + Real Subject
Private Class Data = Public behavior + private data holder
```

---

# 20. Most Important Interview Questions

## Q1. What are Structural Design Patterns?

Structural Design Patterns describe how objects and classes are composed into larger structures. They help manage dependencies, adapt interfaces, represent hierarchies, extend responsibilities, simplify subsystems, share state, and control access.

## Q2. Name the seven classic GoF structural patterns.

Adapter, Bridge, Composite, Decorator, Facade, Flyweight, and Proxy.

## Q3. Is Private Class Data one of the classic GoF structural patterns?

No. SourceMaking includes it in its structural-pattern catalog, but it is not one of the classic seven GoF structural patterns.

## Q4. What is the difference between Adapter and Decorator?

Adapter changes the interface through which a client accesses an object. Decorator normally preserves the Component interface and adds responsibilities around the wrapped object.

## Q5. What is the difference between Adapter and Facade?

Adapter makes an existing interface compatible with the interface the client expects. Facade provides a simpler, higher-level interface to a subsystem that may contain multiple classes.

## Q6. What is the difference between Bridge and Adapter?

Adapter is commonly used to make existing incompatible APIs work together. Bridge separates an abstraction from its implementation so both can vary independently, typically by design.

## Q7. How does Bridge prevent subclass explosion?

It separates independent variation dimensions and connects them through composition. Instead of creating a subclass for every combination, each side can be extended separately and combined at runtime.

## Q8. What is Composite used for?

It represents part-whole hierarchies as trees and allows clients to use individual objects and groups through the same Component interface. A file system is a common example.

## Q9. How does Decorator follow the Open/Closed Principle?

It can extend an object's behavior by introducing a wrapper that implements the same interface, rather than modifying the original class. This does not mean every decorator automatically makes an overall design perfect; wrapper order and compatibility still matter.

## Q10. Can multiple Decorators be chained?

Yes. Since each Decorator implements the same Component interface and wraps another Component, multiple decorators can be nested. The order may change the result.

## Q11. What is the difference between Decorator and Proxy?

Their structures are similar, but their intent differs. Decorator adds responsibilities; Proxy controls access to or stands in for the real object.

## Q12. What is the main purpose of Facade?

To provide a unified and simplified interface to a subsystem, reducing the amount of subsystem detail a client must know. It does not automatically provide transactionality or distributed rollback.

## Q13. Explain intrinsic and extrinsic state in Flyweight.

Intrinsic state is common state that can be shared among equivalent Flyweights. Extrinsic state is context-specific and is supplied by the client or stored in a separate context object.

## Q14. When should Flyweight be avoided?

Avoid it when the number of objects is small, common state is insignificant, or the shared/extrinsic split and caching overhead outweigh memory savings. Measure before optimizing.

## Q15. What are common types of Proxy?

Virtual, protection, remote, caching, and smart-reference/logging proxies. Each controls access or adds behavior around a Real Subject for a different reason.

## Q16. How is Proxy used in Spring?

Spring commonly uses proxies for method interception, including declarative transaction management and other AOP advice, depending on configuration. Proxy boundaries and self-invocation behavior are important details.

## Q17. Can Facade and Adapter be used together?

Yes. An Adapter can normalize one or more external APIs, while a Facade coordinates them behind a simpler workflow for clients.

## Q18. Are design patterns mandatory for clean code?

No. Patterns are vocabulary and reusable approaches, not requirements. Use them when they make responsibilities, change, and dependencies easier to manage.

### Suggested interview answer format

For any pattern, answer in this order:

1. **Intent:** One sentence describing the problem it solves.
2. **Structure:** Name the key participants and how they relate.
3. **Example:** Give one realistic use case.
4. **Trade-off:** Mention a limitation or reason not to use it.

---

# 21. Final Mental Model

Remember these seven verbs:

```text
Adapter    -> TRANSLATE
Bridge     -> SEPARATE
Composite  -> ORGANIZE AS A TREE
Decorator  -> ENHANCE
Facade     -> SIMPLIFY
Flyweight  -> SHARE
Proxy      -> CONTROL
```

And remember the additional SourceMaking catalog entry:

```text
Private Class Data -> RESTRICT DATA ACCESS / MUTATION
```

The most important interview distinction is **intent**. Adapter, Decorator, Facade, and Proxy may all be implemented with wrappers, but they solve different problems. State the problem first, then explain how the roles solve it.

If you can identify the problem, draw the relationships, implement a small Java example, and explain the trade-offs, you have a solid foundation for Structural Design Patterns in Low-Level Design (LLD) interviews.

---

# 22. Reference Material

1. **GeeksforGeeks — Structural Design Patterns**  
   https://www.geeksforgeeks.org/system-design/structural-design-patterns/

2. **SourceMaking — Structural Patterns**  
   https://sourcemaking.com/design_patterns/structural_patterns

### Scope note

These notes prioritize the seven classic GoF structural patterns, which align with the GeeksforGeeks overview. **Private Class Data** is included separately because SourceMaking lists it in its structural-pattern catalog. Explanations and Java examples are rewritten as study notes rather than copied passages.

---

## Study Recommendation

For LLD interview preparation, study the patterns in this order:

```text
1. Adapter
       ↓
2. Decorator
       ↓
3. Proxy
       ↓
4. Facade
       ↓
5. Composite
       ↓
6. Bridge
       ↓
7. Flyweight
       ↓
8. Private Class Data (additional)
```

This order groups some of the most easily confused wrapper patterns early, then moves to tree structures, independent variation, and memory optimization.

For every pattern, practice answering these four questions:

1. **What problem does it solve?**
2. **What is the structure and each participant's role?**
3. **Can I implement it in Java?**
4. **When should I not use it, and how is it different from a similar pattern?**
