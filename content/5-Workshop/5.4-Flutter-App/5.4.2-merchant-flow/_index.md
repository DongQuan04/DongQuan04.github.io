---
title: "Testing the Merchant Workflow"
date: 2026-06-17
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

#### Register a New Merchant

1. From the role selection screen, choose **I'm a Merchant**.
2. Since this is the first launch (no `merchant_id` is stored locally), the app navigates to the **MerchantOnboardingScreen**.
3. Enter the **Merchant Name**, for example, `Phở Thìn Bờ Hồ`. The **Merchant ID** is generated automatically by slugifying the merchant name (removing Vietnamese accents and replacing spaces with hyphens), resulting in `pho-thin-bo-ho`.

![Merchant onboarding](/images/5-Workshop/5.4-flutter/merchant-onboarding.png)

4. Tap **Start Selling 🚀**. The `merchant_id` and `merchant_name` are saved to `SharedPreferences`, and the app navigates to the **MerchantHomeScreen**.

{{% notice info %}}
If you already created menu items using the cURL example in **Section 5.3.2** with `merchant_id = pho-thin-bo-ho`, use the same merchant name here so the existing menu items appear immediately in the app.
{{% /notice %}}

#### Add Menu Items

1. From the **MerchantHomeScreen**, open the menu management page.
2. Add a few menu items by entering the item name, price, and category. Each new item triggers `MerchantProvider.addMenuItem()`, which sends a `POST /menu` request with `action: "ADD"`.

![Add menu items](/images/5-Workshop/5.4-flutter/merchant-menu.png)

3. Open the **DynamoDB Console** and inspect the `Merchants` table. Verify that the menu list has been updated in the item corresponding to your `merchant_id`.

![Verify DynamoDB](/images/5-Workshop/5.4-flutter/dynamodb-menu-check.png)

#### Generate a Table QR Code

1. Billo uses a custom QR code format for each table:

```
BILLO_TABLE|merchant_id|merchant_name|table_number
```

2. Generate a sample QR code using any online QR code generator with the following content:

```
BILLO_TABLE|pho-thin-bo-ho|Phở Thìn Bờ Hồ|Ban-01
```

![Generate a sample table QR code](/images/5-Workshop/5.4-flutter/qr-table-generate.png)

You now have a merchant with a configured menu and a table QR code ready for use. In the next section (**5.4.3**), you will switch to the **Customer** role to scan the QR code, place an order, and complete the payment workflow.