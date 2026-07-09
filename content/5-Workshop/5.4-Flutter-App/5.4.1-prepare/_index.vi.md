---
title : "Chuẩn bị Flutter App"
date : 2026-06-17
weight : 1
chapter : false
pre : " <b> 5.4.1. </b> "
---

#### Cài đặt dependencies

1. Mở terminal tại thư mục gốc project Flutter, chạy:

```bash
flutter pub get
```

![flutter pub get](/images/5-Workshop/5.4-flutter/pub-get.png)

2. Xác nhận các package chính đã có trong `pubspec.yaml`:

+ `provider` — quản lý state (ModeProvider, MerchantProvider, CustomerProvider).
+ `http` — gọi REST API backend.
+ `mobile_scanner` — quét mã QR.
+ `shared_preferences` — lưu cục bộ (user id, mode đã chọn, thông tin quán).
+ `intl` — định dạng tiền tệ và ngày giờ theo `vi_VN`.
+ `google_fonts` — typography (Baloo 2, Inter).

#### Cấu hình kết nối tới Backend

1. Mở file `lib/shared/design.dart`, tìm class `ApiConfig`.
2. Thay giá trị `baseUrl` bằng `ApiUrl` bạn đã lấy được từ output của `sam deploy` ở mục 5.3.1:

```dart
class ApiConfig {
  static const baseUrl = 'https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com/Prod';

  static const menuEndpoint     = '/menu';
  static const invoicesEndpoint = '/invoices';
  static const ordersEndpoint   = '/orders';
  static const paymentsEndpoint = '/payments';
}
```

![Cấu hình ApiConfig](/images/5-Workshop/5.4-flutter/api-config.png)

{{% notice warning %}}
Nếu quên cập nhật `baseUrl`, app sẽ không gọi được API và hiển thị lỗi khi tải menu hoặc gửi đơn — hãy kiểm tra lại bước này đầu tiên nếu app báo lỗi kết nối.
{{% /notice %}}

#### Chạy app trên thiết bị/giả lập

1. Kết nối thiết bị Android/iOS hoặc mở giả lập, kiểm tra thiết bị đã sẵn sàng:

```bash
flutter devices
```

2. Chạy ứng dụng:

```bash
flutter run
```

![Flutter run](/images/5-Workshop/5.4-flutter/flutter-run.png)

3. Ứng dụng khởi động vào màn hình **ModeSelectScreen**, cho phép chọn vai trò **Chủ quán** hoặc **Khách hàng**.

![Màn hình chọn vai trò](/images/5-Workshop/5.4-flutter/mode-select.png)

Ở bước tiếp theo (5.4.2), chúng ta sẽ đóng vai **Chủ quán** để tạo một gian hàng và thêm menu trước khi thử luồng Khách hàng.