# EC2 – Elastic Compute Cloud

## Key Concepts

- **Instance Types**: General purpose (t3, m5), Compute optimized (c5), Memory optimized (r5), Storage optimized (i3), Accelerated (p3/g4)
- **AMI (Amazon Machine Image)**: Pre-configured template for launching instances; can be AWS-provided, community, or custom
- **Security Groups**: Virtual firewall at the instance level – stateful (return traffic automatically allowed)
- **NACLs vs Security Groups**: NACLs are stateless and operate at the subnet level; Security Groups are stateful and at the instance level
- **Key Pairs**: Used for SSH access to Linux instances / RDP for Windows
- **Elastic IP**: Static public IPv4 address that can be associated with an instance
- **User Data**: Bootstrap script that runs once at instance launch
- **Instance Metadata**: Available at `http://169.254.169.254/latest/meta-data/`
- **Placement Groups**: Cluster (low latency, high throughput), Spread (max availability), Partition (large distributed workloads)
- **Tenancy**: Shared (default), Dedicated Instance, Dedicated Host

## Purchasing Options

| Option | Use Case |
|--------|----------|
| On-Demand | Short-term, unpredictable workloads |
| Reserved (1 or 3 yr) | Steady-state workloads, up to 72% discount |
| Savings Plans | Flexible compute savings, up to 66% discount |
| Spot | Fault-tolerant, flexible workloads, up to 90% discount |
| Dedicated Host | Compliance, BYOL (Bring Your Own License) |

## Auto Scaling

- **Launch Template / Launch Configuration**: Defines instance configuration for ASG
- **Scaling Policies**: Target tracking, step scaling, simple scaling, scheduled
- **Cooldown Period**: Time after a scaling event before another can trigger
- **Health Checks**: EC2 health checks or ELB health checks

## Load Balancers

| Type | Use Case |
|------|----------|
| ALB (Application LB) | HTTP/HTTPS, path/host-based routing, Layer 7 |
| NLB (Network LB) | TCP/UDP, ultra-low latency, static IP, Layer 4 |
| CLB (Classic LB) | Legacy, Layer 4 & 7 |
| GWLB (Gateway LB) | Third-party network appliances |

## Common Interview Questions

1. What is the difference between a Security Group and a NACL?
2. What happens to an EC2 instance when it is stopped vs terminated?
3. How do you make an EC2 instance highly available?
4. What is the difference between Spot instances and Reserved instances?
5. How does Auto Scaling work with a Load Balancer?
6. What is an Elastic IP and when would you use it?
7. Explain EC2 instance metadata and how to access it.
8. What are Placement Groups and when would you use each type?

## Labs / Exercises

- [ ] Launch a t2.micro EC2 instance with a custom Security Group (allow SSH from your IP, HTTP from anywhere)
- [ ] Connect via SSH, install Apache, and serve a simple HTML page
- [ ] Create a custom AMI from the running instance
- [ ] Set up an Application Load Balancer with two EC2 instances
- [ ] Configure an Auto Scaling Group with a target tracking policy (CPU > 50%)
- [ ] Use User Data to automate web server setup on launch
- [ ] Test Spot Instance launch and interruption handling
