---
title: "Worklog Tuần 3"
date: 2026-10-19
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---



### Mục tiêu tuần 3:

* Tìm hiểu Amazon Elastic Compute Cloud (EC2) và vai trò của máy chủ ảo trong điện toán đám mây.
* Nghiên cứu EC2 Instance Type, Amazon Machine Image (AMI) và vòng đời của EC2 Instance.
* Tìm hiểu Amazon Elastic Block Store (EBS), EBS Volume và Snapshot.
* Nghiên cứu Amazon Simple Storage Service (S3), bao gồm Bucket, Object, Storage Class và Versioning.
* Tìm hiểu Amazon Relational Database Service (RDS) và các hệ quản trị cơ sở dữ liệu được hỗ trợ.
* Nghiên cứu Amazon DynamoDB và sự khác biệt giữa Relational Database và NoSQL Database.
* So sánh các dịch vụ Compute, Storage và Database dựa trên hiệu suất, bảo mật, khả năng mở rộng và chi phí.
* Xây dựng kiến thức nền tảng để triển khai những ứng dụng thực tế trên AWS.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2 | - Tìm hiểu dịch vụ Amazon EC2 và những tính năng chính.<br>- Nghiên cứu khái niệm máy chủ ảo và các trường hợp sử dụng EC2.<br>- Tìm hiểu EC2 Instance Type và Amazon Machine Image (AMI).<br>- Nghiên cứu vòng đời EC2 Instance: Pending, Running, Stopping, Stopped và Terminated.<br>- Tìm hiểu EC2 Key Pair và Security Group.<br>- Nghiên cứu các phương pháp kết nối và quản trị EC2 an toàn. | 19/10/2026 | 19/10/2026 | <https://cloudjourney.awsstudygroup.com/1-explore/><br><https://000004.awsstudygroup.com/> |
| 3 | - Tìm hiểu dịch vụ Amazon Elastic Block Store (EBS).<br>- Nghiên cứu EBS Volume, Volume Type và Snapshot.<br>- Hiểu cơ chế lưu trữ dữ liệu liên tục của EBS đối với EC2 Instance.<br>- So sánh EBS, Instance Store và Amazon Elastic File System (EFS).<br>- Nghiên cứu phương pháp sao lưu và phục hồi dữ liệu.<br>- Tìm hiểu mối quan hệ giữa tài nguyên Compute và Storage trong AWS. | 20/10/2026 | 20/10/2026 | <https://docs.aws.amazon.com/ebs/> |
| 4 | - Tìm hiểu dịch vụ Amazon Simple Storage Service (S3).<br>- Nghiên cứu S3 Bucket, Object và Object Metadata.<br>- Tìm hiểu các lớp lưu trữ S3 Storage Classes và Lifecycle Policies.<br>- Nghiên cứu S3 Versioning và cơ chế bảo vệ dữ liệu.<br>- Tìm hiểu S3 Bucket Policy và quyền truy cập Object.<br>- Nghiên cứu mô hình Static Website Hosting bằng Amazon S3. | 21/10/2026 | 21/10/2026 | <https://cloudjourney.awsstudygroup.com/1-explore/><br><https://000057.awsstudygroup.com/> |
| 5 | - Tìm hiểu Amazon Relational Database Service (RDS).<br>- Nghiên cứu các Database Engine như MySQL và PostgreSQL.<br>- Tìm hiểu Automated Backup, Snapshot và Multi-AZ Deployment.<br>- Nghiên cứu các vấn đề liên quan đến bảo mật và tính sẵn sàng của Database.<br>- Tìm hiểu Amazon DynamoDB và mô hình NoSQL.<br>- So sánh Relational Database và Key-value/Document Database. | 22/10/2026 | 22/10/2026 | <https://000005.awsstudygroup.com/><br><https://docs.aws.amazon.com/amazondynamodb/> |
| 6 | - Ôn tập Amazon EC2, EBS, S3, RDS và DynamoDB.<br>- So sánh Object Storage, Block Storage và File Storage.<br>- Phân tích kiến trúc ứng dụng Web sử dụng EC2, S3 và RDS.<br>- Tìm hiểu mối liên hệ giữa IAM, VPC, Compute và Database.<br>- Đánh giá cách lựa chọn dịch vụ AWS dựa trên khả năng mở rộng, bảo mật và chi phí.<br>- Tổng hợp kiến thức đã nghiên cứu và chuẩn bị nội dung tuần tiếp theo. | 23/10/2026 | 23/10/2026 | <https://cloudjourney.awsstudygroup.com/1-explore/> |


### Kết quả đạt được tuần 3:

* Hiểu được vai trò của Amazon EC2 trong việc cung cấp tài nguyên tính toán trên AWS.
* Nắm được những khái niệm quan trọng:
  * EC2 Instance Type.
  * Amazon Machine Image.
  * Key Pair.
  * Security Group.
  * Instance Lifecycle.
* Hiểu chức năng của Amazon EBS trong việc cung cấp Block Storage cho EC2.
* Tìm hiểu EBS Volume, Snapshot và các phương pháp sao lưu dữ liệu.
* Nắm được sự khác biệt giữa các dịch vụ lưu trữ:
  * Amazon S3: Object Storage.
  * Amazon EBS: Block Storage.
  * Amazon EFS: Shared File Storage.
* Nghiên cứu Amazon S3 và các tính năng Bucket, Object, Storage Classes, Versioning và Lifecycle Policies.
* Hiểu được cách hoạt động và các trường hợp sử dụng Amazon RDS.
* Tìm hiểu Amazon DynamoDB và các đặc điểm cơ bản của NoSQL.
* Hiểu sự khác biệt giữa Relational Database và NoSQL Database.
* Biết cách phân tích những yêu cầu về Compute, Storage và Database của một ứng dụng.
* Nâng cao khả năng lựa chọn dịch vụ AWS phù hợp dựa trên mục đích sử dụng và chi phí.
* Xây dựng nền tảng kiến thức để nghiên cứu các hệ thống có khả năng mở rộng và tính sẵn sàng cao.


