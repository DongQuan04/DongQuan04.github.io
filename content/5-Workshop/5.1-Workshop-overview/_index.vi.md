---
title: "Tổng quan workshop"
date: 2026-06-29
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

---

## Giới thiệu

Workshop này giới thiệu cách triển khai và kiểm thử dự án **AWS BILLO**, một hệ thống ví điện tử và quản lý cửa tiệm được xây dựng theo kiến trúc serverless trên AWS.

AWS BILLO được thiết kế cho ba vai trò chính:

- **Customer**: sử dụng ứng dụng mobile/web để đăng ký, đăng nhập, xem ví, quét QR, gọi món tại bàn, thanh toán QR và xem lịch sử giao dịch.
- **Merchant**: quản lý không gian kinh doanh, sản phẩm hoặc dịch vụ, bàn, order, bill và phiên thanh toán QR.
- **Admin**: sử dụng Admin Web để xem xét hồ sơ đăng ký Merchant và duyệt hoặc từ chối yêu cầu đăng ký kinh doanh.

Workshop tập trung vào việc deploy hạ tầng backend serverless và chạy demo ứng dụng ở local.

---

## Mục tiêu workshop

Sau khi hoàn thành workshop này, người thực hiện có thể:

- Hiểu kiến trúc hệ thống AWS BILLO.
- Deploy backend bằng AWS SAM.
- Tạo và quản lý tài nguyên AWS thông qua CloudFormation.
- Sử dụng Amazon Cognito cho xác thực và xác nhận OTP.
- Sử dụng API Gateway để cung cấp backend API được bảo vệ.
- Sử dụng Lambda function để xử lý logic nghiệp vụ.
- Sử dụng DynamoDB để lưu dữ liệu ứng dụng.
- Sử dụng S3 để lưu hình ảnh và tài liệu đăng ký kinh doanh.
- Chạy Flutter frontend ở local và kết nối với backend đã deploy.
- Chạy Admin Web ở local để duyệt hồ sơ Merchant.
- Xem log backend bằng CloudWatch Logs.
- Kiểm thử các luồng chính của Customer, Merchant và Admin.

---

## Dịch vụ và công cụ AWS sử dụng

Workshop này sử dụng các dịch vụ và công cụ AWS sau:

| Dịch vụ / Công cụ          | Mục đích                                                                    |
| -------------------------- | --------------------------------------------------------------------------- |
| Amazon Cognito             | Đăng ký người dùng, xác nhận OTP, đăng nhập, JWT token và user group        |
| Amazon API Gateway         | Cung cấp backend API và bảo vệ route bằng JWT authorization                 |
| AWS Lambda                 | Chạy logic nghiệp vụ backend                                                |
| Amazon DynamoDB            | Lưu users, wallets, stores, products, tables, orders, bills và transactions |
| DynamoDB Idempotency Table | Ngăn xử lý trùng khi chuyển tiền hoặc thanh toán                            |
| Amazon S3                  | Lưu hình ảnh upload và tài liệu đăng ký kinh doanh                          |
| Amazon CloudWatch Logs     | Theo dõi log Lambda/API và hỗ trợ debug                                     |
| AWS SAM                    | Build và deploy backend serverless                                          |
| AWS CloudFormation         | Quản lý tài nguyên AWS thông qua stack                                      |

---

## Dịch vụ không nằm trong phạm vi workshop

Các dịch vụ sau không nằm trong phạm vi workshop hiện tại:

| Dịch vụ                                        | Trạng thái      |
| ---------------------------------------------- | --------------- |
| AWS Amplify Hosting                            | Chưa triển khai |
| Amazon S3 + Amazon CloudFront hosting frontend | Chưa triển khai |
| Amazon EC2                                     | Không sử dụng   |
| Amazon VPC                                     | Không sử dụng   |
| Amazon RDS                                     | Không sử dụng   |

Flutter frontend và Admin Web hiện đang chạy local trong giai đoạn phát triển. Workshop này chưa host frontend lên AWS.

---

## Công nghệ và cấu trúc dự án hiện tại

Source code dự án được chia thành ba phần chính:
![img.png](img.png)

---

## Thông tin deploy

Backend development hiện tại sử dụng cấu hình deploy như sau:

| Nội dung             | Giá trị                                                           |
| -------------------- | ----------------------------------------------------------------- |
| AWS Region           | `ap-southeast-1`                                                  |
| CloudFormation Stack | `wallet-app-backend-dev`                                          |
| API Gateway Endpoint | `https://zsqkp5vpb9.execute-api.ap-southeast-1.amazonaws.com/dev` |
| DynamoDB Main Table  | `wallet-app-main-dev`                                             |
| Cognito User Pool ID | `ap-southeast-1_AKc39KB4L`                                        |

---

## Flow demo chính

Workshop demo theo flow sau:

1. Customer đăng ký hoặc đăng nhập bằng số điện thoại thông qua Cognito OTP.
2. Customer vào app, xem ví và lịch sử giao dịch.
3. Customer gửi yêu cầu đăng ký kinh doanh.
4. Admin đăng nhập vào Admin Web và duyệt hồ sơ Merchant.
5. Merchant đăng nhập lại và mở không gian kinh doanh.
6. Merchant tạo danh mục sản phẩm hoặc dịch vụ.
7. Merchant tạo sản phẩm hoặc dịch vụ với tên, giá, ảnh, giảm giá và trạng thái.
8. Merchant tạo bàn và hệ thống sinh QR bàn.
9. Customer quét QR bàn, xem menu, chọn món và gửi order.
10. Merchant mở bill của bàn và kiểm tra order.
11. Merchant tạo QR thanh toán cho bill.
12. Customer quét QR thanh toán, xem hóa đơn và xác nhận thanh toán.
13. Hệ thống ghi giao dịch, cập nhật số dư ví và lưu lịch sử.
14. Merchant hoặc Admin xem order, trạng thái thanh toán và lịch sử giao dịch.

---

## Kết quả mong đợi

Sau khi hoàn thành workshop, backend sẽ được deploy thành công lên AWS, đồng thời Flutter frontend và Admin Web chạy local có thể kết nối với backend đã deploy.

Người thực hiện có thể kiểm thử các luồng chính của AWS BILLO, bao gồm xác thực, duyệt Merchant, quản lý sản phẩm, gọi món bằng QR bàn, thanh toán QR và xem lịch sử giao dịch.

