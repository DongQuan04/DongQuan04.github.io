---
title: "Chạy demo Admin Web"
date: 2026-06-29
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

---

Phần này hướng dẫn chạy Admin Web của AWS BILLO ở môi trường local.

Admin Web được sử dụng bởi vai trò Admin để xem xét hồ sơ đăng ký Merchant, duyệt hoặc từ chối yêu cầu đăng ký kinh doanh và hỗ trợ quy trình kích hoạt quyền Merchant.

Trong giai đoạn phát triển hiện tại, Admin Web chạy local bằng Vite. Workshop này chưa host Admin Web bằng AWS Amplify, Amazon S3 hoặc Amazon CloudFront.

---

## Bước 1: Mở thư mục Admin Web

Mở PowerShell và chuyển đến thư mục Admin Web:

```powershell
cd C:\Users\admin\source\repos\AWS_BILLO\admin-web
```

Kiểm tra nội dung thư mục:

```powershell
dir
```

Thư mục nên có:

```text
src
package.json
vite.config.js
```

---

## Bước 2: Cài dependencies

Chạy lệnh:

```powershell
npm install
```

Lệnh này cài các dependencies cần thiết cho dự án React Admin Web.

---

## Bước 3: Kiểm tra cấu hình backend

Admin Web cần kết nối đến backend API đã deploy trên AWS.

API Gateway endpoint hiện tại của môi trường development là:

```text
https://zsqkp5vpb9.execute-api.ap-southeast-1.amazonaws.com/dev
```

Đảm bảo cấu hình Admin Web đang trỏ đúng về API endpoint này.

Nếu Admin Web sử dụng environment variables, kiểm tra các file như:

```text
.env
.env.local
.env.development
```

File cấu hình cụ thể phụ thuộc vào cách triển khai của dự án.

---

## Bước 4: Chạy Admin Web ở local

Khởi động Admin Web development server:

```powershell
npm run dev
```

Kết quả mong đợi:
![img.png](img.png)

```text
VITE ready
Local: http://127.0.0.1:5173
```

Port thực tế có thể khác tùy Vite. Sử dụng URL được hiển thị trong terminal.

---

## Bước 5: Mở Admin Web trên trình duyệt

Mở URL local của Admin Web trên trình duyệt:

```text
http://127.0.0.1:5173
```

Hoặc sử dụng URL được hiển thị trong terminal.

Kết quả mong đợi:

- Giao diện Admin Web được mở.
- Màn hình đăng nhập hoặc dashboard được hiển thị.
- Admin Web có thể kết nối với backend API đã deploy.

---

## Bước 6: Đăng nhập bằng tài khoản Admin

Sử dụng tài khoản Admin để đăng nhập.

Kết quả mong đợi:

- Admin được xác thực thành công.
- Admin Web nhận được session hoặc token hợp lệ.
- Dashboard hoặc trang hồ sơ đăng ký Merchant được hiển thị.

Nếu đăng nhập thất bại, kiểm tra:

- Thông tin tài khoản Admin.
- Cấu hình Cognito User Pool.
- User có thuộc Admin group hay không.
- Cấu hình API Gateway endpoint.
- Lỗi trong browser console.
- CloudWatch Logs của backend.

---

## Bước 7: Xem hồ sơ đăng ký Merchant

Sau khi đăng nhập, mở trang xem xét hồ sơ đăng ký Merchant.

Admin có thể xem các hồ sơ với trạng thái như:

```text
PENDING
APPROVED
REJECTED
```

Với mỗi hồ sơ, Admin có thể kiểm tra các thông tin như:

- Thông tin người đăng ký.
- Tên doanh nghiệp.
- Loại hình kinh doanh.
- Thông tin đăng ký kinh doanh.
- Tài liệu hoặc hình ảnh đã upload.
- Trạng thái hồ sơ.

---

## Bước 8: Duyệt hồ sơ Merchant

Chọn một hồ sơ Merchant đang chờ duyệt và thực hiện duyệt hồ sơ.

Kết quả mong đợi:

- Trạng thái hồ sơ Merchant được cập nhật trong DynamoDB.
- Người dùng được thêm vào Merchant group trong Cognito.
- Store record được tạo cho Merchant nếu backend workflow hỗ trợ.
- Người dùng có thể đăng xuất và đăng nhập lại để truy cập chức năng Merchant trong Flutter app.

Sau khi được duyệt, người dùng nên mở lại Flutter app và đăng nhập lại để app nhận thông tin Cognito group mới.

---

## Bước 9: Từ chối hồ sơ Merchant

Chọn một hồ sơ Merchant đang chờ duyệt khác và từ chối nếu cần.

Kết quả mong đợi:

- Trạng thái hồ sơ được cập nhật thành `REJECTED`.
- Người dùng không được thêm vào Merchant group.
- Người dùng không thể truy cập chức năng Merchant.

---

## Bước 10: Kiểm tra dữ liệu trong DynamoDB

Mở AWS Management Console và vào:

```text
DynamoDB > Tables > wallet-app-main-dev
```

Kiểm tra bản ghi hồ sơ Merchant đã được cập nhật chưa.

Cấu trúc key cụ thể phụ thuộc vào data model của dự án, nhưng trạng thái hồ sơ cần phản ánh đúng hành động của Admin.

---

## Bước 11: Kiểm tra user group trong Cognito

Mở:

```text
Amazon Cognito > User pools > Users
```

Tìm user đã được duyệt và kiểm tra user group.

Kết quả mong đợi:

- User được duyệt nên nằm trong Merchant group.
- User bị từ chối không được thêm vào Merchant group.

---

## Lỗi thường gặp

### Admin Web không kết nối được backend

Kiểm tra Admin Web có đang dùng đúng API Gateway endpoint không:

```text
https://zsqkp5vpb9.execute-api.ap-southeast-1.amazonaws.com/dev
```

Đồng thời kiểm tra lỗi trong browser console và backend logs trong CloudWatch.

### Đăng nhập Admin thất bại

Nguyên nhân có thể gồm:

- Sai thông tin tài khoản.
- User không nằm trong Admin group.
- Admin Web đang trỏ sai cấu hình Cognito hoặc API.
- JWT token bị thiếu hoặc hết hạn.

### Chức năng Merchant không xuất hiện sau khi được duyệt

User đã được duyệt có thể cần đăng xuất và đăng nhập lại trong Flutter app để app nhận thông tin Cognito group mới.

### Trạng thái hồ sơ không cập nhật

Kiểm tra:

- Request từ API Gateway.
- Lambda logs trong CloudWatch.
- Cập nhật record trong DynamoDB.
- Quyền cập nhật Cognito group.

---

## Kết quả mong đợi

Sau khi hoàn thành phần này:

- Admin Web chạy được ở local.
- Admin có thể đăng nhập.
- Admin có thể xem hồ sơ đăng ký Merchant.
- Admin có thể duyệt hoặc từ chối hồ sơ Merchant.
- Người dùng được duyệt có thể truy cập chức năng Merchant sau khi đăng nhập lại.
- Trạng thái hồ sơ Merchant được cập nhật trong DynamoDB.

