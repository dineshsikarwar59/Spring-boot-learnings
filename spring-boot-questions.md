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
