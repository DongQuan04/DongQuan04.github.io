---
title : "Các bước chuẩn bị"
date : 2026-06-16
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

#### Công cụ cần cài đặt

Trước khi bắt đầu, hãy cài đặt các công cụ sau trên máy của bạn:

1. **AWS CLI** và **AWS SAM CLI** — dùng để build và deploy stack backend.
2. **Python 3.11** — runtime cho toàn bộ Lambda function.
3. **Flutter SDK** (kèm Android Studio hoặc Xcode) — để build và chạy app trên thiết bị/giả lập.
4. **Docker** — SAM CLI cần Docker để build Lambda package theo đúng môi trường runtime của AWS.

![Cài đặt công cụ](/images/5-Workshop/5.2-chuan-bi/tools.png)

#### IAM permissions

Gắn IAM permission policy sau vào tài khoản AWS user của bạn để có thể triển khai và dọn dẹp tài nguyên trong workshop này:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "cloudformation:*",
                "cloudwatch:*",
                "lambda:*",
                "apigateway:*",
                "dynamodb:*",
                "cognito-idp:*",
                "iam:CreateRole",
                "iam:DeleteRole",
                "iam:AttachRolePolicy",
                "iam:DetachRolePolicy",
                "iam:PutRolePolicy",
                "iam:DeleteRolePolicy",
                "iam:PassRole",
                "iam:GetRole",
                "sns:Publish",
                "s3:CreateBucket",
                "s3:PutObject",
                "s3:GetObject"
            ],
            "Resource": "*"
        }
    ]
}
```

{{% notice warning %}}
Policy trên được nới rộng cho mục đích workshop/học tập. Trong môi trường production, hãy giới hạn phạm vi resource theo nguyên tắc least-privilege thay vì dùng `"Resource": "*"`.
{{% /notice %}}

#### Clone source code

1. Clone repository chứa source backend (SAM template) và source Flutter app.
2. Cấu trúc thư mục backend:

```
functions/
 ├── auth/handler.py
 ├── manage_menu/handler.py
 ├── manage_orders/handler.py
 ├── create_invoice/handler.py
 ├── process_payment/handler.py
 ├── manage_wallet/handler.py
 └── manage_shifts/handler.py
layers/common/
template.yaml
```

3. Cấu trúc thư mục Flutter app:

```
lib/
 ├── customer/         # Màn hình & logic cho Khách hàng
 ├── merchant/          # Màn hình & logic cho Chủ quán
 ├── shared/            # Design system, model, provider quản lý mode
 └── main.dart
```

Sau khi chuẩn bị xong công cụ và source code, chuyển sang mục **5.3** để triển khai backend.