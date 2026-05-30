# Synchronous vs Asynchronous Systems

## Overview

A synchronous system provides an immediate response to the caller. The caller waits until the operation is fully completed.

An asynchronous system acknowledges the request quickly and processes the work later. The caller does not wait for the complete operation.

---

## Synchronous System

### Example

```text
Login UI
   ↓
Backend API
   ↓
Database Check
   ↓
JWT Generation
   ↓
Response
```

The user waits until the entire workflow is completed.

### Advantages

- Easy to debug
- Simple architecture
- Immediate response
- Strong consistency

### Disadvantages

- Cannot handle heavy or long-running tasks efficiently
- Tight coupling between services
- Cascading failures possible

Example:

```text
API A → API B → Database
```

If the database becomes slow:

```text
Database Slow
      ↓
API B Slow
      ↓
API A Slow
```

---

## Asynchronous System

An asynchronous system acknowledges the request and processes it later.

### Example

```text
API
 ↓
Queue
 ↓
Response Returned

Worker
 ↓
Process Message
```

### Advantages

- Highly scalable
- Better fault isolation
- Supports retry mechanisms
- Handles traffic spikes efficiently

### Disadvantages

- More complex architecture
- Eventual consistency
- Harder debugging

---

## Queue-Based Architecture

```mermaid
graph LR
A[Producer] --> B[Queue]
B --> C[Consumer]
```

Benefits:

- Decouples producer and consumer
- Buffers sudden traffic spikes
- Supports retries
- Enables independent scaling

---

## When to Use Synchronous Systems

Use when immediate results are required:

- Login
- Payment authorization
- Search APIs
- Profile retrieval

---

## When to Use Asynchronous Systems

Use for long-running workloads:

- Email sending
- AI inference
- Document processing
- Notification systems
- Data pipelines

---

## Polling vs Event-Driven Architecture

### Polling

Consumer continuously checks for messages.

Example:

```text
Consumer → SQS Queue → Check Again → Check Again
```

#### Advantages

- Simple implementation

#### Disadvantages

- Resource wastage
- Higher latency

---

### Event-Driven

Processing is triggered automatically when an event occurs.

Example:

```text
S3 Upload
     ↓
Lambda Trigger
     ↓
Processing Starts
```

#### Advantages

- No continuous polling
- Faster response to events

#### Disadvantages

- Higher distributed-system complexity

---

## AWS SQS Concepts

### Visibility Timeout

The duration during which a message remains invisible after being consumed.

If the consumer fails before deleting the message:

- Message becomes visible again
- Another consumer can process it

### Dead Letter Queue (DLQ)

Messages that repeatedly fail processing are moved to a DLQ.

Benefits:

- Prevents infinite retry loops
- Helps debugging failed messages
- Isolates poison messages

---

## Idempotency

### Definition

Performing the same operation multiple times produces the same final result.

### Example

Idempotent:

```text
Set Account Status = ACTIVE
```

Non-Idempotent:

```text
Add ₹1000 to Account Balance
```

If retried multiple times, the balance changes incorrectly.

---

## Interview Questions

1. Why use asynchronous processing for AI workloads?
2. Why introduce a queue between services?
3. What is the purpose of visibility timeout?
4. What is a DLQ?
5. What is idempotency?
6. Polling vs Event-Driven systems?