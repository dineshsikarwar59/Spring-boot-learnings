# Kafka Java Interview Questions

## 1. What is Kafka?

**Answer:**

Apache Kafka is a distributed event streaming platform used for:

- Real-time data streaming
- Message queues
- Event-driven architectures
- Log aggregation
- Data integration

### It provides:

- High throughput
- Fault tolerance
- Scalability
- Durability

---

## 2. Why use Kafka instead of RabbitMQ?

| Kafka | RabbitMQ |
|--------|----------|
| Distributed log | Traditional message broker |
| Very high throughput | Lower throughput |
| Stores messages for configurable retention | Deletes messages after acknowledgment by default |
| Best for event streaming | Best for task queues |
| Pull-based consumers | Push-based consumers |

---

## 3. Explain Kafka Architecture

### Main Components

- **Producer** – Sends messages (events) to Kafka topics.
- **Consumer** – Reads messages from Kafka topics.
- **Broker** – A Kafka server that stores and manages messages.
- **Topic** – A logical channel where messages are published.
- **Partition** – A topic is divided into partitions for scalability and parallel processing.
- **Offset** – A unique identifier for each message within a partition.
- **ZooKeeper (Older Versions)** – Managed broker metadata and cluster coordination.
- **KRaft (New Versions)** – Replaces ZooKeeper by handling metadata management within Kafka itself.

### Architecture Flow

```text
Producer
    |
    V
+----------------+
|     Broker     |
|----------------|
|    Topic A     |
|  Partition 0   |
|  Partition 1   |
+----------------+
    |
    V
Consumer Group
```

---

## 4. What is a Topic?

A **Topic** is a logical channel in Kafka where messages (events) are stored and organized.

### Examples

- `orders`
- `payments`
- `users`
- `notifications`

### How it works

- **Producers** write messages to topics.
- **Consumers** read messages from topics.

### Example

```text
Producer
    |
    V
+-----------------+
|  Topic: orders  |
+-----------------+
    |
    V
Consumer
```

---

## 5. What is a Partition?

A **Partition** is a division of a Kafka topic that allows messages to be distributed across multiple logs. Partitions enable Kafka to process data in parallel and scale efficiently.

### Example

**Topic:** `orders`

```text
Orders Topic
├── Partition 0
├── Partition 1
└── Partition 2
```

### Benefits

- **Parallel Processing** – Multiple consumers can process different partitions simultaneously.
- **Scalability** – Partitions can be distributed across multiple Kafka brokers.
- **Load Balancing** – Consumer groups automatically distribute partitions among consumers.

### Architecture

```text
                Orders Topic
                     |
    +----------------+----------------+
    |                |                |
    V                V                V
Partition 0     Partition 1     Partition 2
    |                |                |
    +--------+-------+--------+-------+
             |                |
         Consumer 1      Consumer 2
```

---

## 6. What is an Offset?

An **Offset** is a unique sequential number assigned to each message within a Kafka partition. It identifies the position of a message and helps consumers keep track of which messages have already been read.

### Example

```text
Partition 0

Offset 0  --> Message 1
Offset 1  --> Message 2
Offset 2  --> Message 3
Offset 3  --> Message 4
```

### Why are Offsets Important?

- **Track Progress** – Consumers know which messages have already been processed.
- **Resume Processing** – Consumers can continue reading from the last committed offset after a restart.
- **Replay Messages** – Consumers can re-read messages by resetting their offset.
- **Fault Tolerance** – Prevents data loss and supports reliable message processing.

### Consumer Flow

```text
Producer
    |
    V
+------------------------+
|      Partition 0       |
|------------------------|
| Offset 0 -> Message 1  |
| Offset 1 -> Message 2  |
| Offset 2 -> Message 3  |
| Offset 3 -> Message 4  |
+------------------------+
            |
            V
Consumer (Current Offset: 2)
```

---

## 7. What is a Consumer Group?

A **Consumer Group** is a group of consumers that work together to read messages from a Kafka topic. Kafka distributes the topic's partitions among the consumers in the group, allowing messages to be processed in parallel.

### Example

**Topic:** `orders`

```text
Orders Topic

Partition 0  --> Consumer 1
Partition 1  --> Consumer 2
Partition 2  --> Consumer 3
```

### Key Points

- Multiple consumers can belong to the same consumer group.
- Each partition is consumed by **only one consumer** within the same group.
- Different consumer groups can read the **same topic independently**.
- Consumer groups provide **load balancing** and **fault tolerance**.

### Architecture

```text
                 Orders Topic
                      |
      +---------------+---------------+
      |               |               |
      V               V               V
 Partition 0     Partition 1     Partition 2
      |               |               |
      V               V               V
 Consumer 1      Consumer 2      Consumer 3
      \_______________ Consumer Group A _______________/
```

> **Note:** If there are more consumers than partitions, the extra consumers remain idle until a partition becomes available.

---

## 8. Difference between Consumer and Consumer Group

| Consumer | Consumer Group |
|----------|----------------|
| A single application that reads messages from a Kafka topic. | A collection of consumers working together to read messages from a Kafka topic. |
| Reads messages independently. | Distributes partitions among consumers for parallel processing. |
| Processes messages assigned to it. | Provides load balancing and fault tolerance. |
| Can belong to a consumer group. | Can contain one or more consumers. |

### Example

```text
Orders Topic

Partition 0  --> Consumer 1
Partition 1  --> Consumer 2
Partition 2  --> Consumer 3

Consumer Group: Order-Service
```

### Key Points

- A **Consumer** is a single application that reads messages from Kafka.
- A **Consumer Group** is a collection of consumers working together.
- Within the same consumer group, each partition is assigned to **only one consumer**.
- Consumer groups improve **scalability**, **parallel processing**, and **fault tolerance**.

---

## 9. What happens if a Consumer fails?

When a consumer in a Kafka consumer group fails, Kafka performs **rebalancing**. During rebalancing, Kafka redistributes the failed consumer's partitions among the remaining active consumers in the group.

### Example

#### Before Consumer Failure

```text
Consumer Group

C1 --> Partition 0
C2 --> Partition 1
```

#### After C2 Crashes

```text
Consumer Group

C1 --> Partition 0
C1 --> Partition 1
```

### Key Points

- Kafka automatically detects consumer failures using heartbeats.
- Partitions are reassigned to available consumers through rebalancing.
- Processing continues without manual intervention.
- No messages are lost if offsets are managed correctly.
- Proper offset management ensures consumers resume from the correct position after recovery.

---

## 10. Explain Replication

**Replication** is the process of maintaining multiple copies (**replicas**) of a Kafka partition across different brokers. It provides **fault tolerance** and ensures data availability even if a broker fails.

### Example

```text
Partition 0

Broker 1 --> Leader
Broker 2 --> Replica
Broker 3 --> Replica
```

### Broker Failure Scenario

If the leader broker fails:

```text
Before:

Broker 1 --> Leader
Broker 2 --> Replica
Broker 3 --> Replica


After Broker 1 Failure:

Broker 2 --> New Leader
Broker 3 --> Replica
```

### Key Points

- The **leader** handles all read and write requests for a partition.
- **Followers/replicas** copy data from the leader.
- If the leader fails, Kafka elects a new leader from the available replicas.
- Replication provides:
  - Fault tolerance
  - High availability
  - Data durability

---

## 11. What is ISR?

**ISR (In-Sync Replicas)** are replicas of a Kafka partition that are fully synchronized with the leader. These replicas have the latest data and are eligible to become the new leader if the current leader fails.

### Example

```text
Partition 0

Leader       ✓
Replica 1    ✓  (In-Sync)
Replica 2    ✓  (In-Sync)
Replica 3    ✗  (Out of Sync)
```

### Key Points

- ISR contains replicas that are up-to-date with the leader.
- Only replicas in the **ISR list** can be elected as a new leader.
- Out-of-sync replicas are removed from the ISR until they catch up.
- ISR helps maintain **data consistency** and **high availability**.

### Leader Election Example

```text
Before Failure:

Broker 1 --> Leader
Broker 2 --> ISR Replica
Broker 3 --> ISR Replica


After Broker 1 Failure:

Broker 2 --> New Leader
Broker 3 --> Replica
```

---

## 12. Leader and Follower

In Kafka, each partition has one **leader** and one or more **followers**. The leader handles client requests, while followers maintain copies of the partition data.

### Leader

- Handles all **read and write requests** for the partition.
- Receives messages from producers.
- Serves data to consumers.
- Replicates data to followers.

### Follower

- Copies data from the leader.
- Maintains a replica of the partition.
- Can become the leader if the current leader fails.

### Example

```text
Partition 0

Broker 1 --> Leader
              |
              |
      +-------+-------+
      |               |
      V               V
Broker 2 --> Follower
Broker 3 --> Follower
```

### Key Points

- Producers always write to the **leader**.
- Consumers usually read from the **leader**.
- Followers provide **fault tolerance** and **high availability**.
- A follower can be promoted to leader during leader failure.

---

## 13. What is a Producer?

A **Producer** is an application that publishes (writes) messages or events to Kafka topics.

Producers send data to a specific topic, and Kafka stores the messages in topic partitions.

### Example

```java
Producer<String, String> producer =
        new KafkaProducer<>(properties);

producer.send(
        new ProducerRecord<>("orders", "Order Created"));
```

### How Producer Works

```text
Producer
    |
    |  Message/Event
    V
+----------------+
| Kafka Topic    |
|   orders       |
+----------------+
    |
    V
Partition
```

### Key Points

- Producers write messages to Kafka topics.
- Messages are distributed across topic partitions.
- Producers can choose a partition using:
  - Message key
  - Custom partitioner
  - Round-robin strategy
- Producers provide features like:
  - High throughput
  - Message batching
  - Compression
  - Delivery acknowledgment (`acks`)

---

## 14. What is a Consumer?

A **Consumer** is an application that reads messages or events from Kafka topics. Consumers subscribe to topics and process the messages produced by Kafka producers.

### Example

```java
KafkaConsumer<String, String> consumer =
      new KafkaConsumer<>(props);

consumer.subscribe(List.of("orders"));
```

### How Consumer Works

```text
Kafka Topic
    |
    V
+----------------+
|    orders      |
|  Partition 0   |
|  Partition 1   |
+----------------+
    |
    V
Consumer
```

### Key Points

- Consumers read messages from Kafka topics.
- Consumers use **consumer groups** for parallel message processing.
- Each partition is assigned to only one consumer within the same group.
- Consumers track message processing using **offsets**.
- Consumers can resume processing from the last committed offset after a restart.

---

## 15. What are Producer Acknowledgments?

**Producer Acknowledgment (`acks`)** controls the level of confirmation Kafka requires from brokers before considering a message successfully written.

### Types of Acknowledgments

| Configuration | Description | Performance | Data Safety |
|--------------|-------------|-------------|-------------|
| `acks=0` | Producer does not wait for any acknowledgment from the broker. | Fastest | Possible data loss |
| `acks=1` | Leader broker acknowledges the message after writing it. | Faster | Moderate safety |
| `acks=all` | All in-sync replicas acknowledge the message. | Slower | Safest option |

### Example

```text
acks=0

Producer
   |
   V
Broker
(No confirmation)


acks=1

Producer
   |
   V
Leader Broker
   |
   V
Acknowledgment


acks=all

Producer
   |
   V
Leader Broker
   |
   +--> Replica 1 ✓
   +--> Replica 2 ✓
   
All ISR replicas acknowledge
```

### Key Points

- `acks=0` provides maximum speed but lower reliability.
- `acks=1` provides leader-level confirmation.
- `acks=all` provides the highest durability by waiting for all in-sync replicas.
- For critical systems, `acks=all` is commonly preferred.

---

## 16. What is an Idempotent Producer?

An **Idempotent Producer** is a Kafka producer feature that prevents duplicate messages from being written to a topic, even if retries occur due to network failures or broker issues.

### Enable Idempotence

```properties
enable.idempotence=true
```

### Benefits

- Prevents duplicate messages.
- Provides exactly-once message production semantics.
- Handles producer retries safely.
- Improves data consistency.

### Example Scenario

Without Idempotence:

```text
Producer
   |
   V
Send Message
   |
Network Failure
   |
Retry
   |
Duplicate Message Created
```

With Idempotence:

```text
Producer
   |
   V
Send Message
   |
Network Failure
   |
Retry
   |
Kafka Detects Duplicate
   |
Single Message Stored
```

### Key Points

- Kafka assigns a unique **Producer ID (PID)** and sequence number to messages.
- The broker uses these identifiers to detect and remove duplicates.
- Commonly used in applications where duplicate events can cause problems.
- Helps achieve **exactly-once processing** when combined with Kafka transactions.

---

## 17. What is Exactly Once Semantics?

**Exactly Once Semantics (EOS)** guarantees that a message is processed only once, even if failures, retries, or restarts occur.

It prevents:
- Duplicate message production
- Duplicate processing
- Inconsistent results

### Achieved Using

- **Idempotent Producer** – Prevents duplicate messages during retries.
- **Kafka Transactions** – Ensures multiple operations are completed atomically.
- **Kafka Streams** – Provides exactly-once processing for stream applications.

### Example

Without Exactly Once:

```text
Producer
    |
    V
Send Message
    |
Failure occurs
    |
Retry
    |
Duplicate Message
```

With Exactly Once:

```text
Producer
    |
    V
Send Message
    |
Failure occurs
    |
Retry
    |
Kafka removes duplicate
    |
Message processed once
```

### Key Points

- Ensures reliable event processing.
- Important for financial transactions, payments, and order processing systems.
- Combines producer idempotence, transactions, and consumer offset management.

---

## 18. What is At Most Once Delivery?

**At Most Once Delivery** is a message delivery strategy where a message is delivered **zero or one time**. Kafka does not retry failed messages, so duplicates are avoided but messages may be lost.

### Characteristics

- No duplicate messages.
- Possible message loss.
- Provides lower latency compared to stronger delivery guarantees.
- Suitable for applications where occasional data loss is acceptable.

### Example

```text
Producer
    |
    V
Send Message
    |
    V
Consumer

If failure occurs:
Message is not retried
Message may be lost
```

### Key Points

- Message is processed **at most one time**.
- No duplicate processing occurs.
- Data loss is possible if failures happen before successful processing.
- Used when speed is more important than guaranteed delivery.

---

## 19. What is At Least Once Delivery?

**At Least Once Delivery** is a message delivery strategy where Kafka guarantees that a message is delivered **one or more times**. It ensures that messages are not lost, but duplicate messages may occur.

### Characteristics

- Messages are never lost.
- Duplicate messages are possible.
- Requires message processing to handle duplicates.
- Most commonly used delivery guarantee in Kafka applications.

### Example

```text
Producer
    |
    V
Send Message
    |
    V
Broker
    |
    V
Consumer

If acknowledgment fails:
Message is retried
Duplicate message may occur
```

### Key Points

- Message is processed **at least one time**.
- Kafka retries failed message deliveries.
- Consumers should implement **idempotent processing** to handle duplicates.
- Provides better reliability than **At Most Once Delivery**.
- Commonly used for event-driven systems where data loss is unacceptable.

---

## 20. What is Exactly Once Delivery?

**Exactly Once Delivery** is a message delivery guarantee where each message is processed **only one time**. It ensures that messages are neither lost nor duplicated during processing.

### Characteristics

- No duplicate messages.
- No message loss.
- Provides the highest level of reliability.
- Ensures consistent processing results.

### Example

```text
Producer
    |
    V
Send Message
    |
    V
Kafka Broker
    |
    V
Consumer

Message processed exactly once
(No loss + No duplicate)
```

### Achieved Using

- **Idempotent Producer** – Prevents duplicate message writes.
- **Kafka Transactions** – Ensures atomic processing of multiple operations.
- **Consumer Offset Management** – Tracks successful message processing.

### Key Points

- Provides the strongest delivery guarantee in Kafka.
- Useful for critical systems such as payments, banking, and order processing.
- Requires additional configuration and processing overhead compared to At Most Once and At Least Once delivery.

---

## 21. What is Serialization?

**Serialization** is the process of converting data or Java objects into a byte format that can be transmitted and stored by Kafka.

Kafka works with **bytes**, so producers must serialize messages before sending them to Kafka.

### Common Serializers

- **StringSerializer** – Converts strings into bytes.
- **JsonSerializer** – Converts JSON objects into bytes.
- **AvroSerializer** – Converts Avro objects into bytes with schema support.

### Example

```text
Java Object
     |
     V
Serialization
     |
     V
Bytes
     |
     V
Kafka Topic
```

### Producer Example

```java
ProducerRecord<String, String> record =
    new ProducerRecord<>("orders", "Order Created");
```

### Key Points

- Producers serialize messages before sending them to Kafka.
- Consumers deserialize bytes back into objects.
- Serialization format must be compatible between producers and consumers.
- Common formats:
  - String
  - JSON
  - Avro
  - Protobuf

---

## 22. Difference between Serialization and Deserialization

Serialization and deserialization are processes used to convert data between Java objects and byte formats so that Kafka can transmit and store messages.

| Serialization | Deserialization |
|--------------|-----------------|
| Converts Java objects into bytes. | Converts bytes back into Java objects. |
| Used by producers before sending messages to Kafka. | Used by consumers after receiving messages from Kafka. |
| Data flow: Object → Bytes | Data flow: Bytes → Object |

### Serialization Flow

```text
Java Object
     |
     V
Serialization
     |
     V
Bytes
     |
     V
Kafka Topic
```

### Deserialization Flow

```text
Kafka Topic
     |
     V
Bytes
     |
     V
Deserialization
     |
     V
Java Object
```

### Key Points

- Producers use **serializers** to convert messages into bytes.
- Consumers use **deserializers** to convert bytes back into readable objects.
- Both producer and consumer must use compatible serialization formats.
- Common formats include:
  - String
  - JSON
  - Avro
  - Protobuf

---

## 23. What is Kafka Retention?

**Kafka Retention** defines how long Kafka stores messages in a topic before they are automatically deleted.

Kafka stores messages based on a configurable retention period, regardless of whether consumers have already read them.

### Examples

```text
Retention Period

1 hour
7 days
30 days
```

### How It Works

```text
Producer
    |
    V
Kafka Topic
    |
    |  Message stored
    |
    V
Retention Period Expires
    |
    V
Message Deleted
```

### Key Points

- Messages remain in Kafka even after consumers read them.
- Consumers can replay messages within the retention period.
- Retention is configured using topic-level settings.
- Kafka can delete messages based on:
  - **Time-based retention** (example: 7 days)
  - **Size-based retention** (example: delete old messages when storage limit is reached)
- Retention provides durability and supports event replay.

---

## 24. Why are messages not deleted after consumption?

Kafka does **not delete messages immediately after they are consumed** because Kafka works as a **distributed log** rather than a traditional message queue.

Consumers track their own progress using **offsets**, allowing messages to be read again when needed.

### Reasons

- **Distributed Log** – Kafka stores messages in topics as an append-only log.
- **Consumer Offsets** – Consumers maintain offsets to track which messages they have processed.
- **Multiple Consumer Groups** – Different consumer groups can independently read the same messages.

### Example

```text
Topic: orders

Offset 0 --> Order Created
Offset 1 --> Order Paid
Offset 2 --> Order Shipped
```

```text
Consumer Group A
    |
    Reads from Offset 0


Consumer Group B
    |
    Reads from Offset 0
```

Both consumer groups can read the same data independently.

### Key Points

- Messages are deleted based on **retention policy**, not after consumption.
- Multiple applications can consume the same topic data.
- Consumers can replay messages by resetting offsets.
- Kafka supports event replay and historical data processing.

---

## 25. What is Rebalancing?

**Rebalancing** is the process in Kafka where partitions are redistributed among consumers in a consumer group to maintain balanced message processing.

Kafka automatically performs rebalancing when the consumer group membership or topic structure changes.

### When Does Rebalancing Happen?

- A new consumer **joins** the consumer group.
- A consumer **leaves** the consumer group.
- A consumer **fails**.
- The number of **partitions increases**.

### Example

#### Before Rebalancing

```text
Consumer Group

C1 --> Partition 0
C2 --> Partition 1
```

#### New Consumer Joins

```text
Consumer Group

C1 --> Partition 0
C2 --> Partition 1
C3 --> Partition 2
```

### Key Points

- Kafka automatically redistributes partitions among active consumers.
- Rebalancing provides scalability and fault tolerance.
- During rebalancing, message consumption may temporarily pause.
- After rebalancing completes, consumers continue processing from their committed offsets.

---

## 26. What causes Consumer Lag?

**Consumer Lag** is the difference between the latest message offset available in a Kafka partition and the offset that a consumer has already processed.

It means the consumer is **behind the latest offset** and has pending messages to process.

### Example

```text
Partition 0

Latest Offset:      100
Consumer Offset:     80

Consumer Lag = 20 messages
```

### Reasons for Consumer Lag

- **Slow Consumer** – Consumer application cannot process messages fast enough.
- **Large Data Volume** – High message production rate increases the backlog.
- **Network Issues** – Slow communication between consumer and Kafka broker.
- **Long Processing Time** – Complex business logic delays message processing.

### How to Reduce Consumer Lag

- Increase the number of consumers in the consumer group.
- Add more partitions to allow parallel processing.
- Optimize consumer processing logic.
- Improve network and broker performance.

### Key Points

- Consumer lag indicates processing delay.
- High lag can affect real-time applications.
- Monitoring consumer lag helps identify performance issues.


---

## 27. What is Kafka Streams?

**Kafka Streams** is a Java library provided by Apache Kafka for building real-time stream processing applications. It allows developers to process and transform data streams directly from Kafka topics.

### Stream Processing Flow

```text
Read
  |
  V
Filter
  |
  V
Transform
  |
  V
Write
```

### Example

```text
Input Topic
     |
     V
Kafka Streams Application
     |
     +--> Filter unwanted events
     |
     +--> Transform data format
     |
     V
Output Topic
```

### Key Points

- Kafka Streams processes data in real time.
- It reads messages from Kafka topics and writes results back to Kafka topics.
- No separate processing cluster is required.
- Supports:
  - Filtering
  - Mapping
  - Aggregations
  - Joins
  - Windowing
- Provides fault tolerance using Kafka's storage and replication features.

---

## 28. What is a Dead Letter Topic (DLT)?

A **Dead Letter Topic (DLT)** is a Kafka topic used to store messages that cannot be successfully processed due to errors or exceptions.

Instead of losing failed messages, Kafka applications can redirect them to a DLT for analysis, debugging, and later reprocessing.

### Example

```text
orders Topic
      |
      V
Message Processing
      |
      V
Exception Occurs
      |
      V
orders.DLT
```

### Why Use a Dead Letter Topic?

- Stores failed messages safely.
- Helps debug processing failures.
- Allows retry mechanisms.
- Prevents failed messages from blocking normal message processing.

### Key Points

- Failed messages are moved to a separate topic.
- Original message data and error details can be stored for investigation.
- Developers can fix the issue and replay messages from the DLT.
- Commonly used in event-driven architectures for reliable processing.

---

## 29. What is a Key in Kafka?

A **Key** in Kafka is a value associated with a message that determines which partition the message will be sent to.

Messages with the **same key** are always sent to the **same partition**, which helps maintain message ordering.

### Example

```java
producer.send(
    new ProducerRecord<>("orders",
                         "customer1",
                         "Order"));
```

### Partitioning Behavior

```text
Key: customer1

Message 1 --> Partition 0
Message 2 --> Partition 0
Message 3 --> Partition 0


Key: customer2

Message 4 --> Partition 1
Message 5 --> Partition 1
```

### Key Points

- Kafka uses the message key to select a partition.
- Messages with the same key always go to the same partition.
- Ordering is guaranteed only within a partition.
- Keys are useful when events belonging to the same entity must be processed in order.

### Common Use Cases

- Customer events → key by `customerId`
- Order events → key by `orderId`
- Account transactions → key by `accountId`

---

## 30. How does Kafka maintain ordering?

Kafka guarantees **message ordering only within a single partition**. Messages written to the same partition are stored and read in the order they were produced.

To maintain ordering for related messages, use the **same message key** so that all related messages are sent to the same partition.

### Example

```text
Topic: orders

Key: customer1

Message 1 --> Partition 0
Message 2 --> Partition 0
Message 3 --> Partition 0

Order maintained:
Message 1 → Message 2 → Message 3
```

### Key Points

- Kafka guarantees ordering **only within a partition**.
- Kafka does not guarantee ordering across multiple partitions.
- Messages with the same key are routed to the same partition.
- Using keys helps maintain ordering for related events.

### Example Use Case

```text
Customer Events

customer1:
  Order Created
        |
        V
  Payment Completed
        |
        V
  Order Shipped
```

All events for `customer1` should use the same key to ensure they are processed in the correct order.

---

## 31. What is the maximum message size in Kafka?

Kafka has a default maximum message size limit of **1 MB**. This limit controls the maximum size of a single message that can be sent to a Kafka topic.

The limit can be increased by changing broker and client configuration settings.

### Configuration

```properties
message.max.bytes=1048576
```

(Default: 1 MB)

### Related Configurations

**Broker Configuration:**

```properties
message.max.bytes
```

Defines the maximum message size allowed by the broker.

**Producer Configuration:**

```properties
max.request.size
```

Defines the maximum request size a producer can send.

**Consumer Configuration:**

```properties
fetch.max.bytes
```

Defines the maximum amount of data a consumer can fetch.

### Key Points

- Default Kafka message size is **1 MB**.
- Large messages require increasing both broker and client limits.
- Increasing message size can impact:
  - Network bandwidth
  - Memory usage
  - Performance
- For very large payloads, it is often better to store the data externally (for example, in object storage) and send a reference through Kafka.

---

## 32. How do you improve Kafka performance?

Kafka performance can be improved by optimizing producers, consumers, brokers, and topic configurations.

### Performance Optimization Techniques

### 1. Increase Partitions

- More partitions allow more consumers to process messages in parallel.
- Improves scalability and throughput.

```text
Topic

Partition 0 --> Consumer 1
Partition 1 --> Consumer 2
Partition 2 --> Consumer 3
```

### 2. Enable Compression

Compression reduces network traffic and storage usage.

Supported compression types:

- `snappy`
- `lz4`
- `zstd`

Example:

```properties
compression.type=zstd
```

### 3. Batch Records

Batching improves producer throughput by sending multiple messages together.

Configurations:

```properties
batch.size
linger.ms
```

Example:

```properties
batch.size=32768
linger.ms=10
```

### 4. Tune Consumer Fetch Settings

Increase fetch size to improve consumer throughput.

Configuration:

```properties
fetch.min.bytes
```

### 5. Use Asynchronous Sends

Asynchronous sending allows producers to send messages without waiting for each acknowledgment.

Example:

```java
producer.send(record);
```

Benefits:

- Higher throughput
- Better resource utilization

### 6. Optimize Replication and Acknowledgments

Balance reliability and performance using:

```properties
acks=0
acks=1
acks=all
```

- Lower acknowledgments → Higher performance
- Higher acknowledgments → Better data safety

### Key Points

- More partitions improve parallel processing.
- Compression reduces network and storage overhead.
- Batching improves producer throughput.
- Async sends reduce producer waiting time.
- Performance tuning should balance **throughput, latency, and reliability**.

---

## 33. What is the difference between assign() and subscribe()?

In Kafka consumers, `subscribe()` and `assign()` are two ways to specify which partitions a consumer should read from.

| subscribe() | assign() |
|-------------|----------|
| Uses consumer groups. | Manually assigns partitions to a consumer. |
| Kafka automatically assigns partitions. | Application controls partition assignment. |
| Supports automatic rebalancing. | No automatic rebalancing. |
| Best for most production applications. | Useful for custom partition control and testing. |

### subscribe() Example

```java
consumer.subscribe(List.of("orders"));
```

### subscribe() Flow

```text
Consumer Group

Consumer 1
      |
      V
Kafka Coordinator
      |
      V
Automatic Partition Assignment
```

### assign() Example

```java
consumer.assign(
    List.of(new TopicPartition("orders", 0))
);
```

### assign() Flow

```text
Consumer

      |
      V

Manually Assigned Partition

orders - Partition 0
```

### Key Points

- `subscribe()` is recommended when using **consumer groups**.
- `subscribe()` allows Kafka to handle partition distribution and rebalancing.
- `assign()` gives complete control over partition selection.
- `assign()` does not participate in consumer group management.
- Use `assign()` when you need to read from specific partitions only.

---

## 34. How do you commit offsets?

**Offset commit** is the process of saving the position of messages that a Kafka consumer has successfully processed. Kafka uses committed offsets to know where a consumer should resume reading after a restart.

### 1. Automatic Commit

Kafka automatically commits offsets at regular intervals.

Configuration:

```properties
enable.auto.commit=true
```

### Advantages

- Simple to configure.
- Less consumer code.

### Disadvantages

- Less control over message processing.
- May cause message loss or duplicate processing depending on timing.

---

### 2. Manual Synchronous Commit

The consumer manually commits offsets and waits for the commit operation to complete.

Example:

```java
consumer.commitSync();
```

### Advantages

- Provides better control.
- Ensures offsets are committed successfully before continuing.

### Disadvantages

- Slower because the consumer waits for the broker response.

---

### 3. Manual Asynchronous Commit

The consumer commits offsets without waiting for the broker response.

Example:

```java
consumer.commitAsync();
```

### Advantages

- Higher performance.
- Does not block consumer processing.

### Disadvantages

- Commit failures need additional handling.

---

### Offset Commit Flow

```text
Consumer
   |
   | Process Message
   |
   V
Commit Offset
   |
   V
Kafka __consumer_offsets Topic
```

### Key Points

- Automatic commits are simple but provide less control.
- Manual commits help achieve better processing guarantees.
- `commitSync()` provides reliability.
- `commitAsync()` provides better performance.
- Manual offset management is commonly used in critical applications.

---

# 35. Kafka Interview Scenario Questions

## Scenario 1: Duplicate messages are being processed. What could be the reason?

### Answer:

Duplicate message processing can happen due to:

- **At-least-once delivery semantics** – Kafka retries message delivery to avoid data loss.
- **Consumer retries after failure** – A message may be processed again if the consumer fails before committing the offset.
- **Offset committed after processing failure** – If processing fails after the message is handled but before the offset commit, the message may be processed again.
- **Producer retries without idempotence** – Producer retries can create duplicate messages.

### Solution:

- Enable idempotent producer:

```properties
enable.idempotence=true
```

- Use Kafka transactions where required.
- Make consumer processing idempotent.
- Commit offsets only after successful message processing.

---

## Scenario 2: Consumers are very slow. How would you improve performance?

### Answer:

Consumer lag can be reduced by:

- Increasing topic partitions.
- Adding more consumers (up to the number of available partitions).
- Optimizing consumer processing logic.
- Increasing fetch size.
- Scaling consumer instances horizontally.

### Example:

```text
Before:

Partition 0 --> Consumer 1


After Scaling:

Partition 0 --> Consumer 1
Partition 1 --> Consumer 2
Partition 2 --> Consumer 3
```

### Key Points:

- More partitions allow more parallel processing.
- Consumers in the same group share partitions.
- Performance improvements should focus on reducing processing bottlenecks.

---

## Scenario 3: A broker crashes. What happens?

### Answer:

When a Kafka broker crashes, Kafka handles the failure through replication and leader election.

### Example:

```text
Before Failure:

Broker 1 --> Leader
Broker 2 --> ISR Replica
Broker 3 --> ISR Replica


After Broker 1 Crash:

Broker 2 --> New Leader
Broker 3 --> Replica
```

### Key Points:

- An in-sync replica becomes the new leader.
- Producers and consumers continue after leader election.
- No data is lost if:
  - Replication is configured correctly.
  - Acknowledgment settings are appropriate.
  - In-sync replicas are available.

---

# Spring Boot Kafka Questions

## 1. How do you send a message?

In Spring Boot, messages can be sent to Kafka topics using **KafkaTemplate**.

### Example:

```java
@Autowired
private KafkaTemplate<String, String> kafkaTemplate;

kafkaTemplate.send("orders", "Hello Kafka");
```

### Key Points:

- `KafkaTemplate` is used by producers to publish messages.
- The message is sent to the specified Kafka topic.
- Spring Kafka handles producer configuration and communication with Kafka brokers.

---

## 2. How do you receive a message?

Messages can be consumed using the `@KafkaListener` annotation.

### Example:

```java
@KafkaListener(topics = "orders")
public void consume(String message) {
    System.out.println(message);
}
```

### Key Points:

- `@KafkaListener` creates a Kafka consumer.
- Spring automatically polls messages from the configured topic.
- The method is invoked whenever a new message is received.

---

## 3. What is KafkaTemplate?

**KafkaTemplate** is the primary Spring Kafka abstraction used for publishing messages to Kafka topics.

### Responsibilities:

- Sends messages to Kafka topics.
- Handles producer communication.
- Supports synchronous and asynchronous message sending.

### Example:

```java
kafkaTemplate.send("orders", "Order Created");
```

---

## 4. What is @KafkaListener?

`@KafkaListener` is a Spring Kafka annotation that registers a method as a Kafka consumer.

It allows Spring to automatically receive and process messages from specified Kafka topics.

### Example:

```java
@KafkaListener(topics = "orders")
public void consume(String message) {
    System.out.println(message);
}
```

### Key Points:

- Creates a Kafka consumer automatically.
- Supports consuming from one or multiple topics.
- Works with consumer groups.
- Handles message polling and listener invocation.
- Simplifies Kafka consumer development in Spring Boot applications.

---

# Frequently Asked Kafka Interview Questions

## 1. Explain Kafka Architecture.

**Answer:**

Kafka architecture consists of the following components:

- **Producer** – Publishes messages to Kafka topics.
- **Consumer** – Reads messages from Kafka topics.
- **Broker** – Kafka server that stores and manages messages.
- **Topic** – Logical channel where messages are stored.
- **Partition** – Subdivision of a topic that enables parallel processing.
- **Offset** – Unique position number of each message inside a partition.
- **Consumer Group** – Group of consumers working together to process messages.
- **ZooKeeper** – Used in older Kafka versions for cluster management.
- **KRaft** – New Kafka metadata management mode replacing ZooKeeper.

---

## 2. Difference between Topic and Partition.

| Topic | Partition |
|------|-----------|
| Logical channel for storing messages. | Physical division of a topic. |
| Contains one or more partitions. | Stores messages in an ordered sequence. |
| Used for message categorization. | Provides scalability and parallel processing. |

---

## 3. What is an Offset?

**Answer:**

An offset is a unique sequential number assigned to each message inside a Kafka partition.

Example:

```text
Partition 0

Offset 0 --> Message 1
Offset 1 --> Message 2
Offset 2 --> Message 3
```

Consumers use offsets to track processed messages.

---

## 4. What is a Consumer Group?

**Answer:**

A consumer group is a collection of consumers that work together to consume messages from Kafka topics.

Key Points:

- Each partition is consumed by only one consumer within the same group.
- Multiple consumer groups can consume the same topic independently.
- Provides scalability and load balancing.

---

## 5. What happens during consumer rebalance?

**Answer:**

Consumer rebalance occurs when Kafka redistributes partitions among consumers in a consumer group.

Triggers:

- Consumer joins.
- Consumer leaves.
- Consumer failure.
- New partitions are added.

Kafka assigns partitions again and consumers continue processing from committed offsets.

---

## 6. Explain ISR.

**Answer:**

ISR (**In-Sync Replicas**) are replicas that are fully synchronized with the partition leader.

Key Points:

- ISR replicas contain the latest data.
- Only ISR replicas can become a new leader.
- Provides data consistency and fault tolerance.

---

## 7. Leader vs Follower.

| Leader | Follower |
|--------|----------|
| Handles all read and write requests. | Copies data from the leader. |
| Receives messages from producers. | Maintains replica data. |
| Can become unavailable during failure. | Can be promoted as new leader. |

---

## 8. At-most-once vs At-least-once vs Exactly-once Delivery.

| Delivery Type | Description |
|--------------|-------------|
| At-most-once | No duplicates, but messages may be lost. |
| At-least-once | Messages are never lost, but duplicates are possible. |
| Exactly-once | No duplicates and no message loss. |

---

## 9. How does Kafka guarantee ordering?

**Answer:**

Kafka guarantees ordering only within a partition.

Key Points:

- Messages in the same partition maintain their order.
- Using the same message key sends related messages to the same partition.
- Kafka does not guarantee ordering across multiple partitions.

---

## 10. How does Kafka achieve fault tolerance?

**Answer:**

Kafka achieves fault tolerance using replication.

Key Points:

- Each partition can have multiple replicas.
- One replica acts as the leader.
- ISR replicas can become the new leader if failure occurs.
- Replication prevents data loss.

---

## 11. How do you handle duplicate messages?

**Answer:**

Duplicate messages can be handled by:

- Enable idempotent producer:

```properties
enable.idempotence=true
```

- Use Kafka transactions.
- Make consumer processing idempotent.
- Commit offsets after successful processing.

---

## 12. How do you improve Kafka performance?

**Answer:**

Performance can be improved by:

- Increasing partitions.
- Adding more consumers.
- Enabling compression (`snappy`, `lz4`, `zstd`).
- Using producer batching.
- Tuning `batch.size` and `linger.ms`.
- Using asynchronous sends.
- Optimizing acknowledgment settings.

---

## 13. What is Consumer Lag?

**Answer:**

Consumer lag is the difference between the latest Kafka offset and the consumer's processed offset.

Example:

```text
Latest Offset: 100
Consumer Offset: 80

Lag = 20 messages
```

Causes:

- Slow consumer.
- High message volume.
- Network issues.
- Long processing time.

---

## 14. How do you commit offsets?

**Answer:**

Offsets can be committed in three ways:

### Automatic Commit

```properties
enable.auto.commit=true
```

### Manual Synchronous Commit

```java
consumer.commitSync();
```

### Manual Asynchronous Commit

```java
consumer.commitAsync();
```

Manual commits provide better control over processing guarantees.

---

## 15. How do you integrate Kafka with Spring Boot?

**Answer:**

Spring Boot integrates with Kafka using Spring Kafka.

### Sending Messages

```java
@Autowired
private KafkaTemplate<String, String> kafkaTemplate;

kafkaTemplate.send("orders", "Hello Kafka");
```

### Receiving Messages

```java
@KafkaListener(topics = "orders")
public void consume(String message) {
    System.out.println(message);
}
```

### Key Components:

- **KafkaTemplate** – Used to publish messages.
- **@KafkaListener** – Used to consume messages.
- Spring Kafka manages producer and consumer configuration.

---
