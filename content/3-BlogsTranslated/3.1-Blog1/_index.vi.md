---
title: "Blog 1"
date: 2026-11-19
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# KHI AI CÓ THỂ THAY ĐỔI GIAO DIỆN: GENERATIVE UI SẼ HOẠT ĐỘNG NHƯ THẾ NÀO?

**Nếu mỗi người dùng có thể nhìn thấy một giao diện phù hợp với nhu cầu của mình, website sẽ thay đổi như thế nào?**

Gần đây, khi các mô hình AI ngày càng mạnh, mình thấy **Generative UI** là một hướng tiếp cận thú vị: thay vì mọi người đều dùng một bố cục cố định, giao diện có thể thay đổi theo yêu cầu và ngữ cảnh của từng người.

Ví dụ, một khách hàng muốn xem hình ảnh để chọn thực phẩm, trong khi một người mua hàng cho nhà hàng cần so sánh nhanh giá và quy cách. Cùng một danh sách sản phẩm, ứng dụng có thể ưu tiên dạng thẻ cho người đầu tiên và dạng bảng cho người thứ hai.

**Generative UI khác gì với một chatbot thông thường?**

Chatbot thường phản hồi bằng lời nói hoặc văn bản. Với Generative UI, câu trả lời còn có thể đi kèm một cách hiển thị phù hợp: bảng so sánh, thẻ sản phẩm, bộ lọc hoặc biểu đồ. Mình vẫn có thể trò chuyện với AI, nhưng kết quả không nhất thiết chỉ nằm trong khung chat.

- **AI hiểu ý định:** Người dùng có thể nói: “Cho mình xem sản phẩm dạng bảng để dễ so sánh giá”.
- **Ứng dụng thay đổi giao diện:** AI đề xuất cách trình bày, sau đó ứng dụng cập nhật giao diện và DOM thông qua các component đã được xây dựng sẵn.
- **Trải nghiệm cá nhân hơn:** Cùng một danh sách sản phẩm có thể được trình bày dạng bảng hoặc dạng thẻ tùy theo nhu cầu, giúp người dùng thao tác thuận tiện hơn.
- **Dữ liệu vẫn do hệ thống quản lý:** Giá, tồn kho hay thông tin đơn hàng cần được lấy từ API và database; mình không nên để AI tự tạo ra những con số này.
- **Cần có giới hạn an toàn:** Backend cần kiểm tra kết quả AI và quyền của người dùng trước khi áp dụng thay đổi hoặc gọi các chức năng quan trọng.

Với AWS, mình có thể tìm hiểu **Amazon Bedrock** để kết nối mô hình AI, kết hợp **Bedrock Guardrails** để hỗ trợ kiểm tra nội dung. Guardrails là một lớp bảo vệ bổ sung và vẫn cần được kiểm thử với dữ liệu thực tế.

**Generative UI hoạt động như thế nào với A2UI 1.0?**

- Trước tiên, mình định nghĩa **catalog** cho A2UI: danh sách component, thuộc tính, action và function mà ứng dụng hỗ trợ. Chẳng hạn, catalog có thể khai báo `ProductTable` và thao tác `searchProducts`.
- Khách nhập yêu cầu: “Cho mình xem bảng so sánh giá thịt heo”. Frontend chuyển yêu cầu đến agent ở backend. Agent có thể dùng model qua Amazon Bedrock và những công cụ nghiệp vụ được cung cấp để lấy dữ liệu phù hợp.
- **A2UI Agent SDK** hỗ trợ tạo hướng dẫn cho model từ catalog, sau đó phân tích và kiểm tra kết quả do model tạo. SDK tạo các thông điệp A2UI có cấu trúc, chẳng hạn `updateComponents` và `updateDataModel`.
- **Renderer** phía frontend nhận các thông điệp và ánh xạ component trong catalog sang component đã được ứng dụng cài đặt. Với React, React chịu trách nhiệm cập nhật DOM theo trạng thái giao diện.

Trong ví dụ này, model có thể đề xuất hiển thị bảng sản phẩm; renderer tạo bảng từ component đã đăng ký và dữ liệu do ứng dụng cung cấp. Model, agent và API nghiệp vụ do ứng dụng tích hợp; A2UI không tự tạo sẵn các phần đó.

Mình nghĩ cách này giúp giao diện linh hoạt hơn mà vẫn dễ kiểm soát: AI đề xuất **cách trình bày**, còn ứng dụng quyết định component nào được hiển thị và hành động nào được phép thực hiện.

**Nếu component cần gọi API thì AI biết dùng API nào?**

Đây là điều mình thấy quan trọng khi thiết kế Generative UI. Catalog mô tả component và function, còn agent có thể được cung cấp các **tool nghiệp vụ** riêng để tương tác với API. Mình không cần để model tự tìm mọi API trong hệ thống.

- Khi cần tìm sản phẩm, agent có thể đề xuất tool `searchProducts` đã được khai báo; backend kiểm tra tham số và thực thi logic gọi API tương ứng.
- Khi khách bấm nút, A2UI có thể gửi một **event** để agent xử lý, hoặc gọi một **function** đã đăng ký. Function có thể chạy ở renderer hoặc phía agent, tùy cách khai báo.
- Kết quả có thể được dùng để cập nhật dữ liệu hoặc giao diện. Việc ánh xạ tới API và kiểm tra nghiệp vụ vẫn do mã ứng dụng đảm nhận.

**AI có quyền truy cập toàn bộ API và dữ liệu không?**

Theo mình, AI là một phần được backend tích hợp, chứ không mặc nhiên có mọi quyền của backend. Mình chỉ cung cấp những công cụ cần thiết cho từng tác vụ; mỗi lần gọi, backend vẫn phải xác thực người dùng, kiểm tra role và quyền đối với dữ liệu cụ thể. Không đưa API key hoặc token của người dùng vào prompt; chỉ gửi cho mô hình những trường dữ liệu thực sự cần thiết.

Ví dụ, AI có thể được phép đề xuất tìm sản phẩm, nhưng không được tự xem đơn hàng của khách khác. Những thao tác như đặt hàng hoặc thanh toán cần được backend kiểm tra và yêu cầu người dùng xác nhận. Guardrails hỗ trợ kiểm tra nội dung, nhưng không thay thế phân quyền API.

Mình tham khảo cách tổ chức luồng này từ tài liệu A2UI 1.0 và hướng dẫn Quickstart (link ở cuối bài). Catalog và SDK giúp kiểm soát cấu trúc UI, nhưng không tự thay thế việc xác thực, phân quyền hay bảo vệ API.

**Amazon Bedrock Guardrails bảo vệ luồng AI như thế nào?**

Guardrails bổ sung một lớp kiểm tra nội dung trước hoặc sau khi gọi model. Mình có thể cấu hình để hỗ trợ phát hiện và lọc nội dung có hại, chủ đề không mong muốn hoặc thông tin nhạy cảm như email và số điện thoại. Tùy chính sách, hệ thống có thể chặn nội dung hoặc che thông tin nhạy cảm.

Ví dụ, nếu khách vô tình nhập số điện thoại trong yêu cầu xem sản phẩm, mình có thể cấu hình kiểm tra để hạn chế gửi thông tin đó đến model. Việc nhận diện mang tính xác suất, nên cần kiểm thử dữ liệu và ngôn ngữ thực tế. Guardrails không bảo đảm phát hiện mọi trường hợp và không thay thế việc kiểm tra quyền truy cập API.

**Các dịch vụ AWS có thể hỗ trợ gì?**

- **Amazon Bedrock:** Kết nối ứng dụng với mô hình AI để phân tích yêu cầu và đề xuất cấu hình giao diện.
- **Amazon Bedrock Guardrails:** Hỗ trợ đánh giá nội dung đầu vào và đầu ra theo các chính sách đã cấu hình.
- **Amazon ECS và AWS Fargate:** Chạy backend trong container; Fargate giúp giảm công việc quản lý máy chủ.
- **Amazon S3 và Amazon CloudFront:** Lưu trữ và phân phối các tệp frontend đến người dùng.
- **Amazon CloudWatch:** Theo dõi log và các chỉ số để hỗ trợ phát hiện lỗi hoặc vấn đề hiệu năng.

Theo mình, kết hợp những dịch vụ này giúp chia rõ vai trò: Bedrock xử lý AI, backend kiểm soát dữ liệu và quyền, còn các dịch vụ hạ tầng hỗ trợ triển khai và theo dõi ứng dụng. Không phải hệ thống nào cũng cần dùng tất cả; lựa chọn phụ thuộc vào nhu cầu và chi phí.

**Những điều mình cần lưu ý khi triển khai**

- **Không gửi thừa dữ liệu cá nhân:** Chỉ cung cấp cho mô hình thông tin thực sự cần để xử lý yêu cầu.
- **Kiểm tra kết quả AI:** Không mặc định JSON do AI trả về luôn đúng; cần kiểm tra cấu trúc và các lựa chọn được phép.
- **Không để AI tự xác nhận giao dịch:** Những thao tác như đặt hàng hay thanh toán cần đi qua API, quy tắc nghiệp vụ và bước xác nhận phù hợp.
- **Theo dõi chi phí và thời gian phản hồi:** Số lượt gọi và lượng token ảnh hưởng đến chi phí; ứng dụng cũng cần xử lý trường hợp AI phản hồi chậm hoặc không khả dụng.

Theo mình, Generative UI mở ra cơ hội tạo ra những giao diện linh hoạt và bớt nhàm chán hơn. Điều quan trọng là AI phải hỗ trợ đúng nhu cầu của người dùng, còn ứng dụng vẫn kiểm soát dữ liệu và hành động.

**Nguồn tham khảo:**

- [Amazon Bedrock là gì?](https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html)
- [Hội thoại với model qua Converse API](https://docs.aws.amazon.com/bedrock/latest/userguide/conversation-inference.html)
- [Amazon Bedrock Guardrails](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails.html)
- [Bộ lọc thông tin nhạy cảm trong Guardrails](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails-sensitive-filters.html)
- [A2UI 1.0](https://a2ui.org/)
- [Hướng dẫn Quickstart của A2UI](https://a2ui.org/quickstart/)
