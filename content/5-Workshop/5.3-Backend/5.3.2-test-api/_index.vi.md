---
title : "Kiểm tra API Backend"
date : 2026-06-17
weight : 2
chapter : false
pre : " <b> 5.3.2. </b> "
---

#### Kiểm tra bảng DynamoDB

1. Điều hướng đến **DynamoDB console** > **Tables**.
2. Xác nhận 5 bảng đã được tạo: `Merchants`, `Orders`, `Invoices`, `Wallets`, `Shifts`.

![DynamoDB tables](/images/5-Workshop/5.3-backend/dynamodb-tables.png)

#### Kiểm tra Cognito User Pool

1. Điều hướng đến **Amazon Cognito console** > **User pools**.
2. Mở `billo-user-pool`, xác nhận **Sign-in options** đã bật số điện thoại (phone number) và cấu hình SNS gửi OTP.

![Cognito user pool](/images/5-Workshop/5.3-backend/cognito-pool.png)

#### Gọi thử API bằng cURL

1. Lấy `ApiUrl` đã ghi lại từ bước trước, thử gọi endpoint tạo menu cho một quán test:

```bash
curl -X POST "$API_URL/menu" \
  -H "Content-Type: application/json" \
  -H "x-merchant-id: pho-thin-bo-ho" \
  -d '{
    "merchant_id": "pho-thin-bo-ho",
    "action": "ADD",
    "item": { "item_id": "1", "name": "Phở bò", "price": 45000, "category": "Món chính" }
  }'
```

![Gọi thử API tạo menu](/images/5-Workshop/5.3-backend/curl-menu.png)

2. Kiểm tra lại menu vừa tạo bằng endpoint GET:

```bash
curl "$API_URL/menu/pho-thin-bo-ho"
```

![Kết quả GET menu](/images/5-Workshop/5.3-backend/curl-get-menu.png)

{{% notice info %}}
Nếu nhận response `200 OK` kèm dữ liệu món ăn vừa thêm, nghĩa là chuỗi API Gateway → Lambda `ManageMenuFunction` → DynamoDB `Merchants` table đã hoạt động đúng.
{{% /notice %}}

#### Kiểm tra log CloudWatch

1. Điều hướng đến **CloudWatch console** > **Log groups**.
2. Mở log group `/aws/lambda/billo-manage-menu`, xác nhận request vừa gọi được ghi log không lỗi.

![CloudWatch logs](/images/5-Workshop/5.3-backend/cloudwatch-logs.png)

Bạn đã xác minh backend hoạt động đúng ở tầng API. Chuyển sang mục **5.4** để kết nối và chạy thử ứng dụng Flutter với backend vừa triển khai.