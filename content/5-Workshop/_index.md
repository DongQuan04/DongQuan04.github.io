---

title: "Workshop"
date: 2026-06-29
weight: 5
chapter: false
pre: " <b> 5. </b> "
--------------------

# AWS BILLO Workshop

## Deploying the Serverless Backend and Running the Application Demo

This workshop introduces how to deploy and test the **AWS BILLO** project, a serverless digital wallet and store management system built on AWS.

AWS BILLO combines an internal demonstration wallet, merchant POS features, table QR ordering, QR payment, store management, and admin approval workflow. The system is designed for three main user roles: **Customer**, **Merchant**, and **Admin**.

The main purpose of this workshop is to guide users through the process of deploying the backend using AWS serverless services and running the frontend demo locally.

---

## Workshop Scope

This workshop focuses on the actual AWS services and tools used in the AWS BILLO project:

* **Amazon Cognito** for user authentication, OTP confirmation, JWT tokens, and user groups.
* **Amazon API Gateway** for exposing protected backend APIs.
* **AWS Lambda** for backend business logic.
* **Amazon DynamoDB** for storing application data.
* **DynamoDB Idempotency Table** for preventing duplicate transfer and payment processing.
* **Amazon S3** for storing uploaded images and business documents.
* **Amazon CloudWatch Logs** for monitoring and debugging Lambda/API errors.
* **AWS SAM** for building and deploying the serverless backend.
* **AWS CloudFormation** for managing deployed AWS resources through a stack.

The frontend Flutter app and the Admin Web are currently run locally during the development stage. They are not hosted on AWS Amplify, Amazon S3, or Amazon CloudFront in this workshop.

---

## System Architecture

AWS BILLO uses a serverless architecture deployed in the AWS Singapore Region: `ap-southeast-1`.

The frontend communicates with Amazon Cognito for authentication and Amazon API Gateway for backend operations. API Gateway verifies JWT tokens from Cognito before routing requests to AWS Lambda functions. Lambda functions process the main business logic and store data in DynamoDB. S3 is used for uploaded documents and images, while CloudWatch Logs is used for monitoring and troubleshooting.

![AWS BILLO System Architecture](/NTH-DUY-fcaj-workshop/images/5-Workshop/aws-billo-architecture.png)

---

## Workshop Sections

### [5.1 - Workshop Overview](5.1-Workshop-overview/)

This section introduces the AWS BILLO project, the workshop objectives, the system architecture, and the main AWS services used in the project.

### [5.2 - Prerequisites](5.2-Prerequisites/)

This section lists the required tools, accounts, permissions, and local environment setup before deploying the backend and running the application.

### [5.3 - Deploy AWS BILLO Backend](5.3-Deploy-backend/)

This section guides users to build and deploy the backend using AWS SAM and CloudFormation.

### [5.4 - Configure Authentication](5.4-Configure-authentication/)

This section explains how Amazon Cognito is used for phone number registration, OTP confirmation, login, JWT tokens, and user groups.

### [5.5 - Run Flutter Frontend Demo](5.5-Run-frontend-demo/)

This section shows how to run the Flutter app locally and connect it to the deployed AWS backend.

### [5.6 - Run Admin Web Demo](5.6-Run-admin-web-demo/)

This section shows how to run the Admin Web locally for reviewing and approving merchant applications.

### [5.7 - Test Main Business Flows](5.7-Test-main-business-flows/)

This section guides users through the main demo flows, including customer registration, merchant approval, product creation, table QR ordering, QR payment, and transaction history.

### [5.8 - Monitoring and Cleanup](5.8-Monitoring-and-cleanup/)

This section explains how to check Lambda logs in CloudWatch and clean up AWS resources after finishing the workshop.

---

## Expected Result

After completing this workshop, users should be able to:

* Understand the AWS BILLO serverless architecture.
* Deploy the backend using AWS SAM.
* Use Amazon Cognito for authentication and user groups.
* Connect the Flutter frontend to the deployed API Gateway endpoint.
* Run the Admin Web locally.
* Test the main Customer, Merchant, and Admin workflows.
* Check backend logs using CloudWatch Logs.
* Clean up AWS resources after testing.

---

## Important Notes

This workshop is designed for a development and demonstration environment.

The system is not a production financial platform. The wallet balance and transactions are used for demonstration purposes only.

Cognito SMS OTP depends on AWS SMS sandbox or production SMS configuration. During development, only verified phone numbers may be able to receive OTP messages.

The frontend and Admin Web are currently run locally. Hosting with AWS Amplify, Amazon S3, or Amazon CloudFront is not included in this workshop.

