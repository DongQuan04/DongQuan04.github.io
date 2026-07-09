---
title : "Prerequisites"
date : 2026-06-16
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

#### Tools Installation

Before getting started, make sure to install the following tools on your local machine:

1. **AWS CLI** and **AWS SAM CLI** — used to build and deploy the backend infrastructure stack.
2. **Python 3.11** — the runtime environment for all Lambda functions.
3. **Flutter SDK** (alongside Android Studio or Xcode) — required to build and run the mobile app on a physical device or emulator.
4. **Docker** — necessary as SAM CLI requires Docker to containerize and build the Lambda packages matching the AWS native runtime environment.

![Tools Installation](/images/5-Workshop/5.2-chuan-bi/tools.png)

#### IAM Permissions

Attach the following IAM permission policy to your AWS user account so you can successfully deploy and clean up resources throughout this workshop:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "cloudformation:*",
                "cloudwatch:*",
                "lambda:*",
                "apigateway:*",
                "dynamodb:*",
                "cognito-idp:*",
                "iam:CreateRole",
                "iam:DeleteRole",
                "iam:AttachRolePolicy",
                "iam:DetachRolePolicy",
                "iam:PutRolePolicy",
                "iam:DeleteRolePolicy",
                "iam:PassRole",
                "iam:GetRole",
                "sns:Publish",
                "s3:CreateBucket",
                "s3:PutObject",
                "s3:GetObject"
            ],
            "Resource": "*"
        }
    ]
}