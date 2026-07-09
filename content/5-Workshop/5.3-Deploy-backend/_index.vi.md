---
title: "Deploy backend AWS BILLO"
date: 2026-06-29
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

---


Phần này hướng dẫn build và deploy backend AWS BILLO bằng AWS SAM và AWS CloudFormation.

Quá trình deploy backend sẽ tạo các tài nguyên serverless chính trên AWS, bao gồm Cognito, API Gateway, Lambda, DynamoDB, S3, CloudWatch Logs và các quyền IAM liên quan.

---

## Bước 1: Mở thư mục backend

Mở PowerShell và chuyển đến thư mục backend:

```powershell
cd C:\Users\admin\source\repos\AWS_BILLO\backend
```

Kiểm tra các file chính của backend:

```powershell
dir
```

Đảm bảo thư mục có các file và thư mục sau:

```text
template.yaml
samconfig.toml
package.json
src
tests
```

---

## Bước 2: Cài dependencies backend

Cài các thư viện Node.js:

```powershell
npm install
```

Lệnh này cài các package cần thiết cho Lambda functions và backend tests.

---

## Bước 3: Kiểm tra SAM template

Mở file sau trong Visual Studio Code:

```text
backend/template.yaml
```

File này định nghĩa các tài nguyên AWS được sử dụng bởi backend, chẳng hạn như:

- Cognito User Pool và App Client
- Cognito User Pool Groups
- API Gateway
- Lambda functions
- DynamoDB Main Table
- DynamoDB Idempotency Table
- S3 Upload Bucket
- CloudWatch Logs
- IAM roles và permissions

Backend được quản lý dưới dạng Infrastructure as Code thông qua AWS SAM và CloudFormation.

---

## Bước 4: Build backend

Chạy lệnh:

```powershell
sam build
```

Lệnh này build Lambda functions và chuẩn bị artifact để deploy.

Nếu build cache gây lỗi, chạy:

```powershell
sam build --no-cached
```

Kết quả mong đợi:

```text
Build Succeeded
```

---

## Bước 5: Deploy backend

Deploy backend bằng cấu hình SAM hiện có:

```powershell
sam deploy
```

Nếu muốn deploy nhanh và hạn chế hỏi xác nhận nhiều lần, chạy:

```powershell
sam deploy --no-confirm-changeset --no-fail-on-empty-changeset
```

Thông tin stack mong đợi:

```text
Stack name: wallet-app-backend-dev
Region: ap-southeast-1
```

SAM sẽ deploy backend thông qua AWS CloudFormation.

---

## Bước 6: Kiểm tra CloudFormation Stack

Sau khi deploy, mở AWS Management Console và vào:

```text
CloudFormation > Stacks
```

Tìm stack:

```text
wallet-app-backend-dev
```

Kiểm tra trạng thái stack là:

```text
CREATE_COMPLETE
```

hoặc:

```text
UPDATE_COMPLETE
```

![img.png](img.png)
Nếu stack bị lỗi, mở tab **Events** để xem tài nguyên nào deploy thất bại.

---

## Bước 7: Kiểm tra API Gateway endpoint

Backend API endpoint của môi trường development là:
![img_1.png](img_1.png)

```text
https://zsqkp5vpb9.execute-api.ap-southeast-1.amazonaws.com/dev
```

Endpoint này được Flutter frontend và Admin Web sử dụng để gọi backend API.

Endpoint cũng cần được cấu hình trong:

```text
frontend/config/dev.json
```

---

## Bước 8: Kiểm tra DynamoDB Tables

Mở AWS Management Console và vào:

```text
DynamoDB > Tables
```
![img_2.png](img_2.png)

Kiểm tra main table:

```text
wallet-app-main-dev
```

Main table lưu các dữ liệu như:

- User profiles
- Wallets
- Merchant applications
- Stores
- Products hoặc services
- Tables
- Orders
- Bills
- Payment sessions
- Transactions

Dự án cũng sử dụng thiết kế idempotency bằng DynamoDB để ngăn xử lý trùng khi chuyển tiền hoặc thanh toán.

---

## Bước 9: Kiểm tra S3 Bucket

Mở:

```text
Amazon S3 > Buckets
```
![img_3.png](img_3.png)

Kiểm tra upload bucket được tạo bởi backend stack.

S3 bucket được dùng để lưu:

- Tài liệu đăng ký kinh doanh
- Hình ảnh cửa hàng
- Hình ảnh sản phẩm hoặc dịch vụ

File được upload thông qua pre-signed URL do backend tạo.

---

## Bước 10: Kiểm tra Cognito User Pool

Mở:

```text
Amazon Cognito > User pools
```
![img_4.png](img_4.png)


Kiểm tra User Pool ID:

```text
ap-southeast-1_AKc39KB4L
```

![img_5.png](img_5.png)


User Pool xử lý:

- Đăng ký bằng số điện thoại
- Xác nhận OTP
- Đăng nhập
- Cấp JWT token
- User group cho Customer, Merchant và Admin

Lưu ý quan trọng: SMS OTP phụ thuộc vào AWS SMS sandbox hoặc production SMS configuration. Nếu đang ở sandbox mode, chỉ các số điện thoại đã xác minh mới có thể nhận OTP.

---

## Bước 11: Kiểm tra Lambda và CloudWatch Logs

Mở:

```text
AWS Lambda > Functions
```
![img_6.png](img_6.png)


Kiểm tra các Lambda functions của backend đã được tạo thành công.

Sau đó mở:

```text
CloudWatch > Log groups
```
![img_8.png](img_8.png)


Kiểm tra log groups của Lambda functions. Các log này hữu ích khi debug lỗi API, lỗi xác thực, lỗi thanh toán và lỗi validate dữ liệu.

---

## Các lỗi deploy thường gặp

### AWS credentials chưa được cấu hình

Chạy:

```powershell
aws configure
```

Sau đó kiểm tra:

```powershell
aws sts get-caller-identity
```

### SAM build bị lỗi

Thử chạy:

```powershell
sam build --no-cached
```

Đồng thời kiểm tra phiên bản Node.js:

```powershell
node -v
```

### CloudFormation stack bị rollback

Mở:

```text
CloudFormation > Stacks > wallet-app-backend-dev > Events
```
![img_9.png](img_9.png)

![img_10.png](img_10.png)

Đọc event bị lỗi và sửa tài nguyên hoặc quyền liên quan.

### Không nhận được OTP

Nếu Cognito SMS vẫn đang ở sandbox, hãy dùng số điện thoại đã xác minh hoặc cấu hình quyền production SMS.

---

## Kết quả mong đợi

Sau khi hoàn thành phần này:

- Backend stack được deploy thành công.
- Cognito, API Gateway, Lambda, DynamoDB, S3 và CloudWatch Logs đã sẵn sàng.
- API Gateway endpoint có thể được Flutter app và Admin Web sử dụng.
- Dự án đã sẵn sàng cho phần cấu hình xác thực và kiểm thử ứng dụng.

