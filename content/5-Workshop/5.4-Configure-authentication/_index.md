---
title: "Configure Authentication"
date: 2026-06-29
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

---

This section explains how authentication is configured in AWS BILLO using Amazon Cognito.

Amazon Cognito is responsible for user registration, phone number verification, OTP confirmation, login, JWT token issuing, and user group management.

AWS BILLO uses Cognito to support three main roles:

- Customer
- Merchant
- Admin

---

## Authentication Flow

The authentication flow works as follows:

1. A user registers with a phone number and password.
2. Amazon Cognito creates the user account.
3. Cognito sends an OTP code to the registered phone number.
4. The user confirms the OTP code.
5. After confirmation, the backend creates the user profile and wallet.
6. The user logs in and receives JWT tokens.
7. The frontend uses the JWT token to call protected APIs through API Gateway.
8. API Gateway verifies the JWT token before forwarding the request to Lambda.

---

## Step 1: Open Amazon Cognito

Open the AWS Management Console and go to:

```text
Amazon Cognito > User pools
```

Find the User Pool used by the project:

```text
ap-southeast-1_AKc39KB4L
```

This is the Cognito User Pool for the development environment.

---

## Step 2: Check User Pool Configuration

In the User Pool, check the basic configuration:

| Item               | Purpose                                          |
| ------------------ | ------------------------------------------------ |
| Sign-in method     | Allows users to sign in with phone number        |
| OTP / verification | Confirms user phone number                       |
| App Client         | Allows the frontend to authenticate with Cognito |
| User Groups        | Separates Customer, Merchant, and Admin roles    |

The User Pool is created and managed by the AWS SAM backend stack.

---

## Step 3: Check Cognito User Groups

AWS BILLO uses Cognito groups to separate user permissions.

Open:

```text
Amazon Cognito > User pools > Groups
```

Check that the following groups exist:

```text
Customer
Merchant
Admin
```

![img.png](img.png)

### Customer Group

Customer is the default user role. A customer can:

- Register and log in
- View wallet balance
- Transfer money
- Scan table QR codes
- Place orders
- Scan payment QR codes
- View transaction history

### Merchant Group

Merchant users can access business features after Admin approval. A merchant can:

- Manage business space
- Create categories
- Create products or services
- Create tables
- View table bills
- Generate payment QR codes
- View merchant transactions

### Admin Group

Admin users can access the Admin Web and review merchant applications. An admin can:

- View pending merchant applications
- Approve merchant applications
- Reject merchant applications
- Assign merchant permissions through backend workflow

---

## Step 4: Understand OTP SMS Limitation

Cognito OTP uses SMS delivery. During development, SMS delivery may depend on AWS SMS sandbox or production SMS configuration.

If the AWS account is still in SMS sandbox mode, only verified phone numbers may receive OTP messages.

If OTP is not received, check:

- The phone number format
- Whether the phone number is verified in sandbox mode
- AWS SMS settings
- Cognito verification settings
- CloudWatch Logs for backend errors

---

## Step 5: Configure Frontend Authentication Values

The Flutter frontend reads AWS backend configuration from:

```text
frontend/config/dev.json
```

Make sure the file contains the correct development values, such as:

```json
{
  "API_BASE_URL": "https://zsqkp5vpb9.execute-api.ap-southeast-1.amazonaws.com/dev",
  "AWS_REGION": "ap-southeast-1",
  "COGNITO_USER_POOL_ID": "ap-southeast-1_AKc39KB4L"
}
```

The actual key names may depend on the project implementation. The important point is that the frontend must use the same Cognito User Pool and API Gateway endpoint created by the backend stack.

---

## Step 6: Test Registration

Run the Flutter frontend and open the registration screen.

Enter:

```text
Phone number
Password
Confirm password
```

Then submit the registration form.

Expected result:

- Cognito creates the user account.
- OTP is sent to the phone number.
- The app opens the OTP confirmation screen.

---

## Step 7: Confirm OTP

Enter the OTP code received by SMS.

Expected result:

- The user account is confirmed in Cognito.
- The backend creates the user profile.
- The backend creates the demonstration wallet.
- The user can proceed to login.

If OTP confirmation fails, check whether the code is correct and whether the phone number is allowed to receive SMS in the current AWS environment.

---

## Step 8: Test Login

After confirming OTP, log in using:

```text
Phone number
Password
```

Expected result:

- Cognito returns JWT tokens.
- The frontend stores the session during runtime.
- The app opens the Customer interface by default.
- Protected API calls can be made through API Gateway.

---

## Step 9: Verify User in Cognito

Open:

```text
Amazon Cognito > User pools > Users
```

Find the registered phone number.

Check that:

- The user exists.
- The user status is confirmed.
- The user attributes are correct.
- The user belongs to the correct group when applicable.

---

## Step 10: Verify Backend Profile in DynamoDB

Open:

```text
DynamoDB > Tables > wallet-app-main-dev
```
![img_1.png](img_1.png)

Check that profile and wallet records were created for the new user.

The exact partition key and sort key depend on the data model, but the table should contain user-related records after successful registration and confirmation.

---

## Expected Result

After completing this section:

- Cognito authentication is working.
- Users can register with phone number.
- OTP confirmation can be completed.
- Users can log in and receive JWT tokens.
- API Gateway can protect backend APIs using Cognito JWT authorization.
- Customer, Merchant, and Admin roles can be separated using Cognito groups.

---

## Troubleshooting

### OTP is not received

Possible causes:

- AWS SMS is still in sandbox mode.
- The phone number is not verified.
- The phone number format is incorrect.
- SMS quota or regional messaging settings are limited.

### Login fails after registration

Possible causes:

- OTP confirmation was not completed.
- Password is incorrect.
- The frontend is pointing to the wrong Cognito User Pool.
- The App Client configuration does not match the frontend configuration.

### API returns unauthorized

Possible causes:

- JWT token is missing.
- JWT token is expired.
- API Gateway authorizer is not configured correctly.
- The frontend is calling the wrong API Gateway endpoint.

