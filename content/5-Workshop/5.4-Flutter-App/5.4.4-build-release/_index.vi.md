---
title : "Build bản Release"
date : 2026-06-18
weight : 4
chapter : false
pre : " <b> 5.4.4 </b> "
---

#### Build APK cho Android

1. Đảm bảo `ApiConfig.baseUrl` đã trỏ đúng về `ApiUrl` production/staging (không phải endpoint test tạm).
2. Chạy lệnh build APK release:

```bash
flutter build apk --release
```

![Build APK release](/images/5-Workshop/5.4-flutter/build-apk.png)

3. File APK được tạo tại `build/app/outputs/flutter-apk/app-release.apk`, có thể cài trực tiếp lên thiết bị Android thật để demo.

{{% notice info %}}
Nếu muốn build theo kiến trúc CPU cụ thể để giảm dung lượng file, dùng thêm cờ `--split-per-abi`.
{{% /notice %}}

#### Build cho iOS (tùy chọn, cần máy macOS)

```bash
flutter build ios --release
```

Sau đó mở project bằng Xcode (`ios/Runner.xcworkspace`) để archive và cài lên thiết bị test qua TestFlight hoặc cài trực tiếp qua cáp.

#### Cài đặt và kiểm tra trên thiết bị thật

1. Cài file APK (hoặc build iOS) lên điện thoại thật.
2. Lặp lại luồng demo ở mục 5.4.2 và 5.4.3 (đăng ký quán → thêm menu → khách quét QR gọi món → thanh toán) để xác nhận app hoạt động ổn định ngoài môi trường thiết bị thật, không chỉ trên giả lập.

![Demo trên thiết bị thật](/images/5-Workshop/5.4-flutter/real-device-demo.png)

Bạn đã hoàn tất việc build và kiểm thử ứng dụng Flutter trên thiết bị thật, kết nối đầy đủ với backend AWS. Ở bước tiếp theo (5.5), chúng ta sẽ tổng hợp lại toàn bộ kịch bản kiểm thử end-to-end.