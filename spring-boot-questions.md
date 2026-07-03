# Spring-boot-interview questions

### 1. What is Spring Boot?

**Answer:**
Spring Boot is an extension of the Spring Framework that simplifies application development by providing:

- **Auto Configuration** – Automatically configures the application based on the dependencies present.
- **Embedded Servers (Tomcat, Jetty, Undertow)** – Run applications without deploying to an external server.
- **Starter Dependencies** – Simplify dependency management using starter packages.
- **Production-ready Features (Actuator)** – Provides monitoring, health checks, metrics, and application management endpoints.
- **Minimal Configuration** – Reduces boilerplate configuration and enables rapid application development.

It helps developers build standalone, production-ready applications quickly.


### 2. What are the advantages of Spring Boot?

**Answer:**
- Less configuration
- Faster development
- Embedded web server
- Auto configuration
- Starter POMs
- Actuator monitoring
- Easy Microservices development
- Externalized configuration
- Production-ready applications

### 3. Explain Spring Boot Auto Configuration.

**Answer:**

**Spring Boot Auto Configuration** is a feature that automatically configures Spring beans based on:
- The dependencies available in the classpath
- Existing beans in the application context
- Configuration in application.properties or application.yml

It reduces the need for manual configuration and provides sensible default settings, which can be customized if needed.

**Example:**

If you add the spring-boot-starter-data-jpa dependency, Spring Boot automatically configures:
- DataSource
- EntityManagerFactory
- TransactionManager
- Hibernate

using default settings. You only need to provide required properties such as the database URL, username, and password, or override the defaults if you want custom behavior.

**Interview Answer (Short):**
Spring Boot Auto Configuration automatically configures the required Spring beans based on the project's dependencies, existing beans, and application properties. For example, adding spring-boot-starter-data-jpa automatically configures the DataSource, EntityManagerFactory, TransactionManager, and Hibernate, reducing the need for manual configuration.

## 4. Difference between @Component, @Service, @Repository, and @Controller?

**Answer:**
All four are stereotype annotations in Spring. They mark a class as a Spring-managed bean, but each has a different purpose.

1. **@Component** – A generic annotation used when a class does not belong to a specific layer.
2. **@Service** – Used for classes that contain business logic.
3. **@Repository** – Used for the persistence (data access) layer. It also provides automatic exception translation for database-related exceptions.
4. **@Controller** – Used in Spring MVC applications to handle HTTP requests and return view names.

**Interview Answer (Short)**

- **@Component** – Generic Spring bean.
- **@Service** – Used for the business logic layer.
- **@Repository** – Used for the data access layer and provides exception translation.
- **@Controller** – Used in the web layer to handle HTTP requests and return views.

**Note:** For REST APIs, use @RestController instead of @Controller. @RestController is equivalent to @Controller + @ResponseBody, so it returns data (such as JSON) directly in the HTTP response.

## 5. Difference between @Controller and @RestController?

**Answer:**

- **@Controller** = Used to return views (HTML/JSP/Thymeleaf).
- **@RestController** = Used to return REST API responses (JSON/XML/Text).

@RestController = @Controller + @ResponseBody

**Interview Answer (Short):**

@Controller is used for Spring MVC applications that return view pages. @RestController is a combination of @Controller and @ResponseBody, so all methods return data directly in the HTTP response, making it ideal for RESTful APIs.

## 7. Explain Dependency Injection.
**Answer:** Dependency Injection means objects are created by Spring IoC Container instead of manually using new.

Types:
1. Constructor Injection (Preferred)
2. Setter Injection
3. Field Injection (Not recommended)

**Interview Answer (Short):**

**Dependency Injection (DI)** is a design pattern in which the Spring Framework provides the required objects (dependencies) to a class instead of the class creating them itself.

## 9. What is Bean Scope in Spring?

**Answer:** Bean Scope in Spring defines the lifecycle and visibility of a Spring bean inside the application context. It determines how many instances of a bean are created and how long they live.

Spring provides different bean scopes to control the lifecycle of beans:

- **Singleton (Default)** – Only one instance of the bean is created per Spring container.
- **Prototype** – A new instance is created each time the bean is requested.
- **Request** – A new bean instance is created for each HTTP request (used in web applications).
- **Session** – A new bean instance is created for each HTTP session.
- **Application** – A single bean instance is created for the entire ServletContext (application-wide scope).

## 13. Difference between PUT and PATCH?

**Answer:** Both PUT and PATCH are HTTP methods used for updating resources in REST APIs, but they differ in how much data they update.

PUT:
- Updates entire object
- Idempotent

PATCH:
- Partial update
- Only modified fields

**Interview Answer (Short)**

PUT is used to fully update a resource and replaces the entire object, while PATCH is used for partial updates where only specific fields are modified. PUT requires the full request body, whereas PATCH only needs the fields that need to be updated.

## 14. What is @Transactional?

**Answer:** @Transactional is a Spring annotation used to define transaction management at the method or class level. It ensures that a set of database operations are executed within a single transaction, providing ACID properties (Atomicity, Consistency, Isolation, Durability).

**How it works:** When a method is marked with @Transactional:
1. Spring starts a transaction before method execution
2. All database operations inside the method are part of that transaction
3. If the method completes successfully → transaction is committed
4. If an exception occurs → transaction is rolled back

**Key Features**
- Ensures data consistency
- Supports rollback on runtime exceptions
- Can be applied at method or class level
- Integrates with Spring AOP (Aspect-Oriented Programming)

**Important Attributes**

     @Transactional(
     propagation = Propagation.REQUIRED, 
     isolation = Isolation.READ_COMMITTED,
     rollbackFor = Exception.class
     )
     public void saveEmployee(Employee emp) {
        employeeRepository.save(emp);
        salaryRepository.save(emp.getSalary());
     }

- Propagation → How transactions behave across methods
- Isolation → Controls visibility of data changes
- Rollback rules → Defines when rollback should occur

**Interview Answer (Short)**

**@Transactional** in Spring is used to manage database transactions. It ensures that all operations within a method are executed in a single transaction, and either all succeed (commit) or all fail (rollback). It helps maintain data consistency and supports ACID properties.

## 15.Propagation levels in Transaction?
**Answer:** Transaction propagation defines how a transaction behaves when a transactional method calls another transactional method.

- **REQUIRED** – Joins the existing transaction if one exists; otherwise, creates a new transaction. *(Default)*
- **REQUIRES_NEW** – Always creates a new transaction and suspends any existing transaction.
- **SUPPORTS** – Joins an existing transaction if available; otherwise, executes without a transaction.
- **MANDATORY** – Must run within an existing transaction; otherwise, an exception is thrown.
- **NEVER** – Executes without a transaction and throws an exception if a transaction already exists.
- **NESTED** – Executes within a nested transaction if a transaction exists; otherwise, behaves like `REQUIRED`.

**Most Commonly Used**
- **REQUIRED** – Default and most commonly used.
- **REQUIRES_NEW** – Used when a method must run in its own independent transaction (for example, writing audit logs).
- **SUPPORTS** – Used for methods that can run with or without a transaction, such as read-only operations.

**Interview Answer (Short)**
Transaction propagation defines how Spring manages transactions when one transactional method calls another. The default propagation is REQUIRED, which joins an existing transaction or creates a new one if none exists. Other propagation levels include REQUIRES_NEW, SUPPORTS, NOT_SUPPORTED, MANDATORY, NEVER, and NESTED, each controlling transaction behavior in different scenarios.

## 16. What are Isolation Levels in Spring Transactions?
**Answer:**
Isolation levels define how transactions are isolated from each other when multiple transactions execute concurrently. They control the visibility of changes made by one transaction to another and help prevent data inconsistency.

**Common Isolation Levels**

- **READ_UNCOMMITTED** – Allows reading uncommitted changes made by other transactions (dirty reads are possible).
- **READ_COMMITTED** – Only allows reading committed data, preventing dirty reads.
- **REPEATABLE_READ** – Ensures that repeated reads within the same transaction return the same data, preventing dirty and non-repeatable reads.
- **SERIALIZABLE** – The highest isolation level. Transactions are executed sequentially, preventing dirty reads, non-repeatable reads, and phantom reads, but with reduced concurrency.

## 18. What is Lazy Loading and Eager Loading?
1. **Lazy Loading:** The related entity is not loaded until it is actually accessed.
   
       @OneToMany(fetch = FetchType.LAZY)
       private List<Employee> employees;
   **Example**

       Department dept = departmentRepository.findById(1L).get();
       // Employees are loaded only when this is called
       dept.getEmployees();
2. **Eager Loading:** The related entity is loaded immediately along with the parent entity.

       @OneToMany(fetch = FetchType.EAGER)
       private List<Employee> employees;
       Department dept = departmentRepository.findById(1L).get();


   The Department and its employees are fetched together in the same operation.

   **When to Use**

**Lazy Loading**
- Better for large collections.
- Improves performance by loading data only when needed.
- Commonly used for @OneToMany and @ManyToMany relationships.
  
**Eager Loading**
- Suitable when related data is always required.
- Avoid using it for large collections, as it may impact performance.

**Interview Answer (Short)**

Lazy Loading loads related entities only when they are accessed, improving performance by avoiding unnecessary database queries. Eager Loading loads related entities immediately along with the parent entity. Lazy loading is generally preferred for better performance, while eager loading is useful when related data is always needed.

## 19. What is the N+1 Query Problem?

**Answer:**
The N+1 Query Problem is a performance issue in JPA/Hibernate where one query is executed to fetch the parent entities, followed by N additional queries to fetch the related entities for each parent record.
This results in N+1 database queries, which can significantly degrade performance.

**Suppose you have:**
- Department
- Employee (@OneToMany relationship)

       List<Department> departments = departmentRepository.findAll();

 - Hibernate executes:
   
       SELECT * FROM department;       -- 1 query

- When you access employees:
  
      for (Department dept : departments) {
           dept.getEmployees();
      }

- Hibernate executes:
  
       SELECT * FROM employee WHERE department_id = 1;
       SELECT * FROM employee WHERE department_id = 2;
       SELECT * FROM employee WHERE department_id = 3;

  If there are 10 departments, Hibernate executes:

- 1 query to fetch departments
- 10 queries to fetch employees
Total = 11 queries (N+1 Problem).

**How to Solve It**

- Use JOIN FETCH in JPQL

      @Query("SELECT d FROM Department d JOIN FETCH d.employees")
       List<Department> findAllDepartments();

- Use @EntityGraph to fetch related entities efficiently.
- Use DTO projections when only specific fields are needed.

**Why is it a Problem?**
- Increases the number of database queries.
- Slows down application performance.
- Creates unnecessary database load.

## 20. How do you improve Spring Boot performance?

**Answer:**

- **Connection Pool (HikariCP)** – Reuse database connections to reduce connection overhead and improve performance.
- **Caching** – Cache frequently accessed data to reduce database queries.
- **Pagination** – Retrieve data in smaller chunks instead of loading large datasets at once.
- **Async Processing** – Execute long-running tasks asynchronously using `@Async`.
- **Lazy Loading** – Load related entities only when needed to reduce unnecessary database access.
- **Query Optimization** – Write efficient SQL/JPQL queries and avoid unnecessary joins or N+1 query issues.
- **Batch Updates** – Perform bulk inserts and updates to reduce database round trips.
- **Proper Indexes** – Create indexes on frequently queried columns to speed up database operations.
- **Reduce Object Creation** – Minimize unnecessary object creation to reduce memory usage and garbage collection.
- **JVM Tuning** – Optimize heap size, garbage collection, and JVM parameters based on application requirements.

## 21. Explain Spring Boot Profiles?

**Answer:** Spring Boot Profiles allow you to use different configurations for different environments such as development, testing, and production.
Each profile can have its own configuration file, and Spring Boot loads the appropriate configuration based on the active profile.

**Profile Configuration Files**

     application.properties          // Common configuration
     application-dev.properties      // Development
     application-test.properties     // Testing
     application-prod.properties     // Production

**Activate a Profile**
In application.properties:

     spring.profiles.active=dev

## 23. What is Spring Security?

**Answer:** 
**Spring Security** is a powerful framework that provides authentication, authorization, and protection against common security threats for Spring applications.

**Features**

- **Authentication** – Verifies the identity of a user.
- **Authorization** – Controls access to resources based on user roles and permissions.
- **Password Encoding** – Securely stores passwords using hashing algorithms such as BCrypt.
- **JWT (JSON Web Token)** – Supports stateless authentication using tokens.
- **OAuth2** – Enables secure authentication and authorization with third-party providers (e.g., Google, GitHub).
- **CSRF Protection** – Protects applications against Cross-Site Request Forgery (CSRF) attacks.

## 25. Difference between Authentication and Authorization?

**Authentication:** - Who are you?

**Authorization** - What can you access?

## 26. What is CORS?

**Answer:** Cross-Origin Resource Sharing allows requests from different domains.

         @CrossOrigin(origins="http://localhost:3000")

## 27. What is HikariCP?

**Answer**
HikariCP is a high-performance JDBC connection pool used to manage database connections efficiently. It is the default connection pool in Spring Boot (from Spring Boot 2.x onward).

Instead of creating a new database connection for every request, HikariCP maintains a pool of reusable connections, improving performance and reducing database overhead.

**Why Use HikariCP?**
- High performance and low latency
- Fast database connection management
- Reduces the overhead of creating new connections
- Better throughput and lower memory usage
- Default connection pool in Spring Boot

          spring.datasource.hikari.maximum-pool-size=10
          spring.datasource.hikari.minimum-idle=5
          spring.datasource.hikari.connection-timeout=30000
          spring.datasource.hikari.idle-timeout=600000

**Advantages**
- Faster than many other connection pools
- Efficient resource utilization
- Reduces database connection overhead
- Improves application scalability and performance

## 28. Explain Microservices Architecture.

**Answer**

**Microservices Architecture** is a software design approach where an application is divided into small, independent services. Each service is responsible for a specific business capability and can be developed, deployed, and scaled independently.

**Features**

- **Independent Deployment** – Each microservice can be deployed without affecting other services.
- **Database per Service** – Each microservice manages its own database, ensuring loose coupling.
- **API Communication** – Services communicate with each other using APIs such as REST, gRPC, or messaging systems.
- **Scalability** – Individual services can be scaled independently based on demand.
- **Fault Isolation** – Failure in one service does not necessarily affect the entire application, improving system reliability.

## 29. How do Microservices communicate?

**Answer:** 
Microservices communicate with each other using synchronous or asynchronous communication mechanisms.

**Communication Methods**
- **REST APIs** – Synchronous communication over HTTP using RESTful endpoints.
- **gRPC** – High-performance RPC framework that uses Protocol Buffers for efficient communication.
- **Kafka** – Distributed event streaming platform for asynchronous, event-driven communication.
- **RabbitMQ** – Message broker used for reliable asynchronous messaging between services.
- **Event-Driven Messaging** – Services communicate by publishing and subscribing to events, enabling loose coupling and scalability.

## 30. What is API Gateway?

**Answer:** An **API Gateway** is a server that acts as a **single entry point** for all client requests in a microservices architecture. It routes requests to the appropriate microservices and handles common cross-cutting concerns.

**Responsibilities**

- **Routing** – Directs incoming requests to the appropriate microservice.
- **Authentication** – Verifies user identity before allowing access to services.
- **Rate Limiting** – Controls the number of requests from a client to prevent abuse and ensure system stability.
- **Logging** – Records requests and responses for monitoring, debugging, and auditing.
- **Load Balancing** – Distributes incoming traffic across multiple service instances to improve performance and availability.

## 31. What is Circuit Breaker?

**Answer:** A **Circuit Breaker** is a design pattern used in microservices to prevent repeated failures when a service is unavailable. It stops calling a failing service and allows it time to recover, improving system stability.

**Popular Implementation**

- Resilience4j

**States**

- **Closed** – Requests are allowed normally. If failures exceed a threshold, the circuit trips.
- **Open** – Requests are blocked and immediately fail to prevent further load on the failing service.
- **Half-Open** – A limited number of test requests are allowed to check if the service has recovered. If successful, the circuit closes again.

## 32. What is Spring Cloud Config?

**Answer:** **Spring Cloud Config** provides centralized configuration management for distributed systems and microservices. It allows externalizing configuration so that multiple services can share and manage settings from a single location.

**Benefits**
- **Common Configuration** – Central place to manage shared configuration for all microservices.
- **Dynamic Refresh** – Updates configuration without restarting services (using refresh endpoints).
- **Environment-Specific Properties** – Supports different configurations for dev, test, and production environments.

## 33. What is Service Discovery?

**Answer:** **Service Discovery** is a mechanism in microservices architecture where services automatically register themselves with a central registry and can discover other services without hardcoding their network locations.

**Examples**
- **Eureka** – A service registry developed by Netflix, commonly used in Spring Cloud for service discovery.
- **Consul** – A service mesh and discovery tool that provides service registration, health checking, and key-value storage.


## Frequently Asked Scenario-Based Questions (Spring Boot / Microservices)

### 1. A REST API suddenly becomes slow. How would you investigate it?

- Check application logs for errors or slow queries.
- Use APM tools (e.g., Spring Boot Actuator, Prometheus, Grafana, New Relic).
- Identify database bottlenecks (slow queries, missing indexes).
- Analyze thread dumps for blocking or thread pool exhaustion.
- Check external service latency (downstream APIs).
- Review caching effectiveness and hit ratio.

---

### 2. How would you optimize a Spring Boot application handling millions of requests?

- Use caching (Redis, in-memory cache).
- Implement connection pooling (HikariCP).
- Use pagination and avoid loading large datasets.
- Optimize database queries and indexing.
- Use asynchronous processing (`@Async`, messaging queues).
- Scale horizontally using load balancers.
- Tune JVM (heap size, GC tuning).

---

### 3. How do you manage transactions across multiple microservices?

- Avoid distributed transactions where possible.
- Use **Saga Pattern** (choreography or orchestration).
- Use event-driven architecture (Kafka/RabbitMQ).
- Implement compensating transactions for rollback scenarios.

---

### 4. How do you prevent duplicate requests from creating duplicate records?

- Use **idempotency keys** for API requests.
- Enforce unique constraints at the database level.
- Deduplicate messages in message queues.
- Implement request caching or token-based tracking.

---

### 5. How would you implement distributed locking?

- Use **Redis (Redisson)** for distributed locks.
- Use **ZooKeeper** or **etcd** for coordination.
- Use database-based locking (less scalable option).
- Ensure lock timeout to avoid deadlocks.

---

### 6. How do you version REST APIs?

- URI versioning: `/api/v1/users`
- Header versioning: `Accept: application/vnd.api.v1+json`
- Query parameter versioning: `?version=1`
- Maintain backward compatibility when possible.

---

### 7. How do you secure communication between microservices?

- Use HTTPS/TLS encryption.
- Use OAuth2 / JWT for authentication.
- Implement API Gateway security.
- Use mutual TLS (mTLS) for service-to-service authentication.
- Restrict network access using firewalls/service mesh.

---

### 8. How do you implement retry logic for external service failures?

- Use **Spring Retry** (`@Retryable`).
- Implement exponential backoff strategy.
- Use Resilience4j Retry module.
- Combine with Circuit Breaker to avoid cascading failures.

---

### 9. How would you debug a memory leak in a Spring Boot application?

- Analyze heap dumps using tools like VisualVM or Eclipse MAT.
- Monitor memory usage via Actuator metrics.
- Check for unclosed resources (connections, streams).
- Look for static collections growing indefinitely.
- Review caching policies (unbounded caches).

---

### 10. How do you achieve zero-downtime deployments?

- Use rolling deployments (Kubernetes, Load balancers).
- Use blue-green deployment strategy.
- Use canary releases for gradual rollout.
- Ensure backward-compatible database changes.
- Use health checks before routing traffic to new instances.
