---
title: "Worklog Tuần 9"
date: 2026-11-16
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu tuần 9:

* Tích hợp đăng nhập Cognito và xác thực JWT trên API Gateway.
* Xây dựng giao diện hiển thị các sự kiện đã ghi nhận.
* Rà soát IAM, CloudWatch và các yêu cầu giám sát, bảo mật.

### Nội dung công việc theo tuần:

| STT | Công việc | Tài liệu tham khảo |
| --- | --- | --- |
| 1 | Tích hợp đăng nhập Cognito và xác thực JWT trên API Gateway. | <https://docs.aws.amazon.com/cognito/> |
| 2 | Kết nối Dashboard với API truy vấn lịch sử sự kiện. | <https://docs.aws.amazon.com/apigateway/> |
| 3 | Xây dựng giao diện hiển thị các sự kiện đã ghi nhận. | Proposal – Driver Drowsiness & Distraction Detection |
| 4 | Triển khai frontend trên S3 private và CloudFront OAC theo kiến trúc. | <https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/> |
| 5 | Rà soát IAM, CloudWatch và các yêu cầu giám sát, bảo mật. | <https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/> |

### Kết quả đạt được:

* Hoàn thành Dashboard kết nối với backend AWS, hỗ trợ theo dõi các sự kiện đã ghi nhận.
* Kiến trúc truy cập sử dụng Cognito và JWT; frontend được thiết kế với S3 private và CloudFront OAC để hạn chế truy cập trực tiếp vào nguồn lưu trữ.
* Xác định vai trò của IAM và CloudWatch trong phân quyền, ghi log và giám sát hệ thống.
