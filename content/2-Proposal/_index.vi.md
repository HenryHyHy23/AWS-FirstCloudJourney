---
title: "Đề xuất Dự án"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Bite to Business

### 1. Tóm tắt Dự án

**Bite to Business** là dự án xây dựng website bán thực phẩm, ưu tiên khách hàng doanh nghiệp như nhà hàng, khách sạn và đơn vị cung cấp suất ăn, đồng thời hỗ trợ cá nhân/hộ gia đình. Hệ thống hỗ trợ quy trình từ xem sản phẩm, lập báo giá (Quote), backend kiểm tra giá – ưu đãi – VAT – phí giao – tồn kho, khách hàng xác nhận báo giá cuối, thanh toán QR qua SePay đến chuẩn bị và giao hàng.

Điểm nổi bật là **Generative UI**, giúp điều chỉnh giao diện theo ý định, sở thích và ngữ cảnh mua hàng của từng người dùng. Backend gọi **Amazon Bedrock** với model **Amazon Nova Pro** để gợi ý sản phẩm và sinh cấu hình UI dạng JSON từ thư viện component do team xây dựng; **Amazon Bedrock Guardrails** kiểm tra nội dung người dùng nhập vào AI. React hiển thị cấu hình đã kiểm tra; backend quản lý quyền, giá, tồn kho và giao dịch.

Ứng dụng dùng **React/TypeScript**, **Spring Boot (Java 21)** và **Supabase PostgreSQL**, và đã được triển khai trên **AWS** tại region Singapore (`ap-southeast-1`). Dự án hướng tới đơn giản hóa thao tác mua hàng, hỗ trợ phối hợp giữa bán hàng, kho và giao nhận, đồng thời đánh giá hiệu quả của giao diện cá nhân hóa.

### 2. Bài toán Cần giải quyết

#### Vấn đề là gì?

Trong mua thực phẩm B2B, khách hàng có thể cần tìm nguyên liệu mới, so sánh giá theo số lượng, mua lại hàng quen thuộc hoặc chia hàng cho nhiều địa chỉ. Mỗi tác vụ cần cách ưu tiên thông tin và thao tác khác nhau.

- **Giao diện cố định chưa phù hợp với từng nhu cầu:** Một bố cục chung có thể khiến người mua phải tìm kiếm, lọc và chuyển trang nhiều lần, kể cả khi đã diễn đạt rõ mục tiêu.
- **Dễ mất ngữ cảnh khi giao diện thay đổi:** Sản phẩm, số lượng, đơn vị bán, Quote và nhóm giao hàng đang chọn cần được giữ nhất quán khi khách hỏi tiếp hoặc đổi cách hiển thị.
- **Giao diện sinh ra cần đúng dữ liệu và quy trình:** Thay đổi số lượng có thể làm đổi bậc giá, tổng tiền hoặc khả năng đáp ứng. UI cần hiển thị dữ liệu backend hiện hành và chỉ cung cấp thao tác phù hợp với quyền và trạng thái đơn.
- **Phản hồi chậm hoặc lỗi gây gián đoạn:** Kết quả từ yêu cầu cũ có thể ghi đè thao tác mới; quá trình tạo UI bị ngắt có thể làm mất tiến trình hoặc để lại thông tin lỗi thời.

Bài toán của **Bite to Business** là tạo giao diện thích ứng theo ý định và ngữ cảnh của từng người dùng, đồng thời duy trì tác vụ mua hàng và bảo đảm giao dịch đúng quy trình.

#### Giải pháp

**Bite to Business** tích hợp **Generative UI** qua **Amazon Bedrock (Amazon Nova Pro)**. Giải pháp gồm:

- **Cá nhân hóa theo tác vụ:** Backend cung cấp ngữ cảnh và sở thích liên quan; AI đề xuất bố cục từ catalog component của team. Ví dụ, ưu tiên bảng khi mua số lượng lớn hoặc danh sách mua lại khi khách quay lại.
- **Runtime A2UI có kiểm tra:** Tiếp nhận thông điệp JSON theo phiên bản giao thức đã chọn, kiểm tra schema, component, data binding và action được phép, rồi render bằng thư viện React của website.
- **Giữ ngữ cảnh tương tác:** Gửi định danh sản phẩm, số lượng, Quote và phiên bản trạng thái (revision) cùng mỗi thao tác về backend. Áp dụng kết quả còn phù hợp với tác vụ hiện tại và giữ lựa chọn hợp lệ khi đổi bố cục.
- **Backend kiểm soát giao dịch:** Giá, ưu đãi, VAT, phí giao, tồn kho và quyền được kiểm tra tại backend. Khi khách xác nhận báo giá cuối, hệ thống tạo Order, giữ hàng tối đa sáu giờ và cho thanh toán QR qua SePay; trạng thái thanh toán chỉ cập nhật khi webhook SePay hợp lệ.
- **Fallback khi AI lỗi:** Giữ phần giao diện hợp lệ hoặc chuyển về giao diện cơ bản, bảo toàn bản nháp khi có thể và thông báo trạng thái để người dùng tiếp tục thao tác.

Hiệu quả sẽ được so sánh với giao diện cố định qua tỷ lệ và thời gian hoàn thành tác vụ, số thao tác, độ đúng của render/data binding và khả năng phục hồi khi lỗi.

### 3. Kiến trúc Giải pháp

Hệ thống chạy trên AWS tại region Singapore (`ap-southeast-1`); các dịch vụ edge (CloudFront, chứng chỉ ACM và Web ACL cho CloudFront) đặt ở `us-east-1` theo yêu cầu của CloudFront. Trạng thái bên dưới cập nhật đến tháng 11/2026.

| Thành phần | Dịch vụ / cấu hình | Trạng thái |
| --- | --- | --- |
| **Tên miền và HTTPS** | Route 53 quản lý hosted zone; AWS Certificate Manager cấp chứng chỉ HTTPS. | Đã triển khai |
| **Một cửa vào** | Mọi lưu lượng người dùng đi qua CloudFront: `/*` → bản build React/Vite trên S3 private (Origin Access Control; CloudFront Function chuyển các đường dẫn SPA về `index.html`); `/api/*` → Internet Gateway → ALB, không cache, chuyển tiếp toàn bộ header và cookie, chờ origin tối đa 60 giây cho AI stream. Frontend và API dùng chung một domain nên không cần CORS. | Đã triển khai |
| **Bảo vệ** | AWS WAF với 2 Web ACL. **CloudFront:** 5 rule giới hạn tốc độ theo IP cho `/api/*` (API 2.000, AI 30, đăng nhập 30, xác minh/gửi lại OTP 10 request mỗi 5 phút, trả mã 429) cùng 2 bộ rule IP reputation và known bad inputs ở chế độ Block; bộ common rule set và Anti-DDoS để Count trong lúc theo dõi log. Không chặn theo quốc gia hay VPN, để phục vụ cả khách quốc tế và người dùng VPN. **ALB:** chỉ nhận request mang header bí mật do CloudFront gắn, cùng đường webhook SePay (có giới hạn tốc độ), nên không gọi thẳng vào ALB được. | Đã triển khai |
| **Cân bằng tải** | Application Load Balancer (chỉ HTTPS, health check `/api/health`) đứng sau CloudFront; access log lưu vào S3 (tự xóa sau 30 ngày) và truy vấn bằng Amazon Athena. | Đã triển khai |
| **Backend** | Spring Boot chạy trên ECS Fargate, mỗi task 0,5 vCPU / 2 GB; Application Auto Scaling giữ 1–3 task theo mục tiêu CPU 60%. | Đã triển khai |
| **Mạng** | VPC 2 AZ với Internet Gateway; task chạy ở public subnet có public IP, security group chỉ cho phép lưu lượng vào từ ALB; không dùng NAT Gateway. S3 Gateway Endpoint và VPC Flow Logs. | Đã triển khai |
| **Dữ liệu và xác thực** | Supabase PostgreSQL (schema quản lý bằng Flyway); Supabase Auth phát JWT, Spring Security kiểm tra JWT; refresh token lưu trong cookie HttpOnly. | Đã triển khai |
| **AI** | Backend gọi Amazon Bedrock (Converse API) với Amazon Nova Pro qua cross-region inference profile APAC; Bedrock Guardrails kiểm tra nội dung; giới hạn số lần gọi AI theo người dùng. Endpoint `/api/ai/agent` trả kết quả dạng stream (SSE). | Đã triển khai |
| **Event bus và thông báo** | Backend ghi sự kiện nghiệp vụ (`order.*`, `payment.*`) vào bảng outbox trong cùng transaction; relay đẩy lên Amazon EventBridge bus `btb-events`. Rule chuyển sự kiện của khách hàng vào Amazon SQS (`btb-customer-events`); Lambda đọc hàng đợi, ghi thông báo web và gửi email qua Amazon SES API. Gửi lỗi thì thử lại rồi chuyển sang DLQ; Lambda chống xử lý trùng theo event ID. | Đã triển khai |
| **Thêm nơi nhận thông báo** | Thêm rule trên cùng bus: thông báo cho nhân viên (cảnh báo hết hàng cho logistics, yêu cầu sản phẩm mới cho e-commerce và admin) và email marketing (flash sale, Black Friday cho khách đã đăng ký nhận tin, hẹn giờ bằng EventBridge Scheduler); mỗi loại có hàng đợi SQS và Lambda riêng. | Dự kiến |
| **Email** | Amazon SES với domain đã xác minh: Lambda gửi qua SES API, backend gửi qua SES SMTP. | Đã triển khai |
| **Secrets và IAM** | Secrets Manager giữ mật khẩu DB và khóa tích hợp, inject vào container; IAM tách riêng task role, Lambda role, OIDC provider cho GitHub và user SMTP. | Đã triển khai |
| **Giám sát và cảnh báo** | CloudWatch Logs và Alarms; alarm gửi qua Amazon SNS tới email của team; CloudWatch RUM đo hiệu năng frontend (Cognito Identity Pool chỉ dùng để RUM gửi dữ liệu, không dùng để đăng nhập); AWS Budgets theo dõi chi phí hằng tháng. | Đã triển khai |
| **CI/CD** | GitHub Actions dùng OIDC để nhận quyền AWS tạm thời: BE build image → ECR → cập nhật ECS service; FE build → đồng bộ lên S3 → invalidate CloudFront. | Đã triển khai |
| **Generative UI** | Thư viện `@a2ui/react` và `@json-render` đã được thêm vào FE; catalog component và renderer đang xây dựng. | Đang phát triển |
| **Tích hợp ngoài AWS** | Supabase (PostgreSQL + Auth), Cloudinary (ảnh), SePay (QR chuyển khoản + webhook), Vietmap (bản đồ, tuyến giao), GitHub Actions. | Đã tích hợp |

**Luồng request:** Người dùng → CloudFront (+ WAF) → `/*` S3 (web) hoặc `/api/*` → Internet Gateway → ALB (+ WAF) → ECS. Đường duy nhất đi thẳng vào ALB là webhook thanh toán SePay.

**Luồng Generate UI:** Yêu cầu và ngữ cảnh → backend xác thực, lấy dữ liệu trong phạm vi quyền → Guardrails kiểm tra nội dung → Bedrock (Nova Pro) đề xuất UI → backend kiểm tra và chuyển thành A2UI → stream về FE → runtime kiểm tra và render bằng React.

Các action quay về backend để kiểm tra quyền, giá, tồn kho và giao dịch. Runtime giữ ngữ cảnh, loại phản hồi cũ và fallback về giao diện cơ bản khi AI lỗi.

### 4. Triển khai Kỹ thuật

#### Stack Công nghệ

| Nhóm | Công nghệ |
| --- | --- |
| **Frontend** | React 19, TypeScript, Vite, Tailwind CSS, React Router |
| **State và dữ liệu API** | TanStack Query, Zustand, Axios |
| **Form và validation** | React Hook Form, Zod |
| **Backend** | Java 21, Spring Boot 4.1, Spring Security (OAuth2 Resource Server), Spring Data JPA/Hibernate |
| **Database / Migration** | Supabase PostgreSQL, Flyway |
| **Authentication** | Supabase Auth, JWT, refresh token trong cookie HttpOnly |
| **Generative UI** | A2UI, json-render, JSON Schema, React component catalog và runtime (đang phát triển) |
| **AI** | Amazon Bedrock (Converse API), Amazon Nova Pro, Bedrock Guardrails |
| **Edge và bảo mật** | Route 53, CloudFront (+ CloudFront Functions), AWS Certificate Manager, AWS WAF, IAM, Secrets Manager |
| **Compute và mạng** | ECS Fargate, Application Auto Scaling, ALB, ECR, VPC (Internet Gateway, S3 Gateway Endpoint) |
| **Hướng sự kiện và email** | Amazon EventBridge, Amazon SQS (kèm DLQ), AWS Lambda, Amazon SES |
| **Giám sát và chi phí** | CloudWatch Logs, Alarms, RUM; Amazon SNS; VPC Flow Logs; Amazon Athena; AWS Budgets |
| **CI/CD** | Docker, GitHub Actions + OIDC |
| **Tích hợp ngoài** | Supabase, SePay, Cloudinary, Vietmap |
| **Đa ngôn ngữ** | i18next, react-i18next |

#### Các Giai đoạn

1. **Yêu cầu và database:** Viết requirement, xác định luồng nghiệp vụ; thiết kế schema và xây dựng database trên Supabase PostgreSQL. *(Hoàn thành)*
2. **Thiết kế UI và repository:** Thiết kế giao diện trên Figma; tạo GitHub repository và cấu trúc dự án frontend/backend. *(Hoàn thành)*
3. **Phát triển và xác thực:** Code giao diện/API, xây dựng authentication với Supabase Auth và phân quyền cho các vai trò. *(Hoàn thành phần chính)*
4. **Thiết lập AWS và triển khai:** Dựng VPC, ALB, ECS Fargate có auto scaling, S3/CloudFront, WAF, Route 53, SES, EventBridge/SQS/Lambda và pipeline GitHub Actions + OIDC; chuyển API ra sau CloudFront thành một cửa vào. *(Hoàn thành)*
5. **Tích hợp dịch vụ:** SePay, Vietmap, Cloudinary, Bedrock/Nova Pro + Guardrails; hoàn thiện A2UI runtime. *(Đang thực hiện phần Generative UI)*
6. **Tối ưu UX/UI, hiệu năng và chi phí:** Theo dõi RUM, ALB log qua Athena, alarm và Budgets; điều chỉnh task size, auto scaling và rule WAF. *(Đang thực hiện)*

### 5. Ước tính Ngân sách (Chi phí AWS)

Giá lấy từ AWS Price List cho region Singapore (`ap-southeast-1`), bản tháng 11/2026; một tháng tính 730 giờ. Bảng dưới phản ánh cấu hình đang chạy, chưa gồm thuế và các dịch vụ ngoài AWS (Supabase, Cloudinary, Vietmap, SePay).

| Dịch vụ | Cách tính | 1 task (thường xuyên) | 3 task (tối đa) |
| --- | --- | --- | --- |
| **ECS Fargate** (0,5 vCPU / 2 GB mỗi task) | 0,05056 USD/vCPU-giờ + 0,00553 USD/GB-giờ ≈ 26,5 USD/task | ~ 26,5 USD | ~ 79,6 USD |
| **Application Load Balancer** | 0,0252 USD/giờ + 0,008 USD/LCU-giờ (giả định 1 LCU) | ~ 24,2 USD | ~ 24,2 USD |
| **IPv4 công khai** | 0,005 USD/giờ mỗi địa chỉ (2 của ALB + 1 mỗi task) | ~ 11,0 USD | ~ 18,3 USD |
| **AWS WAF** | 5 USD/Web ACL × 2 + 1 USD mỗi rule hoặc bộ rule (9 ở CloudFront + 2 ở ALB) + 0,60 USD/triệu request | ~ 21 USD | ~ 21 USD |
| **Secrets Manager** | 0,40 USD/secret (1 secret dạng JSON) | ~ 0,4 USD | ~ 0,4 USD |
| **S3, CloudFront, Route 53, ECR, CloudWatch, Athena, SNS, EventBridge** | Theo dung lượng, lưu lượng, số request, lượng log, dữ liệu quét và số sự kiện; nhỏ ở quy mô MVP | vài USD | vài USD |
| **Tổng hạ tầng (chưa gồm AI)** | | **~ 85 USD** | **~ 145 USD** |

Việc bỏ NAT Gateway giúp tiết kiệm khoảng 43 USD/tháng tiền giờ (0,059 USD/giờ × 730 giờ), chưa kể phí 0,059 USD cho mỗi GB dữ liệu đi qua NAT.

**Chi phí AI (Amazon Nova Pro, Singapore):** 0,00108 USD/1.000 token input và 0,00432 USD/1.000 token output. Giả định một lần gọi dùng 3.000 token input và 800 token output: 3 × 0,00108 + 0,8 × 0,00432 ≈ 0,0067 USD. Phí Guardrails tính riêng theo lượng văn bản được kiểm tra.

| Quy mô | Số lần gọi AI / tháng | Chi phí model / tháng |
| --- | --- | --- |
| MVP / demo | 1.000 | ~ 6,7 USD |
| Trung bình | 10.000 | ~ 67 USD |
| Cao | 100.000 | ~ 670 USD |

Luồng thông báo SQS → Lambda → SES ở quy mô MVP chủ yếu nằm trong mức miễn phí của SQS và Lambda; SES tính 0,10 USD/1.000 email theo giá trả theo dùng (tài khoản mới từ 21/07/2026 mặc định gói Essentials 0,16 USD/1.000 email).

**Hướng tối ưu:**

- Đo CPU/RAM thực tế để chỉnh task size và ngưỡng auto scaling.
- Log ALB tự xóa sau 30 ngày, CloudWatch Logs giữ 14 ngày (đã áp dụng).
- Dùng AWS Budgets (đã có) và giới hạn số lần gọi AI theo người dùng (đã có trong backend).

### 6. Đánh giá Rủi ro

#### Rủi ro Bảo mật & Riêng tư (Cao)

- **Vấn đề:** Dữ liệu khách hàng doanh nghiệp (thông tin liên hệ, địa chỉ giao, đơn hàng) và giao dịch thanh toán; nguy cơ webhook giả mạo hoặc prompt injection vào AI.
- **Giảm thiểu:**
  - Mọi lưu lượng đi qua CloudFront; ALB chỉ nhận request có header bí mật của CloudFront (cùng webhook SePay), còn task ECS chỉ nhận lưu lượng từ security group của ALB.
  - Giới hạn tốc độ theo IP cho đăng nhập, OTP, AI và API; rule IP reputation và known bad inputs chặn các nguồn tấn công đã biết. Bộ common rule set để Count cho tới khi log xác nhận không chặn nhầm request API thật.
  - Secrets nằm trong Secrets Manager, không nằm trong image hay mã nguồn; CI/CD dùng OIDC thay cho access key dài hạn.
  - Kiểm tra khóa xác thực webhook SePay và đối chiếu số tiền trước khi cập nhật đơn.
  - AI chỉ nhận dữ liệu trong phạm vi quyền người dùng; giá, tồn kho, trạng thái đơn luôn lấy từ backend; Bedrock Guardrails lọc nội dung đầu vào.

#### Rủi ro Chi phí (Trung bình)

- **Vấn đề:** Chi phí cố định của ALB, WAF và IPv4 công khai; auto scaling tăng số task khi bị tấn công; lạm dụng tính năng AI.
- **Giảm thiểu:** Giới hạn tối đa 3 task, WAF chặn request xấu, giới hạn số lần gọi AI, AWS Budgets và alarm gửi qua SNS.

#### Rủi ro Kiến trúc và Vận hành

- **Task ở public subnet:** Không cần NAT nhưng mỗi task có public IP; mọi cổng phải được khóa bằng security group, và không mở thêm port nào ngoài port ứng dụng cho ALB.
- **AI stream qua CloudFront và ALB:** CloudFront chờ origin tối đa 60 giây, ALB đóng kết nối im lặng sau 60 giây; đã tắt nén cho `/api/*`, và luồng SSE nên gửi heartbeat định kỳ để không bị ngắt khi model xử lý lâu.
- **Đổi header bí mật:** Giá trị header CloudFront → ALB phải khớp ở cả hai nơi; khi đổi cần cập nhật origin CloudFront và rule WAF của ALB cùng lúc.
- **Phụ thuộc dịch vụ ngoài AWS:** Supabase, SePay, Vietmap, Cloudinary nằm ngoài AWS; cần timeout, retry và thông báo lỗi rõ ràng.
- **Secret thay đổi:** Secret được inject khi container khởi động; đổi secret phải triển khai lại ECS service.
- **Email trùng:** EventBridge và SQS giao tin ít nhất một lần; Lambda ghi nhận sự kiện đã xử lý để không gửi trùng email.

### 7. Kết quả Dự kiến

- Website bán thực phẩm B2B chạy trên AWS, hỗ trợ đầy đủ luồng báo giá → xác nhận → thanh toán QR → giao hàng, kèm thông báo web và email theo hướng sự kiện, có thể mở rộng sang thông báo cho nhân viên và email marketing.
- Giao diện Generative UI thích ứng theo ý định người mua, có kiểm tra và fallback an toàn về giao diện cơ bản.
- Số liệu so sánh giao diện cố định và giao diện cá nhân hóa: thời gian hoàn thành tác vụ, số thao tác, tỷ lệ lỗi render.
- Hạ tầng có auto scaling, CI/CD tự động, quản lý secrets tập trung, giám sát và cảnh báo chi phí, cùng ước tính chi phí rõ ràng để tối ưu sau giai đoạn MVP.
