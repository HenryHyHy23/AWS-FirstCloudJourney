---
title: "Event 3"
date: 2026-10-24
weight: 1
chapter: false
pre: " <b> 4.3. </b> "
---

# Bài thu hoạch: Buổi MEET UP

### Thông tin sự kiện

| Mục          | Thông tin                                         |
| ------------ | ------------------------------------------------- |
| Ngày tổ chức | 24/10/2026                                        |
| Thời gian    | 09:00–12:00 (UTC+7)                               |
| Địa điểm     | Tầng 26, Bitexco Financial Tower, TP. Hồ Chí Minh |
| Vai trò      | Người tham dự                                     |

### Mục tiêu

Tìm hiểu AI agent, chia sẻ tài nguyên và kiểm tra bảo mật trên AWS.

### Diễn giả tiêu biểu

- **Kiệt Trần** – AI Engineer, Revve AI.
- **Long Huỳnh** – DevSecOps, Styl Solution.
- **Thịnh Nguyễn** – Styl Solution.
- **Xuân An** – Leader AWS Student Builder Group HCMUTE.
- **Khoa Kiều** – Tech Lead ACB.
- **Nghi Danh** – AWS Community Builder.

### Nội dung chính

#### Session 1: Hear, Think, Speak – Voice AI tiếng Việt

- Demo AI gọi phỏng vấn Cloud Engineer bằng tiếng Việt khoảng 3 phút, sau đó có transcript, tóm tắt và điểm đánh giá.
- Quy trình: chào hỏi → kinh nghiệm → hai câu hỏi kỹ thuật → câu hỏi của ứng viên → kết thúc.
- Luồng xử lý STT → LLM → TTS cần xử lý tốt tiếng Việt, ngắt lời, độ trễ và kiểm tra nội dung trước khi phát giọng nói.

#### Session 2: AWS Resource Access Manager

- Chia sẻ subnet, Transit Gateway và Resolver rules giữa các account để dùng chung hạ tầng.
- Owner quản lý tài nguyên; RAM permissions, IAM và SCP kiểm soát quyền sử dụng.
- Hữu ích với doanh nghiệp có nhiều team/account và cần quản lý mạng tập trung.

#### Session 3: AWS Continuum / Security Agent

- Review thiết kế, Terraform/code và PR/MR trên GitHub/GitLab; comment tại dòng lỗi và đề xuất bản vá.
- Pentest SIT/UAT bằng tài khoản thử, kiểm tra chuỗi lỗ hổng và trả về attack graph, bằng chứng. Sửa xong cần kiểm thử lại.
- Cần chú ý luồng đăng nhập, ngữ cảnh nghiệp vụ và task-hours khi chạy kiểm tra.

#### Session 4: Amazon Bedrock AgentCore

- Runtime hỗ trợ chạy agent trong MicroVM cách ly theo session; quản lý version và endpoint DEV/PROD.
- Memory lưu lịch sử tương tác (STM) và trích xuất thông tin dài hạn (LTM) ở nền để dùng qua nhiều phiên.
- Các strategy gồm Summary (tóm tắt), User Preferences (sở thích), Semantic (kiến thức) và Episodic (cách xử lý nhiệm vụ).

#### Session 5: Kiro Crew

- Agent hỗ trợ review code, kiểm tra hệ thống 24/7. Demo thu thập dữ liệu việc làm và tạo biểu đồ dạng Artifacts ngay trong chat.
- Auto Compact rút gọn context; Break down context giúp xem lượng token từng phần. Memory cũ cần được dọn để kiểm soát mức dùng.
- Giới hạn quyền để agent không tự ý xóa hạ tầng hoặc làm lộ secret/credential.

### Bài học rút ra

- Quản lý context và memory để agent giữ đúng thông tin, tránh tốn token.
- Giới hạn quyền của agent, kiểm tra đầu ra và kiểm thử lại sau khi sửa lỗi.

### Cảm nhận

Mình ấn tượng với demo AI phỏng vấn bằng tiếng Việt. Phần mình thích nhất là Q&A của Kiro Crew vì nói thẳng vào chuyện token, memory và quyền của agent.

---

![Đã Check in tại sự kiện](/images/4-EventParticipated/4.3-Event3/check-in.png)

_Đã Check in tại sự kiện_
