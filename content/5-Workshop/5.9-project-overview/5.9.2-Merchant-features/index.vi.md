---
title: "Chức năng Merchant"
date: 2026-07-10
weight: 2
chapter: false
pre: " <b> 5.9.2. </b> "
---

---

Phần này trình bày chi tiết từng chức năng dành cho vai trò Merchant trong AWS BILLO, kèm bước thao tác thực tế, kết quả mong đợi và ảnh minh họa từ bản demo đã deploy tại https://dev.d28z1hw6wfvjzy.amplifyapp.com.

Merchant là Customer đã được Admin duyệt hồ sơ kinh doanh và được cấp thêm quyền quản lý cửa hàng.

---

## 1. Đăng ký kinh doanh

Từ tài khoản Customer, người dùng gửi hồ sơ đăng ký kinh doanh để trở thành Merchant.

Các bước thao tác:

- Đăng nhập bằng tài khoản Customer.
- Vào tab Cá nhân, chọn Đăng ký kinh doanh.
- Nhập thông tin:
  - Họ tên chủ kinh doanh.
  - Tên cửa hàng/doanh nghiệp.
  - Số điện thoại liên hệ.
  - CCCD.
  - Địa chỉ kinh doanh.
  - Ảnh giấy phép kinh doanh.
- Upload ảnh giấy phép kinh doanh.
- Gửi hồ sơ xét duyệt.

![alt text](image.png)

Kết quả mong đợi:

- App yêu cầu pre-signed URL từ backend để upload ảnh.
- Ảnh được lưu trong Amazon S3.
- Hồ sơ đăng ký được lưu trong DynamoDB với trạng thái `PENDING`.
- Hồ sơ xuất hiện trong danh sách chờ duyệt của Admin.
- Người dùng chờ Admin xem xét, chưa có quyền Merchant ngay lúc này.

Chức năng liên quan: Amazon S3 (upload tài liệu qua pre-signed URL), DynamoDB Main Table.

---

## 2. Không gian kinh doanh

Sau khi Admin duyệt hồ sơ, Merchant truy cập được khu vực quản lý cửa hàng riêng.

Các bước thao tác:

- Sau khi được duyệt, đăng xuất và đăng nhập lại (để app nhận Cognito group mới).
- Vào giao diện Merchant / Không gian kinh doanh.
- Kiểm tra các tab hiển thị: Tổng quan, Dịch vụ, Bàn, POS, Đơn hàng.

![alt text](image-1.png)

Kết quả mong đợi:

- User được thêm vào Cognito group `Merchant`.
- Store record được tạo cho Merchant.
- Giao diện Merchant xuất hiện đầy đủ 5 tab quản lý.
- Nếu chưa thấy giao diện Merchant, cần đăng xuất/đăng nhập lại để app tải lại thông tin group.

Chức năng liên quan: Cognito User Group `Merchant`, DynamoDB Main Table (store).

### 2.1. Cập nhật thông tin cửa hàng

Trong tab Tổng quan, Merchant có thể cập nhật:

- Tên quán.
- Địa chỉ.
- Ảnh đại diện/ảnh quán.
- Trạng thái hoạt động (bật/tắt).

![alt text](image-2.png)


Kết quả mong đợi: thông tin cửa hàng được cập nhật trong DynamoDB và phản ánh ngay trên menu phía Customer. Nếu Merchant tắt trạng thái hoạt động, Customer sẽ không đặt món được nữa dù vẫn quét đúng QR bàn.

---

## 3. Quản lý danh mục và sản phẩm/dịch vụ

Merchant tổ chức menu theo danh mục, thêm sản phẩm/dịch vụ và cấu hình giảm giá.

Các bước thao tác — tạo danh mục:

- Vào tab Dịch vụ.
- Tạo danh mục mới, ví dụ: Trà sữa, Món nước, Ăn vặt.

Các bước thao tác — thêm sản phẩm/dịch vụ:

- Thêm món/dịch vụ mới với: tên, giá, ảnh, danh mục.
- Đánh dấu Best seller / Must try nếu có.
- Chọn trạng thái: đang bán hoặc tạm ẩn.
- Lưu lại và kiểm tra món hiển thị đúng trong menu khách hàng.

Các bước thao tác — cấu hình giảm giá:

- Vào sửa món/dịch vụ cần giảm giá.
- Nhập giá gốc, giá giảm, phần trăm giảm hiển thị.
- Thiết lập thời gian bắt đầu/kết thúc, hoặc khung giờ/ngày trong tuần áp dụng.
- Lưu lại.

![alt text](../image-17.png)

Kết quả mong đợi:

- Danh mục và sản phẩm được lưu trong DynamoDB, ảnh sản phẩm được upload lên S3 qua pre-signed URL.
- Sản phẩm ẩn sẽ không hiển thị trong menu Customer.
- Trong thời gian áp dụng giảm giá, Customer thấy đúng giá đã giảm và phần trăm giảm; ngoài thời gian đó giá tự trở lại giá gốc.

Chức năng liên quan: DynamoDB Main Table, Amazon S3 (ảnh sản phẩm).

---

## 4. Quản lý bàn và QR bàn

Merchant tạo bàn cho quán, hệ thống sinh QR riêng cho từng bàn.

Các bước thao tác:

- Vào tab Bàn, bấm Thêm bàn.
- Nhập tên/số bàn, khu vực/tầng nếu có.
- Hệ thống tự sinh QR cho bàn vừa tạo.
- Bấm vào bàn để xem chi tiết: QR của bàn, đơn hiện tại, món khách đã gọi.
- Tải QR bàn, in ra và dán lên bàn thật để Customer quét.

![alt text](image-5.png)

Kết quả mong đợi:

- Bàn được lưu trong DynamoDB.
- QR code sinh ra có thể được Customer quét để mở đúng menu của quán tại đúng bàn đó.
- Xóa một bàn đang có đơn dở cần được xử lý cẩn thận (nên cảnh báo hoặc chặn nếu bàn còn bill đang mở).

Chức năng liên quan: DynamoDB Main Table (table).

---

## 5. Nhận order và xử lý bill

Merchant theo dõi order của từng bàn theo thời gian thực và chỉnh sửa bill trước khi thanh toán.

Các bước thao tác:

- Customer quét QR bàn và gửi order.
- Merchant vào tab Bàn, bấm vào bàn tương ứng.
- Xem đơn đang chờ: món, số lượng, tổng tiền tạm tính.
- Nếu cần, Merchant có thể:
  - Thêm món nếu khách gọi miệng.
  - Xóa món nếu khách đổi ý.
  - Chỉnh số lượng.
  - Lưu bill đã cập nhật.

(Khi bên khách hàng order)

![alt text](image-7.png)

(Đơn được gửi đến bàn)

![alt text](image-6.png)

Kết quả mong đợi:

- Order mới từ Customer xuất hiện gần như tức thời trong giao diện Merchant.
- Bill hiển thị đúng danh sách món, số lượng và tổng tiền hiện tại.
- Mọi thay đổi trên bill (thêm/xóa/sửa) được lưu trước khi tạo QR thanh toán, đảm bảo QR phản ánh đúng số tiền cuối cùng.

Chức năng liên quan: DynamoDB Main Table (order, bill).

---

## 6. Thanh toán

Merchant có hai hình thức chốt đơn: thanh toán qua QR/ví hoặc tiền mặt.

### 6.1. Thanh toán QR/ví

Các bước thao tác:

- Trong chi tiết bàn hoặc đơn hàng, bấm Tạo QR thanh toán (Generate Payment QR).
- Đưa QR cho Customer quét bằng app.
- Customer xác nhận thanh toán bằng PIN.
- Merchant theo dõi trạng thái đơn chuyển sang đã thanh toán.

![alt text](image-9.png)

### 6.2. Thanh toán tiền mặt

Các bước thao tác:

- Merchant chọn hình thức thanh toán tiền mặt.
- Đơn được đánh dấu đã thanh toán thủ công.
- Bàn trở về trạng thái trống, sẵn sàng cho lượt khách tiếp theo.

![alt text](image-8.png)

Kết quả mong đợi:

- Với thanh toán QR: backend tạo payment session, ví Customer bị trừ, ví Merchant được cộng, transaction record được tạo cho cả hai phía, payment session được đánh dấu hoàn tất.
- QR thanh toán cũ sẽ không còn hợp lệ nếu bill thay đổi sau khi đã tạo QR — Merchant cần tạo lại QR mới.
- Sau khi thanh toán (QR hoặc tiền mặt), bàn được giải phóng và active table bị xóa khỏi tài khoản Customer.

Chức năng liên quan: DynamoDB (payment session, transaction); ví Merchant được cộng tiền.

---

## Lỗi thường gặp

| Tình huống | Nguyên nhân có thể |
|---|---|
| Chức năng Merchant không xuất hiện sau khi được duyệt | Chưa đăng xuất/đăng nhập lại để app nhận Cognito group mới |
| Ảnh sản phẩm/giấy phép kinh doanh không upload được | Quyền S3 bucket, lỗi tạo pre-signed URL, loại/kích thước file không hợp lệ |
| QR thanh toán không dùng được | Bill đã thay đổi sau khi tạo QR, payment session đã hết hạn |
| Order của Customer không hiện trong giao diện Merchant | Lỗi kết nối API Gateway/Lambda, kiểm tra CloudWatch Logs |

---

## Kết quả mong đợi chung

Sau khi hoàn thành phần này, các chức năng chính của vai trò Merchant đã được kiểm tra đầy đủ: đăng ký kinh doanh, quản lý cửa hàng/sản phẩm/giảm giá, quản lý bàn và QR bàn, xử lý order/bill theo thời gian thực, và nhận thanh toán qua QR hoặc tiền mặt — tất cả hoạt động đúng trên bản demo đã deploy.