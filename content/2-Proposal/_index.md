---
title: "Proposal"
date: 2026-06-29
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

---

# AWS BILLO

## A Serverless Digital Wallet and Store Management System on AWS

### 1. Executive Summary

AWS BILLO is a serverless digital wallet and store management system developed during the internship period. The project was designed to demonstrate how AWS cloud services can be used to build a modern application that combines an internal demonstration wallet, merchant POS, table ordering, QR payment, and store management.

The system supports three main roles: Customer, Merchant, and Admin. Customers can register with a phone number, verify OTP, log in, manage a demonstration wallet, transfer money, scan QR codes, order products at a table, make QR payments, and view transaction history. Merchants can register their business, manage store information, products or services, tables, orders, bills, and QR payment sessions. Admins can review merchant applications, approve or reject business registrations, and assign merchant permissions.

The project uses a cost-optimized serverless architecture on AWS. The main AWS services include Amazon Cognito, Amazon API Gateway, AWS Lambda, Amazon DynamoDB, Amazon S3, Amazon CloudWatch, AWS SAM, and AWS CloudFormation. The frontend is developed using Flutter and is designed to support both web and mobile interfaces.

The main purpose of AWS BILLO is not to build a production financial system, but to create a practical MVP that demonstrates cloud-native application design, serverless backend development, role-based access control, QR-based workflows, and AWS service integration.

---

### 2. Problem Statement

#### What’s the Problem?

Many small stores, coffee shops, and food service businesses still manage products, tables, orders, bills, and payments using manual processes or separate tools. This can create several problems:

- Orders may be recorded manually and can be lost or duplicated.
- Table orders are difficult to track when customers order multiple times.
- Customers need to wait for staff to view the menu, order products, or request a bill.
- Store information, product images, and business documents may be stored inconsistently.
- Payment information is not always clearly connected to the related order or bill.
- Customers cannot easily review what they paid for after a transaction.
- Merchants need a simple system to manage stores, products, services, tables, and orders.
- Admins need a clear workflow to review and approve merchant registration applications.
- Existing POS and wallet systems may be too complex or costly for small businesses and student learning projects.

#### The Solution

AWS BILLO provides a unified application that combines wallet features, QR payment, store management, and POS functions in one system.

Customers can scan a QR code attached to a table, open the store menu, select products, and submit orders. The system remembers the active table, so customers can reopen the same table session without scanning the QR code again.

Merchants can view the current bill of each table, add or remove items, update the bill, and generate a payment QR code. Customers scan the payment QR code using the BILLO application and confirm the transaction.

After a successful payment, the order is marked as paid, transaction records are created, and the active table is removed from the customer account. To start a new table session, the customer must scan a new table QR code.

Amazon Cognito handles user registration, OTP confirmation, login, and role-based access control. Amazon API Gateway protects backend APIs using JWT authorization. AWS Lambda processes business logic. Amazon DynamoDB stores application data. Amazon S3 stores business documents and product images.

#### Benefits and Return on Investment

AWS BILLO provides the following benefits:

- Reduces manual store and table management.
- Connects products, orders, bills, payments, and transaction history in one system.
- Allows customers to order from a table using QR code.
- Reduces repeated QR scanning during the same table session.
- Supports QR payment and cash payment workflows.
- Provides transaction and bill history for users.
- Uses serverless services without managing EC2 servers.
- Automatically scales based on actual usage.
- Uses pay-per-request services to reduce development and demonstration costs.
- Provides a practical environment for learning AWS serverless architecture.

For the internship project scope, AWS BILLO was developed as an MVP and demonstration system. The main value of the project is the practical experience gained from designing, implementing, testing, and documenting a serverless application on AWS.

---

### 3. Solution Architecture

AWS BILLO uses a serverless architecture deployed in the AWS Singapore Region: `ap-southeast-1`.

Flutter clients communicate with Amazon Cognito for authentication and Amazon API Gateway for protected business operations. API Gateway uses a Cognito JWT Authorizer to verify user identity before forwarding requests to AWS Lambda functions.

Lambda functions handle user profiles, merchant registration, admin approval, store management, product management, table management, orders, bills, payment sessions, wallet transfers, transaction history, and file upload workflows.

Amazon DynamoDB stores profiles, wallets, merchants, stores, products, tables, active table sessions, orders, payment sessions, and transactions. Amazon S3 stores business licenses, store images, and product images using pre-signed URLs. Amazon CloudWatch collects Lambda logs and supports troubleshooting.

The architecture does not require Amazon EC2, custom VPC, NAT Gateway, or RDS because the system is designed around fully managed serverless services.

### 4. AWS BILLO System Architecture
![img_1.png](img_1.png)

1. User interacts with Flutter Mobile App

2. App initiates sign-up / sign-in request

3. Cognito App Client authenticates user with User Pool

4. Cognito requests OTP delivery

5. Cognito routes sign-up requests to User Pool

6. Cognito triggers Post Confirmation event

7. Lambda writes user profile data to DynamoDB

8. SNS sends OTP SMS to User's phone

9. Cognito User Pool issues JWT Tokens to App

10. Cognito validates access permissions via Groups

11. Admin approves/rejects merchant application

12. App includes JWT Token in API requests header

13. App calls API endpoints

14. API Gateway routes requests to the correct Lambda domain function

15. Lambda functions read/write data to DynamoDB

16. App requests & receives S3 pre-signed URL

17. App uploads images directly to S3 Bucket

18. Lambda functions export operational logs to CloudWatch

### AWS Services Used

- **Amazon Cognito**: Manages phone number registration, OTP confirmation, login, JWT tokens, and user groups for Customer, Merchant, and Admin.

- **Amazon API Gateway**: Provides HTTP APIs and validates Cognito JWT tokens before routing requests to backend services.

- **AWS Lambda**: Executes business logic for authentication profiles, wallets, merchant applications, store management, products, tables, orders, bills, and payments.

- **Amazon DynamoDB**: Stores profiles, wallets, merchants, stores, products, tables, orders, payment sessions, and transaction records.

- **DynamoDB Idempotency Table**: Prevents duplicate wallet transfers and duplicate QR payment requests.

- **Amazon S3**: Stores business licenses, store images, and product images.

- **Amazon CloudWatch**: Stores Lambda logs, monitors backend behavior, and supports debugging.

- **AWS SAM / AWS CloudFormation**: Defines, builds, deploys, and manages AWS resources as Infrastructure as Code.

- **AWS Amplify Hosting or Amazon S3 + Amazon CloudFront**: Can be used to host the Flutter Web application.

---

### Component Design

- **Flutter Application**: Provides interfaces for Customer, Merchant, and Admin on web and mobile platforms.

- **Authentication Module**: Handles registration, OTP confirmation, login, logout, and role-based navigation.

- **Customer Module**: Manages wallet balance, money transfer, QR scanning, table ordering, bill payment, profile, and transaction history.

- **Merchant Module**: Manages business registration, store information, products, services, tables, orders, bills, QR payments, and refunds.

- **Admin Module**: Allows Admin to review, approve, or reject merchant registration applications.

- **API Layer**: API Gateway validates JWT tokens and routes requests to Lambda functions.

- **Business Logic Layer**: Lambda functions validate requests, check permissions, calculate order totals, and execute wallet transactions.

- **Data Layer**: DynamoDB stores application data using partition keys and sort keys for serverless access patterns.

- **File Storage Layer**: S3 stores images and documents uploaded through pre-signed URLs.

- **Monitoring Layer**: CloudWatch records Lambda execution logs and application errors.

---

### Main System Flows

#### Authentication Flow

1. The user enters a phone number and password.
2. Amazon Cognito creates the account.
3. Cognito sends an OTP to the registered phone number.
4. The user confirms the OTP.
5. A Post Confirmation Lambda creates the user profile and wallet.
6. Cognito returns JWT tokens after login.
7. The application reads the user group and opens the appropriate interface.

#### Merchant Registration Flow

1. A customer completes the business registration form.
2. The business license image is uploaded to Amazon S3 using a pre-signed URL.
3. The application is stored in DynamoDB with `PENDING` status.
4. An Admin reviews the application.
5. If approved, the user is added to the Cognito Merchant group.
6. The system creates the merchant’s store.
7. The merchant can access the store management interface.

#### Table Ordering Flow

1. The merchant creates a table.
2. The system generates a QR code for the table.
3. The merchant prints and attaches the QR code to the table.
4. A customer scans the table QR code.
5. The application saves the table as the customer’s active table.
6. The customer views the menu and submits an order.
7. Additional products are merged into the current table bill.
8. The customer can reopen the active table without scanning again.

#### QR Payment Flow

1. The merchant opens the table bill.
2. The merchant reviews and edits the products if necessary.
3. The merchant selects **Generate Payment QR**.
4. The system saves the current bill.
5. A payment session and QR code are created.
6. The customer scans the payment QR code.
7. The application displays the store, products, and total amount.
8. The customer confirms the payment.
9. DynamoDB Transaction deducts the customer wallet and credits the merchant wallet.
10. The order and payment session are marked as paid.
11. Transaction records are created for both users.
12. The active table is removed from the customer account.

---

### 4. Technical Implementation

#### Implementation Phases

- **Phase 1 – Research and Requirements Analysis**
  Study the project requirements, identify the main users, analyze business workflows, and research suitable AWS services.

- **Phase 2 – Architecture and Data Design**
  Design the serverless architecture, DynamoDB access patterns, APIs, user roles, wireframes, and security model.

- **Phase 3 – AWS Infrastructure Development**
  Create Cognito, API Gateway, Lambda, DynamoDB, S3, and CloudWatch resources using AWS SAM.

- **Phase 4 – Core Feature Development**
  Implement authentication, wallets, business registration, store management, product management, orders, QR payments, and transaction history.

- **Phase 5 – Table Management and Billing**
  Implement table QR codes, active table sessions, menu ordering, bill editing, and payment QR generation.

- **Phase 6 – UI/UX Improvement**
  Develop a shared Flutter theme and improve the Customer, Merchant, Admin, and Authentication interfaces.

- **Phase 7 – Testing and Deployment**
  Run frontend and backend tests, validate SAM templates, build the application, deploy the AWS stack, and verify major workflows.

#### Technical Requirements

- Flutter SDK for web and mobile application development.
- Node.js 22.x for AWS Lambda functions.
- AWS SAM CLI for build and deployment.
- Amazon Cognito User Pool with Customer, Merchant, and Admin groups.
- API Gateway HTTP API with JWT Authorizer.
- DynamoDB tables using on-demand capacity.
- DynamoDB transactions for wallet transfers and QR payments.
- S3 pre-signed URLs for direct file upload.
- QR code generation and camera scanning in Flutter.
- Idempotency keys for transfer and payment requests.
- CloudWatch Logs for backend monitoring.
- HTTPS for camera access outside localhost.

---

### 5. Timeline & Milestones

#### Project Timeline

- **Week 7**: Research project requirements, AWS services, user roles, and business flows.

- **Week 8**: Design system architecture, wireframes, DynamoDB data model, and API direction.

- **Week 9**: Create the Flutter project, AWS SAM infrastructure, Cognito, API Gateway, DynamoDB, and S3.

- **Week 10**: Implement authentication, wallet transfer, merchant registration, admin approval, store management, and product management.

- **Week 11**: Implement table management, table QR ordering, bill editing, QR payment, transaction history, and UI/UX improvements.

- **Week 12**: Complete system integration, testing, AWS deployment, documentation, final report, and demonstration preparation.

---

### 6. Budget Estimation

The project uses pay-per-request serverless services. Actual costs depend on the number of users, API requests, OTP messages, stored images, data transfer, and CloudWatch Logs.

A demonstration scenario may include:

- Fewer than 100 monthly active users.
- Approximately 10,000 API requests per month.
- Fewer than 100,000 Lambda executions per month.
- Less than 1 GB of DynamoDB data.
- Less than 2 GB of S3 images and documents.
- Limited Flutter Web traffic.
- A small number of verified SMS OTP destinations.

#### Estimated Infrastructure Costs

- **AWS Lambda**: Expected to remain within the free or very low-cost usage range for demonstration traffic.

- **Amazon API Gateway HTTP API**: Approximately $0.01–$0.05 per month for a small number of requests.

- **Amazon DynamoDB**: Approximately $0.10–$1.00 per month with low on-demand read/write usage.

- **Amazon S3**: Approximately $0.05–$0.50 per month for a small image and document collection.

- **Amazon Cognito**: Expected to have low or no user-pool cost for a small number of monthly active users. SMS delivery is charged separately.

- **Amazon CloudWatch**: Approximately $0.00–$1.00 per month when log volume and retention are controlled.

- **Frontend Hosting**: Approximately $1.00–$5.00 per month depending on hosting, build frequency, storage, and traffic.

- **SMS OTP**: Variable cost based on the destination country, message count, and AWS messaging configuration.

#### Estimated Total

- Backend demonstration environment: approximately **$1–$5 per month**, excluding SMS OTP.
- Backend with Flutter Web hosting: approximately **$2–$10 per month**, excluding SMS OTP.
- Development may remain near zero when AWS credits or eligible free-tier allowances are available.

These values are preliminary estimates and should be verified using the AWS Pricing Calculator before production deployment.

[AWS Pricing Calculator](https://calculator.aws/)

---

### 7. Risk Assessment

#### Risk Matrix

- **SMS Sandbox Restrictions**: High impact, high probability during development.
- **Duplicate Payment Requests**: High impact, medium probability.
- **Insufficient Wallet Balance**: Medium impact, medium probability.
- **QR Session Expiration**: Medium impact, medium probability.
- **Unauthorized API Access**: High impact, low probability.
- **Personal Document Exposure**: High impact, low probability.
- **Cloud Cost Growth**: Medium impact, low probability during demonstration.
- **Camera Permission Problems**: Medium impact, medium probability on web browsers.

#### Mitigation Strategies

- Verify testing phone numbers while Cognito SMS remains in sandbox.
- Request production SMS access before public deployment.
- Use Cognito JWT Authorizer and role checks in protected APIs.
- Use DynamoDB transactions for wallet transfers and QR payments.
- Use idempotency keys to prevent duplicate transactions.
- Recalculate product prices in the backend instead of trusting the frontend.
- Set payment session expiration times.
- Invalidate previous payment QR codes whenever a bill changes.
- Use S3 pre-signed URLs with limited expiration.
- Configure CloudWatch log retention and AWS Budget alerts.
- Require HTTPS and provide image upload or manual code alternatives for QR scanning.

#### Contingency Plans

- Use verified sandbox phone numbers for demonstration.
- Use manual payment session entry if camera scanning is unavailable.
- Preserve transaction and order records for troubleshooting.
- Use CloudFormation rollback if AWS deployment fails.
- Restore DynamoDB data using Point-in-Time Recovery when required.
- Disable affected APIs or payment functions if transaction inconsistencies are detected.

---

### 8. Expected Outcomes

#### Technical Improvements

- Replace manual store and table management with a centralized application.
- Provide secure authentication and role-based access control.
- Support serverless wallet transfers and QR payments.
- Connect table orders, bills, payments, and transaction history.
- Allow customers to reopen an active table without rescanning QR.
- Prevent duplicate payment processing.
- Support direct image and document uploads to Amazon S3.
- Provide a scalable backend without managing EC2 servers.

#### Project Deliverables

- Flutter web and mobile application.
- AWS SAM serverless backend.
- Cognito authentication and role management.
- Customer, Merchant, and Admin interfaces.
- Store, product, table, order, bill, wallet, and payment functions.
- DynamoDB data model.
- S3 image and document storage.
- Architecture diagram and API documentation.
- Frontend and backend testing.
- AWS development deployment.
- Final report and demonstration scenario.

#### Long-Term Value

AWS BILLO can serve as a foundation for a larger digital wallet and small-business management platform.

Future improvements may include:

- Production SMS access.
- Secure token storage and automatic token refresh.
- Forgot-password support.
- Push notifications.
- Merchant revenue reports.
- PDF bill export.
- Inventory management.
- CI/CD pipelines.
- Production frontend hosting.
- Fraud detection and transaction limits.
- Audit logs and financial reconciliation.

