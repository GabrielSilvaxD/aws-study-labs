# ElastiCache – In-Memory Caching

## Key Concepts

- **ElastiCache**: Fully managed in-memory data store and cache service
- **Supported Engines**: Redis and Memcached
- **Use Cases**: Database query result caching, session storage, leaderboards, pub/sub messaging, real-time analytics

## Redis vs Memcached

| Feature | Redis | Memcached |
|---------|-------|-----------|
| Data structures | Strings, Lists, Sets, Hashes, Sorted Sets, Streams, HyperLogLog, Bitmaps | String only |
| Persistence | ✅ AOF and RDB snapshots | ❌ No persistence |
| Replication | ✅ Primary + read replicas | ❌ No replication |
| Multi-AZ / Failover | ✅ Automatic failover | ❌ No |
| Pub/Sub | ✅ Yes | ❌ No |
| Transactions | ✅ Yes (MULTI/EXEC) | ❌ No |
| Cluster mode | ✅ Cluster mode (sharding) | ✅ Multi-node (sharding only) |
| Lua scripting | ✅ Yes | ❌ No |
| Use case | Rich data structures, HA, persistence | Simple caching, multi-threaded scale |

## Redis Cluster Modes

### Cluster Mode Disabled
- 1 primary node + up to 5 read replicas
- Multi-AZ with automatic failover
- All data on a single shard
- Vertical scaling (change node type)

### Cluster Mode Enabled
- Up to 500 shards; each shard has a primary + up to 5 replicas
- Horizontal scaling (add/remove shards)
- Partitions data using hash slots (0–16,383)
- Requires Redis cluster-aware client

## Caching Strategies

| Strategy | Description | When to Use |
|----------|-------------|-------------|
| Lazy Loading (Cache-Aside) | Populate cache on cache miss; data may be stale | Read-heavy, tolerance for eventual consistency |
| Write-Through | Write to cache and database simultaneously; no stale data | Low tolerance for stale reads |
| Write-Behind (Write-Back) | Write to cache first; asynchronously persist to DB | High-write throughput needs |
| TTL-based | Expire keys after a set time to prevent stale data | All strategies as a safety net |

## Session Storage

- Store user sessions in ElastiCache Redis instead of the application server
- Enables stateless application servers and supports horizontal scaling
- Keys should have a TTL matching the session timeout

## Security

- Deploy in VPC; use Security Groups to control access
- **Encryption at Rest**: Supported for Redis (using KMS)
- **Encryption in Transit**: Redis supports TLS; Memcached does not
- **AUTH**: Redis supports password-based authentication (`AUTH` command)
- **RBAC (Redis 6+)**: Role-based access control for user-level permissions

## Monitoring (CloudWatch Metrics)

| Metric | Description |
|--------|-------------|
| `CacheHits` / `CacheMisses` | Hit ratio indicates cache effectiveness |
| `CurrConnections` | Current open connections |
| `Evictions` | Keys evicted due to memory pressure |
| `ReplicationLag` | Lag between primary and replica |
| `DatabaseMemoryUsagePercentage` | Memory pressure indicator |

## Common Interview Questions

1. What is the difference between Redis and Memcached in ElastiCache?
2. When would you use ElastiCache vs DynamoDB DAX?
3. What is Lazy Loading (Cache-Aside) and what are its drawbacks?
4. What is Write-Through caching and when is it appropriate?
5. How does Redis Cluster Mode (sharding) differ from Cluster Mode Disabled?
6. How do you ensure ElastiCache is highly available?
7. How do you use ElastiCache for session management?
8. What CloudWatch metrics do you monitor for cache performance?

## Labs / Exercises

- [ ] Create a Redis cluster (Cluster Mode Disabled) with one replica; test read scaling
- [ ] Implement Lazy Loading in a Python/Node.js application with an RDS backend
- [ ] Compare response times with and without cache using application-level timing
- [ ] Configure Multi-AZ with automatic failover; test failover by rebooting the primary
- [ ] Enable encryption at rest and in transit; update the application to connect with TLS
- [ ] Create a Memcached cluster and compare it to Redis for simple string caching
- [ ] Use Redis Pub/Sub to implement a simple real-time notification system
- [ ] Monitor `Evictions` and tune `maxmemory-policy` to handle memory pressure
