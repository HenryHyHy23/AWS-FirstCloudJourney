---
title: "Các bài blogs đã dịch"
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

### [Blog 1 - Tích hợp mã hóa token (tokenization) với Amazon Bedrock Guardrails để xử lý dữ liệu an toàn](3.1-Blog1/)

Bài blog trình bày cách tích hợp Amazon Bedrock Guardrails với các dịch vụ mã hóa token của bên thứ ba để bảo vệ thông tin nhận dạng cá nhân trong các ứng dụng AI tạo sinh. Trong khi Guardrails mặc định chỉ có thể che giấu PII bằng các mặt nạ chung chung, phương pháp tích hợp này thay thế các mặt nạ đó bằng các token giữ nguyên định dạng và có khả năng đảo ngược về giá trị gốc khi cần thiết. Kiến trúc này, được điều phối bằng AWS Step Functions và AWS Lambda, cho phép các hệ thống hạ nguồn hoạt động với dữ liệu có cấu trúc hợp lệ nhưng được bảo vệ bằng mật mã, từ đó duy trì cả tính bảo mật và khả năng chức năng của dữ liệu trong suốt quy trình xử lý.

**https://aws.amazon.com/blogs/machine-learning/integrate-tokenization-with-amazon-bedrock-guardrails-for-secure-data-handling/**

### [Blog 2 - Chạy các agent AI nghiên cứu chuyên sâu trên Amazon Bedrock AgentCore](3.2-Blog2/)

Bài blog giới thiệu về Amazon Bedrock AgentCore Runtime là một nền tảng máy chủ phi tập trung và an toàn, được thiết kế để triển khai các Agent AI nâng cao vào môi trường sản xuất ở quy mô lớn. AgentCore giải quyết thách thức quản lý cơ sở hạ tầng bằng cách cung cấp môi trường được cách ly hoàn toàn cho mỗi phiên, đồng thời hỗ trợ linh hoạt mọi framework agent và mô hình LLM. Với AgentCore Starter Toolkit, các nhà phát triển có thể triển khai các hệ thống phức tạp chỉ với những thay đổi mã tối thiểu, cho phép họ tập trung vào logic agent thay vì lo lắng về bảo mật cấp doanh nghiệp, khả năng mở rộng , và các tính năng như bộ nhớ ổn định.

**https://aws.amazon.com/blogs/machine-learning/running-deep-research-ai-agents-on-amazon-bedrock-agentcore/**

### [Blog 3 - Giới thiệu AWS Card Clash mobile: Học kiến trúc AWS thông qua lối chơi chiến lược](3.3-Blog3/)

Bài blog giới thiệu về việc ra mắt AWS Card Clash trên thiết bị di động, một trò chơi thẻ bài ảo 3D được thiết kế để biến việc học kiến trúc Đám mây AWS và thiết kế giải pháp thành một trải nghiệm hấp dẫn và dễ tiếp cận. Trò chơi có lối chơi turn-based chiến lược, yêu cầu người chơi đặt thẻ dịch vụ AWS vào các ô kiến trúc thích hợp để ghi điểm, từ đó trực quan hóa và nhận biết các khuôn mẫu triển khai AWS phổ biến. AWS Card Clash cung cấp cả chế độ chơi đơn và đấu đối kháng, với các lộ trình học tập chuyên biệt như Cloud Practitioner, Solutions Architect, Serverless Developer và AI Tạo sinh, giúp người chơi xây dựng kiến thức thực tế và chuẩn bị cho các kỳ thi chứng nhận AWS.

**https://aws.amazon.com/blogs/training-and-certification/introducing-aws-card-clash-mobile-learn-aws-architecture-through-strategic-gameplay/**
