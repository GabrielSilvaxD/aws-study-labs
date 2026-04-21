# SNS & SQS – Messaging Services

---

## SNS – Simple Notification Service (Pub/Sub)

### Key Concepts

- **Topic**: Channel where publishers send messages; subscribers receive all messages published to the topic
- **Publisher**: Sends a message to a topic (AWS services, SDK, console)
- **Subscriber**: Receives messages from a topic; supported protocols:
  - SQS, Lambda, HTTP/HTTPS, Email, Email-JSON, SMS, Mobile Push, Kinesis Firehose
- **Message Filtering**: JSON policy on subscription to receive only relevant messages
- **Fan-Out Pattern**: One SNS message triggers multiple SQS queues or Lambda functions in parallel

### Topic Types

| Type | Description |
|------|-------------|
| Standard | At-least-once delivery, best-effort ordering |
| FIFO | Exactly-once delivery, strict ordering; only SQS FIFO as subscriber |

### Message Attributes

- Metadata attached to a message (e.g., `MessageType: order_placed`)
- Used by subscription filter policies to selectively route messages

---

## SQS – Simple Queue Service (Message Queue)

### Key Concepts

- **Queue**: Buffer that decouples producers and consumers
- **Message**: Payload (up to 256 KB); use S3 + Extended Client Library for larger messages
- **Visibility Timeout**: Period after a consumer receives a message during which it is hidden from others (default: 30 sec, max: 12 hours)
- **Retention Period**: How long messages stay in the queue (1 minute – 14 days; default: 4 days)
- **Delivery Delay**: Delay message delivery by 0–15 minutes
- **Long Polling**: Consumer waits up to 20 seconds for messages; reduces empty responses and costs

### Queue Types

| Feature | Standard | FIFO |
|---------|----------|------|
| Ordering | Best-effort | Strict first-in first-out |
| Delivery | At-least-once | Exactly-once |
| Throughput | Unlimited | 300 msg/sec (3,000 with batching) |
| Deduplication | ❌ | ✅ (5-minute deduplication window) |

### Dead Letter Queue (DLQ)

- Stores messages that failed processing after `maxReceiveCount` attempts
- Same type as source queue (Standard DLQ for Standard, FIFO DLQ for FIFO)
- Useful for debugging and reprocessing failed messages

### SQS & Lambda (Event Source Mapping)

- Lambda polls SQS and processes messages in batches
- Batch size: 1–10,000 (standard), 1–10 (FIFO)
- On failure, Lambda can send records to an SQS DLQ or a Lambda destination

---

## Common Patterns

### Fan-Out (SNS → multiple SQS)
```
Producer → SNS Topic → SQS Queue A → Consumer A
                     → SQS Queue B → Consumer B
                     → Lambda C
```

### Request-Response (async decoupling)
```
Frontend → SQS (orders queue) → Order Service Lambda → DynamoDB
                              → DLQ (failed orders)
```

---

## Common Interview Questions

1. What is the difference between SNS and SQS?
2. When would you use a FIFO queue vs a Standard queue?
3. What is a Dead Letter Queue and why would you use one?
4. What is the visibility timeout in SQS and why is it important?
5. How does the Fan-Out pattern work using SNS and SQS?
6. How does message filtering work in SNS?
7. What is long polling in SQS and how does it reduce costs?
8. How does Lambda process messages from an SQS queue?

## Labs / Exercises

- [ ] Create an SNS topic; subscribe an email endpoint and an SQS queue; publish a message
- [ ] Implement the Fan-Out pattern: one SNS topic → two SQS queues → two Lambda consumers
- [ ] Set up a Dead Letter Queue for an SQS queue; trigger failures and verify DLQ delivery
- [ ] Create a FIFO queue; produce and consume messages; verify ordering
- [ ] Configure message filtering on an SNS subscription using a filter policy
- [ ] Process SQS messages with Lambda in batches; test DLQ behavior on Lambda errors
- [ ] Measure the effect of long polling vs short polling on empty-queue API calls
