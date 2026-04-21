# AWS Study Labs 🚀

A collection of hands-on AWS study labs organized by service. This repository serves as a personal reference for AWS interview preparation and practical experience.

---

## 📚 Table of Contents

| Lab | Description |
|-----|-------------|
| [EC2](./ec2/) | Elastic Compute Cloud – virtual servers, AMIs, instance types, security groups, key pairs, auto scaling |
| [S3](./s3/) | Simple Storage Service – buckets, versioning, lifecycle policies, static website hosting, encryption |
| [Lambda](./lambda/) | Serverless compute – function creation, triggers, layers, environment variables, VPC integration |
| [IAM](./iam/) | Identity & Access Management – users, groups, roles, policies, MFA, cross-account access |
| [VPC](./vpc/) | Virtual Private Cloud – subnets, route tables, IGW, NAT gateway, VPC peering, security groups, NACLs |
| [RDS](./rds/) | Relational Database Service – instance creation, Multi-AZ, read replicas, backups, parameter groups |
| [DynamoDB](./dynamodb/) | NoSQL database – tables, partition/sort keys, GSI, LSI, streams, DAX, capacity modes |
| [CloudFormation](./cloudformation/) | Infrastructure as Code – templates, stacks, change sets, nested stacks, drift detection |
| [ECS](./ecs/) | Elastic Container Service – task definitions, services, Fargate vs EC2 launch type, ECR |
| [CloudWatch](./cloudwatch/) | Monitoring & observability – metrics, alarms, dashboards, Logs, Logs Insights, Events/EventBridge |
| [SNS & SQS](./sns-sqs/) | Messaging – SNS topics, SQS queues, standard vs FIFO, dead-letter queues, fan-out pattern |
| [API Gateway](./api-gateway/) | REST & HTTP APIs – resources, methods, stages, authorizers, Lambda proxy integration |
| [Route 53](./route53/) | DNS – hosted zones, record types, routing policies, health checks, domain registration |
| [ElastiCache](./elasticache/) | In-memory caching – Redis vs Memcached, clusters, replication groups, use cases |

---

## 🗂️ Repository Structure

```
aws-study-labs/
├── ec2/
│   └── README.md
├── s3/
│   └── README.md
├── lambda/
│   └── README.md
├── iam/
│   └── README.md
├── vpc/
│   └── README.md
├── rds/
│   └── README.md
├── dynamodb/
│   └── README.md
├── cloudformation/
│   └── README.md
├── ecs/
│   └── README.md
├── cloudwatch/
│   └── README.md
├── sns-sqs/
│   └── README.md
├── api-gateway/
│   └── README.md
├── route53/
│   └── README.md
└── elasticache/
    └── README.md
```

---

## 🎯 How to Use This Repo

1. Each folder contains a `README.md` with key concepts, common interview questions, and lab exercises.
2. Labs build progressively – start with IAM and VPC before moving to application services.
3. Use the notes as a quick-review sheet before interviews.

---

## 📌 Recommended Study Order

1. **IAM** – Security foundation for everything else
2. **VPC** – Networking foundation
3. **EC2** – Core compute
4. **S3** – Core storage
5. **RDS / DynamoDB** – Databases
6. **Lambda / API Gateway** – Serverless
7. **ECS** – Containers
8. **CloudFormation** – IaC
9. **CloudWatch / SNS / SQS** – Monitoring & Messaging
10. **Route 53 / ElastiCache** – DNS & Caching

---

## 🔗 Useful Resources

- [AWS Documentation](https://docs.aws.amazon.com/)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [AWS Cheat Sheets – TutorialsDojo](https://tutorialsdojo.com/aws-cheat-sheets/)
- [AWS FAQs](https://aws.amazon.com/faqs/)

