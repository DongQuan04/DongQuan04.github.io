---
title : "Kiểm thử End-to-End"
date : 2026-07-08
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

#### Kịch bản kiểm thử chính

Tổng hợp lại toàn bộ luồng nghiệp vụ đã thực hiện qua các bước trước, dùng bảng dưới đây làm checklist kiểm thử end-to-end:

| # | Bước | Vai trò | API được gọi | Kết quả mong đợi |
|---|------|---------|---------------|-------------------|
| 1 | Đăng ký quán mới | Chủ quán | (lưu local, chưa gọi API) | `merchant_id` sinh tự động, lưu `SharedPreferences` |
| 2 | Thêm món vào menu | Chủ quán | `POST /menu` | Món xuất hiện trong bảng `Merchants` |
| 3 | Quét QR bàn | Khách hàng | `GET /menu/{merchant_id}` | Menu hiển thị đúng theo quán |
| 4 | Gửi đơn | Khách hàng | `POST /orders` | Đơn mới có `status: NEW` trong bảng `Orders` |
| 5 | Xem đơn đang chờ | Chủ quán | `GET /orders/{merchant_id}/pending` | Đơn vừa gửi hiển thị đúng |
| 6 | Xác nhận & tạo hóa đơn | Chủ quán | `POST /invoices/confirm` | Hóa đơn mới có `status: PENDING` |
| 7 | Quét QR thanh toán | Khách hàng | (parse QR local) | Hiển thị đúng số tiền, mã hóa đơn |
| 8 | Xác nhận thanh toán | Khách hàng | `POST /payments` | Hóa đơn chuyển `status: SUCCESS` |

![Checklist kiểm thử](/images/5-Workshop/5.5-kiem-thu/checklist.png)

#### Kiểm thử các trường hợp lỗi (negative test)

1. **QR sai định dạng**: quét một mã QR bất kỳ không theo prefix `BILLO_TABLE|` hoặc `BILLO|` — app phải hiển thị thông báo lỗi "Không phải mã QR hợp lệ" thay vì crash.

![Test QR sai](/images/5-Workshop/5.5-kiem-thu/invalid-qr.png)

2. **Mất kết nối mạng**: tắt Wi-Fi/dữ liệu di động khi đang gửi đơn — `CustomerProvider.errorMessage` phải được set và hiển thị qua `SnackBar`, không để app treo ở trạng thái loading vô hạn.

3. **Gọi API khi menu rỗng**: quét QR bàn của một quán chưa thêm món nào — màn hình `MenuScreen` phải hiển thị thông báo "Quán chưa cập nhật menu" thay vì danh sách trống không rõ nguyên nhân.

![Menu rỗng](/images/5-Workshop/5.5-kiem-thu/empty-menu.png)

#### Theo dõi qua CloudWatch

Trong suốt quá trình test, mở song song **CloudWatch Logs** của các Lambda function liên quan (`billo-manage-orders`, `billo-create-invoice`, `billo-process-payment`) để đối chiếu request/response thực tế với thao tác trên app, giúp phát hiện nhanh lỗi runtime nếu có.

![CloudWatch song song](/images/5-Workshop/5.5-kiem-thu/cloudwatch-parallel.png)

Sau khi tất cả các bước trong checklist đều pass, hệ thống đã sẵn sàng để demo. Ở bước cuối (5.6), chúng ta sẽ dọn dẹp tài nguyên AWS đã tạo để tránh phát sinh chi phí ngoài ý muốn.