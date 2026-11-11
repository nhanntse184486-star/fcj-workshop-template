---
title: "Worklog Tuần 4"
date: 2026-10-26
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---



### Mục tiêu tuần 4:

* Tìm hiểu các nguyên tắc High Availability, Fault Tolerance và Scalability trong AWS.
* Nghiên cứu Elastic Load Balancing (ELB) và Amazon EC2 Auto Scaling.
* Tìm hiểu Amazon CloudWatch và những phương pháp giám sát tài nguyên, ứng dụng.
* Nghiên cứu Amazon Route 53 và Amazon CloudFront.
* Tìm hiểu Serverless Computing và kiến trúc hướng sự kiện (Event-driven Architecture).
* Nghiên cứu AWS Lambda, bao gồm Function, Trigger, Execution Role và Runtime.
* Tìm hiểu Amazon API Gateway và cách tích hợp với AWS Lambda.
* Phân tích các mô hình kiến trúc đảm bảo khả năng mở rộng, giám sát và vận hành hiệu quả.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2 | - Nghiên cứu High Availability, Scalability và Fault Tolerance.<br>- Tìm hiểu vai trò của nhiều Availability Zone trong việc nâng cao tính sẵn sàng.<br>- Nghiên cứu Elastic Load Balancing và các thành phần chính.<br>- Phân biệt Application Load Balancer và Network Load Balancer.<br>- Tìm hiểu Auto Scaling Group và Scaling Policy.<br>- Phân tích kiến trúc Web Application có tính sẵn sàng cao sử dụng Load Balancer và Auto Scaling. | 26/10/2026 | 26/10/2026 | <https://cloudjourney.awsstudygroup.com/1-explore/><br><https://000021.awsstudygroup.com/> |
| 3 | - Tìm hiểu dịch vụ Amazon CloudWatch và vai trò trong giám sát hệ thống.<br>- Nghiên cứu CloudWatch Metrics, Logs, Dashboards và Alarms.<br>- Phân tích những chỉ số như CPU Utilization, Network Traffic và Request Count.<br>- Tìm hiểu phương pháp theo dõi trạng thái hoạt động của EC2 Instance.<br>- Nghiên cứu cách sử dụng CloudWatch để hỗ trợ xác định lỗi.<br>- Tìm hiểu mối quan hệ giữa CloudWatch Alarm và Auto Scaling. | 27/10/2026 | 27/10/2026 | <https://cloudjourney.awsstudygroup.com/1-explore/><br><https://docs.aws.amazon.com/cloudwatch/> |
| 4 | - Tìm hiểu Amazon Route 53 và hệ thống Domain Name System (DNS).<br>- Nghiên cứu các loại DNS Record và quá trình phân giải tên miền.<br>- Tìm hiểu Route 53 Routing Policy và Health Check.<br>- Nghiên cứu Amazon CloudFront và Content Delivery Network (CDN).<br>- Tìm hiểu Caching và vai trò của Edge Location trong phân phối nội dung.<br>- Phân tích cách kết hợp Route 53, CloudFront và S3 trong ứng dụng Web. | 28/10/2026 | 28/10/2026 | <https://cloudjourney.awsstudygroup.com/1-explore/><br><https://docs.aws.amazon.com/cloudfront/> |
| 5 | - Nghiên cứu Serverless Computing và Event-driven Architecture.<br>- Tìm hiểu AWS Lambda và cách thức hoạt động.<br>- Nghiên cứu Lambda Function, Runtime Environment và Event Trigger.<br>- Tìm hiểu Lambda Execution Role và quyền IAM cần thiết.<br>- So sánh EC2-based Application và Serverless Application.<br>- Nghiên cứu các trường hợp sử dụng Lambda kết hợp với S3 và những dịch vụ AWS khác. | 29/10/2026 | 29/10/2026 | <https://cloudjourney.awsstudygroup.com/4-modernize/><br><https://docs.aws.amazon.com/lambda/> |
| 6 | - Tìm hiểu Amazon API Gateway và nguyên tắc REST API.<br>- Nghiên cứu cách tích hợp API Gateway với AWS Lambda.<br>- Tìm hiểu quá trình tiếp nhận Request và trả về Response trong ứng dụng Serverless.<br>- Nghiên cứu tương tác giữa AWS Lambda, Amazon DynamoDB và Amazon S3.<br>- Tìm hiểu Logging và Error Handling trong Lambda.<br>- Phân tích kiến trúc Serverless sử dụng API Gateway, Lambda và DynamoDB.<br>- Tổng hợp kiến thức đã nghiên cứu trong tuần. | 30/10/2026 | 30/10/2026 | <https://cloudjourney.awsstudygroup.com/4-modernize/><br><https://000078.awsstudygroup.com/> |


### Kết quả đạt được tuần 4:

* Hiểu những khái niệm High Availability, Scalability và Fault Tolerance.
* Nắm được vai trò của Elastic Load Balancing trong việc phân phối lưu lượng truy cập.
* Hiểu cách Amazon EC2 Auto Scaling điều chỉnh năng lực tính toán theo nhu cầu.
* Nghiên cứu kiến trúc triển khai ứng dụng trên nhiều Availability Zone.
* Tìm hiểu Amazon CloudWatch và những thành phần:
  * CloudWatch Metrics.
  * CloudWatch Logs.
  * CloudWatch Dashboards.
  * CloudWatch Alarms.
* Hiểu vai trò của CloudWatch trong việc theo dõi hiệu suất và hỗ trợ xử lý sự cố.
* Nắm được chức năng của Amazon Route 53 trong quản lý DNS.
* Tìm hiểu cách Amazon CloudFront cải thiện tốc độ phân phối nội dung.
* Nghiên cứu những khái niệm nền tảng của Serverless Computing.
* Hiểu các thành phần của AWS Lambda:
  * Lambda Function.
  * Execution Role.
  * Runtime Environment.
  * Event Trigger.
* Nắm được cách Amazon API Gateway kết nối các ứng dụng với Lambda Function.
* Tìm hiểu kiến trúc ứng dụng Serverless kết hợp API Gateway, Lambda và DynamoDB.
* Hiểu sự khác biệt giữa kiến trúc ứng dụng sử dụng EC2 và Serverless.
* Nâng cao kiến thức về giám sát, khả năng mở rộng và tính sẵn sàng của hệ thống Cloud.


