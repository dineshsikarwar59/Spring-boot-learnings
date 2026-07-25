# JPA Interview Questions

## 1. What is JPA?

### Answer:
JPA (Java Persistence API) is a Java specification used for managing relational database operations using Java objects. It provides an object-relational mapping (ORM) approach, allowing developers to work with objects instead of writing SQL queries directly.

### Popular JPA Implementations:
- Hibernate ORM
- EclipseLink
- OpenJPA

---

## 2. What are the advantages of JPA?

### Answer:
JPA provides several advantages for Java database applications:

- Reduces boilerplate JDBC code
- Provides ORM between Java objects and database tables
- Supports database independence
- Provides caching support
- Handles transaction management
- Supports JPQL queries
- Improves maintainability

---

## 3. What is ORM?

### Answer:
ORM (Object Relational Mapping) is a technique that maps Java objects to database tables.

### Example:

**Java Entity:**

```java
@Entity
public class Employee {
    @Id
    private Long id;
    private String name;
}
```

**Database Table:**
```text
EMPLOYEE
ID
NAME
```
The Employee object is mapped to the EMPLOYEE table.

---

## 4. What are the main JPA components?

### Answer:
The main JPA components are:

- **Entity**  
  Java class mapped to a database table.

- **EntityManager**  
  Manages entity lifecycle and database operations.

- **EntityManagerFactory**  
  Creates `EntityManager` instances.

- **Persistence Unit**  
  Configuration containing database and entity information.

- **Persistence Context**  
  Environment where entities are managed.

---

## 5. What is an Entity in JPA?

### Answer:
An Entity is a Java class that represents a database table. Each instance of an entity represents a row in the database table.

### Example:

**Java Entity:**

```java
@Entity
@Table(name="employee")
public class Employee {

    @Id
    private Long id;

    private String name;
}
```

**Rules for Entities:**
- Must have @Entity annotation
- Must have a primary key using @Id
- Must have a no-argument constructor
- Class should not be final


---

## 6. What is EntityManager?

### Answer:
EntityManager is the main interface used by JPA to perform CRUD operations. It manages the lifecycle of entities and provides methods to interact with the persistence context.

### Example:

```java
Employee emp = new Employee();
emp.setName("John");

entityManager.persist(emp);
```

**Common EntityManager Methods:**
-persist() - Saves a new entity to the database
- find() - Retrieves an entity by its primary key
- merge() - Updates or merges an entity state
- remove() - Deletes an entity from the database
- refresh() - Reloads entity data from the database


---

## 7. Difference between persist() and save()

### Answer:

| persist() | save() |
|-----------|--------|
| JPA method | Hibernate-specific method |
| Returns `void` | Returns generated ID |
| Makes entity managed | Saves entity immediately |
| Portable across JPA providers | Hibernate only |

---

## 8. Difference Between `persist()` and `merge()`

### Answer

In JPA, `persist()` and `merge()` are used to save entities, but they serve different purposes based on the entity's lifecycle state.

### `persist()`

`persist()` is used to save **new entities** into the database.

**Characteristics:**
- Used for transient (new) entities
- Entity becomes managed by the persistence context
- Creates a new database record
- Does not return an entity object

Example:

```java
entityManager.persist(employee);
```

**Example Flow:**

```
New Entity → persist() → Managed Entity → Insert into Database
```

---

### `merge()`

`merge()` is used to update **detached entities** or copy the state of an entity into a managed entity.

**Characteristics:**
- Used for detached entities
- Updates an existing database record
- Returns a new managed instance
- Original entity remains detached

Example:

```java
Employee updated = entityManager.merge(employee);
```

**Example Flow:**

```
Detached Entity → merge() → Managed Entity → Update Database
```

---

### Difference Between `persist()` and `merge()`

| Feature | `persist()` | `merge()` |
|---------|-------------|-----------|
| Purpose | Creates a new entity | Updates an existing entity |
| Entity State | Used with transient entities | Used with detached entities |
| Database Operation | INSERT | UPDATE (or INSERT if no existing record) |
| Return Value | `void` | Returns managed entity |
| Entity Management | Original entity becomes managed | Returns a managed copy |

### Example:

```java
// Creating new employee
Employee employee = new Employee();
employee.setName("John");

entityManager.persist(employee);


// Updating existing employee
Employee detachedEmployee = getEmployee();

Employee updatedEmployee = entityManager.merge(detachedEmployee);
```


---

## 9. What are JPA Entity Lifecycle States?

### Answer

In JPA, an **entity lifecycle state** represents the current status of an entity object in relation to the **persistence context** managed by the `EntityManager`.

JPA defines four main lifecycle states:

1. **New / Transient**
2. **Managed / Persistent**
3. **Detached**
4. **Removed**

---

### 1. New / Transient State

An entity object is in the **Transient** state when it is created using the `new` keyword but is not yet associated with a persistence context.

Example:

```java
Employee e = new Employee();
```

**Characteristics:**
- Not managed by `EntityManager`
- No database record exists
- Changes are not tracked by JPA

---

### 2. Managed / Persistent State

An entity is in the **Persistent** state when it is associated with the persistence context and managed by the `EntityManager`.

Example:

```java
entityManager.persist(e);
```

**Characteristics:**
- Entity is tracked by JPA
- Changes are automatically synchronized with the database
- Insert/update operations are handled automatically

---

### 3. Detached State

An entity is in the **Detached** state when it was previously managed but is no longer associated with the persistence context.

Example:

```java
entityManager.detach(e);
```

**Characteristics:**
- Entity exists in memory
- No longer tracked by `EntityManager`
- Changes are not automatically saved to the database

---

### 4. Removed State

An entity is in the **Removed** state when it is marked for deletion from the database.

Example:

```java
entityManager.remove(e);
```

**Characteristics:**
- Entity is scheduled for deletion
- Database record is removed during transaction commit

---

### Entity Lifecycle Flow

```
New / Transient
        |
        | persist()
        ↓
Managed / Persistent
        |
        | detach() / close()
        ↓
Detached
        |
        | remove()
        ↓
Removed
```


---

## 10. What is Persistence Context?

### Answer:
Persistence Context is a first-level cache where JPA manages entity objects. It tracks the lifecycle of entities and stores managed entity instances during a transaction.

### Example:

```java
Employee e1 = entityManager.find(Employee.class, 1);
Employee e2 = entityManager.find(Employee.class, 1);
```
Only one database query is executed because the second request gets the object from the persistence context instead of fetching it again from the database.

---

## 11. What is JPQL?

### Answer:
JPQL (Java Persistence Query Language) is an object-oriented query language used by JPA to perform database operations using entity objects instead of database tables.

### Example:

```java
SELECT e FROM Employee e WHERE e.name='John'
```

JPQL uses entity names and entity attributes instead of table names and column names.

**JPQL:**
```java
SELECT e FROM Employee e WHERE e.name='John'
```
**SQL:**
```java
SELECT * FROM employee WHERE name='John';
```
---

## 12. Difference between JPQL and SQL

### Answer:

| JPQL | SQL |
|------|-----|
| Works with entities | Works with tables |
| Database independent | Database dependent |
| Uses entity fields | Uses column names |
| JPA standard | Database language |

---

## 13. What are JPA Annotations?

### Answer

**JPA annotations** are used to provide metadata that defines how Java classes and their fields are mapped to database tables and columns.

They help JPA understand entity relationships, primary keys, table mappings, and other database configurations.

### Common JPA Annotations

| Annotation | Purpose |
|------------|---------|
| `@Entity` | Defines a Java class as a JPA entity |
| `@Table` | Maps an entity to a specific database table |
| `@Id` | Defines the primary key of an entity |
| `@GeneratedValue` | Automatically generates primary key values |
| `@Column` | Maps a field to a database column |
| `@OneToOne` | Defines a one-to-one relationship |
| `@OneToMany` | Defines a one-to-many relationship |
| `@ManyToOne` | Defines a many-to-one relationship |
| `@ManyToMany` | Defines a many-to-many relationship |
| `@JoinColumn` | Defines a foreign key column for relationships |

### Example

```java
@Entity
@Table(name = "employees")
public class Employee {

    @Id
    @GeneratedValue
    private Long id;

    @Column(name = "employee_name")
    private String name;

    @ManyToOne
    @JoinColumn(name = "department_id")
    private Department department;
}
```

### Key Points

- `@Entity` → Converts a Java class into a database entity.
- `@Id` → Marks the primary key field.
- `@GeneratedValue` → Automatically generates ID values.
- Relationship annotations define associations between entities.
- `@JoinColumn` is commonly used to specify foreign key columns.


---

## 14. Explain Relationship Mappings in JPA

### Answer

In JPA, **relationship mappings** define how entities are associated with each other in the database. JPA supports four main types of relationships:

1. **One-to-One**
2. **One-to-Many**
3. **Many-to-One**
4. **Many-to-Many**

---

### 1. One-to-One Relationship

A **one-to-one** relationship means one entity is associated with exactly one other entity.

**Example:**

```
Person → Passport
```

```java
@OneToOne
private Passport passport;
```

**Use case:**  
One person has one passport, and one passport belongs to one person.

---

### 2. One-to-Many Relationship

A **one-to-many** relationship means one entity can have multiple related entities.

**Example:**

```
Department → Employees
```

```java
@OneToMany
private List<Employee> employees;
```

**Use case:**  
One department can have many employees.

---

### 3. Many-to-One Relationship

A **many-to-one** relationship means multiple entities can be associated with one entity.

**Example:**

```
Employees → Department
```

```java
@ManyToOne
private Department department;
```

**Use case:**  
Many employees belong to one department.

---

### 4. Many-to-Many Relationship

A **many-to-many** relationship means multiple entities can be associated with multiple entities.

**Example:**

```
Student ↔ Course
```

```java
@ManyToMany
private List<Course> courses;
```

**Use case:**  
A student can enroll in multiple courses, and a course can have multiple students.

---

### Summary Table

| Relationship | Annotation | Example |
|--------------|------------|---------|
| One-to-One | `@OneToOne` | Person → Passport |
| One-to-Many | `@OneToMany` | Department → Employees |
| Many-to-One | `@ManyToOne` | Employee → Department |
| Many-to-Many | `@ManyToMany` | Student ↔ Course |


---

## 15. What is FetchType in JPA?

### Answer:
`FetchType` defines when related entity data is loaded from the database. It controls the loading strategy for relationships between entities.

### Types:

### 1. EAGER
Loads related data immediately along with the parent entity.

```java
@ManyToOne(fetch = FetchType.EAGER)
private Department department;
```

### 2. LAZY
Loads related data only when it is accessed.
```java
@OneToMany(fetch = FetchType.LAZY)
private List<Employee> employees;
```

**Default Fetch Types:**

Relationship	Default Fetch Type
- **@OneToOne**	EAGER
- **@ManyToOne**	EAGER
- **@OneToMany**	LAZY
- **@ManyToMany**	LAZY

---

## 16. What is Cascade in JPA?

### Answer

In JPA, **Cascade** defines how operations performed on one entity are automatically applied to its related entities.

For example, when a parent entity is saved, updated, or deleted, the same operation can be automatically applied to its associated child entities.

### Example

```java
@OneToMany(cascade = CascadeType.ALL)
private List<Address> addresses;
```

In this example, operations performed on the parent entity will also be applied to the `Address` entities.

### Cascade Types in JPA

| Cascade Type | Description |
|--------------|-------------|
| `PERSIST` | Saves related entities when the parent entity is persisted |
| `MERGE` | Updates related entities when the parent entity is merged |
| `REMOVE` | Deletes related entities when the parent entity is deleted |
| `REFRESH` | Refreshes related entities from the database |
| `DETACH` | Detaches related entities when the parent entity is detached |
| `ALL` | Applies all cascade operations |

### Example with Entity

```java
@Entity
public class User {

    @OneToMany(
        cascade = CascadeType.ALL
    )
    private List<Address> addresses;
}
```

### Important Note

Use cascade carefully, especially `CascadeType.REMOVE`, because deleting a parent entity may also delete its associated child entities.

---

## 17. What is the Difference Between First-Level and Second-Level Cache?

### Answer

In JPA, **cache** is used to improve application performance by reducing the number of database queries. JPA provides two levels of caching:

1. **First-Level Cache**
2. **Second-Level Cache**

---

## First-Level Cache

The **first-level cache** is the default cache provided by JPA. It is associated with the **persistence context** and managed by the `EntityManager`.

**Characteristics:**

- Enabled by default
- Exists within the `EntityManager` scope
- Stores entities during a persistence context lifecycle
- Cannot be disabled
- Each `EntityManager` has its own cache

Example:

```java
Employee emp1 = entityManager.find(Employee.class, 1L);
Employee emp2 = entityManager.find(Employee.class, 1L);
```

In this example, the first database query fetches the employee. The second call retrieves the entity from the first-level cache instead of hitting the database.

---

## Second-Level Cache

The **second-level cache** is an optional cache that works across multiple persistence contexts. It is managed by the JPA provider (such as Hibernate).

**Characteristics:**

- Disabled by default
- Exists at the `SessionFactory` level (Hibernate)
- Shared across multiple sessions or `EntityManager` instances
- Provider dependent
- Requires additional configuration

---

## Difference Between First-Level and Second-Level Cache

| Feature | First-Level Cache | Second-Level Cache |
|---------|-------------------|---------------------|
| Availability | Enabled by default | Optional |
| Scope | `EntityManager` / Persistence Context | `SessionFactory` scope |
| Sharing | Not shared between sessions | Shared across sessions |
| Storage | Persistence context | Global cache |
| Configuration | No configuration required | Requires provider configuration |
| Disable Support | Cannot be disabled | Can be enabled/disabled |
| Dependency | JPA standard | Provider dependent |

---

### Cache Flow

```
First Request
     |
     ↓
Check First-Level Cache
     |
     ↓
Database Query


Second Request
     |
     ↓
Check First-Level Cache
     |
     ↓
Check Second-Level Cache
     |
     ↓
Database Query (if not found)
```

---

## 18. What is Optimistic Locking in JPA?

### Answer

**Optimistic locking** is a mechanism in JPA that prevents multiple users from updating the same database record at the same time and accidentally overwriting each other's changes.

It assumes that conflicts are rare and checks whether the data was modified before updating it.

JPA uses the `@Version` annotation to implement optimistic locking.

---

### Example

```java
@Entity
public class Employee {

    @Id
    private Long id;

    @Version
    private int version;

}
```

---

### How It Works

1. When an entity is saved, JPA stores the current version number.
2. When an update happens, JPA checks whether the version number is unchanged.
3. If another transaction has already updated the record, the version number will be different.
4. JPA throws an `OptimisticLockException`.

---

### Example Scenario

```
User A reads Employee record (version = 1)

User B reads Employee record (version = 1)

User A updates Employee
Version changes: 1 → 2

User B tries to update Employee
Version mismatch detected

OptimisticLockException is thrown
```

---

### Key Points

- Implemented using `@Version`
- Helps prevent lost updates
- Suitable for applications with many reads and fewer concurrent updates
- Does not lock database rows during reading
- JPA automatically manages version incrementing

### Common Version Field Types

```java
@Version
private int version;
```

or

```java
@Version
private Long version;
```

---

## 19. What is LazyInitializationException?

### Answer

**LazyInitializationException** occurs in JPA/Hibernate when a **lazy-loaded association is accessed after the persistence context (EntityManager/Session) has been closed**.

By default, relationships like `@OneToMany` and `@ManyToMany` are often loaded lazily, meaning related data is fetched only when it is accessed.

#### Example

```java
@Entity
public class Employee {

    @OneToMany(fetch = FetchType.LAZY)
    private List<Address> addresses;

}
```

Accessing the lazy-loaded collection after closing the persistence context:

```java
EntityManager.close();

employee.getAddresses();
```

will cause:

```
LazyInitializationException
```

because Hibernate cannot load the related data without an active session.

### Why Does It Happen?

1. Employee entity is loaded from the database.
2. Addresses are not loaded immediately because of `LAZY` fetching.
3. Persistence context is closed.
4. Application tries to access `addresses`.
5. Hibernate cannot fetch the data and throws `LazyInitializationException`.

### Solutions

### 1. Use `JOIN FETCH`

Fetch related data along with the main entity.

Example:

```java
@Query("SELECT e FROM Employee e JOIN FETCH e.addresses")
List<Employee> findEmployeesWithAddresses();
```

### 2. Keep Transaction Open

Access lazy-loaded data while the transaction and persistence context are still active.

Example:

```java
@Transactional
public Employee getEmployee(Long id) {
    return employeeRepository.findById(id).get();
}
```

### 3. Change Fetch Strategy Carefully

Use eager loading when appropriate:

```java
@OneToMany(fetch = FetchType.EAGER)
private List<Address> addresses;
```

**Note:** Avoid using `EAGER` everywhere because it can cause performance issues by loading unnecessary data.

### Key Points

- Commonly occurs with lazy-loaded relationships.
- Happens when accessing data outside an active persistence context.
- Prefer `JOIN FETCH` or proper transaction boundaries over making everything `EAGER`.

---

## 20. Difference Between Hibernate and JPA

#### Answer

**JPA (Java Persistence API)** and **Hibernate** are used for object-relational mapping (ORM), but they have different roles.

- **JPA** is a specification that defines rules and interfaces for ORM.
- **Hibernate** is an implementation of the JPA specification that provides the actual functionality.


#### Difference Between JPA and Hibernate

| Feature | JPA | Hibernate |
|---------|-----|-----------|
| Type | Specification | Implementation |
| Purpose | Defines ORM standards and interfaces | Provides actual ORM functionality |
| Vendor Support | Vendor independent | Vendor specific |
| API | Uses `EntityManager` | Uses `Session` |
| Query Language | JPQL (Java Persistence Query Language) | HQL (Hibernate Query Language) |
| Configuration | Requires a JPA provider | Provides its own configuration |
| Dependency | Needs an implementation like Hibernate | Can work independently |


#### Example

**Using JPA:**

```java
EntityManager entityManager;
entityManager.persist(employee);
```

**Using Hibernate:**

```java
Session session;
session.save(employee);
```


#### Relationship Between JPA and Hibernate

```
        Application
             |
             ↓
            JPA
     (Specification/API)
             |
             ↓
        Hibernate
     (Implementation)
             |
             ↓
         Database
```

### Key Points

- JPA defines **what should be done**.
- Hibernate defines **how it is done**.
- Hibernate is one of the most popular implementations of JPA.
- Using JPA allows switching between different ORM providers with minimal changes.

---

## 21. What is the N+1 Query Problem?

#### Answer

The **N+1 query problem** occurs when an application executes one query to fetch a list of parent entities and then executes additional queries for each related child entity.

This can cause performance issues because it results in multiple unnecessary database calls.

#### Example

Consider an `Employee` entity with a relationship to `Department`:

```java
@Entity
public class Employee {

    @ManyToOne
    private Department department;

}
```

Fetching all employees:

```java
List<Employee> employees = repository.findAll();
```

This executes one query:

```sql
SELECT * FROM employee;
```

When accessing the department for each employee:

```java
for(Employee employee : employees) {
    System.out.println(employee.getDepartment().getName());
}
```

Hibernate may execute additional queries:

```sql
SELECT * FROM department WHERE id = 1;
SELECT * FROM department WHERE id = 2;
SELECT * FROM department WHERE id = 3;
...
```

If there are **N employees**, the application executes:

```
1 query + N additional queries = N+1 queries
```

#### Solutions

#### 1. Use `JOIN FETCH`

Fetch related entities in a single query.

Example:

```java
@Query("SELECT e FROM Employee e JOIN FETCH e.department")
List<Employee> findEmployeesWithDepartment();
```


#### 2. Use `EntityGraph`

Define which relationships should be loaded together.

Example:

```java
@EntityGraph(attributePaths = {"department"})
List<Employee> findAll();
```

#### 3. Optimize Fetch Strategy

Choose appropriate fetch types:

```java
@ManyToOne(fetch = FetchType.LAZY)
private Department department;
```

Avoid unnecessary eager loading.

#### Key Points

- N+1 query problem reduces application performance.
- Commonly occurs with lazy-loaded relationships.
- Use `JOIN FETCH`, `EntityGraph`, or proper fetch strategies to optimize queries.
- Always monitor generated SQL queries when working with JPA/Hibernate.

---

## 22. What is the Difference Between `find()` and `getReference()`?

##### Answer

In JPA, both `find()` and `getReference()` are used to retrieve an entity by its primary key, but they differ in how and when the data is loaded from the database.

#### `find()`

The `find()` method immediately loads the entity from the database.

#### Characteristics:

- Executes SQL query immediately
- Returns the actual entity object
- Returns `null` if the entity does not exist
- Used when entity data is required immediately

Example:

```java
Employee employee = entityManager.find(Employee.class, 1L);
```

Flow:

```
find()
  |
  ↓
Execute SQL Query
  |
  ↓
Load Entity
  |
  ↓
Return Entity Object
```

---

#### `getReference()`

The `getReference()` method returns a **proxy object** instead of loading the actual entity immediately.

The database query is executed only when a property of the entity is accessed.

#### Characteristics:

- Returns a proxy object
- Uses lazy loading
- SQL executes only when data is accessed
- Throws `EntityNotFoundException` if the entity does not exist when accessed

Example:

```java
Employee employee = entityManager.getReference(Employee.class, 1L);
```

Flow:

```
getReference()
       |
       ↓
Create Proxy Object
       |
       ↓
Access Entity Data
       |
       ↓
Execute SQL Query
```

#### Difference Between `find()` and `getReference()`

| Feature | `find()` | `getReference()` |
|---------|----------|------------------|
| Data Loading | Loads immediately | Loads lazily |
| SQL Execution | Executes SQL immediately | Executes SQL when accessed |
| Return Type | Actual entity object | Proxy object |
| If Entity Not Found | Returns `null` | Throws `EntityNotFoundException` |
| Performance | May be slower if data is not needed | Faster when only reference is required |


#### Example Use Case

Using `find()`:

```java
Employee employee = entityManager.find(Employee.class, 1L);
System.out.println(employee.getName());
```

Use when you need employee details.

Using `getReference()`:

```java
Employee employee = entityManager.getReference(Employee.class, 1L);
department.setManager(employee);
```

Use when you only need a reference, such as setting a relationship, without loading the full entity.

#### Key Points

- Use `find()` when you need the entity data immediately.
- Use `getReference()` when you only need an entity reference.
- `getReference()` can improve performance by avoiding unnecessary database queries.

---

## 23. What is the Purpose of `@Transactional`?

#### Answer

The **`@Transactional`** annotation in Spring is used to manage database transactions automatically. It defines a boundary within which a group of database operations should execute as a single transaction.

If all operations are successful, the transaction is **committed**. If an error occurs, the transaction is **rolled back**.

#### Example

```java
@Transactional
public void updateEmployee(Employee e) {
    repository.save(e);
}
```

In this example, the `save()` operation runs inside a transaction managed by Spring.


#### Responsibilities of `@Transactional`

| Feature | Description |
|---------|-------------|
| **Transaction Boundary** | Defines where a transaction starts and ends |
| **Commit** | Saves all changes permanently when the transaction succeeds |
| **Rollback** | Reverts changes when an exception occurs |
| **Connection Management** | Manages database connections automatically |


#### Example with Multiple Operations

```java
@Transactional
public void transferEmployeeData(Employee employee) {

    employeeRepository.save(employee);

    auditRepository.save(new AuditLog());
}
```

Both operations are treated as one transaction:

```
Transaction Start
        |
        ↓
Save Employee
        |
        ↓
Save Audit Log
        |
        ↓
Transaction Commit
```

If any operation fails:

```
Transaction Start
        |
        ↓
Save Employee
        |
        ↓
Error Occurs
        |
        ↓
Transaction Rollback
```

#### Key Points

- `@Transactional` ensures data consistency.
- It automatically handles commit and rollback.
- It manages transaction boundaries.
- Commonly used in the service layer in Spring applications.
- By default, runtime exceptions trigger rollback.

---

## 24. What is the Difference Between `save()`, `saveAndFlush()`, and `flush()` in Spring Data JPA?

### Answer

In Spring Data JPA, `save()`, `saveAndFlush()`, and `flush()` are used to persist changes to the database, but they differ in **when the SQL statements are executed**.

---

## `save()`

The `save()` method persists an entity, but the SQL statement may not be executed immediately. The changes are synchronized with the database when the transaction is committed or when a flush occurs.

### Characteristics

- Saves or updates an entity
- SQL execution may be delayed
- Changes are stored in the persistence context first

Example:

```java
employeeRepository.save(employee);
```

---

## `saveAndFlush()`

The `saveAndFlush()` method saves the entity and immediately flushes the persistence context, causing the SQL statement to execute right away.

### Characteristics

- Saves or updates an entity
- Immediately synchronizes changes with the database
- Useful when the database changes must be visible before the transaction ends

Example:

```java
employeeRepository.saveAndFlush(employee);
```

---

## `flush()`

The `flush()` method synchronizes all pending changes in the persistence context with the database without committing the transaction.

### Characteristics

- Executes pending SQL statements
- Does not commit the transaction
- Useful when you want changes written to the database before continuing

Example:

```java
entityManager.flush();
```

or

```java
employeeRepository.flush();
```

---

## Difference Between `save()`, `saveAndFlush()`, and `flush()`

| Feature | `save()` | `saveAndFlush()` | `flush()` |
|---------|----------|------------------|-----------|
| Saves Entity | ✅ Yes | ✅ Yes | ❌ No |
| Executes SQL Immediately | ❌ No | ✅ Yes | ✅ Yes (for pending changes) |
| Flushes Persistence Context | ❌ No | ✅ Yes | ✅ Yes |
| Commits Transaction | ❌ No | ❌ No | ❌ No |

---

### Example

```java
employeeRepository.save(employee);

// SQL may execute later

employeeRepository.saveAndFlush(employee);

// SQL executes immediately

employeeRepository.flush();

// Flushes all pending changes to the database
```

---

### Key Points

- Use `save()` for normal persistence operations.
- Use `saveAndFlush()` when changes must be synchronized with the database immediately.
- Use `flush()` to write all pending changes to the database without committing the transaction.
- `flush()` does **not** commit the transaction; it only synchronizes the persistence context with the database.

---

## 25. How Do You Improve JPA Performance?

### Answer

Improving JPA performance involves reducing unnecessary database operations, optimizing queries, and efficiently managing entity loading.

---

### 1. Use Pagination

Retrieve data in smaller chunks instead of loading all records at once.

Example:

```java
Page<Employee> employees =
    employeeRepository.findAll(PageRequest.of(0, 10));
```

**Benefit:** Reduces memory usage and improves response time.

---

### 2. Avoid Unnecessary EAGER Loading

Prefer `FetchType.LAZY` unless related data is always required.

Example:

```java
@OneToMany(fetch = FetchType.LAZY)
private List<Address> addresses;
```

**Benefit:** Loads related entities only when needed.

---

### 3. Use Batch Inserts and Updates

Persist multiple entities in batches instead of one at a time.

Example:

```properties
spring.jpa.properties.hibernate.jdbc.batch_size=50
```

**Benefit:** Reduces the number of database round trips.

---

### 4. Use Projections

Fetch only the required columns instead of entire entities.

Example:

```java
public interface EmployeeProjection {
    String getName();
    String getEmail();
}
```

**Benefit:** Reduces data transfer and improves query performance.

---

### 5. Optimize JPQL Queries

Retrieve only the necessary data and avoid complex or unnecessary joins.

Example:

```java
@Query("SELECT e FROM Employee e WHERE e.salary > :salary")
List<Employee> findHighSalaryEmployees(double salary);
```

**Benefit:** Executes more efficient database queries.

---

### 6. Use Caching Appropriately

Use first-level and second-level caching to minimize repeated database access.

**Benefit:** Improves application performance by reducing database queries.

---

### 7. Avoid the N+1 Query Problem

Fetch related entities using `JOIN FETCH` or `EntityGraph`.

Example:

```java
@Query("SELECT e FROM Employee e JOIN FETCH e.department")
List<Employee> findEmployeesWithDepartment();
```

**Benefit:** Reduces multiple unnecessary database queries.

---

### 8. Use Database Indexes

Create indexes on frequently searched or joined columns.

Example:

```java
@Table(
    indexes = {
        @Index(name = "idx_email", columnList = "email")
    }
)
public class Employee {
}
```

**Benefit:** Speeds up search, filtering, and join operations.

---

### Best Practices Summary

| Practice | Benefit |
|----------|---------|
| Use Pagination | Reduces memory usage |
| Prefer `LAZY` Loading | Avoids unnecessary data fetching |
| Use Batch Operations | Minimizes database round trips |
| Use Projections | Fetches only required data |
| Optimize JPQL Queries | Improves query execution |
| Use Caching | Reduces repeated database access |
| Avoid N+1 Queries | Improves performance with related entities |
| Use Database Indexes | Speeds up query execution |

### Key Points

- Fetch only the data you need.
- Prefer `LAZY` loading over `EAGER` where appropriate.
- Use pagination for large datasets.
- Optimize queries and database indexes.
- Monitor SQL generated by JPA/Hibernate to identify performance bottlenecks.

---


