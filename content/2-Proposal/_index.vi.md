---
title: "Bản đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

Tại phần này, bạn cần tóm tắt các nội dung trong workshop mà bạn **dự tính** sẽ làm.

# Billo — Ứng dụng gọi món & thanh toán qua QR
## Giải pháp AWS Serverless cho quán ăn/cà phê quy mô nhỏ

### 1. Tóm tắt điều hành

Billo là ứng dụng di động phục vụ mô hình gọi món và thanh toán không tiền mặt tại quán ăn/quán cà phê thông qua mã QR, được xây dựng trong đợt thực tập nhằm giải quyết bài toán vận hành thủ công tại các quán quy mô nhỏ. Ứng dụng phục vụ đồng thời 2 vai trò trên cùng một app — **Chủ quán** (quản lý menu, nhận đơn, xuất hóa đơn) và **Khách hàng** (quét QR, gọi món, thanh toán) — kết nối với backend serverless trên AWS. Nền tảng tận dụng AWS Lambda, API Gateway, DynamoDB và Cognito để đảm bảo chi phí vận hành gần như bằng không khi quán chưa có đơn, đồng thời có khả năng mở rộng theo mô hình pay-per-request khi lượng đơn tăng.

### 2. Tuyên bố vấn đề

*Vấn đề hiện tại*
Các quán ăn/cà phê quy mô nhỏ thường xử lý gọi món và thanh toán hoàn toàn thủ công: nhân viên ghi order bằng giấy, dễ nhầm lẫn món hoặc số bàn, thanh toán tiền mặt khó đối soát cuối ngày. Các nền tảng POS thương mại trên thị trường thường có chi phí thuê bao cao và dư thừa tính năng so với nhu cầu thực tế của một quán nhỏ.

*Giải pháp*
Billo dùng Amazon Cognito xác thực người dùng qua số điện thoại (OTP qua SNS), Amazon API Gateway expose các endpoint REST (`/auth`, `/menu`, `/orders`, `/invoices`, `/payments`, `/wallet`, `/shifts`), 7 AWS Lambda function (Python 3.11) xử lý từng nghiệp vụ riêng biệt dùng chung 1 Lambda Layer, và 5 bảng Amazon DynamoDB (`Merchants`, `Orders`, `Invoices`, `Wallets`, `Shifts`) theo mô hình PAY_PER_REQUEST. Phía client là một ứng dụng Flutter đa vai trò (multi-mode), dùng `provider` quản lý state, hỗ trợ quét QR bàn để gọi món và quét QR hóa đơn để thanh toán. Khác với các nền tảng POS thương mại, Billo được thiết kế gọn nhẹ, phục vụ đúng một luồng nghiệp vụ cốt lõi: quét bàn → gọi món → xuất hóa đơn → thanh toán.

*Lợi ích và hoàn vốn đầu tư (ROI)*
Giải pháp giúp quán loại bỏ hoàn toàn việc ghi order giấy, giảm sai sót giữa bếp và nhân viên phục vụ, đồng thời có dữ liệu đơn hàng tập trung trên DynamoDB phục vụ đối soát cuối ca/cuối ngày qua bảng `Shifts`. Nhờ kiến trúc serverless PAY_PER_REQUEST, chi phí hạ tầng gần như bằng 0 trong giai đoạn quán chưa có nhiều đơn, không phát sinh chi phí máy chủ cố định. Thiết bị cần thiết chỉ là smartphone sẵn có của quán và khách, không cần đầu tư phần cứng POS chuyên dụng.

### 3. Kiến trúc giải pháp

Nền tảng áp dụng kiến trúc AWS Serverless hoàn toàn không máy chủ (serverless), được định nghĩa dưới dạng Infrastructure as Code bằng AWS SAM. Khách hàng và chủ quán tương tác qua cùng một ứng dụng Flutter, gọi REST API qua Amazon API Gateway; các Lambda function xử lý nghiệp vụ và đọc/ghi dữ liệu trên DynamoDB; Amazon Cognito đảm nhiệm xác thực người dùng qua số điện thoại.

![Kiến trúc tổng thể Billo](/images/2-Proposal/Diagram1.png)
*Dịch vụ AWS sử dụng*
- *Amazon Cognito*: Xác thực người dùng qua số điện thoại, xác minh OTP gửi qua SNS.
- *Amazon API Gateway*: Expose các endpoint REST cho toàn bộ nghiệp vụ (`/auth`, `/menu`, `/orders`, `/invoices`, `/payments`, `/wallet`, `/shifts`).
- *AWS Lambda*: 7 function xử lý từng nghiệp vụ riêng biệt (đăng ký/đăng nhập, quản lý menu, quản lý đơn, tạo hóa đơn, xử lý thanh toán, quản lý ví, quản lý ca), dùng chung 1 Lambda Layer để tái sử dụng code.
- *Amazon DynamoDB*: 5 bảng (`Merchants`, `Orders`, `Invoices`, `Wallets`, `Shifts`) theo mô hình PAY_PER_REQUEST, không cần quản lý capacity.

*Thiết kế thành phần*
- *Ứng dụng Flutter*: Chia 3 vùng code chính — `customer/` (màn hình & logic Khách hàng), `merchant/` (màn hình & logic Chủ quán), `shared/` (design system, model, provider quản lý mode). Dùng `mobile_scanner` để quét QR bàn và QR hóa đơn.
- *Luồng Chủ quán*: Đăng ký gian hàng (sinh `merchant_id` từ tên quán), quản lý menu, xác nhận đơn và xuất hóa đơn kèm mã QR thanh toán.
- *Luồng Khách hàng*: Quét mã QR bàn (định dạng `BILLO_TABLE|merchant_id|merchant_name|table_number`) để gọi món, sau đó quét mã QR hóa đơn (định dạng `BILLO|invoice_id|merchant_id|total|table_number`) để xác nhận thanh toán.
- *Backend*: AWS SAM đóng gói và triển khai toàn bộ hạ tầng chỉ với `sam build` và `sam deploy --guided`, đảm bảo môi trường có thể tái tạo nhanh chóng.

### 4. Triển khai kỹ thuật

*Các giai đoạn triển khai*
1. *Chuẩn bị*: Cài đặt AWS CLI, AWS SAM CLI, Python 3.11, Flutter SDK, Docker; cấu hình IAM permission cho tài khoản AWS dùng trong workshop.
2. *Triển khai Backend*: Viết `template.yaml` định nghĩa 5 bảng DynamoDB, 1 Cognito User Pool, 7 Lambda function và 1 Lambda Layer; build và deploy bằng SAM; kiểm tra API bằng cURL/Postman và log CloudWatch.
3. *Triển khai Flutter App*: Cấu hình `ApiConfig.baseUrl` trỏ về API Gateway vừa deploy; chạy thử luồng Chủ quán (tạo gian hàng, thêm menu, sinh QR bàn) và luồng Khách hàng (quét QR, gọi món, thanh toán); build bản release APK/iOS để kiểm thử trên thiết bị thật.
4. *Kiểm thử End-to-End*: Chạy trọn vẹn kịch bản quán tạo menu → khách quét QR gọi món → quán xuất hóa đơn → khách thanh toán qua QR, xác nhận dữ liệu đồng bộ đúng trên DynamoDB.

*Yêu cầu kỹ thuật*
- *Backend*: Kiến thức về AWS Lambda (Python 3.11), API Gateway, DynamoDB (thiết kế partition key/sort key cho 5 bảng), Cognito User Pool xác thực qua số điện thoại, và AWS SAM để định nghĩa hạ tầng dưới dạng code.
- *Frontend*: Flutter với `provider` để quản lý state đa vai trò (ModeProvider, MerchantProvider, CustomerProvider), `http` gọi REST API, `mobile_scanner` quét QR, `shared_preferences` lưu cục bộ, `intl` định dạng tiền tệ/ngày giờ theo `vi_VN`.

### 5. Lộ trình & Mốc triển khai

- *Giai đoạn chuẩn bị*: Cài đặt công cụ, tìm hiểu AWS SAM và cấu trúc dự án Flutter multi-mode.
- *Giai đoạn triển khai Backend*: Xây dựng và deploy toàn bộ hạ tầng serverless, kiểm tra API.
- *Giai đoạn triển khai Frontend*: Kết nối Flutter app với backend, hoàn thiện luồng Chủ quán và Khách hàng, build bản release.
- *Giai đoạn kiểm thử*: Kiểm thử end-to-end trên thiết bị thật, sửa lỗi phát sinh.
- *Giai đoạn tổng kết*: Dọn dẹp tài nguyên AWS, hoàn thiện tài liệu báo cáo thực tập.

### 6. Ước tính ngân sách

Nhờ toàn bộ dịch vụ sử dụng mô hình PAY_PER_REQUEST / pay-as-you-go, chi phí hạ tầng trong giai đoạn workshop và demo là gần như không đáng kể.

*Chi phí hạ tầng ước tính (theo lượt sử dụng thấp trong giai đoạn demo/workshop)*
- AWS Lambda: gần 0 USD/tháng (nằm trong free tier 1 triệu request/tháng).
- Amazon API Gateway: chi phí theo số request thực tế, rất thấp ở quy mô demo.
- Amazon DynamoDB (PAY_PER_REQUEST): chi phí theo số lượt đọc/ghi thực tế, không tốn phí khi không có traffic.
- Amazon Cognito: miễn phí trong giới hạn free tier cho số lượng người dùng hoạt động hàng tháng (MAU) ở quy mô workshop.
- Amazon SNS (gửi OTP): chi phí theo số tin nhắn SMS gửi thực tế.

*Tổng*: Ước tính dưới 5 USD/tháng ở quy mô demo/workshop, có thể kiểm tra chi tiết bằng [AWS Pricing Calculator](https://calculator.aws/).

### 7. Đánh giá rủi ro

*Ma trận rủi ro*
- Sai cấu hình `ApiConfig.baseUrl` khiến app không gọi được API: Ảnh hưởng cao, xác suất trung bình.
- Lỗi xác thực Cognito (OTP không gửi được qua SNS): Ảnh hưởng trung bình, xác suất thấp.
- Quét nhầm/lỗi định dạng mã QR bàn hoặc QR hóa đơn: Ảnh hưởng trung bình, xác suất thấp.
- Vượt giới hạn IAM permission khi deploy/dọn dẹp tài nguyên: Ảnh hưởng thấp, xác suất thấp.

*Chiến lược giảm thiểu*
- Kiểm tra kỹ `ApiUrl` output từ `sam deploy` trước khi cấu hình vào Flutter app.
- Kiểm tra log CloudWatch của từng Lambda function để phát hiện lỗi sớm.
- Chuẩn hóa định dạng QR (`BILLO_TABLE|...` và `BILLO|...`) và kiểm thử với nhiều thiết bị quét khác nhau.
- Áp dụng đúng IAM policy least-privilege sau khi hoàn tất workshop, thay cho policy mở rộng dùng trong lúc học.

*Kế hoạch dự phòng*
- Có thể quay lại gọi API thủ công qua cURL/Postman để xác minh backend hoạt động độc lập với lỗi phía Flutter.
- Dùng `sam delete` để dọn dẹp và triển khai lại toàn bộ stack nếu cần reset môi trường.

### 8. Kết quả kỳ vọng

*Cải tiến kỹ thuật*: Hoàn thiện một hệ thống gọi món – thanh toán không tiền mặt chạy thật trên AWS, thay thế hoàn toàn quy trình ghi order giấy và thanh toán tiền mặt thủ công tại quán.

*Giá trị dài hạn*: Kiến trúc serverless dễ mở rộng thêm nghiệp vụ (khuyến mãi, tích điểm, báo cáo doanh thu theo ca) mà không cần thay đổi hạ tầng nền tảng, đồng thời là nền tảng kiến thức thực hành AWS SAM và Flutter multi-mode có thể tái sử dụng cho các dự án tương lai.