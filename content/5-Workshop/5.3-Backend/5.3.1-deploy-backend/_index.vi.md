---
title : "Triển khai Backend bằng AWS SAM"
date : 2026-06-16
weight : 1
chapter : false
pre : " <b> 5.3.1. </b> "
---

#### Kiểm tra template.yaml

Backend của Billo được định nghĩa toàn bộ trong file `template.yaml` theo chuẩn AWS SAM, gồm:

+ 5 bảng **DynamoDB**: `Merchants`, `Orders`, `Invoices`, `Wallets`, `Shifts`.
+ 1 **Cognito User Pool** (`BilloUserPool`) xác thực qua số điện thoại + OTP.
+ 7 **Lambda function**, mỗi function gắn với một hoặc nhiều route trên API Gateway.
+ 1 **Lambda Layer** (`CommonLayer`) dùng chung cho tất cả function.

![Template SAM](/images/5-Workshop/5.3-backend/template-yaml.png)

#### Build stack

1. Mở terminal tại thư mục gốc chứa `template.yaml`, chạy lệnh build:

```bash
sam build
```

![SAM build](/images/5-Workshop/5.3-backend/sam-build.png)

{{% notice info %}}
`sam build` sẽ đóng gói từng Lambda function cùng dependency của nó (ví dụ các thư viện Python trong `requirements.txt`) thành artifact sẵn sàng để deploy.
{{% /notice %}}

2. Sau khi build thành công, chạy lệnh deploy với chế độ hướng dẫn (guided) cho lần đầu tiên:

```bash
sam deploy --guided
```

3. Trong quá trình guided deploy, điền các thông tin:

+ **Stack Name**: `billo-backend`
+ **AWS Region**: chọn region bạn muốn triển khai (ví dụ `ap-southeast-1`)
+ **Confirm changes before deploy**: `Y`
+ **Allow SAM CLI IAM role creation**: `Y`
+ **Save arguments to configuration file**: `Y`

![SAM deploy guided](/images/5-Workshop/5.3-backend/sam-deploy.png)

4. Xác nhận `Deploy this changeset?` bằng cách gõ `Y` và nhấn Enter.

![Deploy thành công](/images/5-Workshop/5.3-backend/deploy-success.png)

#### Lấy thông tin output

Sau khi deploy hoàn tất, SAM sẽ in ra phần **Outputs** gồm:

+ `ApiUrl` — URL gốc của API Gateway, sẽ dùng để cấu hình cho app Flutter ở bước 5.4.
+ `UserPoolId` và `UserPoolClientId` — thông tin Cognito User Pool.

![Stack outputs](/images/5-Workshop/5.3-backend/stack-outputs.png)

{{% notice info %}}
Ghi lại giá trị `ApiUrl` — bạn sẽ cần điền vào biến `ApiConfig.baseUrl` trong source Flutter app ở mục 5.4.
{{% /notice %}}

Bạn đã triển khai thành công toàn bộ backend serverless. Ở bước tiếp theo (5.3.2), chúng ta sẽ kiểm tra các API vừa tạo bằng Postman/cURL trước khi kết nối với Flutter app.