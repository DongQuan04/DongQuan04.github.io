---
title : "Dọn dẹp tài nguyên"
date : 2026-06-08
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

#### Dọn dẹp tài nguyên

Xin chúc mừng bạn đã hoàn thành xong workshop này! Trong workshop này, bạn đã triển khai toàn bộ backend serverless (Lambda, API Gateway, DynamoDB, Cognito) bằng AWS SAM và kết nối thành công với ứng dụng Flutter đa vai trò Billo.

+ Vì toàn bộ hạ tầng được định nghĩa bằng SAM/CloudFormation, việc dọn dẹp có thể thực hiện gọn bằng một lệnh duy nhất, xóa sạch stack cùng mọi resource liên quan (Lambda, API Gateway, DynamoDB, Cognito, IAM Role).

#### Xóa stack backend

1. Tại thư mục gốc chứa `template.yaml`, chạy lệnh:

```bash
sam delete --stack-name billo-backend
```

![sam delete](/images/5-Workshop/5.6-don-dep/sam-delete.png)

2. Xác nhận xóa bằng cách gõ `y` khi được hỏi:

```
Are you sure you want to delete the stack billo-backend in the region ap-southeast-1 ? [y/N]:
```

3. Đợi cho đến khi thấy thông báo `Deleted successfully`.

![Xóa thành công](/images/5-Workshop/5.6-don-dep/delete-success.png)

{{% notice warning %}}
Thao tác `sam delete` sẽ xóa vĩnh viễn toàn bộ dữ liệu trong các bảng DynamoDB (`Merchants`, `Orders`, `Invoices`, `Wallets`, `Shifts`). Nếu cần giữ lại dữ liệu demo, hãy export dữ liệu trước khi xóa.
{{% /notice %}}

#### Kiểm tra lại trên Console

1. Vào **CloudFormation console**, xác nhận stack `billo-backend` không còn xuất hiện trong danh sách.

![CloudFormation trống](/images/5-Workshop/5.6-don-dep/cfn-empty.png)

2. Vào **DynamoDB console**, **Lambda console**, **API Gateway console** để xác nhận không còn resource nào sót lại liên quan đến Billo.

3. Vào **Cognito console**, xác nhận `billo-user-pool` đã bị xóa.

![Kiểm tra tài nguyên còn sót](/images/5-Workshop/5.6-don-dep/final-check.png)

#### Gỡ ứng dụng khỏi thiết bị (tùy chọn)

Nếu bạn đã cài file APK/iOS lên thiết bị thật để demo, có thể gỡ cài đặt app Billo khỏi thiết bị sau khi hoàn tất workshop.

---

Đến đây, bạn đã hoàn thành toàn bộ workshop triển khai Billo — từ hạ tầng serverless trên AWS đến ứng dụng Flutter đa vai trò, và dọn dẹp sạch sẽ mọi tài nguyên đã tạo ra.