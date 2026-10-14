```
// Lambda Practice

0. Git clone

git clone https://github.com/atulkamble/lambda.git
cd lambda


1. create file

New-Item lambda_function.py

2. add code

import json

def lambda_handler(event, context):
    return {
        "statusCode": 200,
        "body": json.dumps({
            "message": "Hello from AWS Lambda!"
        })
    }

3. create role

aws iam create-role \
  --role-name lambda-role \
  --assume-role-policy-document file://trust-policy.json

4. create zip folder containing project code

zip function.zip lambda_function.py
OR

Compress-Archive -Path lambda_function.py -DestinationPath function.zip -Force

5. ls

6. function run

get your accout id and paste in following command

aws sts get-caller-identity


aws lambda create-function \
  --function-name my-lambda-function \
  --runtime python3.13 \
  --role arn:aws:iam::535002879962:role/lambda-role \
  --handler lambda_function.lambda_handler \
  --zip-file fileb://function.zip

OR

aws lambda create-function --function-name my-lambda-function --runtime python3.13 --role arn:aws:iam::535002879962:role/lambda-role --handler lambda_function.lambda_handler --zip-file fileb://function.zip

7. get response


aws lambda invoke \
  --function-name my-lambda-function \
  --payload '{}' \
  --cli-binary-format raw-in-base64-out \
  response.json

cat response.json

8. delete function

aws lambda delete-function \
  --function-name my-lambda-function
```
