---
title: "End-to-End Testing"
date: 2026-07-08
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

#### Main Test Scenario

The following table summarizes the complete business workflow covered in the previous sections. Use it as an end-to-end testing checklist.

| # | Step | Role | API Called | Expected Result |
|---|------|------|------------|-----------------|
| 1 | Register a new merchant | Merchant | (Local storage only, no API call) | `merchant_id` is generated automatically and saved to `SharedPreferences` |
| 2 | Add menu items | Merchant | `POST /menu` | Menu items appear in the `Merchants` table |
| 3 | Scan the table QR code | Customer | `GET /menu/{merchant_id}` | The correct menu is displayed for the selected merchant |
| 4 | Submit an order | Customer | `POST /orders` | A new order with `status: NEW` is created in the `Orders` table |
| 5 | View pending orders | Merchant | `GET /orders/{merchant_id}/pending` | The newly submitted order is displayed |
| 6 | Confirm the order and generate an invoice | Merchant | `POST /invoices/confirm` | A new invoice with `status: PENDING` is created |
| 7 | Scan the payment QR code | Customer | (Local QR parsing) | The correct invoice ID and payment amount are displayed |
| 8 | Confirm payment | Customer | `POST /payments` | The invoice status changes to `SUCCESS` |

![Testing checklist](/images/5-Workshop/5.5-kiem-thu/checklist.png)

#### Negative Test Cases

1. **Invalid QR Code**

   Scan a QR code that does not follow the `BILLO_TABLE|` or `BILLO|` format. The application should display the message **"Invalid QR code"** instead of crashing.

![Invalid QR test](/images/5-Workshop/5.5-kiem-thu/invalid-qr.png)

2. **Network Connection Loss**

   Disable Wi-Fi or mobile data while submitting an order. The `CustomerProvider.errorMessage` property should be populated and displayed using a `SnackBar`, ensuring that the application does not remain stuck in an infinite loading state.

3. **Empty Menu**

   Scan the table QR code of a merchant who has not added any menu items. The `MenuScreen` should display the message **"This merchant has not published a menu yet."** instead of showing an empty list without explanation.

![Empty menu](/images/5-Workshop/5.5-kiem-thu/empty-menu.png)

#### Monitor the Application with CloudWatch

During testing, keep the **CloudWatch Logs** for the relevant Lambda functions open, including:

- `billo-manage-orders`
- `billo-create-invoice`
- `billo-process-payment`

Compare the logged requests and responses with the actions performed in the application to quickly identify and troubleshoot any runtime issues.

![CloudWatch monitoring](/images/5-Workshop/5.5-kiem-thu/cloudwatch-parallel.png)

Once every item in the checklist passes successfully, the system is ready for demonstration. In the final section (**5.6**), you will clean up the AWS resources created during the workshop to avoid unnecessary charges.