---
title: "Blog 2"
date: 2026-11-19
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# KHI AI KHÔNG CHỈ TRẢ LỜI: TÌM HIỂU AI AGENT VỚI AMAZON BEDROCK AGENTCORE

**Nếu AI có thể chia một yêu cầu thành nhiều bước, tìm dữ liệu và kiểm tra kết quả trước khi trả lời thì sao?**

Khi tìm hiểu AI, mình nhận thấy có những bài toán không thể giải quyết chỉ bằng một câu trả lời. Chẳng hạn, để tư vấn mua thực phẩm, hệ thống cần hiểu nhu cầu, tìm sản phẩm phù hợp và kiểm tra thông tin trước khi đưa ra gợi ý.

Đây là lúc mình bắt đầu tìm hiểu **AI agent**: một chương trình kết hợp mô hình AI với công cụ và logic điều phối để thực hiện nhiều bước hướng tới mục tiêu.

**Một AI agent có thể hoạt động như thế nào?**

- Người dùng đặt câu hỏi: “Mình cần nguyên liệu nấu cho 20 người trong ngân sách 500.000 đồng”.
- Agent phân tích yêu cầu và xác định thông tin còn thiếu, chẳng hạn loại món hoặc số bữa.
- Khi cần dữ liệu, agent có thể đề xuất sử dụng công cụ tìm sản phẩm được ứng dụng cung cấp.
- Ứng dụng kiểm tra quyền và gọi API; agent dùng kết quả để tổng hợp câu trả lời.
- Nếu chưa đủ thông tin, agent hỏi lại thay vì tự đặt ra giá hoặc sản phẩm.

Agent có thể gồm một tác nhân hoặc nhiều tác nhân phối hợp. Ví dụ, một agent tìm thông tin và một agent khác kiểm tra kết quả. Tuy nhiên, thêm agent cũng đồng nghĩa với thêm lời gọi mô hình, thời gian xử lý và chi phí; không phải bài toán nào cũng cần nhiều agent.

**Amazon Bedrock AgentCore hỗ trợ gì?**

[Amazon Bedrock AgentCore](https://docs.aws.amazon.com/prescriptive-guidance/latest/agentic-ai-frameworks/amazon-bedrock-agent-core.html) cung cấp các khả năng hỗ trợ xây dựng và vận hành agent:

- **Runtime:** Môi trường serverless để chạy agent và công cụ, giúp giảm công việc quản lý hạ tầng.
- **Gateway:** Hỗ trợ đưa API và công cụ hiện có vào luồng làm việc của agent thông qua MCP.
- **Memory:** Hỗ trợ lưu và truy xuất ngữ cảnh khi ứng dụng cần duy trì thông tin giữa các lượt hoặc phiên.
- **Identity:** Hỗ trợ quản lý xác thực và thông tin truy cập tới các dịch vụ khác.
- **Observability:** Hỗ trợ theo dõi hoạt động, độ trễ và lỗi để mình kiểm tra agent đang làm gì.

Mình không bắt buộc phải dùng tất cả các thành phần này. Ví dụ, nếu agent chỉ cần xử lý một yêu cầu đơn giản và không cần ghi nhớ lâu dài, việc thêm Memory có thể chưa cần thiết.

**Phân biệt Bedrock và AgentCore**

Mình hiểu đơn giản: **Amazon Bedrock** giúp ứng dụng truy cập các mô hình AI; **AgentCore** cung cấp các khả năng bổ sung để chạy và kết nối agent. Mình vẫn cần thiết kế logic agent, khai báo công cụ, kiểm tra kết quả và xác định dữ liệu nào được phép sử dụng. AgentCore Runtime không tự biến một model thành toàn bộ ứng dụng hoàn chỉnh.

**API và dữ liệu được bảo vệ như thế nào?**

- Mình chỉ cung cấp những công cụ cần cho tác vụ, thay vì cho agent toàn quyền truy cập hệ thống.
- Backend vẫn xác thực người dùng và kiểm tra quyền mỗi khi API được gọi.
- Giá, tồn kho và trạng thái đơn hàng phải đến từ hệ thống dữ liệu thực tế.
- Các hành động quan trọng như đặt hàng hoặc thanh toán cần kiểm tra nghiệp vụ và xác nhận phù hợp.
- Thông tin nhạy cảm và dữ liệu ghi log cần được giới hạn theo nhu cầu sử dụng.

Theo mình, agent hữu ích khi một yêu cầu cần kết hợp nhiều bước hoặc nhiều nguồn thông tin. Điều quan trọng là lựa chọn độ phức tạp phù hợp, kiểm thử kết quả và kiểm soát quyền ngay từ đầu.

**Tài liệu tham khảo:**

- [Bài viết AWS gốc về Deep Agents và AgentCore](https://aws.amazon.com/blogs/machine-learning/running-deep-research-ai-agents-on-amazon-bedrock-agentcore/)
- [AgentCore Runtime và Harness](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/harness-vs-runtime.html)
- [Thực hành bảo mật Runtime](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/runtime-security-best-practices.html)
