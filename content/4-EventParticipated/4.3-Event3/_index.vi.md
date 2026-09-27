---
title: "Event 3"
date: 2025-09-09
weight: 1
chapter: false
pre: " <b> 4.3. </b> "
--- 

# Bài thu hoạch sự kiện: AWS Cloud Mastery Series #1 : AI/ML/GenAI on AWS Workshop

### Mục Đích Của Sự Kiện
- **Thấm nhuần tư duy DevOps**: Vượt ra ngoài khuôn khổ công cụ để xây dựng một sự hiểu biết sâu sắc về văn hóa, nguyên tắc và các chỉ số quan trọng (như DORA) của DevOps, vốn là động lực thúc đẩy các đội ngũ công nghệ hiệu suất cao.
- **Cung cấp một bản thiết kế kỹ thuật toàn diện**: Mang đến một buổi tìm hiểu chuyên sâu cả ngày về bộ công cụ DevOps của AWS, bao quát toàn bộ vòng đời phân phối phần mềm từ mã nguồn đến giám sát trên môi trường sản phẩm.
- **Làm chủ tự động hóa qua các buổi demo trực tiếp**: Cung cấp các buổi trình diễn thực tế, thực hành về các quy trình DevOps cốt lõi, bao gồm xây dựng một đường ống CI/CD hoàn chỉnh, triển khai hạ tầng bằng mã lệnh (IaC) và quản lý các ứng dụng container hóa.
- **Xây dựng hệ thống bền vững và có khả năng quan sát**: Trang bị cho người tham dự kiến thức không chỉ để triển khai ứng dụng hiệu quả mà còn để giám sát, truy vết và quản lý chúng một cách hiệu quả trong một môi trường phân tán phức tạp.

### Danh Sách Diễn Giả
-   **Trương Quang Tính** - DevOps Engineer, TymeX
-   **Văn Hoàng Kha** - Cloud Security Engineer, AWS Vietnam
-   **Quốc Bảo** - AWS Vietnam
-   **Phúc Thịnh** - AWS Vietnam
-   **Đại Vĩ** - AWS Vietnam
-   **Long Huỳnh** - AWS Vietnam
-   **Quy Phạm** - AWS Vietnam
-   **Nghiêm Lê** - AWS Vietnam


### Nội Dung Chính & Điểm Nổi Bật

#### Từ Mã nguồn đến Đám mây: Đường ống CI/CD Tự động
- **Trọng tâm**: Phiên chính của buổi sáng tập trung vào việc xây dựng một đường ống phân phối phần mềm hoàn toàn tự động từ đầu.
- **Nội dung**: Các diễn giả đã đi sâu vào bộ công cụ AWS CodeSuite, bao gồm:
    - **Mã nguồn (Source)**: AWS CodeCommit và các phương pháp tốt nhất cho chiến lược Git (GitFlow vs. Trunk-based).
    - **Xây dựng & Kiểm thử (Build & Test)**: Cấu hình các bản build tự động và tích hợp các giai đoạn kiểm thử với CodeBuild.
    - **Triển khai (Deploy)**: Tìm hiểu chi tiết về CodeDeploy, so sánh các chiến lược triển khai hiện đại như Blue/Green, Canary và Rolling updates.
    - **Điều phối (Orchestrate)**: Kết nối mọi thứ lại với nhau thành một quy trình liền mạch với CodePipeline.
- **Khoảnh khắc chính**: Phần demo trực tiếp một đường ống CI/CD hoàn chỉnh đang hoạt động là một điểm nhấn nổi bật. Nó đã kết nối tất cả các mảnh ghép lý thuyết thành một hệ thống hữu hình, hoạt động được, cho thấy một commit mã nguồn duy nhất có thể tự động kích hoạt toàn bộ quy trình xây dựng, kiểm thử và triển khai như thế nào.
- **Phiên liên quan**: "AWS DevOps Services – CI/CD Pipeline".

#### Xây dựng Nền tảng: Hạ tầng bằng Mã lệnh (IaC)
- **Trọng tâm**: Phiên này chuyển từ mã ứng dụng sang hạ tầng, hướng dẫn người tham dự cách quản lý tài nguyên đám mây bằng phương pháp lập trình.
- **Nội dung**: Hội thảo đã khám phá hai công cụ IaC chính trên AWS: sức mạnh của phương pháp khai báo với AWS CloudFormation (templates, stacks, phát hiện sai lệch) và sự linh hoạt của phương pháp mệnh lệnh với AWS CDK, cho phép các nhà phát triển sử dụng các ngôn ngữ lập trình quen thuộc.
- **Khoảnh khắc chính**: Phần demo so sánh trực tiếp việc triển khai hạ tầng bằng cả CloudFormation và CDK đã mang lại sự sáng tỏ vô cùng. Cuộc thảo luận sau đó về việc khi nào nên chọn công cụ nào đã cung cấp cho người tham dự một khuôn khổ thực tế để đưa ra các quyết định kiến trúc sáng suốt.
- **Phiên liên quan**: "Infrastructure as Code (IaC)".

#### Vận hành Ứng dụng Hiện đại: Thế giới Containers
- **Trọng tâm**: Buổi chiều bắt đầu bằng việc tìm hiểu sâu về container hóa, xương sống của kiến trúc microservices hiện đại.
- **Nội dung**: Người tham dự được dẫn dắt qua một hành trình từ các khái niệm cơ bản của Docker đến việc quản lý containers ở quy mô lớn trên AWS. Nội dung bao gồm bảo mật images trong Amazon ECR, so sánh sức mạnh điều phối của Amazon ECS và EKS, và khám phá sự đơn giản của AWS App Runner cho các ứng dụng đơn giản.
- **Khoảnh khắc chính**: Case study và phần demo so sánh các chiến lược triển khai microservices khác nhau là cực kỳ giá trị. Nó đã minh họa những sự đánh đổi trong thực tế giữa sự đơn giản (App Runner), sự kiểm soát có quản lý (ECS) và sự linh hoạt tối đa (EKS).
- **Phiên liên quan**: "Container Services on AWS".

#### Khép lại Vòng lặp: Khả năng Quan sát Toàn diện (Full-Stack Observability)
- **Trọng tâm**: Phiên quan trọng này đã dạy người tham dự cách nhìn sâu vào bên trong các ứng dụng và hạ tầng của họ sau khi đã hoạt động.
- **Nội dung**: Bài trình bày đã vượt ra ngoài việc giám sát cơ bản, bao quát ba trụ cột của khả năng quan sát: logs và metrics với Amazon CloudWatch, và truy vết phân tán với AWS X-Ray. Các phương pháp tốt nhất để thiết lập cảnh báo, dashboards và quy trình trực ca (on-call) cũng được chia sẻ.
- **Khoảnh khắc chính**: Phần demo về khả năng quan sát toàn diện giống như bật đèn trong một căn phòng tối. Nó cho thấy cách truy vết một yêu cầu người dùng duy nhất khi nó đi qua nhiều microservices, xác định chính xác các điểm nghẽn và lỗi.
- **Phiên liên quan**: "Monitoring & Observability".

### Những Gì Học Được

#### Về Tự động hóa & Pipelines
- **Xây dựng Quy trình CI/CD Hoàn chỉnh**: Bạn đã học cách xây dựng một đường ống tự động trên AWS để lấy mã nguồn từ kho Git và triển khai an toàn lên môi trường sản phẩm bằng các chiến lược hiện đại như Blue/Green.
- **Quản lý Hạ tầng bằng Mã lệnh**: Giờ đây, bạn đã hiểu cách định nghĩa và quản lý toàn bộ hạ tầng đám mây của mình bằng mã lệnh, cho phép kiểm soát phiên bản, đánh giá chéo (peer review) và triển khai lặp lại cho các môi trường của bạn. Bạn cũng biết được những khác biệt chính giữa CloudFormation và CDK.

#### Về Triển khai Ứng dụng Hiện đại
- **Làm chủ Container hóa trên AWS**: Bạn đã có được một khuôn khổ rõ ràng để lựa chọn dịch vụ container phù hợp (ECS, EKS hoặc App Runner) dựa trên kỹ năng của đội ngũ, độ phức tạp của ứng dụng và nhu cầu vận hành.
- **Triển khai các Chiến lược Nâng cao**: Bạn đã vượt ra ngoài các phương pháp triển khai đơn giản và học cách sử dụng các kỹ thuật như feature flags và A/B testing để phát hành các tính năng mới cho người dùng với rủi ro tối thiểu và phản hồi tối đa.

#### Về Độ tin cậy và Vận hành Hệ thống
- **Xây dựng các Hệ thống có Khả năng Quan sát**: Bạn đã học cách tạo ra các hệ thống không chỉ được giám sát, mà thực sự có thể quan sát được. Giờ đây, bạn có thể sử dụng CloudWatch và X-Ray cùng nhau để có được những hiểu biết sâu sắc về hiệu suất ứng dụng và nhanh chóng chẩn đoán sự cố trong một kiến trúc phân tán.
- **Áp dụng các Phương pháp DevOps Tốt nhất**: Bạn đã có được những hiểu biết về các phương pháp đã được chứng minh cho việc quản lý sự cố, viết báo cáo sự cố (postmortems) hiệu quả và nuôi dưỡng một văn hóa cải tiến liên tục trong đội ngũ của mình.

### Trải nghiệm tại sự kiện
Buổi hội thảo cả ngày "DevOps on AWS" là một lớp học chuyên sâu đầy thử thách nhưng vô cùng xứng đáng. Ngay từ khi bắt đầu, đã có một cảm giác rõ ràng rằng đây không chỉ là một chuỗi các bài giảng, mà là một hành trình liền mạch xuyên suốt toàn bộ vòng đời của một ứng dụng hiện đại.

#### Buổi sáng: Xây dựng Bản thiết kế
Các phiên buổi sáng mang lại cảm giác như được trao tận tay bản thiết kế kiến trúc để xây dựng một nhà máy phần mềm đẳng cấp thế giới. Phần demo đường ống CI/CD không chỉ là một hướng dẫn kỹ thuật; đó là một khoảnh khắc khai sáng nơi khái niệm trừu tượng về tự động hóa trở thành một thực tế hữu hình và mạnh mẽ. Việc tìm hiểu sâu về IaC cũng mang lại cảm giác đầy sức mạnh tương tự, như thể chúng tôi được trao công cụ để "tạc nên" đám mây bằng mã lệnh, đảm bảo tính nhất quán và loại bỏ mãi mãi các lỗi thủ công.

#### Buổi chiều: Đối mặt với Sự phức tạp của Thế giới thực
Sau bữa trưa, hội thảo chuyển sang những thực tế phức tạp của việc vận hành các ứng dụng hiện đại. Phiên về containers là một lớp học chuyên sâu về việc điều hướng các lựa chọn, làm sáng tỏ bức tranh thường gây bối rối của ECS, EKS và App Runner. Phiên về khả năng quan sát có lẽ là phần mở mang tầm mắt nhất trong ngày. Phần demo cho thấy cách truy vết một yêu cầu duy nhất qua hàng chục dịch vụ là một minh chứng mạnh mẽ về cách "mò kim đáy bể", biến công việc gỡ lỗi khó khăn thành một quy trình có hệ thống.
