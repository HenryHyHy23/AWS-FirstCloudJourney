---
title: "Đề xuất Dự án"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Nền tảng SnapResume

### 1. Tóm tắt Dự án

**SnapResume** là một nền tảng xây dựng hồ sơ năng lực (CV/Resume) thông minh, được hỗ trợ bởi AI, giúp người tìm việc tạo ra các hồ sơ chuyên nghiệp, chuẩn `ATS` (Applicant Tracking System) một cách nhanh chóng và hiệu quả. Nền tảng cung cấp các mẫu thiết kế hiện đại, công cụ chỉnh sửa trực quan và trợ lý AI để tối ưu hóa nội dung, sửa lỗi ngữ pháp và gợi ý từ khóa phù hợp với mô tả công việc.

Điểm khác biệt chính là việc tích hợp Sáng tạo nội dung bằng AI (`GenAI`) sử dụng `AWS Bedrock` (`Claude 3 Sonnet`), cho phép người dùng tạo ra các bản tóm tắt chuyên nghiệp, mô tả kinh nghiệm làm việc ấn tượng chỉ từ những gạch đầu dòng cơ bản. Hệ thống được xây dựng hoàn toàn trên kiến trúc `Serverless`, đảm bảo khả năng mở rộng vô hạn và chi phí tối ưu.

### 2. Bài toán Cần giải quyết

#### Vấn đề là gì?

- **Khó khăn trong Trình bày**: Người tìm việc thường mất hàng giờ để định dạng văn bản trong `Word` hoặc các công cụ thiết kế phức tạp mà vẫn không đạt được vẻ ngoài chuyên nghiệp.
- **Nội dung kém thu hút**: Ứng viên gặp khó khăn trong việc viết nội dung cô đọng, sử dụng từ ngữ chuyên ngành và làm nổi bật thành tích của mình. "Writer's block" (bí ý tưởng) là vấn đề phổ biến.
- **Không vượt qua được ATS**: Các CV thiết kế thủ công thường chứa các yếu tố đồ họa hoặc cấu trúc bảng biểu khiến hệ thống quét hồ sơ tự động (`ATS`) của nhà tuyển dụng không thể đọc được nội dung.

#### Giải pháp

SnapResume giải quyết các vấn đề này bằng một ứng dụng web `React` hiện đại trên nền tảng `AWS Serverless`.

- **Trình chỉnh sửa trực quan**: Giao diện kéo thả hoặc điền biểu mẫu đơn giản, xem trước thời gian thực (`Real-time Preview`).
- **AI Copilot**: Sử dụng `AWS Bedrock` để tự động viết lại câu, đề xuất nội dung dựa trên vị trí ứng tuyển.
- **Chuẩn ATS**: Các mẫu (`Templates`) được thiết kế để thân thiện với máy đọc nhưng vẫn đẹp mắt với người đọc.
- **Quản lý tập trung**: Lưu trữ nhiều phiên bản CV, xuất ra `PDF` chất lượng cao.

### 3. Kiến trúc Giải pháp

Hệ thống sử dụng kiến trúc `Serverless` và `Event-driven` trên `AWS`.
![AWS Architecture](/images/2-Proposal/SnapResume.png)

#### Tổng quan Kiến trúc

- **Frontend**: `React 19` + `Vite` (hosting trên `S3` + `CloudFront` hoặc `AWS Amplify`).
- **API Layer**: `Amazon API Gateway` + `AWS Lambda` (`Node.js`/`Express`).
- **Database**: `Amazon DynamoDB` (Single Table Design) lưu trữ `User`, `Resume`, `Section`, `Template`.
- **Authentication**: `Amazon Cognito` (User Pools & Identity Pools).
- **AI Engine**: `Amazon Bedrock` (`Claude 3 Sonnet`) cho các tác vụ sinh nội dung.
- **Storage**: `Amazon S3` để lưu trữ ảnh đại diện, tệp `PDF` đã xuất.

### 4. Triển khai Kỹ thuật

#### Stack Công nghệ

- **Frontend**: `ReactJS`, `TailwindCSS`, `Ant Design`, `html2pdf.js` cho việc xuất `PDF`.
- **Backend**: `NodeJS`, `TypeScript`, `Express` (chạy trong `Lambda`).
- **IaC**: `Terraform` để quản lý toàn bộ hạ tầng.

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
