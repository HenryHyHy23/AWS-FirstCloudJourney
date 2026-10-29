---
title: "Event 2"
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

# Bài thu hoạch: AWS Cloud Day and AI

**Địa điểm:** Floor 26, Bitexco Financial Tower, TP. Hồ Chí Minh.

**Vai trò:** Người tham dự.

AWS Cloud Day and AI kết nối kiến trúc cloud, hiện đại hóa dữ liệu và Agentic AI với các bài toán doanh nghiệp. Bài thu hoạch tổng hợp tư duy kỹ thuật từ keynote cùng các nội dung tiêu biểu của chương trình, tập trung vào điều kiện để hệ thống ứng dụng AI vận hành tin cậy, có kiểm soát và tạo giá trị thực tế.

### Mục tiêu sự kiện

- Hiểu mối liên hệ giữa đặc điểm workload, tư duy hệ thống và năng lực vận hành.
- Tìm hiểu cách nền tảng dữ liệu, kiến trúc agent và phát triển phần mềm có AI hỗ trợ kết hợp trong toàn bộ vòng đời ứng dụng.
- Liên hệ các case doanh nghiệp với quyết định về độ tin cậy, bảo mật, chi phí và trách nhiệm của con người.

### Diễn giả tiêu biểu

Diễn giả của phần keynote và 39 phiên trong chương trình, được nhóm theo đơn vị.

| Đơn vị / Nhóm | Diễn giả và vai trò |
| --- | --- |
| Amazon / Keynote | **Dr. Werner Vogels** – Chief Technology Officer and Vice President, Amazon (Keynote) |
| AWS – Solutions Architects | **Phuc Nguyen**, **Diep Nguyen Hoang**, **Dzung Ha**, **Khoi Phan**, **Quan Phuong**, **Trung Nguyen**, **Thanh Dang**, **Taiki Dang**, **Hien** |
| AWS – Senior / Specialist Solutions Architects | **Thi Nguyen**, **Lai Trung Hieu**, **Huy Tran** – Senior Solutions Architect; **Trung Tri Nguyen** – Analytics Specialist Solutions Architect; **Toan Huynh** – Specialist Solutions Architect |
| AWS – Customer / Account Management | **Hung Hoang**, **Linh Pham** – Senior Customer Solutions Manager; **Luu Thuy Linh** – Account Manager; **Tan Huynh**, **Trung Nguyen** – Technical Account Manager |
| AWS Professional Services | **Thanh Nguyen** – Senior Delivery Consultant |
| VPBank | **Thai Hong Pham** – Head of Enterprise Data Architect; **Yen Trinh** – Application Platform Engineer |
| Techcom Securities (TCBS) | **Đoàn Thế Long** – Senior Manager, Software Engineering |
| Vietnam International Bank (VIB) | **Tuan Le** – Head of AI Center; **Hoang Nguyen** – AWS Cloud Engineer |
| TechX Corporation | **Ngô Mạnh Hà** – CEO |
| Techcom Life | **Pham Tan Anh Khoa** – Head of Technology Governance & Delivery; **Vu Quang Tam** – Project Manager, AI Initiatives |
| G-AsiaPacific Vietnam | **Vo Xuan Minh Tuan** – Senior Solutions Architect; **Phạm Thị Hồng Hạnh** – Cloud Consultant |
| FCAJ | **Phuong Nguyen Huu** – Cloud Engineering; **Bui Huy Hoang** – Data Engineer |
| VinUni | **Nguyen Duc Manh** – AI Researcher |
| Canon Vietnam | **Dinh Ngoc Hieu** – System Administrator |
| SoftwareOne Việt Nam | **Nguyen Huu Toan** – Solution Architect |
| eCloudvalley | **Vu Gia Khanh** – Solution Architect Lead |
| Cloud Kinetics | **Dung Luong Tien** – Cloud Engineer |
| Katalon | **Vu Bui** – Technical Director |
| NTQ Solution | **Phạm Minh Quang** – DevOps Engineer |
| Renova Cloud | **Nguyễn Thái Bình** – Head of DevOps |
| Datadog | **Bang Truong** – Sales Engineer |
| Vingroup | **Kha Văn** – Cloud Infrastructure Security Specialist |
| GoTymeX | **Nguyễn Trung Trực** – Senior Platform Engineer; **Huỳnh Văn Toán** – Platform Lead |

### Keynote: Tư duy hệ thống và Renaissance Developer

#### 1. Chọn kiến trúc theo cách sử dụng thực tế

- Câu chuyện về sự cố cơ sở dữ liệu lớn tại Amazon cho thấy giới hạn của việc dùng một công nghệ cho mọi kiểu truy cập. Tra cứu key-value đơn giản và nghiệp vụ cần quan hệ dữ liệu có nhu cầu khác nhau; việc phân tích các nhu cầu này góp phần định hướng phát triển Dynamo.
- Bài học là lựa chọn data store dựa trên mẫu truy vấn, yêu cầu nhất quán, quy mô và hành vi khi có lỗi.

#### 2. Thiết kế và đo lường với giả định có sự cố

- Tail latency thể hiện các request chậm mà số trung bình hoặc trung vị có thể che khuất. P99.9 là ngưỡng độ trễ bao phủ khoảng 99,9% request quan sát được, khác với tỷ lệ sẵn sàng của dịch vụ.
- Cô lập lỗi, kiểm tra failover, quy trình khôi phục và diễn tập GameDay giúp biến giả định về khả năng chịu lỗi thành điều có thể kiểm chứng. Sẵn sàng vận hành còn bao gồm hiểu cách các thành phần phụ thuộc phản ứng khi xảy ra sự cố.

#### 3. Phát triển năng lực vượt ra ngoài sinh mã

- Renaissance Developer kết hợp sự tò mò, tư duy hệ thống, ownership, chuyên môn hình chữ T và kỹ năng giao tiếp. Hiểu mục tiêu nghiệp vụ, giải thích được đánh đổi kiến trúc vẫn cần thiết khi AI giúp triển khai nhanh hơn.
- Đầu ra do AI tạo vẫn cần kiểm tra tính đúng đắn, bảo mật và tình huống biên. Kỹ sư chịu trách nhiệm với thay đổi đưa lên production và cần đánh giá ảnh hưởng trên toàn hệ thống.

### Nội dung chính và Key Highlights

#### 1. Nền tảng dữ liệu sẵn sàng cho AI và phân tích doanh nghiệp

- Case VPBank mô tả việc hợp nhất hơn 600 TB từ các kho dữ liệu cũ lên nền tảng phân tích Redshift, kết hợp lakehouse và chia sẻ dữ liệu giữa các team. Vấn đề kiến trúc là hỗ trợ phân tích dùng chung đồng thời kiểm soát sao chép dữ liệu, hiệu năng và governance.
- Các phiên dữ liệu khác đề cập Amazon S3, Apache Iceberg, open catalog và quản trị nhất quán. Độ mới, lineage, chất lượng và quyền truy cập quyết định dữ liệu truy xuất có phù hợp làm căn cứ cho câu trả lời hoặc hành động của agent hay không.

#### 2. AI-native SDLC và kiểm chứng chất lượng độc lập

- TCBS chia sẻ việc áp dụng Kiro, AI-DLC và nêu mức giảm 26% thời gian phát triển trong case của đơn vị. Chủ đề rộng hơn là đưa AI vào yêu cầu, thiết kế, kiểm thử, tài liệu và bàn giao, thay vì chỉ đo tiến độ bằng lượng mã sinh ra.
- Spec-driven development giúp làm rõ yêu cầu, tiêu chí chấp nhận. Ví dụ về chất lượng phần mềm của Katalon nhấn mạnh kiểm chứng độc lập với phần việc AI tạo, lưu bằng chứng kiểm tra và quyết định để có thể truy vết.

#### 3. Kiến trúc agent, framework và dịch vụ vận hành

- Các phiên về agent harness, Strands, LangGraph, Bedrock AgentCore phân tích điều phối, thực thi công cụ, state, memory và observability. Luồng xử lý trong framework và các dịch vụ vận hành được quản lý giải quyết những phần khác nhau của hệ thống.
- Vòng lặp ReAct, công cụ MCP, truy xuất tri thức và chuyên môn hóa nhiều agent nối suy luận với hành động, ngữ cảnh bên ngoài. Thêm agent hoặc công cụ cũng tạo đánh đổi về phối hợp, độ trễ; giá trị cần xuất phát từ yêu cầu nhiệm vụ.
- MCP và Knowledge Server trên codebase bổ sung hướng dẫn AWS, đồ thị phụ thuộc cho coding assistant, giúp đánh giá phạm vi ảnh hưởng của thay đổi.

#### 4. GitOps, Platform Engineering và hiện đại hóa

- Ví dụ Vinamilk kết hợp AI hỗ trợ hạ tầng với GitOps, EKS, Argo CD, bước phê duyệt của con người và quyền truy cập tạm thời. Các phiên IaC liên quan sử dụng pull request để review Terraform/OpenTofu được tạo và xử lý configuration drift.
- Case NestJS/Angular chuyển từ EC2/PM2 sang EKS với FluxCD minh họa Git là nguồn cấu hình chuẩn và rollback bằng Git revert. IDP trên EKS dùng Backstage, Crossplane, Terragrunt để chuẩn hóa cấp phát đa account, giảm TicketOps.
- Các phiên EKS Hybrid Nodes và AWS Transform đề cập container hóa, hiện đại hóa SQL Server/.NET, lập kế hoạch migration theo đợt; vẫn cần kiểm tra phụ thuộc, tương thích và phương án cutover.

#### 5. Bảo mật, danh tính workload và tự chủ có kiểm soát

- Các phiên workload identity liên hệ IAM, STS, phiên ngắn hạn và ngữ cảnh danh tính với least privilege giữa dịch vụ, account. Loại bỏ static key mới là một phần của việc xác định workload nào được phép thực hiện hành động.
- Defense in depth và governance kết hợp bảo vệ dữ liệu, đánh giá, chính sách và phê duyệt của con người. Phạm vi truy cập của agent cần mở rộng dựa trên bằng chứng đã kiểm chứng, thẩm quyền nghiệp vụ và người chịu trách nhiệm rõ ràng.
- Landing Zone cho DevOps Agent đặt vấn đề quyền đọc hạ tầng, tuân thủ và chi phí ẩn. AWS Security Agent bổ sung review bảo mật theo yêu cầu tổ chức và pentest ứng dụng theo yêu cầu.

#### 6. Kiểm soát chi phí và observability cho GenAI

- Các phiên kiểm soát chi phí Bedrock, model routing đề cập prompt dài, agent loop lặp lại, lưu lượng tăng đột biến và chọn model theo độ khó nhiệm vụ. Theo dõi mức dùng cần gắn với giới hạn và kill switch gần thời gian thực; model mạnh chưa chắc hiệu quả nhất cho mọi request.
- Nội dung monitoring bổ sung chi phí, an toàn và chất lượng câu trả lời bên cạnh latency, errors, traffic, saturation. Phiên S3 storage tiering nêu đánh đổi tương tự: lớp lưu trữ rẻ hơn còn phải đáp ứng tốc độ truy xuất, tần suất truy cập và yêu cầu khôi phục.

#### 7. Ứng dụng nghiệp vụ và vận hành dựa trên bằng chứng

- TechcomLife minh họa bảo hiểm cloud-native. Amazon Quick Suite và Enterprise Knowledge Hub nối dữ liệu phân mảnh với hành động có kiểm soát; workshop Quick đề cập tạo agent, knowledge base, prompt, skills và nghiên cứu chuyên sâu.
- Fraud detection kết hợp Iceberg/Athena, đồ thị Neptune, XGBoost và GraphRAG để điều tra liên kết gian lận. Sản xuất tập trung nối dữ liệu SCADA, Historian, MES thành Industrial Data Fabric phục vụ phân tích có ngữ cảnh.
- Quy trình điều tra GoTymeX dùng AWS DevOps Agent liên hệ APM trace, Datadog metric và tín hiệu hạ tầng giữa các service, account; nội dung Xora của TechX kết nối ngữ cảnh sự cố với khôi phục do con người kiểm soát, xác minh kết quả. Một giả thuyết nguyên nhân hợp lý vẫn cần chứng cứ và kiểm tra sau khôi phục.

### Kiến thức và bài học rút ra

- **Bắt đầu từ workload:** Xác định kiểu truy cập, chất lượng kỳ vọng và ràng buộc nghiệp vụ trước khi chọn database, model hoặc kiến trúc agent.
- **Kỹ sư giữ ownership:** Rà soát yêu cầu, thay đổi đề xuất, bằng chứng kiểm thử và phương án rollback dù AI thực hiện phần lớn việc triển khai.
- **Xem production readiness là bài toán toàn hệ thống:** Governance dữ liệu, danh tính, chịu lỗi, observability và kiểm soát chi phí cần phối hợp với nhau.
- **Đo kết quả nghiệp vụ:** Đánh giá nhiệm vụ hoàn thành, khôi phục tin cậy, chất lượng bàn giao và chi phí trên kết quả hữu ích, bên cạnh tốc độ viết mã.

---

![Đã Check in tại sự kiện](/images/4-EventParticipated/4.2-Event2/check-in.png)

_Đã Check in tại sự kiện_
