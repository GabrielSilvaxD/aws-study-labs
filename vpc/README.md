# VPC – Virtual Private Cloud

## Key Concepts

- **VPC**: Logically isolated virtual network within an AWS region; you define the IP range (CIDR block)
- **Subnet**: Sub-division of a VPC within a single AZ; can be public or private
  - **Public Subnet**: Has a route to an Internet Gateway; instances can have public IPs
  - **Private Subnet**: No direct route to the internet
- **CIDR Block**: IP address range (e.g., `10.0.0.0/16`); VPC range `/16`–`/28`
- **Internet Gateway (IGW)**: Enables communication between VPC and the internet; attached to VPC
- **NAT Gateway**: Allows private subnet instances to initiate outbound internet traffic (no inbound); managed by AWS; resides in public subnet
- **NAT Instance**: Self-managed EC2 instance serving as NAT; less preferred (legacy)
- **Route Table**: Set of rules that determines where network traffic is directed; each subnet must be associated with one
- **Elastic Network Interface (ENI)**: Virtual network card; can be attached/detached from instances

## Security

| Feature | Type | Scope | Stateful? |
|---------|------|-------|-----------|
| Security Group | Allow rules only | Instance level | ✅ Yes |
| NACL | Allow + Deny rules | Subnet level | ❌ No (explicit allow for return traffic) |

- **Security Groups**: Evaluate all rules; default SG allows all outbound, denies all inbound
- **NACLs**: Process rules in order (lowest number first); default NACL allows all traffic

## VPC Connectivity

| Feature | Description |
|---------|-------------|
| VPC Peering | Connect two VPCs (same or different account/region); not transitive |
| Transit Gateway | Hub-and-spoke model; connect multiple VPCs and on-prem; transitive routing |
| VPN (Site-to-Site) | IPsec tunnel between on-prem and AWS VPC |
| Direct Connect | Dedicated private network connection from on-prem to AWS |
| VPC Endpoints | Private connectivity to AWS services without IGW or NAT |

## VPC Endpoints

| Type | Description |
|------|-------------|
| Gateway Endpoint | S3 and DynamoDB only; free; route table entry |
| Interface Endpoint (PrivateLink) | ENI with private IP; supports most AWS services; hourly cost |

## DNS in VPC

- **enableDnsSupport**: Allows instances to use Route 53 Resolver (enabled by default)
- **enableDnsHostnames**: Assigns public DNS hostnames to instances with public IPs (disabled by default, must enable for public subnets)

## Default VPC

- Created automatically in each region
- Has one public subnet per AZ, IGW attached, and default route table
- Instances launched get a public IP by default

## High Availability Architecture

```
Region
├── AZ-a
│   ├── Public Subnet (10.0.1.0/24)  → IGW → Internet
│   │   └── NAT Gateway
│   └── Private Subnet (10.0.2.0/24) → NAT GW → Internet
└── AZ-b
    ├── Public Subnet (10.0.3.0/24)
    └── Private Subnet (10.0.4.0/24)
```

## Common Interview Questions

1. What is the difference between a Security Group and a NACL?
2. How does a NAT Gateway work? What's the difference vs a NAT Instance?
3. What is VPC Peering and is it transitive?
4. How would you connect your on-premises network to an AWS VPC?
5. What are VPC Endpoints and when would you use them?
6. How do you design a highly available VPC architecture?
7. What happens if Security Group rules conflict with NACL rules?
8. What is Transit Gateway and how does it differ from VPC Peering?

## Labs / Exercises

- [ ] Create a custom VPC (`10.0.0.0/16`) with public and private subnets across two AZs
- [ ] Attach an IGW and update the public subnet route table
- [ ] Deploy a NAT Gateway in the public subnet; route private subnet traffic through it
- [ ] Launch a bastion host in the public subnet; SSH into a private EC2 instance via the bastion
- [ ] Set up VPC Peering between two VPCs and test connectivity
- [ ] Create a Gateway VPC Endpoint for S3; verify private EC2 can access S3 without internet
- [ ] Configure NACLs to block a specific IP from accessing a public subnet
- [ ] Set up a Site-to-Site VPN using a Virtual Private Gateway
