---
title : "Introduction"
date : 2026-06-16
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Workshop Introduction

In this chapter, you will go through the entire deployment process of **Billo** — a cashless ordering and payment application for restaurants/cafes built during the internship, which consists of 2 parts:

+ **Backend**: A serverless architecture on AWS (Lambda + API Gateway + DynamoDB + Cognito), deployed using the AWS SAM framework.
+ **Frontend**: A Flutter mobile application serving dual roles within a single app — **Customer** (scan QR, order, pay) and **Merchant** (manage menu, receive orders, issue invoices).

![Overall Architecture](/images/5-Workshop/5.1-gioi-thieu/kien-truc-tong-the.png)

#### What You Will Learn

+ How to define serverless infrastructure using **AWS SAM** (Lambda, API Gateway, DynamoDB, Cognito) as Infrastructure as Code (IaC).
+ How to decouple backend business logic into independent Lambda functions following the single-responsibility principle.
+ How to connect a multi-mode Flutter application to a REST API backend.
+ How to test the end-to-end business workflow: from scanning a table QR code → placing an order → generating an invoice → completing the payment.

{{% notice info %}}
This workshop assumes you already have a foundational knowledge of AWS (Lambda, API Gateway, DynamoDB, IAM) and Flutter. If you are not familiar with these technologies, you can refer to the official documentation for [AWS SAM](https://docs.aws.amazon.com/serverless-application-model/) and [Flutter](https://docs.flutter.dev/) before getting started.
{{% /notice %}}

#### Detailed Architecture

+ **Amazon Cognito User Pool**: Authenticates users via phone numbers, with OTP verification delivered through Amazon SNS.
+ **Amazon API Gateway**: Exposes REST endpoints (`/auth`, `/menu`, `/orders`, `/invoices`, `/payments`, `/wallet`, `/shifts`).
+ **AWS Lambda (Python 3.11)**: Consists of 7 functions handling distinct business logic, sharing 1 Lambda Layer for code reusability.
+ **Amazon DynamoDB**: Utilizes 5 tables (Merchants, Orders, Invoices, Wallets, Shifts) operating on the PAY_PER_REQUEST mode.
+ **Flutter App**: Organized into 3 main code modules — `customer/`, `merchant/`, and `shared/` (containing the design system, data models, and the provider managing app modes).

Upon completing this workshop, you will have a live backend running on AWS and a fully connected Flutter application capable of demonstrating a complete end-to-end ordering and payment workflow.