# AWS Lambda – Complete Master Guide (Interview + Hands-On + Projects)

## 1. What is AWS Lambda?

AWS Lambda is a **Serverless Compute Service** that allows you to run code without provisioning or managing servers.

You upload your code, Lambda executes it automatically when triggered by an event.

### Key Features

| Feature           | Description                           |
| ----------------- | ------------------------------------- |
| Serverless        | No server management                  |
| Auto Scaling      | Scales automatically                  |
| Pay Per Use       | Charged only when code runs           |
| Event Driven      | Triggered by AWS services             |
| High Availability | Built-in by AWS                       |
| Multi Language    | Python, Node.js, Java, .NET, Go, Ruby |

---

## AWS Lambda Architecture

```text
               User Request
                     │
                     ▼
             API Gateway
                     │
                     ▼
               AWS Lambda
                     │
        ┌────────────┼────────────┐
        ▼            ▼            ▼
      DynamoDB      S3          SNS
```

---

## Lambda Execution Flow

```text
Event Occurs
      │
      ▼
Trigger Generated
      │
      ▼
Lambda Invoked
      │
      ▼
Code Execution
      │
      ▼
Response Returned
```

---

## AWS Lambda Use Cases

### 1. Website Backend

```text
User
 │
 ▼
API Gateway
 │
 ▼
Lambda
 │
 ▼
DynamoDB
```

---

### 2. Image Processing

```text
Upload Image to S3
        │
        ▼
S3 Event
        │
        ▼
Lambda
        │
        ▼
Resize Image
        │
        ▼
Store in Another Bucket
```

---

### 3. Log Processing

```text
CloudWatch Logs
       │
       ▼
Lambda
       │
       ▼
Elasticsearch/OpenSearch
```

---

### 4. Automated Notifications

```text
EventBridge
      │
      ▼
Lambda
      │
      ▼
SNS
      │
      ▼
Email/SMS
```

---

# AWS Lambda Components

```text
AWS Lambda
│
├── Function
├── Runtime
├── Trigger
├── Layer
├── Environment Variables
├── IAM Role
├── Destination
└── Monitoring
```

---

## Lambda Function

A Lambda Function contains:

```text
Code
+
Configuration
+
Execution Role
```

Example:

```python
def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': 'Hello World'
    }
```

---

## Supported Runtimes

| Runtime        | Supported |
| -------------- | --------- |
| Python         | Yes       |
| Node.js        | Yes       |
| Java           | Yes       |
| Go             | Yes       |
| Ruby           | Yes       |
| .NET           | Yes       |
| Custom Runtime | Yes       |

---

# AWS Lambda Pricing Model

```text
Cost =
Requests
+
Execution Duration
```

### Free Tier

* 1 Million Requests/month
* 400,000 GB-seconds

---

## Lambda Lifecycle

```text
Request
   │
   ▼
Cold Start
   │
   ▼
Initialization
   │
   ▼
Execution
   │
   ▼
Response
```

---

## Cold Start

When Lambda creates a new execution environment.

### Causes

* First invocation
* Scaling out
* Inactive function

### Reduce Cold Starts

* Provisioned Concurrency
* Smaller Packages
* Efficient Code

---

# AWS Lambda Limits

| Item                  | Limit          |
| --------------------- | -------------- |
| Timeout               | 15 Minutes     |
| Memory                | 128 MB – 10 GB |
| Ephemeral Storage     | Up to 10 GB    |
| Deployment Package    | 50 MB ZIP      |
| Concurrent Executions | 1000 (Default) |

---

# Lambda Triggers

## Common Triggers

```text
S3
API Gateway
EventBridge
SNS
SQS
CloudWatch
DynamoDB Streams
Kinesis
ALB
```

---

## Trigger Architecture

```text
          Event Sources

 S3      SNS      SQS
  │        │        │
  └────────┼────────┘
           ▼
       AWS Lambda
           ▼
       Process Data
```

---

# Lambda Layers

Layers allow sharing libraries across functions.

```text
Lambda Function
        │
        ▼
      Layer
        │
        ▼
Python Packages
Libraries
Dependencies
```

Benefits:

* Reusability
* Smaller deployment packages
* Easier updates

---

# Environment Variables

Store configuration securely.

Example:

```text
DB_HOST
DB_NAME
API_KEY
REGION
```

Access in Python:

```python
import os

db = os.environ['DB_HOST']
```

---

# IAM Role for Lambda

Lambda requires permissions.

Example:

```json
{
  "Effect": "Allow",
  "Action": [
    "s3:GetObject"
  ],
  "Resource": "*"
}
```

---

# Lambda Monitoring

Services Used:

* Amazon CloudWatch
* AWS X-Ray
* CloudTrail

---

## Monitoring Flow

```text
Lambda
  │
  ▼
CloudWatch Logs
  │
  ▼
Metrics
  │
  ▼
Alarms
```

---

# Lambda vs EC2

| Feature           | Lambda | EC2        |
| ----------------- | ------ | ---------- |
| Server Management | No     | Yes        |
| Auto Scaling      | Yes    | Manual/ASG |
| Pay Per Request   | Yes    | No         |
| Startup Time      | Fast   | VM Boot    |
| Long Running Apps | No     | Yes        |
| OS Access         | No     | Yes        |

---

# Lambda vs ECS

| Feature           | Lambda     | ECS       |
| ----------------- | ---------- | --------- |
| Serverless        | Yes        | Optional  |
| Container Support | Limited    | Native    |
| Runtime Control   | Limited    | Full      |
| Execution Time    | 15 Minutes | Unlimited |
| Microservices     | Excellent  | Excellent |

---

# Lambda vs Kubernetes

| Feature            | Lambda    | Kubernetes   |
| ------------------ | --------- | ------------ |
| Complexity         | Low       | High         |
| Management         | AWS       | User         |
| Scaling            | Automatic | Configurable |
| Cost for Idle Apps | Zero      | Not Zero     |
| Learning Curve     | Easy      | High         |

---

# Lambda Hands-On Project 1

## Hello World Lambda

### Create Function

```bash
aws lambda create-function \
--function-name hello-lambda \
--runtime python3.12 \
--handler lambda_function.lambda_handler \
--zip-file fileb://function.zip \
--role arn:aws:iam::ACCOUNT_ID:role/LambdaRole
```

---

### Invoke Function

```bash
aws lambda invoke \
--function-name hello-lambda \
output.json
```

---

### View Output

```bash
cat output.json
```

---

# Lambda Hands-On Project 2

## S3 File Upload Trigger

Architecture

```text
Upload File
    │
    ▼
S3 Bucket
    │
 Event
    ▼
Lambda
    │
    ▼
Log Filename
```

Python Code

```python
import json

def lambda_handler(event, context):

    for record in event['Records']:
        bucket = record['s3']['bucket']['name']
        key = record['s3']['object']['key']

        print(bucket, key)

    return {
        'statusCode': 200
    }
```

---

# Lambda Hands-On Project 3

## API Gateway + Lambda + DynamoDB

Architecture

```text
User
 │
 ▼
API Gateway
 │
 ▼
Lambda
 │
 ▼
DynamoDB
```

Use Cases:

* Student Management
* Employee Portal
* Inventory Application

---

# Lambda Hands-On Project 4

## Auto Stop EC2 Instances

```text
EventBridge
      │
      ▼
Lambda
      │
      ▼
Stop EC2
```

Python Example

```python
import boto3

ec2 = boto3.client('ec2')

def lambda_handler(event, context):

    ec2.stop_instances(
        InstanceIds=['i-123456']
    )

    return "Stopped"
```

---

# Important AWS CLI Commands

## List Functions

```bash
aws lambda list-functions
```

---

## Get Function

```bash
aws lambda get-function \
--function-name hello-lambda
```

---

## Update Function Code

```bash
aws lambda update-function-code \
--function-name hello-lambda \
--zip-file fileb://function.zip
```

---

## Delete Function

```bash
aws lambda delete-function \
--function-name hello-lambda
```

---

## View Logs

```bash
aws logs describe-log-groups
```

---

## Tail Logs

```bash
aws logs tail \
/aws/lambda/hello-lambda \
--follow
```

---

# AWS Lambda Interview Questions

### Q1. What is AWS Lambda?

Serverless compute service that runs code in response to events.

---

### Q2. What is a Cold Start?

Delay caused when Lambda creates a new execution environment.

---

### Q3. Maximum Lambda Timeout?

15 Minutes.

---

### Q4. Can Lambda Access VPC?

Yes.

---

### Q5. What is Lambda Layer?

Reusable package of libraries and dependencies.

---

### Q6. Difference Between Lambda and EC2?

Lambda is serverless and event-driven; EC2 requires server management.

---

### Q7. What Triggers Lambda?

S3, SNS, SQS, API Gateway, EventBridge, DynamoDB Streams, Kinesis, ALB.

---

### Q8. What is Provisioned Concurrency?

Keeps Lambda environments warm to reduce cold starts.

---

### Q9. Can Lambda Run Containers?

Yes, using container images up to 10 GB.

---

### Q10. How Does Lambda Scale?

Automatically based on incoming events.

---

# Scenario-Based Interview Questions

### Scenario 1

Users upload images to S3 and thumbnails must be created automatically.

**Solution**

```text
S3
 │
 ▼
Lambda
 │
 ▼
Thumbnail
 │
 ▼
S3
```

---

### Scenario 2

Stop development EC2 instances every night.

**Solution**

```text
EventBridge Schedule
         │
         ▼
      Lambda
         │
         ▼
    Stop EC2
```

---

### Scenario 3

Process millions of messages.

**Solution**

```text
SQS
 │
 ▼
Lambda
 │
 ▼
Database
```

---

### Scenario 4

Build serverless REST API.

**Solution**

```text
API Gateway
      │
      ▼
Lambda
      │
      ▼
DynamoDB
```

---

### Scenario 5

Audit all AWS account activities.

**Solution**

```text
CloudTrail
     │
     ▼
EventBridge
     │
     ▼
Lambda
     │
     ▼
SNS Alert
```

---

# Architecture Patterns

## 1. Event Driven Architecture

```text
S3/SNS/SQS
     │
     ▼
 Lambda
     │
     ▼
Processing
```

---

## 2. Serverless Web Application

```text
CloudFront
    │
    ▼
API Gateway
    │
    ▼
Lambda
    │
    ▼
DynamoDB
```

---

## 3. Data Processing Pipeline

```text
Kinesis
   │
   ▼
Lambda
   │
   ▼
S3
   │
   ▼
Athena
```

---

# Points to Remember for Interviews

✅ Lambda = Serverless Compute

✅ Maximum Timeout = 15 Minutes

✅ Auto Scaling

✅ Event Driven

✅ Supports VPC

✅ Uses IAM Roles

✅ Cold Starts Exist

✅ Layers Share Dependencies

✅ Provisioned Concurrency Reduces Cold Starts

✅ API Gateway + Lambda + DynamoDB = Most Popular Serverless Architecture

✅ CloudWatch = Monitoring

✅ EventBridge = Scheduling

---

# Real-World Project for Resume

### Serverless Student Management System

```text
Frontend (React)
       │
       ▼
API Gateway
       │
       ▼
Lambda
       │
       ▼
DynamoDB
       │
       ▼
CloudWatch
```

Features:

* Student CRUD Operations
* Authentication
* Logging
* Monitoring
* Auto Scaling
* Serverless Architecture

This is an excellent AWS DevOps / Solutions Architect interview project because it covers Lambda, API Gateway, IAM, DynamoDB, CloudWatch, and EventBridge in one end-to-end architecture.


Lambda vs EC2 Operational Effort

Relative infrastructure management effort.

service	effort
Lambda	1
EC2	10
