---
title: "Deploying the Backend with AWS SAM"
date: 2026-06-16
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

#### Review the `template.yaml` File

The Billo backend is fully defined in the `template.yaml` file using the AWS SAM specification. It includes:

+ 5 **DynamoDB** tables: `Merchants`, `Orders`, `Invoices`, `Wallets`, and `Shifts`.
+ 1 **Cognito User Pool** (`BilloUserPool`) for phone number + OTP authentication.
+ 7 **Lambda functions**, each associated with one or more API Gateway routes.
+ 1 **Lambda Layer** (`CommonLayer`) shared across all Lambda functions.

![SAM Template](/images/5-Workshop/5.3-backend/template-yaml.png)

#### Build the Stack

1. Open a terminal in the project root directory containing the `template.yaml` file, then run:

```bash
sam build
```

![SAM build](/images/5-Workshop/5.3-backend/sam-build.png)

{{% notice info %}}
`sam build` packages each Lambda function together with its dependencies (such as the Python libraries listed in `requirements.txt`) into deployment-ready artifacts.
{{% /notice %}}

2. After the build completes successfully, deploy the stack using guided mode for the first deployment:

```bash
sam deploy --guided
```

3. During the guided deployment, provide the following information:

+ **Stack Name**: `billo-backend`
+ **AWS Region**: Select the AWS Region where you want to deploy (for example, `ap-southeast-1`)
+ **Confirm changes before deploy**: `Y`
+ **Allow SAM CLI IAM role creation**: `Y`
+ **Save arguments to configuration file**: `Y`

![SAM deploy guided](/images/5-Workshop/5.3-backend/sam-deploy.png)

4. When prompted with `Deploy this changeset?`, type `Y` and press **Enter**.

![Deployment successful](/images/5-Workshop/5.3-backend/deploy-success.png)

#### Retrieve the Stack Outputs

After the deployment finishes, AWS SAM displays the **Outputs** section, which includes:

+ `ApiUrl` — The base URL of the API Gateway. This will be used to configure the Flutter application in Section **5.4**.
+ `UserPoolId` and `UserPoolClientId` — The Cognito User Pool configuration values.

![Stack outputs](/images/5-Workshop/5.3-backend/stack-outputs.png)

{{% notice info %}}
Save the `ApiUrl` value. You will need to set it as the value of `ApiConfig.baseUrl` in the Flutter application source code in Section **5.4**.
{{% /notice %}}

You have successfully deployed the entire serverless backend. In the next section (**5.3.2**), you will verify the newly created APIs using Postman or cURL before connecting them to the Flutter application.