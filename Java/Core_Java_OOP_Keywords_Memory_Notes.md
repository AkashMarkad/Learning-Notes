# Core Java Notes: OOP, Classes, Interfaces, Keywords, and Memory Management

> **Purpose:** A structured, interview-focused guide to Java
> fundamentals with clear explanations, diagrams, examples, comparisons,
> best practices, and revision questions.
>
> **Audience:** Java developers preparing for technical interviews or
> strengthening core Java knowledge.
>
> **Language baseline:** Modern Java syntax. Most examples work on Java
> 8+, unless a section says otherwise.

------------------------------------------------------------------------

## Table of Contents

1.  [Java and the JVM: Big Picture](#1-java-and-the-jvm-big-picture)
2.  [Class and Object](#2-class-and-object)
3.  [Constructors](#3-constructors)
4.  [The Four Pillars of OOP](#4-the-four-pillars-of-oop)
    -   Encapsulation
    -   Abstraction
    -   Inheritance
    -   Polymorphism
5.  [Abstract Classes](#5-abstract-classes)
6.  [Interfaces](#6-interfaces)
7.  [Abstract Class vs Interface](#7-abstract-class-vs-interface)
8.  [Composition, Association, Aggregation, and
    Dependency](#8-composition-association-aggregation-and-dependency)
9.  [Important Java Keywords](#9-important-java-keywords)
10. [Access Modifiers](#10-access-modifiers)
11. [Stack Memory and Heap Memory](#11-stack-memory-and-heap-memory)
12. [JVM Runtime Memory Areas](#12-jvm-runtime-memory-areas)
13. [Object Creation and Garbage
    Collection](#13-object-creation-and-garbage-collection)
14. [Java Pass-by-Value](#14-java-pass-by-value)
15. [Object Equality: `==`, `equals()`, and
    `hashCode()`](#15-object-equality--equals-and-hashcode)
16. [Immutability and Defensive
    Copying](#16-immutability-and-defensive-copying)
17. [Common Design Principles](#17-common-design-principles)
18. [Common Mistakes and Best
    Practices](#18-common-mistakes-and-best-practices)
19. [Interview Revision Cheat Sheet](#19-interview-revision-cheat-sheet)
20. [Core Java Interview Questions](#20-core-java-interview-questions)
21. [References](#21-references)

------------------------------------------------------------------------

## 1. Java and the JVM: Big Picture

Java is a high-level, class-based programming language. Java source code
is compiled into **bytecode**, which the Java Virtual Machine (JVM)
executes.

``` text
Java source (.java)
        |
        | javac compiler
        v
Bytecode (.class)
        |
        | JVM class loading + execution
        v
Machine-specific execution
```

### JDK vs JRE vs JVM

-   **JVM (Java Virtual Machine):** Executes Java bytecode and manages
    runtime facilities such as memory and garbage collection.
-   **JRE (Java Runtime Environment):** Conceptually includes the JVM
    and libraries needed to run Java applications. Modern Java
    distributions do not always ship a separate JRE package.
-   **JDK (Java Development Kit):** Includes development tools such as
    the compiler (`javac`), launcher (`java`), debugger, and runtime
    components.

### Why Java is considered portable

The compiler produces bytecode rather than directly producing machine
code for one operating system. A compatible JVM can execute that
bytecode on different platforms.

**Important:** Portability does not mean every program behaves
identically in every environment. Native libraries, filesystem
conventions, encodings, and runtime versions can affect behavior.

------------------------------------------------------------------------

## 2. Class and Object

### 2.1 What is a class?

A **class** is a definition that describes the data and behavior its
objects can have.

It can contain: - Fields (instance variables) - Methods - Constructors -
Initialization blocks - Nested types

``` java
class Car {
    String brand;
    int speed;

    void accelerate() {
        speed += 10;
    }

    void display() {
        System.out.println(brand + " is moving at " + speed);
    }
}
```

Here, `Car` describes the state (`brand`, `speed`) and behavior
(`accelerate`, `display`) of car objects.

### 2.2 What is an object?

An **object** is a runtime instance of a class. Each object generally
has its own instance-field state, while methods are defined by the
class.

``` java
Car car1 = new Car();
car1.brand = "Toyota";
car1.speed = 40;

Car car2 = new Car();
car2.brand = "Honda";
car2.speed = 20;

car1.accelerate();

System.out.println(car1.speed); // 50
System.out.println(car2.speed); // 20
```

`car1` and `car2` refer to different objects, so their instance fields
are independent.

### 2.3 Class vs object

  -----------------------------------------------------------------------
  Class                               Object
  ----------------------------------- -----------------------------------
  Defines a type and its behavior     An instance of that type

  Declared in source code             Created at runtime, commonly using
                                      `new`

  Defines instance fields and methods Holds its own instance-field state

  One class can create many objects   Each object has its own identity

  Example: `Car`                      Example: a particular Toyota car
                                      object
  -----------------------------------------------------------------------

### 2.4 Reference variable vs object

``` java
Car first = new Car();
Car second = first;
```

Conceptually:

``` text
first  ----\
            +----> one Car object
second ----/
```

There are two reference variables but only one object. Changes through
either reference affect the same object.

``` java
second.speed = 80;
System.out.println(first.speed); // 80
```

A reference can also be `null`, meaning it refers to no object:

``` java
Car car = null;
// car.accelerate(); // throws NullPointerException
```

### 2.5 Instance members vs static members

-   **Instance field/method:** belongs to an individual object.
-   **Static field/method:** belongs to the class-level type rather than
    one particular instance.

``` java
class Employee {
    String name;                     // instance field
    static String company = "ABC";   // shared class field

    void showName() {
        System.out.println(name);
    }

    static void showCompany() {
        System.out.println(company);
    }
}
```

### When to use a class and object

Use a class to model a meaningful concept, such as `Employee`, `Order`,
`Payment`, or `Product`. Create objects when the application needs
individual instances of that concept.

------------------------------------------------------------------------

## 3. Constructors

A **constructor** initializes a new object. It has the same name as its
class and has no return type---not even `void`.

``` java
class Employee {
    String name;
    int age;

    Employee(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

Employee employee = new Employee("Akash", 25);
```

### Types and terminology

-   **Default constructor:** supplied by the compiler only when you
    declare no constructor at all.
-   **No-argument constructor:** a constructor that accepts no
    arguments. It may be written explicitly.
-   **Parameterized constructor:** accepts arguments to initialize an
    object.
-   **Constructor overloading:** multiple constructors with different
    parameter lists.

``` java
class Point {
    int x;
    int y;

    Point() {
        this(0, 0);
    }

    Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

### Constructor rules

-   Constructors are not inherited and cannot be overridden.
-   A constructor may be overloaded.
-   `this(...)` calls another constructor in the same class.
-   `super(...)` calls a constructor in the parent class.
-   A `this(...)` or `super(...)` constructor invocation must be the
    first statement in a constructor under traditional Java syntax;
    newer Java releases allow limited statements before an explicit
    constructor invocation, subject to language rules.
-   If a constructor does not explicitly invoke `this(...)` or
    `super(...)`, Java normally inserts a no-argument `super()` call
    when permitted. Compilation fails if no accessible matching parent
    constructor exists.

### Constructor vs method

  -----------------------------------------------------------------------
  Constructor                         Method
  ----------------------------------- -----------------------------------
  Initializes a new instance          Implements an operation

  Same name as class                  Has its own method name

  Has no return type                  Declares a return type or `void`

  Invoked as part of object           Invoked through a call
  construction                        

  Cannot be overridden                Instance methods can be overridden
                                      when rules permit
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 4. The Four Pillars of OOP

Object-Oriented Programming (OOP) organizes software around objects that
combine state and behavior. Four commonly taught pillars are
**encapsulation, abstraction, inheritance, and polymorphism**.

## 4.1 Encapsulation

### Definition

Encapsulation combines data and the operations that work on that data,
while controlling how callers access or modify the internal state.

In Java, it is commonly implemented with private fields and public
methods that enforce valid operations.

``` java
class BankAccount {
    private double balance;

    public BankAccount(double initialBalance) {
        if (initialBalance < 0) {
            throw new IllegalArgumentException("Balance cannot be negative");
        }
        this.balance = initialBalance;
    }

    public double getBalance() {
        return balance;
    }

    public void deposit(double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Deposit must be positive");
        }
        balance += amount;
    }

    public boolean withdraw(double amount) {
        if (amount <= 0 || amount > balance) {
            return false;
        }
        balance -= amount;
        return true;
    }
}
```

The caller cannot directly set `balance` to an invalid value.

### Benefits

-   Protects object invariants.
-   Reduces unwanted coupling.
-   Allows implementation changes without changing callers.
-   Centralizes validation and authorization logic.

### Important distinction

Encapsulation is not simply "write getters and setters for every field."
A setter that permits any value can expose the state rather than protect
it. Provide only the operations the class genuinely needs.

------------------------------------------------------------------------

## 4.2 Abstraction

### Definition

Abstraction exposes the essential behavior of a component while hiding
unnecessary implementation details.

For example, a payment caller may need to know that `pay(amount)`
processes a payment, but it should not need to know the internal steps
of a particular payment provider.

``` java
interface PaymentService {
    void pay(double amount);
}

class CardPaymentService implements PaymentService {
    @Override
    public void pay(double amount) {
        System.out.println("Processing card payment: " + amount);
        // Provider-specific processing belongs here.
    }
}
```

The caller depends on the `PaymentService` contract rather than the
internal payment implementation.

### Benefits

-   Simplifies usage.
-   Separates what a component does from how it does it.
-   Supports interchangeable implementations.
-   Helps testing through interfaces and dependency injection.

### Encapsulation vs abstraction

-   **Encapsulation:** controls access to internal state and
    implementation.
-   **Abstraction:** exposes a useful, simplified contract.

They complement each other but are not the same concept.

------------------------------------------------------------------------

## 4.3 Inheritance

### Definition

Inheritance lets a class derive fields and behavior from another class.
Java uses `extends` for class inheritance.

``` java
class Animal {
    void eat() {
        System.out.println("Animal is eating");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Dog is barking");
    }
}

Dog dog = new Dog();
dog.eat();  // inherited behavior
dog.bark(); // Dog-specific behavior
```

### Types often discussed in OOP

-   **Single:** one subclass extends one superclass.
-   **Multilevel:** a class extends a class that itself extends another
    class.
-   **Hierarchical:** multiple subclasses extend the same superclass.
-   **Multiple inheritance of classes:** Java does not allow a class to
    extend multiple classes. Similar behavior can be designed using
    interfaces and composition.

``` text
          Animal
         /      \
       Dog      Cat
```

### Benefits

-   Reuses shared behavior.
-   Represents genuine "is-a" relationships.
-   Supports runtime polymorphism through overriding.

### Risks

-   Tight coupling between parent and child.
-   Fragile base-class changes.
-   Inheritance hierarchies can become hard to understand.
-   A subclass can violate expectations of the parent type if designed
    poorly.

**Rule of thumb:** Use inheritance for a genuine substitutable "is-a"
relationship. Prefer composition when you mainly want to reuse
functionality.

------------------------------------------------------------------------

## 4.4 Polymorphism

### Definition

Polymorphism means one common interface or parent type can represent
objects with different implementations.

### A. Compile-time polymorphism: overloading

Overloaded methods share a name but have different parameter lists.

``` java
class Calculator {
    int add(int a, int b) {
        return a + b;
    }

    int add(int a, int b, int c) {
        return a + b + c;
    }

    double add(double a, double b) {
        return a + b;
    }
}
```

The compiler chooses the applicable overload based on the compile-time
types and arguments.

### B. Runtime polymorphism: overriding

A subclass supplies a new implementation of an inherited instance
method.

``` java
class Animal {
    void sound() {
        System.out.println("Generic sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Bark");
    }
}

class Cat extends Animal {
    @Override
    void sound() {
        System.out.println("Meow");
    }
}

Animal animal = new Dog();
animal.sound(); // Bark
```

The reference type is `Animal`, but the actual object is `Dog`. The
overridden instance method is selected based on the runtime object.

### Overloading vs overriding

  -----------------------------------------------------------------------
  Overloading                         Overriding
  ----------------------------------- -----------------------------------
  Usually within the same class,      Between a subclass and an inherited
  though inherited overloads can      method or through interface
  participate                         implementation

  Different parameter lists           Same signature, subject to Java
                                      override rules

  Resolved at compile time            Instance method implementation
                                      dispatched at runtime

  Return type alone cannot            Return type must be compatible;
  distinguish overloads               covariant reference returns are
                                      allowed

  Static methods can be overloaded    Static methods are hidden, not
                                      overridden
  -----------------------------------------------------------------------

### Polymorphism and reference types

``` java
Animal animal = new Dog();
animal.sound(); // Allowed: declared on Animal and overridden by Dog
// animal.bark(); // Compile error: bark is not declared on Animal
```

The reference type controls which members are accessible at compile
time; the runtime object controls the implementation of an overridden
instance method.

------------------------------------------------------------------------

## 5. Abstract Classes

### Definition

An abstract class is declared using `abstract`. It cannot be
instantiated directly and can contain both abstract methods (without
implementations) and concrete methods (with implementations).

``` java
abstract class Shape {
    abstract double area();

    void describe() {
        System.out.println("I am a shape");
    }
}

class Circle extends Shape {
    private final double radius;

    Circle(double radius) {
        this.radius = radius;
    }

    @Override
    double area() {
        return Math.PI * radius * radius;
    }
}
```

Usage:

``` java
Shape shape = new Circle(5);
System.out.println(shape.area());
shape.describe();

// Shape s = new Shape(); // Compile error
```

### Characteristics

-   Can have constructors.
-   Can have instance and static fields.
-   Can have concrete and abstract methods.
-   Can use access modifiers such as `private`, `protected`, and
    `public` according to member rules.
-   A concrete subclass must implement inherited abstract methods unless
    it is itself abstract.
-   A class can extend only one class, abstract or concrete.

### When to use an abstract class

Use one when closely related classes share state or implementation, but
the base type should not be instantiated on its own.

Examples: - A base `Notification` with shared recipient or logging
behavior. - An abstract `Shape` requiring an `area()` implementation. -
A framework base class with shared workflow and customizable steps.

### Abstract class design tip

Do not make a class abstract merely because it has a method to override.
Use abstraction when the type represents an incomplete base concept or
when shared implementation is meaningful.

------------------------------------------------------------------------

## 6. Interfaces

### Definition

An interface defines a contract that implementing classes agree to
provide. It is declared with `interface` and implemented with
`implements`.

``` java
interface Notifier {
    void send(String message);
}

class EmailNotifier implements Notifier {
    @Override
    public void send(String message) {
        System.out.println("Sending email: " + message);
    }
}

class SmsNotifier implements Notifier {
    @Override
    public void send(String message) {
        System.out.println("Sending SMS: " + message);
    }
}
```

A caller can use either implementation through the same interface:

``` java
void notifyUser(Notifier notifier) {
    notifier.send("Your order has shipped");
}
```

### Interface members

-   Abstract instance methods are implicitly `public abstract` when
    declared without a body.
-   Fields declared in an interface are implicitly `public static final`
    constants.
-   Interfaces can have `default` methods with a body.
-   Interfaces can have `static` methods with a body.
-   Interfaces can have `private` helper methods in modern Java (Java
    9+).
-   Interfaces cannot be instantiated directly and do not have
    constructors.
-   A class can implement multiple interfaces.
-   An interface can extend multiple interfaces.

``` java
interface Logger {
    void log(String message);

    default void logError(String message) {
        log("ERROR: " + message);
    }

    static Logger console() {
        return message -> System.out.println(message);
    }
}
```

### Multiple interface implementation

``` java
interface Flyable {
    void fly();
}

interface Swimmable {
    void swim();
}

class Duck implements Flyable, Swimmable {
    @Override
    public void fly() {
        System.out.println("Flying");
    }

    @Override
    public void swim() {
        System.out.println("Swimming");
    }
}
```

### When to use an interface

-   Different classes need to support the same capability.
-   You want callers to depend on a contract rather than a concrete
    class.
-   Multiple unrelated classes need the same API.
-   You want easy substitution in tests or dependency injection.
-   You need a type that can be implemented alongside another class
    inheritance hierarchy.

------------------------------------------------------------------------

## 7. Abstract Class vs Interface

  -----------------------------------------------------------------------
  Feature                 Abstract class          Interface
  ----------------------- ----------------------- -----------------------
  Declaration             `abstract class A`      `interface A`

  Instantiation           Cannot instantiate      Cannot instantiate
                          directly                directly

  Constructors            Yes                     No

  Instance state          Can hold instance       No ordinary
                          fields                  per-instance fields;
                                                  fields are constants

  Concrete methods        Yes                     Yes, via `default`,
                                                  `static`, and private
                                                  helper methods as
                                                  permitted

  Abstract methods        Yes                     Yes

  Inheritance             A class extends one     A class can implement
                          class                   multiple interfaces

  Multiple inheritance    A class cannot extend   An interface can extend
                          multiple classes        multiple interfaces

  Access control          Members can use         Abstract contract
                          class-appropriate       methods are public;
                          access modifiers        interface fields are
                                                  public static final

  Best fit                Shared state and common Contract/capability
                          implementation among    across potentially
                          related types           unrelated types
  -----------------------------------------------------------------------

### Practical decision rule

Choose an **abstract class** when subclasses share meaningful state or
implementation and form a genuine family.

Choose an **interface** when you need a contract or capability that
multiple classes can implement independently.

You can use both:

``` java
interface Payable {
    void pay();
}

abstract class Payment implements Payable {
    protected final String transactionId;

    protected Payment(String transactionId) {
        this.transactionId = transactionId;
    }

    void printTransactionId() {
        System.out.println(transactionId);
    }
}

class CardPayment extends Payment {
    CardPayment(String transactionId) {
        super(transactionId);
    }

    @Override
    public void pay() {
        System.out.println("Paying by card: " + transactionId);
    }
}
```

------------------------------------------------------------------------

## 8. Composition, Association, Aggregation, and Dependency

These terms describe relationships between objects and are often
discussed in OOP and Low-Level Design interviews.

### Association

A general relationship where one object knows about or interacts with
another.

Example: `Teacher` teaches `Student`.

### Aggregation

A whole-part relationship in which the part can conceptually exist
independently of the whole.

Example: a `Department` has `Professor` objects, while professors can
exist independently of a particular department.

### Composition

A stronger whole-part relationship where the containing object owns the
lifecycle or creation of its parts as a design concept.

Example: an `Order` owns its `OrderLine` objects in a typical domain
model.

### Dependency

One component temporarily uses another, such as a method receiving a
service as a parameter.

### Example: composition

``` java
class Engine {
    void start() {
        System.out.println("Engine started");
    }
}

class Car {
    private final Engine engine;

    Car() {
        this.engine = new Engine();
    }

    void start() {
        engine.start();
    }
}
```

This example constructs an `Engine` internally. In other designs, an
`Engine` may be injected from outside, creating looser coupling.

### Composition vs inheritance

-   Inheritance: **is-a** relationship (`Dog` is an `Animal`).
-   Composition: **has-a** relationship (`Car` has an `Engine`).

Prefer composition when behavior needs to be replaceable or shared
without forcing a rigid class hierarchy.

------------------------------------------------------------------------

## 9. Important Java Keywords

## 9.1 `this`

`this` refers to the current object instance in an instance context.

### Use 1: distinguish fields from parameters

``` java
class User {
    private String name;

    User(String name) {
        this.name = name;
    }
}
```

`this.name` is the instance field; `name` is the constructor parameter.

### Use 2: call another constructor

``` java
class Rectangle {
    int width;
    int height;

    Rectangle() {
        this(1, 1);
    }

    Rectangle(int width, int height) {
        this.width = width;
        this.height = height;
    }
}
```

`this(...)` must be the first constructor invocation under the
applicable Java language rules and cannot be used recursively in a
constructor chain.

### Use 3: pass or return the current object

``` java
class Builder {
    private int value;

    Builder setValue(int value) {
        this.value = value;
        return this;
    }
}
```

This enables method chaining:

``` java
Builder builder = new Builder().setValue(10);
```

### Use 4: pass current object to another method

``` java
someService.register(this);
```

Be careful about exposing `this` from a constructor before the object is
fully initialized.

### `this` restrictions

-   `this` is not available in a static method because a static method
    has no current instance.
-   `this` is different from `super`.
-   A lambda expression generally captures the enclosing `this`; an
    anonymous inner class has its own `this`.

------------------------------------------------------------------------

## 9.2 `static`

`static` declares a class-level member rather than an instance member.

### Static variable

``` java
class Employee {
    String name;
    static int employeeCount = 0;

    Employee(String name) {
        this.name = name;
        employeeCount++;
    }
}
```

`employeeCount` is shared across instances of this class (within the
relevant class-loader context).

### Static method

``` java
class MathUtil {
    static int square(int x) {
        return x * x;
    }
}

int result = MathUtil.square(5);
```

A static method is usually called using the class name. It cannot
directly access instance members without an object reference.

### Static initialization block

``` java
class Configuration {
    static final String APP_NAME;

    static {
        APP_NAME = "OrderService";
    }
}
```

Static initialization runs as part of class initialization, before the
class is actively used, subject to Java's class-initialization rules.

### Static nested class

A nested class declared `static` does not require an instance of the
outer class.

``` java
class Outer {
    static class Nested {
        void hello() {
            System.out.println("Hello");
        }
    }
}

Outer.Nested nested = new Outer.Nested();
```

### Static method hiding vs overriding

Static methods are resolved by the reference/class context and can be
**hidden** in subclasses. They are not dynamically overridden like
instance methods.

### When to use `static`

-   Utility functions that do not need object state.
-   Constants (`static final`) when appropriate.
-   Class-level counters or shared class data, with thread-safety
    considerations.
-   Factory methods.

Avoid turning mutable global state into static fields without
considering testing, concurrency, and lifecycle.

------------------------------------------------------------------------

## 9.3 `final`

`final` restricts reassignment, overriding, or inheritance depending on
where it is used.

### Final variable

``` java
final int maxRetries = 3;
// maxRetries = 4; // Compile error
```

A final variable can be assigned only once. For a blank final instance
field, assignment must be completed in a valid constructor/initializer
path.

### Final reference vs immutable object

``` java
final List<String> names = new ArrayList<>();
names.add("Akash"); // Allowed
// names = new ArrayList<>(); // Not allowed
```

The reference cannot point to another list, but the referenced list
remains mutable.

### Final method

``` java
class Parent {
    final void printId() {
        System.out.println("ID");
    }
}
```

A subclass cannot override this method.

### Final class

``` java
final class SecurityToken {
}
```

A final class cannot be subclassed. `String` is a familiar example of a
final class.

### `final` vs `finally` vs `finalize`

-   `final`: keyword restricting reassignment, overriding, or
    inheritance.
-   `finally`: exception-handling block that normally runs after
    `try`/`catch` as control leaves the construct; process termination
    or abrupt JVM failure can prevent it.
-   `finalize()`: legacy finalization mechanism, deprecated for removal
    and not a reliable cleanup strategy. Prefer try-with-resources and
    explicit resource management.

------------------------------------------------------------------------

## 9.4 `super`

`super` refers to superclass behavior and constructors.

### Call a parent constructor

``` java
class Person {
    String name;

    Person(String name) {
        this.name = name;
    }
}

class Developer extends Person {
    String language;

    Developer(String name, String language) {
        super(name);
        this.language = language;
    }
}
```

### Call an overridden superclass method

``` java
class Parent {
    void display() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    @Override
    void display() {
        super.display();
        System.out.println("Child");
    }
}
```

`super(...)` must be the constructor invocation in the required
position. `super.method()` can call the superclass implementation of an
overridable instance method.

------------------------------------------------------------------------

## 9.5 `abstract`

-   An abstract class cannot be instantiated directly.
-   An abstract method declares behavior without providing its
    implementation in that class.
-   A non-abstract class must implement all inherited abstract methods
    unless they are already implemented.
-   A method cannot be both `abstract` and `final`, because one requires
    overriding while the other forbids it.
-   Abstract methods cannot be `private` or `static`, since they require
    instance overriding semantics.

------------------------------------------------------------------------

## 9.6 `synchronized` and `volatile` (overview)

These keywords are related to concurrency and the Java Memory Model.

### `synchronized`

A synchronized method or block provides mutual exclusion through a
monitor and establishes visibility guarantees around monitor unlock/lock
operations.

``` java
class Counter {
    private int count;

    synchronized void increment() {
        count++;
    }

    synchronized int getCount() {
        return count;
    }
}
```

This protects these operations when all relevant accesses follow the
same synchronization discipline.

### `volatile`

A volatile field provides visibility and ordering guarantees for reads
and writes to that field. It does **not** make compound operations such
as `count++` atomic.

``` java
private volatile boolean running = true;
```

Use `volatile` for suitable shared-state visibility patterns. Use
synchronization, locks, or atomic classes for compound state changes
when required.

------------------------------------------------------------------------

## 9.7 Other keywords worth knowing

  -----------------------------------------------------------------------
  Keyword                             Purpose
  ----------------------------------- -----------------------------------
  `new`                               Creates an object or array

  `instanceof`                        Tests whether an object is
                                      compatible with a type; modern Java
                                      supports pattern matching forms

  `return`                            Returns control, optionally with a
                                      value

  `static`                            Class-level members

  `default`                           Defines a default method in an
                                      interface; also appears in switch
                                      syntax

  `enum`                              Declares a fixed set of named
                                      constants

  `package`                           Declares a package

  `import`                            Makes types available by simple
                                      name

  `throw`                             Throws an exception

  `throws`                            Declares exceptions a
                                      method/constructor may propagate

  `try`, `catch`, `finally`           Exception handling

  `transient`                         Excludes a field from default Java
                                      serialization

  `native`                            Declares a method implemented
                                      outside Java, commonly via JNI

  `strictfp`                          Historically constrained
                                      floating-point evaluation;
                                      redundant under modern Java's
                                      always-strict floating-point
                                      semantics

  `assert`                            Enables runtime assertions when
                                      assertion checking is enabled

  `record`                            Declares a record class in modern
                                      Java

  `sealed`, `permits`, `non-sealed`   Control which classes may directly
                                      extend or implement a sealed type
  -----------------------------------------------------------------------

Java is case-sensitive: `String` and `string` are different identifiers.

------------------------------------------------------------------------

## 10. Access Modifiers

Access modifiers control where a class or member can be accessed.

  -----------------------------------------------------------------------------------
  Modifier              Same class   Same package        Subclass in  Unrelated class
                                                     another package       in another
                                                                              package
  ----------------- -------------- -------------- ------------------ ----------------
  `private`                    Yes             No                 No               No

  package-private              Yes            Yes  No, except access               No
  (no modifier)                                      through package 
                                                          membership 

  `protected`                  Yes            Yes    Yes, subject to               No
                                                    protected-access 
                                                               rules 

  `public`                     Yes            Yes                Yes  Yes, subject to
                                                                        module/export
                                                                                  and
                                                                       enclosing-type
                                                                        accessibility
  -----------------------------------------------------------------------------------

### Example

``` java
class Account {
    private double balance;
    protected String accountType;
    public String getType() {
        return accountType;
    }
}
```

A top-level class can generally be `public` or package-private;
top-level classes cannot be declared `private` or `protected`. Nested
types have additional access options.

**Interview detail:** `protected` access across packages is more nuanced
than "all subclasses can access everything." Access is constrained by
the qualifying reference and subclass rules.

------------------------------------------------------------------------

## 11. Stack Memory and Heap Memory

The terms **stack** and **heap** describe important runtime memory
concepts. The exact layout is JVM-implementation-dependent, so use these
as a conceptual model rather than assuming every local variable or
object is physically stored exactly as pictured.

## 11.1 Stack

Each Java thread has its own JVM stack. Method calls create frames that
hold information such as local variables, operand-stack values, and
return/exception handling state.

Conceptual example:

``` java
void example() {
    int number = 10;
    Car car = new Car();
}
```

A simplified view of the method frame might be:

``` text
Thread Stack
+---------------------------+
| example() frame           |
| number = 10               |
| car = reference ----------+------+
+---------------------------+      |
                                   v
                              Heap object
```

The local reference `car` is conceptually associated with the stack
frame, while the object created by `new Car()` is conceptually on the
heap. JVM optimizations may eliminate or transform allocations while
preserving program behavior.

### Stack characteristics

-   Per-thread.
-   Stores method frames and execution state.
-   Frames are pushed on method calls and removed when calls return or
    unwind.
-   Often small compared with the heap.
-   Excessively deep recursion can cause `StackOverflowError`.

## 11.2 Heap

The heap is the runtime area from which objects and arrays are generally
allocated. It is shared among Java threads.

``` java
Car car1 = new Car();
Car car2 = new Car();
```

The references can refer to heap objects. If `car2 = car1`, both
references point to the same object.

### Heap characteristics

-   Shared across threads.
-   Holds objects and arrays in the usual conceptual model.
-   Managed by garbage collection.
-   Can run out of available memory, resulting in `OutOfMemoryError`.
-   Objects can remain reachable after a method returns if references to
    them survive elsewhere.

## 11.3 Stack vs heap

  -----------------------------------------------------------------------
  Stack                               Heap
  ----------------------------------- -----------------------------------
  Per-thread                          Shared runtime area

  Method frames and local execution   Objects and arrays in the usual
  state                               model

  Frames follow call/return lifecycle Objects remain while reachable and
                                      may later be collected

  Deep recursion can cause            Memory pressure can cause
  `StackOverflowError`                `OutOfMemoryError`

  Usually managed automatically as    Garbage collector manages
  calls return                        reclaiming eligible objects
  -----------------------------------------------------------------------

### Common misconception

It is not strictly correct to say "all primitives are on the stack and
all objects are on the heap." A primitive can be an instance field
inside a heap object; static fields are associated with class-level
runtime state; JIT optimizations can eliminate allocations. The location
depends on context and implementation.

------------------------------------------------------------------------

## 12. JVM Runtime Memory Areas

The JVM specification defines runtime data areas. Their exact physical
implementation is JVM-dependent.

### 12.1 Program Counter (PC) register

Each thread has a PC register that tracks the current JVM instruction
position for the thread, with specification details for native
execution.

### 12.2 JVM Stack

Each thread has its own JVM stack containing frames for method
invocations. A frame includes local variables, an operand stack, and
supporting runtime information.

### 12.3 Heap

Shared area used for object and array allocation in the standard runtime
model. Garbage collection primarily manages heap objects.

### 12.4 Method Area and class metadata

The JVM specification describes a shared method area for per-class
structures such as runtime constant pools, field/method data, and method
code. In HotSpot, much class metadata is stored in **Metaspace**, which
uses native memory; this is an implementation detail, not a universal
JVM rule.

### 12.5 Runtime Constant Pool

A per-class or per-interface runtime representation of constants,
including symbolic references used by the JVM.

### 12.6 Native Method Stack

Supports native method execution. Its implementation and whether it is
distinct from other stacks depend on the JVM.

### Simplified overview

``` text
JVM Process
|
+-- Shared areas
|   +-- Heap: objects and arrays
|   +-- Class metadata / method-area concepts
|
+-- Thread A
|   +-- PC register
|   +-- JVM stack: method frames
|   +-- Native execution support
|
+-- Thread B
    +-- PC register
    +-- JVM stack: method frames
    +-- Native execution support
```

### Errors to distinguish

-   `StackOverflowError`: a thread's stack cannot accommodate further
    frames, often due to deep recursion.
-   `OutOfMemoryError: Java heap space`: heap allocation cannot be
    satisfied.
-   `OutOfMemoryError: Metaspace`: class metadata allocation exceeds
    available/configured native memory.
-   Native-memory exhaustion can also cause failures outside the Java
    heap.

------------------------------------------------------------------------

## 13. Object Creation and Garbage Collection

## 13.1 Object lifecycle (conceptual)

1.  A class is loaded and initialized when needed.
2.  Code requests an object, commonly using `new`.
3.  The JVM allocates and initializes storage according to its
    implementation.
4.  The constructor initializes the object.
5.  References make the object reachable from program roots.
6.  When the object is no longer reachable, it becomes eligible for
    garbage collection.
7.  The garbage collector may reclaim its memory at a later time.

**Eligible for collection does not mean collected immediately.** Java
does not promise an exact collection time.

## 13.2 Reachability

An object is generally live if it can be reached through references from
GC roots, such as active thread stacks, static/class-related references,
and other JVM roots.

``` java
Car car = new Car();
car = null;
```

If no other reachable reference points to the original object, it may
become eligible for collection. If another reference exists, it remains
reachable.

## 13.3 Garbage collection (GC)

Garbage collection reclaims memory occupied by objects that are no
longer reachable. Modern collectors may use generational strategies,
concurrent work, compaction, and other techniques.

### What GC does not guarantee

-   It does not guarantee immediate collection after a reference is
    cleared.
-   It does not automatically close files, sockets, or database
    connections at the moment an object becomes unreachable.
-   It does not prevent all memory leaks. A program can retain
    references to objects it no longer logically needs.

## 13.4 Memory leak in Java

A Java memory leak often means objects remain reachable through
unintended references, so the garbage collector cannot reclaim them.

Common causes: - Unbounded static collections. - Caches without
eviction. - Listeners or callbacks that are never deregistered. -
Long-lived collections retaining request/session objects. - Incorrect
`ThreadLocal` lifecycle management in pooled threads.

## 13.5 Resource management: use try-with-resources

Use `AutoCloseable` resources with try-with-resources rather than
relying on garbage collection for cleanup.

``` java
try (BufferedReader reader =
         new BufferedReader(new FileReader("input.txt"))) {
    System.out.println(reader.readLine());
} catch (IOException e) {
    // Handle the error appropriately.
}
```

The resource is closed automatically when execution leaves the try
block, including exceptional exits.

## 13.6 `System.gc()`

`System.gc()` requests that the JVM make a best effort to perform
garbage collection. It is not a guarantee and should not be used as
normal application memory-management logic.

------------------------------------------------------------------------

## 14. Java Pass-by-Value

Java is **always pass-by-value**. When passing an object variable to a
method, the value copied is the reference. The method receives a copy of
that reference, not a pass-by-reference alias to the caller's variable.

### Primitive example

``` java
static void change(int x) {
    x = 100;
}

int value = 10;
change(value);
System.out.println(value); // 10
```

The method receives a copy of the primitive value.

### Object example: mutate the object

``` java
class Box {
    int value;
}

static void update(Box box) {
    box.value = 100;
}

Box original = new Box();
original.value = 10;

update(original);
System.out.println(original.value); // 100
```

The copied reference still points to the same object, so the object can
be mutated.

### Object example: reassign the parameter

``` java
static void replace(Box box) {
    box = new Box();
    box.value = 500;
}

Box original = new Box();
original.value = 10;

replace(original);
System.out.println(original.value); // 10
```

Reassigning the local parameter does not change the caller's variable.

### Mental model

``` text
Caller variable ----> Object
        |
        | copied reference value into method parameter
        v
Method parameter ----> same Object
```

Both references initially point to the same object. Changing the object
is visible through either reference; assigning the parameter to another
object does not update the caller's reference.

------------------------------------------------------------------------

## 15. Object Equality: `==`, `equals()`, and `hashCode()`

### `==`

-   For primitive values, compares values.
-   For object references, checks whether both references identify the
    same object.

### `equals()`

`Object.equals()` defaults to identity comparison. Classes such as
`String` override it to define logical equality.

``` java
String a = new String("java");
String b = new String("java");

System.out.println(a == b);      // false: typically distinct objects
System.out.println(a.equals(b)); // true: same text
```

### `hashCode()`

Hash-based collections use hash codes to help organize keys. The
contract requires that if two objects are equal according to `equals()`,
they must have the same `hashCode()`.

Two unequal objects may have the same hash code.

### When overriding

If you override `equals()`, also override `hashCode()` consistently.

``` java
import java.util.Objects;

class EmployeeKey {
    private final int id;

    EmployeeKey(int id) {
        this.id = id;
    }

    @Override
    public boolean equals(Object other) {
        if (this == other) return true;
        if (!(other instanceof EmployeeKey)) return false;
        EmployeeKey that = (EmployeeKey) other;
        return id == that.id;
    }

    @Override
    public int hashCode() {
        return Objects.hash(id);
    }
}
```

### Pitfalls

-   Mutating fields used in `equals()` or `hashCode()` while an object
    is a `HashMap` key can make lookup unreliable.
-   Do not use `==` to compare string content.
-   Equality should be reflexive, symmetric, transitive, consistent, and
    false for non-null comparisons to `null`.

------------------------------------------------------------------------

## 16. Immutability and Defensive Copying

An **immutable object** cannot have its observable state changed after
construction.

### Common design practices

-   Declare the class `final` when subclassing could undermine
    immutability.
-   Make fields `private final`.
-   Initialize all fields in the constructor.
-   Do not expose mutators.
-   Make defensive copies of mutable inputs and outputs.

``` java
final class Team {
    private final String name;
    private final List<String> members;

    Team(String name, List<String> members) {
        this.name = name;
        this.members = new ArrayList<>(members);
    }

    public String getName() {
        return name;
    }

    public List<String> getMembers() {
        return Collections.unmodifiableList(members);
    }
}
```

The constructor makes a defensive copy, so later changes to the caller's
original list do not alter the team's internal list. The unmodifiable
view prevents callers from modifying the list through the getter.
Because the internal list is private and never changed after
construction, this design keeps the collection stable.

For mutable elements inside the list, a shallow copy may not be enough;
deep immutability requires considering the elements too.

### Benefits

-   Easier reasoning about state.
-   Safer use as hash-based keys.
-   More naturally thread-safe when all reachable state is immutable.
-   Fewer accidental side effects.

------------------------------------------------------------------------

## 17. Common Design Principles

These are not Java keywords, but they help apply OOP well.

### Program to an interface

Depend on the abstraction where it makes sense, not on a specific
implementation.

``` java
List<String> names = new ArrayList<>();
```

The variable is typed as `List`, allowing another implementation to be
substituted when appropriate.

### Favor composition over inheritance

Compose behavior from collaborators when a strict subtype relationship
is unnecessary.

### SOLID overview

-   **S --- Single Responsibility Principle:** a class should have one
    cohesive reason to change.
-   **O --- Open/Closed Principle:** software entities should be open to
    extension and closed to inappropriate modification.
-   **L --- Liskov Substitution Principle:** a subtype should remain
    usable wherever its base type is expected without breaking
    behavioral expectations.
-   **I --- Interface Segregation Principle:** prefer focused interfaces
    over large contracts that force clients to depend on unused methods.
-   **D --- Dependency Inversion Principle:** high-level policy should
    depend on abstractions rather than low-level implementation details.

### Example: dependency injection

``` java
interface MessageSender {
    void send(String message);
}

class NotificationService {
    private final MessageSender sender;

    NotificationService(MessageSender sender) {
        this.sender = sender;
    }

    void notify(String message) {
        sender.send(message);
    }
}
```

`NotificationService` does not need to know whether the sender is email,
SMS, or a test double. This is a common idea used by
dependency-injection frameworks such as Spring.

------------------------------------------------------------------------

## 18. Common Mistakes and Best Practices

1.  **Confusing a reference with an object:** several variables can
    refer to the same object.
2.  **Making every field public:** this exposes internal state and
    weakens invariants.
3.  **Using inheritance just for code reuse:** prefer composition if the
    relationship is not genuinely "is-a."
4.  **Assuming abstract classes and interfaces are identical:** abstract
    classes can hold instance state and constructors; interfaces provide
    contracts and support multiple implementation.
5.  **Thinking static methods are overridden:** they are hidden, not
    dynamically overridden.
6.  **Thinking `final` makes an object immutable:** a final reference
    can still point to a mutable object.
7.  **Assuming Java passes objects by reference:** Java passes a copy of
    the reference value.
8.  **Assuming all local variables are physically on the stack:** JVM
    implementation and optimization details are more complex.
9.  **Relying on GC to close resources:** use try-with-resources or
    explicit cleanup.
10. **Assuming `System.gc()` guarantees collection:** it is only a
    request.
11. **Overriding `equals()` without `hashCode()`:** breaks the expected
    contract of hash-based collections.
12. **Leaking `this` from a constructor:** another component may observe
    a partially initialized object.
13. **Using static mutable state casually:** it can cause hidden
    dependencies and concurrency bugs.
14. **Using getters/setters without a reason:** expose behavior and
    state intentionally.
15. **Ignoring nullability:** document and validate whether references
    may be `null`.

------------------------------------------------------------------------

## 19. Interview Revision Cheat Sheet

  -----------------------------------------------------------------------
  Topic                               One-line answer
  ----------------------------------- -----------------------------------
  Class                               A definition of state and behavior
                                      for objects

  Object                              A runtime instance with identity
                                      and state

  Constructor                         Initializes a new instance; has the
                                      class name and no return type

  Encapsulation                       Protects internal state and
                                      controls access

  Abstraction                         Exposes essential behavior while
                                      hiding implementation detail

  Inheritance                         Derives a class from another class

  Polymorphism                        Uses a common type/contract for
                                      different implementations

  Overloading                         Same method name with different
                                      parameter lists

  Overriding                          Subclass provides a compatible
                                      implementation of an inherited
                                      instance method

  Abstract class                      Incomplete base class that can
                                      share state and concrete behavior

  Interface                           Contract that classes can
                                      implement; supports multiple
                                      implementation

  `this`                              Reference to the current object in
                                      an instance context

  `super`                             Accesses superclass constructors or
                                      behavior

  `static`                            Class-level member, not tied to one
                                      instance

  `final` variable                    Can be assigned only once

  `final` method                      Cannot be overridden

  `final` class                       Cannot be subclassed

  Stack                               Per-thread method frames and
                                      execution state

  Heap                                Shared area commonly used for
                                      objects and arrays

  GC                                  Reclaims memory of unreachable
                                      objects, at an unspecified time

  Pass-by-value                       Java copies the argument value; for
                                      objects, that value is a reference

  `==` on references                  Tests identity

  `equals()`                          Tests logical equality when
                                      implemented that way

  `hashCode()`                        Supports hash-based collections and
                                      must agree with `equals()`

  Immutability                        Observable state cannot change
                                      after construction

  Composition                         Builds behavior by containing/using
                                      other objects

  `synchronized`                      Provides monitor-based mutual
                                      exclusion and visibility guarantees

  `volatile`                          Provides visibility/order
                                      guarantees for a field, not
                                      compound-operation atomicity
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 20. Core Java Interview Questions

### OOP fundamentals

1.  What is the difference between a class and an object?
2.  Explain the four pillars of OOP with practical examples.
3.  What is encapsulation, and why should fields usually be private?
4.  What is the difference between abstraction and encapsulation?
5.  Explain inheritance and its advantages and disadvantages.
6.  Why does Java not support multiple inheritance of classes?
7.  What is polymorphism?
8.  Explain overloading vs overriding.
9.  Can a static method be overridden?
10. Can a constructor be inherited or overridden?

### Abstract classes and interfaces

11. What is an abstract class?
12. Can an abstract class have a constructor and concrete methods?
13. Can an abstract class be instantiated?
14. What is an interface?
15. Can a class implement multiple interfaces?
16. Can an interface contain method implementations?
17. What are default and static interface methods?
18. When would you choose an interface over an abstract class?
19. Can an interface extend multiple interfaces?
20. How do you resolve conflicting inherited default methods?

### Keywords

21. What does `this` mean?
22. What is the difference between `this()` and `super()`?
23. What is a static variable, and how is it different from an instance
    variable?
24. Can a static method access instance variables directly?
25. What does `final` mean for variables, methods, and classes?
26. What is the difference between `final`, `finally`, and `finalize()`?
27. What is the purpose of `super`?
28. What is the difference between `synchronized` and `volatile`?

### Memory and JVM

29. Explain stack memory vs heap memory.
30. What is a stack frame?
31. What are the JVM runtime memory areas?
32. What is garbage collection?
33. When does an object become eligible for garbage collection?
34. Does setting a reference to `null` immediately free the object?
35. What is a memory leak in Java?
36. What causes `StackOverflowError` vs `OutOfMemoryError`?
37. What is Metaspace?
38. Why should `System.gc()` not be relied upon?

### Object behavior and design

39. Is Java pass-by-value or pass-by-reference?
40. What is the difference between `==` and `equals()`?
41. Why should `hashCode()` be overridden when `equals()` is overridden?
42. How do you create an immutable class?
43. What is composition, and how does it differ from inheritance?
44. Explain SOLID principles.
45. How does dependency injection reduce coupling?

### Suggested interview practice

For each question, prepare: - A concise definition. - A small Java
example. - One practical use case. - One caveat or common misconception.

------------------------------------------------------------------------

## 21. References

Use official documentation for exact language and JVM rules:

1.  **Java Language Specification (JLS):**
    https://docs.oracle.com/javase/specs/
2.  **Java Virtual Machine Specification (JVMS):**
    https://docs.oracle.com/javase/specs/jvms/se21/html/
3.  **Java Tutorials --- Object-Oriented Programming Concepts:**
    https://docs.oracle.com/javase/tutorial/java/concepts/
4.  **Java Tutorials --- Classes and Objects:**
    https://docs.oracle.com/javase/tutorial/java/javaOO/
5.  **Java API documentation:** https://docs.oracle.com/en/java/javase/
6.  **The `Object` class API (`equals`, `hashCode`, etc.):**
    https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Object.html
7.  **Java garbage collection overview:**
    https://docs.oracle.com/en/java/javase/21/gctuning/

------------------------------------------------------------------------

## Final Takeaway

Understand the relationship between **class, object, state, behavior,
and reference** first. Then master the four OOP pillars, choose between
abstract classes and interfaces based on the design, learn the exact
behavior of `this`, `static`, `final`, and `super`, and build a correct
mental model of JVM memory and garbage collection.

For interviews, do not stop at definitions. Be able to write a short
example, explain what happens at compile time versus runtime, and
correct common misconceptions.
