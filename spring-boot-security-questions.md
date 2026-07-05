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
Authentication is the process of verifying a user's identity using credentials such as a username and password. Authorization determines what an authenticated user is allowed to access based on roles or permissions. Authentication happens first, followed by authorization.

