# Route 53 – DNS and Domain Registration

## Key Concepts

- **Hosted Zone**: Container for DNS records for a domain; can be public or private
  - **Public Hosted Zone**: Resolves internet-facing DNS queries
  - **Private Hosted Zone**: Resolves DNS within one or more VPCs; not publicly accessible
- **Record Types**: A, AAAA, CNAME, MX, TXT, NS, SOA, PTR, CAA, SRV, and Alias records
- **Alias Record**: AWS-specific extension to DNS; points to AWS resources (ALB, CloudFront, S3 website, Elastic Beanstalk) for free; resolves at the DNS level (no extra lookup)
- **TTL (Time-to-Live)**: How long DNS resolvers cache the record; lower = faster propagation, higher = fewer queries and cost

## Routing Policies

| Policy | Description | Use Case |
|--------|-------------|----------|
| Simple | Single resource; no health checks per se | Single endpoint |
| Weighted | Distribute traffic across multiple resources by weight (0–255) | A/B testing, gradual migration |
| Latency | Route to the region with lowest network latency for the user | Multi-region apps |
| Failover | Active-passive; route to secondary if primary health check fails | Disaster recovery |
| Geolocation | Route based on user's geographic location (continent, country, state) | Compliance, localization |
| Geoproximity | Route based on location of users and resources; bias adjusts routing | Traffic shifting by location |
| Multi-value Answer | Return up to 8 healthy records at random | Simple load balancing with health checks |
| IP-based | Route based on the client's IP address (CIDR ranges) | ISP-specific routing |

## Health Checks

- Monitor endpoints (HTTP, HTTPS, TCP) or other Route 53 health checks (calculated health checks)
- Used by Failover and other routing policies to determine record availability
- Can trigger CloudWatch Alarms on health check failure
- **Calculated Health Check**: Combines multiple health checks using AND/OR/NOT logic

## ALIAS vs CNAME

| Feature | ALIAS | CNAME |
|---------|-------|-------|
| Can point to AWS resources | ✅ Yes (native integration) | ⚠️ Only if DNS name |
| Works at zone apex (root domain) | ✅ Yes | ❌ No |
| DNS query cost | Free (no additional charge) | Standard query charges |
| Resolves at DNS level | ✅ Yes | Requires extra lookup |

## Domain Registration

- Route 53 can register domains directly; automatic hosted zone creation
- Transfer existing domains to Route 53 for unified management

## Route 53 Resolver

- **Inbound Endpoints**: Allow DNS queries from on-premises to resolve Route 53 private hosted zones
- **Outbound Endpoints**: Allow EC2 instances to resolve on-premises DNS names
- **Resolver Rules**: Forward DNS queries for specific domains to specified DNS servers

## Common Interview Questions

1. What is the difference between an Alias record and a CNAME record?
2. Why can't you use a CNAME at the zone apex?
3. How does the Failover routing policy work?
4. What is the difference between Geolocation and Latency routing policies?
5. How do you configure private DNS resolution for VPC-connected on-premises networks?
6. How do health checks integrate with routing policies?
7. What is a Calculated Health Check?
8. How do you gradually migrate traffic from an old endpoint to a new one using Route 53?

## Labs / Exercises

- [ ] Register a domain (or transfer one) and explore the automatically created hosted zone
- [ ] Create A and CNAME records; test DNS resolution with `nslookup`/`dig`
- [ ] Create an Alias record pointing to an ALB; verify it resolves correctly
- [ ] Set up Weighted Routing between two EC2 instances (80/20 split); verify distribution
- [ ] Configure Failover Routing with health checks; simulate primary failure and test failover
- [ ] Create a Private Hosted Zone for an internal domain; verify resolution from within the VPC
- [ ] Set up Latency-based routing across two regions and test from different locations
- [ ] Configure Route 53 Resolver inbound/outbound endpoints for hybrid DNS resolution
