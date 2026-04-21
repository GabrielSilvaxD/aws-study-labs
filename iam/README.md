# IAM – Identity and Access Management

## Key Concepts

- **User**: Entity representing a person or application; has long-term credentials (password + access keys)
- **Group**: Collection of IAM users; policies attached to the group apply to all members
- **Role**: Identity with temporary security credentials; assumed by users, services, or external accounts
- **Policy**: JSON document defining permissions; attached to users, groups, or roles
- **Principal**: Entity that can make a request (user, role, service, account)

## Policy Types

| Type | Description |
|------|-------------|
| Identity-based | Attached to users, groups, or roles |
| Resource-based | Attached to resources (e.g., S3 bucket policy, Lambda resource policy) |
| Permissions boundary | Maximum permissions an identity can have |
| Organizations SCPs | Maximum permissions for an AWS Organizations member account |
| Session policies | Passed inline when assuming a role |
| ACLs | Legacy; cross-account resource access |

## Policy Structure

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-bucket/*",
      "Condition": {
        "StringEquals": { "aws:RequestedRegion": "us-east-1" }
      }
    }
  ]
}
```

- **Effect**: `Allow` or `Deny`
- **Action**: AWS service action(s) (e.g., `s3:PutObject`, `ec2:*`)
- **Resource**: ARN of the resource the statement applies to
- **Condition**: Optional constraints (IP, MFA, time, etc.)

## Evaluation Logic

1. Explicit **Deny** always wins
2. If no explicit Allow → **implicit Deny**
3. SCPs, permission boundaries, and session policies further restrict the effective permissions

## Authentication Methods

- **Password** + optional MFA (console access)
- **Access Key ID + Secret Access Key** (programmatic access via CLI/SDK)
- **Temporary credentials** via STS (AssumeRole, AssumeRoleWithWebIdentity, GetFederationToken)

## Cross-Account Access

1. Create a role in the target account with a trust policy allowing the source account
2. Source account user/role calls `sts:AssumeRole` to get temporary credentials

## IAM Best Practices

- Enable MFA for root and all IAM users
- Never use root account for day-to-day operations
- Grant least privilege (only permissions needed)
- Use roles for EC2/Lambda/ECS instead of access keys
- Rotate access keys regularly
- Use IAM Access Analyzer to find unintended external access
- Use AWS Organizations + SCPs for multi-account governance

## Common Interview Questions

1. What is the difference between an IAM user, group, and role?
2. What happens when identity-based and resource-based policies conflict?
3. How does cross-account access work in AWS?
4. What is a permissions boundary and when would you use it?
5. Explain the IAM policy evaluation logic.
6. How would you grant an EC2 instance access to S3 without using access keys?
7. What is the difference between authentication and authorization in IAM?
8. How do AWS Organizations SCPs interact with IAM policies?

## Labs / Exercises

- [ ] Create an IAM user with programmatic access and configure the AWS CLI profile
- [ ] Create a group `Developers` with read-only S3 access; add a user and verify permissions
- [ ] Create an IAM role for EC2 (S3 read access); launch an instance with the role and test access from CLI
- [ ] Write a custom policy that allows S3 PutObject only when MFA is present (Condition key: `aws:MultiFactorAuthPresent`)
- [ ] Set up cross-account access: assume a role in Account B from Account A
- [ ] Use IAM Access Analyzer to identify publicly accessible S3 resources
- [ ] Set a permissions boundary on a user to limit max permissions to S3 only
