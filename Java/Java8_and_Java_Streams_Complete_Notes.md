# Java 8 & Java Streams --- Complete Notes

> **Purpose:** Interview-focused, practical notes on Java 8 features and
> the Stream API, with syntax, examples, comparisons, common pitfalls,
> and revision questions. Examples use Java 8-compatible syntax unless
> stated otherwise.

------------------------------------------------------------------------

## Table of Contents

1.  [Java 8 Overview](#1-java-8-overview)
2.  [Java 8 Features at a Glance](#2-java-8-features-at-a-glance)
3.  [Functional Programming
    Fundamentals](#3-functional-programming-fundamentals)
4.  [Lambda Expressions](#4-lambda-expressions)
5.  [Functional Interfaces](#5-functional-interfaces)
6.  [Method References](#6-method-references)
7.  [Default and Static Methods in
    Interfaces](#7-default-and-static-methods-in-interfaces)
8.  [Optional](#8-optional)
9.  [Date and Time API](#9-date-and-time-api)
10. [Stream API Fundamentals](#10-stream-api-fundamentals)
11. [Creating Streams](#11-creating-streams)
12. [Intermediate Operations](#12-intermediate-operations)
13. [Terminal Operations](#13-terminal-operations)
14. [Collectors and
    Collectors.groupingBy](#14-collectors-and-collectorsgroupingby)
15. [Primitive Streams and Numeric
    Operations](#15-primitive-streams-and-numeric-operations)
16. [Parallel Streams](#16-parallel-streams)
17. [Stream Laziness, Execution, and
    Reuse](#17-stream-laziness-execution-and-reuse)
18. [Common Stream Recipes](#18-common-stream-recipes)
19. [Streams vs Collections and
    Loops](#19-streams-vs-collections-and-loops)
20. [Performance, Best Practices, and Common
    Mistakes](#20-performance-best-practices-and-common-mistakes)
21. [Java 8 Features in Real
    Applications](#21-java-8-features-in-real-applications)
22. [Interview Comparisons](#22-interview-comparisons)
23. [Interview Questions and
    Answers](#23-interview-questions-and-answers)
24. [Quick Revision Cheat Sheet](#24-quick-revision-cheat-sheet)
25. [Practice Exercises](#25-practice-exercises)
26. [References](#26-references)

------------------------------------------------------------------------

## 1. Java 8 Overview

Java 8 (released in March 2014) was a major release because it
introduced functional-style programming features while retaining Java's
object-oriented foundation.

### Why Java 8 mattered

Before Java 8, passing behavior often required verbose anonymous inner
classes. Java 8 made behavior easier to express with lambdas and method
references. The Stream API also made common data-processing pipelines
more declarative.

``` java
// Before Java 8
List<String> names = Arrays.asList("Asha", "Akash", "Ravi");
for (String name : names) {
    if (name.startsWith("A")) {
        System.out.println(name);
    }
}

// Java 8
names.stream()
     .filter(name -> name.startsWith("A"))
     .forEach(System.out::println);
```

The Stream version expresses *what* should happen (filter names, then
print) rather than spelling out each iteration step. It is not
automatically faster; performance depends on the workload and
implementation.

------------------------------------------------------------------------

## 2. Java 8 Features at a Glance

  -----------------------------------------------------------------------------
  Feature                 Purpose                 Typical example
  ----------------------- ----------------------- -----------------------------
  Lambda expressions      Represent behavior      `x -> x * 2`
                          concisely               

  Functional interfaces   Target type for lambdas `Predicate<T>`

  Method references       Reuse an existing       `String::toUpperCase`
                          method as behavior      

  Stream API              Process data            `filter`, `map`, `collect`
                          declaratively           

  `Optional`              Represent a possibly    `Optional.ofNullable(x)`
                          absent value            

  Default methods         Add implementations to  `default void log()`
                          interfaces              

  Static interface        Utility methods on      `Comparator.comparing(...)`
  methods                 interfaces              

  New Date-Time API       Safer, clearer          `LocalDate.now()`
                          date/time types         

  `CompletableFuture`     Compose asynchronous    `thenApply(...)`
                          computations            

  Base64 API              Encode/decode Base64    `Base64.getEncoder()`
                          data                    

  Collection/Map          Convenient operations   `removeIf`, `computeIfAbsent`
  enhancements                                    

  Nashorn JavaScript      JavaScript engine       Historical feature; later
  engine                  included at the time    deprecated/removed from newer
                                                  JDKs
  -----------------------------------------------------------------------------

**Scope note:** Java 8 notes focus on the language and library features
still useful for interviews. Some historically introduced components,
such as Nashorn, are not recommended as a modern design choice.

------------------------------------------------------------------------

## 3. Functional Programming Fundamentals

Functional-style Java emphasizes passing behavior, transforming values,
and avoiding unnecessary shared mutable state.

### Core terms

-   **Function:** maps an input to an output.
-   **Predicate:** tests a condition and returns `boolean`.
-   **Consumer:** accepts a value and performs an action; returns
    nothing.
-   **Supplier:** provides a value without taking an input.
-   **Side effect:** an observable change outside a computation, such as
    changing a field, writing to a file, or updating shared state.
-   **Higher-order function:** accepts behavior as an argument or
    returns behavior. Java APIs such as `filter` and `map` accept
    functions.

### Declarative vs imperative style

``` java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);

// Imperative: describe the steps
List<Integer> evens = new ArrayList<>();
for (Integer n : numbers) {
    if (n % 2 == 0) {
        evens.add(n);
    }
}

// Declarative: describe the transformation
List<Integer> evens2 = numbers.stream()
        .filter(n -> n % 2 == 0)
        .collect(Collectors.toList());
```

### Pure functions and state

A pure function returns the same output for the same input and does not
cause side effects. Pure transformations are easier to test and safer to
run in parallel.

Avoid mutating shared state inside stream operations:

``` java
// Avoid: mutation and possible concurrency problems
List<Integer> result = new ArrayList<>();
numbers.parallelStream().forEach(n -> result.add(n));

// Prefer a collector
List<Integer> result2 = numbers.parallelStream()
        .filter(n -> n > 2)
        .collect(Collectors.toList());
```

------------------------------------------------------------------------

## 4. Lambda Expressions

A lambda expression is a concise way to provide an implementation for a
functional interface.

### Syntax

``` java
(parameters) -> expression
(parameters) -> { statements; }
```

Examples:

``` java
Runnable task = () -> System.out.println("Running");

Comparator<Integer> ascending = (a, b) -> Integer.compare(a, b);

Function<String, Integer> length = s -> s.length();

BiFunction<Integer, Integer, Integer> add = (a, b) -> a + b;
```

### Syntax variations

``` java
() -> System.out.println("No parameters")
x -> x * x                         // Parentheses optional for one inferred parameter
(x, y) -> x + y                    // Multiple parameters
(String s) -> s.length()           // Explicit parameter type
x -> {                              // Block lambda
    int doubled = x * 2;
    return doubled;
}
```

A block lambda that returns a value needs an explicit `return`. An
expression lambda returns the expression's value when the target
functional method has a return value.

### Type inference

The compiler determines parameter types from the functional interface
target type.

``` java
Predicate<String> nonEmpty = s -> !s.isEmpty();
```

The type of `s` is inferred as `String`.

### Variable capture and effectively final variables

A lambda can capture a local variable only when it is final or
*effectively final* (assigned once and not reassigned).

``` java
int threshold = 10; // effectively final
Predicate<Integer> greater = n -> n > threshold;
```

This does not compile if `threshold` is reassigned after the lambda
captures it. The rule helps avoid confusing behavior around local
variables.

### `this` inside a lambda

A lambda does not introduce a new `this`; `this` refers to the enclosing
instance. An anonymous inner class has its own `this` referring to that
anonymous object.

------------------------------------------------------------------------

## 5. Functional Interfaces

A functional interface has exactly one abstract method (SAM: Single
Abstract Method). It may also contain default methods, static methods,
and methods matching public `Object` methods.

`@FunctionalInterface` is optional but useful: the compiler checks that
the interface satisfies the rule.

``` java
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);
}

Calculator sum = (a, b) -> a + b;
System.out.println(sum.calculate(4, 5)); // 9
```

### Important built-in functional interfaces

Import `java.util.function.*`.

  -----------------------------------------------------------------------------------------------
  Interface             Abstract method           Input → output      Example
  --------------------- ------------------------- ------------------- ---------------------------
  `Predicate<T>`        `boolean test(T t)`       `T → boolean`       `s -> s.isEmpty()`

  `Function<T,R>`       `R apply(T t)`            `T → R`             `s -> s.length()`

  `Consumer<T>`         `void accept(T t)`        `T → void`          `System.out::println`

  `Supplier<T>`         `T get()`                 no input → `T`      `() -> new ArrayList<>()`

  `UnaryOperator<T>`    `T apply(T t)`            `T → T`             `x -> x * 2` for integers

  `BinaryOperator<T>`   `T apply(T a, T b)`       `(T,T) → T`         `(a,b) -> a + b`

  `BiPredicate<T,U>`    `boolean test(T t,U u)`   `(T,U) → boolean`   `(s,n) -> s.length() > n`

  `BiFunction<T,U,R>`   `R apply(T t,U u)`        `(T,U) → R`         `(a,b) -> a + b`

  `BiConsumer<T,U>`     `void accept(T t,U u)`    `(T,U) → void`      `(k,v) -> ...`

  `IntPredicate`        `boolean test(int)`       `int → boolean`     `n -> n % 2 == 0`

  `ToIntFunction<T>`    `int applyAsInt(T)`       `T → int`           `String::length`
  -----------------------------------------------------------------------------------------------

### Composition

`Predicate`:

``` java
Predicate<Integer> positive = n -> n > 0;
Predicate<Integer> even = n -> n % 2 == 0;

Predicate<Integer> positiveEven = positive.and(even);
Predicate<Integer> notPositive = positive.negate();
Predicate<Integer> positiveOrEven = positive.or(even);
```

`Function`:

``` java
Function<String, String> trim = String::trim;
Function<String, String> upper = String::toUpperCase;

Function<String, String> trimThenUpper = trim.andThen(upper);
Function<String, String> upperThenTrim = trim.compose(upper);
```

-   `andThen(g)`: run the current function first, then `g`.
-   `compose(g)`: run `g` first, then the current function.

------------------------------------------------------------------------

## 6. Method References

A method reference is a shorter form of a lambda when an existing method
already performs the required action.

  ------------------------------------------------------------------------------
  Kind                    Syntax                  Equivalent lambda
  ----------------------- ----------------------- ------------------------------
  Static method           `Integer::parseInt`     `s -> Integer.parseInt(s)`

  Instance method of a    `System.out::println`   `x -> System.out.println(x)`
  particular object                               

  Instance method of an   `String::toLowerCase`   `s -> s.toLowerCase()`
  arbitrary object of a                           
  type                                            

  Constructor reference   `ArrayList::new`        `() -> new ArrayList<>()`
  ------------------------------------------------------------------------------

Examples:

``` java
List<String> names = Arrays.asList("akash", "ravi");
names.stream()
     .map(String::toUpperCase)
     .forEach(System.out::println);

Supplier<List<String>> listFactory = ArrayList::new;
Function<String, Integer> parser = Integer::parseInt;
```

**Interview tip:** A method reference is syntax, not a separate
execution model. Use it when it improves readability; a lambda may be
clearer when extra logic is required.

------------------------------------------------------------------------

## 7. Default and Static Methods in Interfaces

Java 8 allows interfaces to contain:

-   **Abstract methods:** no implementation.
-   **Default methods:** instance methods with a body, inherited by
    implementing classes.
-   **Static methods:** utility methods called using the interface name.

``` java
interface Vehicle {
    void start();

    default void honk() {
        System.out.println("Beep");
    }

    static boolean isMotorized() {
        return true;
    }
}

class Car implements Vehicle {
    @Override
    public void start() {
        System.out.println("Car started");
    }
}

Car car = new Car();
car.start();
car.honk();
boolean motorized = Vehicle.isMotorized();
```

### Why default methods?

They allow library authors to add behavior to interfaces without forcing
every existing implementation to add a new method implementation.

### Default-method conflict rules

If multiple interfaces provide conflicting default methods, the class
must resolve the conflict.

``` java
interface A {
    default void show() { System.out.println("A"); }
}
interface B {
    default void show() { System.out.println("B"); }
}

class C implements A, B {
    @Override
    public void show() {
        A.super.show(); // explicitly select A's implementation
    }
}
```

General precedence: 1. A method from a class takes precedence over an
interface default. 2. A more specific subinterface default takes
precedence over a less specific parent interface. 3. If unrelated
interfaces conflict, the implementing class must override the method.

An interface static method is called through the interface name and is
not inherited as an instance method by implementing classes.

------------------------------------------------------------------------

## 8. Optional

`Optional<T>` is a container that may or may not contain a non-null
value. It is mainly intended to make a possibly absent *return value*
explicit.

``` java
Optional<String> name = Optional.of("Akash");
Optional<String> maybeName = Optional.ofNullable(null);
Optional<String> empty = Optional.empty();
```

### Creating Optional

-   `Optional.of(value)`: throws `NullPointerException` if `value` is
    null.
-   `Optional.ofNullable(value)`: returns empty when value is null.
-   `Optional.empty()`: creates an empty Optional.

### Common operations

``` java
Optional<String> value = Optional.ofNullable(getName());

String result1 = value.orElse("Guest");
String result2 = value.orElseGet(() -> loadDefaultName());

value.ifPresent(System.out::println);

Optional<Integer> length = value.map(String::length);

Optional<String> nonBlank = value.filter(s -> !s.trim().isEmpty());

String required = value.orElseThrow(
        () -> new IllegalStateException("Name is required"));
```

### `orElse` vs `orElseGet`

-   `orElse(defaultValue)` evaluates the argument before the call, even
    if the Optional contains a value.
-   `orElseGet(supplier)` invokes the supplier only if the Optional is
    empty.

``` java
String a = Optional.of("present").orElse(expensiveDefault()); // expensiveDefault runs
String b = Optional.of("present").orElseGet(() -> expensiveDefault()); // supplier not run
```

### Best practices

-   Use Optional primarily as a return type when absence is a meaningful
    result.
-   Avoid calling `get()` without checking presence; prefer `orElse`,
    `orElseGet`, `orElseThrow`, `map`, or `ifPresent`.
-   Do not use Optional as a replacement for every nullable field,
    method parameter, or collection.
-   An Optional variable itself should not be null.
-   `map` transforms a present value; `filter` retains it only if the
    predicate passes.
-   Java 8 Optional does not provide `ifPresentOrElse`, `or`, or
    `stream`; those arrived in later Java versions.

------------------------------------------------------------------------

## 9. Date and Time API

The `java.time` API introduced in Java 8 is clearer and safer than the
legacy `Date` and `Calendar` APIs. Its main date/time value types are
immutable and thread-safe.

  -----------------------------------------------------------------------
  Type                    Represents              Example
  ----------------------- ----------------------- -----------------------
  `LocalDate`             Date without time or    `2026-10-13`
                          zone                    

  `LocalTime`             Time without date or    `10:30`
                          zone                    

  `LocalDateTime`         Date and time without   `2026-10-13T10:30`
                          zone                    

  `ZonedDateTime`         Date/time with a time   date/time in
                          zone                    `Asia/Kolkata`

  `OffsetDateTime`        Date/time with UTC      `+05:30`
                          offset                  

  `Instant`               Point on the UTC        timestamp
                          timeline                

  `Duration`              Time-based amount       hours, seconds

  `Period`                Date-based amount       years, months, days

  `DateTimeFormatter`     Parsing/formatting      `dd-MM-yyyy`
  -----------------------------------------------------------------------

``` java
LocalDate today = LocalDate.now();
LocalDate joiningDate = LocalDate.of(2024, 3, 1);
LocalDate nextWeek = today.plusWeeks(1);

LocalTime now = LocalTime.now();
LocalDateTime meeting = LocalDateTime.of(today, LocalTime.of(14, 30));

ZonedDateTime indiaTime = ZonedDateTime.now(ZoneId.of("Asia/Kolkata"));
Instant timestamp = Instant.now();

DateTimeFormatter formatter = DateTimeFormatter.ofPattern("dd-MM-yyyy");
String formatted = today.format(formatter);
LocalDate parsed = LocalDate.parse("13-10-2026", formatter);
```

### `Period` vs `Duration`

-   `Period`: date-based units such as years, months, and days.
-   `Duration`: time-based units such as seconds and nanoseconds.

``` java
Period period = Period.between(LocalDate.of(2024, 1, 1), LocalDate.of(2025, 3, 1));
Duration duration = Duration.between(
        LocalTime.of(9, 0), LocalTime.of(17, 30));
```

Use a `ZoneId` when a real-world time zone matters. `LocalDateTime` by
itself does not identify a unique global instant.

------------------------------------------------------------------------

## 10. Stream API Fundamentals

A **Stream** is a sequence of elements supporting aggregate operations.
It is not a data structure and does not itself store the data. A stream
commonly reads from a collection, array, generator, or I/O source and
processes elements through a pipeline.

### Typical pipeline

``` java
List<String> result = names.stream()             // source
        .filter(name -> name.startsWith("A"))    // intermediate operation
        .map(String::toUpperCase)                // intermediate operation
        .sorted()                                // intermediate operation
        .collect(Collectors.toList());           // terminal operation
```

### Three parts of a stream pipeline

1.  **Source:** collection, array, generated values, file lines, etc.
2.  **Intermediate operations:** transform/filter a stream and return
    another stream.
3.  **Terminal operation:** produces a result or side effect and
    triggers processing.

### Key properties

-   A stream does not normally modify its source.
-   Stream operations are often lazy until a terminal operation runs.
-   A stream should be used only once; after a terminal operation it is
    consumed.
-   A stream may be sequential or parallel.
-   Stream pipelines are usually written as a chain of transformations.
-   Streams do not necessarily process all elements; short-circuit
    operations can stop early.

------------------------------------------------------------------------

## 11. Creating Streams

``` java
List<String> names = Arrays.asList("Asha", "Akash", "Ravi");

// From a collection
Stream<String> s1 = names.stream();
Stream<String> s2 = names.parallelStream();

// From values
Stream<String> s3 = Stream.of("A", "B", "C");

// From an array
int[] values = {1, 2, 3};
IntStream s4 = Arrays.stream(values);

// Empty stream
Stream<String> empty = Stream.empty();

// Generate values (potentially infinite)
Stream<Double> randoms = Stream.generate(Math::random);

// Iterate values (potentially infinite)
Stream<Integer> integers = Stream.iterate(1, n -> n + 1);
List<Integer> firstFive = Stream.iterate(1, n -> n + 1)
        .limit(5)
        .collect(Collectors.toList());

// From a file; close the stream to release resources
try (Stream<String> lines = Files.lines(Paths.get("input.txt"))) {
    lines.filter(line -> !line.trim().isEmpty())
         .forEach(System.out::println);
}
```

`Stream.iterate(seed, nextFunction)` and `Stream.generate(supplier)` can
produce infinite streams, so use limiting/short-circuit operations when
finite processing is required.

### Stream from a Map

A Map is not directly a `Collection`; use its views:

``` java
Map<String, Integer> scores = new HashMap<>();
scores.put("Asha", 90);
scores.put("Ravi", 82);

scores.entrySet().stream()
      .filter(e -> e.getValue() >= 85)
      .forEach(e -> System.out.println(e.getKey() + ": " + e.getValue()));

scores.keySet().stream().forEach(System.out::println);
scores.values().stream().forEach(System.out::println);
```

------------------------------------------------------------------------

## 12. Intermediate Operations

Intermediate operations return another stream. Most are lazy: they
describe work that will be performed when a terminal operation is
invoked.

### `filter(Predicate<T>)`

Keeps elements that satisfy a condition.

``` java
List<Integer> evens = numbers.stream()
        .filter(n -> n % 2 == 0)
        .collect(Collectors.toList());
```

### `map(Function<T,R>)`

Transforms each element into one output element.

``` java
List<Integer> lengths = names.stream()
        .map(String::length)
        .collect(Collectors.toList());
```

### `flatMap(Function<T, Stream<R>>)`

Transforms each element into a stream and flattens the results into one
stream. Useful for nested collections.

``` java
List<List<String>> nested = Arrays.asList(
        Arrays.asList("A", "B"),
        Arrays.asList("C", "D"));

List<String> flat = nested.stream()
        .flatMap(List::stream)
        .collect(Collectors.toList());
// [A, B, C, D]
```

**`map` vs `flatMap`:** `map` preserves one output per input (the output
may itself be a collection/stream); `flatMap` combines nested streams
into a single stream.

### `distinct()`

Removes duplicate elements using `equals()`/`hashCode()` semantics.

``` java
List<Integer> unique = Arrays.asList(1, 2, 2, 3, 1).stream()
        .distinct()
        .collect(Collectors.toList());
```

### `sorted()`

Natural ordering, where elements must be comparable.

``` java
List<Integer> sorted = numbers.stream().sorted().collect(Collectors.toList());
```

### `sorted(Comparator)`

Custom ordering.

``` java
List<String> byLength = names.stream()
        .sorted(Comparator.comparingInt(String::length)
                .thenComparing(Comparator.naturalOrder()))
        .collect(Collectors.toList());
```

### `limit(n)` and `skip(n)`

``` java
List<Integer> firstThree = numbers.stream().limit(3).collect(Collectors.toList());
List<Integer> afterTwo = numbers.stream().skip(2).collect(Collectors.toList());
```

### `peek(Consumer<T>)`

Primarily intended for observing elements while debugging; it is an
intermediate operation.

``` java
List<Integer> result = numbers.stream()
        .filter(n -> n > 2)
        .peek(n -> System.out.println("After filter: " + n))
        .map(n -> n * 10)
        .collect(Collectors.toList());
```

Do not rely on `peek` for required business side effects. Because stream
execution can be lazy or optimized, the action may not run in the way a
developer expects.

### `takeWhile` / `dropWhile` note

These are **not Java 8 Stream methods**. They were added in Java 9. For
Java 8 compatibility, do not use them.

------------------------------------------------------------------------

## 13. Terminal Operations

Terminal operations trigger the pipeline and produce a non-stream result
or side effect.

### `collect`

Accumulates elements into a collection or another result container.

``` java
List<String> list = names.stream().collect(Collectors.toList());
Set<String> set = names.stream().collect(Collectors.toSet());
```

### `forEach` and `forEachOrdered`

``` java
names.stream().forEach(System.out::println);
names.parallelStream().forEachOrdered(System.out::println);
```

`forEach` does not promise encounter order for parallel streams.
`forEachOrdered` respects encounter order when one exists, potentially
limiting parallel performance.

### `count`

``` java
long count = numbers.stream().filter(n -> n > 10).count();
```

### `min` and `max`

They return `Optional<T>` because the stream may be empty.

``` java
Optional<Integer> min = numbers.stream().min(Integer::compareTo);
Optional<Integer> max = numbers.stream().max(Integer::compareTo);
```

### `findFirst` and `findAny`

Both return `Optional<T>`. `findFirst` requests the first element in
encounter order; `findAny` allows any matching element and can be more
flexible for parallel processing.

``` java
Optional<String> first = names.stream().filter(n -> n.startsWith("A")).findFirst();
Optional<String> any = names.parallelStream().filter(n -> n.length() > 3).findAny();
```

### `anyMatch`, `allMatch`, `noneMatch`

These are short-circuiting boolean operations.

``` java
boolean hasNegative = numbers.stream().anyMatch(n -> n < 0);
boolean allPositive = numbers.stream().allMatch(n -> n > 0);
boolean noNulls = names.stream().noneMatch(Objects::isNull);
```

Be aware of vacuous truth: `allMatch` on an empty stream is `true`, and
`noneMatch` on an empty stream is also `true`; `anyMatch` on an empty
stream is `false`.

### `reduce`

Combines elements into one result.

``` java
int sum = numbers.stream().reduce(0, Integer::sum);

Optional<Integer> maximum = numbers.stream().reduce(Integer::max);

String joined = names.stream().reduce(
        "",
        (a, b) -> a.isEmpty() ? b : a + ", " + b);
```

For ordinary numeric aggregation, primitive streams or
`Collectors.joining()` may be clearer and more efficient than a custom
reduction.

A reduction accumulator should be associative, and the identity value
must be correct, especially when the stream is parallel. Avoid mutable
shared objects in a `reduce` operation.

### `toArray`

``` java
Object[] array = names.stream().toArray();
String[] stringArray = names.stream().toArray(String[]::new);
```

------------------------------------------------------------------------

## 14. Collectors and `Collectors.groupingBy`

`Collectors` provides reusable collector implementations for building
lists, sets, maps, grouped data, and summary statistics.

### Common collectors

``` java
List<String> list = names.stream().collect(Collectors.toList());
Set<String> set = names.stream().collect(Collectors.toSet());

String joined = names.stream().collect(Collectors.joining(", "));

Map<Integer, List<String>> byLength = names.stream()
        .collect(Collectors.groupingBy(String::length));

Map<Boolean, List<String>> partitioned = names.stream()
        .collect(Collectors.partitioningBy(s -> s.length() > 4));
```

### `groupingBy`

Groups elements according to a classifier function.

``` java
class Employee {
    private final String name;
    private final String department;
    private final int salary;

    Employee(String name, String department, int salary) {
        this.name = name;
        this.department = department;
        this.salary = salary;
    }
    public String getName() { return name; }
    public String getDepartment() { return department; }
    public int getSalary() { return salary; }
}

Map<String, List<Employee>> employeesByDepartment = employees.stream()
        .collect(Collectors.groupingBy(Employee::getDepartment));
```

### Downstream collectors

Group and calculate a result for each group:

``` java
// Employee count per department
Map<String, Long> countByDepartment = employees.stream()
        .collect(Collectors.groupingBy(
                Employee::getDepartment,
                Collectors.counting()));

// Total salary per department
Map<String, Integer> salaryByDepartment = employees.stream()
        .collect(Collectors.groupingBy(
                Employee::getDepartment,
                Collectors.summingInt(Employee::getSalary)));

// Average salary per department
Map<String, Double> averageSalaryByDepartment = employees.stream()
        .collect(Collectors.groupingBy(
                Employee::getDepartment,
                Collectors.averagingInt(Employee::getSalary)));

// Names per department
Map<String, List<String>> namesByDepartment = employees.stream()
        .collect(Collectors.groupingBy(
                Employee::getDepartment,
                Collectors.mapping(Employee::getName, Collectors.toList())));
```

### `partitioningBy`

Partitions into exactly two groups based on a predicate: `true` and
`false`.

``` java
Map<Boolean, List<Employee>> highPaid = employees.stream()
        .collect(Collectors.partitioningBy(e -> e.getSalary() >= 100000));
```

Difference: - `groupingBy`: can create many groups based on classifier
keys. - `partitioningBy`: creates two predicate-based groups.

### `toMap`

``` java
Map<String, Integer> salaryByName = employees.stream()
        .collect(Collectors.toMap(Employee::getName, Employee::getSalary));
```

If duplicate keys occur, the two-argument `toMap` throws
`IllegalStateException`. Provide a merge function when duplicates are
possible:

``` java
Map<String, Integer> totalByName = employees.stream()
        .collect(Collectors.toMap(
                Employee::getName,
                Employee::getSalary,
                Integer::sum));
```

### `summarizingInt`

``` java
IntSummaryStatistics stats = employees.stream()
        .collect(Collectors.summarizingInt(Employee::getSalary));

long count = stats.getCount();
int minSalary = stats.getMin();
int maxSalary = stats.getMax();
double average = stats.getAverage();
long total = stats.getSum();
```

**Note:** `Collectors.toList()` does not promise a particular concrete
list implementation or mutability contract. If mutability is required,
explicitly collect into a chosen collection with
`collect(ArrayList::new, ArrayList::add, ArrayList::addAll)`.

------------------------------------------------------------------------

## 15. Primitive Streams and Numeric Operations

Java provides primitive-specialized streams to avoid unnecessary
boxing/unboxing for numeric workloads:

-   `IntStream`
-   `LongStream`
-   `DoubleStream`

``` java
int sum = IntStream.range(1, 5).sum();       // 1 + 2 + 3 + 4 = 10
int sumInclusive = IntStream.rangeClosed(1, 5).sum(); // 15

OptionalDouble average = numbers.stream()
        .mapToInt(Integer::intValue)
        .average();

int[] squares = IntStream.rangeClosed(1, 5)
        .map(n -> n * n)
        .toArray();
```

### Useful primitive operations

-   `sum()`
-   `average()` → `OptionalDouble`
-   `min()` / `max()` → `OptionalInt`, `OptionalLong`, or
    `OptionalDouble`
-   `summaryStatistics()`
-   `range(start, end)` (end exclusive)
-   `rangeClosed(start, end)` (end inclusive)
-   `boxed()` converts primitive values to wrapper-object streams.
-   `mapToInt`, `mapToLong`, `mapToDouble` convert object streams to
    primitive streams.

Example:

``` java
IntSummaryStatistics stats = IntStream.of(10, 20, 30).summaryStatistics();
System.out.println(stats.getSum());     // 60
System.out.println(stats.getAverage()); // 20.0
```

------------------------------------------------------------------------

## 16. Parallel Streams

A parallel stream divides work into subtasks, commonly using the
Fork/Join common pool, and processes them concurrently. Use
`parallelStream()` or `stream().parallel()`.

``` java
long count = numbers.parallelStream()
        .filter(n -> n % 2 == 0)
        .count();
```

### When parallel streams may help

-   Large data sets.
-   Independent, CPU-intensive operations.
-   Work that can be split efficiently.
-   Low coordination and low shared-state contention.
-   Correct associative reductions or collectors.

### When parallel streams may hurt

-   Small data sets where scheduling overhead dominates.
-   Blocking I/O operations.
-   Shared mutable state or synchronized bottlenecks.
-   Order-sensitive processing.
-   Workloads where common-pool contention affects other tasks.
-   Operations that are expensive to split or combine.

### Rules for safe parallel processing

1.  Keep transformations stateless and non-interfering.
2.  Do not mutate shared collections from `forEach`.
3.  Use correct reductions and collectors.
4.  Measure with realistic data before choosing parallel execution.
5.  Remember that parallel does not mean "always faster."
6.  Do not assume a particular thread count or scheduling order.

------------------------------------------------------------------------

## 17. Stream Laziness, Execution, and Reuse

### Laziness

Intermediate operations do not normally process elements until a
terminal operation is invoked.

``` java
Stream<Integer> stream = numbers.stream()
        .filter(n -> {
            System.out.println("Checking " + n);
            return n > 2;
        });

// No output yet: no terminal operation has started the pipeline
long count = stream.count();
```

### Short-circuiting

Operations such as `findFirst`, `findAny`, `anyMatch`, `allMatch`,
`noneMatch`, and `limit` may stop processing once enough information is
available.

``` java
boolean found = numbers.stream()
        .peek(System.out::println)
        .anyMatch(n -> n > 3);
```

Only as many elements as needed may be examined. Do not assume every
element will be visited.

### A stream cannot be reused

``` java
Stream<String> stream = names.stream();
long count = stream.count();
// stream.forEach(System.out::println); // IllegalStateException
```

Create a new stream from the source for another pipeline:

``` java
long count2 = names.stream().count();
names.stream().forEach(System.out::println);
```

### Stateful vs stateless intermediate operations

-   Typically stateless: `filter`, `map`, `flatMap`, `peek`.
-   Stateful: `distinct`, `sorted`; they may need information about
    multiple elements before producing results.

The exact execution strategy is implementation-dependent; avoid
depending on the number or order of lambda invocations except where the
API guarantees it.

------------------------------------------------------------------------

## 18. Common Stream Recipes

The examples below assume appropriate variables are already declared.

### 18.1 Filter even numbers and square them

``` java
List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .map(n -> n * n)
        .collect(Collectors.toList());
```

### 18.2 Find duplicate values

``` java
Set<Integer> seen = new HashSet<>();
Set<Integer> duplicates = numbers.stream()
        .filter(n -> !seen.add(n))
        .collect(Collectors.toSet());
```

This example uses mutable state and is intended for a sequential stream
only. Do not change it to `parallelStream()`; use a different
frequency-counting approach for parallel-safe logic.

Alternative using frequencies:

``` java
Map<Integer, Long> frequency = numbers.stream()
        .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));

Set<Integer> duplicates2 = frequency.entrySet().stream()
        .filter(e -> e.getValue() > 1)
        .map(Map.Entry::getKey)
        .collect(Collectors.toSet());
```

### 18.3 Frequency of each word

``` java
List<String> words = Arrays.asList("java", "sql", "java", "spring");
Map<String, Long> frequency = words.stream()
        .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
```

### 18.4 First non-repeated character

``` java
String input = "swiss";
Map<Character, Long> counts = input.chars()
        .mapToObj(c -> (char) c)
        .collect(Collectors.groupingBy(
                Function.identity(),
                LinkedHashMap::new,
                Collectors.counting()));

Character firstUnique = counts.entrySet().stream()
        .filter(e -> e.getValue() == 1)
        .map(Map.Entry::getKey)
        .findFirst()
        .orElse(null);
```

This uses UTF-16 `char` values and is suitable for ordinary
basic-character examples. Full Unicode code-point handling requires
`codePoints()` and a corresponding representation.

### 18.5 Second-highest distinct number

``` java
Optional<Integer> secondHighest = numbers.stream()
        .distinct()
        .sorted(Comparator.reverseOrder())
        .skip(1)
        .findFirst();
```

This is simple and readable, but sorting costs more than a one-pass
solution for very large inputs.

### 18.6 Top 3 highest numbers

``` java
List<Integer> topThree = numbers.stream()
        .distinct()
        .sorted(Comparator.reverseOrder())
        .limit(3)
        .collect(Collectors.toList());
```

### 18.7 Convert list to map

``` java
Map<Integer, String> idToName = people.stream()
        .collect(Collectors.toMap(Person::getId, Person::getName));
```

Use a merge function if IDs may repeat.

### 18.8 Sort employees by salary

``` java
List<Employee> sortedEmployees = employees.stream()
        .sorted(Comparator.comparingInt(Employee::getSalary))
        .collect(Collectors.toList());
```

Descending:

``` java
List<Employee> descending = employees.stream()
        .sorted(Comparator.comparingInt(Employee::getSalary).reversed())
        .collect(Collectors.toList());
```

### 18.9 Highest-paid employee

``` java
Optional<Employee> highestPaid = employees.stream()
        .max(Comparator.comparingInt(Employee::getSalary));
```

### 18.10 Highest salary by department

``` java
Map<String, Optional<Employee>> highestPaidByDepartment = employees.stream()
        .collect(Collectors.groupingBy(
                Employee::getDepartment,
                Collectors.maxBy(Comparator.comparingInt(Employee::getSalary))));
```

### 18.11 Names joined by comma

``` java
String joinedNames = names.stream().collect(Collectors.joining(", "));
```

### 18.12 Flatten nested lists

``` java
List<Integer> flattened = nestedLists.stream()
        .flatMap(Collection::stream)
        .collect(Collectors.toList());
```

### 18.13 Separate even and odd numbers

``` java
Map<Boolean, List<Integer>> evenOdd = numbers.stream()
        .collect(Collectors.partitioningBy(n -> n % 2 == 0));

List<Integer> evens = evenOdd.get(true);
List<Integer> odds = evenOdd.get(false);
```

### 18.14 Safely handle possible nulls in a source list

Prefer avoiding null elements in the first place. If they may exist:

``` java
List<String> nonNullNames = names.stream()
        .filter(Objects::nonNull)
        .collect(Collectors.toList());
```

### 18.15 Sum salaries by department

``` java
Map<String, Integer> totalSalary = employees.stream()
        .collect(Collectors.groupingBy(
                Employee::getDepartment,
                Collectors.summingInt(Employee::getSalary)));
```

------------------------------------------------------------------------

## 19. Streams vs Collections and Loops

  ----------------------------------------------------------------------------------
  Aspect            Collection        Stream                       Traditional loop
  ----------------- ----------------- ---------------------------- -----------------
  Main role         Stores/manages    Processes data through a     Executes explicit
                    data              pipeline                     iteration logic

  Storage           Holds elements    Does not itself store source Depends on source
                                      elements                     

  Reuse             Can usually       A stream is consumed once    Can usually run
                    iterate                                        loop again
                    repeatedly                                     

  Style             Data              Declarative transformations  Imperative steps
                    structure/API                                  

  Lazy behavior     Collection        Intermediate stream          Loop executes
                    operations        operations are lazy          when reached
                    generally act                                  
                    when called                                    

  Parallelism       Not automatic     Supports parallel pipelines  Must be
                                                                   implemented
                                                                   explicitly

  Best for          Data ownership    Filtering/mapping/reducing   Complex control
                    and manipulation  pipelines                    flow, early
                                                                   exits, mutation
                                                                   where appropriate
  ----------------------------------------------------------------------------------

Streams are not a universal replacement for loops. A loop can be clearer
for complex branching, index-based logic, checked exceptions, or early
control flow. Choose the form that communicates intent best.

------------------------------------------------------------------------

## 20. Performance, Best Practices, and Common Mistakes

### Best practices

-   Prefer readable pipelines over very long, difficult-to-debug chains.
-   Use `map` for one-to-one transformations and `flatMap` for
    flattening nested results.
-   Use primitive streams for numeric calculations when appropriate.
-   Use `groupingBy`, `partitioningBy`, `counting`, `summingInt`, and
    `summarizingInt` rather than manually maintaining maps where
    practical.
-   Use `Optional` deliberately; do not use `get()` blindly.
-   Keep stream lambdas stateless and avoid source mutation during
    traversal.
-   Close streams backed by resources, such as `Files.lines`, with
    try-with-resources.
-   Use `findFirst` when encounter order matters; `findAny` when any
    matching result is acceptable.
-   Benchmark before using parallel streams.
-   Use method references when they improve clarity.

### Common mistakes

**1. Reusing a stream**

``` java
Stream<Integer> s = numbers.stream();
s.count();
// s.count(); // IllegalStateException
```

**2. Calling `Optional.get()` without checking**

``` java
Optional<Integer> value = numbers.stream().filter(n -> n > 1000).findFirst();
// value.get(); // may throw NoSuchElementException
```

**3. Duplicate keys in `toMap`**

``` java
// Can throw IllegalStateException if two people have the same ID
Map<Integer, String> map = people.stream()
        .collect(Collectors.toMap(Person::getId, Person::getName));
```

Provide a merge function when needed.

**4. Side effects in parallel pipelines** Do not append to an ordinary
`ArrayList` from `parallelStream().forEach(...)`. Use a collector.

**5. Confusing `map` and `flatMap`** `map` can produce a stream of
lists; `flatMap` flattens their contents.

**6. Assuming `forEach` preserves order** Parallel `forEach` may process
elements in a different order. Use `forEachOrdered` only when ordering
is necessary.

**7. Assuming `sorted()` is free** Sorting generally requires extra work
and may need to buffer elements.

**8. Using streams for every task** A plain loop may be more readable
for complicated stateful algorithms.

**9. Assuming `Collectors.toList()` returns an `ArrayList`** The API
does not guarantee a particular concrete type or mutability.

**10. Treating `peek` as a business-operation hook** Use `peek` mainly
for debugging; do not make essential behavior depend on it.

------------------------------------------------------------------------

## 21. Java 8 Features in Real Applications

### Example: filter and transform API results

``` java
List<String> activeUserEmails = users.stream()
        .filter(User::isActive)
        .map(User::getEmail)
        .filter(Objects::nonNull)
        .map(String::trim)
        .filter(email -> !email.isEmpty())
        .distinct()
        .collect(Collectors.toList());
```

### Example: optional repository result

``` java
public String getDisplayName(Long id) {
    return userRepository.findById(id)
            .map(User::getName)
            .filter(name -> !name.trim().isEmpty())
            .orElse("Unknown user");
}
```

This is illustrative: actual repository APIs and entity accessors depend
on the framework and application.

### Example: asynchronous composition with `CompletableFuture`

``` java
CompletableFuture<String> future =
        CompletableFuture.supplyAsync(() -> loadUserName())
                .thenApply(String::toUpperCase)
                .exceptionally(ex -> "UNKNOWN");

String result = future.join();
```

`CompletableFuture` can compose asynchronous work. Be mindful of
executor selection, error handling, and blocking calls such as `join()`.

### Collection enhancements introduced in Java 8

``` java
List<String> values = new ArrayList<>(Arrays.asList("a", "", "b"));
values.removeIf(String::isEmpty);

Map<String, Integer> counts = new HashMap<>();
counts.putIfAbsent("java", 0);
counts.computeIfAbsent("spring", key -> key.length());
counts.merge("java", 1, Integer::sum);
```

Other useful additions include `forEach` on `Iterable` and Map methods
such as `forEach`, `compute`, `computeIfPresent`, `computeIfAbsent`,
`merge`, `replaceAll`, and `getOrDefault`.

### Base64

``` java
String encoded = Base64.getEncoder()
        .encodeToString("hello".getBytes(StandardCharsets.UTF_8));

byte[] decoded = Base64.getDecoder().decode(encoded);
String original = new String(decoded, StandardCharsets.UTF_8);
```

Base64 is encoding, not encryption.

------------------------------------------------------------------------

## 22. Interview Comparisons

### `map()` vs `flatMap()`

  -----------------------------------------------------------------------
  `map()`                             `flatMap()`
  ----------------------------------- -----------------------------------
  One output value per input element  Each input can produce zero or more
                                      output elements

  Does not flatten nested streams     Flattens mapped streams into one
                                      stream

  `Function<T,R>`                     `Function<T, Stream<R>>` for a
                                      stream pipeline

  Example: string to length           Example: list of lists to list
  -----------------------------------------------------------------------

### `filter()` vs `map()`

-   `filter`: decides whether an element remains.
-   `map`: transforms each element into another value.

### `findFirst()` vs `findAny()`

-   `findFirst`: first element according to encounter order, when
    defined.
-   `findAny`: any matching element; can allow more flexibility in
    parallel execution.

### `forEach()` vs `forEachOrdered()`

-   `forEach`: no ordering guarantee for parallel execution.
-   `forEachOrdered`: respects encounter order where one exists.

### `reduce()` vs `collect()`

-   `reduce`: combines values into a single result using an associative
    operation.
-   `collect`: performs mutable reduction into a result container such
    as a list, map, or grouped result.

### `groupingBy()` vs `partitioningBy()`

-   `groupingBy`: potentially many groups keyed by a classifier.
-   `partitioningBy`: exactly two predicate-based partitions (`true` and
    `false`).

### `orElse()` vs `orElseGet()`

-   `orElse`: default expression is evaluated before the method call.
-   `orElseGet`: supplier is invoked only when the Optional is empty.

### `Stream` vs `parallelStream()`

-   `stream()`: sequential by default.
-   `parallelStream()`: parallel processing may occur; actual speed
    depends on source, workload, ordering, and contention.

### `Predicate` vs `Function` vs `Consumer` vs `Supplier`

  Interface         Returns     Use
  ----------------- ----------- ------------------------
  `Predicate<T>`    `boolean`   Test a condition
  `Function<T,R>`   `R`         Transform a value
  `Consumer<T>`     `void`      Consume/act on a value
  `Supplier<T>`     `T`         Provide a value

------------------------------------------------------------------------

## 23. Interview Questions and Answers

### Q1. What are the major features introduced in Java 8?

Lambdas, functional interfaces, method references, Stream API, Optional,
default/static interface methods, the `java.time` API,
`CompletableFuture`, Base64, and enhancements to collections and Map.

### Q2. What is a functional interface?

An interface with exactly one abstract method, making it a valid target
type for a lambda or method reference. It can still have default/static
methods and methods corresponding to public `Object` methods.

### Q3. Can a functional interface have multiple default methods?

Yes. The single-abstract-method rule concerns abstract methods, not
default or static methods.

### Q4. What is the difference between a lambda and an anonymous inner class?

A lambda implements a functional interface and does not introduce a new
`this`; an anonymous inner class creates an anonymous class instance
with its own `this`. Lambdas are generally more concise and have
different scoping/compilation behavior.

### Q5. What is a Stream?

A pipeline abstraction for processing a sequence of elements with
aggregate operations. It does not itself store the data and is normally
consumed once.

### Q6. Does a stream modify the original collection?

Not by default. Stream operations should generally be non-interfering;
however, a lambda can still mutate external state if written to do so,
which is often unsafe.

### Q7. What is lazy evaluation in streams?

Intermediate operations describe transformations but normally do not
process elements until a terminal operation starts execution.

### Q8. What is the difference between intermediate and terminal operations?

Intermediate operations return a stream, such as `filter`, `map`, and
`sorted`. Terminal operations produce a result or side effect, such as
`collect`, `count`, `reduce`, and `forEach`.

### Q9. Is a stream reusable?

No. A stream should be operated on only once. Create a new stream from
the source for another pipeline.

### Q10. Explain `map()` and `flatMap()`.

`map` transforms each element into one result. `flatMap` transforms each
element into a stream and flattens those streams into a single output
stream.

### Q11. What is the difference between `filter()` and `map()`?

`filter` retains or removes elements based on a predicate. `map`
transforms elements.

### Q12. What are short-circuiting operations?

Operations that may finish without processing every element, such as
`findFirst`, `findAny`, `anyMatch`, `allMatch`, `noneMatch`, and
`limit`.

### Q13. What is the difference between `findFirst()` and `findAny()`?

`findFirst` requests the first element in encounter order. `findAny` can
return any matching element and is often more flexible for parallel
processing.

### Q14. What is the difference between `reduce()` and `collect()`?

`reduce` combines elements into a single value; `collect` accumulates
them into a result container, often using a mutable reduction.

### Q15. What is `Collectors.groupingBy()`?

A collector that groups stream elements by a classifier function and
returns a map from keys to grouped results. It supports downstream
collectors such as `counting`, `mapping`, `summingInt`, and
`averagingInt`.

### Q16. How do you handle duplicate keys in `Collectors.toMap()`?

Use the overload with a merge function, for example
`Collectors.toMap(keyMapper, valueMapper, (oldValue, newValue) -> oldValue)`.

### Q17. Why does `findFirst()` return an Optional?

Because the stream may contain no elements matching the requested
operation. Optional represents possible absence without returning null.

### Q18. What is the difference between `orElse()` and `orElseGet()`?

The argument to `orElse` is evaluated before the call; `orElseGet`
invokes its supplier only when the Optional is empty.

### Q19. Are streams always faster than loops?

No. Streams can improve readability, but overhead, boxing, allocation,
and pipeline complexity may affect performance. Parallel streams help
only for suitable workloads. Measure.

### Q20. Are parallel streams thread-safe automatically?

The stream framework handles splitting and combination, but user lambdas
must be safe. Shared mutable state can cause races or incorrect results.

### Q21. What is the difference between `IntStream.range()` and `rangeClosed()`?

`range(start, end)` excludes `end`; `rangeClosed(start, end)` includes
it.

### Q22. What does `distinct()` use to detect duplicates?

Object streams use equality semantics (`equals` and `hashCode`) for
distinctness.

### Q23. What is the difference between `groupingBy` and `partitioningBy`?

`groupingBy` can produce many groups. `partitioningBy` divides values
into two groups based on a predicate.

### Q24. Why is `java.time` better than legacy date/time classes?

The main value types are immutable and thread-safe, APIs are clearer,
and date/time, offset, time zone, and duration concepts are represented
more explicitly.

### Q25. What are effectively final variables in lambdas?

Local variables that are not declared `final` but are assigned only once
and never reassigned. Lambdas can capture them.

### Q26. What is the purpose of default methods in interfaces?

To add behavior to an interface while improving backward compatibility
for existing implementations.

### Q27. What is `peek()` used for?

Primarily for observing elements during pipeline processing, often for
debugging. Do not rely on it for required business side effects.

### Q28. Can a Stream be infinite?

Yes. `Stream.generate` and `Stream.iterate` can produce infinite
streams. Use limiting or short-circuit operations where needed.

### Q29. What happens if `allMatch()` is called on an empty stream?

It returns `true`. There is no element that violates the predicate.
`noneMatch` also returns `true`, while `anyMatch` returns `false`.

### Q30. How would you find the second-highest distinct salary?

One readable approach is:

``` java
Optional<Integer> second = employees.stream()
        .map(Employee::getSalary)
        .distinct()
        .sorted(Comparator.reverseOrder())
        .skip(1)
        .findFirst();
```

For very large data sets, consider a one-pass approach if profiling
shows sorting is costly.

------------------------------------------------------------------------

## 24. Quick Revision Cheat Sheet

### Lambda and functional interface

``` java
Predicate<Integer> even = n -> n % 2 == 0;
Function<String, Integer> length = String::length;
Consumer<String> print = System.out::println;
Supplier<List<String>> factory = ArrayList::new;
```

### Stream pipeline

``` java
List<String> output = input.stream()
        .filter(Objects::nonNull)
        .map(String::trim)
        .filter(s -> !s.isEmpty())
        .distinct()
        .sorted()
        .collect(Collectors.toList());
```

### Frequent operations

-   Filter: `filter(predicate)`
-   Transform: `map(function)`
-   Flatten: `flatMap(function)`
-   Remove duplicates: `distinct()`
-   Sort: `sorted()` / `sorted(comparator)`
-   First N: `limit(n)`
-   Skip N: `skip(n)`
-   Count: `count()`
-   Aggregate: `reduce(...)`
-   Collect list: `collect(Collectors.toList())`
-   Group: `collect(Collectors.groupingBy(...))`
-   Partition: `collect(Collectors.partitioningBy(...))`
-   Join strings: `collect(Collectors.joining(", "))`
-   Any/all/none: `anyMatch`, `allMatch`, `noneMatch`
-   Optional result: `findFirst`, `findAny`, `min`, `max`

### Five rules to remember

1.  Intermediate operations are generally lazy.
2.  A stream is consumed once.
3.  Use `map` to transform and `flatMap` to flatten.
4.  Avoid shared mutable state, especially in parallel streams.
5.  Streams improve expression, not guaranteed performance.

------------------------------------------------------------------------

## 25. Practice Exercises

Try solving these without looking at the recipes first.

1.  Given a list of integers, return only odd numbers, square them, and
    collect them into a list.
2.  Find the maximum and minimum values in a list using streams.
3.  Find the second-highest distinct number.
4.  Count how many times each word appears in a list.
5.  Find the first non-repeated character in a string while preserving
    encounter order.
6.  Given employees, group them by department.
7.  Count employees in each department.
8.  Calculate average salary per department.
9.  Find the highest-paid employee in each department.
10. Convert a list of employees to a map keyed by employee ID, handling
    duplicate IDs.
11. Partition a list of numbers into even and odd values.
12. Flatten a list of lists into a single list.
13. Sort employees by salary descending, then by name ascending.
14. Join non-null, non-empty strings using a comma.
15. Use Optional to return a default display name when a repository
    result is absent.
16. Demonstrate the difference between `orElse` and `orElseGet`.
17. Explain why adding to an `ArrayList` inside
    `parallelStream().forEach` is unsafe.
18. Use `IntStream.rangeClosed` to calculate the sum from 1 to 100.
19. Given a list of transactions, group by type and calculate the total
    amount for each type.
20. Write an example of a stream that short-circuits using `anyMatch`.

**Suggested practice order:** lambdas and functional interfaces →
`filter`/`map`/`flatMap` → terminal operations → collectors →
employee/data-processing problems → Optional and Date-Time API →
parallel streams.

------------------------------------------------------------------------

## 26. References

Use official documentation as the source of truth for exact API
behavior.

-   [Oracle Java 8
    Features](https://www.oracle.com/java/technologies/javase/8-whats-new.html)
-   [Java SE 8 API
    Documentation](https://docs.oracle.com/javase/8/docs/api/)
-   [Java SE 8 `java.util.stream`
    package](https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html)
-   [Java SE 8 `Stream`
    API](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html)
-   [Java SE 8 `Collectors`
    API](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html)
-   [Java SE 8 `java.util.function`
    package](https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html)
-   [Java SE 8 `Optional`
    API](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)
-   [Java SE 8 Date-Time
    API](https://docs.oracle.com/javase/8/docs/api/java/time/package-summary.html)
-   [Java Language Specification, Java SE
    8](https://docs.oracle.com/javase/specs/jls/se8/html/index.html)

------------------------------------------------------------------------

**End of notes.** Keep these notes as a reference, but practice writing
the examples yourself. In interviews, explain not only the syntax but
also why you would choose a stream, what happens with empty input,
whether order matters, and whether side effects are safe.
