# ECS – Elastic Container Service

## Key Concepts

- **ECS**: AWS-managed container orchestration service for running Docker containers
- **Cluster**: Logical grouping of tasks or services; can use EC2 or Fargate infrastructure
- **Task Definition**: Blueprint for your application (like a docker-compose file); defines containers, CPU, memory, networking, IAM roles, and volumes
- **Task**: A running instance of a Task Definition; single container or multi-container pod
- **Service**: Maintains a desired number of running tasks; integrates with Load Balancers; supports rolling updates and auto scaling
- **ECR (Elastic Container Registry)**: Managed Docker image registry; supports lifecycle policies and image scanning

## Launch Types

| Feature | Fargate | EC2 |
|---------|---------|-----|
| Infrastructure | AWS-managed (serverless) | Self-managed EC2 instances |
| Provisioning | No servers to manage | Register EC2 instances as container hosts |
| Pricing | Per vCPU/memory second | EC2 instance cost |
| Use Case | Simplicity, variable load | Fine-grained control, GPU, custom AMI |

## Task Definition Parameters

```json
{
  "family": "my-app",
  "networkMode": "awsvpc",
  "requiresCompatibilities": ["FARGATE"],
  "cpu": "256",
  "memory": "512",
  "executionRoleArn": "arn:aws:iam::...:role/ecsTaskExecutionRole",
  "taskRoleArn": "arn:aws:iam::...:role/ecsTaskRole",
  "containerDefinitions": [
    {
      "name": "web",
      "image": "nginx:latest",
      "portMappings": [{ "containerPort": 80 }],
      "logConfiguration": {
        "logDriver": "awslogs",
        "options": {
          "awslogs-group": "/ecs/my-app",
          "awslogs-region": "us-east-1",
          "awslogs-stream-prefix": "ecs"
        }
      }
    }
  ]
}
```

## Networking Modes

| Mode | Description |
|------|-------------|
| `awsvpc` | Each task gets its own ENI and private IP (required for Fargate) |
| `bridge` | Docker's default bridge network on EC2 hosts |
| `host` | Shares EC2 host network namespace |
| `none` | No networking |

## IAM Roles for ECS

- **Task Execution Role**: Used by ECS agent to pull images from ECR and push logs to CloudWatch
- **Task Role**: Used by the application code to call other AWS services (S3, DynamoDB, etc.)

## Service Auto Scaling

- Integrates with Application Auto Scaling
- Policies: Target tracking (e.g., CPU utilization), Step scaling, Scheduled
- Scale in/out the number of running tasks

## ECS vs EKS

| Feature | ECS | EKS |
|---------|-----|-----|
| Orchestrator | AWS proprietary | Kubernetes (open standard) |
| Learning curve | Lower | Higher |
| Ecosystem | AWS-native | Wide Kubernetes ecosystem |
| Pricing | No control plane cost | $0.10/hr per cluster |

## Common Interview Questions

1. What is the difference between a Task Definition and a Task?
2. What is the difference between Fargate and EC2 launch type?
3. How does ECS Service auto scaling work?
4. What are the two IAM roles used in ECS tasks and what is each used for?
5. How do you do a rolling update for an ECS service with zero downtime?
6. What is `awsvpc` network mode and why is it required for Fargate?
7. How do you store container logs from ECS?
8. What is the difference between ECS and EKS?

## Labs / Exercises

- [ ] Push a Docker image to ECR; create a Task Definition using that image
- [ ] Run a Fargate task in a private subnet; verify the container starts and logs to CloudWatch
- [ ] Create an ECS Service behind an ALB; perform a rolling update with a new image version
- [ ] Configure Service Auto Scaling based on CPU utilization
- [ ] Set up the Task Execution Role and Task Role with appropriate permissions
- [ ] Use ECS Exec to connect to a running Fargate container for debugging
- [ ] Implement a blue/green deployment for an ECS service using CodeDeploy
