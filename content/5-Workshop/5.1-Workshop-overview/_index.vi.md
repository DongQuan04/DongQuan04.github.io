---
title : "Giới thiệu"
date : 2026-06-16
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Giới thiệu Workshop

Trong chương này, bạn sẽ đi qua toàn bộ quá trình triển khai **Billo** — ứng dụng gọi món và thanh toán không tiền mặt tại quán ăn/quán cà phê, được xây dựng trong đợt thực tập, gồm 2 phần:

+ **Backend**: kiến trúc serverless trên AWS (Lambda + API Gateway + DynamoDB + Cognito), triển khai bằng AWS SAM.
+ **Frontend**: ứng dụng di động Flutter, phục vụ đồng thời 2 vai trò trên cùng một app — **Khách hàng** (quét QR, gọi món, thanh toán) và **Chủ quán** (quản lý menu, nhận đơn, xuất hóa đơn).

![Kiến trúc tổng thể](/images/5-Workshop/5.1-gioi-thieu/kien-truc-tong-the.png)

#### Bạn sẽ học được gì

+ Cách định nghĩa hạ tầng serverless bằng **AWS SAM** (Lambda, API Gateway, DynamoDB, Cognito) dưới dạng Infrastructure as Code.
+ Cách tách nghiệp vụ backend thành các Lambda function độc lập theo nguyên tắc single-responsibility.
+ Cách kết nối một ứng dụng Flutter đa vai trò (multi-mode) với REST API backend.
+ Cách kiểm thử luồng nghiệp vụ end-to-end: từ quét QR bàn → gọi món → tạo hóa đơn → thanh toán.

{{% notice info %}}
Workshop này giả định bạn đã có kiến thức cơ bản về AWS (Lambda, API Gateway, DynamoDB, IAM) và Flutter. Nếu chưa quen, bạn có thể tham khảo tài liệu chính thức của [AWS SAM](https://docs.aws.amazon.com/serverless-application-model/) và [Flutter](https://docs.flutter.dev/) trước khi bắt đầu.
{{% /notice %}}

#### Kiến trúc chi tiết

+ **Amazon Cognito User Pool**: xác thực người dùng qua số điện thoại, xác minh OTP gửi qua SNS.
+ **Amazon API Gateway**: expose các endpoint REST (`/auth`, `/menu`, `/orders`, `/invoices`, `/payments`, `/wallet`, `/shifts`).
+ **AWS Lambda (Python 3.11)**: 7 function xử lý từng nghiệp vụ riêng biệt, dùng chung 1 Lambda Layer để tái sử dụng code.
+ **Amazon DynamoDB**: 5 bảng (Merchants, Orders, Invoices, Wallets, Shifts) theo mô hình PAY_PER_REQUEST.
+ **Flutter App**: chia 3 vùng code chính — `customer/`, `merchant/`, `shared/` (design system, model, provider quản lý mode).

Sau khi hoàn thành workshop, bạn sẽ có một backend chạy thật trên AWS và một app Flutter kết nối đầy đủ, có thể demo luồng gọi món – thanh toán hoàn chỉnh.