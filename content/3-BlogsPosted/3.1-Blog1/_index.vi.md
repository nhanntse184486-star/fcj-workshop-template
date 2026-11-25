---
title: "Blog 1"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

## CI/CD trên AWS: Tích hợp DevSecOps trước khi triển khai Production

### 1. Bài toán và mục tiêu

Tốc độ phát hành không có nhiều ý nghĩa nếu một phiên bản mới mang theo lỗ hổng bảo mật. DevSecOps đưa việc kiểm tra rủi ro vào quy trình phát triển để lỗi có thể được xử lý ngay tại giai đoạn phát sinh, thay vì chỉ kiểm tra khi ứng dụng đã vận hành.

### 2. Thiết kế pipeline trên AWS

Luồng tham khảo: `Git → CodePipeline → CodeBuild → Staging → Security tests → Manual approval → Production`.

| Thành phần | Vai trò |
|---|---|
| AWS CodePipeline | Điều phối các giai đoạn phát hành |
| AWS CodeBuild | Build, unit test và chạy công cụ quét |
| Amazon S3 | Lưu artifact của pipeline |
| AWS CodeDeploy | Điều phối triển khai nếu sử dụng nền tảng tương thích |
| Amazon ECS | Môi trường chạy ứng dụng container trong kiến trúc tham khảo |
| Amazon CloudWatch | Theo dõi log và kết quả thực thi |

### 3. Tích hợp kiểm tra bảo mật

| Phương pháp | Thời điểm và mục đích |
|---|---|
| SCA | Kiểm tra dependency và lỗ hổng đã được công bố |
| SAST | Phân tích mã nguồn trước khi chạy ứng dụng |
| DAST | Kiểm thử ứng dụng đang chạy trên staging được phép kiểm thử |
| IAST | Phân tích ứng dụng có gắn công cụ quan sát trong khi thực thi |

Cần định nghĩa tiêu chí chặn phát hành, chẳng hạn một phát hiện nghiêm trọng chưa được xử lý. Báo cáo quét phải được lưu lại để người phê duyệt đánh giá. Các công cụ quét cần được cấu hình riêng; CodePipeline không tự thực hiện tất cả các phép kiểm tra này.

### 4. Quy trình triển khai tham khảo

1. Kết nối repository Git với pipeline và cấu hình nhánh phát hành.
2. Tạo build project, chạy unit test và kiểm tra SCA/SAST. Dừng build khi vi phạm chính sách bảo mật.
3. Đóng gói artifact và đưa phiên bản đã build lên staging.
4. Chạy DAST/IAST trong môi trường kiểm thử phù hợp; tổng hợp các phát hiện.
5. Đặt bước Manual Approval trước Production. Sau phê duyệt, triển khai đúng artifact đã kiểm thử.
6. Theo dõi lỗi sau triển khai và chuẩn bị phương án quay lại phiên bản ổn định.

### 5. Giám sát, phân quyền và kiểm soát rủi ro

Sử dụng IAM role riêng cho từng giai đoạn với quyền tối thiểu; không lưu access key trong Git. Có thể dùng Parameter Store hoặc Secrets Manager cho thông tin cấu hình nhạy cảm. CloudTrail hỗ trợ truy vết hoạt động API, AWS Config theo dõi cấu hình tài nguyên, còn Security Hub tập hợp các phát hiện từ những nguồn được tích hợp.

Các tình huống nên kiểm thử bao gồm dependency có lỗ hổng, build thất bại, người dùng không có quyền phê duyệt và phiên bản mới gây lỗi trên staging. Pipeline cần chặn phiên bản không đáp ứng yêu cầu thay vì tiếp tục triển khai.

### 6. Liên hệ với đề tài thực tập

Đối với Driver Drowsiness & Distraction Detection, nguyên tắc kiểm tra trước phát hành có thể áp dụng cho mã Lambda, frontend dashboard và thư viện Python tại Edge. Việc này giúp hạn chế đưa secret vào repository và phát hiện lỗi dependency trước khi cập nhật hệ thống. Đây là hướng áp dụng kiến thức, không phải khẳng định đề tài đang sử dụng CodePipeline hoặc ECS.

### 7. Kết luận

DevSecOps là cách tổ chức quy trình phát hành có trách nhiệm: tự động hóa các kiểm tra lặp lại, xác định điều kiện chấp nhận và bảo đảm mỗi lần triển khai đều có thể truy vết.

### Tài liệu tham khảo

- [AWS CodePipeline](https://docs.aws.amazon.com/codepipeline/latest/userguide/welcome.html)
- [Manual approval](https://docs.aws.amazon.com/codepipeline/latest/userguide/approvals.html)
- [AWS CodeBuild](https://docs.aws.amazon.com/codebuild/)
- [AWS Security Hub](https://docs.aws.amazon.com/securityhub/)
- [AWS CloudTrail](https://docs.aws.amazon.com/cloudtrail/)
