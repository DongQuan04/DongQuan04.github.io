---
title: "Blog 3"
date: 2026-04-17
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

---

# Tự động hóa số hóa hồ sơ y tế với Amazon Bedrock Data Automation và AWS HealthLake

## Tổng quan dự án

Bài blog này giới thiệu một giải pháp serverless giúp tự động hóa quá trình số hóa hồ sơ y tế bằng Amazon Bedrock Data Automation và AWS HealthLake. Dự án tập trung vào việc chuyển đổi các tài liệu y tế không có cấu trúc, chẳng hạn như hồ sơ bệnh án scan, đơn thuốc, ghi chú viết tay và file PDF, thành dữ liệu y tế có cấu trúc theo chuẩn quốc tế.

Trong nhiều tổ chức y tế, hồ sơ bệnh án vẫn còn tồn tại dưới dạng giấy, bản scan hoặc file PDF. Những tài liệu này chứa nhiều thông tin lâm sàng quan trọng, nhưng lại khó tìm kiếm, khó phân tích và khó tích hợp với các hệ thống y tế hiện đại. Việc nhập liệu thủ công thường tốn thời gian, chi phí cao và dễ xảy ra sai sót, đặc biệt khi cần xử lý một lượng lớn hồ sơ bệnh án cũ.

Giải pháp này kết hợp Generative AI, kiến trúc serverless và chuẩn dữ liệu y tế. Amazon Bedrock Data Automation được dùng để trích xuất thông tin lâm sàng có ý nghĩa từ tài liệu y tế. AWS Lambda xử lý và ánh xạ dữ liệu đã trích xuất sang định dạng FHIR. AWS HealthLake lưu trữ dữ liệu đã chuẩn hóa để có thể truy vấn và sử dụng bởi các ứng dụng y tế.

Mục tiêu chính của giải pháp là biến các tài liệu y tế lộn xộn, không có cấu trúc thành dữ liệu sạch, có cấu trúc và có khả năng tương tác giữa các hệ thống.

## Ứng dụng của dịch vụ

Giải pháp này có thể được áp dụng trong nhiều tình huống của ngành y tế.

Trước hết, hệ thống giúp bệnh viện và phòng khám số hóa hồ sơ bệnh án cũ. Thay vì phải đọc và nhập liệu thủ công từ tài liệu scan, hệ thống có thể tự động trích xuất các trường thông tin quan trọng như thông tin bệnh nhân, chẩn đoán, thuốc, chỉ số sinh tồn và kết quả xét nghiệm.

Thứ hai, giải pháp hỗ trợ chuẩn hóa dữ liệu y tế bằng FHIR, viết tắt của Fast Healthcare Interoperability Resources. Đây là một chuẩn dữ liệu quan trọng trong ngành y tế, giúp các hệ thống khác nhau có thể trao đổi và hiểu thông tin bệnh nhân dễ dàng hơn.

Thứ ba, hệ thống hỗ trợ phân tích dữ liệu y tế tốt hơn. Khi dữ liệu được lưu trong AWS HealthLake, việc truy vấn hồ sơ bệnh nhân, phân tích bệnh lý, xem lịch sử dùng thuốc và phục vụ các dự án AI hoặc phân tích dữ liệu trong tương lai sẽ thuận tiện hơn.

Thứ tư, giải pháp giúp thu hẹp khoảng cách giữa hồ sơ bệnh án giấy truyền thống và hệ thống y tế số hiện đại. Điều này đặc biệt hữu ích với các tổ chức y tế muốn hiện đại hóa hạ tầng dữ liệu mà không phải tự xây dựng mô hình machine learning từ đầu.

## Cấu trúc hoạt động của hệ thống

Hệ thống hoạt động theo kiến trúc event-driven và serverless.

### Amazon S3 là lớp lưu trữ tài liệu

Amazon S3 được dùng để lưu trữ file đầu vào và file đầu ra. Khi một hồ sơ y tế scan hoặc file PDF được upload vào S3 input bucket, hệ thống sẽ tự động kích hoạt pipeline xử lý.

S3 cũng lưu kết quả đầu ra do Amazon Bedrock Data Automation tạo ra trước khi dữ liệu tiếp tục được xử lý và chuyển đổi sang định dạng dữ liệu y tế.

### Amazon Bedrock Data Automation là lớp trí tuệ

Amazon Bedrock Data Automation được sử dụng để trích xuất thông tin lâm sàng có cấu trúc từ các tài liệu y tế không có cấu trúc. Thay vì chỉ dựa vào OCR truyền thống, hệ thống có thể hiểu bố cục tài liệu và ngữ cảnh y khoa tốt hơn.

Thông tin được trích xuất có thể bao gồm nhân khẩu học bệnh nhân, chẩn đoán, thuốc, chỉ số sinh tồn, kết quả xét nghiệm và các trường dữ liệu lâm sàng khác. Điều này làm cho dữ liệu hữu ích hơn so với việc chỉ trích xuất văn bản thô.

### AWS Lambda là lớp xử lý

AWS Lambda được dùng để điều phối và chuyển đổi dữ liệu trong pipeline.

Một Lambda function có thể được kích hoạt khi tài liệu y tế mới được upload lên S3. Function này bắt đầu job xử lý của Amazon Bedrock Data Automation.

Một Lambda function khác đọc kết quả JSON đã trích xuất và chuyển đổi dữ liệu sang các tài nguyên FHIR R4. Việc tách chức năng như vậy giúp hệ thống dễ bảo trì, kiểm thử và mở rộng hơn.

### AWS HealthLake là kho dữ liệu y tế

AWS HealthLake lưu dữ liệu y tế đã xử lý theo định dạng FHIR R4. Sau khi dữ liệu được import, hệ thống có thể kiểm tra tính hợp lệ, lập chỉ mục và cho phép truy vấn thông qua các FHIR API tiêu chuẩn.

Nhờ đó, các ứng dụng y tế, công cụ phân tích hoặc hệ thống AI trong tương lai có thể truy cập thông tin bệnh nhân có cấu trúc một cách hiệu quả hơn.

### Giám sát và bảo mật

Amazon CloudWatch có thể được dùng để theo dõi log thực thi Lambda và lỗi của pipeline. AWS CloudTrail có thể hỗ trợ audit hoạt động của các dịch vụ, trong khi AWS KMS có thể giúp mã hóa dữ liệu nhạy cảm khi lưu trữ.

Vì dữ liệu y tế là loại dữ liệu rất nhạy cảm, mọi triển khai thực tế đều cần có kiểm soát bảo mật nghiêm ngặt, quản lý quyền truy cập, mã hóa, audit log và đánh giá tuân thủ.

## Ưu điểm

Giải pháp này mang lại nhiều ưu điểm quan trọng.

Thứ nhất, hệ thống giải quyết một bài toán khó trong ngành y tế: hồ sơ y tế không có cấu trúc. Hồ sơ scan, đơn thuốc và tài liệu viết tay thường lộn xộn và khó xử lý. Amazon Bedrock Data Automation có thể cải thiện khả năng trích xuất dữ liệu nhờ hiểu được cả cấu trúc tài liệu và ngữ cảnh y khoa.

Thứ hai, giải pháp hỗ trợ chuẩn hóa dữ liệu tự động. Dữ liệu thô sau khi được trích xuất có thể được chuyển đổi sang chuẩn FHIR, giúp các hệ thống y tế khác nhau dễ trao đổi và hiểu cùng một thông tin bệnh nhân.

Thứ ba, kiến trúc của hệ thống là serverless. Bằng cách sử dụng S3, Lambda, Amazon Bedrock Data Automation và AWS HealthLake, hệ thống có thể tự động mở rộng theo khối lượng công việc. Điều này cũng giúp giảm gánh nặng vận hành vì không cần quản lý server thủ công.

Thứ tư, giải pháp hỗ trợ các dự án phân tích dữ liệu y tế và AI trong tương lai. Khi hồ sơ y tế được chuyển thành dữ liệu có cấu trúc, chúng có thể trở thành nền tảng quan trọng cho tìm kiếm lâm sàng, báo cáo, xem lịch sử bệnh nhân và các use case machine learning.

## Hạn chế và thách thức

Mặc dù giải pháp có nhiều tiềm năng, nó cũng tồn tại một số hạn chế và thách thức.

Thách thức đầu tiên là bảo mật dữ liệu và tuân thủ quy định. Dữ liệu y tế có thể bao gồm thông tin định danh cá nhân và thông tin sức khỏe được bảo vệ. Khi triển khai thực tế, các tổ chức y tế cần đánh giá kỹ yêu cầu pháp lý, quy định và tuân thủ trước khi đưa dữ liệu bệnh nhân lên cloud.

Thách thức thứ hai là chi phí. Việc xử lý bằng Generative AI có thể phát sinh chi phí lớn nếu hệ thống phải xử lý hàng triệu trang tài liệu hoặc kho hồ sơ bệnh án lịch sử với quy mô lớn. Tổ chức cần có chiến lược lọc dữ liệu, phân loại hồ sơ và giám sát ngân sách trước khi chạy hệ thống ở quy mô lớn.

Thách thức thứ ba là độ trễ. Việc xử lý hồ sơ y tế thông qua AI model sẽ chậm hơn so với việc đọc dữ liệu có cấu trúc trực tiếp từ database. Vì vậy, kiến trúc này phù hợp hơn với xử lý bất đồng bộ hoặc batch processing, thay vì các luồng ra quyết định lâm sàng real-time.

Một thách thức khác là kiểm chứng độ chính xác. Dù AI có thể trích xuất thông tin có cấu trúc, dữ liệu y tế vẫn cần được kiểm tra cẩn thận vì thông tin sai có thể ảnh hưởng đến an toàn bệnh nhân. Với các hồ sơ lâm sàng quan trọng, vẫn cần có bước kiểm duyệt của con người.

## Đánh giá cá nhân

Theo tôi, giải pháp này cho thấy một trong những ứng dụng thực tế nhất của Generative AI. Thay vì chỉ dùng GenAI cho chatbot, dự án áp dụng GenAI vào data automation, một bài toán rất thực tế trong kinh doanh và y tế.

Điểm ấn tượng nhất là khả năng chuyển đổi tài liệu không có cấu trúc thành dữ liệu chuẩn FHIR. Điều này có giá trị lớn vì các hệ thống y tế thường gặp khó khăn do dữ liệu phân mảnh và thiếu thống nhất. Nếu thông tin y tế có thể được chuyển về một chuẩn chung, việc tìm kiếm, trao đổi và phân tích dữ liệu sẽ dễ dàng hơn rất nhiều.

Tôi cũng cho rằng kiến trúc event-driven phù hợp với use case này. Khi một file được upload lên S3, pipeline có thể tự động bắt đầu mà không cần thao tác thủ công. Mỗi thành phần được tách biệt rõ ràng, giúp hệ thống dễ mở rộng và dễ bảo trì hơn.

Tuy nhiên, giải pháp này cần được sử dụng rất thận trọng trong môi trường y tế thực tế. Bảo mật dữ liệu, quyền riêng tư bệnh nhân, tuân thủ pháp lý và kiểm soát chi phí là những yếu tố rất quan trọng. Đối với các quốc gia như Việt Nam, việc lưu trữ và xử lý dữ liệu y tế của bệnh nhân trên cloud quốc tế có thể cần được xem xét kỹ về mặt pháp lý và quản trị dữ liệu.

Nhìn chung, bài blog giúp tôi hiểu rằng Generative AI có thể tạo ra giá trị thực tế khi được dùng để làm sạch, cấu trúc hóa và chuẩn hóa các loại dữ liệu phức tạp.

## Kết luận

Giải pháp này cho thấy cách Amazon Bedrock Data Automation và AWS HealthLake có thể được kết hợp để tự động hóa quá trình số hóa hồ sơ y tế. Bằng kiến trúc serverless event-driven, hệ thống có thể xử lý tài liệu y tế được upload, trích xuất thông tin lâm sàng, chuyển đổi sang định dạng FHIR và lưu vào AWS HealthLake để truy vấn và phân tích trong tương lai.

Bài học quan trọng là Generative AI không chỉ hữu ích cho hội thoại. Một trong những ứng dụng thực tế mạnh nhất của GenAI là data automation: biến tài liệu lộn xộn, không có cấu trúc thành dữ liệu sạch và có cấu trúc.

Giải pháp cũng cho thấy giá trị của kiến trúc event-driven. Bằng cách sử dụng S3 events, Lambda functions, Amazon Bedrock Data Automation và AWS HealthLake, hệ thống có thể hoạt động theo hướng tách rời, tự động và có khả năng mở rộng.

Tuy nhiên, dữ liệu y tế đòi hỏi các kiểm soát nghiêm ngặt về bảo mật, quyền riêng tư và tuân thủ. Trước khi áp dụng phương pháp này với dữ liệu bệnh nhân thật, tổ chức cần đánh giá kỹ yêu cầu pháp lý, kiến trúc bảo mật, chiến lược chi phí và quy trình kiểm chứng bởi con người.

## Hình ảnh

![img.png](img.png)

## Link tham khảo

https://aws.amazon.com/vi/blogs/architecture/automate-medical-record-digitization-with-amazon-bedrock-data-automation-and-aws-healthlake/

## Hướng dẫn

Bài blog này chủ yếu là bài tóm tắt và đánh giá bài viết của AWS. Tôi không triển khai toàn bộ giải pháp vì nội dung liên quan đến xử lý dữ liệu y tế, Amazon Bedrock Data Automation, AWS HealthLake, ánh xạ dữ liệu FHIR, kiểm soát bảo mật và yêu cầu tuân thủ trên cloud.

Thay vào đó, tôi tập trung vào:

- Đọc bài viết gốc của AWS
- Hiểu bài toán hồ sơ y tế không có cấu trúc
- Xác định cách Amazon Bedrock Data Automation trích xuất thông tin lâm sàng
- Hiểu cách AWS Lambda chuyển đổi dữ liệu trích xuất sang FHIR R4
- Tìm hiểu vai trò của AWS HealthLake như một kho dữ liệu y tế
- Phân tích ưu điểm, hạn chế, vấn đề bảo mật và thách thức chi phí
- Tổng hợp cách Generative AI có thể được ứng dụng vào data automation thực tế

