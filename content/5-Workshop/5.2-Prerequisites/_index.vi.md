---
title: "Chuẩn bị môi trường"
date: 2026-06-29
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

---

Trước khi bắt đầu workshop, cần chuẩn bị tài khoản AWS, công cụ local, source code và cấu hình dự án.

---

## Yêu cầu tài khoản AWS

Cần có tài khoản AWS với quyền tạo và quản lý các tài nguyên sau:

- Amazon Cognito User Pool
- Amazon API Gateway
- AWS Lambda
- Amazon DynamoDB
- Amazon S3
- Amazon CloudWatch Logs
- AWS CloudFormation stacks
- IAM roles và policies được tạo bởi AWS SAM

Dự án được deploy tại AWS Region:

```text
ap-southeast-1
```
![img.png](img.png)


Đây là region Singapore.

---

## Công cụ cần cài đặt

Cài đặt các công cụ sau trên máy tính:

| Công cụ            | Mục đích                                             |
| ------------------ | ---------------------------------------------------- |
| AWS CLI            | Cấu hình AWS credentials và thao tác với dịch vụ AWS |
| AWS SAM CLI        | Build và deploy backend serverless                   |
| Node.js 22.x       | Chạy và đóng gói code Lambda backend                 |
| npm                | Cài dependencies cho backend và Admin Web            |
| Flutter SDK        | Chạy Flutter frontend                                |
| Git                | Clone và quản lý source code                         |
| Visual Studio Code | Chỉnh sửa và quản lý file dự án                      |

---

## Kiểm tra AWS CLI

Mở PowerShell và chạy:

```powershell
aws --version
```

Sau đó cấu hình AWS credentials:

```powershell
aws configure
```

Nhập các thông tin cần thiết:

```text
AWS Access Key ID
AWS Secret Access Key
Default region name: ap-southeast-1
Default output format: json
```

Kiểm tra danh tính AWS hiện tại:

```powershell
aws sts get-caller-identity
```

Nếu lệnh trả về AWS account ID và user ARN, AWS CLI đã được cấu hình thành công.

---

## Kiểm tra AWS SAM CLI

Chạy lệnh:

```powershell
sam --version
```

Nếu SAM CLI được cài đúng, terminal sẽ hiển thị phiên bản đã cài.

---

## Kiểm tra Node.js và npm

Chạy:

```powershell
node -v
npm -v
```

Backend sử dụng Node.js 22.x cho Lambda functions.

---

## Kiểm tra Flutter

Chạy:

```powershell
flutter doctor
```

Đảm bảo Flutter đã được cài đặt và Chrome có thể dùng để test web.

Sau đó kiểm tra thiết bị khả dụng:

```powershell
flutter devices
```

Chrome nên xuất hiện trong danh sách thiết bị.

---

## Source code dự án

Source code dự án nên được đặt tại thư mục:

```text
C:\Users\admin\source\repos\AWS_BILLO
```

Các thư mục chính gồm:

```text
AWS_BILLO/
├── backend/
├── frontend/
└── admin-web/
```

---

## File cấu hình backend

Thư mục backend nên có:

```text
backend/
├── template.yaml
├── samconfig.toml
├── package.json
├── src/functions/
├── src/shared/
└── tests/
```

Các file quan trọng:

| File             | Mục đích                                     |
| ---------------- | -------------------------------------------- |
| `template.yaml`  | Định nghĩa tài nguyên AWS để deploy bằng SAM |
| `samconfig.toml` | Lưu cấu hình deploy                          |
| `package.json`   | Lưu dependencies và scripts backend          |
| `src/functions/` | Chứa các Lambda functions                    |
| `src/shared/`    | Chứa code dùng chung cho backend             |

---

## Cấu hình frontend

Flutter app sử dụng file cấu hình để kết nối với backend AWS đã deploy:

```text
frontend/config/dev.json
```

File này nên chứa các giá trị như:

```json
{
  "API_BASE_URL": "https://zsqkp5vpb9.execute-api.ap-southeast-1.amazonaws.com/dev",
  "AWS_REGION": "ap-southeast-1",
  "COGNITO_USER_POOL_ID": "ap-southeast-1_AKc39KB4L"
}
```

Tên key thực tế có thể phụ thuộc vào cách dự án được triển khai. Cần đảm bảo các giá trị trong file khớp với backend AWS đã deploy.

---

## Yêu cầu cho Admin Web

Admin Web nằm trong thư mục:

```text
admin-web/
```

Cài dependencies trước khi chạy:

```powershell
cd C:\Users\admin\source\repos\AWS_BILLO\admin-web
npm install
```

Admin Web được sử dụng để xem xét và duyệt hồ sơ đăng ký Merchant.

---

## Lưu ý quan trọng

Cognito OTP SMS phụ thuộc vào cấu hình SMS của AWS. Nếu tài khoản AWS vẫn đang ở SMS sandbox, chỉ các số điện thoại đã xác minh mới có thể nhận OTP.

Frontend và Admin Web hiện đang chạy local. Workshop này không bao gồm AWS Amplify, Amazon S3 static hosting hoặc Amazon CloudFront.

Trước khi thực hiện workshop, cần đảm bảo tài khoản AWS có đủ quyền và nên cấu hình AWS Budget alerts để tránh phát sinh chi phí ngoài ý muốn.

