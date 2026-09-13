# Redis cache java interview questions 

## Q. What is a cache?
**Ans:** A cache is a temporary storage area that stores frequently accessed data for faster retrieval.

## Q. Why do we use a cache?
**Ans:** We use caching to reduce network calls, decrease response time, and improve application performance.

## Q. How does a cache work?
**Ans:** A cache stores frequently accessed data in a faster storage location, such as memory. When the same data is requested again, it can be retrieved from the cache instead of fetching it from the original source.


## 1. What is Redis?

**Answer:**

Redis (**Remote Dictionary Server**) is an open-source, in-memory data structure store used as a:

- Cache
- Database
- Message broker
- Queue

It stores data in RAM, making it much faster than traditional databases.

### Java Example

```java
redisTemplate.opsForValue().set("user:101", user);
```

---

## 2. Why use Redis as a Cache?

**Answer:**

### Advantages

- Extremely fast (sub-millisecond response)
- Reduces database load
- Improves application performance
- Supports expiration (TTL)
- Highly scalable
- Supports distributed caching

### Example

#### Without Redis

```text
Application
      │
      ▼
Database (every request)
```

#### With Redis

```text
Application
      │
      ▼
 Redis Cache
      │
      ▼ (Cache Miss)
   Database
```

---

## 3. What is Cache Hit and Cache Miss?

**Answer:**

### Cache Hit

A **Cache Hit** occurs when the requested data is already available in Redis, so it is returned immediately without querying the database.

```text
Request
   │
   ▼
Redis ──► Found
   │
   ▼
Return Response
```

### Cache Miss

A **Cache Miss** occurs when the requested data is not available in Redis. The application retrieves it from the database and then stores it in Redis for future requests.

```text
Request
   │
   ▼
Redis ──► Not Found
   │
   ▼
Database
   │
   ▼
Store in Redis
   │
   ▼
Return Response
```

---

## 4. Explain Cache Aside Pattern

**Answer:**

The **Cache Aside Pattern** (also called **Lazy Loading**) is the most commonly used caching pattern.

In this approach, the application first checks Redis for the requested data. If the data is found, it is returned immediately. If the data is not found, the application fetches it from the database, stores it in Redis for future requests, and then returns the result.

### Flow

1. Check Redis
2. If data exists → Return the cached data
3. If data does not exist → Query the database
4. Store the retrieved data in Redis
5. Return the result

### Flow Diagram

```text
           Request
              │
              ▼
        Check Redis
              │
      ┌───────┴────────┐
      │                │
      ▼                ▼
 Cache Hit        Cache Miss
      │                │
      ▼                ▼
Return Data      Query Database
                       │
                       ▼
                Store in Redis
                       │
                       ▼
                  Return Data
```

### Java Example

```java
String key = "user:" + id;

User user = redisTemplate.opsForValue().get(key);

if (user == null) {
    user = userRepository.findById(id).orElse(null);

    if (user != null) {
        redisTemplate.opsForValue().set(key, user);
    }
}

return user;
```

### Advantages

- Reduces database load
- Improves application performance
- Returns cached data quickly
- Simple and widely used caching strategy

---

## 5. What is TTL (Time To Live)?

**Answer:**

**TTL (Time To Live)** defines how long a cache entry remains in Redis before it expires automatically.

Once the TTL duration is reached, Redis automatically deletes the key, ensuring that stale or outdated data is removed from the cache.

### Java Example

```java
redisTemplate.opsForValue()
    .set("user:101", user, Duration.ofMinutes(30));
```

In this example, the key `user:101` will expire automatically after **30 minutes**.

### Flow Diagram

```text
Store Data in Redis
        │
        ▼
 Set TTL = 30 Minutes
        │
        ▼
 Data Available
        │
        ▼
30 Minutes Elapsed
        │
        ▼
Redis Automatically Deletes the Key
```

### Advantages

- Prevents stale data from remaining in the cache
- Frees up memory automatically
- Reduces manual cache cleanup
- Keeps cached data synchronized with the database

---

## 6. Why should cache have expiration?

**Answer:**

Cache expiration ensures that outdated or stale data is automatically removed from Redis after a specified period. This keeps the cache fresh and prevents unnecessary memory usage.

### Without Cache Expiration

- Old data remains in the cache indefinitely.
- Memory usage keeps increasing.
- Users may receive stale or outdated data.

### Benefits of TTL (Cache Expiration)

- Frees up memory automatically
- Refreshes outdated data
- Improves cache efficiency
- Ensures users receive more up-to-date data
- Prevents the cache from growing indefinitely

### Flow Diagram

```text
        Data Cached
             │
             ▼
        TTL Configured
             │
             ▼
      Cache Valid Period
             │
             ▼
      TTL Expires
             │
             ▼
 Redis Automatically Deletes Key
             │
             ▼
 Next Request Fetches Fresh Data
        From Database
```

### Example

```java
redisTemplate.opsForValue()
    .set("user:101", user, Duration.ofMinutes(30));
```

After **30 minutes**, Redis automatically removes the cached entry. The next request retrieves fresh data from the database and stores it back in the cache.

---

## 7. What data structures does Redis support?

**Answer:**

Redis supports several built-in data structures, making it suitable for a wide variety of use cases.

### Redis Data Structures

| Data Structure | Description | Common Use Cases |
|----------------|-------------|------------------|
| **String** | Stores simple key-value pairs | Caching, counters, session storage |
| **Hash** | Stores field-value pairs under a single key | User profiles, objects |
| **List** | Ordered collection of elements | Queues, task processing, recent activities |
| **Set** | Unordered collection of unique elements | Unique visitors, tags, permissions |
| **Sorted Set (ZSet)** | Set with elements sorted by a score | Leaderboards, rankings, priority queues |
| **Bitmap** | Bit-level operations for boolean values | Online/offline status, feature flags |
| **HyperLogLog** | Estimates the count of unique elements with minimal memory | Unique visitor counting, analytics |
| **Stream** | Append-only log of records | Event streaming, messaging, real-time data processing |

### Visual Overview

```text
Redis
│
├── String       → Key-Value Cache
├── Hash         → Objects / User Profiles
├── List         → Queue / Recent Items
├── Set          → Unique Values
├── Sorted Set   → Leaderboards / Rankings
├── Bitmap       → Boolean Flags
├── HyperLogLog  → Approximate Unique Count
└── Stream       → Event & Message Streaming
```

### Java Examples

#### String

```java
redisTemplate.opsForValue().set("name", "John");
```

#### Hash

```java
redisTemplate.opsForHash().put("user:101", "name", "John");
```

#### List

```java
redisTemplate.opsForList().leftPush("tasks", "Task1");
```

#### Set

```java
redisTemplate.opsForSet().add("roles", "ADMIN");
```

#### Sorted Set (ZSet)

```java
redisTemplate.opsForZSet().add("leaderboard", "Alice", 95);
```

Each data structure is optimized for specific operations, allowing Redis to handle caching, messaging, analytics, ranking systems, and real-time applications efficiently.

---

## 8. Difference between String and Hash

**Answer:**

Both **String** and **Hash** are commonly used Redis data structures, but they are designed for different use cases.

### Comparison

| Feature | String | Hash |
|---------|--------|------|
| Data Format | Stores a single value (text, JSON, number, etc.) | Stores multiple field-value pairs |
| Best For | Simple values, JSON objects, tokens | Objects with multiple attributes |
| Updates | Entire value must be updated | Individual fields can be updated |
| Memory Usage | Higher for large objects | More efficient for objects with many fields |
| Example | User stored as JSON | User stored as separate fields |

### String Example

The entire object is stored as a single JSON value.

```text
Key: user:101
Value:
{
  "name": "John",
  "age": 30,
  "city": "NY"
}
```

**Java Example**

```java
redisTemplate.opsForValue().set(
    "user:101",
    "{\"name\":\"John\",\"age\":30,\"city\":\"NY\"}"
);
```

### Hash Example

Each attribute is stored as a separate field under the same key.

```text
Key: user:101

name → John
age  → 30
city → NY
```

**Java Example**

```java
redisTemplate.opsForHash().put("user:101", "name", "John");
redisTemplate.opsForHash().put("user:101", "age", 30);
redisTemplate.opsForHash().put("user:101", "city", "NY");
```

### When to Use

- **Use String** when storing a complete value such as JSON, session data, or tokens.
- **Use Hash** when storing objects with multiple fields that may need to be updated individually.

### Visual Comparison

```text
String
──────

user:101
    │
    ▼
{"name":"John","age":30,"city":"NY"}


Hash
────

user:101
    │
    ├── name → John
    ├── age  → 30
    └── city → NY
```

---

## 9. What is `RedisTemplate`?

**Answer:**

`RedisTemplate` is a Spring Boot class provided by **Spring Data Redis** that simplifies interaction with a Redis server. It offers high-level APIs for performing operations on different Redis data structures such as **String**, **Hash**, **List**, **Set**, and **Sorted Set (ZSet)**.

### Dependency Injection

```java
@Autowired
private RedisTemplate<String, Object> redisTemplate;
```

### Common Operations

| Method | Redis Data Structure | Purpose |
|---------|----------------------|---------|
| `opsForValue()` | String | Store and retrieve simple key-value pairs |
| `opsForHash()` | Hash | Store and retrieve field-value pairs |
| `opsForList()` | List | Work with ordered collections |
| `opsForSet()` | Set | Store unique values |
| `opsForZSet()` | Sorted Set (ZSet) | Store sorted values with scores |

### Examples

#### String

```java
redisTemplate.opsForValue().set("name", "John");
```

#### Hash

```java
redisTemplate.opsForHash().put("user:101", "name", "John");
```

#### List

```java
redisTemplate.opsForList().leftPush("tasks", "Task1");
```

#### Set

```java
redisTemplate.opsForSet().add("roles", "ADMIN");
```

#### Sorted Set (ZSet)

```java
redisTemplate.opsForZSet().add("leaderboard", "Alice", 95);
```

### Visual Overview

```text
RedisTemplate
      │
      ├── opsForValue()  → String
      ├── opsForHash()   → Hash
      ├── opsForList()   → List
      ├── opsForSet()    → Set
      └── opsForZSet()   → Sorted Set
```

### Advantages

- Simplifies Redis operations in Spring Boot
- Supports all major Redis data structures
- Handles serialization and deserialization automatically
- Integrates seamlessly with Spring applications

---

## 10. What is `StringRedisTemplate`?

**Answer:**

`StringRedisTemplate` is a specialized version of `RedisTemplate` provided by Spring Data Redis that is used to store and retrieve **String keys and String values** in Redis.

It is commonly used when working with simple text-based data such as:

- Cache keys
- Tokens
- Session IDs
- Counters
- Simple messages

### Dependency Injection

```java
@Autowired
private StringRedisTemplate template;
```

### Example

```java
template.opsForValue().set("user:101", "John");
```

Retrieve value:

```java
String user = template.opsForValue().get("user:101");
```

### Difference Between `RedisTemplate` and `StringRedisTemplate`

| Feature | RedisTemplate | StringRedisTemplate |
|---------|---------------|---------------------|
| Key Type | Any object | String |
| Value Type | Any object | String |
| Serialization | Requires serializer configuration | Uses String serialization by default |
| Best For | Objects, JSON, complex data | Text-based data |

### Visual Comparison

```text
RedisTemplate<String, Object>

user:101
    │
    ▼
User Object / JSON


StringRedisTemplate

user:101
    │
    ▼
"John"
```

### When to Use

- Use **`StringRedisTemplate`** for simple string-based data.
- Use **`RedisTemplate`** when storing Java objects, JSON, or complex data structures.

---

## 11. How do you cache a Java object?

**Answer:**

A Java object can be cached in Redis by using **JSON serialization**. The object is converted into a JSON format before storing it in Redis and converted back into a Java object when retrieving it.

Spring Boot uses a configured serializer (such as **Jackson JSON serializer**) to handle object conversion automatically.

### Java Example

```java
User user = new User(1, "John", 30);

redisTemplate.opsForValue().set("user:1", user);
```

### Stored in Redis

```json
Key:
user:1

Value:
{
  "id": 1,
  "name": "John",
  "age": 30
}
```

### Retrieve Object

```java
User user = (User) redisTemplate.opsForValue().get("user:1");
```

### How It Works

```text
Java Object
     │
     ▼
JSON Serializer (Jackson)
     │
     ▼
Redis Cache
     │
     ▼
JSON Data Stored


Redis Cache
     │
     ▼
JSON Deserializer
     │
     ▼
Java Object
```

### Common Serializers

- `Jackson2JsonRedisSerializer` → Stores objects as JSON
- `GenericJackson2JsonRedisSerializer` → Supports different object types
- `StringRedisSerializer` → Stores keys/values as strings

### Advantages

- Easy storage of complex Java objects
- Human-readable data in Redis
- Supports object retrieval without manual conversion
- Works well with Spring Boot applications

---

## 12. What serialization options are available?

**Answer:**

Serialization is the process of converting an object into a format that can be stored in Redis. When retrieving the data, deserialization converts it back into the original object.

### Common Serializers

| Serializer | Description | Advantages |
|------------|-------------|------------|
| **JDK Serialization** | Default Java object serialization | Simple but produces larger binary data |
| **Jackson JSON** | Converts objects into JSON format | Human-readable, widely used, interoperable |
| **Gson** | Google's JSON serialization library | Simple JSON conversion |
| **Kryo** | High-performance binary serialization | Faster and more compact |
| **ProtoBuf** | Google's binary serialization format | Efficient and language-independent |

### Jackson JSON Example

```java
redisTemplate.opsForValue()
    .set("user:101", user);
```

Spring converts the Java object into JSON:

```json
{
  "id": 101,
  "name": "John",
  "age": 30
}
```

### Comparison

```text
Java Object
     │
     ▼
 Serialization
     │
 ┌───────────────┬──────────────┬─────────────┐
 │               │              │             │
JDK          Jackson JSON     Kryo        ProtoBuf
 │               │              │             │
Binary        JSON           Binary       Binary
```

### Recommended Choice

**Jackson JSON** is widely preferred in Spring Boot applications because:

- Data is human-readable
- Easy to debug
- Language-independent
- Works well with REST APIs
- Provides good interoperability with other systems

---

## 13. How do you delete cache?

**Answer:**

Cache entries can be removed from Redis using the `delete()` method of `RedisTemplate`.

When a key is deleted, the corresponding data is removed from Redis permanently.

### Java Example

```java
redisTemplate.delete("user:101");
```

### Delete Multiple Keys

```java
redisTemplate.delete(Arrays.asList(
    "user:101",
    "user:102"
));
```

### Flow Diagram

```text
Request
   │
   ▼
Delete Cache Key
   │
   ▼
Redis
   │
   ▼
Remove user:101
```

### Common Use Cases

- After updating user data in the database
- After deleting a record
- When cached data becomes invalid
- During cache refresh operations

### Example: Cache Invalidation

```text
Update User
     │
     ▼
Database Updated
     │
     ▼
Delete Redis Cache
     │
     ▼
Next Request Loads Fresh Data
```

---

## 14. What is Cache Eviction?

**Answer:**

**Cache eviction** is the process of removing keys from Redis when the available memory is full.

Redis uses eviction policies to decide which keys should be removed to make space for new data.

### Redis Eviction Policies

| Policy | Description |
|--------|-------------|
| **noeviction** | Redis does not remove any keys. Write operations fail when memory limit is reached. |
| **allkeys-lru** | Removes the least recently used keys from all keys. |
| **volatile-lru** | Removes the least recently used keys only from keys having TTL set. |
| **allkeys-random** | Removes random keys from all keys. |
| **volatile-random** | Removes random keys only from keys having TTL set. |
| **allkeys-lfu** | Removes the least frequently used keys from all keys. |
| **volatile-lfu** | Removes the least frequently used keys only from keys having TTL set. |

### Eviction Flow

```text
Redis Memory Full
        │
        ▼
Check Eviction Policy
        │
        ▼
Select Keys to Remove
        │
        ▼
Delete Keys
        │
        ▼
Store New Data
```

### Commonly Used Policies

- **allkeys-lru** → Good general-purpose caching strategy
- **volatile-lru** → Useful when only expiring keys should be removed
- **allkeys-lfu** → Useful when frequently accessed data should remain cached

### Configuration Example

```properties
maxmemory 256mb
maxmemory-policy allkeys-lru
```

This configuration limits Redis memory to **256 MB** and removes the least recently used keys when memory is full.

---

## 15. What is LRU (Least Recently Used)?

**Answer:**

**LRU (Least Recently Used)** is a cache eviction strategy where Redis removes the key that has not been accessed for the longest time when memory is required.

Redis tracks the access frequency of keys and removes the least recently used data first.

### Example

Initial Cache:

```text
A
B
C
D
```

Access Pattern:

```text
A → Accessed
C → Accessed
```

Usage Order:

```text
A
C
D
B  ← Least Recently Used
```

Eviction:

```text
Remove B
```

### Redis LRU Policies

- **allkeys-lru** → Removes the least recently used key from all keys.
- **volatile-lru** → Removes the least recently used key only from keys with TTL.

### Use Case

LRU is commonly used for caching scenarios where frequently accessed data should remain available while older, unused data is removed automatically.

---

## 16. What is LFU (Least Frequently Used)?

**Answer:**

**LFU (Least Frequently Used)** is a cache eviction strategy where Redis removes the key that has been accessed the fewest number of times when memory is full.

Redis tracks how frequently each key is accessed and removes the least frequently used data first.

### Example

Cache Access Count:

```text
A → 100 times
B → 2 times
C → 50 times
```

The key with the lowest access count is:

```text
B → 2 times
```

Eviction:

```text
Remove B
```

### Redis LFU Policies

- **allkeys-lfu** → Removes the least frequently used key from all keys.
- **volatile-lfu** → Removes the least frequently used key only from keys with TTL.

### LRU vs LFU

| Feature | LRU | LFU |
|---------|-----|-----|
| Full Form | Least Recently Used | Least Frequently Used |
| Based On | Last access time | Number of accesses |
| Removes | Old unused data | Rarely accessed data |
| Best For | Recently used data matters | Frequently used data matters |

### Use Case

LFU is useful when some data is accessed repeatedly over a long period and should remain cached, while rarely used data can be removed.

---

## 17. What is Cache Penetration?

**Answer:**

**Cache Penetration** occurs when an application receives repeated requests for data that does not exist in both Redis and the database.

Since the data is not available in Redis, every request goes to the database, causing unnecessary database load.

### Example

Request:

```text
User ID = 999999
```

Flow:

```text
Request
   │
   ▼
Redis
   │
   ▼
Cache Miss
   │
   ▼
Database
   │
   ▼
Data Not Found
```

Every repeated request follows the same path:

```text
Request → Redis Miss → Database Miss
```

This can overload the database.

### Solutions

### 1. Cache Null Values

Store a temporary empty result in Redis for non-existing data.

```text
user:999999 → null (TTL: 5 minutes)
```

Future requests return from Redis instead of hitting the database.

---

### 2. Use a Bloom Filter

A Bloom Filter quickly checks whether data may exist before querying Redis or the database.

```text
Request
   │
   ▼
Bloom Filter
   │
   ├── Not Exists → Return Empty
   │
   └── Exists → Check Redis → Database
```

---

### 3. Validate Requests

Reject invalid requests before they reach Redis or the database.

Examples:

- Validate user IDs
- Check input format
- Reject impossible values

### Summary

```text
Cache Penetration

Invalid Data Request
          │
          ▼
     Redis Miss
          │
          ▼
   Database Miss
          │
          ▼
  Repeated DB Load
```

---

## 18. What is Cache Avalanche?

**Answer:**

**Cache Avalanche** occurs when a large number of cache entries expire at the same time, causing many requests to miss the cache simultaneously.

As a result, all requests are redirected to the database, which can create a sudden spike in database traffic and potentially overload the system.

### Example

```text
Many Cache Keys Expire Together

user:101 → Expired
user:102 → Expired
user:103 → Expired
      │
      ▼
Thousands of Requests
      │
      ▼
Redis Cache Miss
      │
      ▼
Database Overload
```

### Solutions

### 1. Randomize TTL

Add random expiration times to prevent all keys from expiring at the same moment.

Example:

```text
user:101 → TTL 30 minutes
user:102 → TTL 34 minutes
user:103 → TTL 28 minutes
```

---

### 2. Use Staggered Expiration

Distribute cache expiration times across different intervals.

```text
Cache Entries

A → Expires at 10:00
B → Expires at 10:05
C → Expires at 10:10
```

---

### 3. Warm the Cache

Preload frequently accessed data into Redis before traffic reaches the application.

```text
Application Start
        │
        ▼
Load Important Data
        │
        ▼
Store in Redis
        │
        ▼
Users Get Cache Hits
```

### Summary

```text
Cache Avalanche

Multiple Keys Expire
          │
          ▼
Large Number of Cache Misses
          │
          ▼
Database Receives Heavy Traffic
          │
          ▼
System Performance Degrades
```

---

## 19. What is Cache Breakdown (Hot Key)?

**Answer:**

**Cache Breakdown** (also called **Hot Key Problem**) occurs when a highly frequently accessed cache key expires suddenly.

Since the key is removed from Redis, many requests try to fetch the same data from the database at the same time, causing a sudden increase in database load.

### Example

A popular product or user profile is stored in Redis:

```text
product:1001 → Expired
```

Millions of users request the same data:

```text
Request 1 ──┐
Request 2 ──┤
Request 3 ──┤
    ...     │
Request N ──┘
             │
             ▼
        Redis Miss
             │
             ▼
        Database
```

The database receives a large number of identical queries simultaneously.

### Solutions

### 1. Use Mutex / Lock

Allow only one request to rebuild the cache while other requests wait.

```text
Request
   │
   ▼
Redis Miss
   │
   ▼
Acquire Lock
   │
   ▼
Query Database
   │
   ▼
Update Redis
   │
   ▼
Release Lock
```

---

### 2. Never Expire Hot Keys

Keep frequently accessed data in Redis without expiration and update it manually when needed.

```text
Hot Key:
product:1001 → No TTL
```

---

### 3. Refresh Asynchronously

Refresh cache data in the background before it expires.

```text
Cache Near Expiry
        │
        ▼
Background Refresh
        │
        ▼
Update Redis
        │
        ▼
Users Continue Getting Data
```

### Cache Breakdown vs Cache Avalanche

| Feature | Cache Breakdown | Cache Avalanche |
|---------|-----------------|-----------------|
| Cause | One hot key expires | Many keys expire together |
| Impact | Heavy load for one data item | Heavy load for many data items |
| Solution | Lock, async refresh | Random TTL, staggered expiration |

---

## 20. What is Distributed Cache?

**Answer:**

A **Distributed Cache** is a caching system shared by multiple application servers. Instead of each server maintaining its own local cache, all servers use a common cache storage such as Redis.

This ensures that cached data is consistent and available across all instances of the application.

### Example

```text
        Server A
            │
        Server B
            │
        Server C
            │
            ▼
          Redis
            │
            ▼
     Shared Cache Data
```

All application servers access the same Redis cache.

### Without Distributed Cache

```text
Server A → Local Cache A
Server B → Local Cache B
Server C → Local Cache C
```

Each server has separate data, which can lead to inconsistency.

### With Distributed Cache

```text
Server A ──┐
Server B ──┼──► Redis
Server C ──┘
```

All servers read and update the same cache.

### Advantages

- Shared cache across multiple servers
- Consistent data between application instances
- Reduces database load
- Supports horizontal scaling
- Useful in microservices architectures

### Common Distributed Cache Technologies

- Redis
- Memcached
- Hazelcast

### Use Case

In a load-balanced application:

```text
User Request
      │
      ▼
Load Balancer
      │
 ┌────┼────┐
 ▼    ▼    ▼
App1 App2 App3
      │
      ▼
    Redis
```

Any server can handle the request because all servers use the same Redis cache.

---

## 21. How does Spring Boot integrate with Redis?

**Answer:**

Spring Boot integrates with Redis using **Spring Data Redis**, which provides easy-to-use APIs for connecting to Redis and performing cache operations.

The `spring-boot-starter-data-redis` dependency provides required Redis libraries and auto-configuration support.

### Dependency

Add the following dependency in `pom.xml`:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

### Redis Configuration

`application.properties`

```properties
spring.redis.host=localhost
spring.redis.port=6379
```

### RedisTemplate Example

Spring Boot automatically configures `RedisConnectionFactory`, which can be used with `RedisTemplate`.

```java
@Autowired
private RedisTemplate<String, Object> redisTemplate;

public void saveUser(User user) {
    redisTemplate.opsForValue()
        .set("user:" + user.getId(), user);
}
```

### Integration Flow

```text
Spring Boot Application
          │
          ▼
 Spring Data Redis
          │
          ▼
    RedisTemplate
          │
          ▼
       Redis Server
```

### Common Spring Redis Components

| Component | Purpose |
|-----------|---------|
| `RedisTemplate` | Perform Redis operations with objects |
| `StringRedisTemplate` | Perform Redis operations with strings |
| `RedisConnectionFactory` | Creates Redis connections |
| `@Cacheable` | Enables annotation-based caching |

### Enable Spring Cache

```java
@EnableCaching
@SpringBootApplication
public class Application {
}
```

Spring Boot can then manage cache operations automatically using Redis as the cache provider.

---

## 22. What is `@Cacheable`?

**Answer:**

`@Cacheable` is a Spring Cache annotation that automatically stores the result of a method in the cache.

When the method is called again with the same parameters, Spring returns the cached result instead of executing the method again.

### Java Example

```java
@Cacheable("users")
public User getUser(Long id) {
    return repository.findById(id).get();
}
```

### Execution Flow

#### First Call

The data is not available in Redis, so the method executes and fetches data from the database.

```text
Request
   │
   ▼
Check Redis
   │
   ▼
Cache Miss
   │
   ▼
Database
   │
   ▼
Store Result in Redis
   │
   ▼
Return User
```

#### Second Call

The data is already cached, so Redis returns the result directly.

```text
Request
   │
   ▼
Check Redis
   │
   ▼
Cache Hit
   │
   ▼
Return User from Redis
```

### Example

```text
First Request:

Application → Redis (Miss) → Database → Redis Store


Second Request:

Application → Redis (Hit) → Return Data
```

### Common Attributes

| Attribute | Purpose |
|-----------|---------|
| `value` / `cacheNames` | Name of the cache |
| `key` | Custom cache key expression |
| `condition` | Cache only when condition is true |
| `unless` | Prevent caching based on result |

### Example with Custom Key

```java
@Cacheable(
    value = "users",
    key = "#id"
)
public User getUser(Long id) {
    return repository.findById(id).get();
}
```

### Benefits

- Reduces database calls
- Improves application performance
- Removes manual cache handling code
- Provides declarative caching support

---

## 23. Difference between `@Cacheable`, `@CachePut`, and `@CacheEvict`

**Answer:**

Spring provides different caching annotations to control how data is stored, updated, and removed from the cache.

### Comparison

| Annotation | Purpose | Behavior |
|------------|---------|----------|
| **`@Cacheable`** | Read from cache and store result if missing | Checks cache first. If data exists, returns cached value. If not, executes method and stores the result. |
| **`@CachePut`** | Always execute method and update cache | Executes the method every time and updates the cache with the latest result. |
| **`@CacheEvict`** | Remove data from cache | Deletes cache entries when data becomes invalid. |

### 1. `@Cacheable`

Used for read operations.

```java
@Cacheable("users")
public User getUser(Long id) {
    return repository.findById(id).get();
}
```

Flow:

```text
Request
   │
   ▼
Check Redis
   │
   ├── Hit → Return Cache Data
   │
   └── Miss → Execute Method → Store Result
```

---

### 2. `@CachePut`

Used when data should always be refreshed.

```java
@CachePut("users")
public User updateUser(User user) {
    return repository.save(user);
}
```

Flow:

```text
Request
   │
   ▼
Execute Method
   │
   ▼
Update Database
   │
   ▼
Update Redis Cache
```

---

### 3. `@CacheEvict`

Used when cached data should be removed.

```java
@CacheEvict(
    value = "users",
    key = "#id"
)
public void deleteUser(Long id) {
    repository.deleteById(id);
}
```

Flow:

```text
Delete Request
      │
      ▼
Delete From Database
      │
      ▼
Remove From Redis Cache
```

### Quick Memory Trick

```text
@Cacheable → Read Cache
@CachePut  → Update Cache
@CacheEvict → Remove Cache
```

### When to Use

| Scenario | Annotation |
|----------|------------|
| Fetch user details | `@Cacheable` |
| Update user details and refresh cache | `@CachePut` |
| Delete user and remove cache | `@CacheEvict` |

---

## 24. What happens if Redis goes down?

**Answer:**

If Redis becomes unavailable, applications cannot access cached data. Since Redis is usually used as a cache layer, the application should continue working by falling back to the database.

### Possible Impacts

- Cache becomes unavailable
- More requests go directly to the database
- Database traffic increases
- Application response time may increase
- Higher load on backend services

### Best Practices

### 1. Graceful Fallback to Database

Handle Redis failures without breaking the application.

```text
Try Redis
    │
    ├── Available → Return Cache Data
    │
    └── Failed → Query Database
```

### 2. Configure Redis Replication or Clustering

Use Redis high-availability features:

- **Replication** → Maintain copies of data across Redis servers
- **Redis Cluster** → Distribute data across multiple nodes


### 3. Monitor Redis Health

Monitor:

- Redis availability
- Memory usage
- Connection status
- Response time
- Error rates

### Summary

```text
Redis Failure
      │
      ▼
Cache Unavailable
      │
      ▼
Fallback to Database
      │
      ▼
Higher Load + Higher Latency
```

A well-designed application should treat Redis as a performance layer and remain functional even when the cache is temporarily unavailable.

---

## 25. How do you prevent cache stampede?

**Answer:**

A **cache stampede** occurs when many requests try to rebuild the same cache entry at the same time after a cache miss or expiration.

This can cause a sudden increase in database requests and overload the database.

### Techniques to Prevent Cache Stampede

### 1. Distributed Locks

Allow only one application instance to rebuild the cache while other requests wait for the result.

```text
Request 1 ──► Acquire Lock ──► Query DB ──► Update Redis
Request 2 ──► Wait for Lock
Request 3 ──► Wait for Lock
```

### 2. Random TTL

Add random expiration times so multiple cache entries do not expire simultaneously.

Example:

```text
User A → TTL 30 minutes
User B → TTL 35 minutes
User C → TTL 28 minutes
```


### 3. Background Cache Refresh

Refresh cache data before it expires instead of waiting for a cache miss.

```text
Cache Near Expiry
        │
        ▼
Background Job Refreshes Data
        │
        ▼
Redis Updated
        │
        ▼
Users Get Cache Hits
```

### 4. Cache Warming

Preload frequently accessed data into Redis before users request it.

```text
Application Startup
        │
        ▼
Load Popular Data
        │
        ▼
Store in Redis
        │
        ▼
Serve Requests from Cache
```

### 5. Request Coalescing

Combine multiple identical requests into a single database request.

```text
Request A ──┐
Request B ──┤
Request C ──┘
             │
             ▼
        Single DB Query
             │
             ▼
        Update Redis
             │
             ▼
      Return Same Result
```

### Summary

| Technique | Purpose |
|-----------|---------|
| Distributed Lock | Prevent multiple cache rebuilds |
| Random TTL | Avoid simultaneous expiration |
| Background Refresh | Refresh before expiry |
| Cache Warming | Preload important data |
| Request Coalescing | Reduce duplicate database calls |

---

## 26. Explain Redis Pub/Sub

**Answer:**

**Redis Pub/Sub (Publish/Subscribe)** is a messaging mechanism where publishers send messages to channels, and subscribers receive messages from those channels.

A publisher does not send messages directly to a subscriber. Instead, it publishes messages to a channel, and all subscribers listening to that channel receive the message.

### Flow

```text
Publisher
    │
    ▼
 Redis Channel
    │
    ├──────────────┐
    ▼              ▼
Subscriber A   Subscriber B
```

### Publisher Example

```java
redisTemplate.convertAndSend(
    "orders",
    "New Order"
);
```

This publishes the message to the `orders` channel.

### Subscriber

Subscribers listen to the channel:

```text
Channel: orders

Message:
"New Order"
```

When a message is published, all active subscribers receive it.

### Example Use Case

Order notification system:

```text
Order Service
      │
      ▼
 Publish Message
      │
      ▼
Redis Channel: orders
      │
 ┌────┴────┐
 ▼         ▼
Email    Notification
Service  Service
```

### Common Use Cases

- Real-time notifications
- Application events
- Chat messages
- Lightweight messaging
- Cache invalidation signals

### Advantages

- Simple message broadcasting
- Low latency communication
- Multiple subscribers can receive the same message

### Limitations

- Messages are not stored
- Offline subscribers miss messages
- Not suitable for guaranteed message delivery

For reliable message processing with persistence, Redis Streams or dedicated message brokers are usually preferred.

---

## 27. Difference between Redis and Ehcache

**Answer:**

Both **Redis** and **Ehcache** are in-memory caching solutions, but they differ mainly in their architecture and usage scenarios.

### Comparison

| Feature | Redis | Ehcache |
|---------|-------|---------|
| Architecture | Distributed cache | Local JVM cache |
| Data Location | Separate in-memory server | Inside application process |
| Sharing | Shared across multiple servers | Per application instance |
| Deployment | Requires Redis server | Embedded Java library |
| Best For | Microservices and distributed systems | Standalone applications |
| Scalability | Highly scalable with clustering | Limited to application instance |
| Network | Requires network communication | No network overhead |
| Persistence | Supports optional persistence | Primarily local memory/disk storage |

### Redis

```text
Application Server A ──┐
Application Server B ──┼──► Redis Server
Application Server C ──┘
```

- Centralized shared cache
- Suitable for distributed applications
- All servers access the same data

### Ehcache

```text
Application Server A
        │
        ▼
   Local JVM Cache

Application Server B
        │
        ▼
   Local JVM Cache
```

- Cache exists inside each application instance
- Faster because there is no network call
- Data is not automatically shared between servers

### When to Use

| Scenario | Recommended Cache |
|----------|-------------------|
| Microservices with multiple instances | Redis |
| Distributed session storage | Redis |
| Shared cache across servers | Redis |
| Single Spring Boot application | Ehcache |
| Local method-level caching | Ehcache |

### Summary

```text
Redis  → Distributed, shared, microservices-friendly cache

Ehcache → Local, JVM-based, single-application cache
```

---

## 28. Difference between Redis and Memcached

**Answer:**

Both **Redis** and **Memcached** are popular in-memory caching systems, but Redis provides more advanced features and data structures.

### Comparison

| Feature | Redis | Memcached |
|---------|-------|-----------|
| Data Structures | Supports String, Hash, List, Set, Sorted Set, Stream, etc. | Strings only |
| Persistence | Supports optional data persistence | No persistence support |
| Pub/Sub | Supported | Not supported |
| Transactions | Supported | Not supported |
| Lua Scripting | Supported | Not supported |
| Data Operations | Rich operations on different data types | Basic key-value operations |
| Replication | Supported | Limited |
| Use Case | Cache, database, messaging, real-time applications | Simple high-speed caching |

### Redis

```text
Redis
 │
 ├── String
 ├── Hash
 ├── List
 ├── Set
 ├── Sorted Set
 ├── Pub/Sub
 ├── Transactions
 └── Lua Scripts
```

### Memcached

```text
Memcached

Key → String Value
```

### When to Use

| Scenario | Recommended |
|----------|-------------|
| Simple object caching | Memcached |
| Distributed caching | Redis |
| Real-time applications | Redis |
| Leaderboards and rankings | Redis |
| Message notifications | Redis |
| Basic temporary cache | Memcached |

### Summary

```text
Redis      → Feature-rich in-memory data store

Memcached  → Simple, lightweight key-value cache
```

Redis is generally preferred when applications need advanced caching features beyond simple key-value storage.


---

## 29. What are Redis Transactions?

**Answer:**

Redis Transactions allow multiple Redis commands to be executed as a single atomic operation.

A transaction groups multiple commands together, queues them, and executes them together using `MULTI` and `EXEC`.

### Redis Transaction Commands

```text
MULTI

SET A 1

SET B 2

EXEC
```

### Spring Boot Example

```java
redisTemplate.multi();

redisTemplate.opsForValue()
    .set("A", "1");

redisTemplate.opsForValue()
    .set("B", "2");

redisTemplate.exec();
```

### Advantages

- Groups multiple commands together
- Ensures commands execute in sequence
- Reduces network round trips
- Useful for related updates

### Important Notes

- Redis transactions do not support automatic rollback like traditional database transactions.
- Commands are executed sequentially.
- Other clients cannot interrupt the execution of commands inside a transaction.

### Common Use Cases

- Updating multiple related keys
- Maintaining counters
- Atomic cache updates
- Ensuring consistent state changes


---

## 30. How would you design a high-performance caching solution?

**Answer:**

A high-performance caching solution should reduce database load, improve response time, and maintain data consistency between the cache and the database.

### High-Level Design

```text
Client Request
       │
       ▼
 Check Redis Cache
       │
 ┌─────┴─────┐
 ▼           ▼
Cache Hit   Cache Miss
 │           │
 ▼           ▼
Return     Query Database
Data           │
               ▼
        Store Data in Redis
               │
               ▼
          Return Response
```

### Implementation Approach

### 1. Use Cache Aside Pattern

Flow:

1. Client requests data
2. Check Redis
3. If cache hit → return cached value
4. If cache miss → fetch from database
5. Store result in Redis with TTL
6. Return response

---

### 2. Use Spring Cache Annotations

For read-heavy operations:

```java
@Cacheable("users")
public User getUser(Long id) {
    return userRepository.findById(id).get();
}
```

For updates:

```java
@CachePut("users")
public User updateUser(User user) {
    return userRepository.save(user);
}
```

For deletes:

```java
@CacheEvict(
    value = "users",
    key = "#id"
)
public void deleteUser(Long id) {
    userRepository.deleteById(id);
}
```

---

### 3. Configure Proper TTL

Use expiration to avoid stale data.

```text
User Cache
    │
    ▼
TTL = 30 minutes
    │
    ▼
Automatic Expiration
```

Add random TTL values to prevent multiple keys from expiring at the same time.

Example:

```text
User A → 30 minutes
User B → 34 minutes
User C → 28 minutes
```

---

### 4. Handle Cache Failures

- Gracefully fall back to the database
- Monitor Redis availability
- Avoid making Redis a single point of failure

---

### 5. Prevent Cache Problems

| Problem | Solution |
|---------|----------|
| Cache Penetration | Cache null values, Bloom Filter, validation |
| Cache Avalanche | Random TTL, staggered expiration, cache warming |
| Cache Breakdown | Distributed locks, async refresh |
| Cache Stampede | Request coalescing, locks |

---

### 6. Monitor Cache Performance

Monitor:

- Cache hit ratio
- Cache miss ratio
- Response latency
- Memory usage
- Eviction rate
- Redis availability

---

### 7. Ensure High Availability and Scalability

Use Redis high-availability features:

- **Redis Sentinel** → Automatic failover and monitoring
- **Redis Cluster** → Data partitioning and horizontal scaling

### Final Architecture

```text
                 Client
                   │
                   ▼
            Application Server
                   │
          ┌────────┴────────┐
          ▼                 ▼
       Redis Cache       Database
          │
          ▼
  TTL + Eviction Policy
  Monitoring + HA Setup
```

### Summary

A high-performance caching solution uses:

- Redis as a distributed cache
- Cache Aside pattern
- Proper TTL management
- Spring caching annotations
- Cache invalidation strategies
- Monitoring and high-availability configuration

---

# Quick Interview Tips

Interviewers often ask practical Redis and caching questions to understand real-world design decisions.

---

## 1. Why is Redis faster than a relational database?

**Answer:**

Redis is faster because it stores data primarily in **memory (RAM)**, while relational databases usually require disk I/O operations.

Reasons:

- In-memory data access
- Simple key-value lookup
- No complex SQL parsing or joins
- Optimized data structures
- Low network overhead for cache operations

```text
Relational Database

Application
     │
     ▼
 Disk / Index / Query Processing
     │
     ▼
 Result


Redis

Application
     │
     ▼
 RAM Lookup
     │
     ▼
 Result
```

---

## 2. When should you avoid caching?

**Answer:**

Avoid caching when:

- Data changes very frequently
- Data must always be real-time
- Data is rarely accessed
- Data size is too large
- Cache consistency is difficult to maintain

Examples:

- Banking transaction balances requiring strict consistency
- Frequently changing inventory counts

---

## 3. How do you handle stale cache data?

**Answer:**

Common approaches:

- Set appropriate TTL values
- Update cache after database updates
- Remove cache entries using `@CacheEvict`
- Refresh cache asynchronously
- Use cache versioning

Example:

```text
Update Database
       │
       ▼
Invalidate Redis Cache
       │
       ▼
Next Request Loads Fresh Data
```

---

## 4. What happens when Redis memory is exhausted?

**Answer:**

When Redis reaches its memory limit, it applies the configured eviction policy.

Possible actions:

- Remove old keys using LRU/LFU policies
- Reject new writes (`noeviction`)
- Remove keys with TTL

Common configuration:

```properties
maxmemory-policy allkeys-lru
```

---

## 5. Difference between Cache Aside and Write Through?

| Feature | Cache Aside | Write Through |
|---------|-------------|---------------|
| Write Flow | Application writes DB, then updates cache manually | Application writes cache and DB together |
| Cache Management | Application controls cache | Cache layer manages updates |
| Complexity | Simple | More complex |
| Common Usage | Most popular pattern | Systems requiring strong consistency |

### Cache Aside

```text
Read:
Application → Redis → Database (if miss)

Write:
Application → Database → Redis
```

### Write Through

```text
Application
     │
     ▼
 Cache Layer
     │
 ├── Redis
 └── Database
```

---

## 6. How do you cache database query results in Spring Boot?

**Answer:**

Use Spring Cache annotations.

Example:

```java
@Cacheable(
    value = "users",
    key = "#id"
)
public User getUser(Long id) {
    return repository.findById(id).get();
}
```

Flow:

```text
First Request:
Application → Redis Miss → Database → Store Cache


Second Request:
Application → Redis Hit → Return Data
```

---

## 7. How do you invalidate cache after updating data?

**Answer:**

Use cache eviction or update the cache after database changes.

### Using `@CacheEvict`

```java
@CacheEvict(
    value = "users",
    key = "#id"
)
public void deleteUser(Long id) {
    repository.deleteById(id);
}
```

### Using `@CachePut`

```java
@CachePut("users")
public User updateUser(User user) {
    return repository.save(user);
}
```

---

## 8. What are Cache Penetration, Cache Avalanche, and Cache Breakdown?

| Problem | Description | Solution |
|---------|-------------|----------|
| Cache Penetration | Requests for non-existing data bypass cache | Cache null values, Bloom Filter |
| Cache Avalanche | Many keys expire together | Random TTL, cache warming |
| Cache Breakdown | Hot key expires causing heavy DB load | Locks, async refresh |

---

## 9. How do you monitor Redis performance?

Monitor:

- Cache hit ratio
- Cache miss ratio
- Memory usage
- Eviction count
- CPU usage
- Connected clients
- Command latency
- Network usage

Useful tools:

- Redis CLI monitoring
- Application metrics
- Prometheus + Grafana dashboards

Example:

```text
Redis Health

Memory     ✓
Latency    ✓
Hit Ratio  ✓
Evictions  ✓
Connections ✓
```

---

## 10. How would you scale Redis in a production microservices environment?

**Answer:**

Use Redis high-availability and scaling features.

### Redis Sentinel

Provides:

- Monitoring
- Automatic failover
- High availability

### Redis Cluster

Provides:

- Data sharding
- Horizontal scaling
- Large dataset support

Architecture:

```text
             Load Balancer
                   │
        ┌──────────┼──────────┐
        ▼          ▼          ▼
     Service A  Service B  Service C
        │          │          │
        └──────────┼──────────┘
                   ▼
              Redis Cluster
                   │
              Database
```

### Production Best Practices

- Use proper TTL values
- Configure eviction policies
- Enable monitoring and alerts
- Use replication/clustering
- Handle Redis failures gracefully
- Monitor cache hit ratio

---

## Final Interview Summary

A strong Redis design should include:

✅ Cache Aside pattern  
✅ Proper TTL management  
✅ Cache invalidation strategy  
✅ Handling penetration, avalanche, and breakdown  
✅ Monitoring and alerting  
✅ High availability using Sentinel/Cluster  
✅ Graceful fallback when Redis is unavailable  
