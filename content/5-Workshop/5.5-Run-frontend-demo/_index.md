---
title: "Run Flutter Frontend Demo"
date: 2026-06-29
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

---

This section shows how to run the AWS BILLO Flutter frontend locally and connect it to the deployed AWS backend.

The Flutter app is currently used as the main interface for Customer and Merchant workflows. It is not hosted on AWS Amplify, Amazon S3, or Amazon CloudFront in this workshop.

---

## Step 1: Open the Frontend Folder

Open PowerShell and go to the frontend folder:

```powershell
cd C:\Users\admin\source\repos\AWS_BILLO\frontend
```

Check the folder content:

```powershell
dir
```

The folder should contain:

```text
lib
test
config
pubspec.yaml
```

---

## Step 2: Install Flutter Dependencies

Run:

```powershell
flutter pub get
```

This command downloads the required Flutter packages defined in `pubspec.yaml`.

---

## Step 3: Check Development Configuration

Open the configuration file:

```text
frontend/config/dev.json
```

This file is used to pass AWS backend values into the Flutter app.

Check that the API Gateway endpoint and Cognito values match the deployed backend.

Example:

```json
{
  "API_BASE_URL": "https://zsqkp5vpb9.execute-api.ap-southeast-1.amazonaws.com/dev",
  "AWS_REGION": "ap-southeast-1",
  "COGNITO_USER_POOL_ID": "ap-southeast-1_AKc39KB4L"
}
```

The actual key names may be different depending on the implementation. Do not change the key names unless the Flutter code expects them.

---

## Step 4: Run the Flutter App on Chrome

Run the Flutter app using the AWS development configuration:

```powershell
flutter run -d chrome --dart-define-from-file=config/dev.json
```

If you want to run the app on a fixed port, use:

```powershell
flutter run -d chrome --web-port 8080 --dart-define-from-file=config/dev.json
```

Expected result:
![img_1.png](img_1.png)

- Chrome opens the Flutter app.
- The app loads the authentication screen.
- The app can call the deployed API Gateway endpoint.

---

## Step 5: Test Customer Registration

In the Flutter app:

1. Open the registration screen.
2. Enter a phone number and password.
3. Submit the registration form.
4. Enter the OTP code received by SMS.
5. Confirm the account.
6. Log in with the registered account.

Expected result:

- The customer account is created in Cognito.
- The profile and wallet are created in DynamoDB.
- The Customer home screen is opened.

---

## Step 6: Test Customer Wallet

After logging in, open the wallet or home screen.

Check that the app can display:

- Wallet balance
- Basic profile information
- Transaction history if available
- QR scan feature

If the wallet data does not appear, check:

- API Gateway endpoint in `config/dev.json`
- JWT token session
- Lambda logs in CloudWatch
- DynamoDB user profile and wallet records

---

## Step 7: Test Business Registration

From the Customer account, open the business registration feature.

Enter business information and upload the required document or image.

Expected result:

- The app requests a pre-signed upload URL from the backend.
- The file is uploaded to S3.
- The merchant application is saved in DynamoDB with `PENDING` status.
- Admin can review the application from Admin Web.

---

## Step 8: Test Merchant Features After Approval

After the Admin approves the merchant application, the user should log out and log in again.

Expected result:

- The user receives updated group information.
- Merchant features become available.
- The user can open the business space.

Merchant can then test:

- Create categories.
- Create products or services.
- Upload product images.
- Set product price, discount, and status.
- Create tables.
- Generate table QR codes.

---

## Step 9: Test Table QR Ordering

Using the Customer interface:

1. Scan a table QR code.
2. Open the store menu.
3. Select products.
4. Submit an order.

Expected result:

- The active table is saved for the customer.
- The order is created or merged into the current table bill.
- Merchant can view the bill from the business interface.

---

## Step 10: Test QR Payment

Using the Merchant interface:

1. Open the table bill.
2. Review the selected products.
3. Add or remove products if needed.
4. Generate the payment QR code.

Using the Customer interface:

1. Scan the payment QR code.
2. Review the bill details.
3. Confirm payment.

Expected result:

- Customer wallet balance is deducted.
- Merchant wallet balance is increased.
- Transaction records are created.
- The order is marked as paid.
- The active table is removed from the customer account.

---

## Common Issues

### The app cannot connect to the backend

Check:

```text
frontend/config/dev.json
```

Make sure the API Gateway endpoint is correct.

### Camera does not work on web

Browser camera access may require HTTPS, except on localhost. If the camera does not work, use localhost or test with manual QR input if the app supports it.

### OTP cannot be received

Cognito SMS may still be in sandbox mode. Use a verified phone number or update SMS configuration.

### Merchant features do not appear

The user may need to log out and log in again after Admin approval so the new Cognito group information can be loaded.

---

## Expected Result

After completing this section:

- The Flutter app runs locally on Chrome.
- The app connects to the deployed AWS backend.
- Customer registration and login can be tested.
- Business registration can be submitted.
- Merchant functions can be tested after Admin approval.
- Table QR ordering and QR payment flows can be demonstrated.

