# Lambda – Serverless Compute

## Key Concepts

- **Function**: Unit of deployment; contains code, runtime, handler, and configuration
- **Handler**: Entry point function that Lambda calls (e.g., `index.handler`)
- **Runtime**: Execution environment – Node.js, Python, Java, Go, .NET, Ruby, or custom runtime
- **Execution Role**: IAM role that Lambda assumes; defines what AWS resources the function can access
- **Concurrency**: Number of simultaneous executions; default 1,000 per region (soft limit)
  - **Reserved Concurrency**: Cap a function's max concurrency
  - **Provisioned Concurrency**: Pre-warm instances to eliminate cold starts
- **Cold Start**: Latency added when Lambda initializes a new execution environment (first invocation or after scaling)
- **Layers**: Shared libraries/dependencies packaged separately from function code (up to 5 layers)
- **Deployment Package**: ZIP file or container image (up to 10 GB)
- **Ephemeral Storage**: `/tmp` directory, 512 MB by default (up to 10 GB)

## Invocation Types

| Type | Description |
|------|-------------|
| Synchronous | Caller waits for response (API Gateway, SDK direct invoke) |
| Asynchronous | Lambda queues the event, retries on failure (S3, SNS, EventBridge) |
| Event Source Mapping | Lambda polls a stream/queue (SQS, Kinesis, DynamoDB Streams) |

## Limits

| Resource | Limit |
|----------|-------|
| Timeout | 15 minutes max |
| Memory | 128 MB – 10,240 MB |
| Deployment package (ZIP) | 50 MB zipped / 250 MB unzipped |
| Container image | 10 GB |
| /tmp storage | 512 MB – 10 GB |
| Environment variables | 4 KB |

## Triggers / Event Sources

- **API Gateway** – HTTP/HTTPS endpoint
- **S3** – Object events (PUT, DELETE, etc.)
- **DynamoDB Streams** – Process table changes
- **Kinesis** – Real-time data stream processing
- **SQS** – Queue-based processing
- **SNS** – Push notifications
- **EventBridge (CloudWatch Events)** – Scheduled or event-driven
- **Cognito / ALB / CloudFront** – Other integrations

## VPC Integration

- Lambda can be deployed inside a VPC to access private resources (RDS, ElastiCache)
- Requires subnets and security groups; uses ENIs (Elastic Network Interfaces)
- Needs a NAT Gateway for internet access when inside a VPC

## Destinations (Async)

- **On Success**: Send result to SQS, SNS, Lambda, or EventBridge
- **On Failure**: Send failure details to SQS, SNS, Lambda, or EventBridge (replaces DLQ for async)

## Common Interview Questions

1. What is a cold start and how can you mitigate it?
2. What is the maximum execution timeout for a Lambda function?
3. How do you share code between multiple Lambda functions?
4. What is the difference between synchronous and asynchronous Lambda invocation?
5. How does Lambda scale? What is reserved vs provisioned concurrency?
6. How do you handle errors in asynchronous Lambda invocations?
7. How would you connect a Lambda function to a private RDS database?
8. What are Lambda Layers and when would you use them?

## Labs / Exercises

- [ ] Create a "Hello World" Lambda in Python; invoke it synchronously from the console and AWS CLI
- [ ] Trigger Lambda from an S3 event (log the object key to CloudWatch Logs)
- [ ] Create a REST API with API Gateway → Lambda → DynamoDB (simple CRUD)
- [ ] Use Lambda Layers to share a common library across two functions
- [ ] Configure provisioned concurrency and measure cold start reduction
- [ ] Process SQS messages with Lambda (standard queue, test DLQ on failure)
- [ ] Schedule a Lambda function using EventBridge (cron expression)
- [ ] Deploy a Lambda function inside a VPC and access an RDS database
