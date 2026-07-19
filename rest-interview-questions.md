# Rest-interview questions

## 1. What is REST and how does it work?

**Answer:**

**REST (Representational State Transfer)** is an architectural style for designing web services. It uses the HTTP protocol to enable communication between clients and servers. In REST, resources are identified by URLs, and clients interact with these resources using standard HTTP methods such as **GET**, **POST**, **PUT**, and **DELETE**.

**Key REST Principles**

- **Client-Server Architecture** – The client and server are independent, allowing each to evolve separately.
- **Statelessness** – Each request contains all the information needed to process it. The server does not store client session state between requests.
- **Cacheable** – Responses can be cached to improve performance and reduce server load.
- **Uniform Interface** – Uses consistent, resource-based URLs and standard HTTP methods for communication.
- **Layered System** – Clients do not need to know the internal architecture or implementation details of the server.

**Common HTTP Methods**

| Method | Purpose |
|---------|---------|
| **GET** | Retrieve data |
| **POST** | Create a new resource |
| **PUT** | Update an existing resource |
| **DELETE** | Remove a resource |

**Example**

```
GET /users/101
```

This request retrieves the user with ID **101** from the server.

**Conclusion**

REST is a lightweight, scalable, and widely adopted architectural style for building web services. Its stateless nature, standard HTTP methods, and resource-based design make it ideal for developing modern web and mobile APIs.


## 2. Difference between REST and SOAP?

**Answer:**

| Feature | REST | SOAP |
|---------|------|------|
| **Type** | Architectural style | Protocol |
| **Data Format** | Uses HTTP with JSON, XML, or other formats | Uses XML only |
| **Performance** | Lightweight and faster | More heavyweight with higher processing overhead |
| **Ease of Use** | Simple and easy to use with web and mobile applications | More complex and commonly used in enterprise legacy systems |
| **Contract** | No strict contract by default | Uses a WSDL (Web Services Description Language) contract |

**Key Differences**

- **REST** is an architectural style that commonly uses HTTP and JSON, making it lightweight and easy to implement.
- **SOAP** is a protocol that relies on XML and provides built-in standards for security, reliability, and transactions.
- **REST** offers better performance due to smaller payloads, while **SOAP** has more processing overhead because of XML.
- **REST** is widely used for modern web and mobile APIs, whereas **SOAP** is commonly found in enterprise and legacy systems.

**Conclusion**

- Use **REST** for modern, lightweight, and high-performance web services.
- Use **SOAP** when applications require strict contracts, advanced security, reliable messaging, or transactional support.


## 3. Explain HTTP methods used in REST APIs.

**Answer:**

REST APIs use standard HTTP methods to perform operations on resources.

| HTTP Method | Purpose | Example |
|-------------|---------|---------|
| **GET** | Retrieve one or more resources | `GET /users` |
| **POST** | Create a new resource | `POST /users` |
| **PUT** | Replace an entire existing resource | `PUT /users/10` |
| **PATCH** | Partially update an existing resource | `PATCH /users/10` |
| **DELETE** | Remove a resource | `DELETE /users/10` |

**HTTP Methods Explained**

1. **GET**
- Retrieves data from the server.
- Does not modify the resource.

**Example:**
```http
GET /users
```

2. **POST**
- Creates a new resource on the server.
- Sends data in the request body.

**Example:**
```http
POST /users
```

3. **PUT**
- Replaces an entire existing resource.
- If the resource exists, it is completely updated.

**Example:**
```http
PUT /users/10
```

4. **PATCH**
- Updates only specific fields of an existing resource.
- More efficient than **PUT** when only a few fields need to change.

**Example:**
```http
PATCH /users/10
```

5. **DELETE**
- Removes a resource from the server.

**Example:**
```http
DELETE /users/10
```

**Conclusion** 
These methods provide a standard and consistent way to perform CRUD (Create, Read, Update, Delete) operations on REST resources.


## 4. What is idempotency in REST?

**Answer:** 
An operation is **idempotent** if making the same request multiple times produces the same result as making it once. Repeated requests do not cause additional side effects beyond the initial request.

**Examples:**

✅ **Idempotent**

```http
PUT /accounts/101
```
Updating the account with the same data multiple times results in the same resource state.

❌ **Not Always Idempotent**

```http
POST /orders
```
Calling this endpoint multiple times may create multiple orders, resulting in different outcomes.


## 6. Explain HTTP status codes commonly used in REST.

**Answer:**

HTTP status codes indicate the outcome of a client's request to the server.

✅ **Success (2xx)**

- **200 OK** – The request was successful (commonly used for `GET`, `PUT`, or `PATCH`).
- **201 Created** – A new resource was successfully created (typically returned after a `POST` request).
- **204 No Content** – The request was successful, but there is no response body (commonly used for `DELETE` or successful updates).

⚠️ **Client Errors (4xx)**

- **400 Bad Request** – The request contains invalid or malformed data.
- **401 Unauthorized** – Authentication is required or the provided credentials are invalid.
- **403 Forbidden** – The user is authenticated but does not have permission to access the resource.
- **404 Not Found** – The requested resource does not exist.
- **409 Conflict** – The request conflicts with the current state of the resource (for example, duplicate data or version conflicts).

❌ **Server Errors (5xx)**

- **500 Internal Server Error** – An unexpected error occurred on the server.
- **503 Service Unavailable** – The server is temporarily unavailable, often due to maintenance or overload.


## 7. How do you handle errors in REST APIs?

**Answer:** 
In REST APIs, errors are handled by returning appropriate HTTP status codes along with a meaningful error response body. The API should provide enough information for the client to understand what went wrong while avoiding exposure of sensitive internal details.

Common HTTP status codes used for errors:

- **400 Bad Request** – Invalid request data or validation errors.
- **401 Unauthorized** – Authentication is required or authentication failed.
- **403 Forbidden** – User does not have permission to access the resource.
- **404 Not Found** – The requested resource does not exist.
- **409 Conflict** – The request conflicts with the current state of the resource.
- **500 Internal Server Error** – Unexpected server-side errors.

A standard error response format is usually returned, for example:

```json
{
  "timestamp": "2026-07-12T10:30:00",
  "status": 400,
  "error": "Bad Request",
  "message": "Invalid user details",
  "path": "/users"
}
```

In real applications, errors are handled using:

- Global exception handling (for example, exception handlers in backend frameworks).
- Input validation before processing requests.
- Proper logging for debugging and monitoring.
- Custom error messages with consistent response formats.
- Avoiding sensitive information in error responses.

**Short Interview Version:** 
I handle REST API errors by using proper HTTP status codes, returning a consistent error response structure, validating requests, handling exceptions globally, and logging errors for troubleshooting. This helps clients understand failures while keeping the API secure.


## 9. How do you secure REST APIs?

**Answer:** 
REST APIs are secured by implementing authentication, authorization, and additional security practices to protect data and resources.

**Authentication** 
Authentication verifies the identity of the user or client.

Common authentication mechanisms:

- **OAuth 2.0** – Industry-standard protocol for secure authorization and delegated access.
- **JWT (JSON Web Token)** – A token-based authentication mechanism where the client sends a signed token with each request.
- **API Keys** – Used to identify and authenticate API clients.
- **Basic Authentication** – Simple username and password-based authentication (less preferred for modern applications).

**Authorization** 
Authorization determines what an authenticated user is allowed to access.

Common approaches:

- **Role-Based Access Control (RBAC)** – Access is controlled based on user roles such as ADMIN, USER, or MANAGER.
- **Permission-Based Access** – Access is granted based on specific permissions.

**Additional Security Practices**

- **HTTPS/TLS** – Encrypts communication between client and server.
- **Input Validation** – Prevents invalid data and protects against attacks like SQL injection.
- **Rate Limiting** – Controls the number of requests to prevent abuse and denial-of-service attacks.
- **CORS Configuration** – Controls which client applications can access the API.
- **Avoid Sensitive Data in URLs** – Do not expose passwords, tokens, or confidential information in query parameters.
- **Token Expiration and Refresh Mechanisms** – Limits token lifetime and provides secure token renewal.


## 13. How do you improve REST API performance?

**Answer:** 
REST API performance can be improved by reducing response time, optimizing resource usage, and efficiently handling client requests.

Common approaches:
- **Enable caching using HTTP headers** – Store frequently accessed data to reduce repeated processing and database calls.
- **Use pagination** – Return data in smaller chunks instead of loading large datasets at once.
- **Compress responses (gzip)** – Reduce response size and improve network transfer speed.
- **Optimize database queries** – Avoid unnecessary database calls and improve query performance.
- **Add database indexes** – Speed up searches and data retrieval operations.
- **Use asynchronous processing for long-running tasks** – Process time-consuming operations in the background instead of blocking API requests.
- **Implement rate limiting** – Control the number of requests from clients to prevent overload.
- **Use load balancing** – Distribute incoming requests across multiple servers to improve scalability and availability.
- **Reduce unnecessary data using DTOs (Data Transfer Objects)** – Send only the required fields instead of returning complete database objects.

## 14. Describe the role of HTTP headers in RESTful web services.

**Answer:**
HTTP headers play a crucial role in RESTful web services by providing meta-information about the request or response. They can include details such as content type, content length, authentication credentials, caching directives, and more.

For example, the Content-Type header indicates the media type of the resource (e.g., JSON, XML), while the Authorization header is used to pass authentication credentials. Headers like Cache-Control and ETag help manage caching and resource versioning.

## 15. How do you version a REST API, and why is it necessary?
**Answer:** 
Versioning is necessary to manage changes without disrupting existing clients. It allows developers to introduce new features and improvements while maintaining support for older versions.

- **URI Versioning:** Including the version number in the URI (e.g., /api/v1/resource).
- **Header Versioning:** Specifying the version number in a custom HTTP header (e.g., X-API-Version: 1).
- **Query Parameter Versioning:** Adding the version number as a query parameter (e.g., /api/resource?version=1).

## 16. How would you handle caching in a REST API to improve performance?
**Answer:**
- **Client-side caching:** Using HTTP headers like Cache-Control and ETag to allow clients to cache responses locally
- **Server-side caching:** Implementing caching layers on the server to store frequently accessed data
- **Content Delivery Networks (CDNs):** Utilizing CDNs to cache and serve static resources closer to the client

## 17. What is the difference between URI and URL in the context of REST APIs?
**Answer:** 
In REST APIs, URI (Uniform Resource Identifier) and URL (Uniform Resource Locator) are related concepts, but they are not the same.

Key Points:
- **URI** = Identifies a resource.
- **URL** = Locates and provides the method to access a resource.

Every URL is a URI, but not every URI is a URL.

Example:
- **URI**: /products/25
- **URL**: https://shop.example.com/products/25
  
**Conclusion:** In REST APIs, the term URI is commonly used when referring to resource identifiers, while URL is used when referring to the complete web address that clients use to access those resources.

## 18. What is the difference between statelessness and statefulness in the context of REST APIs?
**Answer:**
- **Statelessness** is a key principle of REST APIs, meaning that each request from a client to a server must contain all the information needed to understand and process the request. The server doesn't store any client context between requests. Each request is treated as an independent transaction, unrelated to any previous requests.
- **Statefulness**, on the other hand, involves the server keeping track of the client's state across multiple requests. This might involve storing session information or remembering the client's previous actions. Statefulness means the server maintains client session information across multiple requests, making interactions dependent on stored state but increasing the complexity of scaling and session management.


## 19. How do you handle global exceptions?
**Answer:** 

Using @ControllerAdvice and @ExceptionHandler to provide consistent error responses across all APIs.

## 20. What is the difference between 401 and 403?
**Answer:**

- **401:** User is not authenticated.
- **403:** User is authenticated but does not have permission.

## 21. What is ResponseEntity?
**Answer:** 
ResponseEntity is a Spring Framework class used to represent the complete HTTP response in a REST API. It allows us to control the response body, HTTP status code, and HTTP headers that are sent back to the client.

```java
return ResponseEntity
       .status(HttpStatus.CREATED)
       .body(employee);
```

For example, instead of always returning a default 200 OK response, we can return 201 Created, 404 Not Found, or 500 Internal Server Error based on the request outcome.

```java
@GetMapping("/user/{id}")
public ResponseEntity<String> getUser(@PathVariable int id) {
    if (id == 1) {
        return ResponseEntity.ok("User Found");
    } else {
        return ResponseEntity.status(HttpStatus.NOT_FOUND)
                             .body("User Not Found");
    }
}
```

Key points
- It provides full control over the HTTP response.
- It allows us to set status codes, headers, and response body.
- It is commonly used in Spring Boot REST APIs to return meaningful responses to clients.
- It improves API design by making responses more RESTful and flexible.

## 22. How do you validate REST request?
**Answer:** 
REST requests are validated in Spring Boot using Bean Validation (JSR-380/Jakarta Validation) with annotations such as @NotNull, @NotBlank, @Size, @Email, and @Min. The @Valid annotation is used in the controller to trigger validation.

```java
public class User {
    @NotBlank
    private String name;

    @Email
    private String email;
}

@PostMapping("/users")
public ResponseEntity<String> createUser(@Valid @RequestBody User user) {
    return ResponseEntity.ok("User created");
}
```
Key Points:

- Use validation annotations (@NotNull, @NotBlank, @Size, etc.).
- Use @Valid on the request body or request parameters.
- Return appropriate error responses (e.g., 400 Bad Request) when validation fails.

## 23. Difference between @Controller and @RestController?
**Answer:**

- **@Controller** = Used to return views (HTML/JSP/Thymeleaf).
- **@RestController** = Used to return REST API responses (JSON/XML/Text).
- **@RestController** = @Controller + @ResponseBody

**Interview Answer (Short):**

@Controller is used for Spring MVC applications that return view pages. @RestController is a combination of @Controller and @ResponseBody, so all methods return data directly in the HTTP response, making it ideal for RESTful APIs.

## 24. What is @ResponseBody?
**Answer:** 
@ResponseBody is a Spring annotation.
- It converts Java objects into JSON/XML format.
- It is mainly used in REST APIs to return data instead of returning a view.
- Automatically included in: @RestController

**Interview Answer (Short):** 
@ResponseBody converts a Java object into JSON (or other formats) and sends it directly as the HTTP response. It is automatically included in @RestController, so we don't need to add it separately.
