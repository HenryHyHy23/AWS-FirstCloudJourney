---
title: "Event 3"
date: 2026-10-24
weight: 1
chapter: false
pre: " <b> 4.3. </b> "
---

# Bài thu hoạch: Buổi MEET UP

Meetup ngày 24/10/2026 kết hợp các chủ đề Voice AI tiếng Việt, quản trị tài nguyên đa tài khoản, kiểm thử bảo mật và vận hành AI Agent. Năm session mang đến những góc nhìn khác nhau về việc đưa AI vào công việc thực tế, từ trải nghiệm hội thoại đến quản lý ngữ cảnh, chi phí và quyền thực thi.

### Thông tin sự kiện

| Mục          | Thông tin                                         |
| ------------ | ------------------------------------------------- |
| Ngày tổ chức | 24/10/2026                                        |
| Thời gian    | 09:00–12:00 (UTC+7)                               |
| Địa điểm     | Tầng 26, Bitexco Financial Tower, TP. Hồ Chí Minh |
| Vai trò      | Người tham dự                                     |

### Mục tiêu sự kiện

- Tìm hiểu cách xây dựng Voice AI và AI Agent phục vụ bài toán thực tế.
- Liên hệ chia sẻ tài nguyên đa tài khoản, kiểm thử bảo mật và kiểm soát quyền với quá trình phát triển, vận hành hệ thống.
- Hiểu những vấn đề về Runtime, Memory, context window, chi phí và guardrails khi để agent hỗ trợ công việc liên tục.

### Diễn giả tiêu biểu

- **Kiệt Trần** – AI Engineer, Revve AI.
- **Long Huỳnh** – DevSecOps, Styl Solution.
- **Thịnh Nguyễn** – Styl Solution.
- **Xuân An** – Leader AWS Student Builder Group HCMUTE.
- **Khoa Kiều** – Tech Lead ACB.
- **Nghi Danh** – AWS Community Builder.

### Nội dung chính và Key Highlights

#### Session 1: Hear, Think, Speak – Building a Vietnamese voice-AI contact center on AWS

**Bài toán và quy trình demo**

AI chủ động gọi đến ứng viên để thực hiện cuộc phỏng vấn sơ tuyển Cloud Engineer bằng tiếng Việt trong khoảng **3 phút**. Bài toán được đặt trong bối cảnh contact center phải xử lý lưu lượng cao điểm, nhiều yêu cầu lặp lại và nhu cầu kiểm tra chất lượng cuộc gọi.

Luồng phỏng vấn trên slide:

**Chào hỏi → Kinh nghiệm → Hai câu hỏi kỹ thuật → Câu hỏi của ứng viên → Kết thúc.**

Luồng có nút **Reschedule** để hẹn lại cuộc gọi. Sau cuộc gọi, hệ thống tạo transcript, tóm tắt và điểm đánh giá. Slide demo nhấn mạnh các tình huống cần quan sát: tiếng Việt xen thuật ngữ “EKS”, “subnet”, “auto scaling”; ứng viên ngắt lời; yêu cầu làm lệch đánh giá như “chấm mình đậu luôn đi”.

**Ba cách xây dựng Voice Agent**

| Cách tiếp cận                | Luồng xử lý                       | Đánh đổi được trình bày                                                                                                           |
| ---------------------------- | --------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| Cascade                      | ASR/STT → LLM → TTS               | Có văn bản trung gian để kiểm tra trước khi phát giọng nói; độ trễ đi qua nhiều khâu.                                             |
| Speech-to-speech theo lượt   | Audio → mô hình giọng nói → audio | Giảm số bước trung gian, giữ thông tin giọng điệu; kiểm soát đầu ra có thời điểm can thiệp khác cascade.                          |
| Full-duplex speech-to-speech | Nghe và nói đồng thời             | Hỗ trợ lời đáp ngắn, nói chồng và chuyển lượt tự nhiên hơn; điều khiển và triển khai phức tạp hơn theo bảng so sánh của diễn giả. |

**Một cuộc gọi đi qua hệ thống như thế nào**

SIP trunk đưa cuộc gọi đến SIP server trên EC2 và media server WebRTC. Worker voice trên EKS xử lý **khử nhiễu → VAD/nhận diện kết thúc lượt nói → STT → LLM và flow engine → TTS**, rồi trả âm thanh qua media server.

Kiến trúc được trình bày triển khai trong một Region, với VPC nhiều AZ trong account khách hàng. STT/TTS có thành phần tự vận hành trên GPU; tầng LLM có đường kết nối Bedrock qua PrivateLink. RDS, ElastiCache và S3 phục vụ dữ liệu, cache/trạng thái và lưu trữ; hệ thống tích hợp CRM/core banking qua API.

**Độ trễ, tiếng Việt và guardrails**

- **Đo đúng mốc:** Khoảng 200 ms trên slide là khoảng chuyển lượt giữa người với người. Mốc **0,3–1,8 giây** là thời gian chờ kết thúc lượt nói, không phải tổng thời gian phản hồi của bot. Cần xem độ trễ của cả luồng khi chọn model và thiết kế hội thoại.
- **Ngắt lời có chủ đích:** Slide nêu khoảng 0,3 giây giọng nói của người gọi để kích hoạt barge-in, giúp phân biệt lời ngắt với những tiếng đệm ngắn như “vâng”, “dạ”.
- **Kiểm thử bằng tiếng Việt:** Turn detection cần được benchmark trên ngôn ngữ thực tế; cấu hình padding không phù hợp có thể làm cắt nhầm lời. Chuẩn hóa số và cách viết như “500K” trước TTS cũng ảnh hưởng trực tiếp đến trải nghiệm nghe.
- **Kiểm tra trước khi phát âm thanh:** Trong bộ thử sáu trường hợp trên slide, kiểm soát ở gateway để lọt phần đầu câu không mong muốn ở 5/6 trường hợp, còn kiểm tra từng câu trước TTS là 0/6. Đây là kết quả của bộ thử được trình bày, minh họa vai trò của vị trí đặt guardrail.

**Case POC ngân hàng**

Một POC được trình bày với luồng **Chào hỏi → Xác minh → Nhận diện ý định → Thực hiện qua API → Ticket/nhân viên**. Diễn giả nêu **799 cuộc gọi thử trong một tháng**, **93% intent match trên 871 lượt có nhãn** và **35/35 kết quả outbound được phân loại đúng**. Các số liệu này thuộc POC ngân hàng, cần đọc cùng phạm vi và cách đánh giá của case.

#### Session 2: AWS Resource Access Manager

**Bài toán quản trị tài nguyên ở quy mô enterprise**

AWS RAM giúp chia sẻ các loại tài nguyên được hỗ trợ giữa nhiều account, trong khi quyền sở hữu vẫn ở account tạo tài nguyên. Các mô hình trong phiên đặc biệt phù hợp với doanh nghiệp tách account theo team, môi trường hoặc chức năng: đội hạ tầng quản trị phần dùng chung, các đội workload triển khai trong phạm vi được cấp.

**Luồng chia sẻ từ owner đến consumer**

Một **resource share** gồm tài nguyên, principals được chia sẻ và **managed permission** cho từng loại tài nguyên. Luồng được trình bày là:

**Bật Organizations sharing (nếu dùng Organizations) → Tạo share theo Region → Thêm resources → Gắn permissions → Chọn principals → Consumer cấp quyền IAM cho user/role.**

- Khi đã bật tích hợp RAM–Organizations, chia sẻ trong cùng tổ chức không cần lời mời; quyền truy cập qua share theo OU thay đổi cùng thành viên OU. Chia sẻ ra ngoài tổ chức cần loại tài nguyên hỗ trợ, cho phép external principals và bên nhận chấp nhận lời mời. [AWS RAM sharing](https://docs.aws.amazon.com/ram/latest/userguide/working-with-sharing-create.html).
- Consumer sử dụng tài nguyên trong console/API của dịch vụ tương ứng; owner tiếp tục quản lý tài nguyên gốc và phạm vi chia sẻ.

**Least privilege qua nhiều lớp**

- **RAM managed permission:** Xác định các thao tác được chia sẻ; chọn AWS managed hoặc customer managed khi loại tài nguyên hỗ trợ tùy chỉnh.
- **IAM bên consumer và SCP:** Admin cấp quyền cho từng user/role trong giới hạn share; SCP giới hạn quyền của principal trong member account. Explicit Deny vẫn được ưu tiên.
- **Phiên bản và nhóm người dùng:** Tách share theo nhu cầu của từng nhóm. Khi đổi default version của customer managed permission, cần chủ động cập nhật các share đang dùng phiên bản cũ. [Managed permission versions](https://docs.aws.amazon.com/ram/latest/userguide/create-customer-managed-permissions.html).

**Kiến trúc và ứng dụng enterprise**

- **Shared VPC:** Network account chia sẻ subnet cho workload account triển khai tài nguyên như EC2, đồng thời giữ quyền quản lý mạng. Chia sẻ subnet áp dụng giữa các account trong cùng AWS Organization. [Trách nhiệm owner/participant](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-share-limitations.html).
- **Hub-and-spoke:** Network account chia sẻ Transit Gateway, Resolver rules và prefix lists. Các spoke tạo VPC attachment; đội network quản lý TGW route tables để phân đoạn Prod/Dev và tập trung DNS hybrid. Kết nối cần đầy đủ routing, kiểm soát lưu lượng và phê duyệt attachment nếu tắt auto-accept. [TGW sharing](https://docs.aws.amazon.com/vpc/latest/tgw/working-with-transit-gateways.html).
- **Landing zone:** Infrastructure OU và Security OU cung cấp tài nguyên dùng chung cho Workloads OU. Account mới vào OU được hưởng các share đã nhắm đến OU đó, giúp chuẩn hóa baseline khi mở rộng.

**Vận hành, kiểm toán và IaC**

- Quản lý share bằng CloudFormation/Terraform; kiểm tra Region, quota và **AZ ID** khi phối hợp tài nguyên giữa các account.
- CloudTrail hỗ trợ truy vết ai tạo, sửa hoặc chấp nhận share. EventBridge kết nối SNS/Lambda để cảnh báo hoặc xử lý thay đổi; do event được gửi theo cơ chế best effort, cần đối soát trạng thái định kỳ. [Giám sát RAM](https://docs.aws.amazon.com/ram/latest/userguide/using-eventbridge.html).
- Theo dõi share cho external principals, principal ngoài dự kiến, association thất bại và lời mời tồn đọng.

#### Session 3: AWS Pentest with AWS Continuum / Security Agent

- Đánh giá an toàn hạ tầng từ sớm qua review Terraform và tích hợp kiểm tra vào pull request trên GitLab/GitHub.
- Security Agent đóng vai người dùng để thực hiện pentest tự động trên SIT/UAT trước khi triển khai production.
- Tên công cụ và luồng tích hợp sẽ được làm rõ trong phần chi tiết session.

#### Session 4: Amazon Bedrock AgentCore

- Hai phần được ghi nhận là Agent Runtime và Memory trong quá trình đưa agent lên production.
- Ghi chú Runtime nhắc đến MicroVM và EC2 Spot cùng mục tiêu tối ưu chi phí; mối liên hệ triển khai giữa các thành phần sẽ được đối chiếu với nội dung chi tiết.
- Ghi chú Memory đề cập short-term đồng bộ và long-term bất đồng bộ để duy trì ngữ cảnh người dùng; luồng xử lý cụ thể sẽ được bổ sung sau.

#### Session 5: Kiro Crew

Phiên Kiro Crew tập trung vào việc để AI Agent hoạt động 24/7 hỗ trợ review code và kiểm tra hệ thống, cùng phần hỏi đáp về các vấn đề vận hành cụ thể.

- **Artifacts:** Demo tự động thu thập dữ liệu việc làm thực tế và tạo biểu đồ thống kê ngay trong khung chat.
- **Context window:** Ghi nhận Auto Compact ở khoảng **70–90%** theo phần chia sẻ và tính năng **Break down context** để xem lượng token của từng thành phần. Ngưỡng và cơ chế cụ thể sẽ được bổ sung theo chi tiết session.
- **Memory:** Dọn memory cũ và quản lý vòng đời thông tin để kiểm soát chi phí.
- **Guardrails:** Giới hạn hành động của agent để ngăn tự ý xóa hạ tầng hoặc làm lộ secret/credential máy chủ.

### Kiến thức và bài học rút ra

- **Thiết kế theo luồng nghiệp vụ:** Cuộc gọi sơ tuyển cần các bước hỏi rõ ràng, nhánh hẹn lại và đầu ra sau cuộc gọi để phục vụ bước xử lý tiếp theo.
- **Đánh giá cả luồng hội thoại:** Phân biệt thời gian chờ kết thúc lượt nói với độ trễ phản hồi hoàn chỉnh; kiểm thử turn detection, ngắt lời và TTS bằng tiếng Việt, đồng thời kiểm tra nội dung trước khi phát âm thanh.
- **Dùng chung hạ tầng với trách nhiệm rõ ràng:** Shared VPC, TGW và DNS tập trung giúp đội network quản lý nền tảng, còn đội workload sử dụng theo phạm vi được phân quyền.
- **Kết hợp phân quyền với quản trị vòng đời:** RAM managed permissions, IAM và SCP kiểm soát quyền; OU membership, phiên bản permission và nhật ký thay đổi cần được theo dõi khi mở rộng account.
- **Đưa kiểm tra bảo mật vào quá trình phát triển:** Phần Terraform và pentest SIT/UAT cho thấy hai thời điểm kiểm tra khác nhau: khi review thay đổi và khi hệ thống đã có môi trường chạy thử.
- **Quan tâm đến toàn bộ vòng đời agent:** Runtime, memory, context window và guardrails đều ảnh hưởng đến cách agent hỗ trợ công việc liên tục. Phiên Kiro Crew đặt các vấn đề này vào cùng bài toán quản lý hiệu quả và mức tự chủ.

### Trải nghiệm tại sự kiện

Phần live demo AI chủ động gọi đến ứng viên để sơ tuyển Cloud Engineer bằng tiếng Việt trong khoảng ba phút là một điểm nhấn đáng nhớ. Luồng từ chào hỏi, tìm hiểu kinh nghiệm đến câu hỏi kỹ thuật giúp mình thấy rõ cách Voice AI hỗ trợ một công đoạn cụ thể của tuyển dụng. Các tình huống ngắt lời, thuật ngữ tiếng Anh và yêu cầu làm lệch đánh giá còn cho thấy cần xem cả cách hệ thống xử lý hội thoại, bên cạnh chất lượng giọng nói.

Session RAM giúp mình hình dung rõ hơn bài toán enterprise: nhiều team và account cùng dùng hạ tầng nhưng vẫn cần ranh giới sở hữu, quyền truy cập và trách nhiệm vận hành. Giá trị của việc chia sẻ tăng lên khi doanh nghiệp chuẩn hóa tài nguyên theo OU và mở rộng thêm account.

Session Kiro Crew là phần mình tâm đắc nhất, đặc biệt buổi hỏi đáp kỹ thuật cuối chương trình. Các câu hỏi về Artifacts, token, memory và quyền thực thi gắn trực tiếp với mong muốn để agent hỗ trợ review code, kiểm tra hệ thống 24/7 nhưng vẫn kiểm soát được hành động và chi phí.
