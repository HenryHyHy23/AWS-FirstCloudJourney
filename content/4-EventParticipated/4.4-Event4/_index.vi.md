---
title: "Event 4"
date: 2025-09-09
weight: 1
chapter: false
pre: " <b> 4.4. </b> "
--- 

# Bài thu hoạch: AWS Edge Services Workshop

### Mục đích của Sự kiện
- **Làm chủ việc Phân phối Nội dung Toàn cầu**: Cung cấp sự hiểu biết sâu sắc về cách tận dụng Amazon CloudFront để kiến trúc và tối ưu hóa việc phân phối nội dung trên toàn cầu, giảm độ trễ và cải thiện trải nghiệm người dùng.
- **Củng cố Bảo mật Ứng dụng**: Trang bị cho người tham dự kiến thức và công cụ để bảo vệ các ứng dụng web khỏi các mối đe dọa phổ biến, sử dụng AWS WAF để chặn lưu lượng truy cập độc hại, giảm thiểu các lỗ hổng OWASP Top 10 và phòng chống các cuộc tấn công bằng bot.
- **Cung cấp Trải nghiệm Thực hành, Thực tế**: Vượt ra ngoài lý thuyết và cho phép người tham dự áp dụng những gì đã học vào môi trường lab, triển khai các cấu hình tối ưu hóa và bảo mật trong các kịch bản thực tế.
- **Xây dựng Nền tảng An toàn và Tối ưu**: Cung cấp một bản thiết kế toàn diện để sử dụng các Dịch vụ AWS Edge làm lớp nền tảng để xây dựng các ứng dụng web mạnh mẽ, có khả năng mở rộng và an toàn cho người dùng toàn cầu.

### Danh sách Diễn giả
-   **Nguyễn Gia Hưng** - Trưởng phòng Kiến trúc sư Giải pháp (Head of Solutions Architect)
-   **Julian Ju** - Kiến trúc sư Giải pháp Chuyên gia Cấp cao mảng Dịch vụ Edge (Senior Edge Services Specialist Solutions Architect)
-   **Kevin Lim**

### Nội dung chính & Điểm nổi bật

#### Từ Edge đến Origin: CloudFront là Nền tảng của bạn
- **Trọng tâm**: Phiên chính đầu tiên đã thiết lập các nguyên tắc cốt lõi của việc sử dụng Mạng phân phối nội dung (CDN) để tăng tốc và mở rộng quy mô ứng dụng web.
- **Nội dung**: Diễn giả Nguyễn Gia Hưng đã cung cấp một cái nhìn tổng quan toàn diện về Amazon CloudFront. Phiên này bao gồm:
    - **Kiến trúc Toàn cầu**: Cách tận dụng mạng lưới các vị trí biên (edge locations) toàn cầu của AWS để phục vụ nội dung gần người dùng hơn.
    - **Các tính năng CDN cốt lõi**: Các chiến lược lưu trữ đệm (caching), tối ưu hóa việc phân phối nội dung và tích hợp với các máy chủ gốc (origin servers) như S3 hoặc EC2.
    - **Bảo mật tại Lớp Biên**: Giới thiệu về các biện pháp kiểm soát bảo mật có sẵn trực tiếp trong CloudFront để xây dựng một lớp phòng thủ đầu tiên.
- **Điểm nhấn chính**: Các sơ đồ kiến trúc rõ ràng cho thấy cách một yêu cầu của người dùng được định tuyến thông minh đến vị trí biên gần nhất, thay vì phải đi một chặng đường dài đến máy chủ gốc, là một khoảnh khắc "à há" nền tảng. Nó minh họa một cách hoàn hảo những lợi ích về hiệu suất có thể đạt được với một CDN được cấu hình tốt.
- **Phiên liên quan**: "Từ Biên đến Nguồn: CloudFront là Nền tảng của bạn".

#### Phòng thủ Bề mặt Tấn công: WAF & Bảo vệ Ứng dụng
- **Trọng tâm**: Phiên này chuyển từ hiệu suất sang bảo mật, trình bày cách xây dựng một hệ thống phòng thủ mạnh mẽ chống lại một loạt các mối đe dọa trên nền tảng web.
- **Nội dung**: Julian Ju đã trình bày chuyên sâu về Tường lửa Ứng dụng Web của AWS (AWS WAF). Các chủ đề chính bao gồm:
    - **Giảm thiểu Mối đe dọa**: Sử dụng AWS WAF để chặn lưu lượng truy cập độc hại và giảm thiểu các lỗ hổng phổ biến như OWASP Top 10 (ví dụ: SQL injection, Cross-Site Scripting).
    - **Phòng chống Bot**: Các chiến lược và bộ quy tắc để xác định và phòng thủ chống lại các cuộc tấn công tự động bằng bot có thể thu thập dữ liệu, làm sai lệch phân tích hoặc cố gắng đánh cắp thông tin đăng nhập.
    - **Quy tắc được Quản lý & Tùy chỉnh**: Giới thiệu về việc sử dụng cả Quy tắc do AWS quản lý (Managed Rules) để bảo vệ nhanh chóng và cách xây dựng các quy tắc tùy chỉnh cho các mối đe dọa cụ thể của ứng dụng.
- **Điểm nhấn chính**: Phần trình diễn trực tiếp về việc một quy tắc WAF có thể được cấu hình dễ dàng như thế nào để chặn một cuộc tấn công SQL injection giả lập là cực kỳ ấn tượng. Nó cho thấy một mối đe dọa phức tạp có thể bị vô hiệu hóa trong vài phút, mang lại giá trị bảo mật hữu hình và ngay lập tức.
- **Phiên liên quan**: "Phòng thủ Bề mặt Tấn công: WAF & Bảo vệ Ứng dụng".

#### Từ Lý thuyết đến Thực hành: Các Workshop Thực hành
- **Trọng tâm**: Toàn bộ buổi chiều được dành cho việc ứng dụng thực tế, cho phép người tham dự triển khai các khái niệm từ các phiên buổi sáng trong một môi trường lab có hướng dẫn.
- **Nội dung**: Dưới sự dẫn dắt của đội ngũ chuyên gia, các bài lab được chia thành hai lĩnh vực cốt lõi:
    - **Tối ưu hóa**: Người tham dự đã cấu hình một bản phân phối CloudFront, triển khai các chính sách lưu trữ đệm và thực hành qua các kịch bản để tối ưu hóa việc phân phối nội dung cho một ứng dụng web mẫu.
    - **Bảo mật**: Bài lab thứ hai tập trung vào việc triển khai AWS WAF, áp dụng các bộ quy tắc để bảo vệ chống lại các cuộc tấn công phổ biến và quan sát cách tường lửa chặn các yêu cầu độc hại giả lập trong thời gian thực.
- **Điểm nhấn chính**: Bài lab thực hành về bảo mật là một điểm nhấn nổi bật. Sau khi tìm hiểu về các mối đe dọa vào buổi sáng, việc thực sự cấu hình WAF và sau đó chạy một kịch bản tấn công để thấy nó bị chặn mang lại cảm giác vô cùng thỏa mãn và củng cố những kiến thức đã học trong ngày. Việc thảo luận về các triển khai trong thế giới thực với các chuyên gia trong suốt buổi lab là vô giá.
- **Các phiên liên quan**: "Workshop Thực hành: Tối ưu hóa Ứng dụng Web Internet" & "Workshop Thực hành: Bảo mật Ứng dụng Web Internet".

### Những gì bạn đã học được

#### Về Hiệu suất & Quy mô Toàn cầu
- **Kiến trúc cho Độ trễ thấp**: Bạn đã học cách sử dụng Amazon CloudFront để thiết kế một chiến lược phân phối nội dung phục vụ người dùng từ các vị trí biên trên khắp thế giới, cải thiện đáng kể thời gian tải và khả năng phản hồi của ứng dụng.
- **Triển khai các Chiến lược Caching**: Bây giờ bạn đã hiểu cách cấu hình các chính sách lưu trữ đệm để giảm tải cho máy chủ gốc, giảm chi phí truyền dữ liệu và xây dựng một nền tảng ứng dụng có khả năng mở rộng và bền vững hơn.

#### Về Bảo mật Ứng dụng
- **Triển khai Tường lửa Ứng dụng Web**: Bạn đã có được các kỹ năng thực tế để triển khai và cấu hình AWS WAF nhằm bảo vệ ứng dụng của mình khỏi các lỗ hổng phổ biến và lưu lượng truy cập độc hại.
- **Giảm thiểu các Mối đe dọa Phổ biến**: Giờ đây bạn đã được trang bị để giảm thiểu các mối đe dọa được liệt kê trong OWASP Top 10 và bảo vệ ứng dụng của mình khỏi các bot tự động có hại, bảo vệ dữ liệu và người dùng của bạn.

#### Về Triển khai Thực tế
- **Kết nối Lý thuyết và Thực hành**: Thông qua các bài lab thực hành, bạn đã chuyển đổi kiến thức lý thuyết về CDN và WAF thành các kỹ năng thực tế, có thể lặp lại mà bạn có thể áp dụng trực tiếp vào các dự án của riêng mình.
- **Giải quyết các Kịch bản Thực tế**: Bạn đã làm việc qua các kịch bản thực tế cho cả việc tối ưu hóa và bảo mật một ứng dụng web, tích lũy kinh nghiệm trong việc đưa ra các quyết định đánh đổi và triển khai các giải pháp hiệu quả với sự hướng dẫn của chuyên gia.

### Trải nghiệm tại Sự kiện
Workshop cả ngày "Dịch vụ AWS Edge" là một lớp học chuyên sâu, cực kỳ bổ ích. Ngay từ khi bắt đầu, đã có một cảm giác rõ ràng rằng đây không chỉ là một chuỗi các bài giảng, mà là một hành trình gắn kết qua toàn bộ vòng đời của việc bảo mật và tăng tốc một ứng dụng web hiện đại.

#### Buổi sáng: Xây dựng Bản thiết kế
Các phiên buổi sáng giống như được trao cho những bản thiết kế kiến trúc để xây dựng một pháo đài kỹ thuật số. Phiên về CloudFront đã đặt nền móng, chỉ cho chúng tôi cách xây dựng một "xa lộ" toàn cầu siêu tốc cho lưu lượng truy cập ứng dụng của mình. Ngay sau đó, phiên về WAF giống như học cách xây dựng những "bức tường" và "cổng thành" bất khả xâm phạm để bảo vệ xa lộ đó khỏi những kẻ xâm lược. Sự tiến triển hợp lý từ hiệu suất đến bảo mật đã xây dựng nên một mô hình tư duy hoàn chỉnh và mạnh mẽ.

#### Buổi chiều: Vượt qua sự phức tạp trong thực tế
Sau bữa trưa, workshop chuyển từ bản thiết kế sang thi công. Các bài lab thực hành là nơi lý thuyết trở thành hiện thực hữu hình. Việc cấu hình CloudFront để tăng tốc một trang web chậm chạp và sau đó thiết lập các quy tắc WAF để chống lại các cuộc tấn công giả lập mang lại cảm giác vô cùng mạnh mẽ. Đó là sự khác biệt giữa việc nhìn vào sơ đồ của một chiếc khiên và việc thực sự dùng nó để đỡ một đòn tấn công. Khả năng đặt câu hỏi cho các chuyên gia trong suốt buổi lab đã biến nó từ một buổi hướng dẫn đơn giản thành một buổi huấn luyện cá nhân hóa, giúp các bài học trở nên dễ nhớ và sâu sắc.
