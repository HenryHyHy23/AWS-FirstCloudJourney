---
title: "Đề xuất Dự án"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Bite to Business

### 1. Tóm tắt Dự án

Nhóm mình xây dựng **Bite to Business** trong chương trình thực tập AWS First Cloud Journey: một website bán thực phẩm cho khách hàng doanh nghiệp (nhà hàng, khách sạn, suất ăn) và cá nhân. Hệ thống hỗ trợ luồng xem sản phẩm → lập báo giá → khách xác nhận → thanh toán QR qua SePay → giao hàng.

Điểm nổi bật là **Generative UI**: giao diện thay đổi theo ý định và thói quen của từng khách. Backend gọi **Amazon Bedrock (Amazon Nova Pro)**, có **Bedrock Guardrails** kiểm tra nội dung, rồi sinh giao diện **A2UI** từ catalog component của team. Ứng dụng dùng React, Spring Boot và Supabase PostgreSQL, triển khai trên **AWS** tại Singapore (`ap-southeast-1`), có CI/CD tự động bằng GitHub Actions.

### 2. Bài toán Cần giải quyết

#### Vấn đề là gì?

Người mua B2B có nhiều tác vụ khác nhau: tìm nguyên liệu, so sánh giá theo số lượng, mua lại hàng quen hoặc chia hàng cho nhiều địa chỉ.

- **Giao diện cố định** bắt người mua tìm, lọc và chuyển trang nhiều lần.
- **Giao diện do AI sinh ra** phải đúng dữ liệu, đúng quyền và giữ được ngữ cảnh khi khách đổi yêu cầu.
- **AI phản hồi chậm hoặc lỗi** không được làm gián đoạn việc mua hàng.

#### Giải pháp

- **Cá nhân hóa giao diện:** AI chọn bố cục từ catalog component, ví dụ bảng khi mua số lượng lớn hoặc danh sách mua lại khi khách quay lại. Hồ sơ thói quen được tổng hợp từ tương tác của khách.
- **Kiểm tra hai lớp:** Backend kiểm tra thông điệp A2UI trước khi gửi; frontend chỉ render component có trong catalog.
- **Backend kiểm soát giao dịch:** Giá, ưu đãi, VAT, tồn kho và quyền luôn do backend kiểm tra. Khi khách xác nhận, hệ thống giữ hàng tối đa 6 giờ và chỉ cập nhật thanh toán khi webhook SePay hợp lệ.
- **Fallback khi AI lỗi:** Quay về giao diện cơ bản và giữ bản nháp để khách tiếp tục.

#### Nghiệp vụ theo vai trò

| Vai trò | Nghiệp vụ chính |
| --- | --- |
| **Customer** | Đăng ký (Admin duyệt), lập báo giá, dùng mã giảm giá, chia kiện giao nhiều địa chỉ, xác nhận và thanh toán QR, theo dõi đơn, mua lại, yêu cầu sản phẩm mới, đổi trả kiện trong 24 giờ, yêu cầu hoàn tiền, đánh giá sản phẩm, dùng trợ lý AI |
| **E-commerce Executive** | Quản lý sản phẩm, danh mục, thương hiệu, giá và VAT; tạo khuyến mãi, coupon; theo dõi báo giá; kiểm duyệt đánh giá; xử lý hoàn tiền; xem dashboard kinh doanh |
| **Logistics Staff** | Nhập kho, điều chỉnh tồn kho, quản lý sản phẩm kho và nhà phân phối, phản hồi yêu cầu sản phẩm mới của khách |
| **Delivery Staff** | Bật trạng thái sẵn sàng, nhận kiện cần giao, xem tuyến giao trên bản đồ, xác nhận giao xong |
| **Admin** | Duyệt đăng ký khách, tạo tài khoản nhân viên, xử lý giao dịch SePay cần đối soát, xử lý đổi trả kiện, giám sát hệ thống |

#### Lợi ích và giá trị mang lại

- **Mua hàng nhanh hơn:** Khách nói điều mình cần, giao diện tự sắp xếp thông tin phù hợp thay vì phải tự tìm và lọc.
- **Dữ liệu luôn đúng:** Giá, tồn kho và trạng thái đơn đến từ backend, AI không tự tạo số liệu.
- **Quy trình liền mạch:** Từ báo giá, thanh toán, kho đến giao hàng nằm trên cùng một hệ thống cho 5 vai trò.
- **Chi phí kiểm soát được:** Hạ tầng khoảng 85 USD/tháng ở quy mô MVP, có giới hạn lượt gọi AI và cảnh báo ngân sách.
- **Vận hành an toàn:** WAF, Secrets Manager, CI/CD không dùng access key dài hạn, cảnh báo qua email khi có sự cố.
- **Mở rộng được:** ECS tự co giãn 1–3 task; thông báo theo hướng sự kiện dễ thêm loại thông báo mới.

### 3. Kiến trúc Giải pháp

![Sơ đồ kiến trúc AWS của Bite to Business](/images/2-Proposal/BTB-AWS-Architecture-Page-1.drawio.png)

**Luồng request:** Người dùng → CloudFront (+ WAF) → S3 (web) hoặc `/api/*` → ALB (+ WAF) → ECS Fargate. Chỉ webhook SePay đi thẳng vào ALB.

#### Dịch vụ AWS sử dụng

- **Amazon Route 53:** Quản lý tên miền.
- **AWS Certificate Manager:** Cấp chứng chỉ HTTPS.
- **Amazon CloudFront:** Cửa vào duy nhất cho web và API, phân phối frontend.
- **AWS WAF:** Lọc request xấu, giới hạn tốc độ cho đăng nhập, OTP, AI và API.
- **Amazon S3:** Lưu bản build frontend và log của ALB.
- **Elastic Load Balancing (ALB):** Chuyển request API tới backend.
- **Amazon ECS (Fargate) + Application Auto Scaling:** Chạy backend Spring Boot, tự co giãn 1–3 task.
- **Amazon ECR:** Lưu Docker image của backend.
- **Amazon VPC:** Mạng 2 AZ, S3 Gateway Endpoint, VPC Flow Logs.
- **AWS Secrets Manager:** Lưu mật khẩu database và khóa tích hợp.
- **AWS IAM:** Phân quyền tối thiểu cho task, Lambda và GitHub Actions (OIDC).
- **Amazon Bedrock:** Model Nova Pro và Guardrails cho Generative UI.
- **Amazon EventBridge, Amazon SQS, AWS Lambda:** Xử lý sự kiện nghiệp vụ và thông báo bất đồng bộ.
- **Amazon SES:** Gửi email cho khách và nhân viên.
- **Amazon CloudWatch:** Log, alarm, RUM và số liệu cho dashboard Admin.
- **Amazon Cognito:** Identity Pool để CloudWatch RUM gửi dữ liệu.
- **Amazon SNS:** Gửi cảnh báo hệ thống qua email.
- **Amazon Athena:** Truy vấn log ALB.
- **AWS Budgets:** Theo dõi chi phí hằng tháng.

Ngoài AWS: Supabase (PostgreSQL + Auth), SePay (thanh toán), Cloudinary (ảnh), Vietmap (bản đồ), GitHub Actions (CI/CD).

#### Thiết kế thành phần

- **Lớp truy cập và bảo vệ:** Route 53, CloudFront, WAF, ACM nhận mọi request và chặn truy cập xấu.
- **Lớp frontend:** React chạy từ S3 qua CloudFront, gồm giao diện cơ bản, trợ lý AI và runtime A2UI.
- **Lớp ứng dụng:** Spring Boot trên ECS Fargate sau ALB, xử lý nghiệp vụ, phân quyền và điều phối AI.
- **Lớp AI:** Bedrock (Nova Pro) và Guardrails, cùng các tool nghiệp vụ và bộ kiểm tra A2UI.
- **Lớp dữ liệu:** Supabase PostgreSQL và Supabase Auth.
- **Lớp sự kiện và thông báo:** Outbox → EventBridge/SQS → Lambda → SES và thông báo web.
- **Lớp giám sát:** CloudWatch, SNS, Athena, Budgets và dashboard giám sát cho Admin.

#### AI Pipeline

1. **Xác thực và hạn mức:** Chỉ Customer đã đăng nhập được dùng AI, tối đa 20 lượt mỗi giờ.
2. **Guardrails:** Bedrock Guardrails kiểm tra câu khách nhập; nội dung bị chặn thì trả lời từ chối, không gọi model.
3. **Nhận diện ý định:** Nova Pro xác định khách muốn gì (tìm sản phẩm, so sánh, mua lại…) và trích thông tin cần thiết.
4. **Lấy dữ liệu thật:** Backend gọi tool tìm sản phẩm, dữ liệu trang hoặc mua lại, kèm hồ sơ thói quen của khách. Giá và tồn kho do backend gắn vào giao diện; model chỉ thấy tên và số lượng kết quả.
5. **Sinh giao diện:** Nova Pro tạo bố cục A2UI từ catalog component.
6. **Kiểm tra:** Backend kiểm tra component, thuộc tính, action được phép và không cho model tự ghi số tiền. Sai thì thử lại một lần, vẫn sai thì dùng bố cục mặc định.
7. **Hiển thị:** Kết quả stream (SSE) về frontend và render bằng component đã đăng ký. Câu hỏi ngoài phạm vi được trả lời bằng văn bản.

### 4. Triển khai Kỹ thuật

#### Các giai đoạn triển khai

1. **Yêu cầu và database** – Hoàn thành
2. **Thiết kế UI và repository** – Hoàn thành
3. **Phát triển API, giao diện và phân quyền** – Hoàn thành
4. **Triển khai hạ tầng AWS và CI/CD** – Hoàn thành
5. **Tích hợp SePay, Vietmap, Cloudinary, Bedrock và Generative UI** – Hoàn thành
6. **Kiểm thử, tối ưu chi phí và đo hiệu quả Generative UI** – Đang thực hiện

#### Yêu cầu kỹ thuật

| Nhóm | Công nghệ |
| --- | --- |
| **Frontend** | React 19, TypeScript, Vite, Tailwind CSS, TanStack Query, Zustand, i18next (Việt/Anh) |
| **Backend** | Java 21, Spring Boot 4.1, Spring Security, JPA, Flyway |
| **Dữ liệu và xác thực** | Supabase PostgreSQL, Supabase Auth (JWT) |
| **AI và Generative UI** | Amazon Bedrock (Nova Pro), Bedrock Guardrails, A2UI |
| **Hạ tầng AWS** | Route 53, CloudFront, ACM, WAF, S3, ALB, ECS Fargate, ECR, Secrets Manager |
| **Sự kiện và email** | EventBridge, SQS, Lambda, SES |
| **Giám sát** | CloudWatch (Logs, Alarms, RUM), SNS, Athena, AWS Budgets |
| **CI/CD** | Docker, GitHub Actions + OIDC |
| **Tích hợp ngoài** | SePay, Cloudinary, Vietmap |

### 5. Tiến độ và Mốc thực hiện

| Thời gian | Công việc chính | Kết quả |
| --- | --- | --- |
| **18/09–23/09** | Chốt ý tưởng, viết requirement, tạo repository | Repository frontend/backend, tài liệu yêu cầu |
| **25/09–07/10** | Thiết kế database và giao diện | Schema và migration Flyway đầu tiên, thiết kế Figma |
| **09/10–21/10** | Phát triển API nghiệp vụ, đăng nhập, giao hàng; thử nghiệm Bedrock và A2UI | API chính cho các vai trò, xác thực Supabase Auth |
| **23/10–28/10** | Báo giá, đơn hàng, thanh toán SePay | Luồng báo giá → thanh toán QR chạy được |
| **30/10–04/11** | Đa ngôn ngữ, đóng gói Docker | Giao diện Việt/Anh, Docker image backend |
| **06/11–11/11** | Triển khai AWS và CI/CD; thông báo qua SQS, Lambda, SES | Website chạy trên AWS, pipeline GitHub Actions, CloudWatch RUM |
| **13/11–18/11** | Hoàn thiện Generative UI, đối soát SePay, dashboard giám sát, đưa API ra sau CloudFront | Trợ lý AI và trang do AI dựng, hồ sơ thói quen giao diện |
| **Từ 20/11** | Kiểm thử, đo hiệu quả Generative UI, tài liệu và workshop | Kết quả so sánh, tài liệu triển khai, demo |

### 6. Ước tính Ngân sách (Chi phí AWS)

Giá region Singapore, tháng 11/2026, chưa gồm thuế và dịch vụ ngoài AWS.

| Dịch vụ | 1 task | 3 task (tối đa) |
| --- | --- | --- |
| ECS Fargate (0,5 vCPU / 2 GB mỗi task) | ~ 26,5 USD | ~ 79,6 USD |
| Application Load Balancer | ~ 24,2 USD | ~ 24,2 USD |
| IPv4 công khai | ~ 11,0 USD | ~ 18,3 USD |
| AWS WAF | ~ 21 USD | ~ 21 USD |
| Các dịch vụ còn lại (S3, CloudFront, Route 53, CloudWatch, Secrets Manager…) | vài USD | vài USD |
| **Tổng hạ tầng / tháng** | **~ 85 USD** | **~ 145 USD** |

**Chi phí AI (Nova Pro):** khoảng 0,0067 USD mỗi lần gọi (giả định 3.000 token vào, 800 token ra), tức khoảng 6,7 USD cho 1.000 lần gọi và 67 USD cho 10.000 lần gọi mỗi tháng.

### 7. Đánh giá Rủi ro

#### Ma trận rủi ro

| Rủi ro | Mức độ | Mô tả |
| --- | --- | --- |
| **Bảo mật dữ liệu** | Cao | Lộ dữ liệu khách hàng, webhook thanh toán giả mạo, prompt injection vào AI |
| **Chi phí vượt dự kiến** | Trung bình | Lạm dụng AI hoặc bị tấn công làm tăng số task và lượt gọi model |
| **Gián đoạn AI** | Trung bình | Model chậm, kết nối stream bị ngắt sau 60 giây im lặng, AI trả kết quả sai cấu trúc |
| **Phụ thuộc dịch vụ ngoài** | Trung bình | Supabase, SePay, Vietmap, Cloudinary gặp sự cố |

#### Chiến lược giảm thiểu

- **Bảo mật:** Mọi lưu lượng đi qua CloudFront và WAF; ALB chỉ nhận request từ CloudFront; secrets nằm trong Secrets Manager; xác thực webhook SePay; AI chỉ nhận dữ liệu trong phạm vi quyền và có Guardrails.
- **Chi phí:** Giới hạn tối đa 3 task, 20 lượt AI mỗi giờ cho mỗi khách, AWS Budgets và alarm qua SNS.
- **Gián đoạn AI:** Gửi heartbeat trong luồng stream, kiểm tra và thử lại kết quả A2UI, fallback về giao diện cơ bản.
- **Dịch vụ ngoài:** Timeout, retry, đối soát thanh toán định kỳ và thông báo lỗi rõ ràng cho người dùng.

### 8. Kết quả Dự kiến

#### Cải thiện kỹ thuật

- **Giao diện cá nhân hóa:** Generative UI thích ứng theo ý định và thói quen của từng khách, có fallback an toàn.
- **Quy trình đầy đủ:** Báo giá → thanh toán → kho → giao hàng chạy trên cùng hệ thống, kèm thông báo web và email.
- **Vận hành tự động:** CI/CD, auto scaling, giám sát và cảnh báo chi phí.

#### Giá trị lâu dài

- **Số liệu đánh giá:** Kết quả so sánh giao diện cố định và giao diện cá nhân hóa (thời gian hoàn thành tác vụ, số thao tác, tỷ lệ lỗi).
- **Nền tảng mở rộng:** Dễ thêm tool AI, loại thông báo và vai trò mới.
- **Kiến thức của nhóm:** Kinh nghiệm thực tế với Bedrock, ECS, CloudFront, WAF và kiến trúc hướng sự kiện trên AWS.
