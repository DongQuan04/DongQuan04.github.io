---
title: "Project Overview"
date: 2026-07-10
weight: 9
chapter: false
pre: " <b> 5.9. </b> "
---

---

This section provides a high-level overview of the AWS BILLO product after completing the deployment, authentication configuration, demo execution, and testing steps described in the previous sections.

The objective is to help readers understand what AWS BILLO is, what roles are involved, and the respective functionalities of each role. Detailed features for each role are presented in three subsections: 5.9.1, 5.9.2, and 5.9.3.

---

## Project Introduction

AWS BILLO is an e-wallet application integrated with table-ordering and QR-code payment capabilities. It is built entirely on the AWS Serverless platform (Cognito, API Gateway, Lambda, DynamoDB, S3, CloudWatch) and deployed using AWS SAM/CloudFormation.

Link demo: https://drive.google.com/file/d/1VJ5tb5_vHXsGwWCTScZoVjTF9uem38YJ/view

The application includes two front-end interfaces, currently hosted on AWS Amplify:

Flutter app   → https://dev.d28z1hw6wfvjzy.amplifyapp.com (Customer, Merchant)

Admin Web     → https://dev.d3k8atm3w5sdj3.amplifyapp.com (Admin)


Ready-to-use roles for immediate testing:

Admin: +12065550100/AdminDev2026!

Customer: +12065550101/CustomerDev2026!

Merchant: 0853555443/MerchantDev2026!


All data and business workflows in this section are retrieved directly from the actual development environment deployed in the previous steps:

API Gateway: https://zsqkp5vpb9.execute-api.ap-southeast-1.amazonaws.com/dev

Cognito User Pool ID: ap-southeast-1_AKc39KB4L

DynamoDB Main Table: wallet-app-main-dev

---

## Three System Roles

| Role | Interface | Core Functionalities |
|---|---|---|
| Customer | Flutter app | Register wallet, set up PIN, transfer money, order via table QR, pay via QR |
| Merchant | Flutter app | Register business, manage products/categories/tables, process bills, receive payments |
| Admin | Admin Web | Approve Merchant profiles, manage users/stores, view transactions, process refunds |

---

## Detailed Sections

### [5.9.1 - Customer Features](5.9.1-Customer-features/)

Presents detailed customer functionalities: registration/login, PIN setup, wallet & transaction history, money transfers, table QR code scanning for ordering, and QR bill payment — complete with operational steps, expected results, and illustrative screenshots.

### [5.9.2 - Merchant Features](5.9.2-Merchant-features/)

Presents detailed merchant functionalities: business registration, business workspace, category/product/discount management, table & table-QR management, order receiving & bill processing, and payouts — complete with operational steps, expected results, and illustrative screenshots.

### [5.9.3 - Admin Features](5.9.3-Admin-features/)

Presents detailed admin functionalities: login, overview dashboard, merchant profile approval/rejection, user & store management, transaction tracking, and refund processing — complete with operational steps, expected results, and illustrative screenshots.

---

## Overall Workflow

A summary diagram showing how the three roles interact, exactly as demonstrated end-to-end in section 5.7:

```text
Customer registers & applies for business registration
                        ↓
Admin approves the Merchant profile
                        ↓
Merchant creates stores, products, tables, and table QRs
                        ↓
Customer scans table QR to order food
                        ↓
Merchant processes the bill and generates a payment QR
                        ↓
Customer completes the payment using their PIN
                        ↓
Admin monitors transactions & handles refunds (if any)