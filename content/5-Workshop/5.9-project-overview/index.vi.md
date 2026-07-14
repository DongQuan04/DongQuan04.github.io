---
title: "Giới thiệu tổng quan dự án"
date: 2026-07-10
weight: 9
chapter: false
pre: " <b> 5.9. </b> "
---

---

Phần này giới thiệu tổng quan sản phẩm AWS BILLO sau khi đã hoàn thành các bước deploy, cấu hình xác thực, chạy demo và kiểm thử ở các phần trước.

Mục tiêu là giúp người xem hiểu AWS BILLO là gì, gồm những vai trò nào, và mỗi vai trò có các chức năng gì. Chi tiết từng chức năng theo từng vai trò được trình bày trong 3 phần con: 5.9.1, 5.9.2 và 5.9.3.

---

## Giới thiệu dự án

AWS BILLO là ứng dụng ví điện tử kết hợp gọi món/thanh toán bằng QR bàn, được xây dựng trên nền tảng serverless của AWS (Cognito, API Gateway, Lambda, DynamoDB, S3, CloudWatch) và triển khai bằng AWS SAM/CloudFormation.

Ứng dụng gồm hai phần giao diện, hiện đã được host trên AWS Amplify:

Flutter app   → https://dev.d28z1hw6wfvjzy.amplifyapp.com (Customer, Merchant)

Admin Web     → https://dev.d3k8atm3w5sdj3.amplifyapp.com (Admin)


Toàn bộ dữ liệu và luồng nghiệp vụ trong phần này được lấy từ chính môi trường development đã deploy ở các phần trước:

API Gateway: https://zsqkp5vpb9.execute-api.ap-southeast-1.amazonaws.com/dev

Cognito User Pool ID: ap-southeast-1_AKc39KB4L

DynamoDB Main Table: wallet-app-main-dev

---

## Ba vai trò của hệ thống

| Vai trò | Giao diện | Chức năng chính |
|---|---|---|
| Customer | Flutter app | Đăng ký ví, đặt PIN, chuyển tiền, gọi món QR bàn, thanh toán QR |
| Merchant | Flutter app | Đăng ký kinh doanh, quản lý sản phẩm/danh mục/bàn, xử lý bill, nhận thanh toán |
| Admin | Admin Web | Duyệt hồ sơ Merchant, quản lý user/cửa hàng, xem giao dịch, xử lý hoàn tiền |

---

## Các phần chi tiết

### [5.9.1 - Chức năng Customer](5.9.1-Customer-features/)

Trình bày chi tiết từng chức năng của Customer: đăng ký/đăng nhập, đặt PIN, ví và lịch sử giao dịch, chuyển tiền, quét QR bàn gọi món, thanh toán hóa đơn QR — kèm bước thao tác, kết quả mong đợi và ảnh minh họa.

### [5.9.2 - Chức năng Merchant](5.9.2-Merchant-features/)

Trình bày chi tiết từng chức năng của Merchant: đăng ký kinh doanh, không gian kinh doanh, quản lý danh mục/sản phẩm/giảm giá, quản lý bàn và QR bàn, nhận order và xử lý bill, thanh toán — kèm bước thao tác, kết quả mong đợi và ảnh minh họa.

### [5.9.3 - Chức năng Admin](5.9.3-Admin-features/)

Trình bày chi tiết từng chức năng của Admin: đăng nhập, dashboard tổng quan, duyệt/từ chối hồ sơ Merchant, quản lý người dùng và cửa hàng, xem giao dịch và xử lý hoàn tiền — kèm bước thao tác, kết quả mong đợi và ảnh minh họa.

---

## Luồng tổng thể

Sơ đồ tóm tắt cách ba vai trò phối hợp với nhau, đúng như đã demo end-to-end ở phần 5.7:

```text
Customer đăng ký & đăng ký kinh doanh
        ↓
Admin duyệt hồ sơ Merchant
        ↓
Merchant tạo cửa hàng, sản phẩm, bàn, QR bàn
        ↓
Customer quét QR bàn, gọi món
        ↓
Merchant xử lý bill, tạo QR thanh toán
        ↓
Customer thanh toán bằng PIN
        ↓
Admin giám sát giao dịch & xử lý hoàn tiền (nếu có)
```

---

## Kết quả mong đợi

Sau khi hoàn thành phần này:

- Người xem hiểu AWS BILLO là ứng dụng gì và gồm những vai trò nào.
- Người xem nắm được luồng nghiệp vụ tổng thể kết nối ba vai trò với nhau.
- Người xem có thể đi sâu vào từng vai trò cụ thể qua các phần 5.9.1, 5.9.2, 5.9.3.