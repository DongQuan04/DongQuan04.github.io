---
title: "Bài blog đã đăng"
date: 2026-04-17
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

---

Phần này liệt kê và giới thiệu các bài blog tôi đã đăng hoặc chuẩn bị trong quá trình thực tập. Các bài blog này được xây dựng dựa trên các bài viết kỹ thuật của AWS, tập trung vào việc tóm tắt ý chính, đánh giá các trường hợp ứng dụng thực tế và phản ánh cách các dịch vụ AWS được áp dụng trong những tình huống thực tế.

### [Blog 1 - Tự động hóa dựng Landing Zone với AWS Transform](3.1-Blog1/)

Bài blog này tóm tắt và đánh giá cách AWS Transform có thể hỗ trợ tự động hóa quá trình tạo Landing Zone bằng hướng dẫn dựa trên AI. Thay vì mất nhiều tuần để thiết kế và thiết lập Landing Zone theo cách thủ công, AWS Transform có thể hỗ trợ thiết kế cấu trúc tài khoản, lập kế hoạch migration wave và tạo mã hạ tầng bằng AWS CDK hoặc Landing Zone Accelerator YAML.

Bài viết cũng nhấn mạnh một số điểm quan trọng như cơ chế Human-In-The-Loop, phân tích môi trường brownfield, cân nhắc chi phí và độ phức tạp khi decommission Landing Zone sau khi triển khai.

### [Blog 2 - Physical AI trong y tế: Tái định nghĩa chăm sóc sức khỏe tại vùng biên](3.2-Blog2/)

Bài blog này giới thiệu một dự án Physical AI trong lĩnh vực y tế được trình diễn tại Mobile World Congress 2026 thông qua sự hợp tác giữa AWS, NVIDIA và AI-SENSE. Dự án tập trung vào việc đưa khả năng theo dõi và chăm sóc bệnh nhân ở mức gần giống bệnh viện đến gần hơn với môi trường tại nhà thông qua các ứng dụng như Virtual Ward và Health Buddy.

Bài viết giải thích cách AI-native network, Agentic AI, Digital Twin Simulation, Amazon Bedrock, Amazon SageMaker, AWS Outposts, Amazon EKS Anywhere và NVIDIA Omniverse có thể phối hợp với nhau để hỗ trợ giám sát sức khỏe theo thời gian thực và hỗ trợ ra quyết định tại vùng biên.

### [Blog 3 - Tự động hóa số hóa hồ sơ y tế với Amazon Bedrock Data Automation và AWS HealthLake](3.3-Blog3/)

Bài blog này tóm tắt và đánh giá cách Amazon Bedrock Data Automation và AWS HealthLake có thể được kết hợp để tự động hóa quá trình số hóa hồ sơ y tế. Giải pháp tập trung vào việc trích xuất dữ liệu từ các tài liệu y tế không có cấu trúc như hồ sơ bệnh án scan, đơn thuốc, ghi chú viết tay và file PDF, sau đó chuyển đổi thông tin đã trích xuất thành dữ liệu y tế được chuẩn hóa.

Bài viết nhấn mạnh vai trò của Generative AI trong data automation, đặc biệt là khả năng biến các tài liệu y tế lộn xộn, không có cấu trúc thành dữ liệu sạch, có cấu trúc và có khả năng tương tác giữa các hệ thống. Bài viết cũng giải thích cách Amazon S3, AWS Lambda, Amazon Bedrock Data Automation và AWS HealthLake có thể phối hợp với nhau trong kiến trúc serverless theo hướng event-driven.

Bài viết cũng phân tích các lợi ích và thách thức quan trọng như chuẩn hóa dữ liệu tự động theo FHIR, giảm nhập liệu thủ công, khả năng mở rộng của serverless, bảo mật dữ liệu y tế, yêu cầu tuân thủ, chi phí xử lý bằng GenAI, độ trễ và nhu cầu kiểm duyệt bởi con người khi làm việc với hồ sơ y tế nhạy cảm.
