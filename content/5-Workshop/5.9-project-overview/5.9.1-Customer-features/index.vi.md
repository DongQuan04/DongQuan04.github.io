---
title: "Chức năng Customer"
date: 2026-07-10
weight: 1
chapter: false
pre: " <b> 5.9.1. </b> "
---

---

Phần này trình bày chi tiết từng chức năng dành cho vai trò Customer trong AWS BILLO, kèm bước thao tác thực tế, kết quả mong đợi và ảnh minh họa từ bản demo đã deploy tại https://dev.d28z1hw6wfvjzy.amplifyapp.com.

---

## 1. Đăng ký / Đăng nhập

Customer đăng ký tài khoản bằng số điện thoại và mật khẩu, xác nhận OTP qua cơ chế SMS của Amazon Cognito.

Các bước thao tác:

- Mở màn hình đăng ký trên Flutter app.
- Nhập số điện thoại (ví dụ `0853555443`, hệ thống tự hiểu thành `+84853555443`).
- Nhập mật khẩu.
- Nhận mã OTP qua SMS.
- Nhập OTP để xác nhận tài khoản.
- Đăng nhập bằng tài khoản vừa tạo.

Ảnh: Màn hình đăng ký/đăng nhập Customer

![alt text](./image-13.png)

Kết quả mong đợi:

- User được tạo trong Amazon Cognito với trạng thái `CONFIRMED`.
- Profile và ví mô phỏng được tạo trong DynamoDB.
- Ứng dụng mở giao diện Customer sau khi đăng nhập.

Chức năng liên quan: Amazon Cognito (gửi OTP qua SMS, sử dụng hạ tầng Amazon SNS SMS phía sau).

---

## 2. Đặt PIN giao dịch

Sau khi đăng nhập lần đầu, Customer cần đặt PIN 6 số dùng để xác nhận các giao dịch tiền.

Các bước thao tác:

- Vào tab Cá nhân.
- Chọn Đặt PIN giao dịch.
- Nhập PIN 6 số (ví dụ `123456`).
- Xác nhận lại PIN.
- Lưu lại.

Ảnh: Màn hình đặt PIN giao dịch

![alt text](<Screenshot 2026-07-13 215226.png>)

Kết quả mong đợi:

- PIN được yêu cầu ở mọi giao dịch tiền sau đó: chuyển tiền, thanh toán QR, và khi chuyển đổi giao diện Merchant/Customer (xem mục 7).

Chức năng liên quan: DynamoDB Main Table. PIN giao dịch được hash và lưu trong profile người dùng ở DynamoDB, không lưu trực tiếp trong Cognito.

---

## 3. Ví và lịch sử giao dịch

Customer xem số dư ví hiện tại và toàn bộ lịch sử giao dịch đã thực hiện.

Các bước thao tác:

- Vào Trang chủ để xem số dư ví.
- Vào tab Lịch sử để xem danh sách giao dịch.
- Bấm vào một giao dịch để xem chi tiết (số tiền, thời gian, nội dung, trạng thái).

Ảnh: Màn hình trang chủ và số dư ví

![alt text](image-8.png)

Ảnh: Danh sách lịch sử giao dịch

![alt text](image-7.png)

Ảnh: Chi tiết một giao dịch

![alt text](image-12.png)

Kết quả mong đợi:

- Số dư hiển thị đúng, cập nhật ngay sau mỗi giao dịch.
- Lịch sử hiển thị đúng thứ tự thời gian, mới nhất ở trên.
- Chi tiết giao dịch hiển thị đầy đủ thông tin liên quan.

Chức năng liên quan: DynamoDB Main Table (wallet, transaction).

---

## 4. Chuyển tiền

Người dùng chuyển tiền cho một bên khác bằng số điện thoại, mã ví, QR.

Các bước thao tác:

- Vào Trang chủ, bấm Chuyển tiền.
- Nhập số điện thoại hoặc user nhận, mã ví, quét QR.
- Nhập số tiền và nội dung chuyển tiền.
- App hiện màn hình xác nhận người nhận.
- Bấm Chuyển tiền, nhập PIN giao dịch.

Ảnh: Nhập thông tin chuyển tiền

![alt text](image-9.png)

Ảnh: Xác nhận chuyển tiền bằng PIN

![alt text](image-10.png)

Kết quả mong đợi:

- Nếu đúng PIN và đủ số dư: ví người gửi bị trừ, ví người nhận được cộng, transaction record được tạo cho cả hai phía.

Ảnh: Giao dịch chuyển tiền thành công

![alt text](image-11.png)

- Nếu sai PIN hoặc không đủ số dư: giao dịch bị từ chối, không có tiền bị trừ.
- Bấm nút chuyển tiền nhiều lần liên tiếp (double click) không làm giao dịch bị lặp, nhờ DynamoDB Idempotency Table.

Chức năng liên quan: DynamoDB Idempotency Table (chống trùng giao dịch).

---

## 5. Quét QR bàn và gọi món

Customer quét QR tại bàn của quán để mở menu và đặt món.

Các bước thao tác:

- Mở tab Quét QR.
- Quét QR bàn của quán.
- App mở menu của quán/bàn tương ứng.
- Chọn món/dịch vụ, điều chỉnh số lượng.
- Gửi order.

Ảnh: Quét QR bàn

![alt text](image-4.png)

Ảnh: Menu của quán/bàn

![alt text](image-3.png)

Ảnh: Gửi order thành công

![alt text](image-5.png)

Kết quả mong đợi:

- Customer được gắn vào active table session của bàn đó.
- Order được lưu trong DynamoDB và liên kết với bàn.
- Nếu bàn đã có bill đang mở, món mới được gộp vào bill hiện tại.
- Merchant thấy order xuất hiện trong giao diện kinh doanh sau khi làm mới danh sách đơn.

Chức năng liên quan: DynamoDB Main Table (table, order).

---

## 6. Thanh toán hóa đơn QR

Customer thanh toán hóa đơn bằng cách quét QR thanh toán do Merchant tạo.

Các bước thao tác:

- Mở hóa đơn thanh toán (quét QR do Merchant tạo).
- Kiểm tra thông tin: tên quán, danh sách món, tổng tiền, trạng thái hóa đơn.
- Bấm Chuyển tiền.
- Nhập PIN giao dịch để xác nhận.

Ảnh: Hóa đơn thanh toán QR

![alt text](image-1.png)

Ảnh: Xác nhận thanh toán bằng PIN

![alt text](image-2.png)

Ảnh: Bill thanh toán thành công

![alt text](image-6.png)

Kết quả mong đợi:

- Nếu hóa đơn còn hạn và ví đủ tiền: ví Customer bị trừ, ví Merchant được cộng, order và payment session được đánh dấu hoàn tất, active table được xóa khỏi tài khoản Customer.
- Sau khi thanh toán thành công, hệ thống hiển thị bill thanh toán gồm trạng thái, cửa hàng, danh sách món/dịch vụ, tổng tiền, thời gian, mã đơn và mã giao dịch.
- Nếu hóa đơn ở trạng thái `EXPIRED`: nút chuyển tiền bị khóa, Merchant cần tạo lại QR thanh toán mới.

Chức năng liên quan: DynamoDB (payment session, transaction).

---

## 7. Chuyển đổi giao diện Merchant ↔ Customer

Tài khoản đã có quyền Merchant vẫn giữ được đầy đủ chức năng của Customer, và có thể chuyển qua lại giữa hai giao diện. Cả hai chiều chuyển đổi đều yêu cầu xác thực bằng PIN giao dịch.

Các bước thao tác — từ Merchant sang Customer:

- Từ giao diện Merchant, chọn chuyển sang giao diện ví người dùng (Customer).
- Hệ thống yêu cầu xác thực bằng PIN giao dịch.
- Nếu tài khoản chưa có PIN, hệ thống bắt buộc tạo PIN trước khi chuyển đổi.
- Sau khi xác thực, ứng dụng chuyển sang giao diện Customer với đầy đủ chức năng: ví, chuyển tiền, quét QR bàn, thanh toán.

Ảnh: Nút chuyển đổi sang giao diện Customer

![alt text](image-14.png)

Ảnh: Yêu cầu xác thực PIN khi chuyển đổi

![alt text](image-15.png)

Các bước thao tác — từ Customer sang Merchant:

- Từ giao diện Customer, chọn chuyển sang giao diện kinh doanh (Merchant).
- Hệ thống yêu cầu xác thực lại bằng PIN giao dịch.
- Sau khi xác thực, ứng dụng chuyển sang giao diện Merchant với đầy đủ chức năng quản lý cửa hàng.

Ảnh: Chuyển đổi từ giao diện Customer sang Merchant

![alt text](image-17.png)

Kết quả mong đợi:

- Cả hai chiều chuyển đổi (Merchant → Customer và Customer → Merchant) đều bắt buộc xác thực PIN, không cho phép bỏ qua bước này.
- Tài khoản chưa đặt PIN sẽ bị chặn chuyển đổi cho đến khi tạo PIN thành công.
- Người dùng có thể chuyển đổi qua lại nhiều lần giữa hai giao diện, không giới hạn số lần chuyển đổi.
- Dữ liệu ví, lịch sử giao dịch của Customer không bị ảnh hưởng bởi việc có thêm quyền Merchant.

Chức năng liên quan: DynamoDB Main Table (profile, PIN).

---

## Lỗi thường gặp

| Tình huống | Nguyên nhân có thể |
|---|---|
| Không nhận được OTP | Cognito/SMS đang ở sandbox mode, số điện thoại chưa được verify |
| Chuyển tiền/thanh toán thất bại | Sai PIN, không đủ số dư, hóa đơn/QR đã hết hạn |
| Không quét được QR bàn | Trình duyệt chưa cấp quyền camera, app không chạy trên HTTPS/localhost, QR không hợp lệ |
| Không chuyển đổi được giao diện | Chưa đặt PIN giao dịch, hoặc nhập sai PIN khi xác thực chuyển đổi (áp dụng cho cả 2 chiều) |

---

## Kết quả mong đợi chung

Sau khi hoàn thành phần này, các chức năng chính của vai trò Customer đã được kiểm tra đầy đủ: đăng ký/đăng nhập, đặt PIN, xem ví, chuyển tiền, gọi món qua QR bàn, thanh toán hóa đơn QR, và chuyển đổi giao diện Merchant/Customer (2 chiều, đều yêu cầu PIN) — tất cả hoạt động đúng trên bản demo đã deploy.