# DynamoDB – Managed NoSQL Database

## Key Concepts

- **Fully Managed**: No server management; automatic scaling, backups, and replication
- **NoSQL**: Key-value and document data model; schema-less (except primary key)
- **Table**: Collection of items; no fixed schema
- **Item**: A record (row); max 400 KB
- **Attribute**: Key-value pair within an item (column equivalent)

## Primary Keys

| Type | Description |
|------|-------------|
| Partition Key (PK) | Single attribute; must be unique; used to determine physical partition |
| Composite Key (PK + SK) | Partition Key + Sort Key; items in same partition sorted by SK |

- Choose a **high-cardinality** partition key to distribute data evenly and avoid hot partitions

## Secondary Indexes

| Index | Key | Storage | Queries |
|-------|-----|---------|---------|
| LSI (Local Secondary Index) | Same PK, different SK | Stored with base table | Strongly/eventually consistent |
| GSI (Global Secondary Index) | Different PK and/or SK | Separate partition space | Eventually consistent only |

- Max 5 LSIs and 20 GSIs per table
- LSIs must be created at table creation; GSIs can be added/deleted anytime

## Capacity Modes

| Mode | Description |
|------|-------------|
| Provisioned | Specify RCUs and WCUs; supports auto-scaling; cheaper for predictable traffic |
| On-Demand | Pay per request; no capacity planning; good for unpredictable traffic |

- **RCU (Read Capacity Unit)**: 1 strongly consistent read/sec or 2 eventually consistent reads/sec for items ≤ 4 KB
- **WCU (Write Capacity Unit)**: 1 write/sec for items ≤ 1 KB

## Consistency Models

- **Eventually Consistent Read**: Default; reads may reflect slightly stale data; half the RCU cost
- **Strongly Consistent Read**: Always returns latest data; costs 1 RCU per 4 KB

## DynamoDB Streams

- Ordered stream of item-level changes (insert, modify, delete)
- Retained for 24 hours
- Can trigger Lambda for real-time processing (change data capture)

## DAX (DynamoDB Accelerator)

- In-memory cache for DynamoDB; microsecond read latency
- Compatible with existing DynamoDB API calls (drop-in cache)
- Not suitable for strongly consistent reads or write-heavy workloads

## TTL (Time-to-Live)

- Automatically delete expired items at no extra cost
- Specify an attribute containing a Unix epoch timestamp; items deleted within 48 hours after expiry

## Global Tables

- Multi-region, multi-active replication; all replicas are writable
- Eventual consistency across regions (< 1 second typically)
- Good for globally distributed applications requiring low-latency reads/writes

## Transactions

- `TransactWriteItems` and `TransactGetItems`; all-or-nothing across up to 100 items
- ACID guarantees within a single region; 2× the RCU/WCU cost

## Common Interview Questions

1. What is the difference between a partition key and a composite key in DynamoDB?
2. What is the difference between a GSI and an LSI?
3. How does DynamoDB achieve high availability and durability?
4. When would you use On-Demand vs Provisioned capacity mode?
5. What is a hot partition and how do you prevent it?
6. How does DynamoDB Streams work and what can you use it for?
7. What is DAX and when should you use it?
8. How does DynamoDB handle transactions?

## Labs / Exercises

- [ ] Create a table with a composite key (PK: `userId`, SK: `timestamp`); insert and query items
- [ ] Add a GSI to enable querying by a different attribute; compare query performance
- [ ] Enable DynamoDB Streams and trigger a Lambda function to log all changes
- [ ] Implement a TTL attribute to auto-expire session records after 1 hour
- [ ] Use the AWS SDK to perform a `TransactWriteItems` operation
- [ ] Enable DAX and compare read latency with and without cache
- [ ] Enable Global Tables in two regions and test multi-region writes
- [ ] Use PartiQL to query DynamoDB items with SQL-like syntax
