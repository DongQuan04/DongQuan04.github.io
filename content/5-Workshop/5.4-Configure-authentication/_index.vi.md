---
title: "Cấu hình xác thực"
date: 2026-06-29
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

---

Phần này giải thích cách AWS BILLO cấu hình xác thực người dùng bằng Amazon Cognito.

Amazon Cognito chịu trách nhiệm đăng ký người dùng, xác minh số điện thoại, xác nhận OTP, đăng nhập, cấp JWT token và quản lý user group.

AWS BILLO sử dụng Cognito để hỗ trợ ba vai trò chính:

- Customer
- Merchant
- Admin

---

## Luồng xác thực

Luồng xác thực hoạt động như sau:

1. Người dùng đăng ký bằng số điện thoại và mật khẩu.
2. Amazon Cognito tạo tài khoản người dùng.
3. Cognito gửi mã OTP đến số điện thoại đã đăng ký.
4. Người dùng xác nhận mã OTP.
5. Sau khi xác nhận, backend tạo profile và ví cho người dùng.
6. Người dùng đăng nhập và nhận JWT token.
7. Frontend dùng JWT token để gọi các API được bảo vệ thông qua API Gateway.
8. API Gateway kiểm tra JWT token trước khi chuyển request đến Lambda.

---

## Bước 1: Mở Amazon Cognito

Mở AWS Management Console và vào:

```text
Amazon Cognito > User pools
```

Tìm User Pool đang được dự án sử dụng:

```text
ap-southeast-1_AKc39KB4L
```

Đây là Cognito User Pool của môi trường development.

---

## Bước 2: Kiểm tra cấu hình User Pool

Trong User Pool, kiểm tra các cấu hình cơ bản:

| Nội dung           | Mục đích                                         |
| ------------------ | ------------------------------------------------ |
| Sign-in method     | Cho phép người dùng đăng nhập bằng số điện thoại |
| OTP / verification | Xác nhận số điện thoại người dùng                |
| App Client         | Cho phép frontend xác thực với Cognito           |
| User Groups        | Phân tách vai trò Customer, Merchant và Admin    |

User Pool được tạo và quản lý bởi backend stack AWS SAM.

---

## Bước 3: Kiểm tra Cognito User Groups

AWS BILLO sử dụng Cognito groups để phân tách quyền người dùng.

Mở:

```text
Amazon Cognito > User pools > Groups
```

Kiểm tra các group sau:

```text
Customer
Merchant
Admin
```

![img.png](img.png)

### Customer Group

Customer là vai trò người dùng mặc định. Customer có thể:

- Đăng ký và đăng nhập
- Xem số dư ví
- Chuyển tiền
- Quét QR bàn
- Gửi order
- Quét QR thanh toán
- Xem lịch sử giao dịch

### Merchant Group

Merchant có thể truy cập các chức năng kinh doanh sau khi được Admin duyệt. Merchant có thể:

- Quản lý không gian kinh doanh
- Tạo danh mục
- Tạo sản phẩm hoặc dịch vụ
- Tạo bàn
- Xem bill theo bàn
- Tạo QR thanh toán
- Xem giao dịch của Merchant

### Admin Group

Admin có thể truy cập Admin Web và duyệt hồ sơ đăng ký Merchant. Admin có thể:

- Xem hồ sơ Merchant đang chờ duyệt
- Duyệt hồ sơ Merchant
- Từ chối hồ sơ Merchant
- Gán quyền Merchant thông qua backend workflow

---

## Bước 4: Hiểu giới hạn OTP SMS

Cognito OTP sử dụng SMS để gửi mã xác thực. Trong quá trình phát triển, việc gửi SMS có thể phụ thuộc vào cấu hình AWS SMS sandbox hoặc production SMS.

Nếu tài khoản AWS vẫn đang ở SMS sandbox mode, chỉ các số điện thoại đã xác minh mới có thể nhận OTP.

Nếu không nhận được OTP, cần kiểm tra:

- Định dạng số điện thoại
- Số điện thoại đã được verify trong sandbox chưa
- Cấu hình AWS SMS
- Cấu hình verification của Cognito
- CloudWatch Logs để kiểm tra lỗi backend

---

## Bước 5: Cấu hình thông tin xác thực cho frontend

Flutter frontend đọc cấu hình AWS backend từ file:

```text
frontend/config/dev.json
```

Đảm bảo file này chứa đúng giá trị development, ví dụ:

```json
{
  "API_BASE_URL": "https://zsqkp5vpb9.execute-api.ap-southeast-1.amazonaws.com/dev",
  "AWS_REGION": "ap-southeast-1",
  "COGNITO_USER_POOL_ID": "ap-southeast-1_AKc39KB4L"
}
```

Tên key thực tế có thể phụ thuộc vào cách dự án triển khai. Điểm quan trọng là frontend phải sử dụng đúng Cognito User Pool và API Gateway endpoint đã được backend stack tạo ra.

---

## Bước 6: Kiểm thử đăng ký

Chạy Flutter frontend và mở màn hình đăng ký.

Nhập:

```text
Số điện thoại
Mật khẩu
Xác nhận mật khẩu
```

Sau đó gửi form đăng ký.

Kết quả mong đợi:

- Cognito tạo tài khoản người dùng.
- OTP được gửi đến số điện thoại.
- Ứng dụng mở màn hình xác nhận OTP.

---

## Bước 7: Xác nhận OTP

Nhập mã OTP nhận được qua SMS.

Kết quả mong đợi:

- Tài khoản người dùng được xác nhận trong Cognito.
- Backend tạo profile người dùng.
- Backend tạo ví mô phỏng.
- Người dùng có thể đăng nhập.

Nếu xác nhận OTP thất bại, cần kiểm tra mã OTP có đúng không và số điện thoại có được phép nhận SMS trong môi trường AWS hiện tại không.

---

## Bước 8: Kiểm thử đăng nhập

Sau khi xác nhận OTP, đăng nhập bằng:

```text
Số điện thoại
Mật khẩu
```

Kết quả mong đợi:

- Cognito trả về JWT token.
- Frontend lưu session trong quá trình chạy.
- Ứng dụng mở giao diện Customer mặc định.
- Các API được bảo vệ có thể được gọi thông qua API Gateway.

---

## Bước 9: Kiểm tra user trong Cognito

Mở:

```text
Amazon Cognito > User pools > Users
```


Tìm số điện thoại đã đăng ký.

Kiểm tra:

- User đã tồn tại.
- User có trạng thái confirmed.
- Thuộc tính người dùng chính xác.
- User nằm trong group phù hợp nếu có.

---

## Bước 10: Kiểm tra profile trong DynamoDB

Mở:

```text
DynamoDB > Tables > wallet-app-main-dev
```
![img_1.png](img_1.png)


Kiểm tra profile và wallet record đã được tạo cho người dùng mới.

Partition key và sort key cụ thể phụ thuộc vào data model của dự án, nhưng sau khi đăng ký và xác nhận thành công, bảng sẽ có các record liên quan đến người dùng.

---

## Kết quả mong đợi

Sau khi hoàn thành phần này:

- Cognito authentication hoạt động.
- Người dùng có thể đăng ký bằng số điện thoại.
- Người dùng có thể xác nhận OTP.
- Người dùng có thể đăng nhập và nhận JWT token.
- API Gateway có thể bảo vệ backend API bằng Cognito JWT authorization.
- Vai trò Customer, Merchant và Admin có thể được phân tách bằng Cognito groups.

---

## Xử lý lỗi

### Không nhận được OTP

Nguyên nhân có thể gồm:

- AWS SMS vẫn đang ở sandbox mode.
- Số điện thoại chưa được verify.
- Định dạng số điện thoại không đúng.
- SMS quota hoặc cấu hình messaging theo region bị giới hạn.

### Đăng nhập thất bại sau khi đăng ký

Nguyên nhân có thể gồm:

- Chưa xác nhận OTP.
- Sai mật khẩu.
- Frontend đang trỏ sai Cognito User Pool.
- Cấu hình App Client không khớp với frontend.

### API trả về unauthorized

Nguyên nhân có thể gồm:

- Thiếu JWT token.
- JWT token hết hạn.
- API Gateway authorizer cấu hình chưa đúng.
- Frontend đang gọi sai API Gateway endpoint.

