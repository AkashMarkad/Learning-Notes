# Table of Contents

- [1. What Are Behavioral Design Patterns?](#1-what-are-behavioral-design-patterns)
- [2. Why Do We Need Behavioral Patterns?](#2-why-do-we-need-behavioral-patterns)
- [3. The 11 GoF Behavioral Design Patterns](#3-the-11-gof-behavioral-design-patterns)
- [4. Quick Comparison](#4-quick-comparison)
- [5. Chain of Responsibility Pattern](#5-chain-of-responsibility-pattern)
  - [5.1 Definition and Intent](#51-definition-and-intent)
  - [5.2 Problem](#52-problem)
  - [5.3 Structure](#53-structure)
  - [5.4 Java Implementation](#54-java-implementation)
  - [5.5 Advantages, Disadvantages, and Use Cases](#55-advantages-disadvantages-and-use-cases)
- [6. Command Pattern](#6-command-pattern)
  - [6.1 Definition and Intent](#61-definition-and-intent)
  - [6.2 Problem](#62-problem)
  - [6.3 Structure](#63-structure)
  - [6.4 Java Implementation](#64-java-implementation)
  - [6.5 Undo, Redo, and Macro Commands](#65-undo-redo-and-macro-commands)
  - [6.6 Advantages, Disadvantages, and Use Cases](#66-advantages-disadvantages-and-use-cases)
- [7. Interpreter Pattern](#7-interpreter-pattern)
  - [7.1 Definition and Intent](#71-definition-and-intent)
  - [7.2 Problem and Grammar](#72-problem-and-grammar)
  - [7.3 Structure](#73-structure)
  - [7.4 Java Implementation](#74-java-implementation)
  - [7.5 Advantages, Disadvantages, and Use Cases](#75-advantages-disadvantages-and-use-cases)
- [8. Iterator Pattern](#8-iterator-pattern)
  - [8.1 Definition and Intent](#81-definition-and-intent)
  - [8.2 Problem](#82-problem)
  - [8.3 Structure](#83-structure)
  - [8.4 Java Implementation](#84-java-implementation)
  - [8.5 Internal vs External Iterators](#85-internal-vs-external-iterators)
  - [8.6 Advantages, Disadvantages, and Use Cases](#86-advantages-disadvantages-and-use-cases)
- [9. Mediator Pattern](#9-mediator-pattern)
  - [9.1 Definition and Intent](#91-definition-and-intent)
  - [9.2 Problem](#92-problem)
  - [9.3 Structure](#93-structure)
  - [9.4 Java Implementation](#94-java-implementation)
  - [9.5 Mediator vs Observer](#95-mediator-vs-observer)
  - [9.6 Advantages, Disadvantages, and Use Cases](#96-advantages-disadvantages-and-use-cases)
- [10. Memento Pattern](#10-memento-pattern)
  - [10.1 Definition and Intent](#101-definition-and-intent)
  - [10.2 Problem](#102-problem)
  - [10.3 Structure](#103-structure)
  - [10.4 Java Implementation](#104-java-implementation)
  - [10.5 Advantages, Disadvantages, and Use Cases](#105-advantages-disadvantages-and-use-cases)
- [11. Observer Pattern](#11-observer-pattern)
  - [11.1 Definition and Intent](#111-definition-and-intent)
  - [11.2 Problem](#112-problem)
  - [11.3 Structure](#113-structure)
  - [11.4 Java Implementation](#114-java-implementation)
  - [11.5 Observer vs Polling and Publish-Subscribe](#115-observer-vs-polling-and-publish-subscribe)
  - [11.6 Advantages, Disadvantages, and Use Cases](#116-advantages-disadvantages-and-use-cases)
- [12. State Pattern](#12-state-pattern)
  - [12.1 Definition and Intent](#121-definition-and-intent)
  - [12.2 Problem](#122-problem)
  - [12.3 Structure](#123-structure)
  - [12.4 Java Implementation](#124-java-implementation)
  - [12.5 State vs Strategy](#125-state-vs-strategy)
  - [12.6 Advantages, Disadvantages, and Use Cases](#126-advantages-disadvantages-and-use-cases)
- [13. Strategy Pattern](#13-strategy-pattern)
  - [13.1 Definition and Intent](#131-definition-and-intent)
  - [13.2 Problem](#132-problem)
  - [13.3 Structure](#133-structure)
  - [13.4 Java Implementation](#134-java-implementation)
  - [13.5 Strategy vs Template Method](#135-strategy-vs-template-method)
  - [13.6 Advantages, Disadvantages, and Use Cases](#136-advantages-disadvantages-and-use-cases)
- [14. Template Method Pattern](#14-template-method-pattern)
  - [14.1 Definition and Intent](#141-definition-and-intent)
  - [14.2 Problem](#142-problem)
  - [14.3 Structure](#143-structure)
  - [14.4 Java Implementation](#144-java-implementation)
  - [14.5 Advantages, Disadvantages, and Use Cases](#145-advantages-disadvantages-and-use-cases)
- [15. Visitor Pattern](#15-visitor-pattern)
  - [15.1 Definition and Intent](#151-definition-and-intent)
  - [15.2 Problem](#152-problem)
  - [15.3 Structure](#153-structure)
  - [15.4 Java Implementation](#154-java-implementation)
  - [15.5 Advantages, Disadvantages, and Use Cases](#155-advantages-disadvantages-and-use-cases)
- [16. Commonly Confused Behavioral Patterns](#16-commonly-confused-behavioral-patterns)
- [17. How Behavioral Patterns Work Together](#17-how-behavioral-patterns-work-together)
- [18. Behavioral Patterns in Java and Spring](#18-behavioral-patterns-in-java-and-spring)
- [19. Advantages of Behavioral Patterns](#19-advantages-of-behavioral-patterns)
- [20. Disadvantages and Common Mistakes](#20-disadvantages-and-common-mistakes)
- [21. Interview-Oriented Decision Tree](#21-interview-oriented-decision-tree)
- [22. One-Page Cheat Sheet](#22-one-page-cheat-sheet)
- [23. Important Interview Questions and Answers](#23-important-interview-questions-and-answers)
- [24. Final Mental Model](#24-final-mental-model)
- [25. Reference Material](#25-reference-material)
- [Study Recommendation](#study-recommendation)

---

# Behavioral Design Patterns — Complete Notes

> **Purpose:** Interview-friendly notes for understanding and implementing the classic GoF Behavioral Design Patterns in Java. The notes include definitions, problems, structures, code examples, use cases, trade-offs, comparisons, and interview questions.
>
> **Primary references:**
> - GeeksforGeeks — [Behavioral Design Patterns](https://www.geeksforgeeks.org/system-design/behavioral-design-patterns/)
> - SourceMaking — [Behavioral Patterns](https://sourcemaking.com/design_patterns/behavioral_patterns)
>
> **Scope note:** These notes cover only the **11 behavioral patterns from the Gang of Four (GoF)**. SourceMaking also lists Null Object, but Null Object is not one of the original 23 GoF patterns, so it is intentionally excluded. The GeeksforGeeks overview discusses 10 patterns and does not include Iterator; Iterator is included here because it is one of the classic GoF patterns and is listed by SourceMaking.

---

# 1. What Are Behavioral Design Patterns?

**Behavioral Design Patterns describe how objects and classes communicate, distribute responsibilities, and coordinate work.** They help manage the flow of control between objects without making every component tightly dependent on every other component.

In an application, behavior can become difficult to maintain when:

- One class contains large `if-else` or `switch` statements for many behaviors.
- The sender of a request knows too much about the object that executes it.
- Several objects depend directly on one another to coordinate a workflow.
- The application needs to notify many objects when state changes.
- We need to save an object's previous state, traverse a collection, or add a new operation to a stable object structure.
- Multiple classes repeat the same workflow but differ in only a few steps.

Behavioral patterns provide proven ways to structure those relationships and algorithms.

### Core idea

Without deliberate behavioral design, communication may become tangled:

```text
Object A <------> Object B
   ^  \            /  ^
   |   \          /   |
   v    v        v    v
Object C <------> Object D

Many direct dependencies make changes risky.
```

With an appropriate behavioral pattern, responsibility is made explicit:

```text
Client / Request
       |
       v
Behavioral Abstraction
       |
       +----> Handler / Strategy / Observer / State / Command
```

The abstraction differs by pattern. For example, Strategy encapsulates an algorithm, Observer manages notifications, Command represents a request as an object, and Mediator centralizes communication among colleagues.

### What behavioral patterns are not

Behavioral patterns are not simply a collection of interfaces. The goal is to solve a recurring **responsibility or communication problem**. Adding classes without a clear problem can make the design harder to understand rather than better.

---

# 2. Why Do We Need Behavioral Patterns?

## 2.1 Reduce coupling

A sender can work with an abstraction instead of knowing a concrete receiver. An Observer subject can notify listeners without knowing their exact implementations.

## 2.2 Replace complicated conditionals

Strategy can replace a large conditional that chooses an algorithm. State can move state-specific behavior out of a large conditional in a context object.

## 2.3 Make behavior configurable

An application can select a pricing, sorting, validation, or routing strategy at runtime without rewriting the client.

## 2.4 Represent operations as data

Command packages a request into an object. That enables queuing, logging, scheduling, retrying, and sometimes undo/redo.

## 2.5 Coordinate complex workflows

Chain of Responsibility allows a request to pass through multiple handlers. Mediator centralizes coordination among collaborating objects.

## 2.6 Preserve state and traverse structures safely

Memento captures state without exposing an object's internals. Iterator provides sequential access to a collection without exposing its underlying representation.

## 2.7 Reuse an algorithm's overall workflow

Template Method keeps the high-level steps fixed while subclasses customize selected steps. Visitor separates new operations from a stable object structure.

> **Important:** Use a behavioral pattern when it clarifies responsibilities or makes a likely change easier—not merely to increase the number of classes.

---

# 3. The 11 GoF Behavioral Design Patterns

The classic Gang of Four catalog contains these 11 behavioral patterns:

| # | Pattern | Primary idea |
|---:|---|---|
| 1 | **Chain of Responsibility** | Pass a request through potential handlers until it is handled or the chain ends. |
| 2 | **Command** | Represent a request as an object. |
| 3 | **Interpreter** | Represent a grammar and interpret expressions in that language. |
| 4 | **Iterator** | Traverse a collection without exposing its internal representation. |
| 5 | **Mediator** | Centralize communication between collaborating objects. |
| 6 | **Memento** | Capture and restore an object's state without exposing its internals. |
| 7 | **Observer** | Notify subscribed objects when another object's state changes. |
| 8 | **State** | Change an object's behavior according to its current state. |
| 9 | **Strategy** | Encapsulate interchangeable algorithms. |
| 10 | **Template Method** | Define an algorithm skeleton and let subclasses customize selected steps. |
| 11 | **Visitor** | Add operations to a stable set of element types without putting each operation into those element classes. |

```text
Behavioral Patterns
│
├── Request and communication
│   ├── Chain of Responsibility
│   ├── Command
│   ├── Mediator
│   └── Observer
│
├── Algorithms and control flow
│   ├── Interpreter
│   ├── Iterator
│   ├── State
│   ├── Strategy
│   └── Template Method
│
└── State/history and operations over structures
    ├── Memento
    └── Visitor
```

This grouping is only a learning aid; several patterns can fit more than one category.

---

# 4. Quick Comparison

| Pattern | Main question | Typical mechanism | Example |
|---|---|---|---|
| Chain of Responsibility | Who can handle this request? | A linked sequence of handlers | Support escalation, request filters |
| Command | How can a request be stored or passed around? | Request object with an execution method | Editor action, queued job |
| Interpreter | How do I evaluate a small language? | Expression objects following grammar rules | Rule engine, arithmetic expression |
| Iterator | How do I traverse items without exposing storage? | Iterator with `hasNext()` and `next()` | Collection traversal |
| Mediator | Who coordinates these collaborating objects? | Central coordinator | Chat room, UI dialog |
| Memento | How can I restore an earlier state? | Opaque snapshot | Undo, checkpoint |
| Observer | Who needs to know that something changed? | Subject plus subscribed observers | UI events, domain notifications |
| State | How should behavior vary with current state? | State objects delegated to by a context | Order or vending-machine workflow |
| Strategy | Which algorithm should be used? | Interchangeable strategy implementations | Pricing, shipping, sorting |
| Template Method | Which workflow steps are fixed and which vary? | Base-class algorithm with overridable steps | Import pipeline |
| Visitor | How can I add operations to existing element types? | Visitor plus element dispatch | Compiler AST operations |

### Easy memory trick

```text
Chain of Responsibility -> PASS IT ALONG
Command                 -> REQUEST AS AN OBJECT
Interpreter             -> UNDERSTAND A GRAMMAR
Iterator                -> TRAVERSE
Mediator                -> COORDINATE
Memento                 -> RESTORE
Observer                -> NOTIFY
State                   -> BEHAVIOR BY STATE
Strategy                -> CHOOSE AN ALGORITHM
Template Method         -> FIX THE SKELETON
Visitor                 -> ADD AN OPERATION
```

---

# 5. Chain of Responsibility Pattern

## 5.1 Definition and Intent

**Chain of Responsibility lets multiple handlers have a chance to process a request by passing it along a chain.** The sender does not need to know which handler will ultimately process the request.

Each handler can:

1. Handle the request itself.
2. Pass the request to the next handler.
3. Reject or stop processing according to the design.

The pattern is especially useful when the appropriate handler depends on runtime conditions or when the processing sequence should be configurable.

## 5.2 Problem

Suppose a support application contains:

```java
if (issueType.equals("BILLING")) {
    // billing support
} else if (issueType.equals("TECHNICAL")) {
    // technical support
} else if (issueType.equals("GENERAL")) {
    // general support
}
```

If there are many handlers, their selection and execution can become tightly coupled to the request sender. Adding a new handling rule often requires editing the same central conditional.

Chain of Responsibility separates each handler and links them together. The sender submits a request to the first handler, rather than selecting a concrete handler itself.

## 5.3 Structure

```text
Client
  |
  v
Handler A ----> Handler B ----> Handler C ----> null
     |               |               |
     +-- handle?     +-- handle?     +-- handle?
          |                |
       handled          pass along
```

Main participants:

- **Handler:** Defines the request-handling contract and usually stores a reference to the next handler.
- **Concrete Handler:** Handles a request when appropriate or forwards it.
- **Client:** Builds/configures the chain and sends the request to its beginning.

## 5.4 Java Implementation

The following example routes support tickets based on the issue type. It uses object composition to link handlers.

```java
enum IssueType {
    BILLING, TECHNICAL, GENERAL
}

record SupportTicket(IssueType type, String description) {}

abstract class SupportHandler {
    private SupportHandler next;

    public SupportHandler setNext(SupportHandler next) {
        this.next = next;
        return next; // supports fluent chain construction
    }

    public final void handle(SupportTicket ticket) {
        if (canHandle(ticket)) {
            process(ticket);
        } else if (next != null) {
            next.handle(ticket);
        } else {
            System.out.println("No handler for: " + ticket.description());
        }
    }

    protected abstract boolean canHandle(SupportTicket ticket);

    protected abstract void process(SupportTicket ticket);
}

class BillingHandler extends SupportHandler {
    @Override
    protected boolean canHandle(SupportTicket ticket) {
        return ticket.type() == IssueType.BILLING;
    }

    @Override
    protected void process(SupportTicket ticket) {
        System.out.println("Billing team handles: " + ticket.description());
    }
}

class TechnicalHandler extends SupportHandler {
    @Override
    protected boolean canHandle(SupportTicket ticket) {
        return ticket.type() == IssueType.TECHNICAL;
    }

    @Override
    protected void process(SupportTicket ticket) {
        System.out.println("Technical team handles: " + ticket.description());
    }
}

class GeneralHandler extends SupportHandler {
    @Override
    protected boolean canHandle(SupportTicket ticket) {
        return ticket.type() == IssueType.GENERAL;
    }

    @Override
    protected void process(SupportTicket ticket) {
        System.out.println("General team handles: " + ticket.description());
    }
}
```

Usage:

```java
SupportHandler billing = new BillingHandler();
SupportHandler technical = new TechnicalHandler();
SupportHandler general = new GeneralHandler();

billing.setNext(technical).setNext(general);

billing.handle(new SupportTicket(IssueType.TECHNICAL, "Login fails"));
billing.handle(new SupportTicket(IssueType.BILLING, "Incorrect invoice"));
```

The request begins at `billing`. If it is not a billing ticket, the handler forwards it. A handler processes the ticket once it recognizes the issue.

> **Java version note:** `record` requires Java 16 or later. For earlier Java versions, replace `SupportTicket` with a regular immutable class. The examples in these notes are teaching examples; related classes may be placed in separate source files in a real project.

## 5.5 Advantages, Disadvantages, and Use Cases

### Advantages

- The sender does not need to know the concrete handler.
- Handlers can be reordered, added, or removed independently when the chain is configured appropriately.
- Each handler has a focused responsibility.
- A request may be handled without executing every handler.

### Disadvantages

- A request may reach the end without being handled.
- The chain's order affects behavior and must be tested.
- Debugging can require tracing several handlers.
- A badly designed chain may contain cycles or do redundant work.

### When to use

Use it when multiple handlers may process a request and the sender should not decide which one handles it.

Examples include support escalation, servlet/filter pipelines, validation chains, middleware, and request-processing pipelines. Not every filter pipeline is a direct GoF implementation, but the underlying chain idea is similar.

---

# 6. Command Pattern

## 6.1 Definition and Intent

**Command encapsulates a request as an object.** The object can carry the operation's required data and expose a uniform method, such as `execute()`.

Command separates:

- The object that asks for an action (**invoker**).
- The object that performs the action (**receiver**).
- The request itself (**command**).

Once a request is represented as an object, it can be queued, logged, retried, stored, scheduled, or sometimes reversed.

## 6.2 Problem

Imagine a toolbar with buttons for saving, copying, and deleting. If the toolbar contains the entire implementation of each action, it becomes tightly coupled to the objects being modified. Adding more actions leads to more conditional logic.

Instead, each action can be an object that implements the same command interface. The toolbar only calls `execute()` and does not need to know the details of each action.

## 6.3 Structure

```text
Client ---- creates/configures ----> ConcreteCommand
                                         |
Invoker ---- execute() ------------------+
                                         |
                                         v
                                      Receiver
                                   performs work
```

Participants:

- **Command:** Common execution interface.
- **Concrete Command:** Implements the request and calls a receiver.
- **Receiver:** Performs the real business operation.
- **Invoker:** Triggers the command but does not implement its business logic.
- **Client:** Creates the receiver and command, then connects the command to the invoker.

## 6.4 Java Implementation

Example: a simple editor toolbar that can append text to a document.

```java
interface Command {
    void execute();
}

class TextDocument {
    private final StringBuilder content = new StringBuilder();

    public void append(String text) {
        content.append(text);
    }

    public void removeSuffix(String suffix) {
        String current = content.toString();
        if (!current.endsWith(suffix)) {
            throw new IllegalStateException(
                "Cannot undo: document no longer ends with the command text"
            );
        }
        content.delete(content.length() - suffix.length(), content.length());
    }

    public String getContent() {
        return content.toString();
    }
}

class AppendTextCommand implements Command {
    private final TextDocument document;
    private final String text;

    public AppendTextCommand(TextDocument document, String text) {
        this.document = document;
        this.text = text;
    }

    @Override
    public void execute() {
        document.append(text);
    }
}

class ToolbarButton {
    private Command command;

    public void setCommand(Command command) {
        this.command = command;
    }

    public void click() {
        if (command == null) {
            throw new IllegalStateException("No command configured");
        }
        command.execute();
    }
}
```

Usage:

```java
TextDocument document = new TextDocument();
ToolbarButton button = new ToolbarButton();

button.setCommand(new AppendTextCommand(document, "Hello, "));
button.click();

button.setCommand(new AppendTextCommand(document, "Java!"));
button.click();

System.out.println(document.getContent()); // Hello, Java!
```

The button knows only the `Command` interface. It does not know how text is stored or appended.

## 6.5 Undo, Redo, and Macro Commands

A command can also provide an `undo()` operation when the action is reversible and it can capture enough information to reverse the change.

```java
interface UndoableCommand extends Command {
    void undo();
}

class AppendTextUndoableCommand implements UndoableCommand {
    private final TextDocument document;
    private final String text;
    private boolean executed;

    public AppendTextUndoableCommand(TextDocument document, String text) {
        this.document = document;
        this.text = text;
    }

    @Override
    public void execute() {
        if (executed) {
            throw new IllegalStateException("Command is already executed");
        }
        document.append(text);
        executed = true;
    }

    @Override
    public void undo() {
        if (!executed) {
            throw new IllegalStateException("Command has not been executed");
        }
        document.removeSuffix(text);
        executed = false;
    }
}
```

This simplified command assumes undo happens in reverse execution order, as it would with a stack-based history. If later edits have changed the document suffix, `removeSuffix()` fails rather than silently deleting unrelated text. Real editors usually track exact document ranges or use a richer edit model.

### Command history

A history can store executed commands in a stack. Undo pops the most recent undoable command and calls `undo()`; redo typically stores undone commands in another stack and calls `execute()` again. Commands that depend on external side effects may not be reliably reversible.

### Macro command

A macro command groups several commands and executes them in sequence:

```java
class MacroCommand implements Command {
    private final java.util.List<Command> commands;

    public MacroCommand(java.util.List<Command> commands) {
        this.commands = java.util.List.copyOf(commands);
    }

    @Override
    public void execute() {
        for (Command command : commands) {
            command.execute();
        }
    }
}
```

For a production macro, decide what should happen if a middle command fails: stop immediately, compensate earlier commands, or report partial completion.

## 6.6 Advantages, Disadvantages, and Use Cases

### Advantages

- Decouples request senders from receivers.
- Makes queuing, scheduling, logging, retries, and command history possible.
- Supports undo/redo when commands capture adequate state.
- Supports composing several operations into one macro command.

### Disadvantages

- Introduces more classes or objects.
- Undo is difficult for irreversible actions or operations with external side effects.
- Retry logic must consider idempotency and duplicate execution.
- A command queue requires decisions about ordering, failure handling, and concurrency.

### When to use

Use Command when requests need to be represented, stored, parameterized, delayed, queued, logged, or undone.

Examples include UI actions, job queues, transaction-like workflows, remote invocation, and undoable editor operations.

---

# 7. Interpreter Pattern

## 7.1 Definition and Intent

**Interpreter defines a representation for a language's grammar and an interpreter that uses that representation to evaluate sentences in the language.**

It is suitable for small, well-defined languages: arithmetic expressions, simple business rules, filters, permission expressions, and domain-specific configuration syntax.

The pattern commonly represents the grammar as a hierarchy of expression objects. Each expression implements an interpretation operation.

## 7.2 Problem and Grammar

Suppose a system must evaluate expressions such as:

```text
(10 + 5) - 3
```

Hard-coding every possible expression is not scalable. A simple grammar can instead define how numbers, addition, and subtraction combine.

An illustrative grammar is:

```text
Expression ::= Number
             | Expression + Expression
             | Expression - Expression
```

This grammar is intentionally simplified. A production parser must also address precedence, parentheses, tokenization, malformed input, and error reporting.

## 7.3 Structure

```text
Client
  |
  v
Expression.interpret(context)
  |
  +----> NumberExpression
  +----> AddExpression
  +----> SubtractExpression
```

Participants:

- **Abstract Expression:** Defines the interpretation operation.
- **Terminal Expression:** Represents a terminal symbol, such as a number or variable.
- **Nonterminal Expression:** Represents a grammar rule composed from other expressions.
- **Context:** Optional data needed to interpret variables or resolve names.
- **Client:** Builds the expression tree and asks it to interpret itself.

## 7.4 Java Implementation

The following example evaluates an expression tree. It focuses on the object model rather than parsing raw text.

```java
interface Expression {
    int interpret();
}

class NumberExpression implements Expression {
    private final int value;

    public NumberExpression(int value) {
        this.value = value;
    }

    @Override
    public int interpret() {
        return value;
    }
}

class AddExpression implements Expression {
    private final Expression left;
    private final Expression right;

    public AddExpression(Expression left, Expression right) {
        this.left = left;
        this.right = right;
    }

    @Override
    public int interpret() {
        return left.interpret() + right.interpret();
    }
}

class SubtractExpression implements Expression {
    private final Expression left;
    private final Expression right;

    public SubtractExpression(Expression left, Expression right) {
        this.left = left;
        this.right = right;
    }

    @Override
    public int interpret() {
        return left.interpret() - right.interpret();
    }
}
```

Usage:

```java
Expression expression = new SubtractExpression(
    new AddExpression(new NumberExpression(10), new NumberExpression(5)),
    new NumberExpression(3)
);

System.out.println(expression.interpret()); // 12
```

The tree represents `(10 + 5) - 3`. Each expression knows how to evaluate itself, and composite expressions delegate to child expressions.

### Extending the example

A more complete expression language could add variables using an evaluation context, multiplication, comparisons, boolean operators, or function calls. For untrusted input, enforce limits on expression depth, execution cost, and permitted operations.

## 7.5 Advantages, Disadvantages, and Use Cases

### Advantages

- Grammar rules can be represented with composable classes.
- Simple expressions are easy to extend and test.
- Expression trees can be inspected, transformed, or evaluated in different contexts.

### Disadvantages

- A class per grammar rule can create many classes.
- The pattern is awkward for large or frequently changing grammars.
- Real parsers require robust syntax and semantic error handling.
- Unrestricted interpretation of user-provided expressions can create security and performance risks.

### When to use

Use Interpreter when the grammar is small and relatively stable, and the expression hierarchy is a natural model.

For complex languages, prefer a parser generator or a dedicated parsing library rather than building a large Interpreter hierarchy manually.

---

# 8. Iterator Pattern

## 8.1 Definition and Intent

**Iterator provides a way to access elements of a collection sequentially without exposing the collection's underlying representation.**

The client can traverse the elements without knowing whether the collection uses an array, linked list, tree, database cursor, or another structure.

Java's `Iterator`, `Iterable`, and enhanced `for` loop are familiar examples of iterator-based traversal.

## 8.2 Problem

If client code directly accesses an array or the internal nodes of a custom collection, the client becomes coupled to that representation. Changing the collection from an array to a tree can then break the traversal code.

Iterator moves traversal logic behind an interface and lets the collection supply an appropriate traversal object.

## 8.3 Structure

```text
Client
  |
  v
Iterator: hasNext(), next()
  ^
  |
ConcreteIterator ----> Aggregate / Collection
                            |
                            +-- createIterator()
```

Participants:

- **Iterator:** Defines traversal operations.
- **Concrete Iterator:** Tracks the current position and implements traversal.
- **Aggregate:** Defines a way to obtain an iterator.
- **Concrete Aggregate:** Provides an iterator appropriate for its representation.

## 8.4 Java Implementation

Java's built-in `Iterable` and `Iterator` already provide these contracts. This example creates a playlist that can be traversed without exposing its internal list.

```java
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;
import java.util.NoSuchElementException;

record Song(String title) {}

class Playlist implements Iterable<Song> {
    private final List<Song> songs = new ArrayList<>();

    public void add(Song song) {
        songs.add(song);
    }

    @Override
    public Iterator<Song> iterator() {
        return new Iterator<>() {
            private int index = 0;

            @Override
            public boolean hasNext() {
                return index < songs.size();
            }

            @Override
            public Song next() {
                if (!hasNext()) {
                    throw new NoSuchElementException();
                }
                return songs.get(index++);
            }
        };
    }
}
```

Usage:

```java
Playlist playlist = new Playlist();
playlist.add(new Song("Track A"));
playlist.add(new Song("Track B"));

for (Song song : playlist) {
    System.out.println(song.title());
}
```

The enhanced `for` loop calls `iterator()` and then repeatedly calls `hasNext()` and `next()`.

> In production, also define the collection's behavior if it is modified during iteration. Java collections often use fail-fast iterators as a best-effort bug-detection mechanism; this is not the same as thread safety.

## 8.5 Internal vs External Iterators

| Type | Who controls traversal? | Typical example |
|---|---|---|
| **External iterator** | The client explicitly advances the iterator | `while (iterator.hasNext()) { ... }` |
| **Internal iterator** | The collection or library controls the traversal operation | `items.forEach(...)`, streams |

An external iterator gives the client more control over when to advance, while an internal iterator often allows more declarative processing. Java streams use an internal-iteration style even though their implementation details are more extensive than the GoF Iterator example.

## 8.6 Advantages, Disadvantages, and Use Cases

### Advantages

- Hides the collection's internal representation.
- Provides a consistent traversal interface.
- Supports multiple traversal strategies for one collection.
- Separates traversal logic from business logic.

### Disadvantages

- Adds an iterator object and associated state.
- Concurrent modification behavior must be understood.
- Some iterators are one-way or single-use.
- Advanced traversals can complicate the API.

### When to use

Use Iterator when clients need to traverse a structure without being coupled to its storage or when multiple traversal methods are useful.

Examples include Java collections, tree traversal, file-system walking, paginated API results, and database cursors.

---

# 9. Mediator Pattern

## 9.1 Definition and Intent

**Mediator defines an object that encapsulates how a set of objects interact.** Instead of each object communicating directly with every other object, objects notify the mediator, which coordinates the interaction.

The mediator centralizes a collaboration protocol while reducing direct dependencies among participating objects, known as colleagues.

## 9.2 Problem

Consider a group chat. If every user object directly manages message delivery to every other user, each user needs to know too much about the other users. Adding rooms, moderation, or new delivery rules can become difficult.

A chat-room mediator receives a message from one user and distributes it to the other members. User objects communicate through the mediator rather than managing all recipients themselves.

## 9.3 Structure

```text
             Mediator
            /   |   \
           /    |    \
      Colleague A  Colleague B  Colleague C

Colleagues notify the mediator; the mediator coordinates them.
```

Participants:

- **Mediator:** Defines the coordination contract.
- **Concrete Mediator:** Knows the colleagues and contains the interaction logic.
- **Colleagues:** Collaborating objects that communicate through the mediator.

## 9.4 Java Implementation

A minimal chat-room example:

```java
import java.util.ArrayList;
import java.util.List;

interface ChatMediator {
    void register(User user);
    void sendMessage(String message, User sender);
}

class ChatRoom implements ChatMediator {
    private final List<User> users = new ArrayList<>();

    @Override
    public void register(User user) {
        if (!users.contains(user)) {
            users.add(user);
            user.setMediator(this);
        }
    }

    @Override
    public void sendMessage(String message, User sender) {
        for (User user : users) {
            if (user != sender) {
                user.receive(message, sender.getName());
            }
        }
    }
}

abstract class User {
    private final String name;
    private ChatMediator mediator;

    protected User(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setMediator(ChatMediator mediator) {
        this.mediator = mediator;
    }

    public void send(String message) {
        if (mediator == null) {
            throw new IllegalStateException("User is not registered");
        }
        mediator.sendMessage(message, this);
    }

    public abstract void receive(String message, String senderName);
}

class ChatUser extends User {
    public ChatUser(String name) {
        super(name);
    }

    @Override
    public void receive(String message, String senderName) {
        System.out.println(senderName + ": " + message + " -> " + getName());
    }
}
```

Usage:

```java
ChatMediator room = new ChatRoom();
User akash = new ChatUser("Akash");
User priya = new ChatUser("Priya");
User sam = new ChatUser("Sam");

room.register(akash);
room.register(priya);
room.register(sam);

akash.send("Hello everyone!");
```

The sending user does not know the recipients. `ChatRoom` owns the message-distribution rule.

### Design caution

A mediator can become a **god object** if it accumulates every business rule. Keep it focused on coordination and split separate workflows into separate mediators or services when necessary.

## 9.5 Mediator vs Observer

- **Mediator:** Centralizes and coordinates interactions. Colleagues typically know the mediator.
- **Observer:** A subject broadcasts notifications to subscribed observers without needing to know their concrete types.

A mediator may use Observer internally for subscriptions or notifications, so the patterns are not mutually exclusive.

## 9.6 Advantages, Disadvantages, and Use Cases

### Advantages

- Reduces direct dependencies among colleagues.
- Moves coordination logic into one explicit place.
- Makes interaction workflows easier to inspect and change.
- Can simplify complex UI or workflow interactions.

### Disadvantages

- The mediator may become too large or too central.
- Indirect communication may make control flow less obvious.
- The mediator still needs clear interfaces and tests.

### When to use

Use Mediator when many objects interact in complex ways and a central coordinator can simplify their relationships.

Examples include chat rooms, UI dialogs with dependent controls, workflow coordination, and collaboration between components in a bounded subsystem.

---

# 10. Memento Pattern

## 10.1 Definition and Intent

**Memento captures and externalizes an object's internal state so it can be restored later, without exposing the object's internal representation.**

Memento is commonly used for undo, rollback, checkpoints, and version history.

## 10.2 Problem

Suppose a text editor must restore an earlier document. Exposing every private field to an external history manager breaks encapsulation. Reconstructing a previous state from public getters may also be incomplete or fragile.

Memento lets the object create a snapshot of its own state. A separate caretaker stores snapshots but does not inspect or alter their contents.

## 10.3 Structure

```text
Originator ---- creates ----> Memento
     ^                           |
     |                           v
restore(memento)             Caretaker
                                 |
                           stores snapshots
                           without inspecting
```

Participants:

- **Originator:** Owns the state and creates/restores snapshots.
- **Memento:** Holds a captured state behind a controlled interface.
- **Caretaker:** Stores snapshots and determines when to request a restore; it should not modify the snapshot's internal data.

## 10.4 Java Implementation

```java
import java.util.ArrayDeque;
import java.util.Deque;

class TextEditor {
    private String text = "";

    public void type(String newText) {
        text += newText;
    }

    public String getText() {
        return text;
    }

    public Snapshot save() {
        return new Snapshot(text);
    }

    public void restore(Snapshot snapshot) {
        this.text = snapshot.savedText;
    }

    // Nested snapshot keeps its data hidden from external caretaker code.
    public static final class Snapshot {
        private final String savedText;

        private Snapshot(String savedText) {
            this.savedText = savedText;
        }
    }
}

class EditorHistory {
    private final Deque<TextEditor.Snapshot> history = new ArrayDeque<>();

    public void save(TextEditor editor) {
        history.push(editor.save());
    }

    public boolean restorePrevious(TextEditor editor) {
        if (history.isEmpty()) {
            return false;
        }
        editor.restore(history.pop());
        return true;
    }
}
```

Usage:

```java
TextEditor editor = new TextEditor();
EditorHistory history = new EditorHistory();

editor.type("Hello");
history.save(editor);

editor.type(", world");
System.out.println(editor.getText()); // Hello, world

history.restorePrevious(editor);
System.out.println(editor.getText()); // Hello
```

The history stores snapshots without reading their content. The editor itself is responsible for creating and restoring snapshots.

### Important implementation details

- A snapshot must capture enough state to restore the intended behavior, not just a convenient subset.
- If the originator contains mutable objects, decide whether to copy them deeply or whether an immutable snapshot is possible.
- Snapshot storage can consume significant memory. Consider limits, deltas, compression, or persisted checkpoints for large state.
- Access control and nested types may need adjustment depending on whether the caretaker is inside or outside the originator's enclosing class. The key design goal is to keep snapshot internals opaque to the caretaker.

## 10.5 Advantages, Disadvantages, and Use Cases

### Advantages

- Preserves encapsulation while supporting restoration.
- Simplifies undo and rollback logic.
- Keeps snapshot history separate from the originator's business logic.

### Disadvantages

- Snapshots may require significant memory.
- Deep copying complex state can be costly.
- Correct snapshot boundaries and lifetime policies require design.
- External side effects, such as sent emails or completed payments, cannot be undone merely by restoring in-memory state.

### When to use

Use Memento when an object's state must be saved and restored while its internal representation should remain private.

Examples include editor history, game checkpoints, form drafts, workflow rollback, and configuration snapshots.

---

# 11. Observer Pattern

## 11.1 Definition and Intent

**Observer defines a one-to-many dependency: when a subject changes state, its subscribed observers are notified.**

The subject maintains a set of observers and notifies them when relevant events occur. Observers can subscribe and unsubscribe without the subject knowing their concrete implementation details.

Observer is common in event-driven code, GUI listeners, domain events, and reactive systems.

## 11.2 Problem

Suppose a weather station's temperature is displayed in a mobile app, a dashboard, and an alert service. If the station directly calls each concrete display, adding or removing a consumer requires modifying the station.

Observer lets each consumer subscribe to temperature updates. The station publishes changes through a shared notification contract.

## 11.3 Structure

```text
             Subject
          /     |     \
         v      v      v
     Observer A Observer B Observer C

Subject: subscribe / unsubscribe / notify
Observer: update(event or state)
```

Participants:

- **Subject/Publisher:** Maintains observer registrations and notifies them.
- **Observer/Subscriber:** Defines the update contract.
- **Concrete Observer:** Reacts to notifications.

## 11.4 Java Implementation

```java
import java.util.ArrayList;
import java.util.List;
import java.util.function.Consumer;

record TemperatureChanged(double celsius) {}

class WeatherStation {
    private final List<Consumer<TemperatureChanged>> observers = new ArrayList<>();
    private double temperature;

    public void subscribe(Consumer<TemperatureChanged> observer) {
        observers.add(observer);
    }

    public void unsubscribe(Consumer<TemperatureChanged> observer) {
        observers.remove(observer);
    }

    public void setTemperature(double temperature) {
        this.temperature = temperature;
        notifyObservers(new TemperatureChanged(temperature));
    }

    public double getTemperature() {
        return temperature;
    }

    private void notifyObservers(TemperatureChanged event) {
        // Copy to avoid iterator invalidation if an observer changes subscriptions.
        for (Consumer<TemperatureChanged> observer : List.copyOf(observers)) {
            observer.accept(event);
        }
    }
}
```

Usage:

```java
WeatherStation station = new WeatherStation();

Consumer<TemperatureChanged> dashboard = event ->
    System.out.println("Dashboard: " + event.celsius() + " C");
Consumer<TemperatureChanged> alert = event -> {
    if (event.celsius() > 35) {
        System.out.println("Alert: high temperature!");
    }
};

station.subscribe(dashboard);
station.subscribe(alert);
station.setTemperature(37.2);
station.unsubscribe(alert);
station.setTemperature(24.0);
```

The subject publishes an event; it does not contain the dashboard's or alert's business logic.

### Production considerations

- Choose whether notifications are synchronous or asynchronous.
- Decide how to handle exceptions thrown by one observer so that one failure does not unintentionally prevent delivery to every other observer.
- Define thread-safety rules for registration and notification if multiple threads are involved.
- Avoid retaining short-lived observers forever; unsubscribe appropriately or use a framework with lifecycle-aware subscriptions.
- Decide whether observers receive the event payload, query the subject for new state, or both.

## 11.5 Observer vs Polling and Publish-Subscribe

### Observer vs polling

- **Observer:** A producer pushes a notification when a relevant change occurs.
- **Polling:** A consumer repeatedly checks for changes, even when nothing changed.

Observer can reduce unnecessary checks, but it introduces subscription and lifecycle concerns.

### Observer vs publish-subscribe

These concepts overlap, but they are not always identical:

- In a basic Observer implementation, a subject generally maintains references to its observers directly.
- In a broker-based publish-subscribe architecture, publishers and subscribers are often decoupled through a message broker or event bus and may not know each other or even run in the same process.

## 11.6 Advantages, Disadvantages, and Use Cases

### Advantages

- Supports one-to-many notifications.
- Reduces direct dependencies between producer and consumers.
- Allows observers to be added or removed at runtime.
- Fits event-driven designs.

### Disadvantages

- Notification order may be undefined unless explicitly designed.
- Large observer graphs can make event flow difficult to trace.
- Memory leaks can occur when subscriptions outlive observers.
- Synchronous notification can create latency or failure-propagation problems.

### When to use

Use Observer when several independent components need to react to changes in another component without tight coupling.

Examples include UI events, dashboard updates, domain event listeners, cache invalidation signals, and in-process event notification.

---

# 12. State Pattern

## 12.1 Definition and Intent

**State allows an object to change its behavior when its internal state changes.** From the client's perspective, the object may appear to change its class because it delegates behavior to a different state object.

Each concrete state contains the behavior valid for that state. The context maintains a reference to the current state and delegates state-dependent operations to it.

## 12.2 Problem

Consider an order workflow with statuses such as `NEW`, `PAID`, `SHIPPED`, and `CANCELLED`. A single service full of checks can become hard to reason about:

```java
if (status == NEW) { ... }
else if (status == PAID) { ... }
else if (status == SHIPPED) { ... }
```

State moves each state's allowed operations into its own class. Invalid transitions can be rejected by the current state instead of being scattered across the application.

## 12.3 Structure

```text
Client
  |
  v
Context ---- delegates operation ----> State interface
   |                                      ^
   |                                      |
   +-- currentState              ConcreteStateA / B / C
```

Participants:

- **Context:** Holds the current state and delegates requests.
- **State:** Defines state-specific operations.
- **Concrete States:** Implement behavior and may request state transitions.

## 12.4 Java Implementation

This example models a simple order that can be paid, shipped, or cancelled according to its current state.

```java
interface OrderState {
    void pay(Order order);
    void ship(Order order);
    void cancel(Order order);
    String name();
}

class NewOrderState implements OrderState {
    @Override
    public void pay(Order order) {
        order.setState(new PaidOrderState());
        System.out.println("Order paid");
    }

    @Override
    public void ship(Order order) {
        throw new IllegalStateException("An unpaid order cannot be shipped");
    }

    @Override
    public void cancel(Order order) {
        order.setState(new CancelledOrderState());
        System.out.println("Order cancelled");
    }

    @Override
    public String name() { return "NEW"; }
}

class PaidOrderState implements OrderState {
    @Override
    public void pay(Order order) {
        throw new IllegalStateException("Order is already paid");
    }

    @Override
    public void ship(Order order) {
        order.setState(new ShippedOrderState());
        System.out.println("Order shipped");
    }

    @Override
    public void cancel(Order order) {
        order.setState(new CancelledOrderState());
        System.out.println("Paid order cancelled; refund policy must be applied");
    }

    @Override
    public String name() { return "PAID"; }
}

class ShippedOrderState implements OrderState {
    @Override
    public void pay(Order order) {
        throw new IllegalStateException("Shipped order cannot be paid again");
    }

    @Override
    public void ship(Order order) {
        throw new IllegalStateException("Order is already shipped");
    }

    @Override
    public void cancel(Order order) {
        throw new IllegalStateException("A shipped order needs a return workflow");
    }

    @Override
    public String name() { return "SHIPPED"; }
}

class CancelledOrderState implements OrderState {
    @Override
    public void pay(Order order) {
        throw new IllegalStateException("Cancelled order cannot be paid");
    }

    @Override
    public void ship(Order order) {
        throw new IllegalStateException("Cancelled order cannot be shipped");
    }

    @Override
    public void cancel(Order order) {
        throw new IllegalStateException("Order is already cancelled");
    }

    @Override
    public String name() { return "CANCELLED"; }
}

class Order {
    private OrderState state = new NewOrderState();

    public void pay() { state.pay(this); }
    public void ship() { state.ship(this); }
    public void cancel() { state.cancel(this); }

    public void setState(OrderState state) {
        this.state = state;
    }

    public String getStateName() {
        return state.name();
    }
}
```

Usage:

```java
Order order = new Order();
order.pay();
order.ship();
System.out.println(order.getStateName()); // SHIPPED
```

The example focuses on state-dependent behavior. A production order workflow should model payment, refund, shipping, and persistence as explicit domain operations and should avoid performing external side effects before a state transition is safely recorded.

## 12.5 State vs Strategy

Both patterns use composition and interchangeable implementations. Their **intent** differs:

- **State:** The object's current state determines which behavior is valid. State transitions are usually part of the model.
- **Strategy:** A client or context selects an algorithm to perform a task. Strategies are usually interchangeable policies, and the algorithm does not normally represent a lifecycle state.

See [Section 16](#16-commonly-confused-behavioral-patterns) for more comparisons.

## 12.6 Advantages, Disadvantages, and Use Cases

### Advantages

- Organizes state-specific behavior into focused classes.
- Reduces complicated state conditionals in the context.
- Makes invalid transitions easier to isolate and test.
- Supports adding states without rewriting every branch in one large method.

### Disadvantages

- Can create many small classes.
- State transitions can be harder to understand if distributed across multiple state classes.
- Shared or persistent state still needs clear ownership.
- A simple enum with a few conditions may be clearer for a small workflow.

### When to use

Use State when an object's behavior and valid operations vary substantially with its lifecycle state, and those rules are becoming complex.

Examples include order lifecycle, vending machines, media players, connection lifecycles, approval workflows, and game characters.

---

# 13. Strategy Pattern

## 13.1 Definition and Intent

**Strategy defines a family of algorithms, encapsulates each one, and makes them interchangeable.** The client or context can choose the algorithm without changing the code that uses it.

Strategy helps satisfy the principle of **composition over inheritance**: behavior is supplied to an object instead of requiring many subclasses for each algorithm combination.

## 13.2 Problem

A checkout service may calculate shipping cost using standard, express, or international rules. A single method containing all these conditions grows as new methods are added.

Strategy gives each shipping rule a common interface and lets the checkout service delegate the calculation to the selected strategy.

## 13.3 Structure

```text
Client ---> Context
              |
              v
        Strategy interface
           ^       ^
           |       |
     Standard    Express
     Strategy    Strategy
```

Participants:

- **Strategy:** Shared interface for an algorithm.
- **Concrete Strategy:** Implements one version of the algorithm.
- **Context:** Uses the selected strategy without embedding its detailed algorithm.
- **Client:** Selects or configures the strategy.

## 13.4 Java Implementation

```java
interface ShippingStrategy {
    double calculateCost(double orderWeightKg);
}

class StandardShipping implements ShippingStrategy {
    @Override
    public double calculateCost(double orderWeightKg) {
        return 50.0 + (orderWeightKg * 10.0);
    }
}

class ExpressShipping implements ShippingStrategy {
    @Override
    public double calculateCost(double orderWeightKg) {
        return 100.0 + (orderWeightKg * 20.0);
    }
}

class InternationalShipping implements ShippingStrategy {
    @Override
    public double calculateCost(double orderWeightKg) {
        return 500.0 + (orderWeightKg * 80.0);
    }
}

class CheckoutService {
    private ShippingStrategy shippingStrategy;

    public CheckoutService(ShippingStrategy shippingStrategy) {
        this.shippingStrategy = shippingStrategy;
    }

    public void setShippingStrategy(ShippingStrategy shippingStrategy) {
        this.shippingStrategy = shippingStrategy;
    }

    public double shippingCost(double weightKg) {
        return shippingStrategy.calculateCost(weightKg);
    }
}
```

Usage:

```java
CheckoutService checkout = new CheckoutService(new StandardShipping());
System.out.println(checkout.shippingCost(2.0));

checkout.setShippingStrategy(new ExpressShipping());
System.out.println(checkout.shippingCost(2.0));
```

The rates are intentionally simplified. Real shipping calculations may depend on dimensions, destination, carrier rules, taxes, and currency. The pattern is about swapping algorithms, not the business formula itself.

### Strategy selection

The strategy can be injected at construction time, supplied per request, or selected from configuration. Avoid blindly accepting arbitrary class names from user input; map allowed options to known implementations.

## 13.5 Strategy vs Template Method

- **Strategy** typically uses composition: a context delegates to a separate algorithm object.
- **Template Method** typically uses inheritance: a base class fixes the algorithm's overall sequence and subclasses customize selected steps.

Strategy is often more flexible at runtime. Template Method is useful when a shared workflow sequence should remain controlled by a base class.

## 13.6 Advantages, Disadvantages, and Use Cases

### Advantages

- Removes large algorithm-selection conditionals from context classes.
- Makes algorithms independently testable and replaceable.
- Supports runtime configuration or dependency injection.
- Makes adding a new strategy relatively localized.

### Disadvantages

- Adds interfaces and strategy classes.
- The client still needs a way to choose the correct strategy.
- Passing many strategy objects through the system can add complexity.
- If algorithms differ only trivially, separate classes may be unnecessary.

### When to use

Use Strategy when several algorithms solve the same problem and should be interchangeable.

Examples include pricing, tax calculation, sorting, compression, authentication policies, retry policies, routing, and payment fee calculation.

---

# 14. Template Method Pattern

## 14.1 Definition and Intent

**Template Method defines the skeleton of an algorithm in a base class and lets subclasses redefine specific steps without changing the overall algorithm structure.**

The base class owns the sequence. Subclasses customize selected operations.

## 14.2 Problem

Suppose CSV and JSON imports follow the same overall workflow:

1. Read the source.
2. Parse its contents.
3. Validate the records.
4. Save the valid data.
5. Report the result.

Duplicating the entire workflow in each importer invites inconsistencies. Template Method places shared orchestration in one method and makes only the variable steps abstract or overridable.

## 14.3 Structure

```text
AbstractClass
  |
  +-- final templateMethod()
  |      +-- commonStep()
  |      +-- abstractStep()
  |      +-- hook()
  |
  +----> ConcreteClassA implements abstractStep()
  +----> ConcreteClassB implements abstractStep()
```

Participants:

- **Abstract Class:** Declares the template method and common steps.
- **Template Method:** Defines the stable sequence, often marked `final` in Java to prevent subclasses from changing it.
- **Primitive Operations:** Steps subclasses must implement.
- **Hooks:** Optional methods with default behavior that subclasses may override.
- **Concrete Classes:** Implement or customize the variable steps.

## 14.4 Java Implementation

```java
import java.util.List;

abstract class DataImporter {
    // Keep workflow order stable; subclasses customize individual steps.
    public final void importData(String source) {
        String rawData = read(source);
        List<String> records = parse(rawData);
        validate(records);
        save(records);
        afterImport(records.size());
    }

    protected String read(String source) {
        // Simplified: a real implementation would read a file or remote source.
        return source;
    }

    protected abstract List<String> parse(String rawData);

    protected void validate(List<String> records) {
        if (records.stream().anyMatch(String::isBlank)) {
            throw new IllegalArgumentException("Blank records are not allowed");
        }
    }

    protected void save(List<String> records) {
        System.out.println("Saving " + records.size() + " records");
    }

    // Hook: optional behavior after the common workflow.
    protected void afterImport(int recordCount) {
        System.out.println("Imported " + recordCount + " records");
    }
}

class CsvImporter extends DataImporter {
    @Override
    protected List<String> parse(String rawData) {
        return rawData.lines().toList();
    }
}

class LineImporter extends DataImporter {
    @Override
    protected List<String> parse(String rawData) {
        return rawData.lines().map(String::trim).toList();
    }
}
```

Usage:

```java
DataImporter importer = new CsvImporter();
importer.importData("row-1\nrow-2\nrow-3");
```

This is an intentionally simplified importer: `read()` returns its input, and parsing splits it into lines. A real CSV parser needs to handle quoting, escaping, delimiters, and data types.

### Hook vs abstract operation

- An **abstract operation** must be implemented by subclasses.
- A **hook** has a default implementation and is optional to override.

Template Method should expose only the extension points the design truly needs.

## 14.5 Advantages, Disadvantages, and Use Cases

### Advantages

- Reuses the shared algorithm structure.
- Prevents subclasses from changing critical step order when the template method is final.
- Reduces duplication across workflows.
- Gives subclasses controlled customization points.

### Disadvantages

- Uses inheritance and can be harder to combine with unrelated class hierarchies.
- Subclass hooks can make behavior less visible.
- An overly generic base class can accumulate many hooks.
- Changes to the shared algorithm can affect all subclasses.

### When to use

Use Template Method when multiple workflows share the same overall sequence but differ in selected operations.

Examples include import/export pipelines, report generation, test frameworks, build steps, and processing jobs with stable orchestration.

---

# 15. Visitor Pattern

## 15.1 Definition and Intent

**Visitor represents an operation to be performed on elements of an object structure, allowing new operations to be added without changing the element classes for every new operation.**

Visitor separates operations from the objects on which they operate. It is especially useful when the set of element types is relatively stable but new operations are frequently needed.

## 15.2 Problem

Suppose a drawing application contains circles and rectangles. New operations may be required to calculate area, export JSON, render SVG, or validate constraints.

If every new operation is added as a method to every shape class, the shape classes continue to change. Visitor moves each operation into a separate visitor implementation.

## 15.3 Structure

```text
Client ---> Visitor
              ^   ^
              |   |
       AreaVisitor ExportVisitor

Element.accept(visitor)
      |          |
   Circle     Rectangle
      |          |
 visitor.visit(Circle / Rectangle)
```

Participants:

- **Visitor:** Declares a visit operation for each concrete element type.
- **Concrete Visitor:** Implements an operation across the element types.
- **Element:** Defines `accept(visitor)`.
- **Concrete Element:** Calls the matching visitor method, enabling double dispatch.
- **Object Structure:** Holds or traverses the elements.

## 15.4 Java Implementation

This generic form returns a value from each visit operation, so one visitor can calculate area while another can export text.

```java
interface ShapeVisitor<T> {
    T visit(Circle circle);
    T visit(Rectangle rectangle);
}

interface Shape {
    <T> T accept(ShapeVisitor<T> visitor);
}

record Circle(double radius) implements Shape {
    @Override
    public <T> T accept(ShapeVisitor<T> visitor) {
        return visitor.visit(this);
    }
}

record Rectangle(double width, double height) implements Shape {
    @Override
    public <T> T accept(ShapeVisitor<T> visitor) {
        return visitor.visit(this);
    }
}

class AreaVisitor implements ShapeVisitor<Double> {
    @Override
    public Double visit(Circle circle) {
        return Math.PI * circle.radius() * circle.radius();
    }

    @Override
    public Double visit(Rectangle rectangle) {
        return rectangle.width() * rectangle.height();
    }
}

class DescriptionVisitor implements ShapeVisitor<String> {
    @Override
    public String visit(Circle circle) {
        return "Circle(radius=" + circle.radius() + ")";
    }

    @Override
    public String visit(Rectangle rectangle) {
        return "Rectangle(width=" + rectangle.width()
            + ", height=" + rectangle.height() + ")";
    }
}
```

Usage:

```java
java.util.List<Shape> shapes = java.util.List.of(
    new Circle(2.0),
    new Rectangle(3.0, 4.0)
);

AreaVisitor areaVisitor = new AreaVisitor();
DescriptionVisitor descriptionVisitor = new DescriptionVisitor();

for (Shape shape : shapes) {
    System.out.println(shape.accept(descriptionVisitor));
    System.out.println("Area = " + shape.accept(areaVisitor));
}
```

The client calls `accept()` on each shape. When `Circle.accept()` executes, `visitor.visit(this)` invokes `visit(Circle)`; when `Rectangle.accept()` executes, it invokes `visit(Rectangle)`. That is the central double-dispatch technique used by the classic Visitor pattern.

### Visitor trade-off

Adding a new operation is straightforward: create another visitor. Adding a new element type is more expensive because the visitor interface and every concrete visitor generally need a new method for that type.

## 15.5 Advantages, Disadvantages, and Use Cases

### Advantages

- Separates operations from element classes.
- Makes adding new operations convenient when element types are stable.
- Can centralize an operation that spans a heterogeneous object structure.
- Reduces the need to scatter operation-specific conditionals over every element.

### Disadvantages

- Adding a new element type requires updating visitors.
- Visitor can expose or depend on element-specific details.
- The pattern may be over-engineering for a tiny or highly dynamic object model.
- Mutable object structures and complex return values can make visitors harder to reason about.

### When to use

Use Visitor when the element types are relatively stable and new operations over those types are expected.

Examples include compiler abstract syntax trees, document object models, expression trees, geometry operations, and static analysis over structured objects.

---

# 16. Commonly Confused Behavioral Patterns

## 16.1 Chain of Responsibility vs Command

| Chain of Responsibility | Command |
|---|---|
| Focuses on selecting/forwarding a request through possible handlers. | Focuses on representing a request as an object. |
| A handler may process or forward the request. | A command encapsulates an operation and invokes a receiver. |
| Useful for pipelines and escalation. | Useful for queues, logs, scheduling, and undoable actions. |

They can be combined: each handler can receive a Command object.

## 16.2 Observer vs Mediator

| Observer | Mediator |
|---|---|
| Notifies subscribers about events or state changes. | Coordinates interactions among collaborating objects. |
| Communication is generally one-to-many. | Communication may be multi-directional and workflow-oriented. |
| Subject knows the observer registrations, not their business details. | Colleagues usually know the mediator abstraction. |

A mediator can use Observer for event delivery, but they solve different primary problems.

## 16.3 State vs Strategy

| State | Strategy |
|---|---|
| Behavior depends on the context's current state. | Behavior depends on the algorithm/policy selected for a task. |
| State transitions are usually part of the model. | The client or context usually selects the strategy. |
| Useful for lifecycles and valid-transition rules. | Useful for interchangeable calculations or algorithms. |

Their class structures can look almost identical. Choose based on intent rather than class diagrams alone.

## 16.4 Strategy vs Template Method

| Strategy | Template Method |
|---|---|
| Uses composition and delegates to an algorithm object. | Uses inheritance and overrides selected steps. |
| Algorithms can often be swapped at runtime. | Overall workflow is defined by the base class. |
| Good when behavior should vary independently. | Good when the step sequence must stay consistent. |

## 16.5 Command vs Strategy

| Command | Strategy |
|---|---|
| Represents a request/action. | Represents an algorithm/policy. |
| Often has `execute()` and may support queueing or undo. | Usually exposes a calculation or algorithm method. |
| The action may change application state. | The algorithm computes or decides how a task is done. |

A Command answers, “What action should be performed?” A Strategy answers, “Which way should this task be performed?”

## 16.6 Memento vs Command

- **Memento** captures the state needed for restoration.
- **Command** encapsulates an action.
- Undo can use either or both: a command may store the data needed to reverse itself, or it may save a Memento before executing.

## 16.7 Iterator vs Visitor

- **Iterator** provides a way to traverse elements.
- **Visitor** provides a way to perform operations on elements of different concrete types.

They are often combined: an Iterator traverses an object structure and a Visitor applies an operation to each element.

## 16.8 Interpreter vs Visitor

Interpreter models grammar rules as expression objects and evaluates expressions. Visitor can add operations over a stable expression tree, such as printing, optimization, or validation. The abstract syntax tree may use Composite-like structure, while Visitor performs additional operations over it.

## 16.9 Chain of Responsibility vs Mediator

- **Chain of Responsibility** passes a request through a sequence of possible handlers.
- **Mediator** centralizes coordination between collaborators.

A chain is usually directional; a mediator may coordinate a multi-directional workflow.

## 16.10 Template Method vs Strategy vs State at a glance

| Question | Pattern |
|---|---|
| “Which algorithm should I plug in?” | Strategy |
| “Which behavior is valid in the current lifecycle state?” | State |
| “Which steps vary while the overall workflow stays fixed?” | Template Method |

---

# 17. How Behavioral Patterns Work Together

Patterns solve distinct problems and can be composed when the design benefits from it.

## 17.1 Command + Memento

A command that supports undo can capture the pre-operation state in a Memento. The command performs the action, and undo restores the saved snapshot. This is useful when reversing an operation manually would be complicated.

## 17.2 Iterator + Visitor

An Iterator can traverse a collection or tree, while a Visitor performs an operation on each encountered element. For example, a visitor can calculate metrics for each node in a syntax tree.

## 17.3 Chain of Responsibility + Command

Handlers can process command objects. The chain can validate, authorize, enrich, or route a command before a receiver executes it.

## 17.4 Mediator + Observer

A Mediator can coordinate workflow decisions while using Observer-like subscriptions to receive notifications from colleagues. The mediator should still have a clear responsibility boundary.

## 17.5 State + Strategy

A state object may delegate a calculation to a strategy. For example, an order state determines whether shipping is allowed, while a shipping strategy calculates the cost for the chosen shipping option.

## 17.6 Interpreter + Visitor

A grammar can be represented as an expression tree. Interpreter evaluates the expression; separate visitors can format it, validate it, collect variables, or optimize it.

> Combine patterns only when each has a clear job. If several abstractions merely forward the same method call, simplify the design.

---

# 18. Behavioral Patterns in Java and Spring

The following are familiar examples, though framework implementations do not always map one-to-one to a pure GoF pattern.

| Pattern | Java / ecosystem example | Why it is related |
|---|---|---|
| Chain of Responsibility | Servlet filters, Spring Security filter chains, validation pipelines | Requests pass through ordered handlers/filters. |
| Command | `Runnable`, queued tasks, UI action objects | A unit of work can be passed to an invoker for execution. `Runnable` is command-like, though not the full GoF structure. |
| Interpreter | Regular-expression engines, expression evaluators, small rule languages | Expressions or languages are parsed/evaluated according to defined syntax. |
| Iterator | `Iterator`, `Iterable`, collection traversal | Standard sequential traversal abstraction. |
| Mediator | UI dialog coordinator, workflow coordinator, chat room | Interactions are managed through a coordinating object. |
| Memento | Editor undo snapshots, game checkpoints | Captures an earlier state for later restoration. |
| Observer | Event listeners, property-change listeners, Spring application events | Components react to published events. |
| State | State-driven workflow objects, protocol/session state | Behavior is delegated based on lifecycle state. |
| Strategy | `Comparator`, pluggable policies, Spring-injected implementations | Interchangeable algorithms are supplied through an abstraction. |
| Template Method | Framework lifecycle hooks and base classes with overridable steps | Base class defines workflow order and extension points. |
| Visitor | Visitors over syntax trees or structured document models | Operations are dispatched based on element type. |

### Notes for Java interviews

- `Comparator` is a particularly recognizable Strategy-like abstraction because different comparison policies can be passed into sorting operations.
- `Iterator` and `Iterable` are explicit APIs for iterator-based traversal.
- `Runnable` packages work behind `run()`, making it command-like, but a complete Command implementation often also includes a receiver and invoker.
- Spring filters commonly form a chain, but the details of the framework's control flow matter; do not assume every filter implementation is identical to the textbook diagram.
- Event publication in Spring is Observer-like; asynchronous delivery and distributed events introduce additional architectural concerns.

---

# 19. Advantages of Behavioral Patterns

- **Loose coupling:** Senders and receivers can depend on abstractions instead of each other's concrete implementations.
- **Clear responsibilities:** Algorithms, states, request handling, and notifications become easier to isolate.
- **Extensibility:** New handlers, strategies, observers, commands, or visitors can often be added with limited impact.
- **Testability:** Focused components can be tested independently.
- **Reduced conditional complexity:** State and Strategy can replace large behavior-selection conditionals when used appropriately.
- **Workflow control:** Template Method, Mediator, and Chain of Responsibility make common process structures explicit.
- **Reusability:** Behavior can be reused across different contexts through well-designed interfaces.

---

# 20. Disadvantages and Common Mistakes

## 20.1 Too many classes

A separate class for every minor operation can hide the actual workflow. Use the simplest design that still handles realistic change requirements.

## 20.2 Choosing by name rather than intent

State and Strategy may share the same structure, but the business meaning is different. Pick the pattern because its intent matches the problem.

## 20.3 Hiding control flow

Observer, Mediator, and Chain of Responsibility can distribute execution across several objects. Use clear names, logging, and tests so the flow remains traceable.

## 20.4 Overgrown coordinators

A Mediator that owns every business rule becomes a god object. Keep coordination separate from domain logic where practical.

## 20.5 Ignoring failure behavior

For handlers, commands, and observers, explicitly define what happens when no handler matches, execution fails, or a listener throws an exception.

## 20.6 Unsafe undo assumptions

Not every action can be reversed. Email delivery, an external payment, or a remote API call may require a compensating action rather than in-memory restoration.

## 20.7 Leaking state or snapshots

Memento should preserve encapsulation. Ensure snapshots are immutable or safely isolated and limit history growth.

## 20.8 Using Interpreter for an entire programming language

Hand-built expression hierarchies are appropriate for small grammars. Complex syntax often warrants a real parser.

## 20.9 Inheritance where composition is more appropriate

Template Method relies on inheritance. If algorithms need to vary independently at runtime, Strategy may be the better option.

---

# 21. Interview-Oriented Decision Tree

Use the problem's intent to narrow the choice.

```text
Does the problem mainly involve behavior or object communication?
|
+-- A request may be handled by one of several possible receivers
|      -> Chain of Responsibility
|
+-- A request/action should be represented, queued, logged, or undone
|      -> Command
|
+-- A small grammar or expression language must be evaluated
|      -> Interpreter
|
+-- A collection should be traversed without exposing its structure
|      -> Iterator
|
+-- Many objects need a coordinator for their interactions
|      -> Mediator
|
+-- An earlier state must be saved and restored
|      -> Memento
|
+-- Subscribers need notifications when something changes
|      -> Observer
|
+-- Valid behavior depends on the object's current lifecycle state
|      -> State
|
+-- One of several interchangeable algorithms should be selected
|      -> Strategy
|
+-- The workflow is fixed but some steps differ by implementation
|      -> Template Method
|
+-- New operations are needed over stable element types
       -> Visitor
```

### Four interview questions to ask yourself

1. What changes frequently: algorithm, state, workflow step, receiver, or object interaction?
2. Who should own the decision or coordination responsibility?
3. Which classes should know about each other, and which should remain decoupled?
4. What are the failure cases, lifecycle rules, or extension costs introduced by this pattern?

---

# 22. One-Page Cheat Sheet

| Pattern | Remember | Use when |
|---|---|---|
| Chain of Responsibility | **Pass along** | Multiple handlers may handle a request. |
| Command | **Request object** | Work needs to be queued, stored, logged, or undone. |
| Interpreter | **Grammar / expression tree** | A small language or expression needs evaluation. |
| Iterator | **Traverse** | The client should not know how a collection is stored. |
| Mediator | **Coordinator** | Collaborating objects have complicated interdependencies. |
| Memento | **Snapshot / restore** | State needs to be restored without exposing internals. |
| Observer | **Notify subscribers** | Many consumers react to changes in a subject. |
| State | **Behavior by state** | An object's lifecycle state changes its valid behavior. |
| Strategy | **Interchangeable algorithm** | Multiple algorithms solve the same task. |
| Template Method | **Fixed skeleton** | Workflows share a stable sequence but differ in steps. |
| Visitor | **New operations** | Element types are stable but operations keep growing. |

### Quick comparisons to memorize

```text
State vs Strategy
- State: behavior depends on current state / transitions.
- Strategy: behavior depends on selected algorithm / policy.

Strategy vs Template Method
- Strategy: composition; swap algorithm objects.
- Template Method: inheritance; override steps in a fixed workflow.

Observer vs Mediator
- Observer: notify subscribers about changes.
- Mediator: coordinate interactions among colleagues.

Command vs Strategy
- Command: represent an action/request.
- Strategy: represent a way to perform a task.

Iterator vs Visitor
- Iterator: how to traverse.
- Visitor: what operation to perform on element types.
```

---

# 23. Important Interview Questions and Answers

## Q1. What are behavioral design patterns?

They are reusable design approaches for organizing communication, responsibility, and control flow between objects and classes.

## Q2. How many classic GoF behavioral patterns are there?

There are **11** classic GoF behavioral patterns.

## Q3. What is the key difference between Chain of Responsibility and Command?

Chain of Responsibility forwards a request through handlers. Command encapsulates a request as an object so it can be executed independently of the sender.

## Q4. What is the main benefit of Command?

It decouples the invoker from the receiver and can enable queuing, logging, scheduling, retries, and undo/redo.

## Q5. When would you use Interpreter?

When a relatively small and stable grammar or expression language must be represented and evaluated. For complex languages, use a dedicated parser.

## Q6. How does Iterator help encapsulation?

It lets clients traverse elements through a standard interface without depending on how the collection stores them.

## Q7. What is the difference between Mediator and Observer?

Mediator centralizes coordination among collaborators. Observer establishes notifications from a subject to subscribed observers. A mediator can use observer-style notifications internally.

## Q8. What is the role of the caretaker in Memento?

The caretaker stores and manages snapshots, such as a history stack, without inspecting or modifying their internal state.

## Q9. Why is Observer useful?

A subject can notify multiple consumers without containing consumer-specific behavior or knowing each consumer's concrete implementation.

## Q10. What is the biggest difference between State and Strategy?

State represents behavior tied to an object's current state and often owns transitions. Strategy represents an interchangeable algorithm selected to perform a task.

## Q11. How does Strategy follow composition over inheritance?

The context holds a reference to a strategy interface and delegates an operation to that object instead of relying on a subclass for each algorithm variation.

## Q12. What is a hook in Template Method?

A hook is an optional method with a default implementation that a subclass may override to customize the workflow.

## Q13. What is the key trade-off of Visitor?

Adding a new operation is usually easy by creating a new visitor, but adding a new element type generally requires updating the visitor interface and concrete visitors.

## Q14. Can patterns be combined?

Yes. For example, Command can use Memento for undo, Iterator can traverse a structure processed by Visitor, and Mediator can use Observer-style event notifications.

## Q15. Does every `if-else` statement need a pattern?

No. Patterns are useful when a recurring design problem or likely change justifies the extra abstraction. Simple conditional logic is often clearer for a small, stable requirement.

## Q16. Which behavioral pattern is useful for undo/redo?

Command is useful for representing actions; Memento is useful for snapshotting state. Applications may use either pattern or combine them, depending on how restoration should work.

## Q17. Is Java's `Comparator` related to Strategy?

Yes. Different `Comparator` implementations encapsulate different comparison rules and can be supplied interchangeably to sorting operations.

## Q18. Is a Spring filter chain an example of Chain of Responsibility?

It is a common chain-like example: a request passes through ordered filters, each of which can process the request or continue the chain. Framework details may add behavior beyond the basic textbook pattern.

## Q19. Which pattern is useful when multiple objects coordinate a UI workflow?

Mediator can centralize the interactions, while Observer may be used to deliver control-change notifications. The right choice depends on whether the main problem is coordination or subscription-based notification.

## Q20. Which pattern is useful for a stable object model with new operations added frequently?

Visitor is a candidate, because operations are placed in visitor classes. It works best when the set of element types is relatively stable.

---

# 24. Final Mental Model

Remember these 11 ideas:

```text
Chain of Responsibility -> PASS IT ALONG
Command                 -> REQUEST AS AN OBJECT
Interpreter             -> GRAMMAR / EVALUATION
Iterator                -> TRAVERSE
Mediator                -> COORDINATE
Memento                 -> SAVE / RESTORE
Observer                -> NOTIFY
State                   -> BEHAVIOR BY STATE
Strategy                -> CHOOSE AN ALGORITHM
Template Method         -> FIX THE SKELETON
Visitor                 -> ADD AN OPERATION
```

For each pattern, practice explaining four things:

1. **What problem does it solve?**
2. **What are its main participants and their responsibilities?**
3. **Can I implement a small Java example?**
4. **What trade-offs would make me avoid it?**

The strongest interview answer explains not only the definition but also why the pattern is appropriate, how it differs from similar patterns, and what complexity it introduces.

---

# 25. Reference Material

1. **GeeksforGeeks — Behavioral Design Patterns**  
   https://www.geeksforgeeks.org/system-design/behavioral-design-patterns/

2. **SourceMaking — Behavioral Patterns**  
   https://sourcemaking.com/design_patterns/behavioral_patterns

3. **Design Patterns: Elements of Reusable Object-Oriented Software** — Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides (the “Gang of Four” / GoF). This is the classic catalog defining the 23 patterns across creational, structural, and behavioral categories.

---

## Study Recommendation

For Low-Level Design and Java interviews, study the patterns in this order:

```text
1. Strategy
       ↓
2. Observer
       ↓
3. Chain of Responsibility
       ↓
4. Command
       ↓
5. State
       ↓
6. Template Method
       ↓
7. Mediator
       ↓
8. Iterator
       ↓
9. Memento
       ↓
10. Visitor
       ↓
11. Interpreter
```

Start with Strategy, Observer, Chain of Responsibility, Command, and State because their intent is easy to connect to everyday backend/application code. Then study the patterns that need more specific modeling, such as Interpreter and Visitor. For every pattern, create a small Java example and compare it with at least one similar pattern.
