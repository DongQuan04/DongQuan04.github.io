---
title: "Workshop Overview"
date: 2026-06-29
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

---

## Introduction

This workshop introduces how to deploy and test the **AWS BILLO** project, a serverless digital wallet and store management system built on AWS.

AWS BILLO is designed for three main user roles:

- **Customer**: uses the mobile/web app to register, log in, view wallet balance, scan QR codes, order products at a table, make QR payments, and view transaction history.
- **Merchant**: manages business space, products or services, tables, orders, bills, and QR payment sessions.
- **Admin**: uses the Admin Web to review merchant applications and approve or reject business registration requests.

The workshop focuses on deploying the backend serverless infrastructure and running the application demo locally.

---

## Workshop Objectives

After completing this workshop, users should be able to:

- Understand the AWS BILLO system architecture.
- Deploy the backend using AWS SAM.
- Create and manage AWS resources through CloudFormation.
- Use Amazon Cognito for authentication and OTP confirmation.
- Use API Gateway to expose protected backend APIs.
- Use Lambda functions to process business logic.
- Use DynamoDB to store application data.
- Use S3 to store uploaded images and business documents.
- Run the Flutter frontend locally and connect it to the deployed backend.
- Run the Admin Web locally for merchant approval.
- Check backend logs using CloudWatch Logs.
- Test the main Customer, Merchant, and Admin workflows.

---

## AWS Services and Tools Used

This workshop uses the following AWS services and tools:

| Service / Tool             | Purpose                                                                          |
| -------------------------- | -------------------------------------------------------------------------------- |
| Amazon Cognito             | User registration, OTP confirmation, login, JWT tokens, and user groups          |
| Amazon API Gateway         | Exposes backend APIs and protects routes with JWT authorization                  |
| AWS Lambda                 | Runs backend business logic                                                      |
| Amazon DynamoDB            | Stores users, wallets, stores, products, tables, orders, bills, and transactions |
| DynamoDB Idempotency Table | Prevents duplicate transfer and payment processing                               |
| Amazon S3                  | Stores uploaded images and business registration documents                       |
| Amazon CloudWatch Logs     | Monitors Lambda/API logs and supports debugging                                  |
| AWS SAM                    | Builds and deploys the serverless backend                                        |
| AWS CloudFormation         | Manages AWS resources through a stack                                            |

---

## Services Not Included in This Workshop

The following services are not part of the current workshop scope:

| Service                                        | Status          |
| ---------------------------------------------- | --------------- |
| AWS Amplify Hosting                            | Not implemented |
| Amazon S3 + Amazon CloudFront frontend hosting | Not implemented |
| Amazon EC2                                     | Not used        |
| Amazon VPC                                     | Not used        |
| Amazon RDS                                     | Not used        |

The Flutter frontend and Admin Web are currently run locally during development. They are not hosted on AWS in this workshop.

---

## Current Development Stack

The project source code is organized into three main parts:

![img.png](img.png)

---

## Deployment Information

The current development backend uses the following deployment configuration:

| Item                 | Value                                                             |
| -------------------- | ----------------------------------------------------------------- |
| AWS Region           | `ap-southeast-1`                                                  |
| CloudFormation Stack | `wallet-app-backend-dev`                                          |
| API Gateway Endpoint | `https://zsqkp5vpb9.execute-api.ap-southeast-1.amazonaws.com/dev` |
| DynamoDB Main Table  | `wallet-app-main-dev`                                             |
| Cognito User Pool ID | `ap-southeast-1_AKc39KB4L`                                        |

---

## Main Demo Flow

The workshop demo follows this flow:

1. Customer registers or logs in using a phone number through Cognito OTP.
2. Customer opens the app and views wallet and transaction history.
3. Customer submits a business registration request.
4. Admin logs in to the Admin Web and approves the merchant application.
5. Merchant logs in again and opens the business space.
6. Merchant creates product or service categories.
7. Merchant creates products or services with name, price, image, discount, and status.
8. Merchant creates tables and the system generates table QR codes.
9. Customer scans a table QR code, views the menu, selects products, and submits an order.
10. Merchant opens the table bill and reviews the order.
11. Merchant generates a payment QR code for the bill.
12. Customer scans the payment QR code, reviews the bill, and confirms payment.
13. The system records the transaction, updates wallet balances, and saves history.
14. Merchant or Admin reviews orders, payment status, and transaction history.

---

## Expected Result

At the end of this workshop, the backend should be deployed successfully on AWS, and the local Flutter frontend and Admin Web should be able to connect to the deployed backend.

The user should be able to test the main workflows of AWS BILLO, including authentication, merchant approval, product management, table QR ordering, QR payment, and transaction history.

