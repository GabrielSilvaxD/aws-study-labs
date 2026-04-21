# CloudWatch – Monitoring & Observability

## Key Concepts

- **Metrics**: Time-series data points published by AWS services or custom code (e.g., `CPUUtilization`, `RequestCount`)
- **Namespace**: Container for metrics (e.g., `AWS/EC2`, `AWS/Lambda`, custom namespaces)
- **Dimension**: Key-value pair used to identify a metric (e.g., `InstanceId=i-1234`)
- **Statistics**: Aggregations over a period – Average, Sum, Min, Max, SampleCount, Percentile
- **Resolution**: Standard (1-minute granularity) or High-Resolution (1-second, custom metrics only)
- **Alarms**: Monitor a metric and trigger actions (SNS, Auto Scaling, EC2 actions) when thresholds are breached
- **Dashboards**: Customizable visualizations for metrics across services and regions

## Alarms

- **States**: OK, ALARM, INSUFFICIENT_DATA
- **Actions**: SNS notification, Auto Scaling policy, EC2 action (stop/start/terminate/reboot)
- **Composite Alarms**: Combine multiple alarms using AND/OR logic

## CloudWatch Logs

- **Log Group**: Container for log streams (e.g., `/aws/lambda/my-function`)
- **Log Stream**: Sequence of log events from a single source (e.g., one Lambda instance)
- **Retention Policy**: Set per log group; default is never expire (can be 1 day to 10 years)
- **Metric Filters**: Extract metric data from log events (e.g., count ERROR occurrences)
- **Subscription Filters**: Stream log data in real time to Lambda, Kinesis, Firehose, or OpenSearch
- **CloudWatch Logs Insights**: Interactive query and analysis of log data using a query language

## CloudWatch Logs Insights – Sample Queries

```sql
-- Most recent 20 errors from Lambda
fields @timestamp, @message
| filter @message like /ERROR/
| sort @timestamp desc
| limit 20

-- P99 duration of Lambda invocations
stats pct(@duration, 99) as p99Duration by bin(5m)
```

## CloudWatch Agent

- Install on EC2 or on-premises servers to collect:
  - **System-level metrics**: Memory, disk usage, network (not available by default)
  - **Custom application logs**: Any log file on the instance
- Configuration stored in SSM Parameter Store for easy deployment

## EventBridge (formerly CloudWatch Events)

- Event-driven automation; react to AWS service events or custom events
- **Rules**: Match events using patterns or schedules (cron/rate expressions)
- **Targets**: Lambda, SQS, SNS, Step Functions, ECS, EC2, CodePipeline, and more
- **Event Bus**: Default (AWS events), custom (your events), partner (SaaS integrations)

## CloudWatch Container Insights

- Collect metrics and logs from ECS, EKS, and Kubernetes on EC2
- Automatically discovers containers and collects CPU, memory, network, and disk metrics

## CloudWatch Application Insights

- Automatically sets up monitoring for .NET and Java applications; detects anomalies

## X-Ray Integration

- AWS X-Ray provides distributed tracing for applications
- CloudWatch ServiceLens combines X-Ray traces with CloudWatch metrics and logs in a single view

## Common Interview Questions

1. What is the difference between CloudWatch Metrics and CloudWatch Logs?
2. How do you monitor memory utilization on an EC2 instance?
3. What are CloudWatch Alarms and what actions can they trigger?
4. How do you use CloudWatch Logs Insights to query logs?
5. What is an EventBridge rule and how does it differ from a CloudWatch Alarm?
6. How do you stream CloudWatch Logs to an external system in real time?
7. What is the default retention period for CloudWatch Logs?
8. How does CloudWatch Contributor Insights help with anomaly detection?

## Labs / Exercises

- [ ] Create a CloudWatch Alarm on EC2 CPUUtilization > 80%; send an SNS email notification
- [ ] Install the CloudWatch Agent on an EC2 instance; collect memory and disk metrics
- [ ] Create a Metric Filter on Lambda logs to count ERROR occurrences; alarm when count > 5
- [ ] Use CloudWatch Logs Insights to find the 10 slowest API Gateway requests
- [ ] Create an EventBridge rule to trigger a Lambda when an EC2 instance is stopped
- [ ] Build a CloudWatch Dashboard showing EC2, RDS, and Lambda metrics side-by-side
- [ ] Enable Container Insights on an ECS cluster and visualize container metrics
