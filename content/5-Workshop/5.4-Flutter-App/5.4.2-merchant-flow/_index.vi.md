---
title : "Thử luồng Chủ quán"
date : 2026-06-17
weight : 2
chapter : false
pre : " <b> 5.4.2. </b> "
---

#### Đăng ký quán mới

1. Từ màn hình chọn vai trò, chọn **Tôi là chủ quán**.
2. Vì đây là lần đầu (chưa có `merchant_id` lưu cục bộ), app sẽ chuyển đến **MerchantOnboardingScreen**.
3. Nhập **Tên quán**, ví dụ `Phở Thìn Bờ Hồ`. Trường **Mã quán** sẽ tự sinh bằng cách slugify tên quán (loại dấu tiếng Việt, nối dấu gạch ngang): `pho-thin-bo-ho`.

![Onboarding chủ quán](/images/5-Workshop/5.4-flutter/merchant-onboarding.png)

4. Nhấn **Bắt đầu bán hàng 🚀**. Thông tin `merchant_id` và `merchant_name` được lưu vào `SharedPreferences`, chuyển sang **MerchantHomeScreen**.

{{% notice info %}}
Nếu bạn đã gọi API tạo món ăn bằng cURL ở mục 5.3.2 cho `merchant_id = pho-thin-bo-ho`, hãy dùng đúng tên quán này để món ăn hiển thị ngay trong app.
{{% /notice %}}

#### Thêm món vào menu

1. Từ **MerchantHomeScreen**, vào màn hình quản lý menu.
2. Thêm một vài món ăn (tên, giá, danh mục) — mỗi lần thêm, `MerchantProvider.addMenuItem()` sẽ gọi `POST /menu` với `action: "ADD"`.

![Thêm món vào menu](/images/5-Workshop/5.4-flutter/merchant-menu.png)

3. Kiểm tra lại trên DynamoDB console, bảng `Merchants`, xác nhận danh sách món đã được cập nhật trong item ứng với `merchant_id` của bạn.

![Kiểm tra DynamoDB](/images/5-Workshop/5.4-flutter/dynamodb-menu-check.png)

#### Sinh mã QR bàn

1. Billo dùng định dạng QR riêng cho từng bàn: `BILLO_TABLE|merchant_id|merchant_name|table_number`.
2. Tự tạo một mã QR mẫu (dùng công cụ tạo QR online) với nội dung, ví dụ:

```
BILLO_TABLE|pho-thin-bo-ho|Phở Thìn Bờ Hồ|Ban-01
```

![Tạo QR bàn mẫu](/images/5-Workshop/5.4-flutter/qr-table-generate.png)

Bạn đã có một quán với menu và mã QR bàn sẵn sàng. Ở bước tiếp theo (5.4.3), chúng ta sẽ đóng vai **Khách hàng** quét mã QR này để gọi món và thanh toán.