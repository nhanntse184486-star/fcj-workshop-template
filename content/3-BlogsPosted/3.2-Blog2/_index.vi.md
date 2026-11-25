---
title: "Blog 2"
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

## Amazon S3: Lưu trữ, bảo vệ và phân phối dữ liệu trên AWS

### 1. Tổng quan Amazon S3

Amazon S3 cung cấp mô hình lưu trữ đối tượng, phù hợp với tệp ứng dụng, tài liệu, dữ liệu phân tích, bản sao lưu và tài nguyên website tĩnh. Khác với ổ đĩa gắn vào máy chủ, ứng dụng tương tác với dữ liệu thông qua các thao tác API đối với object.

### 2. Cấu trúc dữ liệu

Bucket là vùng chứa; object là dữ liệu cùng metadata; object key xác định đối tượng trong bucket. Chẳng hạn key `dashboard/assets/app.js` biểu diễn một tệp JavaScript. Dấu gạch chéo trong key tạo cách tổ chức theo prefix, không yêu cầu tạo thư mục vật lý.

### 3. Lựa chọn Storage Class

| Lớp lưu trữ | Nhu cầu phù hợp |
|---|---|
| S3 Standard | Dữ liệu được truy cập thường xuyên |
| S3 Intelligent-Tiering | Tần suất truy cập biến động |
| S3 Standard-IA | Truy cập ít, cần lấy dữ liệu nhanh |
| S3 One Zone-IA | Dữ liệu có thể tái tạo, chấp nhận lưu một AZ |
| S3 Glacier Instant Retrieval | Lưu lâu, cần truy xuất nhanh |
| S3 Glacier Flexible Retrieval | Lưu trữ dài hạn, chấp nhận chờ khôi phục |
| S3 Glacier Deep Archive | Dữ liệu lưu trữ rất ít truy cập |
| S3 Express One Zone | Tác vụ cần độ trễ thấp, sử dụng directory bucket |

Cần cân nhắc phí truy xuất, thời gian lưu tối thiểu và khả năng khôi phục trước khi chọn lớp lưu trữ.

### 4. Quản lý và bảo mật

- Giữ Block Public Access cho bucket riêng tư; chỉ cấp quyền IAM và bucket policy cần thiết.
- Sử dụng mã hóa phía máy chủ; cân nhắc SSE-KMS khi cần quản lý khóa và kiểm toán phù hợp.
- Dùng Versioning để hỗ trợ khôi phục khi ghi đè hoặc xóa nhầm; kiểm soát các phiên bản cũ bằng Lifecycle.
- Thiết lập Lifecycle để chuyển lớp hoặc hết hạn đối tượng theo chính sách lưu giữ.

### 5. Hướng dẫn thực hành: phân phối frontend an toàn

1. Tạo general purpose bucket tại Region đã chọn, giữ Block Public Access.
2. Tải các file HTML, CSS và JavaScript của một website mẫu lên bucket; kiểm tra kiểu nội dung của file.
3. Tạo CloudFront distribution với S3 REST origin (không dùng S3 website endpoint) và Origin Access Control (OAC).
4. Cập nhật bucket policy để chỉ cho phép CloudFront distribution được chỉ định đọc object cần thiết.
5. Cấu hình default root object là `index.html`, sử dụng HTTPS và kiểm tra truy cập qua tên miền CloudFront.
6. Kiểm tra rằng truy cập trực tiếp object không có quyền bị từ chối, trong khi CloudFront vẫn phân phối được nội dung. Xóa tài nguyên thử nghiệm khi không còn cần thiết.

### 6. Ứng dụng trong Data & Machine Learning

S3 có thể lưu bộ dữ liệu, artifact mô hình và kết quả phân tích. Có thể tổ chức dữ liệu theo prefix, phân quyền các bước xử lý và đặt thời hạn lưu giữ phù hợp. Chi phí phụ thuộc không chỉ vào dung lượng mà còn vào request, truy xuất và truyền dữ liệu.

### 7. Liên hệ với đề tài thực tập

Trong kiến trúc Driver Drowsiness & Distraction Detection, S3 giữ các tài nguyên frontend của dashboard và CloudFront phân phối giao diện thông qua OAC. Video webcam không được đưa lên S3; metadata sự kiện được lưu trong DynamoDB. Cách tách này phù hợp mục tiêu giảm truyền dữ liệu và bảo vệ quyền riêng tư của đề tài.

### 8. Kết luận

Một thiết kế S3 hiệu quả cần xác định rõ dữ liệu nào được lưu, ai được đọc, dữ liệu tồn tại bao lâu và được truy xuất qua kênh nào. Không nên mở bucket công khai chỉ để đơn giản hóa việc triển khai website.

### Tài liệu tham khảo

- [Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
- [S3 security](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security.html)
- [S3 Lifecycle](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html)
- [CloudFront private S3 origin](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html)
