# RDS – Relational Database Service

## Key Concepts

- **Managed Service**: AWS handles OS patching, backups, hardware provisioning, and software updates
- **Supported Engines**: MySQL, PostgreSQL, MariaDB, Oracle, SQL Server, and Amazon Aurora
- **DB Instance**: The compute and storage resource running the database
- **DB Instance Class**: db.t3 (burstable), db.m5 (general), db.r5 (memory optimized)
- **Storage Types**:
  | Type | Use Case |
  |------|----------|
  | gp2/gp3 (SSD) | General purpose; gp3 allows independent IOPS/throughput tuning |
  | io1/io2 (SSD) | High-performance OLTP workloads; provisioned IOPS |
  | Magnetic | Legacy; not recommended |

## High Availability

- **Multi-AZ**: Synchronous replication to a standby in another AZ; automatic failover (~1–2 min); standby not readable
- **Read Replicas**: Asynchronous replication; up to 5 replicas; can be cross-region; readable; can be promoted to standalone
- **Difference**: Multi-AZ = HA/disaster recovery; Read Replicas = read scaling

## Backups

- **Automated Backups**: Daily snapshots + transaction logs; retained 1–35 days; point-in-time recovery (PITR)
- **Manual Snapshots**: User-initiated; retained indefinitely until deleted
- **Restore**: Always creates a new DB instance; cannot restore in-place

## Security

- **Encryption at Rest**: Uses AWS KMS; must be enabled at creation; encrypted snapshots and replicas
- **Encryption in Transit**: SSL/TLS; enforce via parameter group (`require_ssl`)
- **VPC Deployment**: RDS runs inside a VPC; use private subnets and Security Groups
- **IAM Authentication**: Use IAM tokens instead of passwords (for MySQL/PostgreSQL)

## Aurora

- AWS-proprietary MySQL/PostgreSQL-compatible database; up to 5x faster than MySQL
- **Storage**: Auto-scales from 10 GB to 128 TB; 6 copies across 3 AZs
- **Aurora Replicas**: Up to 15; failover < 30 seconds
- **Aurora Serverless**: Auto-scales compute capacity on demand; good for intermittent workloads
- **Global Database**: Spans multiple regions; < 1 second replication lag; disaster recovery

## RDS Proxy

- Fully managed database proxy; pools and shares connections; improves Lambda scalability
- Reduces database connection overhead; supports IAM authentication; failover handling

## Parameter Groups & Option Groups

- **Parameter Group**: Database engine configuration (e.g., `max_connections`, `slow_query_log`)
- **Option Group**: Enable additional features (e.g., Oracle STATSPACK, MSSQL Transparent Data Encryption)

## Common Interview Questions

1. What is the difference between Multi-AZ and Read Replicas?
2. Can you read from a Multi-AZ standby instance? Why or why not?
3. How does RDS automated backup work? What is PITR?
4. What is Aurora and how does it differ from standard RDS MySQL?
5. How would you scale an RDS database for read-heavy workloads?
6. How does RDS Proxy improve Lambda → RDS connectivity?
7. How do you encrypt an existing unencrypted RDS instance?
8. What happens during an RDS Multi-AZ failover?

## Labs / Exercises

- [ ] Launch an RDS MySQL instance in a private subnet with a custom Security Group
- [ ] Enable Multi-AZ; trigger a manual failover and observe the endpoint behavior
- [ ] Create a Read Replica in the same region; connect and run read queries
- [ ] Test PITR by restoring a database to a specific point in time
- [ ] Enable Performance Insights and identify slow queries
- [ ] Set up an Aurora MySQL cluster with one writer and two Aurora Replicas
- [ ] Configure RDS Proxy and connect a Lambda function through it
- [ ] Migrate an unencrypted RDS instance to an encrypted one using a snapshot
