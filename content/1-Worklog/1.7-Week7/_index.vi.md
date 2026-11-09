---
title: "Báo cáo công việc Tuần 7"
date: 2026-10-26
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

- Hiểu cách CodePipeline điều phối source, build/test và deploy ứng dụng Node.js lên EC2.
- Phân biệt buildspec, AppSpec, artifact và vai trò IAM trong quá trình triển khai.
- Hiểu cách S3 File Gateway kết nối file share SMB/NFS với S3 và vai trò của local cache.
- Hiểu FSx for Windows, Multi-AZ, quyền SMB và cách theo dõi hiệu năng, tiết kiệm dung lượng, khôi phục file.
- Hiểu luồng ingestion, catalog, ETL, truy vấn SQL và trực quan hóa dữ liệu trong data lake trên S3.
- Hiểu cách thiết kế dữ liệu DynamoDB theo access pattern, bảo vệ cập nhật và tích hợp CDC, tìm kiếm ngữ nghĩa.
- Hiểu Savings Plans, Reserved Instances và cách chọn mức cam kết phù hợp với nhu cầu sử dụng.

### Các công việc cần triển khai trong tuần này:

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 1 | **Triển khai EC2 với AWS CodePipeline** <br> - Tìm hiểu pipeline triển khai Node.js lên EC2: commit vào CodeCommit kích hoạt CodePipeline qua EventBridge, rồi chuyển qua các bước build/test và deploy. <br> - CodeBuild chạy lệnh trong `buildspec.yml`, tạo artifact lưu trên S3. CodeDeploy dùng `appspec.yml` để biết file cần cài và các script hook như BeforeInstall, ApplicationStart. <br> - EC2 cần CodeDeploy Agent hoạt động, instance profile đọc được artifact và tag khớp deployment group. Phân biệt quyền service role với quyền của EC2; xét thêm quyền KMS khi artifact mã hóa bằng khóa KMS. <br> - Đọc log từng stage và lifecycle event để phân loại lỗi source, build, tải artifact hay khởi động ứng dụng. Kiểm tra ứng dụng qua HTTP sau deployment; build thành công chưa đủ xác nhận bản chạy đúng. | 26/10/2026 | 26/10/2026 | - [Module 3 - 21: Deploy applications to EC2 with AWS CodePipeline](https://000023.awsfcaj.com/) |
| 2 | **Deploy File Storage Gateway** <br> - Tìm hiểu S3 File Gateway để truy cập object S3 dưới dạng file qua SMB/NFS. Bài dùng appliance AMI trên EC2, kích hoạt gateway và cấp đĩa cache riêng. <br> - Tạo SMB share liên kết bucket, cấu hình guest access theo lab và dùng `net use` map ổ đĩa Windows. Phân biệt xác thực SMB với IAM role truy cập S3; kiểm tra đường mạng và SG cho client. <br> - File ghi vào cache rồi tải lên S3 bất đồng bộ. Dùng file thử để đối chiếu object trong bucket; mount thành công chưa xác nhận upload xong. Thay đổi trực tiếp trên S3 cần refresh danh mục cache để share cập nhật. | 27/10/2026 | 27/10/2026 | - [Module 3 - 22: Deploy File Storage Gateway](https://000024.awsfcaj.com/) |
| 3 | **Amazon FSx for Windows File Server** <br> - Tìm hiểu FSx for Windows là file system được quản lý, dùng SMB và Active Directory. Bài chuẩn bị VPC/client bằng CloudFormation; Multi-AZ sao chép đồng bộ giữa hai AZ và tự failover. <br> - Map share qua DNS/UNC, quản lý bằng Shared Folders/PowerShell; phân biệt quyền share/NTFS với IAM quản lý tài nguyên. <br> - So sánh SSD/HDD và tìm hiểu đo đọc/ghi bằng DiskSpd/fio, đối chiếu throughput, IOPS và latency trên CloudWatch. Phân biệt tăng dung lượng với tăng throughput capacity. <br> - Dedup giảm dữ liệu trùng; shadow copies khôi phục phiên bản file và dùng dung lượng file system. Quota Track theo dõi sử dụng; Enforce chặn ghi khi vượt hạn mức. | 28/10/2026 | 28/10/2026 | - [Module 3 - 23: Amazon FSx for Windows File Server](https://000025.awsfcaj.com/) |
| 4 | **Data Lake on AWS** <br> - Tìm hiểu data lake phân tích lượt nghe nhạc: KDG tạo JSON → Firehose gom lô → S3. Tách raw, reference và processed; buffering tạo độ trễ trước khi dữ liệu tới bucket. <br> - Glue Crawler suy luận schema/partition và lưu metadata trong Data Catalog; dữ liệu vẫn nằm ở S3. Phân biệt catalog với ETL. <br> - Đọc notebook PySpark để lọc activity, join danh mục bài hát theo `track_id`, bỏ cột thừa và ghi Parquet; cập nhật catalog cho dữ liệu đã xử lý. <br> - Athena truy vấn SQL; Parquet giúp giảm quét cột không cần thiết. Kết nối Athena với Quick Sight (Amazon Quick), phân biệt dataset, analysis và dashboard; kiểm tra schema/số bản ghi trước trực quan hóa. | 29/10/2026 | 29/10/2026 | - [Module 3 - 24: Data Lake on AWS](https://000035.awsfcaj.com/) |
| 5 | **Amazon DynamoDB Immersion Day** <br> - Tìm hiểu CRUD, Query/Scan và thiết kế PK/SK, GSI theo access pattern; filter không giảm read capacity. <br> - Phân biệt transaction, backup/PITR và CDC qua Lambda: cập nhật nguyên tử, khôi phục dữ liệu và xử lý thay đổi, tránh ghi trùng. <br> - Khảo sát DynamoDB → OpenSearch → Bedrock: embeddings hỗ trợ tìm ngữ nghĩa, LLM tạo gợi ý từ dữ liệu truy hồi; DynamoDB giữ dữ liệu gốc. | 30/10/2026 | 30/10/2026 | - [Module 3 - 25: Amazon DynamoDB Immersion Day](https://000039.awsfcaj.com/) |
| 6 | **Introduction Savings Plan** <br> - Tìm hiểu Savings Plans: cam kết USD/giờ trong 1 hoặc 3 năm, với ba cách thanh toán; phần cam kết chưa dùng vẫn phải trả. <br> - So sánh Compute SP cho EC2/Fargate/Lambda với EC2 Instance SP giới hạn family/Region; đọc Recommendations theo lịch sử sử dụng. <br> - Đối chiếu EC2 RI và Reserved DB Instances; kiểm tra cấu hình áp dụng, utilization/coverage để chọn cam kết phù hợp nhu cầu. | 31/10/2026 | 31/10/2026 | - [Module 3 - 26: Introduction Savings Plan](https://000042.awsfcaj.com/) |

### Kết quả đạt được tuần 7:

- Phân biệt công việc của CodeCommit, CodeBuild, CodeDeploy và CodePipeline.
- Hiểu cách kiểm tra artifact, agent, quyền truy cập và lifecycle hook để tìm lỗi; xác nhận ứng dụng sau deployment.
- Phân biệt xác thực SMB, quyền IAM truy cập S3 và quá trình upload bất đồng bộ; biết cách kiểm tra file share và object đích.
- Phân biệt storage capacity với throughput capacity, quota Track với Enforce; hiểu shadow copies hỗ trợ khôi phục file và vẫn cần backup.
- Phân biệt dữ liệu S3 với metadata trong Catalog và crawler với ETL; hiểu cách làm giàu dữ liệu, dùng Parquet và kiểm tra kết quả trước khi tạo biểu đồ.
- Hiểu ảnh hưởng của khóa/GSI và filter tới truy vấn, capacity; phân biệt transaction, backup, CDC và tìm kiếm trên dữ liệu sao chép.
- Phân biệt phạm vi ưu đãi và nghĩa vụ chi tiêu cam kết; biết đối chiếu khuyến nghị, utilization/coverage với nhu cầu thực tế.
