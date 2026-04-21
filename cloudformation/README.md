# CloudFormation – Infrastructure as Code

## Key Concepts

- **Template**: JSON or YAML file describing AWS resources to provision
- **Stack**: A collection of AWS resources managed as a single unit from a CloudFormation template
- **Change Set**: Preview of changes before applying an update to a stack
- **Stack Policy**: Controls which resources can be updated or deleted (protects critical resources)
- **Drift Detection**: Identifies resources that have been changed outside of CloudFormation

## Template Structure

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: "My stack description"

Parameters:
  InstanceType:
    Type: String
    Default: t2.micro
    AllowedValues: [t2.micro, t3.micro]

Mappings:
  RegionAMI:
    us-east-1:
      AMI: ami-0abcdef1234567890

Conditions:
  IsProd: !Equals [!Ref Environment, "prod"]

Resources:
  MyEC2:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: !Ref InstanceType
      ImageId: !FindInMap [RegionAMI, !Ref AWS::Region, AMI]

Outputs:
  InstanceId:
    Value: !Ref MyEC2
    Export:
      Name: MyInstanceId
```

## Intrinsic Functions

| Function | Description |
|----------|-------------|
| `!Ref` | Returns the default value of a resource or parameter |
| `!GetAtt` | Returns an attribute from a resource |
| `!Sub` | Substitutes variables in a string |
| `!FindInMap` | Looks up a value in a Mappings section |
| `!If` / `!Equals` | Conditional logic |
| `!ImportValue` | Imports an exported output from another stack |
| `!Join` | Joins a list of values with a delimiter |
| `!Select` | Selects an item from a list |
| `!Split` | Splits a string into a list |

## Nested Stacks

- Break large templates into reusable modular templates
- Parent stack references child stacks via `AWS::CloudFormation::Stack`
- Child outputs can be used in the parent via `!GetAtt ChildStack.Outputs.OutputKey`

## Cross-Stack References

- Export values from one stack using `Outputs > Export`
- Import in another stack using `!ImportValue`
- Stacks with imports cannot be deleted before the exporting stack

## Stack Sets

- Deploy stacks to multiple accounts and/or regions from a single template
- Requires Organizations or self-managed (manual) trust relationships

## CloudFormation vs Terraform

| Feature | CloudFormation | Terraform |
|---------|---------------|-----------|
| Vendor | AWS only | Multi-cloud |
| State | Managed by AWS | Local or remote state file |
| Language | JSON/YAML | HCL |
| Drift detection | Built-in | `terraform plan` |
| Rollback | Automatic | Manual |

## Common Interview Questions

1. What is the difference between a CloudFormation Stack and a Stack Set?
2. How do Change Sets work and why are they important?
3. What happens if a CloudFormation stack update fails?
4. How do you share values between two CloudFormation stacks?
5. What is a Nested Stack and when would you use it?
6. How does Drift Detection work in CloudFormation?
7. What is the difference between `!Ref` and `!GetAtt`?
8. How do you protect a production database from accidental deletion in CloudFormation?

## Labs / Exercises

- [ ] Write a template that creates a VPC, public subnet, IGW, and route table
- [ ] Add Parameters and Mappings to select the correct AMI per region
- [ ] Use a Change Set to preview modifications before updating a stack
- [ ] Create a Nested Stack that provisions an RDS instance; reference outputs in the parent
- [ ] Enable a Stack Policy to prevent the RDS instance from being replaced
- [ ] Deploy a Stack Set across two regions from a single template
- [ ] Trigger Drift Detection and intentionally drift a resource (modify a Security Group rule manually)
- [ ] Use `cfn-lint` to validate a template before deployment
