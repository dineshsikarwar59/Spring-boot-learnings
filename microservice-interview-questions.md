# Microservice-interview questions

### 1. What are Microservices?

**Answer:**
Microservices are a software architecture style in which an application is built as a collection of small, independent services. Each service focuses on a specific business function, communicates with other services through APIs, and can be developed, deployed, and scaled independently.

Microservices is an architectural style where an application is divided into multiple independent services. Each service:

- Has its own business functionality
- Can be deployed independently
- Owns its own database
- Communicates using REST, gRPC, or messaging systems like Kafka or RabbitMQ

**Example:**

An e-commerce application may have:

- User Service
- Product Service
- Order Service
- Payment Service
- Notification Service

Each service can scale independently.

**Characteristics of Microservices**
- **Independent:** Each service runs independently of others.
- **Single Responsibility:** Each service performs one specific business function.
- **API-based Communication:** Services communicate using REST APIs, gRPC, or messaging systems.
- **Independent Deployment:** A service can be updated without redeploying the entire application.
- **Scalability:** Individual services can be scaled based on demand.
- **Technology Flexibility:** Different services can use different programming languages or databases if needed.


## 2. What are the advantages of Microservices?

**Answer:**
- **Independent deployment** – Each service can be deployed without affecting the others, making updates faster and reducing downtime.
- **Better scalability** – Individual services can be scaled independently based on demand, improving resource utilization and performance.
- **Technology flexibility** – Different services can use different programming languages, frameworks, or databases that best fit their requirements.
- **Fault isolation** – If one service fails, it is less likely to bring down the entire application, improving overall system reliability.
- **Faster development** – Multiple teams can work on different services simultaneously, speeding up the development process.
- **Smaller codebase** – Each service contains only the code required for its specific functionality, making it easier to understand, maintain, and test.
- **Independent teams** – Teams can own and manage specific services independently, enabling faster decision-making and reducing coordination overhead.


## 3. What are the disadvantages?

**Answer:**

- **Distributed system complexity** – Managing multiple services communicating over a network is more complex than managing a single application. Developers need to handle service communication, failures, and dependencies.
- **Network latency** – Services communicate through networks, which can introduce delays and affect application performance compared to direct in-process communication.
- **Data consistency challenges** – Maintaining consistent data across multiple services can be difficult, especially when services have separate databases.
- **Difficult debugging** – Finding and fixing issues is harder because a single request may pass through multiple services, making it challenging to trace the source of errors.
- **Monitoring complexity** – Multiple services require advanced monitoring and logging tools to track system health, performance, and failures.
- **DevOps overhead** – Managing deployments, infrastructure, automation, and service operations requires additional DevOps practices and tools.
- **Deployment complexity** – Deploying many independent services requires proper CI/CD pipelines, configuration management, and version control to avoid compatibility issues.


## 4.Monolith vs Microservices.

**Answer:**

| Monolith | Microservices |
|----------|---------------|
| **Single deployment** – The entire application is deployed as one unit. | **Independent deployment** – Each service can be deployed separately without affecting other services. |
| **Single database** – All application modules typically share one database. | **Database per service** – Each service can manage its own database based on its requirements. |
| **Hard to scale** – The whole application needs to be scaled even if only one part requires more resources. | **Easy to scale** – Individual services can be scaled independently based on demand. |
| **Tight coupling** – Components are highly dependent on each other, making changes more difficult. | **Loose coupling** – Services are independent and communicate through APIs, making changes easier. |
| **One technology stack** – The application usually uses a single programming language and framework. | **Multiple technologies possible** – Different services can use different languages, frameworks, or databases. |
| **Failure affects entire app** – A problem in one module can impact the whole application. | **Failure isolated** – Failure in one service is less likely to affect the entire system. |




## 5. How do Microservices communicate?

**Answer:** 
Microservices communicate using different communication patterns based on the application's requirements. The three main communication patterns are:

### 1. Synchronous Communication
- Uses RESTful APIs or web services for request-response communication.
- The client waits for the service to respond before continuing.
- Simple to implement but can create tighter coupling between services.
- Best suited for real-time requests and workflow-specific operations.

### 2. Asynchronous Communication
- Uses message queues or message brokers (such as RabbitMQ or Amazon SQS).
- Services send messages without waiting for an immediate response.
- Improves reliability, as messages are delivered even if a service is temporarily unavailable or under heavy load.
- Ideal for long-running, high-load, or unpredictable processes.

### 3. Data Streaming
- Used for continuous, real-time data exchange between services.
- Suitable for applications that process high-frequency data, such as stock market updates, IoT systems, or real-time analytics.
- Common technologies include Apache Kafka and AWS Kinesis.

**Conclusion:**

Microservices communicate through:
- **Synchronous communication** (REST APIs)
- **Asynchronous communication** (Message Queues)
- **Data Streaming** (Apache Kafka, AWS Kinesis)

The choice depends on factors such as response time, scalability, reliability, and business requirements.


## 6. What is API Gateway?

**Answer:** 
An API Gateway is a server or service that acts as a single entry point between clients (applications/users) and backend services. It receives API requests from clients, routes them to the appropriate backend service, and returns the response.

Key functions of an API Gateway:
- **Request routing** – Directs incoming API requests to the correct microservice or backend application.
- **Authentication and authorization** – Verifies user identity and access permissions.
- **Load balancing** – Distributes requests across multiple servers to improve reliability.
- **Rate limiting** – Controls the number of requests a client can make to prevent abuse.
- **Caching** – Stores frequently requested data to improve response speed.
- **Request/response transformation** – Modifies data formats between clients and services.
- **Monitoring and logging** – Tracks API usage, errors, and performance.

Example:

In an online shopping application, a mobile app may send requests through an API Gateway. The gateway routes:
- User login requests → Authentication service
- Product searches → Product service
- Orders → Order service
- Payments → Payment service

Popular API Gateway solutions include Spring Cloud Gateway, Amazon API Gateway.

**In short**, an API Gateway manages communication between clients and backend services while improving security, scalability, and performance.

## 7. Why use Service Discovery? Or What is Service Discovery?
**Answer:**
Service Discovery is a mechanism used in microservices architecture to automatically detect and locate available services so that applications can communicate with each other without needing to know the exact network locations (IP addresses or ports) of those services.

Why use Service Discovery?

1. **Dynamic service location** – Services can start, stop, or move across servers, and service discovery automatically keeps track of their current locations.
2. **Load balancing** – It distributes requests among multiple instances of a service to improve performance and availability.
3. **Scalability** – New service instances can be added or removed without changing application configurations.
4. **Fault tolerance** – It can detect unhealthy services and prevent requests from being sent to failed instances.
5. **Reduced manual configuration** – Developers do not need to maintain hard-coded IP addresses and connection details.
6. **Supports microservices communication** – It enables services to find and communicate with each other efficiently in distributed systems.
Example:

Instead of:
```text
http://10.10.20.30
```
Use:
```text
http://payment-service
```
In an e-commerce application:

- The Order Service needs to communicate with the Payment Service.
- Instead of storing the Payment Service’s fixed IP address, the Order Service queries the Service Discovery system.
- Service Discovery returns an available Payment Service instance, and the request is sent there.

Popular service discovery tools include Netflix Eureka, HashiCorp Consul, and Apache ZooKeeper.

In short, Service Discovery helps microservices find, connect to, and communicate with each other automatically in a scalable and reliable way.


## 8. Client-side vs Server-side Discovery.

**Answer:**
Summary:
- Client-side discovery: Client is responsible for finding services and choosing instances.
- Server-side discovery: A gateway or load balancer handles service discovery and routing.

1. **In Client-Side Discovery** The User Service does all the detective work.
   - **Step 1:** The User Service asks the Service Registry: "Where are all the active instances of the Order Service?"
   - **Step 2:** The Registry replies with a list of IP addresses (e.g., Instance 1: 10.0.0.5, Instance 2: 10.0.0.6).
   - **Step 3:** The User Service picks one IP (e.g., Instance 1) and sends the request directly to that specific Order Service instance.

2. **In Server-Side Discovery** The User Service remains completely blind to where the Order Service actually lives.
   - **Step 1:** The User Service simply sends a request to a central Load Balancer/Router: "Hey, get this request to the Order Service."
   - **Step 2:** The Load Balancer looks at its registry, finds an active Order Service instance, and forwards the request for you.


## 9.Why should every microservice have its own database?

**Answer:** 
Each microservice should have its own database because it supports the core principles of independence, loose coupling, and autonomy. Here are the main reasons:

1. **Independent Development and Deployment**
    - Each microservice can evolve its database schema without affecting other services.
    - Teams can deploy updates independently.
2. **Loose Coupling**
    - Services communicate through APIs rather than directly accessing each other's databases.
    - This reduces dependencies between services.
3. **Technology Flexibility**
    - Each service can choose the database that best suits its needs (polyglot persistence).
    - For example, one service might use MySQL, while another uses MongoDB.
4. **Better Scalability**
    - Databases can be scaled independently based on the workload of each microservice.
    - High-traffic services won't affect the performance of others.
5. **Improved Fault Isolation**
    - If one database fails, only the corresponding microservice is impacted.
    - Other services continue to operate normally.
6. **Enhanced Security**
    - Access to data is restricted to the owning microservice.
    - This prevents unauthorized direct access by other services.
7. **Clear Data Ownership**
    - Each microservice owns and manages its own data.
    - This avoids conflicts and maintains data consistency within the service.

**Conclusion:** 
Having a separate database for each microservice ensures service autonomy, scalability, flexibility, fault isolation, and maintainability, making the overall microservices architecture more robust and easier to manage.

## 10. 5. REST vs gRPC

**Answer:**

| Feature | REST | gRPC |
|---------|------|------|
| **Protocol** | HTTP/1.1 (commonly) | HTTP/2 |
| **Data Format** | JSON | Protocol Buffers (Protobuf) |
| **Performance** | Slower due to larger JSON payloads | Faster with compact binary data |
| **Communication** | Request–response | Supports unary, client streaming, server streaming, and bidirectional streaming |
| **Ease of Use** | Easy to understand and widely supported | Requires Protobuf definitions and code generation |
| **Browser Support** | Excellent | Limited (typically requires gRPC-Web) |
| **Best For** | Public APIs, web and mobile applications | Internal microservice communication and high-performance systems |

### Key Differences

- **REST** is simple, human-readable, and ideal for public APIs because it uses JSON over HTTP.
- **gRPC** is faster and more efficient because it uses Protocol Buffers (Protobuf) and HTTP/2, making it well-suited for communication between microservices.
- **REST** is easier to test with tools like **Postman**, whereas **gRPC** requires generated client and server code.

### Conclusion

- Use **REST** for external/public APIs and applications that require broad compatibility.
- Use **gRPC** for internal microservice communication where high performance, low latency, and efficient data transfer are important.


## 8. What is Circuit Breaker?

**Answer:** 
A **Circuit Breaker** is a design pattern used in microservices to prevent repeated requests to a failing service. It improves fault tolerance by stopping calls to an unavailable service and allowing it time to recover.

**How It Works**

The Circuit Breaker has three states:
 1. **Closed**
    - Requests are sent to the target service normally.
    - If failures exceed a predefined threshold, the circuit moves to the **Open** state.

 2. **Open**
    - Requests are blocked immediately without calling the failing service.
    - This prevents unnecessary load and gives the service time to recover.

 3. **Half-Open**
    - After a timeout period, a limited number of test requests are allowed.
    - If the requests succeed, the circuit moves back to **Closed**.
    - If the requests fail, it switches back to **Open**.

**Benefits**

- Prevents cascading failures across microservices.
- Improves system reliability and resilience.
- Reduces unnecessary network calls to failed services.
- Enables faster recovery by avoiding overloaded services.

**Example:** 
Suppose the **Order Service** calls the **Payment Service**. If the Payment Service becomes unavailable, the Circuit Breaker opens after repeated failures and immediately returns an error or fallback response instead of repeatedly attempting failed requests.

**Common Circuit Breaker Libraries**

- **Resilience4j** (recommended for Java/Spring Boot)
- **Netflix Hystrix** (legacy, now in maintenance mode)
- **Spring Cloud Circuit Breaker**

**Conclusion**

A **Circuit Breaker** is a resilience pattern that detects service failures, temporarily stops requests to failing services, and automatically resumes communication once the service recovers.
