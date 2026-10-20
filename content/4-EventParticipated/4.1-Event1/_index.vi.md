---
title: "Event 1"
date: 2026-09-26
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Bài thu hoạch: The Thinking Behind Building the Bot

Talkshow **The Thinking Behind Building the Bot** do **CMC Global phối hợp cùng VietAWS** tổ chức tập trung vào tư duy xây dựng AI Agent: từ bài toán lập trình, ứng dụng doanh nghiệp đến việc quyết định mức độ trao quyền cho bot.

### Thông tin sự kiện

| Mục            | Thông tin                                                                      |
| -------------- | ------------------------------------------------------------------------------ |
| Ngày tổ chức   | Thứ Bảy, ngày 26/09/2026                                                       |
| Thời gian      | 09:00–12:00 (UTC+7)                                                            |
| Địa điểm       | Tầng 26, Bitexco Financial Tower, 2 Hải Triều, phường Sài Gòn, TP. Hồ Chí Minh |
| Đơn vị tổ chức | CMC Global × VietAWS                                                           |
| Vai trò        | Người tham dự                                                                  |

### Mục tiêu sự kiện

- Hiểu cách AI Agent chuyển từ tạo câu trả lời sang lựa chọn công cụ, thực hiện hành động để đạt mục tiêu xác định.
- Liên hệ thiết kế agent với phối hợp business–dev trái múi giờ: ngữ cảnh ticket, ghi chú tiến độ, thông báo vấn đề và mức tự chủ được kiểm soát.
- Tiếp cận AI Agent Challenge, trao đổi về kỹ năng Cloud & AI, dự án thực tế và định hướng nghề nghiệp.

### Diễn giả tiêu biểu

- **Bùi Nhật Trường** – Technical Leader, CMC Global.
- **Nguyễn Gia Hưng** – Head of Solutions Architect, AWS.

### Điểm nhấn chính

#### 1. Chương trình được công bố

| Thời gian (UTC+7) | Hoạt động                                 | Nội dung chính                                                                   |
| ----------------- | ----------------------------------------- | -------------------------------------------------------------------------------- |
| 09:00–09:30       | Buildathon Kickoff                        | Giới thiệu thử thách, mục tiêu, tiêu chí đánh giá và tổng quan AWS Cloud, GenAI. |
| 09:30–09:45       | Ice-breaking                              | Làm quen, kết nối và chuẩn bị phối hợp giữa các thành viên.                      |
| 09:45–10:30       | Talkshow: Building a Career in Cloud & AI | Góc nhìn ngành, dự án thực tế và cơ hội nghề nghiệp.                             |
| 10:30–10:45       | Q&A                                       | Trao đổi về AWS, GenAI, nghề nghiệp, chứng chỉ và thử thách.                     |
| 10:45–11:15       | AI Agent Challenge                        | Prompt engineering, RAG, knowledge base và các khái niệm agent.                  |
| 11:15–11:20       | Closing Remarks                           | Tổng kết, tài liệu học và hoạt động cộng đồng.                                   |

#### 2. Tư duy phía sau việc xây dựng agent

Trọng tâm là cách AI Agent hiểu ngữ cảnh, sử dụng bộ nhớ, lựa chọn bước tiếp theo và hỗ trợ developer như một đồng đội. Từ đó đặt ra câu hỏi về công việc nào nên giao cho agent và hệ thống được phép tự quyết đến đâu.

Buổi trao đổi có góc nhìn từ bài toán phát triển phần mềm, ứng dụng doanh nghiệp và thử thách tìm hiểu cách bot hoạt động. Bên cạnh phần chuyên môn thì còn có hoạt động kết nối với doanh nghiệp, trao đổi và phỏng vấn.

#### 3. Demo phối hợp business–dev trái múi giờ và QA

- Demo đặt ra tình huống team business và team dev làm việc ở các múi giờ khác nhau. Khi dev đang ngoài giờ làm việc, business gửi ticket cho AI Agent tiếp nhận và xử lý.
- Agent xử lý yêu cầu, ghi chú công việc và các vấn đề phát sinh, đồng thời thông báo để dev xem lại khi bắt đầu ngày làm việc. Tình huống minh họa cách agent hỗ trợ phối hợp bất đồng bộ và giữ ngữ cảnh giữa các team.

### Kết luận và bài học quan trọng

#### 1. Thiết kế mục tiêu và vòng lặp thực thi

- Xác định nhiệm vụ cụ thể, tiêu chí hoàn thành, công cụ được phép và điều kiện dừng. Với agent dùng LLM, mô hình đề xuất bước tiếp theo; mã ứng dụng thực thi công cụ được cho phép rồi đưa kết quả trở lại vòng xử lý. Xem [mô tả cơ chế điều phối agent của AWS](https://docs.aws.amazon.com/bedrock/latest/userguide/agents-how.html).
- Demo ticket cho thấy ghi chú tiến độ cần phân biệt việc đã hoàn thành, điểm đang vướng và hành động tiếp theo dành cho dev. Với công việc liên quan QA, thông báo hoàn tất cần đi kèm kết quả kiểm tra hoặc ghi rõ phần còn cần xác minh.

#### 2. Phân biệt ngữ cảnh, bộ nhớ và RAG

- Lịch sử hội thoại giúp giữ ngữ cảnh trong quá trình tương tác; bộ nhớ cần chiến lược lưu, truy hồi thông tin rõ ràng. RAG tìm tài liệu liên quan làm căn cứ trả lời, có vai trò khác với ghi nhớ yêu cầu trước của người dùng. Tham khảo [AgentCore Memory](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/memory.html) và [Bedrock Knowledge Bases](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base.html).
- Khi bàn giao trái múi giờ, dev cần xem được yêu cầu ban đầu, các hành động đã thực hiện và câu hỏi còn mở. Lưu trạng thái ticket phục vụ mục đích khác với truy xuất tài liệu tham khảo; chất lượng nguồn và quyền truy cập vẫn cần được kiểm tra riêng.

#### 3. Trao quyền phù hợp với rủi ro nghiệp vụ

- Giới hạn quyền công cụ và kiểm tra tham số. Thao tác thay đổi dữ liệu hoặc hệ thống quan trọng cần cơ chế phê duyệt, khôi phục phù hợp; quyết định của mô hình không thay thế việc phân quyền. AWS mô tả [cơ chế xác nhận trước hành động của agent](https://docs.aws.amazon.com/bedrock/latest/userguide/agents-userconfirmation.html) như một cách triển khai.
- Đánh giá tỷ lệ hoàn thành nhiệm vụ, trường hợp lỗi, độ trễ, chi phí; xem trace thực thi khi hành vi khác kỳ vọng. Những kiểm tra này giúp chuyển từ demo ấn tượng sang ứng dụng có thể vận hành, cải thiện. Tham khảo [AgentCore Observability](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/observability.html).

### Nguồn tham khảo

- [Chương trình và poster sự kiện](https://luma.com/8fe9vvps)
- [CMC Global Careers: bài recap sự kiện](https://vn.linkedin.com/company/cmc-global-careers)
- [AWS: AI Agent và các thành phần](https://aws.amazon.com/what-is/ai-agents/)
- [AWS: AgentCore Memory](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/memory.html)
- [AWS: Amazon Bedrock Knowledge Bases](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base.html)
- [AWS: xác nhận trước khi thực hiện hành động](https://docs.aws.amazon.com/bedrock/latest/userguide/agents-userconfirmation.html)
- [AWS: AgentCore Observability](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/observability.html)

---

![Đã Check in tại sự kiện](/images/4-EventParticipated/4.1-Event1/check-in.png)

*Đã Check in tại sự kiện*
