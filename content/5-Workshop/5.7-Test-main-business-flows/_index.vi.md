---
title: "Kiểm thử các luồng nghiệp vụ chính"
date: 2026-06-29
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

---

Phần này hướng dẫn kiểm thử các luồng nghiệp vụ chính của AWS BILLO sau khi backend đã được deploy, Flutter frontend và Admin Web đã chạy ở local.

Mục tiêu của phần này là kiểm tra các luồng Customer, Merchant và Admin có thể hoạt động cùng với backend AWS đã deploy hay không.

---

## Tổng quan flow

Flow demo chính gồm:

1. Customer đăng ký và đăng nhập.
2. Customer xem ví và lịch sử giao dịch.
3. Customer gửi hồ sơ đăng ký kinh doanh.
4. Admin duyệt hồ sơ Merchant.
5. Merchant truy cập không gian kinh doanh.
6. Merchant tạo danh mục.
7. Merchant tạo sản phẩm hoặc dịch vụ.
8. Merchant tạo bàn và sinh QR bàn.
9. Customer quét QR bàn và gọi món.
10. Merchant kiểm tra bill và tạo QR thanh toán.
11. Customer quét QR thanh toán và xác nhận thanh toán.
12. Hệ thống cập nhật giao dịch, ví, order và trạng thái thanh toán.

---

## Bước 1: Đăng ký và đăng nhập Customer

Mở Flutter frontend.

Đăng ký tài khoản Customer mới bằng:

```text
Số điện thoại
Mật khẩu
Xác nhận OTP
```

Sau khi xác nhận OTP, đăng nhập bằng tài khoản mới.

Kết quả mong đợi:

- User được tạo trong Amazon Cognito.
- Profile người dùng được tạo trong DynamoDB.
- Ví mô phỏng được tạo cho người dùng.
- Ứng dụng mở giao diện Customer.

---

## Bước 2: Kiểm tra ví và lịch sử của Customer

Sau khi đăng nhập, mở màn hình ví hoặc màn hình home.

Kiểm tra các thông tin sau:

- Số dư ví.
- Thông tin profile.
- Lịch sử giao dịch.
- Chức năng quét QR.

Kết quả mong đợi:

- App có thể tải dữ liệu Customer từ backend đã deploy.
- Dữ liệu ví và giao dịch được đọc từ DynamoDB.
- Customer có thể tiếp tục thực hiện các flow tiếp theo.

---

## Bước 3: Gửi hồ sơ đăng ký kinh doanh

Từ tài khoản Customer, mở chức năng đăng ký kinh doanh.

Nhập các thông tin cần thiết, ví dụ:

```text
Tên doanh nghiệp
Loại hình kinh doanh
Mô tả doanh nghiệp
Thông tin chủ sở hữu
Tài liệu hoặc hình ảnh đăng ký kinh doanh
```

Upload tài liệu hoặc hình ảnh cần thiết.

Kết quả mong đợi:

- App yêu cầu pre-signed URL từ backend.
- File được upload lên Amazon S3.
- Hồ sơ đăng ký kinh doanh được lưu trong DynamoDB.
- Trạng thái hồ sơ là `PENDING`.

---

## Bước 4: Admin xem xét hồ sơ

Mở Admin Web ở local.

```powershell
cd C:\Users\admin\source\repos\AWS_BILLO\admin-web
npm run dev
```

Mở URL Admin Web local được hiển thị trong terminal, thường là:

```text
http://127.0.0.1:5173
```

Đăng nhập bằng tài khoản Admin.

Kết quả mong đợi:

- Admin truy cập được Admin Web.
- Admin xem được các hồ sơ Merchant đang chờ duyệt.
- Hồ sơ đăng ký kinh doanh vừa gửi xuất hiện trong danh sách.

---

## Bước 5: Admin duyệt hồ sơ Merchant

Trong Admin Web, chọn hồ sơ Merchant đang chờ duyệt và thực hiện duyệt.

Kết quả mong đợi:

- Trạng thái hồ sơ được cập nhật thành `APPROVED`.
- User được thêm vào Merchant group trong Cognito.
- Store record được tạo cho Merchant nếu backend workflow hỗ trợ.
- User được duyệt có thể truy cập chức năng Merchant sau khi đăng nhập lại.

Sau khi được duyệt, quay lại Flutter app, đăng xuất và đăng nhập lại.

---

## Bước 6: Mở không gian kinh doanh của Merchant

Sau khi đăng nhập lại, mở phần Merchant hoặc không gian kinh doanh.

Kết quả mong đợi:

- Chức năng Merchant xuất hiện.
- User có thể truy cập các chức năng quản lý cửa hàng.
- User có thể quản lý danh mục, sản phẩm, dịch vụ, bàn và order.

Nếu chức năng Merchant không xuất hiện, hãy đăng xuất và đăng nhập lại để app tải lại thông tin Cognito group.

---

## Bước 7: Tạo danh mục sản phẩm hoặc dịch vụ

Trong giao diện Merchant, tạo một danh mục.

Ví dụ:

```text
Tên danh mục: Đồ uống
Mô tả: Cà phê, trà và các loại nước uống khác
```

Kết quả mong đợi:

- Danh mục được lưu trong DynamoDB.
- Danh mục xuất hiện trong màn hình quản lý sản phẩm của Merchant.
- Sản phẩm hoặc dịch vụ có thể được gán vào danh mục này.

---

## Bước 8: Tạo sản phẩm hoặc dịch vụ

Tạo sản phẩm hoặc dịch vụ với các thông tin như:

```text
Tên
Giá
Hình ảnh
Mô tả
Giảm giá
Best seller hoặc must try
Trạng thái bán hàng
```

Kết quả mong đợi:

- Sản phẩm hoặc dịch vụ được lưu trong DynamoDB.
- Hình ảnh sản phẩm được upload lên S3 thông qua pre-signed URL.
- Sản phẩm xuất hiện trong menu của cửa hàng.
- Merchant có thể cập nhật hoặc ẩn sản phẩm khi cần.

---

## Bước 9: Tạo bàn và sinh QR bàn

Trong giao diện Merchant, mở chức năng quản lý bàn.

Tạo một bàn mới.

Ví dụ:

```text
Tên bàn: Bàn 01
Trạng thái: Available
```

Kết quả mong đợi:

- Bàn được lưu trong DynamoDB.
- Hệ thống sinh QR code cho bàn.
- QR code có thể được Customer sử dụng để mở menu của cửa hàng tại bàn đó.

---

## Bước 10: Customer quét QR bàn và gọi món

Chuyển sang giao diện Customer.

Quét QR bàn được Merchant tạo.

Kết quả mong đợi:

- Customer tham gia vào active table session.
- App mở menu của cửa hàng.
- Customer có thể chọn sản phẩm và gửi order.

Gửi order.

Kết quả mong đợi:

- Order được lưu trong DynamoDB.
- Order được liên kết với bàn.
- Nếu bàn đã có bill đang hoạt động, món mới được gộp vào bill hiện tại.
- Merchant có thể xem order từ giao diện kinh doanh.

---

## Bước 11: Merchant kiểm tra bill

Chuyển sang giao diện Merchant.

Mở bill của bàn.

Kết quả mong đợi:

- Merchant thấy order của Customer.
- Bill hiển thị sản phẩm, số lượng và tổng tiền.
- Merchant có thể thêm, xóa hoặc sửa món nếu khách gọi miệng.
- Bill đã cập nhật được lưu trước khi tạo QR thanh toán.

---

## Bước 12: Tạo QR thanh toán

Trong màn hình bill của Merchant, chọn:

```text
Generate Payment QR
```

Kết quả mong đợi:

- Backend tạo payment session.
- QR thanh toán được sinh ra.
- QR code chứa thông tin payment session.
- QR thanh toán cũ sẽ không còn hợp lệ nếu bill thay đổi.

---

## Bước 13: Customer quét QR thanh toán và trả tiền

Chuyển lại giao diện Customer.

Quét QR thanh toán.

Kết quả mong đợi:

- App hiển thị tên cửa hàng.
- App hiển thị chi tiết bill.
- App hiển thị tổng tiền.
- Customer có thể xác nhận thanh toán.

Xác nhận thanh toán.

Kết quả mong đợi:

- Ví Customer bị trừ tiền.
- Ví Merchant được cộng tiền.
- Transaction record được tạo cho cả hai phía.
- Order được đánh dấu là đã thanh toán.
- Payment session được đánh dấu hoàn tất.
- Active table được xóa khỏi tài khoản Customer.

---

## Bước 14: Kiểm tra lịch sử giao dịch và order

Sau khi thanh toán, kiểm tra lịch sử giao dịch trong app Customer.

Kết quả mong đợi:

- Customer thấy giao dịch thanh toán.
- Giao dịch có thông tin số tiền, thời gian và nội dung liên quan.

Sau đó kiểm tra lịch sử giao dịch hoặc order của Merchant.

Kết quả mong đợi:

- Merchant thấy khoản tiền nhận được.
- Trạng thái order được cập nhật.
- Bill không còn ở trạng thái chờ thanh toán.

---

## Bước 15: Kiểm tra dữ liệu trên AWS Console

Để xác nhận hệ thống hoạt động đúng, kiểm tra các dịch vụ AWS liên quan.

### DynamoDB

Mở:

```text
DynamoDB > Tables > wallet-app-main-dev
```
![img.png](img.png)

Kiểm tra các record liên quan đến:

- User profile.
- Wallet.
- Merchant application.
- Store.
- Product hoặc service.
- Table.
- Order.
- Payment session.
- Transaction.

### S3

Mở:

```text
Amazon S3 > Buckets
```

Kiểm tra các file upload như:

- Tài liệu đăng ký kinh doanh.
- Hình ảnh cửa hàng.
- Hình ảnh sản phẩm.

### Cognito

Mở:

```text
Amazon Cognito > User pools > Users
```

Kiểm tra user đã được duyệt có thuộc Merchant group hay không.

---

## Lỗi thường gặp

### Chức năng Merchant không xuất hiện

User có thể cần đăng xuất và đăng nhập lại sau khi Admin duyệt.

### Ảnh sản phẩm không upload được

Kiểm tra:

- Quyền của S3 bucket.
- Việc tạo pre-signed URL.
- Lỗi trong browser console.
- Lambda logs trong CloudWatch.

### Không quét được QR bàn

Kiểm tra:

- Quyền camera của trình duyệt.
- App có đang chạy trên localhost không.
- QR code có hợp lệ không.
- QR session hoặc table còn tồn tại không.

### Thanh toán thất bại

Kiểm tra:

- Số dư ví Customer.
- Payment session đã hết hạn chưa.
- QR code có bị cũ không.
- Lỗi DynamoDB Transaction.
- CloudWatch Logs.

---

## Kết quả mong đợi

Sau khi hoàn thành phần này, luồng nghiệp vụ chính của AWS BILLO sẽ được demo thành công từ đầu đến cuối:

```text
Customer registration
→ Business registration
→ Admin approval
→ Merchant setup
→ Product and table creation
→ Customer table ordering
→ Merchant bill review
→ QR payment
→ Transaction history
```

Điều này xác nhận Flutter frontend, Admin Web, Cognito, API Gateway, Lambda, DynamoDB, S3 và CloudWatch Logs đang phối hợp với nhau trong dự án AWS BILLO.

