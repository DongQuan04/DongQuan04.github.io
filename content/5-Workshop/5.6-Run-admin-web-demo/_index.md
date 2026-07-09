---
title: "Run Admin Web Demo"
date: 2026-06-29
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

---

This section shows how to run the AWS BILLO Admin Web locally.

The Admin Web is used by the Admin role to review merchant registration applications, approve or reject requests, and support the merchant activation workflow.

In the current development stage, the Admin Web runs locally using Vite. It is not hosted on AWS Amplify, Amazon S3, or Amazon CloudFront in this workshop.

---

## Step 1: Open the Admin Web Folder

Open PowerShell and go to the Admin Web folder:

```powershell
cd C:\Users\admin\source\repos\AWS_BILLO\admin-web
```

Check the folder content:

```powershell
dir
```

The folder should contain:

```text
src
package.json
vite.config.js
```

---

## Step 2: Install Dependencies

Run:

```powershell
npm install
```

This command installs the dependencies required by the React Admin Web project.

---

## Step 3: Check Backend Configuration

The Admin Web must connect to the deployed AWS backend API.

The current development API Gateway endpoint is:

```text
https://zsqkp5vpb9.execute-api.ap-southeast-1.amazonaws.com/dev
```

Make sure the Admin Web configuration points to the correct API endpoint.

If the Admin Web uses environment variables, check files such as:

```text
.env
.env.local
.env.development
```

The exact configuration file depends on the project implementation.

---

## Step 4: Run Admin Web Locally

Start the Admin Web development server:

```powershell
npm run dev
```

Expected result:
![img_1.png](img_1.png)

```text
VITE ready
Local: http://127.0.0.1:5173
```

The port may be different depending on Vite. Use the URL displayed in the terminal.

---

## Step 5: Open Admin Web in Browser

Open the local Admin Web URL in a browser:

```text
http://127.0.0.1:5173
```

or use the URL shown in the terminal.

Expected result:

- The Admin Web interface is opened.
- The login screen or dashboard is displayed.
- The Admin Web can connect to the deployed backend API.

---

## Step 6: Log in as Admin

Use an Admin account to log in.

Expected result:

- The Admin user is authenticated.
- The Admin Web receives a valid session or token.
- The dashboard or merchant application page is displayed.

If login fails, check:

- Admin account credentials.
- Cognito User Pool configuration.
- Whether the user belongs to the Admin group.
- API Gateway endpoint configuration.
- Browser console errors.
- CloudWatch Logs for backend errors.

---

## Step 7: Review Merchant Applications

After logging in, open the merchant application review page.

The Admin should be able to view merchant applications with statuses such as:

```text
PENDING
APPROVED
REJECTED
```

For each application, the Admin may review information such as:

- Applicant information.
- Business name.
- Business type.
- Business registration details.
- Uploaded business document or image.
- Application status.

---

## Step 8: Approve a Merchant Application

Select a pending merchant application and approve it.

Expected result:

- The merchant application status is updated in DynamoDB.
- The user is added to the Merchant group in Cognito.
- A store record is created for the merchant if the backend workflow supports it.
- The user can log out and log in again to access merchant features in the Flutter app.

After approval, the user should reopen the Flutter app and log in again so the updated Cognito group information can be loaded.

---

## Step 9: Reject a Merchant Application

Select another pending merchant application and reject it if needed.

Expected result:

- The merchant application status is updated to `REJECTED`.
- The user is not added to the Merchant group.
- The user cannot access merchant features.

---

## Step 10: Verify Data in DynamoDB

Open the AWS Management Console and go to:

```text
DynamoDB > Tables > wallet-app-main-dev
```

Check whether the merchant application record has been updated.

The exact key structure depends on the project data model, but the application status should reflect the Admin action.

---

## Step 11: Verify User Group in Cognito

Open:

```text
Amazon Cognito > User pools > Users
```

Find the approved user and check the user groups.

Expected result:

- Approved users should be in the Merchant group.
- Rejected users should not be added to the Merchant group.

---

## Common Issues

### Admin Web cannot connect to backend

Check whether the Admin Web is using the correct API Gateway endpoint:

```text
https://zsqkp5vpb9.execute-api.ap-southeast-1.amazonaws.com/dev
```

Also check browser console errors and backend logs in CloudWatch.

### Admin login fails

Possible causes:

- Wrong account credentials.
- The user is not in the Admin group.
- The Admin Web is pointing to the wrong Cognito or API configuration.
- JWT token is missing or expired.

### Merchant features do not appear after approval

The approved user may need to log out and log in again in the Flutter app so the new Cognito group information can be loaded.

### Application status does not update

Check:

- API Gateway request.
- Lambda logs in CloudWatch.
- DynamoDB record update.
- Cognito group update permissions.

---

## Expected Result

After completing this section:

- The Admin Web runs locally.
- Admin can log in.
- Admin can view merchant registration applications.
- Admin can approve or reject merchant applications.
- Approved users can access Merchant features after logging in again.
- Merchant application status is updated in DynamoDB.

