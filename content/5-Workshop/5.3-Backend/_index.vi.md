---
title: "Workshop"
date: 2026-06-17
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

# Triển khai Billo - Ứng dụng gọi món & thanh toán qua QR trên nền tảng Flutter và AWS Serverless

#### Tổng quan

**Billo** là ứng dụng di động phục vụ mô hình gọi món và thanh toán không tiền mặt tại quán ăn/quán cà phê thông qua mã QR, được xây dựng trong đợt thực tập.

Trong workshop này, chúng ta sẽ triển khai toàn bộ hệ thống gồm 2 phần: backend serverless trên AWS (Lambda, API Gateway, DynamoDB, Cognito) và ứng dụng di động Flutter phục vụ đồng thời 2 vai trò trên cùng một app — **Khách hàng** và **Chủ quán**.

+ **Backend** - Toàn bộ hạ tầng được định nghĩa bằng AWS SAM (Infrastructure as Code), gồm 7 Lambda function tách theo từng nghiệp vụ, kết nối qua Amazon API Gateway, lưu dữ liệu trên 5 bảng DynamoDB, xác thực người dùng qua Amazon Cognito.
+ **Frontend** - Ứng dụng Flutter đa vai trò (multi-mode), dùng `provider` để quản lý state, kết nối backend qua REST API, hỗ trợ quét QR để gọi món và thanh toán.

#### Nội dung

1. [Tổng quan về workshop](5.1-Workshop-overview/)
2. [Chuẩn bị](5.2-Prerequiste/)
3. [Triển khai Backend](5.3-Backend/)
4. [Triển khai Flutter App](5.4-Flutter-App/)
5. [Kiểm thử End-to-End](5.5-e2e-testing/)
6. [Dọn dẹp tài nguyên](5.6-Cleanup/)