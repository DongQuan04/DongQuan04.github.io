---
title: "Testing the Customer Workflow"
date: 2026-06-17
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---

#### Switch to Customer Mode

1. On the **MerchantHomeScreen**, tap the role switch button (⇄ icon) in the AppBar. This action calls `ModeProvider.switchMode()`.
2. The app navigates back to the **ModeSelectScreen** and then directly to the **CustomerHomeScreen** because `hasChosenBefore = true`.

![Switch roles](/images/5-Workshop/5.4-flutter/switch-mode.png)

#### Scan the Table QR Code and Place an Order

1. From the **CustomerHomeScreen**, select **Scan Table QR Code**.
2. Point the camera at the table QR code created in **Section 5.4.2** (`BILLO_TABLE|...`). The `QrScanScreen` recognizes the QR code, calls `CustomerProvider.startTableSession()`, and navigates to the **MenuScreen**.

![Scan table QR code](/images/5-Workshop/5.4-flutter/qr-scan-table.png)

3. On the **MenuScreen**, select several menu items. The selected quantity is updated in real time in the bottom bar.

![Select menu items](/images/5-Workshop/5.4-flutter/menu-select.png)

4. Tap **Place Order**. `CustomerProvider.submitOrder()` sends a `POST /orders` request and navigates to the **OrderWaitingScreen**.

![Order submitted successfully](/images/5-Workshop/5.4-flutter/order-waiting.png)

{{% notice info %}}
Open the `Orders` table in the DynamoDB Console. You should see a new item with `status: "NEW"` containing the menu items that were just submitted.
{{% /notice %}}

#### Generate an Invoice (Merchant) and Complete Payment (Customer)

1. Switch back to the **Merchant** role, open the pending orders screen (`fetchPendingOrders`), confirm the order, and generate an invoice. This sends a `POST /invoices/confirm` request.
2. The merchant app displays a payment QR code using the following format:

```
BILLO|invoice_id|merchant_id|total|table_number
```

![Invoice and payment QR code](/images/5-Workshop/5.4-flutter/invoice-qr.png)

3. Switch back to the **Customer** role. From the **OrderWaitingScreen**, select **Scan Payment QR Code** and scan the invoice QR code.
4. The **InvoiceConfirmScreen** displays the invoice amount, invoice ID, and table number. Tap **Confirm Payment** to send a `POST /payments` request.

![Confirm payment](/images/5-Workshop/5.4-flutter/invoice-confirm.png)

5. After the payment is processed successfully, the app navigates to the **PaymentSuccessScreen**.

![Payment successful](/images/5-Workshop/5.4-flutter/payment-success.png)

You have successfully completed the entire business workflow: the merchant creates a menu, the customer scans a table QR code to place an order, the merchant generates an invoice, and the customer completes the payment by scanning the payment QR code. In the next section (**5.4.4**), you will build a release version of the application and test it on a physical device.