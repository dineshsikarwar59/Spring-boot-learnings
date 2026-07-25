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
