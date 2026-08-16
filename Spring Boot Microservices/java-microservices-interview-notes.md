# Java Microservices Interview Questions & Answers

> **Purpose:** Interview preparation notes for Java / Spring Boot Microservices.
>
> **Source:** GeeksforGeeks — *Java Microservices Interview Questions and Answers*

> **Note:** The questions follow the structure of the referenced article. The answers below are concise, interview-oriented notes written in original wording for revision.

---

## Table of Contents

### Beginner
1. [What are Microservices?](#q1)
2. [Why are Microservices used?](#q2)
3. [Monolithic vs Microservices Architecture](#q3)
4. [What is REST API in Microservices?](#q4)
5. [Main components of Microservices Architecture](#q5)
6. [Why is Spring Boot widely used for Microservices?](#q6)
7. [What is Eureka Server?](#q7)
8. [Advantages of Microservices over Monolithic Architecture](#q8)
9. [What is Inter-Service Communication?](#q9)
10. [What does Stateless mean in Microservices?](#q10)
11. [What is an API Gateway?](#q11)
12. [Synchronous vs Asynchronous Communication](#q12)
13. [Stateless vs Stateful Microservices](#q13)
14. [What is Database-per-Service?](#q14)
15. [Why should each Microservice have its own Database?](#q15)
16. [Common communication protocols](#q16)
17. [Challenges of Microservices](#q17)
18. [What is Load Balancing?](#q18)

### Intermediate
19. [What is Centralized Configuration?](#q19)
20. [What is Client-Side Load Balancing?](#q20)
21. [How does Spring Cloud Config work?](#q21)
22. [What is Fault Tolerance?](#q22)
23. [What is the Circuit Breaker Pattern?](#q23)
24. [What is Resilience4j?](#q24)
25. [What is Distributed Tracing?](#q25)
26. [Why is Docker used?](#q26)
27. [Why is Kubernetes used?](#q27)
28. [How do Microservices communicate?](#q28)
29. [How do you monitor Microservices?](#q29)
30. [Common Microservices deployment strategies](#q30)
31. [How do you manage configuration across Microservices?](#q31)
32. [How do you test Microservices?](#q32)
33. [REST vs gRPC vs Message Queues](#q33)

### Advanced
34. [How do you decompose a Monolith using DDD and Bounded Context?](#q34)
35. [What is the Saga Pattern?](#q35)
36. [What is Eventual Consistency?](#q36)
37. [Best practices for designing Microservices](#q37)
38. [How do you secure Microservices?](#q38)
39. [Event Sourcing vs Traditional CRUD](#q39)
40. [How do you manage Schema Evolution in event-driven systems?](#q40)
41. [When should you use WebSockets instead of REST APIs?](#q41)

---

# Beginner Microservices Interview Questions
<a id="q1"></a>
## 1. What are Microservices?

**Answer:**

Microservices is an architectural style in which a large application is divided into small, independent services. Each service performs a specific business function and communicates with other services using lightweight protocols such as HTTP/REST, gRPC, or messaging systems like Kafka or RabbitMQ.

Key characteristics:

- Loosely coupled services
- High cohesion within each service
- Independent deployment
- Independent scaling
- Service-specific data ownership
- Communication through APIs or messaging
- Better fault isolation

**Example:**

An e-commerce system can be divided into:

- User Service
- Product Service
- Order Service
- Payment Service
- Notification Service

---
<a id="q2"></a>
## 2. Why are Microservices used?

**Answer:**

Microservices are used to simplify the development, deployment, and maintenance of large applications by dividing them into smaller independent services.

### Main advantages

- Independent development and deployment
- Independent scaling
- Faster release cycles
- Better fault isolation
- Smaller and more maintainable codebases
- Technology flexibility
- Easier CI/CD adoption

**Interview point:** Microservices are especially useful when a system is large, has multiple teams, and different business capabilities need to evolve independently.

---
<a id="q3"></a>
## 3. What is the difference between Monolithic and Microservices Architecture?

**Answer:**

| Aspect | Monolithic | Microservices |
|---|---|---|
| Deployment | Entire application deployed together | Services deployed independently |
| Scaling | Whole application is scaled | Individual services can be scaled |
| Coupling | Usually tightly coupled | Loosely coupled |
| Database | Common database is common | Preferably database per service |
| Failure isolation | Lower | Better |
| Technology | Usually one technology stack | Different stacks can be used |
| Deployment complexity | Lower initially | Higher |
| Suitable for | Small/simple systems | Large/distributed systems |

### Simple example

**Monolith:**

```text
Client
  |
  v
+-------------------------------+
| User + Order + Payment +      |
| Product + Notification        |
+-------------------------------+
              |
              v
           Database
```

**Microservices:**

```text
                 +----------------+
                 |  API Gateway   |
                 +-------+--------+
                         |
        +----------------+----------------+
        |        |        |       |       |
        v        v        v       v       v
      User    Product   Order   Payment  Notify
       |        |        |       |       |
      DB       DB       DB      DB      DB
```

---
<a id="q4"></a>
## 4. What is REST API in Microservices?

**Answer:**

REST (Representational State Transfer API) is a commonly used HTTP-based communication style between microservices.

Typical HTTP methods:

| Method | Purpose |
|---|---|
| GET | Read data |
| POST | Create data |
| PUT | Replace/update data |
| PATCH | Partially update data |
| DELETE | Delete data |

**Example:**

```http
GET /orders/101
POST /orders
PUT /orders/101
DELETE /orders/101
```

REST is generally simple and widely supported, making it suitable for public APIs and synchronous service communication.

---
<a id="q5"></a>
## 5. What are the main components of a Microservices Architecture?

**Answer:**

A typical microservices ecosystem contains:

- Each module (Employee, Course, Customer) is an independent microservice.
- Git stores the source code for each service.
- Jenkins automatically builds, tests, and deploys the services (CI/CD).
- Each service runs on its own server/container.
- Every microservice has its own database, ensuring loose coupling and independent data management.
- Services can be developed, deployed, updated, and scaled independently.

- Microservices
- API Gateway
- Service Discovery / Service Registry
- Load Balancer
- Configuration Server
- Databases
- Message Broker
- Monitoring and Logging
- Distributed Tracing
- CI/CD pipeline
- Container runtime/orchestration

**Typical flow:**

```text
Client
  |
  v
API Gateway
  |
  +----> Service A ----> DB
  |
  +----> Service B ----> DB
  |
  +----> Service C ----> DB
           |
           v
      Message Broker
```

---
<a id="q6"></a>
## 6. Why is Spring Boot widely used for Microservices?

**Answer:**

Spring Boot reduces the amount of configuration required to build production-ready Java applications.

### Important features

- Auto-configuration
- Starter dependencies
- Embedded server
- Easy REST API development
- Spring Cloud integration
- Spring Security integration
- Actuator for monitoring
- Easy testing support

**Interview point:** Spring Boot handles much of the infrastructure setup so developers can focus on business functionality.

---
<a id="q7"></a>
## 7. What is Eureka Server?

**Answer:**

Eureka is a service registry used for service discovery in the Spring Cloud ecosystem.

Instead of hardcoding service URLs, services register themselves with Eureka and other services can discover their current locations.

```text
Service A ----\
Service B -----+----> Eureka Server
Service C ----/

Service A ---> asks Eureka ---> "Where is Service B?"
Service A <--- Eureka -------- "Service B is at instance X"
```

### Benefits

- Dynamic service discovery
- Avoids hardcoded URLs
- Supports multiple service instances
- Helps with dynamic scaling

---
<a id="q8"></a>
## 8. What are the advantages of Microservices over Monolithic Architecture?

**Answer:**

Important advantages include:

1. Independent deployment
2. Independent scaling
3. Faster releases
4. Better fault isolation
5. Smaller codebases
6. Easier team ownership
7. Technology flexibility
8. Better alignment with business capabilities

**Trade-off:** Microservices also introduce distributed-system complexity, so they should not be adopted automatically for every application.

---
<a id="q9"></a>
## 9. What is Inter-Service Communication?

**Answer:**

Inter-service communication is the mechanism through which microservices exchange data and trigger operations.

### Common approaches

- REST APIs
- OpenFeign
- gRPC
- Apache Kafka
- RabbitMQ

### Example

```text
Order Service
     |
     | REST / Feign
     v
Payment Service
```

Or asynchronously:

```text
Order Service
     |
     | OrderCreated event
     v
   Kafka
     |
     v
Notification Service
```

---
<a id="q10"></a>
## 10. What does Stateless mean in the context of Microservices?

**Answer:**

A stateless microservice does not retain / store client-specific session information between requests.

Each request contains enough information for the service to process it.

### Benefits

- Easier horizontal scaling
- Easier load balancing
- Better fault tolerance
- Easier replacement of instances

**Example:** A REST API using JWT authentication can remain stateless because authentication information is carried in the token.

---
<a id="q11"></a>
## 11. What is an API Gateway?

**Answer:**

An API Gateway is the common entry point between clients and backend microservices.

### Responsibilities

- Request routing
- Authentication and authorization
- Rate limiting
- Load balancing
- Request/response transformation
- Logging
- Monitoring
- Sometimes caching

```text
Client
  |
  v
API Gateway
  |
  +----> User Service
  +----> Order Service
  +----> Payment Service
```

**Interview point:** The gateway hides internal service topology from clients.

---
<a id="q12"></a>
## 12. What is Synchronous and Asynchronous Communication?

**Answer:**

### Synchronous

The caller waits for the receiver's response.

```text
Order Service ---> Payment Service
       |                 |
       | <--- Response --|
       |
       continues
```

Common technologies:

- REST
- gRPC

### Asynchronous

The sender publishes a message/event and does not need to wait for an immediate response.

```text
Order Service ---> Kafka ---> Payment/Notification Service
```

Common technologies:

- Kafka
- RabbitMQ

| Feature | Synchronous | Asynchronous |
|---|---|---|
| Response | Immediate | Later/not required |
| Coupling | Higher | Lower |
| Typical use | Request/response | Events/workflows |
| Examples | REST, gRPC | Kafka, RabbitMQ |

---
<a id="q13"></a>
## 13. What is the difference between Stateless and Stateful Microservices?

**Answer:**

| Aspect | Stateless | Stateful |
|---|---|---|
| Client state | Not stored in service memory | State is maintained |
| Scaling | Easier | More difficult |
| Load balancing | Easier | Requires state awareness or external state |
| Failure recovery | Easier | More complex |
| Example | REST API | Session-heavy application |

**Best practice:** Keep services stateless where practical and externalize shared state when required.

---
<a id="q14"></a>
## 14. What is the Database-per-Service Pattern?

**Answer:**

Database-per-Service means every microservice owns its data store and other services should not directly access that database.

```text
User Service ----> User DB
Order Service ---> Order DB
Payment Service -> Payment DB
```

Services communicate through APIs or events rather than directly querying another service's database.

### Benefits

- Loose coupling
- Independent scaling
- Independent schema evolution
- Better data ownership

---
<a id="q15"></a>
## 15. Why should each Microservice have its own Database?

**Answer:**

The main reason is **data ownership and loose coupling**.

If multiple services directly share one database:

- Schema changes can break multiple services.
- Services become tightly coupled.
- Independent deployment becomes harder.
- One service can accidentally modify another service's data.

With database-per-service:

- Each service owns its data.
- Services can choose appropriate database technologies.
- Scaling can be done independently.

---
<a id="q16"></a>
## 16. What are the common communication protocols used in Microservices?

**Answer:**

| Protocol/Technology | Typical Usage |
|---|---|
| REST/HTTP | Request-response APIs |
| gRPC | High-performance internal communication |
| Kafka | Event streaming |
| RabbitMQ | Message queuing |
| WebSockets | Real-time bidirectional communication |

### Rule of thumb

- REST → simple APIs
- gRPC → high-performance internal calls
- Kafka → event streaming/high-throughput events
- RabbitMQ → message queues/work distribution
- WebSockets → real-time client communication

---
<a id="q17"></a>
## 17. What are the Challenges of Microservices?

**Answer:**

Microservices solve some scalability problems but introduce distributed-system challenges.

### Major challenges

- Network failures
- Increased latency
- Distributed transactions
- Data consistency
- Service discovery
- Configuration management
- Authentication/authorization
- Monitoring and distributed tracing
- Log aggregation
- Deployment complexity
- Version compatibility
- Debugging across services

**Interview point:** The biggest conceptual challenge is moving from in-process communication to unreliable network communication.

---
<a id="q18"></a>
## 18. What is Load Balancing in Microservices?

**Answer:**

Load balancing distributes incoming requests across multiple instances of a service.

```text
                +--> Service Instance 1
Client -> LB ---+--> Service Instance 2
                +--> Service Instance 3
```

### Benefits

- Prevents instance overload
- Improves availability
- Supports horizontal scaling
- Helps distribute traffic

### Types

- Client-side load balancing (Spring Cloud LoadBalancer)
- Server-side load balancing (Nginx, HAProxy).

Examples include Spring Cloud LoadBalancer, Nginx, and HAProxy.

---

# Intermediate Microservices Interview Questions
<a id="q19"></a>
## 19. What is Centralized Configuration?

**Answer:**

Centralized configuration stores configuration for multiple microservices in a common location.

Example:

```text
                 Config Server
                /      |      \
               /       |       \
          Service A Service B Service C
```

### Benefits

- One source of configuration
- Environment-specific configuration
- Version-controlled configuration
- Easier configuration changes
- Reduced duplication

---
<a id="q20"></a>
## 20. What is Client-Side Load Balancing?

**Answer:**

In client-side load balancing, the client/service calling another service selects which service instance should receive the request.

```text
Order Service
     |
     v
Client-side LB
   /   |   \
  v    v    v
 P1   P2   P3
```

The client obtains available instances from service discovery and applies a load-balancing algorithm.

**Spring example:** Spring Cloud LoadBalancer.

---
<a id="q21"></a>
## 21. How does Spring Cloud Config work?

**Answer:**

Spring Cloud Config provides centralized configuration management for distributed applications.

Typical flow:

```text
Git Repository
      |
      v
Config Server
      |
      +----> Service A
      +----> Service B
      +----> Service C
```

### Steps

1. Configuration files are stored in a repository.
2. Config Server reads them.
3. Microservices request their configuration.
4. Services use environment-specific properties.
5. Configuration can be refreshed depending on the setup.

### Benefits

- Centralized management
- Git version control
- Environment-specific configuration
- Reduced duplication

---
<a id="q22"></a>
## 22. What is Fault Tolerance in Microservices?

**Answer:**

Fault tolerance is the ability of a system to continue providing useful functionality even when some components fail.

Common techniques:

- Timeouts
- Retries
- Circuit breakers
- Fallbacks
- Bulkheads
- Load balancing
- Redundancy
- Graceful degradation

**Goal:** Prevent a failure in one service from becoming a cascading failure across the entire system.

---
<a id="q23"></a>
## 23. What is the Circuit Breaker Pattern?

**Answer:**

A circuit breaker prevents continuous calls to an unhealthy downstream service.

### States

#### 1. Closed

Requests flow normally.

```text
Client ---> Service B
```

#### 2. Open

After failures cross a configured threshold, requests are blocked temporarily.

```text
Client ---> Circuit Breaker -X-> Service B
                    |
                    +--> Fallback
```

#### 3. Half-Open

A limited number of test requests are allowed.

- If successful → circuit returns to Closed.
- If failures continue → circuit returns to Open.

### Benefits

- Prevents cascading failures
- Reduces unnecessary network calls
- Enables graceful degradation
- Gives failing services time to recover

---
<a id="q24"></a>
## 24. What is Resilience4j?

**Answer:**

Resilience4j is a lightweight Java fault-tolerance library commonly used with Spring Boot.

Important modules:

- **CircuitBreaker** → stops repeated calls to failing services
- **Retry** → retries transient failures
- **RateLimiter** → limits request rate
- **Bulkhead** → isolates resources
- **TimeLimiter** → limits execution time
- **Fallback** → provides alternative behavior

Example:

```java
@CircuitBreaker(name = "paymentService", fallbackMethod = "fallback")
public PaymentResponse makePayment() {
    // call payment service
}
```

---
<a id="q25"></a>
## 25. What is Distributed Tracing?

**Answer:**

Distributed tracing tracks a request as it moves across multiple services.

```text
Client
  |
  v
Gateway
  |
  v
Order Service
  |
  v
Payment Service
  |
  v
Notification Service
```

A trace/correlation identifier allows engineers to follow the complete request path.

### Benefits

- Finds slow services
- Identifies failures
- Measures latency
- Helps debug distributed requests
- Provides end-to-end visibility

Common technologies:

- OpenTelemetry
- Zipkin
- Jaeger

---
<a id="q26"></a>
## 26. Why is Docker used in Microservices?

**Answer:**

Docker packages an application together with its runtime dependencies into a container.

```text
+-------------------------+
| Docker Container        |
| Spring Boot Application |
| JDK / Runtime           |
| Dependencies            |
| Configuration           |
+-------------------------+
```

### Benefits

- Consistent environments
- Portable deployments
- Faster startup
- Isolation
- Easier scaling
- Works well with Kubernetes

---
<a id="q27"></a>
## 27. Why is Kubernetes used in Microservices?

**Answer:**

Kubernetes is a container orchestration platform used to deploy, manage, and scale containerized microservices automatically.

It automates:

- Deployment
- Scaling
- Service discovery
- Load balancing
- Self-healing
- Rolling updates
- Rollbacks

### Example

```text
Kubernetes Cluster
       |
       +-- Order Deployment
       |      +-- Pod 1
       |      +-- Pod 2
       |
       +-- Payment Deployment
              +-- Pod 1
              +-- Pod 2
```

---
<a id="q28"></a>
## 28. How do Microservices communicate with each other?

**Answer:**

Microservices communicate with each other using synchronous or asynchronous communication mechanisms over a network.

Microservices generally use:

### Synchronous communication

- REST
- OpenFeign
- gRPC

The caller waits for a response.

### Asynchronous communication

- Kafka
- RabbitMQ

The producer sends a message/event and consumers process it independently.

**Interview consideration:** Choose communication based on latency, coupling, reliability, throughput, and business requirements.

---
<a id="q29"></a>
## 29. How do you monitor Microservices effectively?

**Answer:**

Monitoring should cover **logs, metrics, traces, and health**.

### Common tools

| Requirement | Tool |
|---|---|
| Health checks | Spring Boot Actuator |
| Metrics | Prometheus |
| Visualize metrics and Dashboards | Grafana |
| Centralized logs | ELK/EFK |
| Distributed tracing | OpenTelemetry, Zipkin, Jaeger |

### What should be monitored?

- CPU/memory
- Request count
- Error rate
- Response time
- Database latency
- Kafka lag
- Service availability
- Dependency failures

---
<a id="q30"></a>
## 30. What are common deployment strategies for Microservices?

**Answer:**

### 1. Blue-Green Deployment

Maintain two environments:

```text
Blue  -> Current production
Green -> New version
```

Traffic is switched to Green after validation.

### 2. Canary Deployment

Release the new version to a small percentage of users first.

```text
95% -> Old version
5%  -> New version
```

### 3. Rolling Deployment

Gradually replace old instances with new instances.

### 4. Recreate Deployment

Stop the old version and deploy the new version.

### 5. A/B Testing

Different users receive different versions to compare behavior.

---
<a id="q31"></a>
## 31. How do you manage configuration across Microservices?

**Answer:**

Common approaches include:

- Spring Cloud Config Server
- Git-based configuration
- Environment variables
- `application.properties`
- `application.yml`
- Kubernetes ConfigMaps
- Kubernetes Secrets
- Secret-management tools such as Vault

**Best practice:** Keep configuration outside application code and keep secrets out of source control.

---
<a id="q32"></a>
## 32. How do you test Microservices?

**Answer:**

Microservices should be tested at multiple levels.

### 1. Unit Testing

Tests individual classes/methods.

**Tools:** JUnit, Mockito

### 2. Integration Testing

Tests interaction with databases, APIs, messaging systems, etc.

### 3. Contract Testing

Verifies that service consumers and providers agree on an API contract.

**Tools:** Spring Cloud Contract, Pact

### 4. End-to-End Testing

Tests a complete business workflow across multiple services.

### 5. Performance Testing

Tests throughput, latency, and behavior under load.

**Tools:** JMeter, Gatling

---
<a id="q33"></a>
## 33. What are the differences between REST, gRPC, and Message Queues?

**Answer:**

| Feature | REST | gRPC | Message Queue |
|---|---|---|---|
| Communication | Usually synchronous | Usually synchronous | Asynchronous |
| Protocol | HTTP/HTTPS | HTTP/2 | Broker-based |
| Data | JSON/XML commonly | Protocol Buffers | Messages/events |
| Performance | Moderate | High | High for async workloads |
| Best for | Public APIs | Internal service calls | Event-driven workflows |
| Examples | REST API | gRPC | Kafka, RabbitMQ |

**Interview point:** There is no universally best option. Choose based on use case.

---

# Advanced Microservices Interview Questions
<a id="q34"></a>
## 34. How do you decompose a Monolithic application into Microservices using DDD and Bounded Context?

**Answer:**

Do not split a monolith simply by technical layers such as controller/service/repository. Split it according to **business capabilities**.

### Steps

1. Understand the business domain.
2. Identify business capabilities.
3. Define bounded contexts.
4. Identify entities and aggregates.
5. Define service boundaries.
6. Assign data ownership.
7. Extract services incrementally.
8. Introduce APIs/events between services.
9. Monitor and validate service boundaries.

### Example

```text
E-Commerce Monolith
       |
       +--> Product Context  -> Product Service
       +--> Order Context    -> Order Service
       +--> Payment Context  -> Payment Service
       +--> User Context     -> User Service
```

### What is Bounded Context?

A bounded context defines a clear boundary within which a particular domain model and terminology are valid.

---
<a id="q35"></a>
## 35. What is the Saga Pattern?

**Answer:**

Saga is a pattern for managing distributed business transactions without using one global database transaction.

A business transaction is divided into multiple local transactions.

### Example

```text
Create Order
    |
    v
Reserve Inventory
    |
    v
Process Payment
    |
    v
Confirm Order
```

If payment fails, a compensating action can release the reserved inventory.

### Types

#### Choreography

Services react to events.
Services communicate through events without a central coordinator.

```text
Order -> Event -> Inventory
Inventory -> Event -> Payment
Payment -> Event -> Order
```

#### Orchestration

A central Saga orchestrator coordinates the steps.

```text
        Saga Orchestrator
       /       |        \
      v        v         v
   Order   Inventory   Payment
```

### Key point

Saga provides eventual consistency and uses **compensating transactions** instead of a traditional global transaction.

---
<a id="q36"></a>
## 36. What is Eventual Consistency?

**Answer:**

Eventual consistency means that distributed services may temporarily have different versions of data, but they converge to a consistent state after updates propagate.

Example:

```text
Order Service
     |
 OrderCreated
     v
   Kafka
   /   \
  v     v
Inventory Notification
```

The Order service may confirm the order before Inventory has processed the event.

### Benefits

- Better scalability
- Higher availability
- Loose coupling
- Suitable for event-driven systems

### Trade-off

Applications must tolerate temporary inconsistencies.

---
<a id="q37"></a>
## 37. What are the Best Practices for designing Microservices?

**Answer:**

### Architecture

- Design around business capabilities.
- Use DDD (Domain-Driven Design) where appropriate.
- Keep services highly cohesive.
- Keep coupling low.

### Data

- Prefer database-per-service.
- Define clear ownership.
- Avoid direct database sharing.

### Communication

- Use well-defined APIs/events.
- Set timeouts.
- Use retries carefully.
- Use idempotency where required.

### Resilience

- Circuit breakers
- Bulkheads
- Retries
- Timeouts
- Fallbacks

### Observability

- Centralized logging
- Metrics
- Distributed tracing
- Health checks

### Security

- Authentication
- Authorization
- TLS
- Secret management

### Deployment

- CI/CD
- Containers
- Automated testing
- Safe deployment strategies

---
<a id="q38"></a>
## 38. How do you secure Microservices?

**Answer:**

Security should be applied at multiple layers.

### Authentication

Verify who the user/service is.

Common approaches:

- OAuth 2.0
- OpenID Connect
- JWT

### Authorization

Verify what the authenticated identity is allowed to do.

Examples:

- RBAC
- Scopes
- Permissions

### Network security

- HTTPS/TLS
- Secure service-to-service communication
- Network policies

### API security

- API Gateway
- Rate limiting
- Input validation
- CORS configuration where required

### Secret management

Do not hardcode secrets in source code.

Use:

- Kubernetes Secrets
- Vault
- Cloud secret managers

---
<a id="q39"></a>
## 39. What is the difference between Event Sourcing and Traditional CRUD?

**Answer:**

### Traditional CRUD

Stores the current state.

```text
Account Balance = 5000
```

Previous changes may not be available unless an audit mechanism is added.

### Event Sourcing

Stores state changes as immutable events.

```text
AccountCreated
MoneyDeposited(10000)
MoneyWithdrawn(3000)
MoneyWithdrawn(2000)
```

Current state can be rebuilt by replaying events.

| Aspect | Event Sourcing | Traditional CRUD |
|---|---|---|
| Storage | Events | Current state |
| History | Built-in | Requires separate audit |
| Recovery | Replay events | Read current state |
| Complexity | Higher | Lower |
| Best use | Event-driven/audit-heavy systems | Standard business CRUD |

---
<a id="q40"></a>
## 40. How do you manage Schema Evolution in Event-Driven Systems?

**Answer:**

Schema evolution means changing event structures without breaking existing producers or consumers.

### Best practices

1. Use a Schema Registry where appropriate.
2. Maintain backward compatibility.
3. Consider forward compatibility.
4. Version schemas/events.
5. Prefer adding optional fields.
6. Avoid removing or renaming fields abruptly.
7. Use formats such as Avro, Protobuf, or JSON Schema.
8. Test compatibility before deployment.

### Example

Old event:

```json
{
  "orderId": 101,
  "amount": 500
}
```

New compatible event:

```json
{
  "orderId": 101,
  "amount": 500,
  "currency": "INR"
}
```

Adding an optional field is generally safer than removing an existing field.

---
<a id="q41"></a>
## 41. When should you use WebSockets instead of REST APIs in Microservices?

**Answer:**

Use WebSockets when the application requires continuous, low-latency, bidirectional communication.

### Good use cases

- Chat applications
- Real-time notifications
- Online gaming
- Live dashboards
- Collaborative applications
- Live market/price updates

### REST is better for

- CRUD operations
- Standard request-response APIs
- Public APIs
- Stateless interactions

### WebSocket advantages

- Full-duplex communication
- Persistent connection
- Efficient real-time updates
- Avoids repeated polling requests

### WebSocket limitations

- More complex connection management
- Persistent connections consume resources
- Scaling requires additional considerations
- Not necessary for simple request-response operations

---

# Quick Revision: Must-Know Topics

For a **Java + Spring Boot Microservices interview**, prioritize these topics:

## Architecture

- Microservices vs Monolith
- Service boundaries
- DDD
- Bounded Context
- Database-per-Service
- API Gateway
- Service Discovery

## Communication

- REST
- OpenFeign
- gRPC
- Kafka
- RabbitMQ
- Synchronous vs Asynchronous communication

## Resilience

- Circuit Breaker
- Retry
- Timeout
- Fallback
- Bulkhead
- Resilience4j

## Distributed Data

- Saga Pattern
- Eventual Consistency
- Distributed Transactions
- Event Sourcing
- Idempotency

## Spring Cloud

- Eureka
- Spring Cloud Config
- Spring Cloud Gateway
- Spring Cloud LoadBalancer
- Resilience4j

## Observability

- Spring Boot Actuator
- Prometheus
- Grafana
- ELK
- OpenTelemetry
- Zipkin / Jaeger

## DevOps

- Docker
- Kubernetes
- CI/CD
- Blue-Green Deployment
- Canary Deployment
- Rolling Deployment

## Security

- Spring Security
- OAuth2
- JWT
- RBAC
- HTTPS/TLS
- API Gateway security
- Secret management

---

# Interviewer Follow-Up Questions to Prepare

After answering the basic questions above, be ready for practical follow-ups such as:

1. Why did you choose microservices instead of a monolith?
2. How do two Spring Boot services communicate using OpenFeign?
3. What happens when the downstream service is unavailable?
4. How would you implement Circuit Breaker using Resilience4j?
5. How does Eureka service discovery work internally?
6. How does an API Gateway route requests?
7. How would you secure microservices using JWT?
8. How do you handle authentication at the API Gateway?
9. How do you handle distributed transactions?
10. Explain Saga choreography vs orchestration.
11. How do you prevent duplicate Kafka message processing?
12. What is idempotency and how would you implement it?
13. How do you handle Kafka consumer failures?
14. How do you maintain data consistency across services?
15. Why should microservices avoid a shared database?
16. How do you trace one request across multiple services?
17. How do you monitor Spring Boot microservices in production?
18. How do you deploy microservices using Docker and Kubernetes?
19. How do you handle configuration for Dev, QA, and Production?
20. What happens if Eureka Server itself goes down?
21. What happens if the API Gateway goes down?
22. How would you design a highly available microservices system?
23. How would you migrate a monolith to microservices?
24. How would you decide service boundaries?
25. What are the disadvantages of microservices?

---

# One-Minute Microservices Architecture Revision

```text
                         CLIENT
                           |
                           v
                    +--------------+
                    | API GATEWAY  |
                    +------+-------+
                           |
          +----------------+----------------+
          |                |                |
          v                v                v
    +-----------+    +-----------+    +-----------+
    | User      |    | Order     |    | Payment   |
    | Service   |    | Service   |    | Service   |
    +-----+-----+    +-----+-----+    +-----+-----+
          |                |                |
         DB               DB               DB
                           |
                           v
                     +-----------+
                     |   Kafka   |
                     +-----+-----+
                           |
              +------------+------------+
              |                         |
              v                         v
       Notification                 Inventory
         Service                    Service

Supporting Infrastructure:
- Service Discovery
- Config Server
- Load Balancer
- Circuit Breaker / Resilience4j
- Monitoring
- Distributed Tracing
- Centralized Logging
- Docker / Kubernetes
- CI/CD
```

---

# Final Interview Tip

For experienced Java/Spring Boot interviews, do not stop at definitions.

For every important topic, prepare these four points:

1. **What is it?**
2. **Why do we need it?**
3. **How does it work internally?**
4. **How have you used it in a real project?**

For example, for **Circuit Breaker**:

> "Circuit Breaker prevents repeated calls to an unhealthy downstream service. It has Closed, Open, and Half-Open states. In Spring Boot, I can implement it using Resilience4j with configured failure thresholds, wait duration, timeout, and fallback behavior. In a real system, this prevents a downstream failure from causing cascading failures."

This structure makes answers much stronger in Java Microservices interviews.
