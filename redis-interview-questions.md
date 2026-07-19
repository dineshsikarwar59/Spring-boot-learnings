# Redis interview questions 

## 1. What is Redis?
**Answer:** 
Redis (REmote DIctionary Server) is an open-source, in-memory data store used primarily as a database, cache, and message broker. It supports key-value data structures and is known for its high performance and low latency.

Redis is commonly used to:
- Speed up applications by caching frequently accessed data.
- Store session data for web applications.
- Implement real-time features (e.g., leaderboards, pub/sub messaging).

## 2. What is redis key features?
**Answer:** 

1. **In-Memory Storage**
     - Data is stored in RAM, which makes it extremely fast (sub-millisecond latency).
     - Suitable for caching and scenarios where speed is critical.

2. **Rich Data Types:** 
   Redis supports several advanced data types beyond simple strings:
    - Strings – basic key-value pairs
    - Lists – ordered sequences
    - Sets – unordered collections of unique elements
    - Sorted Sets – sets ordered by score
    - Hashes – key-value pairs within a key (like dictionaries)
    - Bitmaps, HyperLogLogs, and Streams

3. **Persistence Options:** 
Redis offers multiple persistence mechanisms:
    - **RDB (Snapshotting):** Saves the dataset at specified intervals.
    - **AOF (Append-Only File):** Logs every write operation for durability.
You can configure it for in-memory only, disk-persistent, or hybrid usage.


4. **Pub/Sub Messaging:** 
Supports publish/subscribe messaging patterns, making it great for real-time chat, notifications, or streaming systems.

5. **Replication and High Availability**
   - Master-slave replication for scalability.
   - Redis Sentinel provides high availability and automatic failover.
   - Redis Cluster for sharding and horizontal scaling.


6. **Atomic Operations:** 
All operations in Redis are atomic, meaning they are completed fully or not at all — important for concurrent systems.

7. **Lua Scripting:** 
You can run Lua scripts directly on the server for batch operations or to reduce round trips.

8. **Lightweight and Fast**
Redis is highly optimized and can handle millions of requests per second with minimal resource usage.

**Common Use Cases**

- Caching (e.g., database query results)
- Session storage
- Real-time analytics
- Message queues
- Leaderboards/gaming
- Rate limiting
