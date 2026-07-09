---
title: "Testing the Backend APIs"
date: 2026-06-17
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

#### Verify the DynamoDB Tables

1. Navigate to the **DynamoDB Console** > **Tables**.
2. Confirm that the following five tables have been created: `Merchants`, `Orders`, `Invoices`, `Wallets`, and `Shifts`.

![DynamoDB tables](/images/5-Workshop/5.3-backend/dynamodb-tables.png)

#### Verify the Cognito User Pool

1. Navigate to the **Amazon Cognito Console** > **User pools**.
2. Open the `billo-user-pool` user pool and verify that **Phone number** is enabled as a **Sign-in option** and that Amazon SNS is configured for OTP delivery.

![Cognito user pool](/images/5-Workshop/5.3-backend/cognito-pool.png)

#### Test the API Using cURL

1. Use the `ApiUrl` obtained in the previous step and send a request to create a menu item for a test restaurant:

```bash
curl -X POST "$API_URL/menu" \
  -H "Content-Type: application/json" \
  -H "x-merchant-id: pho-thin-bo-ho" \
  -d '{
    "merchant_id": "pho-thin-bo-ho",
    "action": "ADD",
    "item": { "item_id": "1", "name": "Phở bò", "price": 45000, "category": "Main Course" }
  }'
```

![Test menu creation API](/images/5-Workshop/5.3-backend/curl-menu.png)

2. Verify that the menu item has been created by calling the GET endpoint:

```bash
curl "$API_URL/menu/pho-thin-bo-ho"
```

![GET menu response](/images/5-Workshop/5.3-backend/curl-get-menu.png)

{{% notice info %}}
If you receive a `200 OK` response containing the newly added menu item, it confirms that the API Gateway → `ManageMenuFunction` Lambda → `Merchants` DynamoDB table workflow is functioning correctly.
{{% /notice %}}

#### Check the CloudWatch Logs

1. Navigate to the **CloudWatch Console** > **Log groups**.
2. Open the `/aws/lambda/billo-manage-menu` log group and verify that the request was logged successfully without any errors.

![CloudWatch logs](/images/5-Workshop/5.3-backend/cloudwatch-logs.png)

You have successfully verified that the backend APIs are working correctly. Proceed to **Section 5.4** to connect the Flutter application to the deployed backend and run the complete system.