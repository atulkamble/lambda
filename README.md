# AWS Lambda – Training Notes & Practical Lab

Topics: Introduction | Architecture | Function Creation | Testing | Monitoring | AWS CLI

## 1. AWS Lambda Introduction

Definition: AWS Lambda is a serverless compute service that runs code without provisioning or managing servers. Functions execute in response to events.

| Feature           | Details                                                  |
| ----------------- | -------------------------------------------------------- |
| Service           | Serverless Compute                                       |
| Execution         | Event-driven                                             |
| Server Management | AWS Managed                                              |
| Runtimes          | Python, Node.js, Java, .NET, etc.                        |
| Maximum Timeout   | 900 seconds (15 minutes)                                 |
| Memory            | 128 MB – 10,240 MB                                       |
| Scaling           | Automatic, subject to quotas                             |
| Permissions       | IAM Execution Role                                       |
| Monitoring        | Amazon CloudWatch                                        |
| Billing           | Requests and execution duration, plus applicable charges |

## 2. AWS Lambda Architecture

Event Source

S3 / API Gateway / EventBridge / SQS

AWS Lambda Function

Runtime → Handler → Execution

Response / AWS Service

CloudWatch Logs

## 3. Important Terms

| Term           | Definition                                    |
| -------------- | --------------------------------------------- |
| Function       | Code deployed to Lambda                       |
| Runtime        | Environment executing the code                |
| Handler        | Entry point of the function                   |
| Trigger        | Event source invoking Lambda                  |
| Event          | Input data passed to Lambda                   |
| Context        | Execution metadata                            |
| Execution Role | IAM role granting permissions                 |
| Timeout        | Maximum execution duration                    |
| Concurrency    | Number of simultaneous executions             |
| Cold Start     | Initialization of a new execution environment |

## 4. Create Lambda Function – AWS Console

1. Open AWS Console → Lambda → Create function.
2. Select Author from scratch.
3. Function name: `my-lambda-function`
4. Runtime: `Python 3.13`
5. Architecture: `x86_64`
6. Permissions: Create a new role with basic Lambda permissions.
7. Click Create function.
8. Add Python code and click Deploy.
9. Open Test → Create new event → Test.

## 5. Python Lambda Function

File: `lambda_function.py`

```
import jsondef lambda_handler(event, context):    return {        "statusCode": 200,        "body": json.dumps({            "message": "Hello from AWS Lambda!"        })    }
```

Test Event

```

{
  "name": "Atul",
  "service": "AWS Lambda"
}

```

Expected Output

```

{
  "statusCode": 200,
  "body": "{\"message\": \"Hello from AWS Lambda!\"}"
}

```

## 6. AWS CLI Commands

Configure AWS CLI

```

aws configure
aws sts get-caller-identity

```

Package Lambda Code

```

zip function.zip lambda_function.py

```

Create Lambda Function

Requires an existing IAM execution role named `lambda-role`, with Lambda trust permissions and basic CloudWatch Logs access.

```

aws lambda create-function \
  --function-name my-lambda-function \
  --runtime python3.13 \
  --role arn:aws:iam::<ACCOUNT_ID>:role/lambda-role \
  --handler lambda_function.lambda_handler \
  --zip-file fileb://function.zip

```

Replace `<ACCOUNT_ID>` with your AWS account ID.

List Lambda Functions

```

aws lambda list-functions

```

Invoke Lambda

```

aws lambda invoke \
  --function-name my-lambda-function \
  --payload '{}' \
  --cli-binary-format raw-in-base64-out \
  response.json

cat response.json

```

Update Function Code

```

aws lambda update-function-code \
  --function-name my-lambda-function \
  --zip-file fileb://function.zip

```

View CloudWatch Logs

```

aws logs tail \
  /aws/lambda/my-lambda-function \
  --follow

```

Delete Function

```

aws lambda delete-function \
  --function-name my-lambda-function

```

## 7. Common Lambda Triggers

| AWS Service      | Use Case                   |
| ---------------- | -------------------------- |
| API Gateway      | REST API Backend           |
| Amazon S3        | File Upload Processing     |
| EventBridge      | Scheduled Automation       |
| DynamoDB Streams | Database Change Processing |
| Amazon SQS       | Queue Processing           |
| Amazon SNS       | Notification Processing    |

## 8. Lambda vs EC2

| Feature           | AWS Lambda          | Amazon EC2                |
| ----------------- | ------------------- | ------------------------- |
| Compute           | Serverless          | Virtual Server            |
| Server Management | AWS Managed         | Customer manages OS       |
| Scaling           | Automatic           | Manual / Auto Scaling     |
| Execution Limit   | 15 minutes          | No fixed limit            |
| Billing           | Requests + Duration | Instance usage            |
| Best For          | Event-driven tasks  | Long-running applications |

## 9. Points to Remember

1. Serverless: No server provisioning or OS management required.
2. Event-driven: Executes when invoked by an event or request.
3. Timeout: Maximum 15 minutes (900 seconds).
4. Memory: 128 MB to 10,240 MB.
5. Handler: Entry point of the Lambda function.
6. IAM Role: Provides permissions to access AWS services.
7. CloudWatch: Used for logs, metrics and monitoring.
8. Concurrency: Multiple executions can run simultaneously, subject to limits.
9. Stateless: Use S3, DynamoDB or other external services for persistent storage.
10. Cost: Based primarily on requests and execution duration, with applicable additional charges.

## 10. Practical Lab Checklist

Lab Progress

0/7 completed

Create Lambda function using Python 3.13

Add code and deploy function

Create JSON test event

Run test and verify response

Check CloudWatch logs

Invoke Lambda using AWS CLI

Delete Lambda function

Expected Outcome: Students can create, deploy, test, monitor, invoke and delete an AWS Lambda function using the AWS Console and AWS CLI.
