# S3 – Simple Storage Service

## Key Concepts

- **Buckets**: Global namespace (unique across all AWS accounts), regional resource
- **Objects**: Files stored in S3; max size 5 TB; metadata (key-value pairs) supported
- **Storage Classes**:
  | Class | Use Case |
  |-------|----------|
  | Standard | Frequent access |
  | Standard-IA | Infrequent access, retrieval fee |
  | One Zone-IA | Infrequent access, single AZ |
  | Glacier Instant Retrieval | Archive, milliseconds retrieval |
  | Glacier Flexible Retrieval | Archive, minutes–hours retrieval |
  | Glacier Deep Archive | Long-term archive, 12–48 hours retrieval |
  | Intelligent-Tiering | Unknown or changing access patterns |

- **Versioning**: Preserves multiple versions of an object; once enabled, can only be suspended
- **MFA Delete**: Requires MFA to permanently delete object versions (requires versioning enabled)
- **Lifecycle Policies**: Automatically transition or expire objects based on age/access
- **Replication**:
  - **CRR (Cross-Region Replication)**: Replicate objects to a bucket in another region
  - **SRR (Same-Region Replication)**: Replicate within the same region
- **Pre-signed URLs**: Temporary access URL to a private object; useful for time-limited downloads/uploads

## Access Control

- **Bucket Policies**: JSON-based, resource-based policy; can grant cross-account access
- **ACLs (Access Control Lists)**: Legacy; operate at the bucket and object level
- **Block Public Access**: Account-level or bucket-level setting to prevent accidental public exposure
- **IAM Policies**: Identity-based, grant users/roles access to S3 resources

## Encryption

| Type | Description |
|------|-------------|
| SSE-S3 | Server-side encryption with S3-managed keys (AES-256) |
| SSE-KMS | Server-side encryption with AWS KMS keys; audit trail via CloudTrail |
| SSE-C | Server-side encryption with customer-provided keys |
| Client-Side | Encrypt data before uploading to S3 |

## Performance

- **Multipart Upload**: Required for objects > 5 GB; recommended for > 100 MB
- **Transfer Acceleration**: Uses CloudFront edge locations to speed up uploads
- **S3 Select / Glacier Select**: Query subset of data using SQL expressions
- **Request Rate**: 3,500 PUT/COPY/POST/DELETE and 5,500 GET/HEAD requests per second per prefix

## Static Website Hosting

- Enable via bucket properties; supports index and error documents
- Bucket must be public or served through CloudFront

## Event Notifications

- Trigger Lambda, SNS, or SQS on object PUT/DELETE/Restore events

## Common Interview Questions

1. What is the difference between S3 Standard and S3 Standard-IA?
2. How does S3 versioning work and what happens when you delete a versioned object?
3. How do you make an S3 bucket publicly accessible? How do you restrict access?
4. What are pre-signed URLs and when would you use them?
5. How does Cross-Region Replication work and what are its requirements?
6. What is the maximum object size in S3 and how do you upload large files?
7. Explain the different server-side encryption options for S3.
8. How would you host a static website on S3?

## Labs / Exercises

- [ ] Create a bucket with versioning enabled; upload, overwrite, and restore object versions
- [ ] Configure a lifecycle policy to transition objects to Standard-IA after 30 days and expire after 365 days
- [ ] Set up a static website on S3 and serve it via CloudFront with HTTPS
- [ ] Create a pre-signed URL for a private object and test time-expiry
- [ ] Configure S3 event notifications to trigger a Lambda function on object upload
- [ ] Enable CRR between two buckets in different regions
- [ ] Use S3 Select to query a CSV file stored in S3
