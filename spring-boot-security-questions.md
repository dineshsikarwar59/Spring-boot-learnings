# Spring-boot-security-interview questions

## 1. What is Spring Security?

**Answer:** Spring Security is a powerful authentication and authorization framework for Spring applications.
It helps secure applications by protecting resources and controlling user access.

It provides features such as:

- **Authentication** – Verifies the identity of a user.
- **Authorization** – Determines what an authenticated user is allowed to access.
- **Password Encryption** – Supports secure password hashing (for example, BCrypt).
- **Protection against common attacks** – Such as CSRF, session fixation, and clickjacking.
- **JWT and OAuth2 support** – For securing REST APIs and integrating with external identity providers.
- **Role-based access control** – Restricts access based on user roles (e.g., ADMIN, USER).

**Interview Answer (Short)** 
Spring Security is a framework used to secure Spring applications. It provides authentication (verifying user identity), authorization (controlling access to resources), password encryption, protection against common web attacks, and support for JWT, OAuth2, and role-based access control

## 2. Explain Authentication vs Authorization.

**Answer:** Authentication and Authorization are two key concepts in Spring Security.

**Authentication:** Authentication is the process of verifying a user's identity.

**Example:**
- User enters a username and password.
- Spring Security validates the credentials.
- If valid, the user is authenticated.

**Authorization:** Authorization is the process of determining what an authenticated user can access.

**Example**
- ADMIN → Can access /admin
- USER → Can access /profile
- GUEST → Can access only public endpoints

**Real-World Example**
- **Authentication:** Logging into your bank account using your username and password.
- **Authorization:** After logging in, only account owners can view their account details, while bank employees may have additional permissions.

**Interview Answer (Short)** 
Authentication is the process of verifying a user's identity using credentials such as a username and password. Authorization determines what an authenticated 

## 3. Explain the Spring Security Architecture.

**Answer:**
Spring Security processes every request through a security filter chain. The request is authenticated, authorized, and then either allowed to access the resource or rejected.

**Spring Security Flow**

```text
Client Request
      │
      ▼
Security Filter Chain
      │
      ▼
AuthenticationManager
      │
      ▼
AuthenticationProvider
      │
      ▼
UserDetailsService
      │
      ▼
Database / In-Memory User Store
      │
      ▼
Authentication Success
      │
      ▼
Authorization (Roles/Permissions)
      │
      ▼
Controller
```

### Flow Explanation

1. **Client Request** – The client sends an HTTP request to the application.
2. **Security Filter Chain** – Intercepts the request and applies security filters.
3. **AuthenticationManager** – Delegates authentication to an appropriate `AuthenticationProvider`.
4. **AuthenticationProvider** – Validates the user's credentials.
5. **UserDetailsService** – Loads user details (username, password, roles) from the data source.
6. **Database / In-Memory User Store** – Stores user credentials and authorities.
7. **Authentication Success** – If credentials are valid, the user is authenticated.
8. **Authorization** – Spring Security checks whether the authenticated user has the required roles or permissions.
9. **Controller** – If authorization succeeds, the request reaches the controller; otherwise, access is denied.

## Main Components of Spring Security

### 1. Security Filter Chain
- The first component that intercepts every HTTP request.
- Applies security checks such as authentication and authorization before the request reaches the application.

### 2. AuthenticationManager
- Receives the authentication request.
- Delegates the authentication process to an appropriate `AuthenticationProvider`.

### 3. AuthenticationProvider
- Validates the user's credentials.
- Uses `UserDetailsService` to load user information.
- Verifies the password using a `PasswordEncoder`.

### 4. UserDetailsService
- Loads user details from a database, LDAP, or an in-memory user store.
- Returns a `UserDetails` object containing the username, password, and authorities.

### 5. PasswordEncoder
- Compares the entered password with the stored encrypted password.
- Commonly uses **BCryptPasswordEncoder** for secure password hashing.

### 6. Authorization
- Performed after successful authentication.
- Spring Security checks the user's roles or authorities to determine whether access is allowed.
- If authorized, the request proceeds to the controller; otherwise, access is denied.

user is allowed to access based on roles or permissions. Authentication happens first, followed by authorization.

**Interview Answer (Short)**
Spring Security processes requests through the Security Filter Chain. The request is passed to the AuthenticationManager, which delegates authentication to an AuthenticationProvider. The provider uses UserDetailsService to load user information and PasswordEncoder to verify the password. After successful authentication, Spring Security performs authorization by checking the user's roles or permissions before allowing access to the requested resource.


## 4. What is SecurityFilterChain?

**Answer:**
SecurityFilterChain is the core component of Spring Security that intercepts every incoming HTTP request and applies security rules such as authentication and authorization.

**It determines:**
- Which URLs are secured
- Who can access them
- Which authentication mechanism to use (Form Login, JWT, OAuth2, etc.)

From Spring Security 5.7+, SecurityFilterChain is the recommended approach instead of extending WebSecurityConfigurerAdapter.

    @Configuration
    @EnableWebSecurity
    public class SecurityConfig {

          @Bean
          public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
          
              http
                  .authorizeHttpRequests(auth -> auth
                      .requestMatchers("/public/**").permitAll()
                      .requestMatchers("/admin/**").hasRole("ADMIN")
                      .anyRequest().authenticated()
                  )
                  .formLogin();
                  
              return http.build();
            }
        }

**Advantages**
- Centralized security configuration
- Replaces the deprecated WebSecurityConfigurerAdapter
- Supports Form Login, JWT, OAuth2, and custom authentication
- Flexible URL-based security configuration

**Interview Answer (Short)**
SecurityFilterChain is the main component in Spring Security that intercepts every HTTP request and applies security filters such as authentication, authorization, CSRF protection, and session management. It is configured using HttpSecurity and is the recommended way to configure Spring Security in modern Spring Boot applications.

## 5. Why was WebSecurityConfigurerAdapter removed?

**Answer:**
Because Spring Security moved toward:
- Functional configuration
- Bean-based configuration
- Better customization
- Reduced inheritance

## 6. What is UserDetailsService?

**Answer:**
UserDetailsService is a Spring Security interface used to load user details during authentication.

When a user attempts to log in, Spring Security calls the loadUserByUsername() method to retrieve the user's information (such as username, password, and roles) from a database, LDAP, or another user store.

**UserDetailsService Interface**

      public interface UserDetailsService {
          UserDetails loadUserByUsername(String username)
                  throws UsernameNotFoundException;
      }
**Interview Answer (Short)**
UserDetailsService is a Spring Security interface used to load user details during authentication. It defines the loadUserByUsername() method, which retrieves the user's username, password, and roles from a database or another user store and returns them as a UserDetails object for authentication.

## 7. What is UserDetails?

**Answer:**
UserDetails is a Spring Security interface that represents the authenticated user's information.

It contains details such as:
- Username
- Password
- Roles/Authorities
- Account status (expired, locked, enabled, etc.)

Spring Security uses the UserDetails object during authentication and authorization.

**Interview Answer (Short)**

UserDetails is a Spring Security interface that represents an authenticated user. It contains the user's username, password, roles (authorities), and account status (such as whether the account is locked or enabled). It is returned by UserDetailsService and used by Spring Security during authentication and authorization.

## 8. Explain PasswordEncoder

**Answer:** PasswordEncoder is a Spring Security interface used to hash (encrypt) passwords before storing them and to verify passwords during authentication.

Instead of storing plain-text passwords, Spring Security stores hashed passwords, making them much more secure.

       @Bean
       public PasswordEncoder passwordEncoder() {
           return new BCryptPasswordEncoder();
       }

**Why Use PasswordEncoder?**
- Passwords are stored as hashed values, not plain text.
- Protects user credentials if the database is compromised.
- Supports secure hashing algorithms like BCrypt.
- Prevents direct recovery of the original password.

**Interview Answer (Short)**

PasswordEncoder is a Spring Security interface used to hash passwords before storing them and to verify passwords during login. The most commonly used implementation is BCryptPasswordEncoder, which securely hashes passwords and compares the entered password with the stored hash during authentication.

## 9. Why BCrypt?

**Answer:** **BCrypt** is a password hashing algorithm widely used in Spring Security to securely store user passwords. It is designed to make password cracking difficult and protect against common attacks.

**Advantages**

- **Salt Included** – Automatically generates and stores a unique salt for each password, preventing rainbow table attacks.
- **Slow Hashing** – Uses an adjustable work factor (cost factor) that intentionally slows down hashing, making brute-force attacks more difficult.
- **Resistant to Brute Force** – The computationally expensive hashing process significantly increases the time required to guess passwords.
- **One-Way Hashing** – Passwords cannot be decrypted. During login, the entered password is hashed and compared with the stored hash.


## 10. Explain JWT Authentication
**Answer:**

JWT (JSON Web Token) is a token-based authentication mechanism used to secure REST APIs. After a user logs in successfully, the server generates a JWT and sends it to the client. The client includes this token in subsequent requests, and the server validates it before allowing access.

Since JWT is stateless, the server does not need to store session information.

**JWT Structure**

A JWT consists of three parts:

        Header.Payload.Signature
        
- Header – Contains the token type and signing algorithm.
- Payload – Contains claims such as username, roles, and expiration time.
- Signature – Ensures the token has not been modified.

Example:

       xxxxx.yyyyy.zzzzz
       
**Advantages**
- Stateless authentication (no server-side session)
- Scalable for distributed systems and microservices
- Faster than session-based authentication
- Suitable for REST APIs
- Can include user roles and other claims

**Interview Answer (Short)**

JWT (JSON Web Token) is a stateless authentication mechanism used to secure REST APIs. After successful login, the server generates a JWT and returns it to the client. The client sends the token in the Authorization: Bearer <token> header with each request. The server validates the token, and if it is valid, grants access to protected resources. JWT consists of three parts: Header, Payload, and Signature.

## 11. Why JWT instead of Session?

**Answer:**
JWT (JSON Web Token) is often preferred over traditional session-based authentication, especially in microservices and distributed systems.

**JWT (Advantages)**
- **Stateless** – No server-side session storage is required.
- **No Server Session** – All user information is stored in the token itself.
- **Better for Microservices** – Easily works across multiple services without shared session storage.
- **Better Scalability** – Since the server does not store session data, it can handle more requests efficiently.

**Session-Based Authentication (Limitations)**
- **Stored in Server** – Session data is stored on the backend.
- **Memory Consumption** – Increases server memory usage as users grow.
- **Difficult in Distributed Systems** – Requires session sharing mechanisms like Redis or sticky sessions across multiple servers.

## 12. Explain JWT Components.

Header:

       Algorithm
       Type

Payload:

       username
       role
       expiry
       claims

Signature:

     Secret Key + Header + Payload

## 13. How do you validate JWT?

**Answer:** 
JWT validation ensures that the token is authentic, not tampered with, and still valid before granting access to secured resources.

**Steps to Validate JWT**

- **Check Signature** – Verify the token signature using the secret key or public key to ensure it has not been modified.
- **Check Expiry** – Validate the `exp` (expiration) claim to ensure the token has not expired.
- **Verify Issuer** – Ensure the token was issued by a trusted authority (`iss` claim).
- **Verify Audience** – Confirm the token is intended for your application (`aud` claim).
- **Extract Claims** – Retrieve user details (like username, roles, permissions) from the token payload for authorization.

If valid

        Authenticated

Else

       401 Unauthorized


## 14. What is OncePerRequestFilter?

**Answer:**
OncePerRequestFilter is a Spring Security filter base class that ensures a filter is executed only once per HTTP request, even if the request is forwarded or included multiple times within the application.

It is commonly used in JWT authentication, logging, and request preprocessing.

**Why it is used?**

In a servlet environment, a request may pass through the filter chain multiple times due to:
- Forwarding
- Including other resources
- Dispatcher behavior

OncePerRequestFilter ensures the filter logic runs only once per request lifecycle.
```java
     public class JwtAuthFilter extends OncePerRequestFilter {

         @Override
         protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain)
                                    throws ServletException, IOException {

               String token = request.getHeader("Authorization");

               if (token != null && token.startsWith("Bearer ")) {
                   token = token.substring(7);
                   // validate token and set authentication
               }

              filterChain.doFilter(request, response);
             }
        }
```

**Interview Answer (Short)**

OncePerRequestFilter is a Spring Security base filter class that ensures a filter is executed only once per HTTP request, even if the request is forwarded or dispatched multiple times. It is commonly used in JWT authentication and request preprocessing to avoid duplicate filter execution.

## 15. Explain OAuth2

**Answer:** 

OAuth 2.0 is an authorization framework that allows a user to grant a third-party application limited access to their resources without sharing their password.
It is commonly used for social login (Google, GitHub, Facebook) and secure API access.

**Note:** OAuth 2.0 is for authorization. It is often combined with OpenID Connect (OIDC) to provide authentication.

**Example**

A user clicks "Sign in with Google":
- User is redirected to Google.
- User logs in and grants permission.
- Google returns an authorization code.
- The application exchanges the code for an access token.
- The access token is used to access the user's profile or other permitted resources.

**Advantages**
- No need to share user passwords with third-party applications.
- Secure access using access tokens.
- Supports Single Sign-On (SSO).
- Widely used for REST APIs and social login.

**Interview Answer (Short)**

OAuth 2.0 is an authorization framework that allows users to grant third-party applications limited access to their resources without sharing their passwords. It works by issuing access tokens after user consent. OAuth2 is widely used for social login and securing REST APIs, and is commonly combined with OpenID Connect (OIDC) for authentication.


## 16. Difference between OAuth2 and JWT

**Answer:**

OAuth2 and JWT are related but serve different purposes.

- **OAuth2** is an authorization framework that defines how access is granted.
- **JWT (JSON Web Token)** is a token format used to securely transmit information between parties.

A common misconception is that they are alternatives. In practice, OAuth2 often uses JWT as the access token format, although OAuth2 can also use opaque tokens.


## 17. What is a Refresh Token?

**Answer:**

A Refresh Token is a long-lived token used to obtain a new Access Token when the current Access Token expires, without requiring the user to log in again.
This improves both security and user experience.

**How it Works**

1. User logs in successfully.
2. **Server issues two tokens:**
   - **Access Token** – Short-lived token used to access protected APIs.
   - **Refresh Token** – Long-lived token used to obtain a new Access Token.
3. Client uses the Access Token to access protected APIs.
4. When the Access Token expires, the client sends the Refresh Token to the server.
5. If the Refresh Token is valid, the server issues a new Access Token.


## 18. Explain CSRF

**Answer:** 
CSRF (Cross-Site Request Forgery) is a web security attack in which a malicious website tricks a logged-in user into sending an unwanted request to another website where they are already authenticated.

The browser automatically includes the user's session cookies, causing the server to treat the request as legitimate.

**Example**
1. A user logs into their **banking application**.
2. Without logging out, the user visits a **malicious website**.
3. The malicious site silently submits a hidden request such as:

```http
POST /transfer
amount=10000
to=attacker
```

4. The browser automatically includes the user's **session cookie** with the request.
5. If **CSRF protection is not enabled**, the banking application treats the request as legitimate and processes the money transfer.

**How Spring Security Prevents CSRF**

Spring Security protects against CSRF by generating a CSRF token.

- The server generates a unique CSRF token.
- The client sends the token with each state-changing request (POST, PUT, PATCH, DELETE).
- The server validates the token before processing the request.
- If the token is missing or invalid, the request is rejected.

**Example**
```java
     @Bean
     SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {

           http
               .csrf(csrf -> csrf.disable()); // Commonly disabled for stateless JWT APIs

           return http.build();
     }
```
**Note:** For session-based web applications, CSRF protection should generally remain enabled. It is commonly disabled only for stateless REST APIs that use JWT or another token-based authentication mechanism instead of cookies.

**Advantages of CSRF Protection**
- Prevents unauthorized actions on behalf of authenticated users.
- Protects against forged requests.
- Enabled by default in Spring Security for session-based applications.

**Interview Answer (Short)**

CSRF (Cross-Site Request Forgery) is an attack where a malicious website tricks an authenticated user into performing unwanted actions on another website. Spring Security prevents CSRF by using CSRF tokens, which are validated on state-changing requests. For session-based applications, CSRF protection should remain enabled, while it is commonly disabled for stateless JWT-based REST APIs.


## 20. What is CORS?

**Answer:** 
CORS (Cross-Origin Resource Sharing) is a browser security mechanism that allows or restricts web applications from making requests to a different origin (domain, protocol, or port) than the one from which the application was loaded.

By default, browsers block cross-origin requests due to the Same-Origin Policy. CORS allows the server to explicitly specify which origins are permitted.

**Example**

Frontend:

      http://localhost:3000

Backend:

      http://localhost:8080

Since the ports are different, these are different origins. Without CORS configuration, the browser blocks the request.

**Enable CORS in Spring Boot**

Using @CrossOrigin:
```java
          @RestController
          @CrossOrigin(origins = "http://localhost:3000")
          public class EmployeeController {

             @GetMapping("/employees")
             public List<Employee> getEmployees() {
                 return employeeService.findAll();
             }
         }
```
Or configure it globally:

```java
         @Configuration
         public class CorsConfig implements WebMvcConfigurer {

             @Override
             public void addCorsMappings(CorsRegistry registry) {
                 registry.addMapping("/**")
                           .allowedOrigins("http://localhost:3000")
                           .allowedMethods("GET", "POST", "PUT", "DELETE");
             }
         }
```

**Advantages**
- Allows secure communication between different origins.
- Enables frontend and backend applications hosted on different domains or ports to communicate.
- Prevents unauthorized cross-origin access.

**Interview Answer (Short):** 
CORS (Cross-Origin Resource Sharing) is a browser security feature that allows or restricts requests from one origin to another. By default, browsers block cross-origin requests due to the Same-Origin Policy. In Spring Boot, CORS can be enabled using @CrossOrigin or by configuring it globally with WebMvcConfigurer.


## 21. Explain Method Level Security

**Answer:** Method Level Security is a Spring Security feature that allows you to secure individual methods based on user roles or permissions, instead of securing only URLs.

It is commonly implemented using annotations such as:

- **@PreAuthorize** – Checks authorization **before** the method is executed using Spring Expression Language (SpEL).
- **@PostAuthorize** – Checks authorization **after** the method executes, allowing access based on the returned object.
- **@Secured** – Restricts access based on one or more specified roles.
- **@RolesAllowed** – A JSR-250 annotation used to allow access only to users with specified roles.

**Enable Method Security**

```java
@Configuration
@EnableMethodSecurity
public class SecurityConfig {
}
```
**Example** using @PreAuthorize
```java
@Service
public class EmployeeService {

    @PreAuthorize("hasRole('ADMIN')")
    public void deleteEmployee(Long id) {
        // delete logic
    }
}
```

**Other Annotations**

@Secured

```java
@Secured("ROLE_ADMIN")
public void deleteEmployee(Long id) {
}
```
@RolesAllowed
```java
@RolesAllowed("ADMIN")
public void deleteEmployee(Long id) {
}
```
@PostAuthorize

Checks authorization after the method executes.
```java
@PostAuthorize("returnObject.username == authentication.name")
public User getUser(Long id) {
    return userRepository.findById(id).get();
}
```

**Advantages**
- Secures business logic at the service layer.
- Provides fine-grained access control.
- Reduces dependency on URL-based security.
- Supports Spring Expression Language (SpEL) with @PreAuthorize and @PostAuthorize.

**Interview Answer (Short)** 
Method Level Security allows you to secure individual methods using annotations such as **@PreAuthorize**, **@PostAuthorize**, **@Secured**, and **@RolesAllowed**. It provides fine-grained access control based on user roles or permissions. To enable it, use **@EnableMethodSecurity** in the security configuration.



## 22. Difference between hasRole() and hasAuthority()

**Answer:** 
Both hasRole() and hasAuthority() are used in Spring Security to control access, but they differ in how they interpret roles and authorities.

**Example**

Using hasRole():
```java
@PreAuthorize("hasRole('ADMIN')")
public void deleteEmployee() {
}
```
Using hasAuthority():
```java
@PreAuthorize("hasAuthority('ROLE_ADMIN')")
public void deleteEmployee() {
}
```
Or for a custom permission:
```java
@PreAuthorize("hasAuthority('READ_PRIVILEGE')")
public List<Employee> getEmployees() {
}
```
**When to Use**
- **hasRole()** → When your application uses roles such as ADMIN, USER, or MANAGER.
- **hasAuthority()** → When you need fine-grained permissions such as READ_PRIVILEGE, WRITE_PRIVILEGE, or DELETE_PRIVILEGE.

**Interview Answer (Short)**

hasRole() is used to check roles and automatically prefixes the value with ROLE_. For example, hasRole("ADMIN") checks for ROLE_ADMIN. hasAuthority() checks the exact authority or permission without adding any prefix, making it suitable for both roles and fine-grained permissions like READ_PRIVILEGE or WRITE_PRIVILEGE.


## 23. What is AuthenticationManager?

**Answer:**

AuthenticationManager is the main interface in Spring Security responsible for authenticating users.

It receives an authentication request (such as a username and password), delegates the authentication process to an appropriate AuthenticationProvider, and returns an authenticated Authentication object if the credentials are valid.
```text
User Login
     │
     ▼
AuthenticationManager
     │
     ▼
AuthenticationProvider
     │
     ▼
UserDetailsService
     │
     ▼
Database / In-Memory User Store
     │
     ▼
PasswordEncoder
     │
     ▼
Authentication Success
```

**Example**
```java
@Autowired
private AuthenticationManager authenticationManager;

public void authenticate(String username, String password) {

    Authentication authentication =
        authenticationManager.authenticate(
            new UsernamePasswordAuthenticationToken(username, password)
        );

    // Authentication successful
}
```

**How it Works**
- User submits username and password.
- AuthenticationManager receives the authentication request.
- It delegates authentication to an AuthenticationProvider.
- The AuthenticationProvider uses UserDetailsService to load the user.
- PasswordEncoder verifies the password.
- If the credentials are valid, an authenticated Authentication object is returned; otherwise, an exception is thrown.

**Advantages**
- Centralizes authentication logic.
- Supports multiple authentication mechanisms.
- Works with custom AuthenticationProvider implementations.
- Integrates seamlessly with Spring Security.

**Interview Answer (Short)**

AuthenticationManager is the core Spring Security interface responsible for authenticating users. It receives the authentication request, delegates it to an AuthenticationProvider, and returns an authenticated Authentication object if the credentials are valid. It is the main entry point for authentication in Spring Security.


## 24. What is AuthenticationProvider?

**Answer:**
AuthenticationProvider is a Spring Security interface responsible for validating user credentials during authentication.

It is called by the AuthenticationManager to authenticate the user.

**Responsibilities**

- Validates the username and password.
- Uses UserDetailsService to load user information.
- Uses PasswordEncoder to verify the password.
- Returns an authenticated Authentication object if authentication succeeds.
- Throws an exception if authentication fails.

**Example**
```java
@Component
public class CustomAuthenticationProvider implements AuthenticationProvider {

    @Override
    public Authentication authenticate(Authentication authentication)
            throws AuthenticationException {

        String username = authentication.getName();
        String password = authentication.getCredentials().toString();

        // Validate credentials

        return new UsernamePasswordAuthenticationToken(
                username,
                password,
                List.of(new SimpleGrantedAuthority("ROLE_USER"))
        );
    }

    @Override
    public boolean supports(Class<?> authentication) {
        return UsernamePasswordAuthenticationToken.class
                .isAssignableFrom(authentication);
    }
}
```

**Interview Answer (Short)**

AuthenticationProvider is a Spring Security interface that performs the actual authentication by validating user credentials. It is called by the AuthenticationManager, uses UserDetailsService to load user details and PasswordEncoder to verify the password, and returns an authenticated Authentication object if the credentials are valid.


## 25. Explain SecurityContextHolder.

**Answer:**
Stores authenticated user.

**Example**
```java
Authentication auth =
SecurityContextHolder.getContext().getAuthentication();
```
Used throughout request lifecycle.


## 26. How do you secure Microservices?

### Answer

Securing microservices involves implementing authentication, authorization, encrypted communication, and secure service-to-service interactions.

**Best Practices**

- **JWT (JSON Web Token)** – Use stateless token-based authentication for securing APIs.
- **OAuth2** – Implement secure authentication and authorization using OAuth2 providers.
- **API Gateway** – Route requests through an API Gateway to enforce authentication, authorization, rate limiting, and logging.
- **HTTPS** – Encrypt all client-to-service and service-to-service communication using TLS.
- **Mutual TLS (mTLS)** – Use client and server certificates for secure service-to-service authentication when required.
- **Role-Based Authorization** – Control access to resources using user roles and permissions.
- **Service-to-Service Authentication** – Authenticate communication between microservices using JWT, OAuth2, or mTLS.
- **Token Validation at Gateway/Resource Server** – Validate JWT tokens at the API Gateway or Resource Server before forwarding requests to backend services.


## 27. How do you secure REST APIs?

**Answer:** 
Securing REST APIs involves implementing authentication, authorization, encryption, and protection against common security vulnerabilities.

**Best Practices**

- **HTTPS Only** – Encrypt all client-server communication using TLS/SSL.
- **JWT (JSON Web Token)** – Use stateless token-based authentication for secure API access.
- **BCrypt Passwords** – Store passwords securely using the BCrypt hashing algorithm.
- **OAuth2** – Use OAuth2 for secure authentication and authorization, especially with third-party identity providers.
- **Input Validation** – Validate and sanitize all user inputs to prevent SQL Injection, XSS, and other attacks.
- **Rate Limiting** – Limit the number of requests from clients to prevent abuse and denial-of-service attacks.
- **CORS (Cross-Origin Resource Sharing)** – Configure CORS to allow requests only from trusted origins.
- **CSRF Disabled (for Stateless APIs)** – Disable CSRF protection for JWT-based stateless APIs since they do not rely on server-side sessions.
- **Exception Handling** – Return standardized error responses without exposing sensitive implementation details.
- **Token Expiry** – Use short-lived access tokens to reduce the impact of token compromise.
- **Refresh Tokens** – Use long-lived refresh tokens to securely obtain new access tokens without requiring users to log in again.


## 28. Explain Session Management

**Answer:** 
Session Management is the process of maintaining a user's authenticated state across multiple HTTP requests.

Since HTTP is stateless, Spring Security uses a session (typically JSESSIONID) to remember that a user has already been authenticated.

**How Session Management Works**

1. User logs in with a username and password.
2. Spring Security authenticates the user.
3. The server creates an HTTP session.
4. A session ID (JSESSIONID) is sent to the browser as a cookie.
5. The browser includes the session ID in subsequent requests.
6. Spring Security uses the session to identify the authenticated user.

**Session Creation Policy**
- ALWAYS
- IF_REQUIRED
- NEVER
- STATELESS

Spring Security provides different session creation policies:
```java
http.sessionManagement(session -> session
    .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
);
```
**Interview Answer (Short)**

Session Management is the process of maintaining a user's authenticated state across multiple HTTP requests. After successful login, Spring Security creates an HTTP session and sends a JSESSIONID cookie to the client. For subsequent requests, the browser sends this cookie, allowing the server to recognize the user. For JWT-based REST APIs, SessionCreationPolicy.STATELESS is typically used, so no server-side session is created.


## 29. What is the difference between Stateful and Stateless Authentication?

**Answer:**

Stateful authentication stores user session information on the server, whereas stateless authentication stores authentication data in a token (such as JWT) that is sent with every request.

| **Stateful Authentication** | **Stateless Authentication** |
|-----------------------------|------------------------------|
| Session is stored on the server | No server-side session is maintained |
| Uses **JSESSIONID** (or similar session ID) | Uses **JWT (JSON Web Token)** |
| Higher server memory usage | More scalable since no session storage is required |
| Best suited for traditional web applications | Best suited for REST APIs and microservices |

**Summary**

- **Stateful Authentication**
  - Stores session data on the server.
  - Requires session management.
  - Ideal for traditional web applications.

- **Stateless Authentication**
  - Stores user information inside a signed token (JWT).
  - Every request carries the token for authentication.
  - Ideal for distributed systems, REST APIs, and microservices due to better scalability.



## 30. What security best practices have you implemented in production?

A strong answer for a senior developer:

- Used BCryptPasswordEncoder for password hashing.
- Implemented JWT with short-lived access tokens and refresh tokens.
- Enabled HTTPS in all environments.
- Configured role-based and method-level authorization.
- Used SecurityFilterChain with custom JWT filters.
- Disabled CSRF for stateless APIs and configured CORS appropriately.
- Secured secrets using environment variables or a secret management solution.
- Added centralized exception handling for authentication and authorization failures.
- Implemented audit logging for login and sensitive operations.
- Applied API rate limiting and input validation.
- Performed regular dependency updates to address security vulnerabilities.


### Scenario-Based Questions

## Q: A JWT token is stolen. How can you reduce the risk?

- Use HTTPS.
- Keep access tokens short-lived.
- Use refresh token rotation.
- Store tokens securely (avoid local storage when possible; consider secure, HttpOnly cookies where appropriate).
- Implement token revocation or blacklisting if required.
- Monitor and log suspicious activity.

## Q: Why do you use OncePerRequestFilter for JWT validation?

It guarantees the filter executes only once for each HTTP request, preventing duplicate token validation and ensuring the authenticated user is set in the security context before authorization.

## Q: How do you allow public endpoints while securing the rest?
```java
http.authorizeHttpRequests(auth -> auth
    .requestMatchers("/login", "/register").permitAll()
    .anyRequest().authenticated()
);
```
