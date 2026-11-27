---
title: "Kiểm tra quyền gọi model và quota Bedrock"
menuTitle: "Amazon Bedrock"
weight: 4
chapter: false
pre: "<b> 5.6.4. </b> "
---

1. **Mở Amazon Bedrock tại Singapore → Quotas.** Lọc theo model và inference type. Policy mẫu ở 5.4.2 dùng APAC Nova Pro; hộp thoại quota sau là của Nova 2 Lite global cross-region.

![Danh sách quota theo model và inference type.](/images/5-Workshop/5.6-Backend-AI/5.6.4-amazon-bedrock/aws-bedrock-step-1.webp)

2. **Chọn model → Request RPM quota increase.** Đọc quota áp dụng cho số request/phút. Ảnh báo đã có yêu cầu đang chờ. Kiểm tra yêu cầu đó trong Service Quotas.

![Hộp thoại RPM báo đã có yêu cầu đang chờ.](/images/5-Workshop/5.6-Backend-AI/5.6.4-amazon-bedrock/aws-bedrock-step-2-request-rpm.webp)

3. **Mở Request TPM quota increase.** Đối chiếu tokens/phút cho đúng model. Nếu cần tăng, nhập tổng quota mong muốn theo workload và theo dõi trạng thái riêng của yêu cầu.

![Hộp thoại TPM của Nova 2 Lite.](/images/5-Workshop/5.6-Backend-AI/5.6.4-amazon-bedrock/aws-bedrock-step-2-request-tpm.webp)

**Kiểm tra kết quả.** Dùng tài khoản lab gửi một câu hỏi qua BTB. Ghép timestamp/request ID với log ECS. AccessDenied cần kiểm tra task role và Region đích của profile; ThrottlingException cần kiểm tra đúng RPM/TPM. Bộ ảnh chưa xác nhận endpoint production đã trả kết quả.
