---
title: "Chạy demo Flutter Frontend"
date: 2026-06-29
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

---

Phần này hướng dẫn chạy Flutter frontend của AWS BILLO ở local và kết nối với backend AWS đã deploy.

Flutter app hiện là giao diện chính cho các luồng Customer và Merchant. Trong workshop này, app chưa được host bằng AWS Amplify, Amazon S3 hoặc Amazon CloudFront.

---

## Bước 1: Mở thư mục frontend

Mở PowerShell và chuyển đến thư mục frontend:

```powershell
cd C:\Users\admin\source\repos\AWS_BILLO\frontend
```

Kiểm tra nội dung thư mục:

```powershell
dir
```

Thư mục nên có:

```text
lib
test
config
pubspec.yaml
```

---

## Bước 2: Cài dependencies Flutter

Chạy:

```powershell
flutter pub get
```

Lệnh này tải các package Flutter được khai báo trong `pubspec.yaml`.

---

## Bước 3: Kiểm tra file cấu hình development

Mở file cấu hình:

```text
frontend/config/dev.json
```

File này dùng để truyền các giá trị backend AWS vào Flutter app.

Kiểm tra API Gateway endpoint và thông tin Cognito có khớp với backend đã deploy không.

Ví dụ:

```json
{
  "API_BASE_URL": "https://zsqkp5vpb9.execute-api.ap-southeast-1.amazonaws.com/dev",
  "AWS_REGION": "ap-southeast-1",
  "COGNITO_USER_POOL_ID": "ap-southeast-1_AKc39KB4L"
}
```

Tên key thực tế có thể khác tùy cách triển khai. Không nên đổi tên key nếu Flutter code đang đọc đúng tên key hiện tại.

---

## Bước 4: Chạy Flutter app trên Chrome

Chạy Flutter app với cấu hình AWS development:

```powershell
flutter run -d chrome --dart-define-from-file=config/dev.json
```

Nếu muốn chạy app ở port cố định, dùng:

```powershell
flutter run -d chrome --web-port 8080 --dart-define-from-file=config/dev.json
```

Kết quả mong đợi:
![img.png](img.png)

- Chrome mở Flutter app.
- Ứng dụng hiển thị màn hình xác thực.
- Ứng dụng có thể gọi API Gateway endpoint đã deploy.

---

## Bước 5: Kiểm thử đăng ký Customer

Trong Flutter app:

1. Mở màn hình đăng ký.
2. Nhập số điện thoại và mật khẩu.
3. Gửi form đăng ký.
4. Nhập mã OTP nhận được qua SMS.
5. Xác nhận tài khoản.
6. Đăng nhập bằng tài khoản vừa đăng ký.

Kết quả mong đợi:

- Tài khoản Customer được tạo trong Cognito.
- Profile và ví được tạo trong DynamoDB.
- Màn hình Customer home được mở.

---

## Bước 6: Kiểm thử ví Customer

Sau khi đăng nhập, mở màn hình ví hoặc màn hình home.

Kiểm tra app có thể hiển thị:

- Số dư ví
- Thông tin profile cơ bản
- Lịch sử giao dịch nếu có
- Chức năng quét QR

Nếu dữ liệu ví không hiển thị, kiểm tra:

- API Gateway endpoint trong `config/dev.json`
- JWT token session
- Lambda logs trong CloudWatch
- Profile và wallet records trong DynamoDB

---

## Bước 7: Kiểm thử đăng ký kinh doanh

Từ tài khoản Customer, mở chức năng đăng ký kinh doanh.

Nhập thông tin kinh doanh và upload tài liệu hoặc hình ảnh cần thiết.

Kết quả mong đợi:

- App yêu cầu pre-signed upload URL từ backend.
- File được upload lên S3.
- Hồ sơ đăng ký Merchant được lưu trong DynamoDB với trạng thái `PENDING`.
- Admin có thể xem xét hồ sơ từ Admin Web.

---

## Bước 8: Kiểm thử chức năng Merchant sau khi được duyệt

Sau khi Admin duyệt hồ sơ Merchant, người dùng nên đăng xuất và đăng nhập lại.

Kết quả mong đợi:

- User nhận thông tin group mới.
- Chức năng Merchant xuất hiện.
- Người dùng có thể mở không gian kinh doanh.

Merchant sau đó có thể kiểm thử:

- Tạo danh mục.
- Tạo sản phẩm hoặc dịch vụ.
- Upload ảnh sản phẩm.
- Thiết lập giá, giảm giá và trạng thái sản phẩm.
- Tạo bàn.
- Sinh QR bàn.

---

## Bước 9: Kiểm thử gọi món bằng QR bàn

Sử dụng giao diện Customer:

1. Quét QR bàn.
2. Mở menu của cửa hàng.
3. Chọn sản phẩm.
4. Gửi order.

Kết quả mong đợi:

- Active table được lưu cho Customer.
- Order được tạo hoặc gộp vào bill hiện tại của bàn.
- Merchant có thể xem bill từ giao diện kinh doanh.

---

## Bước 10: Kiểm thử thanh toán QR

Sử dụng giao diện Merchant:

1. Mở bill của bàn.
2. Kiểm tra các sản phẩm đã chọn.
3. Thêm hoặc xóa sản phẩm nếu cần.
4. Tạo QR thanh toán.

Sử dụng giao diện Customer:

1. Quét QR thanh toán.
2. Xem chi tiết bill.
3. Xác nhận thanh toán.

Kết quả mong đợi:

- Ví Customer bị trừ tiền.
- Ví Merchant được cộng tiền.
- Bản ghi transaction được tạo.
- Order được đánh dấu đã thanh toán.
- Active table được xóa khỏi tài khoản Customer.

---

## Lỗi thường gặp

### App không kết nối được backend

Kiểm tra:

```text
frontend/config/dev.json
```

Đảm bảo API Gateway endpoint chính xác.

### Camera không hoạt động trên web

Trình duyệt có thể yêu cầu HTTPS để dùng camera, ngoại trừ localhost. Nếu camera không hoạt động, hãy chạy bằng localhost hoặc dùng nhập mã QR thủ công nếu app hỗ trợ.

### Không nhận được OTP

Cognito SMS có thể vẫn đang ở sandbox mode. Hãy dùng số điện thoại đã verify hoặc cập nhật cấu hình SMS.

### Chức năng Merchant không xuất hiện

Người dùng có thể cần đăng xuất và đăng nhập lại sau khi Admin duyệt để app nhận thông tin Cognito group mới.

---

## Kết quả mong đợi

Sau khi hoàn thành phần này:

- Flutter app chạy local trên Chrome.
- App kết nối được với backend AWS đã deploy.
- Có thể kiểm thử đăng ký và đăng nhập Customer.
- Có thể gửi hồ sơ đăng ký kinh doanh.
- Có thể kiểm thử chức năng Merchant sau khi Admin duyệt.
- Có thể demo luồng gọi món bằng QR bàn và thanh toán QR.

