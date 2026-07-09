---
title: "Blog 1"
date: 2026-04-17
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

---

# Tự động hóa dựng Landing Zone với AWS Transform

Việc xây dựng Landing Zone là một trong những bước quan trọng khi một tổ chức bắt đầu đưa hệ thống hoặc workload lên AWS. Một Landing Zone được thiết kế tốt thường cần tuân theo các best practices của AWS Well-Architected, bao gồm mô hình nhiều tài khoản, tách biệt môi trường security và workload, phân chia public/private subnet, áp dụng Service Control Policies, ghi log, giám sát và thiết lập các cơ chế quản trị hạ tầng.

Thông thường, việc thiết kế và triển khai một Landing Zone hoàn chỉnh có thể mất nhiều tuần, thậm chí từ 4 đến 12 tuần tùy theo độ phức tạp của tổ chức. Tuy nhiên, với AWS Transform, quá trình này có thể được rút ngắn đáng kể. AWS Transform sử dụng AI để hỗ trợ thiết kế, rà soát và sinh cấu hình hạ tầng phục vụ việc tạo Landing Zone.

## Các điểm chính

### Tương tác bằng ngôn ngữ tự nhiên

Một trong những điểm nổi bật của AWS Transform là khả năng tương tác với AI Agent bằng ngôn ngữ tự nhiên. AI Agent có thể đóng vai trò như một mentor hoặc trợ lý kiến trúc Cloud. Dựa trên thông tin dự án và các migration waves, hệ thống có thể đề xuất cấu trúc tài khoản, organizational units và cấu hình hạ tầng phù hợp.

Người dùng cũng có thể trao đổi, phản biện và yêu cầu điều chỉnh thiết kế trước khi áp dụng. Điều này giúp quá trình lập kế hoạch dễ tiếp cận hơn, đặc biệt với những nhóm đang trong giai đoạn tìm hiểu cách thiết kế môi trường AWS đúng chuẩn.

### Cơ chế Human-In-The-Loop

Mặc dù AWS Transform sử dụng AI, hệ thống không tự động thay đổi hoặc triển khai hạ tầng nếu chưa có sự phê duyệt. Công cụ này hoạt động theo cơ chế Human-In-The-Loop. Điều đó có nghĩa là AI có thể đề xuất giải pháp và sinh mã hạ tầng như AWS CDK hoặc Landing Zone Accelerator YAML, nhưng quyền xem xét, phê duyệt và triển khai cuối cùng vẫn thuộc về quản trị viên.

Điều này rất quan trọng vì các thay đổi hạ tầng Cloud có thể ảnh hưởng đến bảo mật, chi phí và độ ổn định của hệ thống. Việc có con người kiểm tra giúp đảm bảo kiến trúc được tạo ra phù hợp trước khi triển khai.

### Phân tích môi trường Brownfield

AWS Transform cũng có thể làm việc với các môi trường brownfield, tức là những môi trường AWS đã có sẵn tài nguyên và cấu hình. AI Agent có thể quét môi trường hiện tại và phát hiện các cấu hình còn thiếu hoặc chưa tối ưu.

Ví dụ, hệ thống có thể nhận diện việc tổ chức chưa có Sandbox OU, chưa áp dụng đầy đủ Service Control Policies hoặc chưa giới hạn phù hợp các hoạt động của root user. Nhờ đó, đội ngũ kỹ thuật có thể cải thiện môi trường AWS hiện tại thay vì chỉ xây dựng hệ thống mới từ đầu.

### Lưu ý về chi phí

AWS Transform có thể giúp giảm thời gian và công sức khi xây dựng Landing Zone, nhưng người dùng vẫn cần hiểu rằng các dịch vụ AWS liên quan có thể phát sinh chi phí. Một số dịch vụ như AWS Control Tower, AWS Config, AWS CloudTrail, logging, monitoring và các công cụ governance khác có thể được kích hoạt trong quá trình thiết lập.

Vì vậy, khi thực hành trong môi trường lab, người dùng cần theo dõi billing cẩn thận. Nếu quên dọn dẹp tài nguyên không sử dụng, tài khoản AWS có thể phát sinh chi phí ngoài dự kiến.

### Độ phức tạp khi decommission

Một điểm cần chú ý khác là việc hủy hoặc decommission một Landing Zone có thể khá phức tạp. Vì Landing Zone thường bao gồm nhiều tài khoản, log, policy, cấu hình bảo mật và dịch vụ quản trị, việc xóa không đúng cách có thể gây lỗi hoặc làm mất dữ liệu log quan trọng.

Do đó, trước khi triển khai, người dùng nên hiểu rõ kiến trúc và chuẩn bị kế hoạch cleanup hoặc decommission nếu môi trường chỉ dùng cho mục đích thử nghiệm.

## Đánh giá cá nhân

Theo tôi, AWS Transform cho thấy AI có thể hỗ trợ công việc DevOps và Cloud Engineering theo hướng rất thực tế. Thay vì dành quá nhiều thời gian để viết cấu hình hạ tầng thủ công, kỹ sư có thể tập trung nhiều hơn vào việc rà soát thiết kế, kiểm tra yêu cầu bảo mật, đánh giá tác động chi phí và phê duyệt quyết định kiến trúc.

Sự kết hợp giữa AI và Infrastructure as Code rất hữu ích vì giúp giảm các tác vụ thiết lập lặp lại, nhưng vẫn giữ vai trò kiểm soát của con người đối với các quyết định quan trọng. Tuy nhiên, người dùng vẫn cần nắm kiến thức nền tảng về AWS, vì hạ tầng do AI sinh ra vẫn cần được kiểm tra kỹ trước khi triển khai.

Nhìn chung, AWS Transform có thể là một công cụ mạnh cho các tổ chức muốn tăng tốc quá trình migration và thiết lập Landing Zone, nhưng cần được sử dụng cẩn thận cùng với việc theo dõi chi phí, rà soát bảo mật và lập kế hoạch dọn dẹp tài nguyên.

## Hình ảnh

![img_1.png](img_1.png)

![img_2.png](img_2.png)

## Link tham khảo

https://aws.amazon.com/vi/blogs/migration-and-modernization/automate-your-landing-zone-creation-with-aws-transform/

## Hướng dẫn

Bài blog này chủ yếu là bài tóm tắt và đánh giá bài viết của AWS. Tôi không triển khai lab đầy đủ vì việc tạo Landing Zone có thể kích hoạt các dịch vụ như AWS Control Tower, AWS Config và AWS CloudTrail, từ đó có thể phát sinh chi phí.

Thay vào đó, tôi tập trung vào:

- Đọc bài viết gốc của AWS
- Xác định các khái niệm kiến trúc chính
- Hiểu cách AWS Transform hỗ trợ tạo Landing Zone
- Đánh giá lợi ích, rủi ro và các lưu ý về chi phí
- Tổng hợp nhận xét cá nhân về tự động hóa hạ tầng bằng AI

