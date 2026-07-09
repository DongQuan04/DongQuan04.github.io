---
title: "Giám sát và dọn dẹp tài nguyên"
date: 2026-06-29
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

---

Phần này giải thích cách theo dõi backend AWS BILLO và dọn dẹp tài nguyên AWS sau khi hoàn thành workshop.

Monitoring rất quan trọng vì backend chạy trên các dịch vụ serverless như API Gateway, Lambda, DynamoDB, S3 và Cognito. Khi xảy ra lỗi, CloudWatch Logs và AWS Console có thể giúp xác định nguyên nhân.

Cleanup cũng quan trọng để tránh phát sinh chi phí AWS không cần thiết sau khi kiểm thử.

---

## 1. Monitoring với CloudWatch Logs

AWS BILLO sử dụng Amazon CloudWatch Logs để theo dõi hoạt động backend và debug lỗi Lambda/API.

CloudWatch Logs có thể giúp kiểm tra:

- Kết quả thực thi Lambda.
- Lỗi API request.
- Vấn đề xác thực.
- Lỗi đọc/ghi DynamoDB.
- Lỗi upload S3.
- Lỗi thanh toán và giao dịch.
- Lỗi trong quy trình đăng ký kinh doanh.

---

## Bước 1: Mở CloudWatch Logs

Mở AWS Management Console và vào:

```text id="sw7h9q"
CloudWatch > Logs > Log groups
```

![img.png](img.png)
Tìm các log group liên quan đến Lambda functions của AWS BILLO.

Lambda log group thường có định dạng:

```text id="z6iv3p"
/aws/lambda/<function-name>
```

---

## Bước 2: Mở Lambda Log Group

Chọn một Lambda log group.

Sau đó mở log stream mới nhất.

Một log stream thường có các thông tin như:

```text id="mmwe2l"
START RequestId
Function logs
Error messages
END RequestId
REPORT RequestId
```

Các log này được dùng để kiểm tra Lambda function chạy thành công hay bị lỗi.

---

## Bước 3: Debug lỗi API

Nếu frontend nhận lỗi API, kiểm tra Lambda logs liên quan trong CloudWatch.

Các lỗi API thường gặp gồm:

| Lỗi                         | Nguyên nhân có thể                                     |
| --------------------------- | ------------------------------------------------------ |
| `401 Unauthorized`          | Thiếu JWT token, token hết hạn hoặc token không hợp lệ |
| `403 Forbidden`             | User không có đúng quyền cần thiết                     |
| `400 Bad Request`           | Thiếu hoặc sai dữ liệu request                         |
| `404 Not Found`             | Sai API path hoặc không tìm thấy record                |
| `500 Internal Server Error` | Lỗi Lambda, lỗi DynamoDB hoặc exception chưa xử lý     |

Khi debug, cần kiểm tra:

- API Gateway endpoint.
- Request payload.
- JWT token.
- Log của Lambda function.
- Record trong DynamoDB.
- Vai trò người dùng trong Cognito.

---

## Bước 4: Kiểm tra dữ liệu DynamoDB

Mở:

```text id="n9xs4v"
DynamoDB > Tables > wallet-app-main-dev
```
![img_1.png](img_1.png)

Kiểm tra các record mong đợi sau khi chạy demo.

Các record quan trọng có thể gồm:

- User profile.
- Wallet.
- Merchant application.
- Store.
- Product hoặc service.
- Table.
- Order.
- Bill.
- Payment session.
- Transaction.

Nếu một chức năng hoạt động không đúng, hãy so sánh dữ liệu mong đợi với dữ liệu thực tế trong DynamoDB.

---

## Bước 5: Kiểm tra file upload trên S3

Mở:

```text id="te7jxs"
Amazon S3 > Buckets
```

Tìm upload bucket được tạo bởi backend stack.

Kiểm tra các file upload như:

- Tài liệu đăng ký kinh doanh.
- Hình ảnh cửa hàng.
- Hình ảnh sản phẩm hoặc dịch vụ.

Nếu upload thất bại, kiểm tra:

- Pre-signed URL có được tạo thành công không.
- Quyền của S3 bucket.
- Loại file và kích thước file.
- Lỗi trong browser console.
- Lambda logs trong CloudWatch.

---

## Bước 6: Kiểm tra Cognito Users và Groups

Mở:

```text id="el7vfn"
Amazon Cognito > User pools > Users
```
![img_2.png](img_2.png)

Kiểm tra tài khoản người dùng được tạo trong quá trình demo.

Xác minh:

- User tồn tại.
- User có trạng thái confirmed.
- Số điện thoại chính xác.
- User nằm trong group phù hợp.

Với Merchant đã được duyệt, kiểm tra user đã được thêm vào group:

```text id="qflssi"
Merchant
```

Với tài khoản Admin, kiểm tra user thuộc group:

```text id="hzux3y"
Admin
```

---

## Bước 7: Kiểm tra CloudFormation Stack

Mở:

```text id="8zjufd"
CloudFormation > Stacks
```

Tìm stack:

```text id="l52x26"
wallet-app-backend-dev
```

Kiểm tra trạng thái stack.

Các trạng thái thành công thường là:

```text id="6fxkty"
CREATE_COMPLETE
UPDATE_COMPLETE
```
![img_3.png](img_3.png)

Nếu stack bị lỗi trong quá trình deploy, mở tab **Events** để xem tài nguyên nào bị lỗi.

---

## 2. Cleanup tài nguyên AWS

Sau khi hoàn thành workshop, xóa các tài nguyên backend đã deploy nếu không còn cần sử dụng.

Vì backend được deploy bằng AWS SAM, có thể cleanup bằng SAM hoặc CloudFormation.

---

## Cách 1: Xóa tài nguyên bằng SAM

Mở PowerShell và chuyển đến thư mục backend:

```powershell id="uzow4p"
cd C:\Users\admin\source\repos\AWS_BILLO\backend
```

Chạy:

```powershell id="s77lee"
sam delete
```

SAM sẽ hỏi xác nhận trước khi xóa stack.

Kết quả mong đợi:

- CloudFormation stack được xóa.
- Các tài nguyên do stack quản lý được xóa.
- Backend resources như Lambda, API Gateway, DynamoDB, S3 và Cognito có thể bị xóa tùy theo cấu hình stack.

---

## Cách 2: Xóa stack từ AWS Console

Mở:

```text id="snz19p"
CloudFormation > Stacks
```

Chọn:

```text id="a6dosm"
wallet-app-backend-dev
```

Chọn:

```text id="xo8n66"
Delete
```
![img_5.png](img_5.png)

Xác nhận thao tác xóa.

Kết quả mong đợi:

- CloudFormation bắt đầu xóa stack.
- Stack chuyển sang trạng thái `DELETE_IN_PROGRESS`.
- Khi hoàn tất, stack sẽ biến mất.

---

## Lưu ý quan trọng khi cleanup

Trước khi xóa tài nguyên, cần đảm bảo không còn cần dữ liệu test.

Một số tài nguyên có thể chứa dữ liệu demo quan trọng, như:

- Tài khoản người dùng trong Cognito.
- Wallet và transaction records trong DynamoDB.
- Tài liệu đăng ký kinh doanh và hình ảnh sản phẩm trong S3.
- Lambda logs trong CloudWatch.

Nếu dự án vẫn đang được phát triển, không nên xóa backend stack. Thay vào đó, chỉ nên xóa dữ liệu test không còn cần thiết.

---

## Tùy chọn: Chỉ xóa dữ liệu test

Nếu muốn giữ backend nhưng xóa dữ liệu test, kiểm tra và xóa thủ công các record test trong:

```text id="6zd7wd"
DynamoDB > Tables > wallet-app-main-dev
```

Có thể xóa các file test đã upload trong:

```text id="ptqmxt"
Amazon S3 > Buckets
```

Cần cẩn thận khi xóa dữ liệu vì có thể ảnh hưởng đến user demo và các flow test hiện có.

---

## Tùy chọn: Kiểm soát chi phí CloudWatch Logs

CloudWatch Logs có thể tiếp tục lưu log sau khi kiểm thử.

Để giảm chi phí, cấu hình log retention:

```text id="ooe89d"
CloudWatch > Logs > Log groups > Select log group > Retention settings
```
![img_6.png](img_6.png)

Thời gian lưu log khuyến nghị cho môi trường development:

```text id="4c0k7v"
7 days
14 days
30 days
```
![img_7.png](img_7.png)


Không nên để log development được lưu vĩnh viễn nếu không cần thiết.

---

## Tùy chọn: Cấu hình AWS Budget Alert

Để tránh phát sinh chi phí AWS ngoài ý muốn, nên cấu hình AWS Budget alert.

Mở:

```text id="aa1geq"
Billing and Cost Management > Budgets
```

Tạo monthly cost budget.

Ví dụ:

```text id="rs0nm5"
Budget amount: 5 USD
Alert threshold: 80%
Email notification: your email address
```

Điều này giúp phát hiện sớm chi phí bất thường từ các dịch vụ như SMS, CloudWatch Logs, S3 storage hoặc API usage.

---

## Lỗi cleanup thường gặp

### Stack deletion thất bại

Nguyên nhân có thể gồm:

- S3 bucket chưa rỗng.
- DynamoDB table có deletion protection.
- Thiếu quyền IAM.
- Một tài nguyên đã bị chỉnh sửa thủ công ngoài CloudFormation.

Kiểm tra CloudFormation Events để xác định tài nguyên bị lỗi.

### Không xóa được S3 bucket

Nếu bucket chưa rỗng, hãy xóa các object trong bucket trước.

Sau đó chạy lại `sam delete` hoặc xóa stack từ CloudFormation.

### Tài nguyên vẫn còn sau khi xóa

Một số tài nguyên có thể mất vài phút để biến mất khỏi console.

Refresh AWS Console và kiểm tra CloudFormation stack events.

---

## Kết quả mong đợi

Sau khi hoàn thành phần này:

- Có thể kiểm tra log Lambda trong CloudWatch.
- Có thể xác minh dữ liệu trong DynamoDB.
- Có thể kiểm tra file upload trong S3.
- Có thể kiểm tra user và group trong Cognito.
- Có thể xóa backend stack bằng SAM hoặc CloudFormation.
- Có thể giảm chi phí AWS không cần thiết sau workshop.

