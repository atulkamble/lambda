# AWS Lambda – Short Notes & Hands-On Guide

Topics: Introduction to AWS Lambda | Creating a Lambda Function | Testing | Monitoring | AWS CLI

## 1. Introduction to AWS Lambda

AWS Lambda is a serverless compute service that runs code without managing servers. It automatically scales based on incoming events.

| Feature            | Description                                                                  |
| ------------------ | ---------------------------------------------------------------------------- |
| Service Type       | Serverless Compute                                                           |
| Server Management  | Managed by AWS                                                               |
| Execution          | Event-driven                                                                 |
| Supported Runtimes | Python, Node.js, Java, .NET and others                                       |
| Maximum Timeout    | 15 minutes (900 seconds)                                                     |
| Memory             | 128 MB – 10,240 MB                                                           |
| Scaling            | Automatic, subject to concurrency limits                                     |
| Pricing            | Based on requests and execution duration, with additional applicable charges |
| Monitoring         | Amazon CloudWatch                                                            |
| Permissions        | AWS IAM Execution Role                                                       |

## 2. AWS Lambda Architecture

Execution Flow: Event → Lambda Trigger → Function Execution → AWS Service/Response → CloudWatch Logs

## 3. Important Terms & Definitions

| Term           | Definition                                    |
| -------------- | --------------------------------------------- |
| Function       | Code deployed to AWS Lambda                   |
| Runtime        | Environment used to execute code              |
| Handler        | Entry point of the function                   |
| Trigger        | Event source that invokes Lambda              |
| Event          | Input data passed to the function             |
| Context        | Runtime information about execution           |
| Execution Role | IAM role defining AWS permissions             |
| Timeout        | Maximum execution time                        |
| Concurrency    | Number of simultaneous executions             |
| Cold Start     | Initialization of a new execution environment |
| Layer          | Reusable dependencies shared with functions   |

## 4. Steps to Create a Lambda Function

1. Open AWS Console → Lambda.
2. Click Create function.
3. Select Author from scratch.
4. Enter function name: `my-lambda-function`.
5. Select runtime: Python 3.13.
6. Architecture: x86_64.
7. Choose Create a new role with basic Lambda permissions.
8. Click Create function.
9. Add Python code and click Deploy.
10. Open Test → Create new event → Test.

## 5. Python Lambda Function Code

File: `lambda_function.py`

```
import jsondef lambda_handler(event, context):    return {        "statusCode": 200,        "body": json.dumps({            "message": "Hello from AWS Lambda!"        })    }
```

### Test Event

```
{
  "name": "Atul",
  "service": "AWS Lambda"
}
```

### Expected Output

```
{
  "statusCode": 200,
  "body": "{\"message\": \"Hello from AWS Lambda!\"}"
}
```

## 6. AWS CLI Commands

### Check AWS Configuration

```
aws configure
aws sts get-caller-identity
```

### Package Lambda Code

```
zip function.zip lambda_function.py
```

### Create Lambda Function

Requires an existing IAM execution role with a trust policy for `lambda.amazonaws.com`.

```
aws lambda create-function \
  --function-name my-lambda-function \
  --runtime python3.13 \
  --role arn:aws:iam::<ACCOUNT_ID>:role/lambda-role \
  --handler lambda_function.lambda_handler \
  --zip-file fileb://function.zip
```

Replace `<ACCOUNT_ID>` with your AWS account ID and ensure `lambda-role` exists.

### List Functions

```
aws lambda list-functions
```

### Invoke Lambda

```
aws lambda invoke \
  --function-name my-lambda-function \
  --payload '{}' \
  --cli-binary-format raw-in-base64-out \
  response.json

cat response.json
```

### View Function Configuration

```
aws lambda get-function \
  --function-name my-lambda-function
```

### Update Function Code

```
aws lambda update-function-code \
  --function-name my-lambda-function \
  --zip-file fileb://function.zip
```

### View CloudWatch Logs

```
aws logs tail \
  /aws/lambda/my-lambda-function \
  --follow
```

### Delete Function

```
aws lambda delete-function \
  --function-name my-lambda-function
```

## 7. Common Lambda Triggers

| AWS Service      | Lambda Use Case           |
| ---------------- | ------------------------- |
| API Gateway      | Backend REST API          |
| Amazon S3        | Process uploaded files    |
| EventBridge      | Scheduled automation      |
| DynamoDB Streams | React to database changes |
| Amazon SQS       | Process queue messages    |
| Amazon SNS       | Process notifications     |

## 8. Lambda vs EC2

| Feature           | Lambda              | EC2                       |
| ----------------- | ------------------- | ------------------------- |
| Compute           | Serverless          | Virtual Server            |
| Server Management | AWS Managed         | Customer Managed          |
| Scaling           | Automatic           | Manual / Auto Scaling     |
| Execution Limit   | 15 minutes          | No fixed limit            |
| Billing           | Requests + Duration | Instance usage            |
| Best For          | Event-driven tasks  | Long-running applications |

## 9. Points to Remember

- Lambda is a serverless, event-driven compute service.
- Maximum execution timeout is 900 seconds.
- Lambda requires an IAM execution role.
- Functions can be triggered by AWS services or invoked directly.
- CloudWatch provides logs and execution metrics.
- Lambda automatically scales, subject to concurrency quotas.
- Avoid hardcoding credentials; use IAM roles and secure secret storage.
- Lambda functions are stateless; use external storage for persistent data.
- Lambda can integrate with API Gateway, S3, DynamoDB, SQS and SNS.
- Always test the function and check CloudWatch logs.

Hands-on practice: Create a Python Lambda function → Deploy → Test with JSON → Verify output → Check CloudWatch logs → Invoke using AWS CLI → Delete the function.
