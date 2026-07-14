---
title: "Admin Features"
date: 2026-07-10
weight: 3
chapter: false
pre: " <b> 5.9.3. </b> "
---

---

This section describes in detail each functionality designed for the Admin role in AWS BILLO, complete with actual operational steps, expected results, and illustrative screenshots from the deployed demo at https://dev.d3k8atm3w5sdj3.amplifyapp.com

The Admin is responsible for system operations: approving Merchant profiles, managing users/stores, monitoring transactions, and processing refunds.

---

## 1. Admin Login

Only accounts belonging to the Cognito group `Admin` are authorized to access the Admin Web.

Operational steps:

- Open the Admin Web at https://dev.d3k8atm3w5sdj3.amplifyapp.com.
- Enter the phone number and password of the Admin account.
- Click **Đăng nhập quản trị** (Admin Login).

![alt text](image.png)

Expected results:

- If the account belongs to the `Admin` group: login succeeds, navigating directly to the administration dashboard.
- If the account does not belong to the `Admin` group (even with valid Customer/Merchant credentials): access is denied.
- If login fails, check: account information, Cognito User Pool configuration, JWT tokens, errors in the browser console, and backend CloudWatch Logs.

Related components: Amazon Cognito (User Group `Admin`).

---

## 2. Overview Dashboard

Upon logging in, the Admin immediately sees the current operational status of the system.

Operational steps:

- Review the metrics displayed on the dashboard:
  - Number of Merchant profiles pending approval.
  - Number of stores.
  - Number of users.
  - Recent transactions.
  - Pending refunds.
  - Operational status: profiles awaiting review, disabled stores, locked accounts.

![alt text](<Screenshot 2026-07-14 113307-1.png>)

Expected results:

- Metrics displayed on the dashboard accurately match the live data in the DynamoDB table.
- The Admin can leverage the dashboard to quickly identify urgent tasks (pending profiles, pending refunds) without having to navigate to each separate section.
- Detailed technical logs (Lambda, API) are not displayed here — the Admin Web only visualizes business data, while technical logs remain viewable in AWS CloudWatch.

Related components: DynamoDB Main Table.

---

## 3. Approve / Reject Merchant Profiles

The Admin reviews business registration profiles submitted by Customers.

Operational steps — reviewing a profile:

- Navigate to the **Hồ sơ Merchant** (Merchant Profiles) section.
- Select a profile currently in `PENDING` status.
- Review the details: business owner, store name, Citizen ID (CCCD), phone number, and the business license image.

Operational steps — approving a profile:

- Click **Duyệt** (Approve).

Operational steps — rejecting a profile:

- Click **Từ chối** (Reject).
- Enter a reason for rejection if applicable.

![alt text](image-2.png)

Expected results upon Approval:

- The profile status transitions to `APPROVED`.
- The user is assigned to the `Merchant` group in Cognito.
- A store record is generated for the Merchant.
- The Merchant must log in again on the Flutter app to access the business interface.

Expected results upon Rejection:

- The profile status transitions to `REJECTED`.
- The user is not added to the `Merchant` group and remains unauthorized to access business features.

Related components: Amazon Cognito (User Group `Merchant`), DynamoDB Main Table.

---

## 4. User and Store Management

The Admin monitors and intervenes in user accounts as well as store operational statuses.

![alt text](image-3.png)


### 4.1. User Management

Operational steps:

- Navigate to the user list.
- Check current information and roles (customer/merchant/admin).
- Lock/unlock accounts if the feature is enabled.

![alt text](image-4.png) 

User account after being locked

![alt text](image-6.png)

### 4.2. Revoke Merchant Permissions

Operational steps:

- Navigate to the specific user/merchant whose permissions need to be revoked.
- Click **Thu hồi quyền merchant** (Revoke Merchant Permissions).

![alt text](<Screenshot 2026-07-14 113931.png>)

Expected results:

- The user is removed from the `Merchant` group in Cognito.
- The profile reverts to a standard customer.
- The corresponding store is set to inactive.
- Transaction history is preserved intact and will not be deleted.
- The system logs an audit trail for the revocation action.

### 4.3. Store Management

Operational steps:

- Navigate to the store list.
- View the status of each store.
- Enable/disable the operational status as needed.

Expected results: When a store is set to inactive, Customers can no longer place orders even if they scan the table QR code successfully.

Related components: Amazon Cognito (User Group), DynamoDB Main Table.

---

## 5. Admin Logs and Refunds

Admin logs allow monitoring of all system activities, and the refund function facilitates handling refund requests.

### 5.1. View Admin Logs

Operational steps:

- Navigate to the **Nhật ký Admin** (Admin Logs) section.
- View the detailed list of actions, targets, and timestamps.

![alt text](image-7.png)

### 5.2. Process Refunds

Operational steps:

- Navigate to the **Hoàn tiền** (Refunds) section.
- View pending refund requests.
- Approve or reject the request.

![alt text](image-8.png)

Expected results upon Refund Approval:

- The system deducts the amount from the Merchant's wallet.
- The amount is credited back to the Customer's wallet.
- The status of the associated order/transaction is updated to refunded.
- Clicking approve twice consecutively for the exact same request must not trigger a double refund.

Expected results upon Refund Rejection: No funds are transferred between wallets, and the request status is updated to rejected.

Related components: DynamoDB Main Table (transaction, refund).

---

## Troubleshooting / Common Errors

| Scenario | Possible Cause |
|---|---|
| Admin login fails | Incorrect account details, user does not belong to the `Admin` group, or incorrect Cognito/API configuration |
| Merchant profile status fails to update | API Gateway request error, Lambda failure, or insufficient permissions to update Cognito groups |
| Dashboard metrics do not match live data | DynamoDB data is out of sync; check Lambda logs in CloudWatch |
| Refund fails to update balances properly | DynamoDB transaction error; check CloudWatch Logs |

---

## General Expected Outcome

Upon completing this section, core functionalities for the Admin role have been fully validated: logging in, monitoring the dashboard, approving/rejecting Merchant profiles, managing users and stores, viewing transactions, and processing refunds — all working properly on the deployed demo.