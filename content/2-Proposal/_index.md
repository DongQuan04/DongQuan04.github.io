---
title: "The Proposal"
date: 2026-06-25
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

In this section, you will outline the summary of the workshop content that you **intend** to deliver.

# Billo — QR Ordering & Payment Application
## AWS Serverless Solution for Small-Scale Diners and Cafés

### 1. Executive Summary

Billo is a mobile application tailored for the tableside ordering and cashless payment model at small-scale diners and cafés via QR codes. Developed during an internship, it addresses the challenges of manual operations in small establishments. The application simultaneously serves two roles within a single app — **Merchant** (menu management, order receiving, invoice generation) and **Customer** (QR scanning, ordering, payment) — connecting to an AWS serverless backend. The platform leverages AWS Lambda, API Gateway, DynamoDB, and Cognito to ensure operational costs remain near zero when the shop has no orders, while maintaining the capability to scale seamlessly under a pay-per-request model as order volume grows.

### 2. Problem Statement

*Current Problem*
Small diners and cafés often handle ordering and payments completely manually: staff write orders on paper, which easily leads to mixed-up dishes or table numbers, and cash payments make end-of-day reconciliation difficult. Commercial POS platforms on the market usually incur high subscription fees and offer redundant features that exceed the actual needs of a small establishment.

*Solution*
Billo utilizes Amazon Cognito for user authentication via phone numbers (OTP via SNS), and Amazon API Gateway to expose REST endpoints (`/auth`, `/menu`, `/orders`, `/invoices`, `/payments`, `/wallet`, `/shifts`). It employs 7 AWS Lambda functions (Python 3.11) to handle distinct business logics sharing a single Lambda Layer, along with 5 Amazon DynamoDB tables (`Merchants`, `Orders`, `Invoices`, `Wallets`, `Shifts`) configured under the PAY_PER_REQUEST model. The client-side is a multi-mode Flutter application using `provider` for state management, supporting table QR scanning for ordering and invoice QR scanning for payments. Unlike commercial POS platforms, Billo is designed to be lightweight and strictly serves a core operational flow: table scanning → ordering → invoice generation → payment.

*Benefits and Return on Investment (ROI)*
The solution completely eliminates paper ordering for the shop, reduces communication errors between the kitchen and waitstaff, and centralizes order data on DynamoDB to facilitate end-of-shift/end-of-day reconciliation via the `Shifts` table. Thanks to the PAY_PER_REQUEST serverless architecture, infrastructure costs are virtually zero when traffic is low, with no fixed server maintenance costs incurred. The only required devices are the existing smartphones of the shop owner and customers, eliminating the need to invest in dedicated POS hardware.

### 3. Solution Architecture

The platform adopts a fully serverless AWS architecture, defined as Infrastructure as Code using the AWS SAM. Customers and merchants interact through the same Flutter application, making REST API calls via Amazon API Gateway; Lambda functions handle the business logic and perform read/write operations on DynamoDB; Amazon Cognito manages user authentication via phone numbers.

![Kiến trúc tổng thể Billo](/images/2-Proposal/Diagram1.png)
*AWS Services Used*
- *Amazon Cognito*: User authentication via phone number, verifying OTPs sent through SNS.
- *Amazon API Gateway*: Exposes REST endpoints for all business workflows (`/auth`, `/menu`, `/orders`, `/invoices`, `/payments`, `/wallet`, `/shifts`).
- *AWS Lambda*: 7 functions handling distinct tasks (registration/login, menu management, order management, invoice generation, payment processing, wallet management, shift management), utilizing a single Lambda Layer for code reuse.
- *Amazon DynamoDB*: 5 tables (`Merchants`, `Orders`, `Invoices`, `Wallets`, `Shifts`) operating under the PAY_PER_REQUEST model, eliminating the need for manual capacity management.

*Component Design*
- *Flutter Application*: Divided into 3 main code areas — `customer/` (Customer screens & logic), `merchant/` (Merchant screens & logic), and `shared/` (design system, models, and provider for mode management). Uses `mobile_scanner` to scan table and invoice QR codes.
- *Merchant Workflow*: Register the shop (generating a `merchant_id` from the shop's name), manage the menu, confirm orders, and issue invoices embedded with payment QR codes.
- *Customer Workflow*: Scan the table QR code (format: `BILLO_TABLE|merchant_id|merchant_name|table_number`) to place orders, and later scan the invoice QR code (format: `BILLO|invoice_id|merchant_id|total|table_number`) to confirm payment.
- *Backend*: AWS SAM packages and deploys the entire infrastructure with just `sam build` and `sam deploy --guided`, ensuring environment reproducibility.

### 4. Technical Implementation

*Implementation Phases*
1. *Preparation*: Install AWS CLI, AWS SAM CLI, Python 3.11, Flutter SDK, and Docker; configure IAM permissions for the AWS account used in the workshop.
2. *Backend Deployment*: Write `template.yaml` to define the 5 DynamoDB tables, 1 Cognito User Pool, 7 Lambda functions, and 1 Lambda Layer; build and deploy using SAM; verify APIs using cURL/Postman and CloudWatch logs.
3. *Flutter App Deployment*: Configure `ApiConfig.baseUrl` to point to the newly deployed API Gateway; test the Merchant workflow (create store, add menu, generate table QR) and Customer workflow (scan QR, order, pay); build the release APK/iOS version for testing on physical devices.
4. *End-to-End Testing*: Execute the full scenario: merchant creates menu → customer scans QR to order → merchant generates invoice → customer pays via QR, confirming data is accurately synchronized on DynamoDB.

*Technical Requirements*
- *Backend*: Knowledge of AWS Lambda (Python 3.11), API Gateway, DynamoDB (partition key/sort key design for 5 tables), Cognito User Pool for phone number authentication, and AWS SAM for Infrastructure as Code.
- *Frontend*: Flutter with `provider` for multi-role state management (ModeProvider, MerchantProvider, CustomerProvider), `http` for REST API calls, `mobile_scanner` for QR scanning, `shared_preferences` for local storage, and `intl` for currency/datetime formatting adapted to `vi_VN`.

### 5. Roadmap & Implementation Milestones

- *Preparation Phase*: Install tools, research AWS SAM, and explore the structure of the multi-mode Flutter project.
- *Backend Deployment Phase*: Build and deploy the entire serverless infrastructure, and verify APIs.
- *Frontend Deployment Phase*: Connect the Flutter app to the backend, complete the Merchant and Customer workflows, and build the release version.
- *Testing Phase*: Conduct end-to-end testing on real devices and fix emerging bugs.
- *Wrap-up Phase*: Clean up AWS resources and finalize the internship report documentation.

### 6. Budget Estimation

Since all services utilize the PAY_PER_REQUEST / pay-as-you-go model, infrastructure costs during the workshop and demo phase are virtually negligible.

*Estimated Infrastructure Costs (Based on low usage during the demo/workshop phase)*
- AWS Lambda: ~$0 USD/month (well within the free tier of 1 million requests/month).
- Amazon API Gateway: Charges based on the actual number of requests, extremely low at demo scale.
- Amazon DynamoDB (PAY_PER_REQUEST): Charges based on actual read/write operations, incurring no costs when there is no traffic.
- Amazon Cognito: Free within the free tier limits for Monthly Active Users (MAU) at a workshop scale.
- Amazon SNS (OTP delivery): Cost based on the actual number of SMS messages sent.

*Total*: Estimated under $5 USD/month for the demo/workshop scale, which can be verified in detail using the [AWS Pricing Calculator](https://calculator.aws/).

### 7. Risk Assessment

*Risk Matrix*
- Misconfiguration of `ApiConfig.baseUrl` preventing the app from calling the API: High impact, medium probability.
- Cognito authentication failure (OTP fails to send via SNS): Medium impact, low probability.
- Mis-scanning or format errors in table or invoice QR codes: Medium impact, low probability.
- Exceeding IAM permission boundaries when deploying/cleaning up resources: Low impact, low probability.

*Mitigation Strategies*
- Double-check the `ApiUrl` output from `sam deploy` before configuring it in the Flutter app.
- Check CloudWatch logs for each Lambda function to detect errors early.
- Standardize QR formats (`BILLO_TABLE|...` and `BILLO|...`) and test across various scanning devices.
- Apply strict least-privilege IAM policies after completing the workshop, replacing the broader policies used during learning.

*Contingency Plan*
- Fall back to manual API calls via cURL/Postman to verify that the backend functions independently of Flutter-side bugs.
- Use `sam delete` to tear down and redeploy the entire stack if an environment reset is required.

### 8. Expected Outcomes

*Technical Improvement*: Delivery of a fully operational cashless ordering and payment system running on AWS, completely replacing manual paper ordering and cash handling workflows at the venue.

*Long-term Value*: The serverless architecture allows for seamless scalability to incorporate additional features (promotions, loyalty points, shift revenue reports) without altering the foundational infrastructure. It also serves as a reusable codebase and knowledge base for AWS SAM and multi-mode Flutter development in future projects.