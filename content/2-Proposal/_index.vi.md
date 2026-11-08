---
title: "Đề xuất Dự án"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Bite to Business

### 1. Tóm tắt Dự án

**Bite to Business** là dự án xây dựng website bán thực phẩm, ưu tiên khách hàng doanh nghiệp như nhà hàng, khách sạn và đơn vị cung cấp suất ăn, đồng thời hỗ trợ cá nhân/hộ gia đình. Hệ thống hỗ trợ quy trình từ xem sản phẩm, lập yêu cầu mua (Quote), nhân viên duyệt đến xác nhận đơn khi đủ hàng, chuẩn bị và giao hàng.

Điểm nổi bật là **Generative UI**, giúp điều chỉnh giao diện theo ý định, sở thích và ngữ cảnh mua hàng của từng người dùng. Hệ thống dự kiến tích hợp **Amazon Bedrock**, với **Claude Haiku 5.5** được đề xuất làm model chính cho MVP, để gợi ý sản phẩm và sinh cấu hình UI dạng JSON từ thư viện component do team xây dựng. React hiển thị cấu hình đã kiểm tra; backend quản lý quyền, giá, tồn kho và giao dịch. AI cũng hỗ trợ chatbot theo quyền của từng vai trò và soạn nội dung/bố cục poster để nhân viên E-commerce xem trước, chỉnh sửa và duyệt.

Ứng dụng sử dụng **React/TypeScript**, **Spring Boot** và **Supabase PostgreSQL**, với kế hoạch triển khai trên **AWS**. Dự án hướng tới đơn giản hóa thao tác mua hàng, hỗ trợ phối hợp giữa bán hàng, kho và giao nhận, đồng thời đánh giá hiệu quả của giao diện cá nhân hóa.

### 2. Bài toán Cần giải quyết

#### Vấn đề là gì?

Trong mua thực phẩm B2B, khách hàng có thể cần tìm nguyên liệu mới, so sánh giá theo số lượng, mua lại hàng quen thuộc hoặc chia hàng cho nhiều địa chỉ. Mỗi tác vụ cần cách ưu tiên thông tin và thao tác khác nhau.

- **Giao diện cố định chưa phù hợp với từng nhu cầu:** Một bố cục chung có thể khiến người mua phải tìm kiếm, lọc và chuyển trang nhiều lần, kể cả khi đã diễn đạt rõ mục tiêu.
- **Dễ mất ngữ cảnh khi giao diện thay đổi:** Sản phẩm, số lượng, đơn vị bán, Quote và nhóm giao hàng đang chọn cần được giữ nhất quán khi khách hỏi tiếp hoặc đổi cách hiển thị.
- **Giao diện sinh ra cần đúng dữ liệu và quy trình:** Thay đổi số lượng có thể làm đổi bậc giá, tổng tiền hoặc khả năng đáp ứng. UI cần hiển thị dữ liệu backend hiện hành và chỉ cung cấp thao tác phù hợp với quyền, trạng thái duyệt.
- **Phản hồi chậm hoặc lỗi gây gián đoạn:** Kết quả từ yêu cầu cũ có thể ghi đè thao tác mới; quá trình tạo UI bị ngắt có thể làm mất tiến trình hoặc để lại thông tin lỗi thời.

Bài toán của **Bite to Business** là tạo giao diện thích ứng theo ý định và ngữ cảnh của từng người dùng, đồng thời duy trì tác vụ mua hàng và bảo đảm giao dịch đúng quy trình.

#### Giải pháp

**Bite to Business** đề xuất tích hợp **Generative UI** qua **Amazon Bedrock**, với **Claude Haiku 5.5** là model được đề xuất cho MVP. Giải pháp gồm:

- **Cá nhân hóa theo tác vụ:** Backend cung cấp ngữ cảnh và sở thích liên quan; AI đề xuất bố cục từ catalog component của team. Ví dụ, ưu tiên bảng khi mua số lượng lớn hoặc danh sách mua lại khi khách quay lại.
- **Runtime A2UI có kiểm tra:** Tiếp nhận thông điệp JSON theo phiên bản giao thức đã chọn, kiểm tra schema, component, data binding và action được phép, rồi render bằng thư viện React của website.
- **Giữ ngữ cảnh tương tác:** Gửi định danh sản phẩm, số lượng, Quote và phiên bản trạng thái cùng mỗi thao tác về backend. Áp dụng kết quả còn phù hợp với tác vụ hiện tại và giữ lựa chọn hợp lệ khi đổi bố cục.
- **Backend kiểm soát giao dịch:** Giá, ưu đãi, tồn kho, quyền và bước duyệt được kiểm tra tại backend. Xác nhận Order chỉ thành công khi đủ toàn bộ hàng, đã qua bước duyệt và người mua xác nhận.
- **Fallback khi AI lỗi:** Giữ phần giao diện hợp lệ hoặc chuyển về giao diện cơ bản, bảo toàn bản nháp khi có thể và thông báo trạng thái để người dùng tiếp tục thao tác.

Hiệu quả sẽ được so sánh với giao diện cố định qua tỷ lệ và thời gian hoàn thành tác vụ, số thao tác, độ đúng của render/data binding và khả năng phục hồi khi lỗi.

### 3. Kiến trúc Giải pháp

**Bite to Business** dự kiến triển khai trên AWS, với các thành phần:

| Thành phần | Vai trò |
| --- | --- |
| **Frontend** | React/TypeScript trên S3 private + CloudFront; gồm giao diện cơ bản và A2UI runtime. |
| **Backend** | Spring Boot trên ECS Fargate private, sau ALB; xử lý API, quyền và nghiệp vụ. |
| **Dữ liệu / Xác thực** | Supabase PostgreSQL lưu dữ liệu; Supabase Auth quản lý xác thực. |
| **AI** | Backend gọi Amazon Bedrock — Claude Haiku 5.5 để gợi ý sản phẩm và đề xuất cấu hình UI. |
| **Tích hợp ngoài** | Cloudinary cho ảnh, Vietmap cho bản đồ, SePay cho thanh toán và SMTP cho email. |
| **Vận hành** | ECR lưu image, Secrets Manager quản lý secrets, CloudWatch thu log và metric. |

**Luồng Generate UI:** Yêu cầu và ngữ cảnh → backend xác thực/lấy dữ liệu → Bedrock đề xuất UI → backend kiểm tra/chuyển thành A2UI → runtime kiểm tra và render bằng React.

Các action quay về backend để kiểm tra quyền, giá, tồn kho và giao dịch. Runtime giữ ngữ cảnh, loại phản hồi cũ và fallback về giao diện cơ bản khi AI lỗi.

### 4. Triển khai Kỹ thuật

#### Stack Công nghệ

| Nhóm | Công nghệ |
| --- | --- |
| **Frontend** | React, TypeScript, Vite, Tailwind CSS, React Router |
| **State và dữ liệu API** | TanStack Query, Zustand, Axios |
| **Form và validation** | React Hook Form, Zod |
| **Backend** | Java 21, Spring Boot, Spring Security, Spring Data JPA/Hibernate |
| **Database / Migration** | Supabase PostgreSQL, Flyway |
| **Authentication** | Supabase Auth, JWT |
| **Generative UI — dự kiến** | A2UI, JSON Schema, React component catalog và runtime |
| **AI — dự kiến** | Amazon Bedrock, Claude Haiku 5.5 |
| **Triển khai — dự kiến** | Docker, ECR, ECS Fargate, ALB, S3, CloudFront |
| **Giám sát và secrets — dự kiến** | CloudWatch, Secrets Manager |
| **Tích hợp ngoài** | Cloudinary (dự kiến), Vietmap, SePay, SMTP |
| **Đa ngôn ngữ** | i18next, react-i18next |

#### Các Giai đoạn

- **Giai đoạn 1: Core Foundation (Tháng 1)**
  - Thiết lập hạ tầng `AWS` (`Terraform`).
  - Xây dựng Authentication (`Cognito`).
  - `CRUD` cơ bản cho Hồ sơ (`DynamoDB` + `Lambda`).
- **Giai đoạn 2: Editor & Templates (Tháng 2)**
  - Phát triển giao diện Editor kéo thả.
  - Xây dựng hệ thống Template động.
  - Tính năng xuất `PDF` (`html2pdf.js` / `Puppeteer`).
- **Giai đoạn 3: AI Integration & Polish (Tháng 3)**
  - Tích hợp `Amazon Bedrock` cho tính năng "AI Writer".
  - Tối ưu hóa `UX`/`UI`.
  - Kiểm thử tải và bảo mật.

### 5. Ước tính Ngân sách (Chi phí AWS)

Mô hình định giá `Serverless` giúp chi phí tỉ lệ thuận với mức sử dụng. Chi phí AI (`Bedrock`) sẽ là yếu tố mới cần quan tâm so với các app truyền thống.

#### Giả định

- 1 Resume = 5 lần gọi AI (Tối ưu hóa, viết lại description).
- 1 lần gọi AI = 1.000 input tokens + 500 output tokens.
- Giá `Bedrock` (`Claude 3 Sonnet`): $3.00/1M input, $15.00/1M output.
- Chi phí AI/Resume: ~ $0.05 (1.200 VND).
- Dung lượng `PDF`: 2MB.

| Dịch vụ                  | Mô hình Tính giá | Lưu lượng Thấp (MVP/Test) | Lưu lượng Trung bình    | Lưu lượng Cao            |
| :----------------------- | :--------------- | :------------------------ | :---------------------- | :----------------------- |
| **Quy mô**               |                  | **< 500 Users/tháng**     | **~ 5.000 Users/tháng** | **~ 50.000 Users/tháng** |
| Số lượng Resume tạo ra   |                  | 200 CV                    | 2.000 CV                | 20.000 CV                |
| Amazon S3                | Lưu trữ          | $0.10                     | $1.00                   | $15.00                   |
| CloudFront               | CDN              | $0.50                     | $5.00                   | $50.00                   |
| API GW + Lambda          | Compute          | Free Tier                 | $5.00                   | $40.00                   |
| DynamoDB                 | Database         | Free Tier                 | $2.00                   | $20.00                   |
| Cognito                  | Auth             | Free Tier                 | Free Tier               | Free Tier (< 50k MAU)    |
| Amazon Bedrock (AI)      | Tokens           | $10.00                    | $100.00                 | $1.000.00                |
| WAF + Route53            | Security         | $7.00                     | $7.00                   | $15.00                   |
| **Tổng Chi phí / Tháng** |                  | **~ $18**                 | **~ $120**              | **~ $1.140**             |

> Lưu ý: Chi phí AI (`Bedrock`) chiếm phần lớn khi quy mô tăng. Có thể tối ưu bằng cách sử dụng model nhỏ hơn (`Claude 3 Haiku`) cho các tác vụ đơn giản, giúp giảm chi phí AI xuống khoảng 1/5 (ví dụ: `Haiku` rẻ hơn `Sonnet` rất nhiều).

### 6. Đánh giá Rủi ro

#### Rủi ro Bảo mật & Riêng tư (Cao)

- **Vấn đề**: CV chứa thông tin cá nhân nhạy cảm (`PII`) - SĐT, Email, Địa chỉ.
- **Giảm thiểu**:
  - Mã hóa dữ liệu tại chỗ (`Encryption at rest`) trong `DynamoDB` và `S3`.
  - Quyền truy cập nghiêm ngặt (`IAM Roles`).
  - Không sử dụng dữ liệu người dùng để train AI (Mặc định `AWS Bedrock` không lưu dữ liệu train).

#### Rủi ro Chi phí (Trung bình)

- **Vấn đề**: Tấn công `DDoS` vào `API` hoặc lạm dụng tính năng AI gây tốn kém.
- **Giảm thiểu**:
  - Thiết lập Throttling (giới hạn tốc độ) trên `API Gateway`.
  - Giới hạn số lần gọi AI mỗi ngày cho mỗi tài khoản miễn phí.
  - Đặt `AWS Budget Alerts` để cảnh báo khi chi phí vượt ngưỡng.

#### Rủi ro Kiến trúc

- **Vấn đề**: Cold start của `Lambda` làm chậm trải nghiệm người dùng lần đầu.
- **Giảm thiểu**: Sử dụng `Lambda SnapStart` (cho `Java`) hoặc giữ ấm (`Provisioned Concurrency`) nếu cần thiết (tuy nhiên `Node.js` cold start thường khá nhanh).

### 7. Kết quả Dự kiến

- Sở hữu một nền tảng `SaaS` hoàn chỉnh cho thị trường tuyển dụng.
- Hệ thống có khả năng phục vụ hàng chục nghìn người dùng mà không cần quản lý máy chủ.
- Tích hợp AI tạo lợi thế cạnh tranh rõ rệt so với các công cụ tạo CV truyền thống.
