---
title : "Thử luồng Khách hàng"
date : 2026-06-17
weight : 3
chapter : false
pre : " <b> 5.4.3. </b> "
---

#### Chuyển sang vai trò Khách hàng

1. Trên **MerchantHomeScreen**, nhấn nút chuyển đổi vai trò (icon ⇄) trên AppBar — thao tác này gọi `ModeProvider.switchMode()`.
2. App điều hướng về **ModeSelectScreen** rồi vào thẳng **CustomerHomeScreen** (vì `hasChosenBefore = true`).

![Chuyển vai trò](/images/5-Workshop/5.4-flutter/switch-mode.png)

#### Quét QR bàn và gọi món

1. Từ **CustomerHomeScreen**, chọn **Quét QR bàn**.
2. Đưa camera vào mã QR bàn đã tạo ở bước 5.4.2 (`BILLO_TABLE|...`). `QrScanScreen` sẽ nhận diện, gọi `CustomerProvider.startTableSession()` rồi chuyển sang **MenuScreen**.

![Quét QR bàn](/images/5-Workshop/5.4-flutter/qr-scan-table.png)

3. Trên **MenuScreen**, chọn vài món, số lượng hiển thị realtime ở thanh dưới cùng.

![Chọn món](/images/5-Workshop/5.4-flutter/menu-select.png)

4. Nhấn **Gửi đơn** — `CustomerProvider.submitOrder()` gọi `POST /orders`, chuyển sang **OrderWaitingScreen**.

![Gửi đơn thành công](/images/5-Workshop/5.4-flutter/order-waiting.png)

{{% notice info %}}
Kiểm tra lại DynamoDB table `Orders`, bạn sẽ thấy một item mới với `status: "NEW"` chứa danh sách món vừa gửi.
{{% /notice %}}

#### Tạo hóa đơn (phía quán) và thanh toán (phía khách)

1. Quay lại vai trò **Chủ quán**, vào màn hình đơn đang chờ (`fetchPendingOrders`), xác nhận đơn và tạo hóa đơn — gọi `POST /invoices/confirm`.
2. App phía quán hiển thị mã QR thanh toán theo định dạng: `BILLO|invoice_id|merchant_id|total|table_number`.

![Hóa đơn & QR thanh toán](/images/5-Workshop/5.4-flutter/invoice-qr.png)

3. Chuyển lại vai trò **Khách hàng**, từ **OrderWaitingScreen** chọn **Quét QR thanh toán**, quét mã QR hóa đơn vừa tạo.
4. **InvoiceConfirmScreen** hiển thị số tiền, mã hóa đơn, bàn — nhấn **Xác nhận thanh toán**, gọi `POST /payments`.

![Xác nhận thanh toán](/images/5-Workshop/5.4-flutter/invoice-confirm.png)

5. Thanh toán thành công, chuyển đến **PaymentSuccessScreen**.

![Thanh toán thành công](/images/5-Workshop/5.4-flutter/payment-success.png)

Bạn đã hoàn thành trọn vẹn một chu trình nghiệp vụ: quán tạo menu → khách quét QR gọi món → quán xuất hóa đơn → khách thanh toán qua QR. Ở bước tiếp theo (5.4.4), chúng ta sẽ build bản release để chạy thử trên thiết bị thật.