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



