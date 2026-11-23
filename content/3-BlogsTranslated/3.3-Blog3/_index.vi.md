---
title: "Blog 3"
date: 2026-11-19
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# GỬI EMAIL XÁC NHẬN ĐƠN HÀNG MÀ KHÔNG LÀM CHẬM WEBSITE: MÌNH TÌM HIỂU AMAZON SQS, AWS LAMBDA VÀ AMAZON SES

**Nếu dịch vụ gửi email bị chậm vài giây, thì khách có phải đứng chờ nút "Đặt hàng" quay mãi không?**

Khi tìm hiểu cách làm một website bán hàng, mình thấy cách dễ nghĩ ra nhất là: API lưu đơn hàng xong thì gửi email xác nhận luôn, rồi mới trả kết quả cho khách. Cách này chạy được khi thử ở máy mình. Nhưng nếu dịch vụ email chậm, khách phải chờ theo. Nếu dịch vụ email lỗi, API có thể báo "đặt hàng thất bại" dù đơn đã được lưu, hoặc email bị mất luôn.

Từ đó mình tìm hiểu cách tách việc gửi email ra khỏi luồng đặt hàng, bằng **Amazon SQS**, **AWS Lambda** và **Amazon SES**. Bài này là những gì mình hiểu được, mọi người thấy chỗ nào chưa đúng thì góp ý giúp mình nhé.

**Ý tưởng: đặt một hàng đợi ở giữa**

Mình hay hình dung giống quầy order ở quán ăn. Thu ngân ghi phiếu, đặt lên quầy rồi phục vụ khách tiếp theo; bếp lấy phiếu và nấu sau. Thu ngân không đứng chờ bếp nấu xong mới nhận khách mới.

Trong ví dụ này, backend là thu ngân, hàng đợi là quầy để phiếu, còn phần gửi email là bếp.

- **Amazon SQS:** dịch vụ hàng đợi. Backend bỏ một tin nhắn "đơn hàng mới" vào hàng đợi rồi trả kết quả cho khách ngay.
- **AWS Lambda:** chạy code mà không cần quản lý server. Lambda tự lấy tin nhắn từ hàng đợi theo từng lô (batch) và xử lý.
- **Amazon SES:** dịch vụ gửi email. Lambda gọi SES để gửi email xác nhận cho khách.
- **Dead-letter queue (DLQ):** một hàng đợi riêng, chứa những tin nhắn xử lý thất bại quá nhiều lần, để mình xem lại sau thay vì để chúng lặp mãi.

**Luồng hoạt động (xem ảnh)**

![Gửi email xác nhận đơn hàng bằng Amazon SQS, AWS Lambda và Amazon SES](/images/3-BlogsTranslated/3.3-Blog3/architecture.png)

- Khách bấm "Đặt hàng". Backend lưu đơn vào database (ví dụ Amazon RDS) và trả kết quả cho khách ngay.
- Backend gửi tin nhắn "đơn hàng mới" vào hàng đợi SQS.
- Lambda nhận tin nhắn từ SQS. Trước khi gửi, Lambda kiểm tra trong Amazon DynamoDB xem email cho đơn này đã được gửi chưa.
- Nếu chưa gửi, Lambda gọi SES gửi email tới hộp thư của khách, rồi ghi lại là đã gửi.
- Nếu một tin nhắn thất bại nhiều lần, SQS chuyển nó sang DLQ. Một CloudWatch alarm báo cho mình biết khi DLQ có tin nhắn.

Có một chi tiết mình thấy dễ bị bỏ qua: nếu backend lưu đơn xong mà gửi tin nhắn sang SQS bị lỗi, email sẽ mất. Cách mình tìm được là **transactional outbox**: backend ghi đơn hàng và "sự kiện cần gửi" vào cùng một transaction trong database, rồi một tiến trình riêng mới đẩy sự kiện đó sang SQS.

**Những bẫy mình thấy khi đọc tài liệu**

- **Một tin nhắn có thể được giao hơn một lần.** Hàng đợi SQS loại Standard đảm bảo giao tin "ít nhất một lần", nên đôi khi cùng một tin nhắn tới 2 lần. Nếu code không chống trùng, khách có thể nhận 2 email cho một đơn. Vì vậy mình lưu mã đơn hàng đã gửi email vào DynamoDB và kiểm tra trước khi gửi.
- **Visibility timeout quá ngắn.** Khi Lambda đang xử lý, tin nhắn bị ẩn khỏi hàng đợi trong khoảng visibility timeout. Nếu khoảng này ngắn hơn thời gian Lambda chạy, tin nhắn sẽ hiện lại và bị xử lý lần nữa. AWS khuyên đặt visibility timeout ít nhất gấp 6 lần timeout của Lambda.
- **Một tin lỗi kéo cả lô chạy lại.** Mặc định Lambda lấy tối đa 10 tin nhắn mỗi lần. Nếu một tin lỗi mà Lambda báo lỗi cả lô, những tin đã gửi email thành công cũng quay lại hàng đợi. Bật "ReportBatchItemFailures" để Lambda chỉ trả lại đúng những tin bị lỗi.
- **DLQ có rồi nhưng không ai xem.** Tài liệu gợi ý đặt "maxReceiveCount" (số lần nhận tối đa trước khi chuyển sang DLQ) từ 5 trở lên. Nhưng nếu không đặt alarm, tin nhắn có thể nằm im trong DLQ mà không ai biết.
- **SES ban đầu ở chế độ sandbox.** Ở chế độ này, SES chỉ gửi tới địa chỉ email đã xác minh, tối đa 200 email mỗi 24 giờ và 1 email mỗi giây. Muốn gửi cho khách thật, mình phải gửi yêu cầu production access cho từng region.

**Chi phí thì sao?**

Mình thử tính cho ví dụ 30.000 đơn hàng mỗi tháng, theo các trang giá mình xem tháng 11/2026:

- **SQS:** mỗi tháng miễn phí 1 triệu request. Mỗi tin nhắn tốn vài request (gửi, nhận, xoá), tức khoảng 90.000 request. Lambda cũng liên tục hỏi hàng đợi, các lần hỏi này cũng tính là request, nhưng với lưu lượng nhỏ thường vẫn nằm trong phần miễn phí.
- **Lambda:** mỗi tháng miễn phí 1 triệu request và 400.000 GB-giây, nên 30.000 lần gửi email ngắn gần như không tốn gì.
- **SES:** giá kiểu cũ là 0,10 USD cho 1.000 email, tức khoảng 3 USD cho 30.000 email. Theo trang giá SES, từ 21/07/2026 tài khoản mới mặc định dùng gói Essentials với giá 0,16 USD cho 1.000 email, tức khoảng 4,8 USD.

Mình thấy tiền không phải lợi ích chính của kiến trúc này. Lợi ích là API trả lời nhanh hơn, email không bị mất khi dịch vụ email lỗi tạm thời, và việc thử lại được làm tự động.

**Mình sẽ kiểm tra lại như thế nào?**

- Đặt một đơn hàng test và đo xem API có trả kết quả mà không phải chờ email không.
- Gửi cùng một tin nhắn 2 lần vào hàng đợi, khách chỉ được nhận 1 email.
- Cố tình làm Lambda lỗi để xem tin nhắn có sang DLQ sau 5 lần và alarm có báo không.
- Dùng mailbox simulator của SES để thử gửi mà không cần email thật.

Theo mình, hàng đợi là một trong những ý tưởng đơn giản nhưng hữu ích nhất khi mới học AWS: việc gì không cần làm ngay thì cho vào hàng đợi, để phần quan trọng như đặt hàng luôn nhanh và ổn định.

**Tài liệu tham khảo:**

- [Amazon SQS standard queues](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/standard-queues.html)
- [Dùng Lambda với Amazon SQS](https://docs.aws.amazon.com/lambda/latest/dg/with-sqs.html)
- [Dead-letter queue trong SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html)
- [Thoát chế độ sandbox của SES](https://docs.aws.amazon.com/ses/latest/dg/request-production-access.html)
- [Transactional outbox pattern](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/transactional-outbox.html)
