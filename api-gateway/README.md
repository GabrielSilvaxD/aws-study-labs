# API Gateway – Managed API Service

## Key Concepts

- **API Gateway**: Fully managed service for creating, publishing, maintaining, monitoring, and securing REST, HTTP, and WebSocket APIs
- **REST API**: Full-featured; supports API keys, usage plans, request/response transformation, caching
- **HTTP API**: Lightweight and faster; lower cost; supports Lambda proxy and HTTP integrations, JWT authorizers
- **WebSocket API**: Full-duplex communication; real-time two-way messaging

## REST API Components

- **Resource**: URL path segment (e.g., `/users`, `/users/{id}`)
- **Method**: HTTP verb on a resource (GET, POST, PUT, DELETE, PATCH, ANY)
- **Integration**: Backend target (Lambda, HTTP, AWS Service, Mock, VPC Link)
- **Stage**: Named snapshot of the API deployment (e.g., `dev`, `staging`, `prod`)
- **Stage Variables**: Key-value pairs available at runtime; useful for pointing to different Lambda aliases or backends per stage

## Integration Types

| Type | Description |
|------|-------------|
| Lambda Proxy | Passes entire request to Lambda; Lambda returns full response |
| Lambda Custom | Control request/response mapping with VTL templates |
| HTTP Proxy | Forward request to any HTTP endpoint |
| AWS Service | Directly invoke AWS services (e.g., SQS, SNS, DynamoDB) |
| Mock | Return a response without calling a backend |

## Request/Response Flow (REST API)

```
Client → Method Request → Integration Request → Backend
Client ← Method Response ← Integration Response ← Backend
```

- Apply request validation, transformation (mapping templates), and response shaping at each stage

## Authorizers

| Type | Description |
|------|-------------|
| IAM | Sigv4 signed requests; uses IAM policies for authorization |
| Lambda (Custom) | Custom logic in a Lambda function; returns IAM policy |
| Cognito User Pools | Validate JWT tokens from a Cognito User Pool |
| JWT (HTTP API) | Validate JWT from any OIDC-compliant identity provider |

## Throttling & Quotas

- **Default Limits**: 10,000 RPS (requests/sec) per account, 5,000 burst
- **Usage Plans**: Throttle and quota individual API consumers (via API keys)
- **Per-method throttling**: Override limits at the method level

## Caching

- Enable response caching per stage (TTL 0–3,600 seconds); reduces backend calls
- Cache capacity: 0.5 GB – 237 GB; cache can be invalidated by clients with `Cache-Control: max-age=0`

## CORS

- Enable for browser-based clients calling the API from a different origin
- API Gateway returns `Access-Control-Allow-*` headers in the OPTIONS preflight response

## Deployment Best Practices

- Use **Canary Releases** to route a percentage of traffic to a new deployment before full rollout
- Use **VPC Link** to securely connect API Gateway to private resources (NLB in a VPC)
- Enable **Access Logs** to CloudWatch for request-level logging
- Use **X-Ray** tracing for end-to-end performance insights

## Common Interview Questions

1. What is the difference between REST API and HTTP API in API Gateway?
2. What is the difference between Lambda Proxy integration and Lambda Custom integration?
3. How do you secure an API Gateway endpoint?
4. How does API Gateway caching work? How do you invalidate the cache?
5. What are Stage Variables and how would you use them?
6. How would you implement rate limiting for API consumers?
7. How do you enable CORS on API Gateway?
8. How does VPC Link work and when would you use it?

## Labs / Exercises

- [ ] Create a REST API with a `/hello` GET resource using Lambda Proxy integration
- [ ] Add path parameters (`/users/{userId}`) and query string parameters; validate them
- [ ] Secure the API with a Cognito User Pool authorizer; test with a valid and invalid JWT
- [ ] Enable response caching on the prod stage; verify cache hits via CloudWatch metrics
- [ ] Create a Usage Plan with API Keys; throttle a consumer to 100 requests/minute
- [ ] Build an HTTP API with Lambda integration; compare performance and cost with REST API
- [ ] Set up CORS and call the API from a browser-based application
- [ ] Use a VPC Link to connect API Gateway to a private NLB in a VPC
