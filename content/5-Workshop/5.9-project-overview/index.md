---
title: "Project Overview"
date: 2026-07-10
weight: 9
chapter: false
pre: " <b> 5.9. </b> "
---

---

This section provides an overview of the AWS BILLO product after completing the deployment, authentication configuration, demo run, and testing steps in the previous sections.

The goal is to help readers understand what AWS BILLO is, what roles it includes, and what functions each role has. The details of each function by role are presented in three sub-sections: 5.9.1, 5.9.2, and 5.9.3.

---

## Project Introduction

AWS BILLO is an e-wallet application combined with table-QR food ordering/payment, built on the AWS serverless platform (Cognito, API Gateway, Lambda, DynamoDB, S3, CloudWatch) and deployed using AWS SAM/CloudFormation.

The application consists of two interfaces, both currently hosted on AWS Amplify:

Flutter app   → https://dev.d28z1hw6wfvjzy.amplifyapp.com (Customer, Merchant)

Admin Web     → https://dev.d3k8atm3w5sdj3.amplifyapp.com (Admin)


All data and business flows in this section are taken directly from the development environment deployed in the previous sections:

API Gateway: https://zsqkp5vpb9.execute-api.ap-southeast-1.amazonaws.com/dev

Cognito User Pool ID: ap-southeast-1_AKc39KB4L

DynamoDB Main Table: wallet-app-main-dev

---

## The System's Three Roles

| Role | Interface | Main Functions |
|---|---|---|
| Customer | Flutter app | Wallet registration, PIN setup, money transfer, table-QR food ordering, QR payment |
| Merchant | Flutter app | Business registration, product/category/table management, bill processing, receiving payments |
| Admin | Admin Web | Approving Merchant applications, user/store management, viewing transactions, processing refunds |

---

## Detailed Sections

### [5.9.1 - Customer Functions](5.9.1-Customer-features/)

Presents each Customer function in detail: sign-up/login, PIN setup, wallet and transaction history, money transfer, table-QR scanning and ordering, bill payment via QR — including operation steps, expected results, and illustrative images.

### [5.9.2 - Merchant Functions](5.9.2-Merchant-features/)

Presents each Merchant function in detail: business registration, business workspace, category/product/discount management, table and table-QR management, receiving orders and processing bills, payment — including operation steps, expected results, and illustrative images.

### [5.9.3 - Admin Functions](5.9.3-Admin-features/)

Presents each Admin function in detail: login, overview dashboard, approving/rejecting Merchant applications, user and store management, viewing transactions and processing refunds — including operation steps, expected results, and illustrative images.

---

## Overall Flow

A summary diagram of how the three roles work together, matching the end-to-end demo shown in section 5.7:

```text
Customer signs up & registers a business
        ↓
Admin approves the Merchant application
        ↓
Merchant creates the store, products, tables, and table QR codes
        ↓
Customer scans the table QR code and orders food
        ↓
Merchant processes the bill and generates the payment QR code
        ↓
Customer pays using their PIN
        ↓
Admin monitors transactions & processes refunds (if any)
```

---

## Expected Results

After completing this section:

- Readers understand what AWS BILLO is and what roles it includes.
- Readers grasp the overall business flow connecting the three roles.
- Readers can dive deeper into each specific role through sections 5.9.1, 5.9.2, and 5.9.3.