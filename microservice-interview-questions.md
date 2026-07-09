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
Microservices communicate with each other using APIs and messaging systems. Since each service is independent, communication mechanisms allow them to exchange data and perform operations.
**In short:**  Microservices communicate through lightweight protocols to enable independent, scalable, and loosely coupled services.

**1. Synchronous Communication**
In **synchronous communication**, one service sends a request to another service and waits until it receives a response before continuing its operation.

 **Common Methods**
- **REST APIs (HTTP/HTTPS)** – Services communicate using HTTP requests and responses. It is one of the most commonly used methods for service-to-service communication.
- **gRPC** – A high-performance communication framework that uses Protocol Buffers for faster and efficient communication between services.

**2. Asynchronous Communication:** 
In asynchronous communication, a service sends a message without waiting for an immediate response.

**Common methods:**
- Message queues
- Event brokers

**Examples include:**
- RabbitMQ
- Apache Kafka
- Amazon SQS


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

