# AWS CLI Commands

The AWS CLI is a powerful tool for interacting with AWS services from the command line.

## Table of Contents

1. [Installation and Configuration](#installation-and-configuration)
2. [Basic Commands](#basic-commands)
3. [EC2 Commands](#ec2-commands)
4. [S3 Commands](#s3-commands)
5. [IAM Commands](#iam-commands)
6. [Lambda Commands](#lambda-commands)
7. [RDS Commands](#rds-commands)
8. [CloudWatch Commands](#cloudwatch-commands)
9. [DynamoDB Commands](#dynamodb-commands)
10. [ECS Commands](#ecs-commands)
11. [EKS Commands](#eks-commands)
12. [CloudFormation Commands](#cloudformation-commands)
13. [Route 53 Commands](#route-53-commands)
14. [SNS Commands](#sns-commands)
15. [SQS Commands](#sqs-commands)
16. [Secrets Manager Commands](#secrets-manager-commands)
17. [Systems Manager (SSM) Commands](#systems-manager-ssm-commands)
18. [CloudTrail Commands](#cloudtrail-commands)
19. [Advanced Commands](#advanced-commands)
20. [More](#more)

## Installation and Configuration

### Install AWS CLI

- **macOS (using Homebrew)**:
  ```bash
  brew install awscli
  ```

- **Linux**:
  ```bash
  curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
  unzip awscliv2.zip
  sudo ./aws/install
  ```

- **Windows**:
  Download and run the MSI installer from the [AWS CLI website](https://aws.amazon.com/cli/).

### Configure AWS CLI

```bash
aws configure
```
- Prompts for AWS Access Key ID, Secret Access Key, default region (e.g., `us-east-1`), and output format (e.g., `json`, `text`, `table`).

### Verify Installation

```bash
aws --version
```
- Displays the installed AWS CLI version (e.g., `aws-cli/2.x.x`).

---

## Basic Commands

### Check AWS CLI Version

```bash
aws --version
```

### List Available Services

```bash
aws help
```
- Shows all available AWS services and commands.

### Set Default Region

```bash
aws configure set region <region-name>
```
- Example: `aws configure set region us-west-2`.

### Set Default Output Format

```bash
aws configure set output <json|text|table>
```
- Example: `aws configure set output table`.

### List All AWS Regions

```bash
aws ec2 describe-regions --output table
```
- Displays all available AWS regions in a readable format.

---

## EC2 Commands

### List EC2 Instances

```bash
aws ec2 describe-instances
```
- Returns details of all EC2 instances in the current region.

### Start an EC2 Instance

```bash
aws ec2 start-instances --instance-ids <instance-id>
```
- Example: `aws ec2 start-instances --instance-ids i-1234567890abcdef0`.

### Stop an EC2 Instance

```bash
aws ec2 stop-instances --instance-ids <instance-id>
```

### Create a New EC2 Instance

```bash
aws ec2 run-instances \
  --image-id <ami-id> \
  --instance-type <instance-type> \
  --key-name <key-pair-name> \
  --subnet-id <subnet-id> \
  --security-group-ids <security-group-id> \
  --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=<instance-name>}]'
```
- Example: `aws ec2 run-instances --image-id ami-0abcdef1234567890 --instance-type t2.micro --key-name MyKeyPair ...`.

### Terminate an EC2 Instance

```bash
aws ec2 terminate-instances --instance-ids <instance-id>
```

### **New:** Create a Security Group

```bash
aws ec2 create-security-group --group-name <group-name> --description "<description>" --vpc-id <vpc-id>
```
- Example: `aws ec2 create-security-group --group-name MySG --description "My security group" --vpc-id vpc-12345678`.

### **New:** Add Rule to Security Group

```bash
aws ec2 authorize-security-group-ingress --group-id <security-group-id> --protocol tcp --port 22 --cidr 0.0.0.0/0
```
- Adds an inbound rule (e.g., SSH access).

### **New:** Describe Security Groups

```bash
aws ec2 describe-security-groups
```

---

## S3 Commands

### List S3 Buckets

```bash
aws s3api list-buckets
```

### Create an S3 Bucket

```bash
aws s3api create-bucket --bucket <bucket-name> --region <region>
```
- Example: `aws s3api create-bucket --bucket my-bucket --region us-east-1`.

### Upload a File to S3

```bash
aws s3 cp <local-file> s3://<bucket-name>/<path>
```

### Download a File from S3

```bash
aws s3 cp s3://<bucket-name>/<path> <local-file>
```

### Sync a Directory to S3

```bash
aws s3 sync <local-directory> s3://<bucket-name>/<path>
```

### Delete an S3 Bucket

```bash
aws s3api delete-bucket --bucket <bucket-name>
```

### **New:** List Objects in a Bucket

```bash
aws s3api list-objects --bucket <bucket-name>
```

### **New:** Delete an Object

```bash
aws s3api delete-object --bucket <bucket-name> --key <object-key>
```

### **New:** Set Bucket Policy

```bash
aws s3api put-bucket-policy --bucket <bucket-name> --policy file://policy.json
```
- `policy.json` defines access permissions.

---

## IAM Commands

### List IAM Users

```bash
aws iam list-users
```

### Create an IAM User

```bash
aws iam create-user --user-name <username>
```

### Attach a Policy to an IAM User

```bash
aws iam attach-user-policy --user-name <username> --policy-arn <policy-arn>
```
- Example: `aws iam attach-user-policy --user-name johndoe --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess`.

### Create an IAM Role

```bash
aws iam create-role --role-name <role-name> --assume-role-policy-document file://trust-policy.json
```

### List IAM Roles

```bash
aws iam list-roles
```

### **New:** Create an IAM Group

```bash
aws iam create-group --group-name <group-name>
```

### **New:** Add User to Group

```bash
aws iam add-user-to-group --user-name <username> --group-name <group-name>
```

### **New:** List Attached User Policies

```bash
aws iam list-attached-user-policies --user-name <username>
```

---

## Lambda Commands

### List Lambda Functions

```bash
aws lambda list-functions
```

### Create a Lambda Function

```bash
aws lambda create-function \
  --function-name <function-name> \
  --runtime <runtime> \
  --role <role-arn> \
  --handler <handler> \
  --zip-file fileb://<zip-file>
```
- Example: `aws lambda create-function --function-name MyFunction --runtime python3.8 ...`.

### Invoke a Lambda Function

```bash
aws lambda invoke --function-name <function-name> output.txt
```

### Update a Lambda Function

```bash
aws lambda update-function-code --function-name <function-name> --zip-file fileb://<zip-file>
```

### **New:** Delete a Lambda Function

```bash
aws lambda delete-function --function-name <function-name>
```

### **New:** List Function Versions

```bash
aws lambda list-versions-by-function --function-name <function-name>
```

### **New:** Get Function Configuration

```bash
aws lambda get-function-configuration --function-name <function-name>
```

---

## RDS Commands

### List RDS Instances

```bash
aws rds describe-db-instances
```

### Create an RDS Instance

```bash
aws rds create-db-instance \
  --db-instance-identifier <instance-id> \
  --db-instance-class <instance-class> \
  --engine <engine> \
  --master-username <username> \
  --master-user-password <password> \
  --allocated-storage <storage>
```

### Delete an RDS Instance

```bash
aws rds delete-db-instance --db-instance-identifier <instance-id> --skip-final-snapshot
```

### **New:** Describe DB Snapshots

```bash
aws rds describe-db-snapshots
```

### **New:** Create a DB Snapshot

```bash
aws rds create-db-snapshot --db-instance-identifier <instance-id> --db-snapshot-identifier <snapshot-id>
```

### **New:** Restore from Snapshot

```bash
aws rds restore-db-instance-from-db-snapshot --db-instance-identifier <new-instance-id> --db-snapshot-identifier <snapshot-id>
```

---

## CloudWatch Commands

### List CloudWatch Logs

```bash
aws logs describe-log-groups
```

### Get CloudWatch Metrics

```bash
aws cloudwatch list-metrics --namespace <namespace>
```
- Example: `aws cloudwatch list-metrics --namespace AWS/EC2`.

### Set a CloudWatch Alarm

```bash
aws cloudwatch put-metric-alarm \
  --alarm-name <alarm-name> \
  --metric-name <metric-name> \
  --namespace <namespace> \
  --statistic <statistic> \
  --period <period> \
  --threshold <threshold> \
  --comparison-operator <operator> \
  --dimensions <dimensions> \
  --evaluation-periods <evaluation-periods> \
  --alarm-actions <alarm-actions>
```

### **New:** List Alarms

```bash
aws cloudwatch describe-alarms
```

### **New:** Describe Alarm History

```bash
aws cloudwatch describe-alarm-history --alarm-name <alarm-name>
```

### **New:** Put Metric Data

```bash
aws cloudwatch put-metric-data --namespace <namespace> --metric-data <metric-data>
```
- Example: `aws cloudwatch put-metric-data --namespace MyApp --metric-data 'MetricName=Errors,Value=5,Unit=Count'`.

---

## DynamoDB Commands

### Create a DynamoDB Table

```bash
aws dynamodb create-table \
  --table-name <table-name> \
  --attribute-definitions <attribute-definitions> \
  --key-schema <key-schema> \
  --provisioned-throughput <provisioned-throughput>
```

### List DynamoDB Tables

```bash
aws dynamodb list-tables
```

### Insert an Item into a DynamoDB Table

```bash
aws dynamodb put-item --table-name <table-name> --item <item>
```

### Query a DynamoDB Table

```bash
aws dynamodb query --table-name <table-name> --key-condition-expression <expression> --expression-attribute-values <values>
```

### Delete a DynamoDB Table

```bash
aws dynamodb delete-table --table-name <table-name>
```

### **New:** Scan a Table

```bash
aws dynamodb scan --table-name <table-name>
```

### **New:** Update an Item

```bash
aws dynamodb update-item --table-name <table-name> --key <key> --update-expression <update-expression> --expression-attribute-values <values>
```

### **New:** Describe a Table

```bash
aws dynamodb describe-table --table-name <table-name>
```

---

## ECS Commands

### List ECS Clusters

```bash
aws ecs list-clusters
```

### Describe an ECS Cluster

```bash
aws ecs describe-clusters --clusters <cluster-name>
```

### List ECS Tasks

```bash
aws ecs list-tasks --cluster <cluster-name>
```

### Run an ECS Task

```bash
aws ecs run-task --cluster <cluster-name> --task-definition <task-definition> --count <count>
```

### Stop an ECS Task

```bash
aws ecs stop-task --cluster <cluster-name> --task <task-id>
```

### **New:** Register a Task Definition

```bash
aws ecs register-task-definition --family <family> --container-definitions <container-definitions>
```

### **New:** List Services

```bash
aws ecs list-services --cluster <cluster-name>
```

### **New:** Update a Service

```bash
aws ecs update-service --cluster <cluster-name> --service <service-name> --desired-count <count>
```

---

## EKS Commands

### List EKS Clusters

```bash
aws eks list-clusters
```

### Describe an EKS Cluster

```bash
aws eks describe-cluster --name <cluster-name>
```

### Update EKS Cluster Configuration

```bash
aws eks update-cluster-config --name <cluster-name> --resources-vpc-config <vpc-config>
```

### List EKS Nodegroups

```bash
aws eks list-nodegroups --cluster-name <cluster-name>
```

### **New:** Create a Nodegroup

```bash
aws eks create-nodegroup --cluster-name <cluster-name> --nodegroup-name <nodegroup-name> --node-role <node-role-arn> --subnets <subnet-ids>
```

### **New:** Describe a Nodegroup

```bash
aws eks describe-nodegroup --cluster-name <cluster-name> --nodegroup-name <nodegroup-name>
```

### **New:** Delete a Nodegroup

```bash
aws eks delete-nodegroup --cluster-name <cluster-name> --nodegroup-name <nodegroup-name>
```

---

## CloudFormation Commands

### Create a CloudFormation Stack

```bash
aws cloudformation create-stack --stack-name <stack-name> --template-body file://<template-file> --parameters <parameters>
```

### Describe a CloudFormation Stack

```bash
aws cloudformation describe-stacks --stack-name <stack-name>
```

### Delete a CloudFormation Stack

```bash
aws cloudformation delete-stack --stack-name <stack-name>
```

### List CloudFormation Stacks

```bash
aws cloudformation list-stacks
```

### **New:** Validate a Template

```bash
aws cloudformation validate-template --template-body file://<template-file>
```

### **New:** List Stack Resources

```bash
aws cloudformation describe-stack-resources --stack-name <stack-name>
```

### **New:** Update a Stack

```bash
aws cloudformation update-stack --stack-name <stack-name> --template-body file://<template-file> --parameters <parameters>
```

---

## Route 53 Commands

### List Hosted Zones

```bash
aws route53 list-hosted-zones
```

### Create a Hosted Zone

```bash
aws route53 create-hosted-zone --name <domain-name> --caller-reference <unique-string>
```

### List DNS Records

```bash
aws route53 list-resource-record-sets --hosted-zone-id <zone-id>
```

### Create a DNS Record

```bash
aws route53 change-resource-record-sets --hosted-zone-id <zone-id> --change-batch file://<change-batch-file>
```

### **New:** Get Hosted Zone Details

```bash
aws route53 get-hosted-zone --id <zone-id>
```

---

## SNS Commands

### List SNS Topics

```bash
aws sns list-topics
```

### Create an SNS Topic

```bash
aws sns create-topic --name <topic-name>
```

### Publish a Message to an SNS Topic

```bash
aws sns publish --topic-arn <topic-arn> --message "<message>"
```

### Subscribe to an SNS Topic

```bash
aws sns subscribe --topic-arn <topic-arn> --protocol <protocol> --notification-endpoint <endpoint>
```

### **New:** List Subscriptions

```bash
aws sns list-subscriptions
```

### **New:** Confirm a Subscription

```bash
aws sns confirm-subscription --topic-arn <topic-arn> --token <token>
```

### **New:** Unsubscribe from a Topic

```bash
aws sns unsubscribe --subscription-arn <subscription-arn>
```

---

## SQS Commands

### List SQS Queues

```bash
aws sqs list-queues
```

### Create an SQS Queue

```bash
aws sqs create-queue --queue-name <queue-name>
```

### Send a Message to an SQS Queue

```bash
aws sqs send-message --queue-url <queue-url> --message-body "<message>"
```

### Receive Messages from an SQS Queue

```bash
aws sqs receive-message --queue-url <queue-url>
```

### Delete an SQS Queue

```bash
aws sqs delete-queue --queue-url <queue-url>
```

### **New:** Get Queue Attributes

```bash
aws sqs get-queue-attributes --queue-url <queue-url> --attribute-names All
```

### **New:** Set Queue Attributes

```bash
aws sqs set-queue-attributes --queue-url <queue-url> --attributes <attributes>
```
- Example: `aws sqs set-queue-attributes --queue-url <queue-url> --attributes '{"VisibilityTimeout": "60"}'`.

### **New:** Purge a Queue

```bash
aws sqs purge-queue --queue-url <queue-url>
```

---

## Secrets Manager Commands

### List Secrets

```bash
aws secretsmanager list-secrets
```

### Create a Secret

```bash
aws secretsmanager create-secret --name <secret-name> --secret-string <secret-string>
```

### Retrieve a Secret

```bash
aws secretsmanager get-secret-value --secret-id <secret-id>
```

### Delete a Secret

```bash
aws secretsmanager delete-secret --secret-id <secret-id>
```

### **New:** Rotate a Secret

```bash
aws secretsmanager rotate-secret --secret-id <secret-id>
```

### **New:** Describe a Secret

```bash
aws secretsmanager describe-secret --secret-id <secret-id>
```

### **New:** List Secret Versions

```bash
aws secretsmanager list-secret-version-ids --secret-id <secret-id>
```

---

## Systems Manager (SSM) Commands

### List SSM Parameters

```bash
aws ssm describe-parameters
```

### Get an SSM Parameter

```bash
aws ssm get-parameter --name <parameter-name>
```

### Put an SSM Parameter

```bash
aws ssm put-parameter --name <parameter-name> --value <value> --type <type>
```

### Run a Command on an EC2 Instance Using SSM

```bash
aws ssm send-command --instance-ids <instance-id> --document-name "AWS-RunShellScript" --parameters 'commands=["<command>"]'
```

### **New:** List Documents

```bash
aws ssm list-documents
```

### **New:** Describe Instance Information

```bash
aws ssm describe-instance-information
```

### **New:** Start a Session

```bash
aws ssm start-session --target <instance-id>
```

---

## CloudTrail Commands

### List CloudTrail Trails

```bash
aws cloudtrail describe-trails
```

### Create a CloudTrail Trail

```bash
aws cloudtrail create-trail --name <trail-name> --s3-bucket-name <bucket-name> --is-multi-region-trail
```

### Start Logging for a CloudTrail Trail

```bash
aws cloudtrail start-logging --name <trail-name>
```

### Stop Logging for a CloudTrail Trail

```bash
aws cloudtrail stop-logging --name <trail-name>
```

### **New:** Get Trail Status

```bash
aws cloudtrail get-trail-status --name <trail-name>
```

### **New:** Update a Trail

```bash
aws cloudtrail update-trail --name <trail-name> --s3-bucket-name <new-bucket-name>
```

### **New:** Delete a Trail

```bash
aws cloudtrail delete-trail --name <trail-name>
```

---

## Advanced Commands

### Use AWS CLI with Assume Role

```bash
aws sts assume-role --role-arn <role-arn> --role-session-name <session-name>
```
- Exports credentials to environment variables:
  ```bash
  aws sts assume-role --role-arn arn:aws:iam::<account-id>:role/<role-name> --role-session-name MySession \
    --query 'Credentials.[AccessKeyId,SecretAccessKey,SessionToken]' --output text | \
    awk '{print "export AWS_ACCESS_KEY_ID="$1"\nexport AWS_SECRET_ACCESS_KEY="$2"\nexport AWS_SESSION_TOKEN="$3}' > creds.sh
  source creds.sh
  ```

### Use AWS CLI with MFA

```bash
aws sts get-session-token --serial-number <mfa-serial-number> --token-code <mfa-code>
```

### Use AWS CLI with Pagination

```bash
aws s3api list-objects --bucket <bucket-name> --page-size <page-size> --max-items <max-items>
```

### Use AWS CLI with JMESPath

```bash
aws ec2 describe-instances --query 'Reservations[*].Instances[?State.Name==`running`].InstanceId'
```

---

## More

1. **Use `--dry-run` to Test Commands**
   ```bash
   aws ec2 run-instances --dry-run ...
   ```
   - Simulates the command without making changes.

2. **Use `--output table` for Readable Output**
   ```bash
   aws ec2 describe-instances --output table
   ```

3. **Use `--query` to Filter JSON Output**
   ```bash
   aws ec2 describe-instances --query 'Reservations[*].Instances[*].PublicIpAddress'
   ```

4. **Enable CLI Auto-Prompt**
   ```bash
   aws configure set cli_auto_prompt on
   ```
   - Provides interactive command completion.

5. **Use AWS CLI Pagination**
   ```bash
   aws s3api list-objects --bucket <bucket-name> --page-size 100 --max-items 1000
   ```
