---
title: "Đề xuất dự án"
date: 2026-06-29
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

---

# AWS BILLO

## Hệ thống ví điện tử và quản lý cửa tiệm serverless trên AWS

### 1. Tóm tắt dự án

AWS BILLO là hệ thống ví điện tử và quản lý cửa tiệm theo kiến trúc serverless được phát triển trong quá trình thực tập. Dự án được thiết kế nhằm minh họa cách sử dụng các dịch vụ AWS để xây dựng một ứng dụng hiện đại, kết hợp ví nội bộ dạng mô phỏng, POS cho cửa tiệm, gọi món tại bàn, thanh toán bằng QR và quản lý cửa hàng.

Hệ thống hỗ trợ ba vai trò chính: Customer, Merchant và Admin. Customer có thể đăng ký bằng số điện thoại, xác thực OTP, đăng nhập, quản lý ví mô phỏng, chuyển tiền, quét QR, gọi món tại bàn, thanh toán bằng QR và xem lịch sử giao dịch. Merchant có thể đăng ký kinh doanh, quản lý thông tin cửa hàng, sản phẩm hoặc dịch vụ, bàn, đơn hàng, bill và phiên thanh toán QR. Admin có thể xem xét hồ sơ đăng ký Merchant, duyệt hoặc từ chối hồ sơ và gán quyền Merchant cho người dùng phù hợp.

Dự án sử dụng kiến trúc serverless tối ưu chi phí trên AWS. Các dịch vụ AWS chính bao gồm Amazon Cognito, Amazon API Gateway, AWS Lambda, Amazon DynamoDB, Amazon S3, Amazon CloudWatch, AWS SAM và AWS CloudFormation. Frontend được phát triển bằng Flutter và hướng đến việc hỗ trợ cả giao diện web và mobile.

Mục tiêu chính của AWS BILLO không phải là xây dựng một hệ thống tài chính dùng cho môi trường production, mà là tạo ra một MVP thực tế để minh họa thiết kế ứng dụng cloud-native, phát triển backend serverless, phân quyền theo vai trò, luồng nghiệp vụ dựa trên QR và tích hợp các dịch vụ AWS.

---

### 2. Vấn đề đặt ra

#### Vấn đề là gì?

Nhiều cửa hàng nhỏ, quán cà phê và cơ sở kinh doanh dịch vụ ăn uống vẫn quản lý sản phẩm, bàn, đơn hàng, bill và thanh toán bằng phương pháp thủ công hoặc bằng nhiều công cụ rời rạc. Điều này có thể dẫn đến một số vấn đề:

- Đơn hàng có thể được ghi thủ công và dễ bị thất lạc hoặc trùng lặp.
- Khó theo dõi đơn hàng theo từng bàn khi khách gọi nhiều lần.
- Khách hàng phải chờ nhân viên đưa menu, ghi món hoặc yêu cầu thanh toán.
- Thông tin cửa hàng, hình ảnh sản phẩm và tài liệu kinh doanh không được lưu trữ nhất quán.
- Thông tin thanh toán không phải lúc nào cũng được liên kết rõ ràng với đơn hàng hoặc bill tương ứng.
- Khách hàng khó xem lại mình đã thanh toán cho nội dung gì.
- Merchant cần một hệ thống đơn giản để quản lý cửa hàng, sản phẩm, dịch vụ, bàn và order.
- Admin cần một quy trình rõ ràng để kiểm tra và phê duyệt hồ sơ đăng ký kinh doanh.
- Các hệ thống POS và ví điện tử thương mại hiện có có thể quá phức tạp hoặc tốn kém đối với cửa hàng nhỏ và dự án học tập.

#### Giải pháp

AWS BILLO cung cấp một ứng dụng thống nhất, kết hợp chức năng ví, thanh toán QR, quản lý cửa hàng và POS trong cùng một hệ thống.

Customer có thể quét QR được gắn tại bàn, mở menu của cửa hàng, chọn sản phẩm và gửi order. Hệ thống ghi nhớ bàn đang hoạt động, giúp Customer có thể mở lại phiên bàn hiện tại mà không cần quét lại mã QR.

Merchant có thể xem bill hiện tại của từng bàn, thêm hoặc xóa món, cập nhật bill và tạo QR thanh toán. Customer quét QR thanh toán bằng ứng dụng BILLO và xác nhận giao dịch.

Sau khi thanh toán thành công, order được đánh dấu là đã thanh toán, các bản ghi giao dịch được tạo và bàn đang hoạt động sẽ được xóa khỏi tài khoản Customer. Để bắt đầu một phiên bàn mới, Customer cần quét mã QR của bàn mới.

Amazon Cognito xử lý đăng ký người dùng, xác thực OTP, đăng nhập và phân quyền theo vai trò. Amazon API Gateway bảo vệ backend API bằng JWT authorization. AWS Lambda xử lý logic nghiệp vụ. Amazon DynamoDB lưu trữ dữ liệu ứng dụng. Amazon S3 lưu trữ tài liệu kinh doanh và hình ảnh sản phẩm.

#### Lợi ích và giá trị mang lại

AWS BILLO mang lại các lợi ích sau:

- Giảm thao tác quản lý cửa hàng và bàn theo cách thủ công.
- Kết nối sản phẩm, order, bill, thanh toán và lịch sử giao dịch trong một hệ thống.
- Cho phép khách hàng gọi món tại bàn bằng QR code.
- Giảm việc phải quét QR lặp lại trong cùng một phiên bàn.
- Hỗ trợ quy trình thanh toán QR và thanh toán tiền mặt.
- Cung cấp lịch sử giao dịch và bill cho người dùng.
- Sử dụng dịch vụ serverless mà không cần quản lý EC2 server.
- Tự động mở rộng theo nhu cầu sử dụng thực tế.
- Sử dụng mô hình trả phí theo mức dùng để giảm chi phí trong giai đoạn phát triển và demo.
- Tạo môi trường thực tế để học kiến trúc serverless trên AWS.

Trong phạm vi thực tập, AWS BILLO được phát triển như một MVP và hệ thống demo. Giá trị chính của dự án là kinh nghiệm thực tế trong việc thiết kế, triển khai, kiểm thử và viết tài liệu cho một ứng dụng serverless trên AWS.

---

### 3. Kiến trúc giải pháp

AWS BILLO sử dụng kiến trúc serverless được triển khai tại AWS Singapore Region: `ap-southeast-1`.

Flutter client giao tiếp với Amazon Cognito để xác thực và Amazon API Gateway để thực hiện các nghiệp vụ được bảo vệ. API Gateway sử dụng Cognito JWT Authorizer để xác minh danh tính người dùng trước khi chuyển request đến AWS Lambda.

Các Lambda function xử lý profile người dùng, đăng ký Merchant, phê duyệt của Admin, quản lý cửa hàng, quản lý sản phẩm, quản lý bàn, order, bill, payment session, chuyển tiền ví, lịch sử giao dịch và upload file.

Amazon DynamoDB lưu trữ profile, wallet, merchant, store, product, table, active table session, order, payment session và transaction. Amazon S3 lưu giấy phép kinh doanh, hình ảnh cửa hàng và hình ảnh sản phẩm thông qua pre-signed URL. Amazon CloudWatch thu thập log của Lambda và hỗ trợ xử lý lỗi.

Kiến trúc này không yêu cầu Amazon EC2, custom VPC, NAT Gateway hoặc RDS vì hệ thống được thiết kế dựa trên các dịch vụ serverless được quản lý hoàn toàn.

### 4. AWS BILLO System Architecture

![img.png](img.png)

1. Người dùng tương tác với Ứng dụng Di động Flutter

2. Ứng dụng gửi yêu cầu đăng ký / đăng nhập

3. Cognito App Client xác thực người dùng với User Pool

4. Cognito gửi yêu cầu gửi mã OTP

5. Cognito định tuyến yêu cầu đăng ký vào User Pool

6. Cognito kích hoạt sự kiện Post Confirmation (sau khi xác nhận thành công)

7. Lambda ghi dữ liệu hồ sơ người dùng vào DynamoDB

8. SNS gửi tin nhắn SMS chứa mã OTP đến điện thoại người dùng

9. Cognito User Pool cấp mã token JWT cho ứng dụng
 
10. Cognito kiểm tra quyền truy cập thông qua các Nhóm (Groups)

11. Admin phê duyệt hoặc từ chối hồ sơ đăng ký của Merchant

12. Ứng dụng đính kèm token JWT vào tiêu đề (header) của các yêu cầu API

13. Ứng dụng gọi các cổng API (endpoints)

14. API Gateway định tuyến các yêu cầu đến đúng hàm Lambda xử lý nghiệp vụ

15. Các hàm Lambda đọc và ghi dữ liệu vào DynamoDB

16. Ứng dụng yêu cầu và nhận đường dẫn S3 Pre-signed URL

17. Ứng dụng tải ảnh trực tiếp lên S3 Bucket

18. Các hàm Lambda xuất nhật ký hoạt động (logs) sang CloudWatch

### Các dịch vụ AWS sử dụng

- **Amazon Cognito**: Quản lý đăng ký bằng số điện thoại, xác thực OTP, đăng nhập, JWT token và các nhóm người dùng Customer, Merchant, Admin.

- **Amazon API Gateway**: Cung cấp HTTP API và xác thực Cognito JWT token trước khi định tuyến request đến backend service.

- **AWS Lambda**: Thực thi logic nghiệp vụ cho profile xác thực, ví, hồ sơ Merchant, quản lý cửa hàng, sản phẩm, bàn, order, bill và thanh toán.

- **Amazon DynamoDB**: Lưu trữ profile, wallet, merchant, store, product, table, order, payment session và transaction record.

- **DynamoDB Idempotency Table**: Ngăn chặn các yêu cầu chuyển tiền hoặc thanh toán QR bị xử lý trùng lặp.

- **Amazon S3**: Lưu trữ giấy phép kinh doanh, hình ảnh cửa hàng và hình ảnh sản phẩm.

- **Amazon CloudWatch**: Lưu log Lambda, theo dõi hành vi backend và hỗ trợ debug.

- **AWS SAM / AWS CloudFormation**: Định nghĩa, build, deploy và quản lý tài nguyên AWS dưới dạng Infrastructure as Code.

- **AWS Amplify Hosting hoặc Amazon S3 + Amazon CloudFront**: Có thể được sử dụng để host ứng dụng Flutter Web.

---

### Thiết kế thành phần

- **Flutter Application**: Cung cấp giao diện cho Customer, Merchant và Admin trên web và mobile.

- **Authentication Module**: Xử lý đăng ký, xác thực OTP, đăng nhập, đăng xuất và điều hướng theo vai trò.

- **Customer Module**: Quản lý số dư ví, chuyển tiền, quét QR, gọi món tại bàn, thanh toán bill, profile và lịch sử giao dịch.

- **Merchant Module**: Quản lý đăng ký kinh doanh, thông tin cửa hàng, sản phẩm, dịch vụ, bàn, order, bill, thanh toán QR và hoàn tiền.

- **Admin Module**: Cho phép Admin xem xét, duyệt hoặc từ chối hồ sơ đăng ký Merchant.

- **API Layer**: API Gateway xác thực JWT token và định tuyến request đến Lambda function.

- **Business Logic Layer**: Lambda function kiểm tra request, phân quyền, tính tổng tiền order và thực hiện giao dịch ví.

- **Data Layer**: DynamoDB lưu dữ liệu ứng dụng bằng partition key và sort key phù hợp với access pattern serverless.

- **File Storage Layer**: S3 lưu hình ảnh và tài liệu được upload thông qua pre-signed URL.

- **Monitoring Layer**: CloudWatch ghi log quá trình chạy Lambda và lỗi ứng dụng.

---

### Các luồng chính của hệ thống

#### Luồng xác thực

1. Người dùng nhập số điện thoại và mật khẩu.
2. Amazon Cognito tạo tài khoản.
3. Cognito gửi OTP đến số điện thoại đã đăng ký.
4. Người dùng xác nhận OTP.
5. Post Confirmation Lambda tạo profile và ví cho người dùng.
6. Cognito trả về JWT token sau khi đăng nhập.
7. Ứng dụng đọc group của người dùng và mở giao diện phù hợp.

#### Luồng đăng ký Merchant

1. Customer hoàn thành form đăng ký kinh doanh.
2. Hình ảnh giấy phép kinh doanh được upload lên Amazon S3 bằng pre-signed URL.
3. Hồ sơ được lưu vào DynamoDB với trạng thái `PENDING`.
4. Admin xem xét hồ sơ.
5. Nếu được duyệt, người dùng được thêm vào Cognito Merchant group.
6. Hệ thống tạo cửa hàng cho Merchant.
7. Merchant có thể truy cập giao diện quản lý cửa hàng.

#### Luồng gọi món tại bàn

1. Merchant tạo bàn.
2. Hệ thống sinh QR code cho bàn.
3. Merchant in và dán QR code lên bàn.
4. Customer quét mã QR của bàn.
5. Ứng dụng lưu bàn đó thành active table của Customer.
6. Customer xem menu và gửi order.
7. Các sản phẩm gọi thêm được gộp vào bill hiện tại của bàn.
8. Customer có thể mở lại bàn đang hoạt động mà không cần quét lại QR.

#### Luồng thanh toán QR

1. Merchant mở bill của bàn.
2. Merchant kiểm tra và chỉnh sửa sản phẩm nếu cần.
3. Merchant chọn **Generate Payment QR**.
4. Hệ thống lưu bill hiện tại.
5. Payment session và QR code được tạo.
6. Customer quét QR thanh toán.
7. Ứng dụng hiển thị cửa hàng, sản phẩm và tổng tiền.
8. Customer xác nhận thanh toán.
9. DynamoDB Transaction trừ ví Customer và cộng ví Merchant.
10. Order và payment session được đánh dấu là đã thanh toán.
11. Bản ghi giao dịch được tạo cho cả hai người dùng.
12. Active table được xóa khỏi tài khoản Customer.

---

### 4. Triển khai kỹ thuật

#### Các giai đoạn triển khai

- **Giai đoạn 1 – Nghiên cứu và phân tích yêu cầu**
  Tìm hiểu yêu cầu dự án, xác định người dùng chính, phân tích luồng nghiệp vụ và nghiên cứu các dịch vụ AWS phù hợp.

- **Giai đoạn 2 – Thiết kế kiến trúc và dữ liệu**
  Thiết kế kiến trúc serverless, access pattern DynamoDB, API, vai trò người dùng, wireframe và mô hình bảo mật.

- **Giai đoạn 3 – Phát triển hạ tầng AWS**
  Tạo Cognito, API Gateway, Lambda, DynamoDB, S3 và CloudWatch bằng AWS SAM.

- **Giai đoạn 4 – Phát triển chức năng cốt lõi**
  Triển khai xác thực, ví, đăng ký kinh doanh, quản lý cửa hàng, quản lý sản phẩm, order, QR payment và lịch sử giao dịch.

- **Giai đoạn 5 – Quản lý bàn và bill**
  Triển khai QR bàn, active table session, gọi món từ menu, chỉnh sửa bill và tạo QR thanh toán.

- **Giai đoạn 6 – Cải thiện UI/UX**
  Phát triển theme Flutter dùng chung và cải thiện giao diện Customer, Merchant, Admin và Authentication.

- **Giai đoạn 7 – Kiểm thử và triển khai**
  Chạy kiểm thử frontend/backend, validate SAM template, build ứng dụng, deploy AWS stack và kiểm tra các luồng chính.

#### Yêu cầu kỹ thuật

- Flutter SDK để phát triển ứng dụng web và mobile.
- Node.js 22.x cho AWS Lambda function.
- AWS SAM CLI để build và deploy.
- Amazon Cognito User Pool với các group Customer, Merchant và Admin.
- API Gateway HTTP API với JWT Authorizer.
- DynamoDB table sử dụng on-demand capacity.
- DynamoDB Transaction cho chuyển tiền ví và thanh toán QR.
- S3 pre-signed URL để upload file trực tiếp.
- Sinh QR code và quét QR bằng camera trong Flutter.
- Idempotency key cho các request chuyển tiền và thanh toán.
- CloudWatch Logs để giám sát backend.
- HTTPS cho camera access khi chạy ngoài localhost.

---

### 5. Timeline & Milestones

#### Timeline dự án

- **Tuần 7**: Nghiên cứu yêu cầu dự án, dịch vụ AWS, vai trò người dùng và business flow.

- **Tuần 8**: Thiết kế kiến trúc hệ thống, wireframe, mô hình dữ liệu DynamoDB và định hướng API.

- **Tuần 9**: Tạo dự án Flutter, hạ tầng AWS SAM, Cognito, API Gateway, DynamoDB và S3.

- **Tuần 10**: Triển khai xác thực, chuyển tiền ví, đăng ký Merchant, phê duyệt Admin, quản lý cửa hàng và quản lý sản phẩm.

- **Tuần 11**: Triển khai quản lý bàn, gọi món bằng QR bàn, chỉnh sửa bill, QR payment, lịch sử giao dịch và cải thiện UI/UX.

- **Tuần 12**: Hoàn thiện tích hợp hệ thống, kiểm thử, deploy AWS, tài liệu, báo cáo cuối kỳ và chuẩn bị demo.

---

### 6. Ước tính chi phí

Dự án sử dụng các dịch vụ serverless trả phí theo mức sử dụng. Chi phí thực tế phụ thuộc vào số lượng người dùng, API request, tin nhắn OTP, dung lượng hình ảnh lưu trữ, data transfer và CloudWatch Logs.

Một kịch bản demo có thể bao gồm:

- Dưới 100 monthly active users.
- Khoảng 10.000 API requests mỗi tháng.
- Dưới 100.000 Lambda executions mỗi tháng.
- Dưới 1 GB dữ liệu DynamoDB.
- Dưới 2 GB hình ảnh và tài liệu trên S3.
- Lưu lượng Flutter Web giới hạn.
- Một số điện thoại đã được xác minh để kiểm thử SMS OTP.

#### Chi phí hạ tầng dự kiến

- **AWS Lambda**: Dự kiến nằm trong mức miễn phí hoặc chi phí rất thấp với lưu lượng demo.

- **Amazon API Gateway HTTP API**: Khoảng $0.01–$0.05 mỗi tháng với số lượng request nhỏ.

- **Amazon DynamoDB**: Khoảng $0.10–$1.00 mỗi tháng với mức đọc/ghi on-demand thấp.

- **Amazon S3**: Khoảng $0.05–$0.50 mỗi tháng cho tập hình ảnh và tài liệu nhỏ.

- **Amazon Cognito**: Dự kiến chi phí thấp hoặc không đáng kể với số lượng monthly active users nhỏ. SMS được tính phí riêng.

- **Amazon CloudWatch**: Khoảng $0.00–$1.00 mỗi tháng nếu kiểm soát log volume và log retention.

- **Frontend Hosting**: Khoảng $1.00–$5.00 mỗi tháng tùy theo phương án hosting, tần suất build, dung lượng và traffic.

- **SMS OTP**: Chi phí biến động theo quốc gia nhận tin nhắn, số lượng tin nhắn và cấu hình AWS messaging.

#### Tổng chi phí dự kiến

- Môi trường backend demo: khoảng **$1–$5 mỗi tháng**, chưa bao gồm SMS OTP.
- Backend có hosting Flutter Web: khoảng **$2–$10 mỗi tháng**, chưa bao gồm SMS OTP.
- Chi phí phát triển có thể gần bằng 0 nếu sử dụng AWS credits hoặc các mức miễn phí phù hợp.

Các con số trên là ước tính sơ bộ và nên được kiểm tra lại bằng AWS Pricing Calculator trước khi triển khai production.

[AWS Pricing Calculator](https://calculator.aws/)

---

### 7. Đánh giá rủi ro

#### Ma trận rủi ro

- **Giới hạn SMS Sandbox**: Ảnh hưởng cao, xác suất cao trong giai đoạn phát triển.
- **Request thanh toán bị trùng**: Ảnh hưởng cao, xác suất trung bình.
- **Số dư ví không đủ**: Ảnh hưởng trung bình, xác suất trung bình.
- **Payment QR hết hạn**: Ảnh hưởng trung bình, xác suất trung bình.
- **Truy cập API trái phép**: Ảnh hưởng cao, xác suất thấp.
- **Lộ tài liệu cá nhân/doanh nghiệp**: Ảnh hưởng cao, xác suất thấp.
- **Chi phí Cloud tăng**: Ảnh hưởng trung bình, xác suất thấp trong giai đoạn demo.
- **Lỗi quyền camera trên trình duyệt**: Ảnh hưởng trung bình, xác suất trung bình.

#### Biện pháp giảm thiểu

- Xác minh số điện thoại test khi Cognito SMS còn ở sandbox.
- Yêu cầu quyền production SMS trước khi triển khai công khai.
- Sử dụng Cognito JWT Authorizer và kiểm tra vai trò trong các API được bảo vệ.
- Sử dụng DynamoDB Transaction cho chuyển tiền ví và thanh toán QR.
- Sử dụng idempotency key để ngăn giao dịch trùng.
- Tính lại giá sản phẩm ở backend thay vì tin hoàn toàn vào frontend.
- Thiết lập thời gian hết hạn cho payment session.
- Vô hiệu hóa QR thanh toán cũ khi bill thay đổi.
- Sử dụng S3 pre-signed URL có thời gian hết hạn giới hạn.
- Cấu hình CloudWatch log retention và AWS Budget alerts.
- Yêu cầu HTTPS và cung cấp phương án upload ảnh hoặc nhập mã thủ công khi không thể quét QR.

#### Kế hoạch dự phòng

- Dùng số điện thoại đã xác minh trong sandbox để demo.
- Cho phép nhập payment session thủ công nếu camera không khả dụng.
- Giữ lại transaction và order record để phục vụ xử lý lỗi.
- Sử dụng CloudFormation rollback nếu deploy AWS thất bại.
- Khôi phục dữ liệu DynamoDB bằng Point-in-Time Recovery khi cần thiết.
- Tạm khóa API hoặc chức năng thanh toán bị ảnh hưởng nếu phát hiện dữ liệu giao dịch không nhất quán.

---

### 8. Kết quả mong đợi

#### Cải thiện kỹ thuật

- Thay thế việc quản lý cửa hàng và bàn thủ công bằng một ứng dụng tập trung.
- Cung cấp xác thực an toàn và phân quyền theo vai trò.
- Hỗ trợ chuyển tiền ví và thanh toán QR theo kiến trúc serverless.
- Kết nối order theo bàn, bill, thanh toán và lịch sử giao dịch.
- Cho phép Customer mở lại bàn đang hoạt động mà không cần quét lại QR.
- Ngăn xử lý thanh toán trùng lặp.
- Hỗ trợ upload trực tiếp hình ảnh và tài liệu lên Amazon S3.
- Cung cấp backend có khả năng mở rộng mà không cần quản lý EC2 server.

#### Sản phẩm bàn giao

- Ứng dụng Flutter web và mobile.
- Backend serverless bằng AWS SAM.
- Xác thực và quản lý vai trò bằng Cognito.
- Giao diện Customer, Merchant và Admin.
- Chức năng store, product, table, order, bill, wallet và payment.
- Mô hình dữ liệu DynamoDB.
- Lưu trữ hình ảnh và tài liệu trên S3.
- Sơ đồ kiến trúc và tài liệu API.
- Kiểm thử frontend và backend.
- Môi trường triển khai AWS development.
- Báo cáo cuối kỳ và kịch bản demo.

#### Giá trị dài hạn

AWS BILLO có thể đóng vai trò nền tảng cho một hệ thống ví điện tử và quản lý cửa hàng nhỏ ở quy mô lớn hơn.

Các hướng cải tiến trong tương lai có thể bao gồm:

- Production SMS access.
- Lưu token an toàn và tự động refresh token.
- Chức năng quên mật khẩu.
- Push notifications.
- Báo cáo doanh thu cho Merchant.
- Xuất bill PDF.
- Quản lý tồn kho.
- CI/CD pipelines.
- Hosting frontend production.
- Phát hiện gian lận và giới hạn giao dịch.
- Audit logs và đối soát tài chính.

