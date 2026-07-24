# Micro-service Java interview questions

## 1. What are Microservices?

**Answer:**

**Microservices** is an architectural style where an application is divided into small, independent services. Each service focuses on a specific business capability and can be developed, deployed, and scaled independently.

### Characteristics of Microservices

- Each service has its own business responsibility
- Services can be deployed independently
- Services can scale independently based on demand
- Services communicate with each other using APIs or messaging
- Each service can have its own database or data storage strategy
- Different services can use different technologies when required

### Example: E-commerce Application

An e-commerce system can be divided into multiple microservices:

```text
                  API Gateway
                       |
        --------------------------------
        |        |        |            |
        ▼        ▼        ▼            ▼
      User    Order   Payment    Inventory
    Service  Service  Service     Service
```

### Communication Between Services

Microservices commonly communicate using:

- REST APIs
- gRPC
- Message brokers (Kafka, RabbitMQ)
- Event-driven messaging

Example:

```text
Order Service
       │
       ▼
Payment Service
       │
       ▼
Inventory Service
```

### Advantages

- Independent deployment
- Better scalability
- Fault isolation
- Easier maintenance for large applications
- Teams can work independently on different services

### Challenges

- Distributed system complexity
- Network communication failures
- Data consistency challenges
- Service monitoring and debugging
- Deployment and infrastructure management

### Monolithic vs Microservices

| Monolithic | Microservices |
|------------|---------------|
| Single large application | Multiple small services |
| Single deployment unit | Independent deployments |
| Shared codebase | Separate codebases |
| Scaling affects entire application | Individual services can scale |
| Easier initially | Better for large systems |

### Summary

```text
Microservices = Small + Independent + Business-focused Services

```

Each service:
✓ Own responsibility
✓ Independent deployment
✓ Independent scaling

---

## 2. What are the advantages of Microservices?

**Answer:**

Microservices provide several benefits by breaking a large application into smaller, independent services.

### Benefits of Microservices

| Advantage | Description |
|-----------|-------------|
| **Independent Deployment** | Each service can be deployed, updated, or rolled back independently without affecting other services. |
| **Better Scalability** | Individual services can be scaled based on their specific load requirements. |
| **Technology Flexibility** | Each service can use different technologies, programming languages, or databases based on business needs. |
| **Fault Isolation** | Failure in one service does not necessarily bring down the entire application. |
| **Smaller Codebases** | Each service has a focused responsibility, making code easier to understand and maintain. |
| **Faster Development Cycles** | Smaller teams can develop, test, and release services faster. |
| **Easier Team Ownership** | Teams can own specific services and manage them independently. |


### Example

```text
Large Application

Before:

        E-commerce Application
                │
        Single Large Codebase


After:

        API Gateway
             │
 ┌───────────┼───────────┐
 ▼           ▼           ▼
User      Order      Payment
Service   Service    Service
```

### Scaling Example

If the order system receives more traffic:

```text
Before:

Scale Entire Application
        │
        ▼
Large Deployment


After:

Scale Only Order Service

User Service   → 2 Instances
Order Service  → 10 Instances
Payment Service → 3 Instances
```

### Summary

Microservices enable:

- Faster releases
- Independent scaling
- Better maintainability
- Improved team productivity
- More resilient applications

---

## 3. What are the challenges of Microservices?

**Answer:**

While microservices provide scalability and flexibility, they also introduce complexity because the application becomes a distributed system.

### Common Challenges

| Challenge | Description |
|-----------|-------------|
| **Network Failures** | Services communicate over networks, so latency, timeouts, and communication failures can occur. |
| **Distributed Transactions** | Maintaining data consistency across multiple services is complex because each service manages its own data. |
| **Service Discovery** | Services need a mechanism to find and communicate with each other dynamically. |
| **Monitoring Complexity** | Tracking requests across multiple services requires centralized logging, metrics, and tracing. |
| **Data Consistency** | Keeping data synchronized between services requires patterns like Saga and event-driven communication. |
| **Increased Deployment Complexity** | Managing multiple services requires automation, CI/CD pipelines, containers, and orchestration tools. |
| **Security Management** | Authentication, authorization, and secure communication must be handled across multiple services. |

### Example Challenges in an E-commerce System

```text
        API Gateway
             |
 ┌───────────┼───────────┐
 ▼           ▼           ▼
Order     Payment    Inventory
Service   Service    Service
  │          │           │
  └──────────┼───────────┘
             │
     Distributed Data
     + Network Calls
```

A failure in communication:

```text
Order Service
      │
      ▼
Payment Service
      │
      ✖ Timeout / Failure
```

The system needs proper handling using retries, timeouts, circuit breakers, and fallback mechanisms.

### Common Solutions

| Challenge | Solution |
|-----------|----------|
| Network failures | Timeout, Retry, Circuit Breaker |
| Service discovery | Eureka, Consul, Kubernetes Service Discovery |
| Monitoring | Centralized logs, Metrics, Distributed Tracing |
| Data consistency | Saga Pattern, Event-driven architecture |
| Deployment complexity | CI/CD, Docker, Kubernetes |
| Security | OAuth2, JWT, API Gateway security |

### Summary

Microservices provide flexibility and scalability, but require strong engineering practices for:

- Communication
- Monitoring
- Security
- Deployment
- Data management

---

## 4. Difference between Monolithic and Microservices Architecture

**Answer:**

Monolithic and Microservices are two different approaches to designing software applications.

### Comparison

| Feature | Monolithic Architecture | Microservices Architecture |
|---------|-------------------------|-----------------------------|
| Application Structure | Single large application | Multiple independent services |
| Deployment | Single deployment unit | Independent deployment of each service |
| Database | Usually shared database | Usually separate databases per service |
| Scaling | Entire application must be scaled | Individual services can be scaled independently |
| Coupling | Tightly coupled components | Loosely coupled services |
| Development | Single codebase | Multiple service codebases |
| Technology | Usually one technology stack | Different technologies can be used |
| Fault Isolation | Failure can affect the entire application | Failure can be isolated to a specific service |

### Monolithic Architecture

```text
             Application
                  |
 --------------------------------
 |       |        |             |
User   Order   Payment     Inventory
Module Module  Module      Module
                  |
             Shared Database
```

Characteristics:

- All features are inside one application
- One deployment process
- Components share the same codebase and database

---

### Microservices Architecture

```text
              API Gateway
                   |
 ---------------------------------
 |          |          |          |
User     Order     Payment   Inventory
Service  Service   Service    Service
 |          |          |          |
DB        DB         DB         DB
```

Characteristics:

- Each service has a specific responsibility
- Services are independently deployed
- Services can scale separately

---

### Scaling Example

**Monolithic:**

```text
High Order Traffic

Scale Entire Application
          │
          ▼
Increase All Modules
```

**Microservices:**

```text
High Order Traffic

Scale Only Order Service

User Service      → 2 Instances
Order Service     → 10 Instances
Payment Service   → 3 Instances
```

### Summary

```text
Monolithic
→ Simple initially, but harder to scale and maintain as the application grows.

Microservices
→ More flexible and scalable, but requires handling distributed system complexity.
```

---

## 5. How do microservices communicate with each other?

**Answer:**

Microservices communicate with each other using two common approaches:

1. **Synchronous Communication**
2. **Asynchronous Communication**

---

## 1. Synchronous Communication

In synchronous communication, one service directly calls another service and waits for a response.

Common technologies:

- REST API
- gRPC

### Example

```text
Order Service
       │
       │  REST API Call
       ▼
Payment Service
       │
       ▼
Payment Response
```

### Flow

```text
User Places Order
        │
        ▼
Order Service
        │
        ▼
Call Payment Service
        │
        ▼
Receive Payment Result
        │
        ▼
Complete Order
```

### Advantages

- Simple request-response model
- Immediate response
- Easy to implement

### Challenges

- Services become dependent on each other
- Network failures can affect requests
- Increased latency

---

## 2. Asynchronous Communication

In asynchronous communication, services communicate through message brokers. The sender publishes messages and does not wait for an immediate response.

Common message brokers:

- Kafka
- RabbitMQ
- ActiveMQ

### Example

```text
Order Service
       │
       ▼
    Kafka Topic
       │
       ▼
Inventory Service
```

### Flow

```text
Order Created
       │
       ▼
Publish Event
       │
       ▼
Kafka Message Broker
       │
       ▼
Inventory Service Consumes Event
```

### Advantages

- Loose coupling between services
- Better scalability
- Improved fault tolerance
- Services can process messages independently

### Challenges

- Event consistency handling
- Message ordering
- Debugging complexity

---

## Synchronous vs Asynchronous Communication

| Feature | Synchronous | Asynchronous |
|---------|-------------|--------------|
| Communication | Direct service call | Message-based |
| Response | Immediate | Later processing |
| Technologies | REST, gRPC | Kafka, RabbitMQ |
| Coupling | Higher | Lower |
| Availability | Depends on called service | More resilient |
| Best For | Real-time responses | Event-driven workflows |

### Summary

```text
Synchronous:
Service A ─────► Service B
        Request / Response


Asynchronous:
Service A ─────► Message Broker ─────► Service B
        Publish Event              Consume Event
```

---

## 6. What is REST API in Microservices?

**Answer:**

**REST (Representational State Transfer)** is a communication style used by microservices to exchange data over HTTP.

A REST API allows one service to communicate with another service using standard HTTP methods.

### Common HTTP Methods

| Method | Purpose | Example |
|--------|---------|---------|
| **GET** | Fetch data | Get user details |
| **POST** | Create new data | Create a user |
| **PUT** | Update existing data | Update user information |
| **DELETE** | Remove data | Delete a user |

### Example

Request:

```http
GET /api/users/101
```

Flow:

```text
Client
  │
  ▼
User Service
  │
  ▼
Fetch User Data
  │
  ▼
Return Response
```

Response:

```json
{
  "id": 101,
  "name": "John"
}
```

### REST Communication Between Microservices

Example:

```text
Order Service
      │
      │ HTTP REST Call
      ▼
Payment Service
      │
      ▼
Payment Response
```

### Advantages of REST APIs

- Simple and widely used
- Language independent
- Uses standard HTTP protocols
- Easy integration between services
- Works well with web and mobile applications

### Common REST API Practices

- Use meaningful URLs

```text
GET    /api/users/101
POST   /api/users
PUT    /api/users/101
DELETE /api/users/101
```

- Use proper HTTP status codes

| Status Code | Meaning |
|-------------|---------|
| 200 | Success |
| 201 | Created |
| 400 | Bad Request |
| 404 | Not Found |
| 500 | Server Error |

### Summary

```text
REST API

Microservice A
       │
       │ HTTP Request
       ▼
Microservice B
       │
       ▼
HTTP Response
```

REST APIs provide a simple way for microservices to communicate synchronously.

---

## 7. What is Spring Boot's role in Microservices?

**Answer:**

Spring Boot simplifies the development and deployment of microservices by providing built-in features for creating production-ready applications with minimal configuration.

### Features Provided by Spring Boot

| Feature | Description |
|---------|-------------|
| **Embedded Servers** | Provides built-in servers like Tomcat, allowing applications to run independently without external server deployment. |
| **Auto Configuration** | Automatically configures application components based on dependencies. |
| **REST Support** | Makes it easy to build REST APIs using annotations like `@RestController` and `@GetMapping`. |
| **Dependency Management** | Simplifies dependency handling using Spring Boot starters. |
| **Actuator Monitoring** | Provides production monitoring endpoints for health checks and metrics. |
| **Security Integration** | Supports authentication and authorization using Spring Security, JWT, OAuth2, etc. |

### Example: REST Controller

```java
@RestController
public class UserController {

    @GetMapping("/users")
    public List<User> getUsers() {
        return service.getUsers();
    }
}
```

### Microservice Flow

```text
Client Request
       │
       ▼
 Spring Boot Service
       │
       ├── REST Controller
       │
       ├── Business Service
       │
       └── Database
```

### Spring Boot in a Microservices Ecosystem

```text
                API Gateway
                    │
       ┌────────────┼────────────┐
       ▼            ▼            ▼
 User Service  Order Service  Payment Service
(Spring Boot) (Spring Boot)  (Spring Boot)
       │            │            │
       ▼            ▼            ▼
    Database     Database     Database
```

### Advantages

- Faster microservice development
- Less configuration
- Production-ready features
- Easy REST API creation
- Integration with cloud and distributed systems

### Summary

Spring Boot provides:

✓ Easy service creation
✓ REST API support
✓ Embedded deployment
✓ Monitoring
✓ Security
✓ Microservice-friendly configuration

---

## 8. What is Service Discovery?

**Answer:**

In a microservices architecture, service instances can change dynamically because services may be scaled up, scaled down, restarted, or moved to different servers.

**Service Discovery** is a mechanism that helps microservices find and communicate with each other without using hardcoded service URLs.

A service registers itself with a **Service Registry**, and other services use the registry to locate available service instances.

### Example

```text
             Eureka Server
          (Service Registry)
                  │
        ┌─────────┴─────────┐
        │                   │
        ▼                   ▼
 Order Service        Payment Service
```

### Flow

```text

Payment Service starts
│
▼
Registers with Eureka Server
│
▼
Order Service needs Payment Service
│
▼
Queries Eureka Server
│
▼
Gets Payment Service location
│
▼
Calls Payment Service
```

### Without Service Discovery

```text
Order Service
       │
       ▼
Hardcoded URL

http://192.168.1.10:8080/payment
```

**Problems:**

- Service IP addresses may change
- Manual configuration is required
- Difficult to manage multiple service instances

**With Service Discovery**

```text
Order Service
       │
       ▼
 Eureka Server
       │
       ▼
Find Payment Service Instance
       │
       ▼
Communicate with Payment Service
```

**Common Service Discovery Tools**

- Eureka → Netflix service registry commonly used with Spring Cloud
- Consul → Service discovery and configuration management tool
- Kubernetes Service Discovery → Built-in discovery mechanism using Kubernetes Services

**Benefits**

- Removes hardcoded service URLs
- Supports dynamic service registration
- Enables load balancing between service instances
- Helps scale microservices easily
- Improves reliability in distributed systems

**Summary**

Service Discovery

```text
Service A
    │
    ▼
Service Registry
    │
    ▼
Find Service B
    │
    ▼
Communicate with Service B
```

---

## 9. What is Eureka Server?

**Answer:**

**Eureka Server** is a service registry provided by **Netflix OSS** that is commonly used in Spring Cloud microservices architectures.

It acts as a central registry where microservices register themselves and where other services can discover available service instances.

### Example

```text
              Eureka Server
           (Service Registry)
                  │
                  │
        ┌─────────┴─────────┐
        │                   │
        ▼                   ▼
 Payment-Service       Order-Service
```

### How It Works

1. A microservice starts up.
2. The service registers itself with Eureka Server.
3. Eureka stores service information such as:
   - Service name
   - IP address
   - Port number
   - Availability status
4. Other services query Eureka to find and communicate with it.

### Registration Flow

```text
Payment Service
       │
       ▼
Register Service Details
       │
       ▼
Eureka Registry
       │
       ▼
Available for Other Services
```

### Service Discovery Flow

```text
Order Service
       │
       ▼
Query Eureka Server
       │
       ▼
Find Payment-Service Location
       │
       ▼
Call Payment Service
```

### Example Service Registry

```text
Eureka Server

Registered Services:

PAYMENT-SERVICE
    Host: 192.168.1.10
    Port: 8081

ORDER-SERVICE
    Host: 192.168.1.11
    Port: 8082
```

### Advantages

- Removes hardcoded service URLs
- Supports dynamic service registration
- Enables service discovery
- Works with client-side load balancing
- Helps manage multiple service instances

### Summary

```text
Eureka Server

Microservice A
      │
      ▼
Register / Discover
      │
      ▼
Eureka Registry
      │
      ▼
Microservice B
```

Eureka acts as a **phone directory for microservices**, allowing services to find each other dynamically.

---

## 10. What is Eureka Client?

**Answer:**

A **Eureka Client** is a microservice application that registers itself with the **Eureka Server** and uses it to discover other available services.

Each microservice that wants to participate in service discovery acts as a Eureka Client.

### Example Configuration

`application.properties`

```properties
spring.application.name=payment-service
```

When the application starts, it automatically registers itself with the Eureka Server using this service name.

### Registration Flow

```text
Payment Service
       │
       ▼
 Eureka Client
       │
       ▼
Register Service Details
       │
       ▼
 Eureka Server
```

### Example

```text
Eureka Server

Registered Services:

PAYMENT-SERVICE
ORDER-SERVICE
USER-SERVICE
```

The `payment-service` application registers:

```text
Service Name:
payment-service

Status:
UP

Instance:
192.168.1.10:8081
```

### Eureka Client Responsibilities

- Registers the service with Eureka Server
- Sends heartbeat signals to indicate availability
- Fetches information about other services
- Helps services communicate dynamically

### Service Communication Example

```text
Order Service (Eureka Client)
          │
          ▼
   Query Eureka Server
          │
          ▼
 Find Payment Service
          │
          ▼
 Call Payment Service
```

### Summary

```text
Eureka Server
      ▲
      │
      │ Register / Discover
      │
Eureka Client
      │
      ▼
Microservice Application
```

**Eureka Client = A microservice that registers with Eureka Server and participates in service discovery.**

---

## 11. What is API Gateway?

**Answer:**

An **API Gateway** is a single entry point for clients to access multiple microservices.

Instead of clients directly calling individual services, all requests go through the API Gateway. The gateway routes requests to the appropriate microservice and handles common cross-cutting concerns.

### Architecture

```text
             Client
                │
                ▼
          API Gateway
                │
      ┌─────────┼─────────┐
      ▼         ▼         ▼
    User      Order    Payment
   Service   Service   Service
```

### Responsibilities of API Gateway

| Responsibility | Description |
|----------------|-------------|
| **Routing** | Routes client requests to the correct microservice |
| **Authentication** | Validates user identity using mechanisms like JWT or OAuth2 |
| **Rate Limiting** | Controls the number of requests to protect services |
| **Logging** | Collects request and response logs |
| **Request Transformation** | Modifies requests or responses when required |
| **Load Balancing** | Distributes requests across service instances |

### Example Flow

```text
Client Request

GET /api/orders/101

        │
        ▼

    API Gateway

        │
        ▼

   Order Service

        │
        ▼

   Response to Client
```

### Without API Gateway

```text
Client
  │
  ├──► User Service
  │
  ├──► Order Service
  │
  └──► Payment Service
```

Problems:

- Client needs to know all service locations
- Multiple authentication implementations
- More network calls

### With API Gateway

```text
Client
  │
  ▼
API Gateway
  │
  ├──► User Service
  ├──► Order Service
  └──► Payment Service
```

Benefits:

- Simplifies client communication
- Centralizes security
- Reduces client complexity
- Provides better monitoring and control

### Popular API Gateway Tools

- **Spring Cloud Gateway**
- **Kong**
- **Nginx**

### Summary

```text
API Gateway

Client
  │
  ▼
Single Entry Point
  │
  ▼
Multiple Microservices
```

API Gateway acts as a **front door for microservices**, handling common tasks before requests reach backend services.

---

## 12. Why do we need API Gateway?

**Answer:**

An **API Gateway** is needed in microservices architecture to provide a single entry point for clients and manage communication between clients and backend services.

Without an API Gateway, clients need to know about and communicate directly with multiple microservices.

### Without API Gateway

```text
             Mobile App
                 │
      ┌──────────┼──────────┐
      ▼          ▼          ▼
    User      Order      Payment
   Service   Service    Service
```

Problems:

- Client must know all service URLs
- More complex client-side logic
- Security needs to be implemented in every service
- Difficult to manage multiple API calls

---

### With API Gateway

```text
             Mobile App
                 │
                 ▼
            API Gateway
                 │
      ┌──────────┼──────────┐
      ▼          ▼          ▼
    User      Order      Payment
   Service   Service    Service
```

The client communicates only with the API Gateway, and the gateway forwards requests to the appropriate services.

---

### Benefits of API Gateway

| Benefit | Description |
|---------|-------------|
| **Hides Internal Services** | Clients do not need to know the location or details of backend services. |
| **Central Security** | Authentication and authorization can be handled in one place. |
| **Simplifies Client Communication** | Clients interact with a single endpoint instead of multiple services. |
| **Request Routing** | Routes requests to the correct microservice. |
| **Rate Limiting** | Protects services from excessive traffic. |
| **Logging and Monitoring** | Centralizes request tracking and analytics. |

### Example Flow

```text
Client
  │
  ▼
API Gateway
  │
  ├── /users     → User Service
  │
  ├── /orders    → Order Service
  │
  └── /payments  → Payment Service
```

### Summary

```text
Without API Gateway:
Client → Multiple Microservices

With API Gateway:
Client → API Gateway → Multiple Microservices
```

API Gateway acts as a **single, secure entry point** that simplifies communication and improves control in a microservices system.

---

## 13. What is Circuit Breaker?

**Answer:**

A **Circuit Breaker** is a design pattern used in microservices to prevent **cascading failures** when one service becomes unavailable.

It monitors service calls and temporarily stops requests to a failing service instead of continuously sending requests that will fail.

### Example

```text
Order Service
       │
       ▼
Payment Service
       │
       ✖ Down / Not Available
```

Without a circuit breaker:

```text
Order Service
       │
       ▼
Payment Service (Failed)
       │
       ▼
Repeated Failed Requests
       │
       ▼
System Overload
```

With a circuit breaker:

```text
Order Service
       │
       ▼
 Circuit Breaker
       │
       ▼
Payment Service (Down)

Circuit OPEN

       │
       ▼
Fallback Response
```

### Circuit Breaker States

| State | Description |
|-------|-------------|
| **CLOSED** | Requests flow normally to the service. |
| **OPEN** | Requests are blocked because the service is failing. |
| **HALF-OPEN** | Allows limited requests to check if the service has recovered. |

### State Flow

```text
        Success
           │
           ▼
       CLOSED
           │
     Failures exceed limit
           │
           ▼
        OPEN
           │
   Wait for recovery time
           │
           ▼
     HALF-OPEN
           │
    ┌──────┴──────┐
    ▼             ▼
 Success       Failure
    │             │
    ▼             ▼
 CLOSED        OPEN
```

### Example

```text
Order Service
       │
       ▼
Payment Service

Payment Service unavailable

       │
       ▼

Circuit Breaker Opens

       │
       ▼

Return:
"Payment service temporarily unavailable"
```

### Benefits

- Prevents cascading failures
- Improves system resilience
- Reduces unnecessary network calls
- Provides fallback responses
- Helps services recover gracefully

### Tools

- **Resilience4j** → Recommended modern Java circuit breaker library
- **Hystrix** → Netflix circuit breaker library (deprecated)

### Summary

```text
Circuit Breaker

Service Failure
       │
       ▼
Stop Repeated Calls
       │
       ▼
Return Fallback Response
       │
       ▼
Allow Recovery Later
```

---

## 14. Explain Circuit Breaker States

**Answer:**

A **Circuit Breaker** works with three main states:

1. **Closed**
2. **Open**
3. **Half Open**

These states help the system handle service failures and recover gracefully.

---

## 1. Closed State

**Normal operation.**

In the Closed state, requests are allowed to flow normally from one service to another.

```text
Request
   │
   ▼
Circuit Breaker
   │
   ▼
Service
```

Example:

```text
Order Service
       │
       ▼
Payment Service
       │
       ▼
Success Response
```

If failures remain below the configured threshold, the circuit stays closed.

---

## 2. Open State

**Service failure detected.**

When the number of failures exceeds the configured limit, the circuit breaker opens.

Requests are blocked and are not sent to the failed service.

```text
Request
   │
   ▼
Circuit Breaker
   │
   ▼
Fallback Response
```

Example:

```text
Order Service
       │
       ▼
Circuit OPEN
       │
       ▼
Payment Service (Not Called)

Return:
"Payment service unavailable"
```

Purpose:

- Prevent repeated failed calls
- Protect the system from cascading failures
- Reduce load on the failing service

---

## 3. Half Open State

**Tests whether the service has recovered.**

After a timeout period, the circuit breaker allows a limited number of requests to check if the service is available again.

```text
Request
   │
   ▼
Circuit Breaker
   │
   ▼
Service
```

Results:

```text
Success
   │
   ▼
Move to CLOSED


Failure
   │
   ▼
Move back to OPEN
```

---

## Circuit Breaker State Flow

```text
              Failures
                 │
                 ▼
            ┌────────┐
            │ CLOSED │
            └────────┘
                 │
                 ▼
          Failure Threshold Reached
                 │
                 ▼
            ┌────────┐
            │  OPEN  │
            └────────┘
                 │
          Recovery Timeout
                 │
                 ▼
         ┌────────────┐
         │ HALF OPEN  │
         └────────────┘
              │
       ┌──────┴──────┐
       ▼             ▼
   Success       Failure
       │             │
       ▼             ▼
    CLOSED        OPEN
```

### Summary

| State | Meaning | Behavior |
|-------|---------|----------|
| **Closed** | Service is healthy | Requests pass normally |
| **Open** | Service is failing | Requests blocked, fallback returned |
| **Half Open** | Testing recovery | Limited requests allowed |

Circuit Breaker helps microservices remain stable even when dependent services fail.

---

## 15. What is Resilience4j?

**Answer:**

**Resilience4j** is a lightweight fault tolerance library designed for Java applications, especially microservices built with Spring Boot.

It helps applications handle failures gracefully by providing patterns that improve reliability and resilience.

### Features of Resilience4j

| Feature | Description |
|---------|-------------|
| **Circuit Breaker** | Prevents repeated calls to failing services and provides fallback responses. |
| **Retry** | Automatically retries failed operations based on configured rules. |
| **Rate Limiter** | Controls the number of requests allowed within a time period. |
| **Bulkhead** | Limits concurrent calls to prevent one service failure from affecting the entire system. |
| **Time Limiter** | Controls timeout duration for service calls. |

---

## Example: Circuit Breaker with Resilience4j

```java
@CircuitBreaker(
    name = "paymentService",
    fallbackMethod = "fallback"
)
public Payment pay() {
    return paymentClient.process();
}
```

### Fallback Method

```java
public Payment fallback(Exception e) {
    return new Payment("Payment service unavailable");
}
```

### Flow

```text
Order Service
       │
       ▼
 Resilience4j Circuit Breaker
       │
       ▼
 Payment Service
       │
       ├── Success → Return Payment Response
       │
       └── Failure → Execute Fallback
```

---

## Retry Example

If a service temporarily fails:

```text
Request
   │
   ▼
Payment Service
   │
   ✖ Failure
   │
   ▼
Retry Request
```

---

## Rate Limiter Example

Controls excessive traffic:

```text
1000 Requests/sec

        │
        ▼

Rate Limiter

        │
        ▼

Allow only configured requests
```

---

## Bulkhead Example

Prevents one service from consuming all resources:

```text
Payment Calls
      │
      ▼
Bulkhead Limit

Allowed: 50 concurrent requests

Extra Requests → Rejected/Fallback
```

---

## Benefits of Resilience4j

- Prevents cascading failures
- Improves application availability
- Provides fallback mechanisms
- Handles temporary service failures
- Improves fault tolerance in distributed systems

### Summary

```text
Resilience4j

        ┌─────────────────┐
        │ Circuit Breaker │
        ├─────────────────┤
        │ Retry           │
        ├─────────────────┤
        │ Rate Limiter    │
        ├─────────────────┤
        │ Bulkhead        │
        ├─────────────────┤
        │ Time Limiter    │
        └─────────────────┘

        Improves Microservice Reliability
```

---

## 16. What is Feign Client?

**Answer:**

**Feign Client** is a declarative REST client used in Spring Boot microservices for making service-to-service HTTP calls.

It simplifies communication between microservices by allowing developers to call another service using a Java interface instead of writing manual REST client code.

### Dependency

Add OpenFeign dependency:

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

Enable Feign Clients:

```java
@SpringBootApplication
@EnableFeignClients
public class Application {
}
```

---

## Example

### Payment Service Client

```java
@FeignClient(name = "payment-service")
public interface PaymentClient {

    @GetMapping("/payment/{id}")
    Payment getPayment(
        @PathVariable Long id
    );
}
```

The `@FeignClient` name matches the registered service name in Eureka or another service registry.

---

## How Feign Client Works

```text
Order Service
       │
       ▼
 PaymentClient Interface
       │
       ▼
 Feign Client
       │
       ▼
 Payment Service
       │
       ▼
 Response
```

Feign automatically:

- Creates HTTP requests
- Handles service communication
- Converts responses into Java objects
- Integrates with service discovery

---

## Without Feign Client

Manual REST call:

```java
RestTemplate restTemplate = new RestTemplate();

Payment payment =
    restTemplate.getForObject(
        "http://payment-service/payment/1",
        Payment.class
    );
```

Problems:

- More boilerplate code
- URL management required
- Harder to maintain

---

## With Feign Client

```java
Payment payment = paymentClient.getPayment(1L);
```

Simple interface-based communication.

---

## Benefits of Feign Client

- Reduces REST client code
- Easy service-to-service communication
- Integrates with Eureka and Load Balancing
- Supports declarative programming style
- Improves code readability

---

## Feign Client with Service Discovery

```text
Order Service
      │
      ▼
Feign Client
      │
      ▼
Eureka Server
      │
      ▼
Find Payment Service Instance
      │
      ▼
Payment Service
```

### Summary

```text
Feign Client

Java Interface
      │
      ▼
HTTP REST Call
      │
      ▼
Another Microservice
```

Feign Client makes microservice communication easier by converting REST calls into simple Java method calls.

---

## 17. RestTemplate vs WebClient vs Feign

**Answer:**

**RestTemplate**, **WebClient**, and **Feign Client** are different approaches for making HTTP calls between services in Spring applications.

### Comparison

| Feature | RestTemplate | WebClient | Feign |
|---------|--------------|-----------|-------|
| **Style** | Imperative | Reactive | Declarative |
| **Async Support** | No | Yes | No (supports synchronous calls by default) |
| **Syntax** | Medium | Medium | Easy |
| **Spring Cloud Integration** | Limited | Good | Excellent |
| **Programming Model** | Template-based | Non-blocking reactive | Interface-based |
| **Best Use Case** | Simple REST calls | High-performance reactive applications | Microservice-to-microservice communication |

---

## 1. RestTemplate

**RestTemplate** is a traditional synchronous REST client.

Example:

```java
RestTemplate restTemplate = new RestTemplate();

Payment payment =
    restTemplate.getForObject(
        "http://payment-service/payment/1",
        Payment.class
    );
```

Characteristics:

- Blocking call
- Simple to use
- Older Spring approach
- Suitable for basic REST communication

---

## 2. WebClient

**WebClient** is a modern reactive HTTP client provided by Spring WebFlux.

Example:

```java
WebClient webClient = WebClient.create();

Mono<Payment> payment =
    webClient.get()
        .uri("/payment/1")
        .retrieve()
        .bodyToMono(Payment.class);
```

Characteristics:

- Non-blocking
- Supports asynchronous communication
- Handles high concurrent requests efficiently
- Suitable for reactive applications

---

## 3. Feign Client

**Feign Client** is a declarative REST client designed for microservice communication.

Example:

```java
@FeignClient(name = "payment-service")
public interface PaymentClient {

    @GetMapping("/payment/{id}")
    Payment getPayment(
        @PathVariable Long id
    );
}
```

Usage:

```java
Payment payment =
    paymentClient.getPayment(1L);
```

Characteristics:

- Simple interface-based approach
- Less boilerplate code
- Works well with Spring Cloud and service discovery

---

## When to Use Which?

| Scenario | Recommended |
|----------|-------------|
| Simple synchronous REST calls | RestTemplate |
| High-performance reactive applications | WebClient |
| Microservices communication | Feign Client |
| Service discovery integration | Feign Client |
| Streaming or asynchronous APIs | WebClient |

---

## Summary

```text
RestTemplate
→ Traditional blocking REST client


WebClient
→ Modern non-blocking reactive client


Feign Client
→ Simple declarative client for microservices
```

For Spring Cloud microservices, **Feign Client is commonly preferred** because it provides clean service-to-service communication with minimal code.

---

## 18. What is Distributed Transaction?

**Answer:**

A **Distributed Transaction** is a transaction that involves multiple services or databases in a distributed system.

In a microservices architecture, each service usually manages its own database. When a business operation requires multiple services to complete successfully, maintaining data consistency becomes a challenge.

### Example: Order Creation

```text
        Order Service
              |
              ▼
       Payment Service
              |
              ▼
      Inventory Service
```

### Transaction Flow

```text

Create Order
│
▼
Process Payment
│
▼
Update Inventory
```

#### Problem Scenario

What happens if payment succeeds but inventory update fails?

```text
Order Service
      │
      ▼
Payment Service
      │
      ▼
Payment Successful ✓
      │
      ▼
Inventory Service
      │
      ✖ Failure
```

**Result:**

```text
Payment Status  → Success
Order Status    → Created
Inventory       → Not Updated
```

The system is now in an inconsistent state.

Solutions
### 1. Saga Pattern
The Saga Pattern breaks a distributed transaction into multiple local transactions.

Each service completes its own transaction and communicates using events.

**Example:**

```text
Order Service
      │
      ▼
Order Created Event
      │
      ▼
Payment Service
      │
      ▼
Payment Completed Event
      │
      ▼
Inventory Service
```

If any step fails, a compensating transaction is executed.

### 2. Event-Driven Transactions
Services communicate using events through message brokers.

**Examples:**

- Kafka
- RabbitMQ
- ActiveMQ

Flow:

```text
Order Service
      │
      ▼
 Publish Event
      │
      ▼
 Message Broker
      │
      ▼
Payment / Inventory Service
```

**Benefits:**

- Loose coupling
- Better scalability
- Improved fault tolerance

### 3.Compensating Actions
A compensating action reverses a previous successful operation when a later operation fails.

**Example:**

```text
Payment Completed
        │
        ▼
Inventory Update Failed
        │
        ▼
Refund Payment
        │
        ▼
Cancel Order
```

**Summary**

```text
Distributed Transaction

Multiple Services
        │
        ▼
Multiple Local Transactions
        │
        ▼
Maintain Data Consistency
```

In microservices, distributed transactions are commonly handled using:

- Saga Pattern
- Event-driven architecture
- Compensating actions

---

## 19. Explain Saga Pattern

**Answer:**

The **Saga Pattern** is a design pattern used to manage **distributed transactions** in microservices.

Instead of using one large transaction across multiple services, Saga breaks the transaction into a sequence of **smaller local transactions**. Each service completes its own transaction and communicates with other services using events or commands.

If any step fails, **compensating actions** are executed to undo the previous successful operations.


### Example: Order Processing

Normal Flow:

```text
Create Order
      │
      ▼
Pay Money
      │
      ▼
Reserve Product
```

Each step is handled by a different service:

```text
Order Service
      │
      ▼
Payment Service
      │
      ▼
Inventory Service
```


### Failure Scenario

If inventory reservation fails after payment is completed:

```text
Create Order       ✓
      │
      ▼
Pay Money          ✓
      │
      ▼
Reserve Product    ✖ Failed
```

Compensating actions:

```text
Refund Payment
      │
      ▼
Cancel Order
```

---

### Types of Saga Pattern

#### 1. Choreography Saga

In **Choreography Saga**, services communicate through events without a central controller.

Each service listens for events and performs its own action.

Example:

```text
Order Service
      │
      ▼
Order Created Event
      │
      ▼
Payment Service
      │
      ▼
Payment Completed Event
      │
      ▼
Inventory Service
```

#### Advantages

- Simple for small workflows
- Loose coupling
- No central coordinator

#### Disadvantages

- Difficult to manage complex workflows
- Harder to track the overall transaction flow


### 2. Orchestration Saga

In **Orchestration Saga**, a central component called an **Orchestrator** controls the workflow and tells each service what action to perform.

Example:

```text
              Saga Orchestrator
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼
 Order Service  Payment    Inventory
                Service     Service
```

Flow:

```text
Orchestrator
      │
      ▼
Create Order
      │
      ▼
Process Payment
      │
      ▼
Reserve Inventory
```

If something fails:

```text
Orchestrator
      │
      ▼
Refund Payment
      │
      ▼
Cancel Order
```

#### Advantages

- Easier to understand and monitor
- Central control of workflow
- Better for complex business processes

#### Disadvantages

- Orchestrator can become complex
- More coupling to the coordinator


### Choreography vs Orchestration

| Feature | Choreography | Orchestration |
|---------|--------------|---------------|
| Control | Decentralized | Centralized |
| Communication | Events | Commands |
| Coordinator | No | Yes |
| Complexity | Simple workflows | Complex workflows |
| Coupling | Lower | Higher |


#### Summary

```text
Saga Pattern

Distributed Transaction
          │
          ▼
Multiple Local Transactions
          │
          ▼
Compensating Actions on Failure
```

Saga helps microservices maintain data consistency without using traditional distributed database transactions.

---

## 20. What is Kafka's role in Microservices?

**Answer:**

**Apache Kafka** is a distributed event streaming platform used in microservices for **asynchronous communication** between services.

Instead of services calling each other directly, one service publishes an event to Kafka, and other services consume that event independently.

#### Example

```text
             Order Service

                  │
                  ▼

        Order Created Event

                  │
                  ▼

             Kafka Topic

                  │
        ┌─────────┴─────────┐
        ▼                   ▼
 Inventory Service     Email Service
```

#### How Kafka Works

1. A service produces an event.
2. Kafka stores the event in a topic.
3. Other services consume the event.
4. Each service processes the event independently.

#### Example Flow

```text
Customer Places Order

        │
        ▼

Order Service

        │

Publish:
"Order Created"

        │
        ▼

Kafka Topic: orders

        │
   ┌────┴────┐
   ▼         ▼

Inventory   Notification
Service     Service
```


### Benefits of Kafka in Microservices

| Benefit | Description |
|---------|-------------|
| **Loose Coupling** | Services do not directly depend on each other. |
| **High Throughput** | Kafka can handle millions of messages efficiently. |
| **Event-Driven Architecture** | Services react to events instead of direct calls. |
| **Scalability** | Consumers can be scaled independently. |
| **Fault Tolerance** | Messages can be stored and processed reliably. |


### Kafka vs REST Communication

#### REST Communication

```text
Order Service
       │
       ▼
Payment Service
       │
       ▼
Response
```

- Synchronous
- Sender waits for response
- Tighter coupling

---

#### Kafka Communication

```text
Order Service
       │
       ▼
Kafka Topic
       │
 ┌─────┴─────┐
 ▼           ▼
Payment   Inventory
Service   Service
```

- Asynchronous
- Services work independently
- Better scalability

#### Common Microservices Use Cases

- Order processing
- Payment events
- Notification systems
- Audit logging
- Data synchronization
- Event-driven workflows

#### Summary

```text
Kafka in Microservices

Service A
    │
    ▼
 Publish Event
    │
    ▼
 Kafka Topic
    │
    ▼
 Multiple Services Consume Event
```

Kafka enables **event-driven microservices architecture** by providing reliable, scalable, and asynchronous communication between services.

---

## 21. What is Event Driven Architecture?

**Answer:**

**Event Driven Architecture (EDA)** is a software architecture style where services communicate with each other by producing and consuming **events**.

Instead of directly calling another service, a service publishes an event when something happens, and other services react to that event.

#### Example

When an order is created:

```text
Order Service

      │
      ▼

Publish Event

{
  "event": "ORDER_CREATED",
  "orderId": 123
}

      │
      ▼

Message Broker (Kafka)

      │
 ┌────┴────┐
 ▼         ▼

Inventory  Notification
Service    Service
```

### How Event Driven Architecture Works

1. A service performs an action.
2. The service publishes an event.
3. The event is stored in a message broker.
4. Other services consume and process the event.

#### Flow

```text
User Places Order

        │
        ▼

Order Service

        │

Publish ORDER_CREATED Event

        │
        ▼

Kafka Topic

        │
 ┌──────┼──────┐
 ▼             ▼
Inventory   Email
Service     Service
```

#### Example Event

```json
{
  "event": "ORDER_CREATED",
  "orderId": 123,
  "customerId": 456,
  "amount": 250
}
```

Consumers can react:

```text
Inventory Service
→ Reserve Product


Email Service
→ Send Order Confirmation


Payment Service
→ Process Payment
```


### Benefits of Event Driven Architecture

| Benefit | Description |
|---------|-------------|
| **Loose Coupling** | Services do not directly depend on each other. |
| **Scalability** | Services can process events independently. |
| **High Availability** | Temporary service failures do not block the entire workflow. |
| **Better Extensibility** | New consumers can subscribe to existing events easily. |
| **Asynchronous Processing** | Services do not wait for immediate responses. |


### Event Driven vs REST Communication

#### REST

```text
Order Service
       │
       ▼
Payment Service
       │
       ▼
Response
```

- Direct communication
- Synchronous
- Service dependency is higher


#### Event Driven

```text
Order Service
       │
       ▼
Kafka Event
       │
 ┌─────┴─────┐
 ▼           ▼
Payment   Inventory
Service   Service
```

- Asynchronous communication
- Loose coupling
- Better scalability


### Common Technologies

- Apache Kafka
- RabbitMQ
- ActiveMQ
- AWS SNS/SQS

#### Summary

```text
Event Driven Architecture

Producer Service
        │
        ▼
      Event
        │
        ▼
 Message Broker
        │
        ▼
Consumer Services
```

Event Driven Architecture allows microservices to communicate through events, making systems more scalable, flexible, and loosely coupled.

---

## 22. How do you secure Microservices?

**Answer:**

Security in microservices is important because multiple independent services communicate over networks. A secure microservices architecture uses authentication, authorization, encryption, and centralized security controls.

#### Common Security Approaches

| Approach | Description |
|----------|-------------|
| **OAuth2** | Provides secure authorization between clients and services. |
| **JWT Authentication** | Uses JSON Web Tokens to securely transfer user identity information. |
| **API Gateway Security** | Centralizes authentication and security checks before requests reach services. |
| **Role-Based Authorization** | Controls access based on user roles and permissions. |
| **HTTPS Communication** | Encrypts data transferred between clients and services. |


#### Authentication Flow

```text
Client

   │
   │ JWT Token
   ▼

API Gateway

   │
   │ Validate Token
   ▼

Microservices
```

#### Example Flow

```text

User logs in
    │
    ▼

Authentication Service generates JWT Token
    │
    ▼

Client sends JWT with requests
    │
    ▼

API Gateway validates token
    │
    ▼

Request forwarded to Microservice
```

**Security Architecture**

```text
                 Client
                    │
                    ▼
              API Gateway
                    │
        Authentication + Authorization
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼
     User       Order       Payment
    Service    Service      Service
```

**Additional Security Best Practices**
- Use HTTPS for all communication
- Validate and expire JWT tokens
- Store secrets securely
- Apply least privilege access
- Use service-to-service authentication
- Monitor security logs
- Implement rate limiting

**Summary**

Microservices Security

```text
Client
  │
  ▼
JWT / OAuth2 Authentication
  │
  ▼
API Gateway Security
  │
  ▼
Authorized Microservices
```

A secure microservices system combines authentication, authorization, encrypted communication, and centralized security controls.

---

## 23. What is JWT?

**Answer:**

**JWT (JSON Web Token)** is a compact token format used for securely transmitting information between parties, commonly used for **authentication and authorization** in microservices.

After successful login, the server generates a JWT token and sends it to the client. The client sends this token with every request, allowing services to verify the user's identity.


### JWT Structure

A JWT contains three parts:

```text
JWT Token

Header.Payload.Signature
```

#### 1. Header

Contains information about the token type and signing algorithm.

Example:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

#### 2. Payload

Contains user-related information (claims).

Example:

```json
{
  "user": "john",
  "role": "ADMIN"
}
```

Common claims:

- User ID
- Username
- Roles
- Token expiration time


#### 3. Signature

Used to verify that the token has not been modified.

```text
Signature =
Hash(
 Header + Payload + Secret Key
)
```

#### JWT Authentication Flow

```text
Client
  │
  ▼
Login Request
  │
  ▼
Authentication Service
  │
  ▼
Generate JWT Token
  │
  ▼
Return Token to Client
```

For future requests:

```text
Client

   │
   │ JWT Token
   ▼

API Gateway

   │
   │ Validate Token
   ▼

Microservices
```

#### Example Request

```http
GET /api/orders

Authorization: Bearer eyJhbGciOiJIUzI1...
```

The server validates the token before allowing access.


#### Advantages of JWT

- Stateless authentication
- No need to store sessions on the server
- Easy integration with microservices
- Supports role-based authorization
- Compact and easy to transmit


#### JWT in Microservices

```text
Client
  │
  ▼
JWT Token
  │
  ▼
API Gateway
  │
  ▼
User Service / Order Service / Payment Service
```

#### Summary

```text
JWT

Header
   +
Payload
   +
Signature

= Secure Authentication Token
```

JWT is commonly used in microservices to provide secure, stateless authentication and authorization.

---

## 24. How do you handle logging in Microservices?

**Answer:**

Logging in microservices is challenging because an application consists of multiple independent services running across different servers or containers.

To solve this, microservices commonly use **centralized logging**, where logs from all services are collected and stored in a single logging system.

#### Architecture

```text
        Service A
            │
            │
        Service B
            │
            │
        Service C
            │
            ▼
 Central Logging System
```

#### Centralized Logging Flow

```text
User Request

      │
      ▼

API Gateway

      │
 ┌────┼────┐
 ▼    ▼    ▼

Service A  Service B  Service C

      │
      ▼

Log Collector

      │
      ▼

Central Log Storage
```


### Popular Logging Tools

| Tool | Purpose |
|------|---------|
| **ELK Stack** | Elasticsearch + Logstash + Kibana for collecting, storing, and visualizing logs |
| **Splunk** | Enterprise log monitoring and analysis platform |
| **Grafana Loki** | Lightweight log aggregation system integrated with Grafana |


#### What Should Be Logged?

Common information:

- Request ID / Correlation ID
- Service name
- Timestamp
- Error details
- API endpoint
- Response status
- User activity
- Performance metrics

Example log:

```text
2026-01-01 10:30:15
Service: Order-Service
RequestId: abc123
Action: Create Order
Status: SUCCESS
```


#### Correlation ID

A **Correlation ID** helps track a single request across multiple services.

Example:

```text
Client Request
      │
      ▼
API Gateway
Request ID: 12345
      │
      ▼
Order Service
Request ID: 12345
      │
      ▼
Payment Service
Request ID: 12345
```

This makes debugging distributed systems easier.

#### Benefits of Centralized Logging

- View logs from all services in one place
- Faster debugging
- Track requests across services
- Monitor application health
- Detect failures quickly


#### Summary

```text
Microservices

Service A ─┐
Service B ─┼──► Central Logging System
Service C ─┘

          ▼

   Search + Monitor + Analyze Logs
```

Centralized logging helps teams monitor, troubleshoot, and maintain large-scale microservices applications efficiently.

---

## 25. What is Distributed Tracing?

**Answer:**

**Distributed Tracing** is a technique used in microservices to track a single request as it travels across multiple services.

Since a microservices application contains many independent services, tracing helps identify where delays or failures occur during request processing.

#### Example

```text
User Request

      │
      ▼

 API Gateway

      │
      ▼

 Order Service

      │
      ▼

 Payment Service

      │
      ▼

 Database
```

A distributed tracing system records each step of this request flow.


#### How Distributed Tracing Works

Each request is assigned a unique **Trace ID**.

Example:

```text
Trace ID: abc-123

API Gateway
      │
      ▼
Order Service
      │
      ▼
Payment Service
      │
      ▼
Database
```

Each service creates a **Span** representing its part of the request.

```text
Trace

├── Gateway Span
├── Order Service Span
├── Payment Service Span
└── Database Span
```

#### Example Scenario

A user places an order:

```text
Request
   │
   ▼
Gateway
   │  20 ms
   ▼
Order Service
   │  50 ms
   ▼
Payment Service
   │  5000 ms
   ▼
Database
```

Tracing helps identify that the Payment Service caused the delay.


#### Distributed Tracing Tools

| Tool | Description |
|------|-------------|
| **Zipkin** | Distributed tracing system used to collect and visualize request traces |
| **Jaeger** | Open-source distributed tracing platform |
| **OpenTelemetry** | Standard framework for collecting traces, metrics, and logs |


#### Benefits of Distributed Tracing

- Finds performance bottlenecks
- Tracks requests across services
- Helps debug failures
- Measures service latency
- Improves system monitoring


#### Logging vs Distributed Tracing

| Logging | Distributed Tracing |
|---------|---------------------|
| Records application events | Tracks request flow across services |
| Service-level information | End-to-end request journey |
| Used for debugging errors | Used for finding bottlenecks |

---

#### Summary

```text
Distributed Tracing

User Request
      │
      ▼
 Service A
      │
      ▼
 Service B
      │
      ▼
 Service C

      ↓

Trace ID + Spans

      ↓

Understand Complete Request Flow
```

Distributed tracing is essential in microservices because it provides visibility into how requests move through a distributed system.

---

## 26. What is Spring Boot Actuator?

**Answer:**

**Spring Boot Actuator** is a module that provides production-ready monitoring and management endpoints for Spring Boot applications.

It helps developers and operations teams monitor application health, metrics, and runtime information without writing custom monitoring code.


### Common Actuator Endpoints

| Endpoint | Purpose |
|-----------|---------|
| `/actuator/health` | Shows application health status |
| `/actuator/metrics` | Provides application metrics |
| `/actuator/info` | Displays application information |

#### Examples

Health Check:

```text
GET /actuator/health
```

Response:

```json
{
  "status": "UP"
}
```

Metrics:

```text
GET /actuator/metrics
```

Provides information such as:

- JVM memory usage
- CPU usage
- HTTP request counts
- Response times

### How Actuator Works in Microservices

```text
Microservice Application

        │
        ▼

 Spring Boot Actuator

        │
 ┌──────┼──────┐
 ▼      ▼      ▼

Health Metrics Info
```


### Uses of Spring Boot Actuator

#### 1. Health Checks

Checks whether the application and its dependencies are running.

Example:

```text
Application Status:

Database     ✓
Redis        ✓
Service      ✓
```


#### 2. Monitoring

Provides runtime information:

- Memory usage
- CPU usage
- Request statistics
- Application metrics


#### 3. Kubernetes Probes

Kubernetes uses Actuator endpoints to check application availability.

Example:

```text
Kubernetes

      │
      ▼

/actuator/health

      │
 ┌────┴────┐
 ▼         ▼

Liveness  Readiness
```

- **Liveness Probe** → Checks if the application is alive
- **Readiness Probe** → Checks if the application is ready to receive traffic


#### Benefits

- Production monitoring support
- Easy health checks
- Integration with monitoring tools
- Helps detect application failures
- Useful in cloud and container environments


#### Summary

```text
Spring Boot Actuator

Application
     │
     ▼
Actuator Endpoints
     │
     ├── Health
     ├── Metrics
     └── Info
     │
     ▼
Monitoring Systems
```

Spring Boot Actuator provides the visibility needed to monitor and manage microservices in production environments.

---

## 27. How do you handle configuration in Microservices?

**Answer:**

In a microservices architecture, each service requires configuration such as database URLs, API keys, environment settings, and application properties.

Instead of storing configuration separately inside each service, a **centralized configuration management** approach is used.

#### Centralized Configuration Architecture

```text
             Config Server

                  │

     ┌────────────┼────────────┐
     ▼            ▼            ▼

   User        Order       Payment
  Service     Service      Service
```

The Config Server provides configuration values to all microservices.


#### Common Configuration Management Tools

| Tool | Description |
|------|-------------|
| **Spring Cloud Config Server** | Centralized configuration management for Spring Boot microservices |
| **Kubernetes ConfigMaps** | Stores configuration values in Kubernetes environments |
| **Vault** | Securely manages secrets such as passwords, tokens, and API keys |


#### Example Using Spring Cloud Config

Configuration stored centrally:

```properties
# user-service.properties

server.port=8081
database.url=jdbc:mysql://localhost/users
```

The User Service fetches its configuration from the Config Server during startup.


#### Flow

```text
Microservice Startup

        │
        ▼

Request Configuration

        │
        ▼

Config Server

        │
        ▼

Load Properties

        │
        ▼

Start Application
```

#### Benefits of Centralized Configuration

- Single place to manage configurations
- Avoids duplicate configuration files
- Easy environment management
- Supports configuration changes without rebuilding services
- Improves security for sensitive properties

#### Example Environments

```text
Config Server

 ├── application-dev.properties
 ├── application-test.properties
 └── application-prod.properties
```

Each environment can have different settings.

#### Managing Secrets

Sensitive data should not be stored directly in configuration files.

Examples:

- Database passwords
- API keys
- Encryption keys

Use tools like:

- HashiCorp Vault
- Kubernetes Secrets


#### Summary

```text
Centralized Configuration

Config Server
       │
       ├── User Service
       ├── Order Service
       └── Payment Service
```

Centralized configuration makes microservices easier to manage, secure, and maintain across different environments.

---

## 28. How do you manage database changes in Microservices?

**Answer:**

In a microservices architecture, each service should manage its own database and database changes independently.

The recommended approach is **Database per Service**, where each microservice owns its data and controls its database schema changes.

#### Database Per Service Architecture

```text
        User Service
             │
             ▼
          User DB


        Order Service
             │
             ▼
          Order DB
```

Each service has:

- Its own database
- Its own schema
- Independent database migrations
- Full ownership of its data


### Best Practices

#### 1. Database Per Service

Each microservice should have a separate database.

Example:

```text
User Service
      │
      ▼
   User Database


Order Service
      │
      ▼
   Order Database


Payment Service
      │
      ▼
 Payment Database
```

Benefits:

- Loose coupling
- Independent scaling
- Better service ownership
- Technology flexibility

#### 2. Version-Controlled Database Migrations

Database changes should be stored and managed using version control.

Example:

```text
Migration Files

V1__create_user_table.sql

V2__add_email_column.sql

V3__create_order_table.sql
```

Benefits:

- Track database changes
- Repeat deployments safely
- Rollback changes when needed


#### 3. Use Migration Tools

Common database migration tools:

| Tool | Description |
|------|-------------|
| **Flyway** | Database migration tool that manages schema changes using versioned scripts |
| **Liquibase** | Database change management tool using XML, YAML, JSON, or SQL formats |

---

#### Example Flow

```text
Developer creates migration

        │
        ▼

Commit migration script

        │
        ▼

Application Deployment

        │
        ▼

Migration Tool Executes Changes

        │
        ▼

Database Updated
```

#### Handling Data Changes Between Services

Since services have separate databases, they should not directly access each other's databases.

Bad approach:

```text
Order Service
      │
      ▼
Direct Access
      │
      ▼
User Database
```

Better approach:

```text
Order Service
      │
      ▼
API / Event Communication
      │
      ▼
User Service
```

#### Summary

```text
Microservices Database Management

Service A
   │
   ▼
Database A
   │
   ▼
Own Migration


Service B
   │
   ▼
Database B
   │
   ▼
Own Migration
```

Best practices:

✓ Database per service  
✓ Version-controlled migrations  
✓ Use Flyway or Liquibase  
✓ Avoid direct database sharing between services  
✓ Use APIs or events for data communication  


---

## 29. What is Docker's role in Microservices?

**Answer:**

**Docker** is a containerization platform used in microservices to package applications along with all required dependencies into lightweight, portable containers.

It ensures that a microservice runs consistently across different environments such as development, testing, and production.

#### Docker Packaging

A Docker container packages:

```text
Application
      +
Java Runtime
      +
Libraries
      +
Configuration
      │
      ▼
 Docker Image
```

The Docker image is used to create running containers.


#### Docker in Microservices Architecture

```text
             API Gateway

                 │

     ┌───────────┼───────────┐
     ▼           ▼           ▼

 User Service  Order Service  Payment Service

 Docker        Docker          Docker
 Container     Container       Container
```

Each microservice runs independently inside its own container.


#### Docker Workflow

```text
Developer Code

      │
      ▼

Create Docker Image

      │
      ▼

Store Image in Registry

      │
      ▼

Run Docker Container

      │
      ▼

Microservice Running
```

#### Benefits of Docker in Microservices

| Benefit | Description |
|---------|-------------|
| **Consistent Environments** | Application runs the same way across development, testing, and production. |
| **Easy Deployment** | Containers can be quickly created, started, and deployed. |
| **Isolation** | Each service runs independently without affecting other services. |
| **Portability** | Containers can run on different machines and cloud platforms. |
| **Scalability** | Multiple instances of a service can be created easily. |


#### Example

Without Docker:

```text
Developer Machine
       │
       ▼
"Works on my machine"
```

Problems:

- Different Java versions
- Missing libraries
- Environment differences

With Docker:

```text
Docker Container

Application
Java Version
Dependencies
Configuration
```

The same container runs everywhere.


#### Docker with Kubernetes

In production environments, Docker containers are commonly managed by orchestration platforms like Kubernetes.

```text
Docker Container

        │

        ▼

 Kubernetes

        │

        ▼

Deploy and Scale Microservices
```


#### Summary

```text
Docker

Microservice
     │
     ▼
Package Application + Dependencies
     │
     ▼
Docker Image
     │
     ▼
Container
     │
     ▼
Deploy Anywhere
```

Docker provides **consistent, isolated, and portable environments**, making it easier to develop, deploy, and scale microservices.

---

## 30. What is Kubernetes' role in Microservices?

**Answer:**

**Kubernetes** is a container orchestration platform used to manage, deploy, and scale containerized applications in a microservices architecture.

It automates tasks such as deploying containers, managing service communication, scaling applications, and recovering from failures.

#### Kubernetes in Microservices Architecture

```text
             Kubernetes Cluster

        ┌──────────────────────┐
        │                      │
        ▼                      ▼

    User Pods             Order Pods

        │                      │

        ▼                      ▼

    User Service          Order Service


                 Payment Pods
                      │
                      ▼
                Payment Service
```


#### Key Features of Kubernetes

| Feature | Description |
|---------|-------------|
| **Service Discovery** | Allows microservices to find and communicate with each other using Kubernetes Services. |
| **Scaling** | Automatically increases or decreases application instances based on demand. |
| **Load Balancing** | Distributes traffic across multiple service instances. |
| **Self Healing** | Automatically restarts failed containers and replaces unhealthy instances. |
| **Deployment Management** | Manages application deployments, updates, and rollbacks. |


#### Kubernetes Workflow

```text
Docker Image

      │
      ▼

Kubernetes Deployment

      │
      ▼

Create Pods

      │
      ▼

Expose Services

      │
      ▼

Application Available
```

#### Example: Scaling a Service

Before scaling:

```text
Order Service

    Pod 1
```

High traffic:

```text
Order Service

    Pod 1
    Pod 2
    Pod 3
```

Kubernetes automatically creates additional pods to handle increased load.


#### Self-Healing Example

If a pod fails:

```text
Order Service

Pod 1  ✖ Failed

        │
        ▼

Kubernetes Detects Failure

        │
        ▼

Creates New Pod

        │
        ▼

Order Service Available
```

#### Kubernetes Components Used in Microservices

| Component | Purpose |
|-----------|---------|
| **Pod** | Smallest deployable unit that runs containers |
| **Service** | Provides stable networking and service discovery |
| **Deployment** | Manages application replicas and updates |
| **ConfigMap** | Stores application configuration |
| **Secret** | Stores sensitive information |


#### Benefits of Kubernetes in Microservices

- Automates deployment
- Simplifies scaling
- Provides high availability
- Handles failures automatically
- Manages container lifecycle
- Supports cloud-native applications


#### Summary

```text
Kubernetes

Docker Containers
        │
        ▼
Kubernetes Cluster
        │
        ├── Deploy
        ├── Scale
        ├── Load Balance
        ├── Discover Services
        └── Recover Failures
```

Kubernetes acts as the **management layer for microservices containers**, making applications more reliable, scalable, and easier to operate in production.

---

## 31. How do you design a scalable Microservice?

**Answer:**

A **scalable microservice** is designed to handle increasing traffic and workload by adding resources efficiently while maintaining performance and reliability.

A good microservice design focuses on scalability, availability, performance, and fault tolerance.


### Key Considerations for Designing Scalable Microservices

#### 1. Stateless Services

Services should avoid storing user session data locally.

Instead, store shared state in external systems like:

- Redis
- Databases
- Distributed storage

Example:

```text
User Request

      │
      ▼

Service Instance 1
Service Instance 2
Service Instance 3

(All can handle requests)
```

Benefits:

- Easy horizontal scaling
- Better load distribution
- No dependency on a specific server

#### 2. Horizontal Scaling

Increase capacity by adding more service instances instead of increasing server size.

Example:

Before:

```text
Order Service

    Pod 1
```

After scaling:

```text
Order Service

    Pod 1
    Pod 2
    Pod 3
```


#### 3. Load Balancing

Distribute incoming requests across multiple service instances.

Example:

```text
             Client

                │

          Load Balancer

        ┌───────┼───────┐
        ▼       ▼       ▼

     Service  Service  Service
      Pod 1    Pod 2    Pod 3
```

Benefits:

- Better performance
- Prevents single server overload
- Improves availability


#### 4. Caching Using Redis

Use caching to reduce database load and improve response time.

Example:

```text
Request

   │
   ▼

 Redis Cache

   │
   ├── Hit → Return Data
   │
   └── Miss → Database → Store in Redis
```

Benefits:

- Faster responses
- Reduced database traffic
- Improved scalability


#### 5. Asynchronous Processing Using Kafka

Use message brokers for time-consuming operations.

Example:

```text
Order Service

      │

      ▼

 Kafka Topic

      │

 ┌────┴────┐
 ▼         ▼

Inventory  Notification
Service    Service
```

Benefits:

- Loose coupling
- Better throughput
- Improved fault tolerance


#### 6. Database Optimization

Use efficient database strategies:

- Database indexing
- Query optimization
- Connection pooling
- Database per service
- Read replicas

Example:

```text
Order Service

      │

      ▼

 Order Database

      │

      ▼

 Optimized Queries
```


#### 7. Monitoring

Monitor application health and performance using:

- Spring Boot Actuator
- Prometheus
- Grafana
- ELK Stack

Monitor:

- Response time
- Error rate
- CPU usage
- Memory usage
- Request traffic

#### 8. Fault Tolerance

Design services to handle failures gracefully.

Techniques:

- Circuit Breaker
- Retry mechanism
- Timeout handling
- Fallback responses
- Bulkhead pattern

Example:

```text
Order Service

      │

      ▼

Circuit Breaker

      │

      ▼

Payment Service

      │

      ✖ Failure

      │

      ▼

Fallback Response
```


#### Scalable Microservice Architecture

```text
              Client

                │

                ▼

           API Gateway

                │

        Load Balancer

                │

 ┌──────────────┼──────────────┐
 ▼              ▼              ▼

User Service  Order Service  Payment Service

      │              │              │

      ▼              ▼              ▼

 Redis          Kafka          Database
 Cache          Events        Optimization
```

#### Summary

A scalable microservice design uses:

✓ Stateless services  
✓ Horizontal scaling  
✓ Load balancing  
✓ Redis caching  
✓ Kafka-based asynchronous processing  
✓ Database optimization  
✓ Monitoring and observability  
✓ Fault tolerance mechanisms  

These practices help microservices handle high traffic, remain available, and scale efficiently in production environments.

---

## 32. How do you handle failures between services?

**Answer:**

In a microservices architecture, service-to-service communication can fail due to network issues, service downtime, or high load.

To handle these failures gracefully, we use different **fault tolerance patterns**.


#### Common Failure Handling Techniques

| Technique | Description |
|-----------|-------------|
| **Timeout** | Limits how long a service waits for a response from another service. |
| **Retry** | Attempts the failed request again based on configured rules. |
| **Circuit Breaker** | Stops calling a failing service temporarily and provides fallback responses. |
| **Fallback Methods** | Returns an alternative response when a service is unavailable. |
| **Bulkhead Pattern** | Isolates failures by limiting resources used by a service call. |


#### 1. Timeout

Prevents a service from waiting indefinitely.

Example:

```text
Order Service

      │
      ▼

Payment Service

      │
      ✖ No Response

      │
      ▼

Timeout Occurs
```

Benefits:

- Prevents resource blocking
- Improves system responsiveness


#### 2. Retry

Retries a failed request for temporary failures.

Example using Resilience4j:

```java
@Retry(name = "payment")
public Payment callPayment() {
    return client.pay();
}
```

Flow:

```text
Request

   │

Payment Service

   │

Failure

   │

Retry Request

   │

Success / Final Failure
```

#### 3. Circuit Breaker

Stops repeated calls to an unhealthy service.

Example:

```text
Order Service

      │

Circuit Breaker

      │

Payment Service (Down)

      │

Fallback Response
```

Benefits:

- Prevents cascading failures
- Gives failing services time to recover


#### 4. Fallback Methods

Provides an alternative response when a service fails.

Example:

```java
public Payment fallback(Exception e) {
    return new Payment("Payment service unavailable");
}
```

Flow:

```text
Payment Service Failure

          │

          ▼

Fallback Method

          │

          ▼

Return Default Response
```


#### 5. Bulkhead Pattern

Limits the number of requests to a service so that one failure does not affect the entire system.

Example:

```text
Payment Service Calls

Maximum Allowed: 50

Request 51+

       │

       ▼

Rejected / Fallback
```

Benefits:

- Prevents resource exhaustion
- Isolates failures


#### Complete Failure Handling Flow

```text
Client Request

       │

       ▼

Order Service

       │

       ▼

Timeout + Retry

       │

       ▼

Circuit Breaker

       │

       ▼

Payment Service

       │

       ✖ Failure

       │

       ▼

Fallback Response
```

#### Summary

To handle failures between microservices, use:

✓ Timeout to avoid waiting too long  
✓ Retry for temporary failures  
✓ Circuit Breaker to prevent cascading failures  
✓ Fallback methods for graceful responses  
✓ Bulkhead pattern for service isolation  

These patterns improve the **reliability and resilience** of microservices systems.

---

## 33. What is Idempotency in Microservices?

**Answer:**

**Idempotency** means that performing the same operation multiple times produces the same result as performing it once.

In microservices, idempotency is important because network failures, retries, or duplicate requests can cause the same API request to be sent multiple times.

An idempotent operation prevents duplicate processing.


#### Example: Payment API

Request:

```http
POST /payment

{
  "transactionId": "123",
  "amount": 100
}
```

First request:

```text
Payment Created ✓
```

Second request with the same transaction ID:

```text
Duplicate Request

Payment Already Exists

No New Payment Created
```

Final Result:

```text
Only One Payment Record Created
```

#### Why Idempotency is Needed?

In distributed systems:

```text
Client
  │
  ▼
Payment Service
  │
  ✖ Network Timeout

Client Retries Request

  │
  ▼

Payment Service
```

Without idempotency:

```text
Payment 1 Created ✓
Payment 2 Created ✓

Duplicate Payment
```

With idempotency:

```text
Payment 1 Created ✓

Retry Request

Duplicate Detected

No New Payment Created
```


### How to Implement Idempotency

#### 1. Unique Transaction IDs

Generate a unique identifier for every business operation.

Example:

```text
transactionId = TXN12345
```

Store it with the transaction record.

---

#### 2. Database Constraints

Use unique constraints to prevent duplicate records.

Example:

```sql
CREATE TABLE payments (
    id BIGINT,
    transaction_id VARCHAR(100) UNIQUE,
    amount DECIMAL
);
```

If the same transaction ID is inserted again, the database rejects it.


#### 3. Idempotency Keys

Client sends a unique key with each request.

Example:

```http
POST /payment

Idempotency-Key: abc-12345
```

Server stores the key after successful processing.

Future requests with the same key return the existing result.


#### Idempotent vs Non-Idempotent Operations

| Operation | Idempotent? | Example |
|-----------|-------------|---------|
| GET | Yes | Fetch user details |
| PUT | Yes | Update user information |
| DELETE | Usually Yes | Delete a resource |
| POST | Usually No | Create a new order/payment |


#### Example Flow

```text
Client

  │
  ▼

Payment API

  │
  ▼

Check Idempotency Key

  │
 ┌─────────────┐
 ▼             ▼

Exists       New Request

Return       Process Payment
Existing          │
Result            ▼
              Save Key
```


#### Benefits

- Prevents duplicate transactions
- Handles retries safely
- Improves reliability
- Maintains data consistency
- Important for payment and order systems


#### Summary

```text
Idempotency

Same Request
     │
     ▼
Multiple Times
     │
     ▼
Same Final Result
```

Idempotency ensures that repeated requests in a distributed system do not create duplicate operations.

---

## 34. Common Microservices Production Scenario Questions

### Q1: Payment Service is Down. What Happens?

**Answer:**

When a dependent service like the Payment Service becomes unavailable, the system should handle the failure gracefully instead of allowing it to affect the entire application.

### Handling Steps:

1. **Circuit Breaker Opens**

The circuit breaker detects repeated failures and stops sending requests to the unavailable service.

```text
Order Service

      │

      ▼

Circuit Breaker

      │

      ▼

Payment Service (Down)

      │

      ▼

Circuit OPEN
```

---

2. **Return Fallback Response**

Instead of failing completely, the application returns an alternative response.

Example:

```text
"Payment service is temporarily unavailable. Please try again later."
```

---

3. **Retry Later**

The system can retry the request after a configured delay.

```text
Request Failed

      │

      ▼

Wait

      │

      ▼

Retry Payment Service
```

---

4. **Log Failure**

Store failure details for troubleshooting.

Example:

```text
Service: Payment-Service
Error: Connection Timeout
Time: 10:30 AM
Request ID: ABC123
```

---

5. **Alert Operations Team**

Monitoring systems notify the team about service failures.

Tools:

- Prometheus
- Grafana
- ELK Stack
- PagerDuty

---

### Q2: Database is Slow. How Will You Improve Performance?

**Answer:**

Database performance issues can be handled using optimization techniques.

### Solutions:

### 1. Add Indexes

Indexes improve query search performance.

Example:

```sql
CREATE INDEX idx_user_email
ON users(email);
```

---

### 2. Introduce Caching

Store frequently accessed data in Redis.

```text
Request

   │

   ▼

Redis Cache

   │
   ├── Hit → Return Data
   │
   └── Miss → Database
```

Benefits:

- Faster response time
- Reduced database load

---

### 3. Optimize Queries

Improve database queries by:

- Removing unnecessary joins
- Selecting only required columns
- Analyzing query execution plans

Example:

Avoid:

```sql
SELECT *
FROM users;
```

Prefer:

```sql
SELECT id, name
FROM users;
```

---

### 4. Use Read Replicas

Separate read and write operations.

```text
Application

     │

     ├── Write → Primary Database
     │
     └── Read → Replica Database
```

Benefits:

- Improves read performance
- Reduces primary database load

---

### 5. Async Processing

Move slow operations to background processing.

Example:

```text
Order Service

      │

      ▼

Kafka Event

      │

      ▼

Email / Notification Service
```

---

# Q3: How Will You Deploy 100 Microservices?

**Answer:**

Deploying many microservices requires automation, containerization, orchestration, and monitoring.

### Technologies and Practices:

---

## 1. Docker Containers

Package each microservice with its dependencies.

```text
Application
     +
Dependencies
     │
     ▼
Docker Image
     │
     ▼
Container
```

---

## 2. Kubernetes

Manage deployment and scaling of containers.

Kubernetes provides:

- Service discovery
- Load balancing
- Auto scaling
- Self healing
- Deployment management

Example:

```text
Kubernetes Cluster

 ┌─────────────┐
 │ User Pods   │
 ├─────────────┤
 │ Order Pods  │
 ├─────────────┤
 │ Payment Pods│
 └─────────────┘
```

---

## 3. CI/CD Pipelines

Automate:

- Build
- Testing
- Deployment

Example flow:

```text
Developer Commit

        │

        ▼

CI Pipeline

        │

        ▼

Automated Tests

        │

        ▼

Docker Build

        │

        ▼

Kubernetes Deployment
```

---

## 4. Automated Testing

Include:

- Unit tests
- Integration tests
- API tests
- Security tests

---

## 5. Monitoring

Monitor application health using:

- Spring Boot Actuator
- Prometheus
- Grafana
- ELK Stack

Monitor:

- CPU usage
- Memory
- Errors
- Response time
- Service availability

---

## Summary

### Production Failure Handling

```text
Service Failure

      │

      ▼

Circuit Breaker

      │

      ▼

Fallback + Retry + Alert
```

### Database Performance

```text
Slow Database

      │

      ▼

Indexes + Cache + Query Optimization + Replicas
```

### Large-Scale Deployment

```text
Microservices

      │

      ▼

Docker

      │

      ▼

Kubernetes

      │

      ▼

CI/CD + Monitoring
```

These practices help build reliable, scalable, and production-ready microservices systems.

---

# Most Frequently Asked Microservices Interview Topics for Java Developers

For Java developers, microservices interviews usually focus on architecture concepts, Spring ecosystem tools, distributed system challenges, and real-world production scenarios.

## 1. Spring Boot REST APIs

**Focus Areas:**

- Creating REST controllers
- HTTP methods (GET, POST, PUT, DELETE)
- Request and response handling
- Exception handling
- Validation
- REST API best practices
- Spring Boot Actuator

Example:

```java
@RestController
@RequestMapping("/users")
public class UserController {

    @GetMapping("/{id}")
    public User getUser(@PathVariable Long id) {
        return service.getUser(id);
    }
}
```

---

## 2. Spring Cloud

**Focus Areas:**

- Building cloud-native microservices
- Service discovery
- Configuration management
- Load balancing
- API Gateway integration
- Fault tolerance

Common Spring Cloud components:

- Eureka
- Spring Cloud Gateway
- OpenFeign
- Config Server
- Resilience4j

---

## 3. API Gateway

**Focus Areas:**

- Single entry point for clients
- Request routing
- Authentication
- Rate limiting
- Logging
- Request transformation

Architecture:

```text
Client

   │

   ▼

API Gateway

   │
 ┌─┼─┐
 ▼ ▼ ▼

User Order Payment
Services
```

---

## 4. Eureka / Service Discovery

**Focus Areas:**

- Service registration
- Service discovery
- Dynamic service lookup
- Load balancing

Flow:

```text
Payment Service

      │

      ▼

Eureka Server

      │

      ▼

Order Service discovers Payment Service
```

---

## 5. Feign Client

**Focus Areas:**

- Service-to-service communication
- Declarative REST clients
- Integration with Eureka

Example:

```java
@FeignClient(name = "payment-service")
public interface PaymentClient {

    @GetMapping("/payment/{id}")
    Payment getPayment(@PathVariable Long id);
}
```

---

## 6. Kafka

**Focus Areas:**

- Event-driven architecture
- Producers and consumers
- Topics
- Asynchronous communication
- Message processing

Example:

```text
Order Service

      │

      ▼

Kafka Topic

      │

 ┌────┴────┐
 ▼         ▼

Inventory  Notification
Service    Service
```

---

## 7. Redis Caching

**Focus Areas:**

- Cache-aside pattern
- Cache hit and miss
- TTL
- Cache eviction
- Distributed caching

Example:

```text
Request

   │

   ▼

Redis Cache

   │
   ├── Hit → Return Data
   │
   └── Miss → Database
```

---

## 8. JWT Security

**Focus Areas:**

- Authentication
- Authorization
- Token validation
- Role-based access control
- Stateless security

Flow:

```text
Client

  │

JWT Token

  │

API Gateway

  │

Microservices
```

---

## 9. Circuit Breaker (Resilience4j)

**Focus Areas:**

- Fault tolerance
- Preventing cascading failures
- Fallback mechanisms
- Retry handling

States:

```text
Closed
  │
  ▼
Open
  │
  ▼
Half Open
```

Example:

```java
@CircuitBreaker(
    name="paymentService",
    fallbackMethod="fallback"
)
public Payment pay(){
    return paymentClient.process();
}
```

---

## 10. Docker & Kubernetes

### Docker Focus Areas:

- Containerization
- Docker images
- Docker containers
- Environment consistency

```text
Application
+
Dependencies
+
Runtime

      ↓

Docker Image
```

### Kubernetes Focus Areas:

- Container orchestration
- Scaling
- Load balancing
- Self-healing
- Deployment management

```text
Kubernetes Cluster

User Pods
Order Pods
Payment Pods
```

---

## 11. Saga Pattern

**Focus Areas:**

- Distributed transactions
- Data consistency
- Compensation actions

Example:

```text
Create Order

      ↓

Payment

      ↓

Reserve Inventory
```

Failure:

```text
Inventory Failed

      ↓

Refund Payment

      ↓

Cancel Order
```

Types:

- Choreography Saga
- Orchestration Saga

---

## 12. Distributed Tracing

**Focus Areas:**

- Tracking requests across services
- Trace ID
- Span information
- Performance debugging

Tools:

- Zipkin
- Jaeger
- OpenTelemetry

Example:

```text
User Request

     ↓

Gateway

     ↓

Order Service

     ↓

Payment Service

     ↓

Database
```

---

## 13. Database Per Service

**Focus Areas:**

- Independent databases
- Data ownership
- Database migrations
- Avoiding shared databases

Example:

```text
User Service
      |
    User DB


Order Service
      |
   Order DB
```

---

## 14. Exception Handling

**Focus Areas:**

- Global exception handling
- Custom exceptions
- Proper HTTP status codes
- Error response formats

Example:

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(UserNotFoundException.class)
    public ResponseEntity<?> handleException(Exception e){
        return ResponseEntity
              .status(HttpStatus.NOT_FOUND)
              .body(e.getMessage());
    }
}
```

---

## 15. Production Troubleshooting Scenarios

Interviewers commonly ask:

### Payment Service is down

Expected discussion:

- Circuit breaker
- Retry
- Fallback response
- Logging
- Monitoring alerts

---

### Database is slow

Solutions:

- Query optimization
- Indexing
- Redis caching
- Read replicas
- Async processing

---

### High traffic on services

Solutions:

- Horizontal scaling
- Load balancing
- Kubernetes autoscaling
- Caching

---

### Deployment of many microservices

Approach:

```text
Source Code

      ↓

CI/CD Pipeline

      ↓

Docker Images

      ↓

Kubernetes Deployment

      ↓

Monitoring
```

---

# Final Interview Preparation Priority

For Java Microservices interviews, focus most on:

1. Spring Boot REST APIs  
2. Spring Cloud components  
3. API Gateway  
4. Eureka / Service Discovery  
5. Feign Client  
6. Kafka and Event-Driven Architecture  
7. Redis Caching  
8. JWT Security  
9. Resilience4j Circuit Breaker  
10. Docker & Kubernetes  
11. Saga Pattern  
12. Distributed Tracing  
13. Database per Service  
14. Exception Handling  
15. Real production troubleshooting scenarios  

Mastering these topics covers the majority of questions asked in Java Microservices interviews.

