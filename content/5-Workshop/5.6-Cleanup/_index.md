---
title: "Cleaning Up Resources"
date: 2026-06-08
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

#### Clean Up AWS Resources

Congratulations on completing this workshop!

Throughout this workshop, you deployed a complete serverless backend using AWS SAM—including AWS Lambda, API Gateway, DynamoDB, and Amazon Cognito—and successfully connected it to the multi-role Billo Flutter application.

Because the entire infrastructure is defined using AWS SAM and AWS CloudFormation, you can remove all provisioned resources with a single command. This deletes the CloudFormation stack along with all associated resources, including Lambda functions, API Gateway, DynamoDB tables, Cognito resources, and IAM roles.

#### Delete the Backend Stack

1. In the project root directory containing the `template.yaml` file, run:

```bash
sam delete --stack-name billo-backend
```

![sam delete](/images/5-Workshop/5.6-don-dep/sam-delete.png)

2. Confirm the deletion by typing `y` when prompted:

```text
Are you sure you want to delete the stack billo-backend in the region ap-southeast-1? [y/N]:
```

3. Wait until the CLI displays the message `Deleted successfully`.

![Deletion successful](/images/5-Workshop/5.6-don-dep/delete-success.png)

{{% notice warning %}}
Running `sam delete` permanently removes all data stored in the DynamoDB tables (`Merchants`, `Orders`, `Invoices`, `Wallets`, and `Shifts`). If you want to preserve your demo data, export it before deleting the stack.
{{% /notice %}}

#### Verify Resource Removal in the AWS Console

1. Open the **CloudFormation Console** and verify that the `billo-backend` stack no longer appears in the list.

![Empty CloudFormation console](/images/5-Workshop/5.6-don-dep/cfn-empty.png)

2. Open the **DynamoDB**, **Lambda**, and **API Gateway** consoles to confirm that no Billo-related resources remain.

3. Open the **Amazon Cognito Console** and verify that the `billo-user-pool` has been deleted.

![Verify remaining resources](/images/5-Workshop/5.6-don-dep/final-check.png)

#### Remove the Application from Your Device (Optional)

If you installed the Android APK or iOS build on a physical device for demonstration purposes, you may uninstall the Billo application after completing the workshop.

---

You have now completed the entire Billo workshop—from deploying a serverless backend on AWS, connecting it to a multi-role Flutter application, validating the complete end-to-end workflow, and finally cleaning up all the AWS resources created during the process.