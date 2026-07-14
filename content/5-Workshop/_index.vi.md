---
title: "Workshop"
date: 2026-06-29
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

---

# AWS BILLO Workshop

## Triển khai backend serverless và chạy demo ứng dụng

Workshop này giới thiệu cách triển khai và kiểm thử dự án **AWS BILLO**, một hệ thống ví điện tử và quản lý cửa tiệm được xây dựng theo kiến trúc serverless trên AWS.

AWS BILLO kết hợp ví nội bộ dạng mô phỏng, chức năng POS cho Merchant, gọi món bằng QR tại bàn, thanh toán QR, quản lý cửa hàng và quy trình Admin duyệt hồ sơ Merchant. Hệ thống được thiết kế cho ba vai trò chính: **Customer**, **Merchant** và **Admin**.

Mục tiêu chính của workshop là hướng dẫn triển khai backend bằng các dịch vụ serverless của AWS và chạy demo frontend ở môi trường local.

---

## Phạm vi workshop

Workshop này chỉ tập trung vào các dịch vụ và công cụ AWS đang được sử dụng thực tế trong dự án AWS BILLO:

- **Amazon Cognito** để xác thực người dùng, xác nhận OTP, cấp JWT token và quản lý user group.
- **Amazon API Gateway** để cung cấp các backend API được bảo vệ.
- **AWS Lambda** để xử lý logic nghiệp vụ backend.
- **Amazon DynamoDB** để lưu trữ dữ liệu ứng dụng.
- **DynamoDB Idempotency Table** để tránh xử lý trùng khi chuyển tiền hoặc thanh toán.
- **Amazon S3** để lưu hình ảnh và tài liệu đăng ký kinh doanh.
- **Amazon CloudWatch Logs** để theo dõi và debug lỗi Lambda/API.
- **AWS SAM** để build và deploy backend serverless.
- **AWS CloudFormation** để quản lý tài nguyên AWS thông qua stack.

Frontend Flutter và Admin Web hiện đang chạy local trong giai đoạn phát triển. Workshop này chưa triển khai hosting bằng AWS Amplify, Amazon S3 hoặc Amazon CloudFront.

---

## Kiến trúc hệ thống

AWS BILLO sử dụng kiến trúc serverless được triển khai tại AWS Singapore Region: `ap-southeast-1`.

Frontend giao tiếp với Amazon Cognito để xác thực và Amazon API Gateway để gọi các nghiệp vụ backend. API Gateway kiểm tra JWT token từ Cognito trước khi chuyển request đến AWS Lambda. Lambda xử lý logic nghiệp vụ chính và lưu dữ liệu vào DynamoDB. S3 được dùng để lưu tài liệu và hình ảnh upload, còn CloudWatch Logs được dùng để theo dõi và xử lý lỗi.

![AWS BILLO System Architecture](image-1.png)

---

## Các phần trong workshop

### [5.1 - Tổng quan workshop](5.1-Workshop-overview/)

Phần này giới thiệu dự án AWS BILLO, mục tiêu workshop, kiến trúc hệ thống và các dịch vụ AWS chính được sử dụng.

### [5.2 - Chuẩn bị môi trường](5.2-Prerequisites/)

Phần này liệt kê các công cụ, tài khoản, quyền truy cập và môi trường local cần chuẩn bị trước khi deploy backend và chạy ứng dụng.

### [5.3 - Deploy backend AWS BILLO](5.3-Deploy-backend/)

Phần này hướng dẫn build và deploy backend bằng AWS SAM và CloudFormation.

### [5.4 - Cấu hình xác thực](5.4-Configure-authentication/)

Phần này giải thích cách Amazon Cognito được sử dụng cho đăng ký bằng số điện thoại, xác nhận OTP, đăng nhập, JWT token và user group.

### [5.5 - Chạy demo Flutter Frontend](5.5-Run-frontend-demo/)

Phần này hướng dẫn chạy ứng dụng Flutter ở local và kết nối với backend AWS đã deploy.

### [5.6 - Chạy demo Admin Web](5.6-Run-admin-web-demo/)

Phần này hướng dẫn chạy Admin Web ở local để xem xét và duyệt hồ sơ đăng ký Merchant.

### [5.7 - Kiểm thử các luồng chính](5.7-Test-main-business-flows/)

Phần này hướng dẫn demo các luồng chính như đăng ký Customer, duyệt Merchant, tạo sản phẩm, tạo QR bàn, gọi món tại bàn, thanh toán QR và xem lịch sử giao dịch.

### [5.8 - Monitoring và cleanup](5.8-Monitoring-and-cleanup/)

Phần này giải thích cách xem log Lambda trong CloudWatch và dọn dẹp tài nguyên AWS sau khi hoàn thành workshop.

### [5.9 - Giới thiệu tổng quan dự án](5.9-Project-overview/)

Phần này trình bày các chức năng chính của AWS BILLO theo từng vai trò
(Customer, Merchant, Admin), kèm ảnh chụp màn hình minh họa và bước thao tác
chi tiết cho từng chức năng. Xem chi tiết tại 5.9.1, 5.9.2 và 5.9.3.

---

## Kết quả mong đợi

Sau khi hoàn thành workshop này, người thực hiện có thể:

- Hiểu kiến trúc serverless của AWS BILLO.
- Deploy backend bằng AWS SAM.
- Sử dụng Amazon Cognito cho xác thực và phân quyền theo group.
- Kết nối Flutter frontend với API Gateway endpoint đã deploy.
- Chạy Admin Web ở local.
- Kiểm thử các luồng chính của Customer, Merchant và Admin.
- Xem log backend bằng CloudWatch Logs.
- Dọn dẹp tài nguyên AWS sau khi kiểm thử.

---

## Lưu ý quan trọng

Workshop này được thiết kế cho môi trường phát triển và demo.

Hệ thống không phải là nền tảng tài chính production. Số dư ví và giao dịch chỉ được sử dụng cho mục đích minh họa.

Cognito SMS OTP phụ thuộc vào cấu hình AWS SMS sandbox hoặc production SMS. Trong giai đoạn phát triển, chỉ các số điện thoại đã được xác minh mới có thể nhận OTP.

Frontend và Admin Web hiện đang chạy local. Việc hosting bằng AWS Amplify, Amazon S3 hoặc Amazon CloudFront chưa nằm trong phạm vi workshop này.

