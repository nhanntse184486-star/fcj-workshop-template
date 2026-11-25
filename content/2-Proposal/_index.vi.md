---
title: "Bản đề xuất"
date: 2026-11-25
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Driver Drowsiness & Distraction Detection
## Giải pháp giám sát trạng thái tài xế bằng Edge AI và AWS Serverless

### 1. Tóm tắt điều hành

Dự án đề xuất xây dựng nguyên mẫu hệ thống phát hiện dấu hiệu buồn ngủ và mất tập trung của tài xế bằng Computer Vision, Edge AI và AWS Cloud. Webcam thu thập hình ảnh khuôn mặt và xử lý trực tiếp tại thiết bị bằng OpenCV kết hợp MediaPipe Face Mesh với 478 điểm mốc khuôn mặt. Các đặc trưng Eye Aspect Ratio (EAR), Mouth Aspect Ratio (MAR), tư thế đầu (Head Pose) và hướng nhìn (Gaze Direction, đang thử nghiệm) được trích xuất để phục vụ phân tích theo thời gian dựa trên luật.

Thay vì liên tục tải video lên cloud, thiết bị chỉ gửi metadata của sự kiện tới AWS. Backend serverless kiểm tra, lưu trữ và truy vấn sự kiện; dashboard web có xác thực cho phép người dùng được cấp quyền xem lịch sử. Thiết kế hướng đến giảm lưu lượng mạng, độ trễ, bảo vệ quyền riêng tư và giảm công việc quản trị hạ tầng. Đây là nguyên mẫu hỗ trợ nhận thức và nghiên cứu, không phải hệ thống an toàn lái xe đã được chứng nhận.

### 2. Tuyên bố vấn đề

#### Vấn đề hiện tại

Nhắm mắt kéo dài, ngáp, thay đổi tư thế đầu và hướng nhìn có thể là dấu hiệu liên quan đến buồn ngủ hoặc mất tập trung. Các dấu hiệu này thường khó nhận biết kịp thời. Xử lý video liên tục trên cloud đòi hỏi băng thông, có thể làm tăng độ trễ và chi phí, đồng thời ảnh hưởng quyền riêng tư đối với dữ liệu khuôn mặt. Kết nối mạng không ổn định cũng có thể gián đoạn việc ghi nhận sự kiện.

#### Giải pháp đề xuất

Hệ thống tách biệt quá trình nhận diện tại thiết bị và quản lý sự kiện trên cloud. Thiết bị biên trích xuất đặc trưng khuôn mặt và phân tích theo thời gian. Khi phát hiện một sự kiện ứng viên, hệ thống tạo `event_id` và chuẩn bị metadata để lưu đệm cục bộ và đồng bộ. AWS HTTP API chuyển sự kiện tới Lambda Writer để kiểm tra dữ liệu và thực hiện ghi có điều kiện vào DynamoDB nhằm tránh tạo bản ghi trùng lặp. Lambda Reader riêng biệt phục vụ truy vấn lịch sử đã được cấp quyền. Dashboard sử dụng Amazon Cognito để xác thực và Amazon CloudFront kết hợp S3 private để phân phối giao diện.

#### Lợi ích và hoàn vốn đầu tư (ROI)

- Xử lý video cục bộ giúp giảm sự phụ thuộc vào Internet trong quá trình nhận diện và tránh tải video gốc lên cloud.
- Lịch sử sự kiện tập trung hỗ trợ tra cứu và phân tích sau này mà không cần duy trì máy chủ riêng.
- Hạ tầng serverless tính phí theo mức sử dụng giúp hạn chế chi phí tài nguyên nhàn rỗi ở quy mô nguyên mẫu.
- Có thể tận dụng laptop và webcam hiện có để tránh chi phí mua thêm phần cứng.

Giá trị kỳ vọng chủ yếu về kỹ thuật và học tập: pipeline Edge AI có thể tái sử dụng, mô hình tích hợp cloud và kinh nghiệm thực hành AWS. Mức tiết kiệm tài chính, mức giảm tai nạn và ROI bằng tiền chưa được đo lường, do đó không đưa ra thời gian hoàn vốn cụ thể.

### 3. Kiến trúc giải pháp

Hệ thống đề xuất gồm ba lớp: xử lý tại thiết bị biên, quản lý sự kiện trên cloud và dashboard có xác thực.

![Sơ đồ kiến trúc hệ thống](/fcj-workshop-template/images/2-Proposal/architecture.jpg)

*Hình 1. Kiến trúc tổng thể hệ thống Driver Drowsiness & Distraction Detection sử dụng Edge AI kết hợp AWS Cloud.*

#### Luồng xử lý dữ liệu

1. Webcam thu thập hình ảnh khuôn mặt cục bộ. Video gốc không được gửi lên AWS.
2. OpenCV và MediaPipe Face Mesh trích xuất 478 điểm mốc, tính EAR, MAR, Head Pose và đặc trưng Gaze đang thử nghiệm.
3. Temporal Analysis sử dụng các luật nhận diện ứng viên và đánh giá dữ liệu CSV để phân biệt dấu hiệu bất thường kéo dài với chuyển động ngắn.
4. Mỗi sự kiện được cấp `event_id` duy nhất. Local Event Buffer dự kiến lưu metadata chưa đồng bộ và gửi lại khi kết nối được khôi phục.
5. Thiết bị gửi HTTPS `POST /events` đến Amazon API Gateway (HTTP API). Route tiếp nhận phải xác thực thiết bị hoặc danh tính được cấp quyền trước khi triển khai.
6. AWS Lambda Event Writer kiểm tra payload và sử dụng `PutItem` có điều kiện trên DynamoDB dựa vào khóa sự kiện duy nhất. Sự kiện gửi lại không được tạo bản ghi mới; sự kiện trùng đã được xác nhận cần được coi là đã đồng bộ.
7. Người dùng đăng nhập dashboard bằng Amazon Cognito và nhận JWT. API Gateway sử dụng JWT Authorizer cho các route được bảo vệ.
8. Request `GET /events` đã được cấp quyền gọi Lambda Event Reader để truy vấn DynamoDB và trả lịch sử theo phân trang. Backend phải kiểm tra quyền sở hữu hoặc phạm vi truy cập, không chỉ kiểm tra JWT hợp lệ.
9. Amazon CloudFront phân phối dashboard tĩnh từ Amazon S3 private thông qua Origin Access Control (OAC).
10. Amazon CloudWatch theo dõi API access logs, Lambda logs, lỗi, thời gian thực thi và DynamoDB metrics. AWS IAM, CloudTrail và AWS Budgets hỗ trợ phân quyền, kiểm toán và theo dõi chi phí. AWS WAF là tùy chọn; nếu sử dụng cần xác minh vị trí tích hợp và phạm vi bảo vệ.

#### Các dịch vụ AWS sử dụng hoặc dự kiến sử dụng

| Dịch vụ | Vai trò |
| --- | --- |
| Amazon API Gateway | HTTP API tiếp nhận và truy vấn sự kiện; JWT Authorizer cho các route được bảo vệ |
| AWS Lambda | Hai hàm riêng biệt Event Writer và Event Reader |
| Amazon DynamoDB | Lưu metadata, ghi có điều kiện và truy vấn lịch sử |
| Amazon Cognito | Đăng ký, đăng nhập và cấp JWT |
| Amazon S3 | Lưu trữ private các tệp dashboard tĩnh |
| Amazon CloudFront | Phân phối frontend qua HTTPS với OAC |
| Amazon CloudWatch | Logs, chỉ số vận hành và cảnh báo |
| AWS IAM | Phân quyền tối thiểu cần thiết cho tài nguyên AWS |
| AWS CloudTrail | Kiểm toán hoạt động tài khoản AWS |
| AWS Budgets | Theo dõi ngân sách và cảnh báo chi phí |
| AWS WAF (tùy chọn) | Bổ sung bảo vệ web nếu tương thích và phù hợp ngân sách |

#### Thiết kế thành phần và dữ liệu

Pipeline xử lý tại Edge hoạt động độc lập với kết nối cloud. Cấu trúc sự kiện dự kiến gồm `event_id`, `device_id`, `event_type`, thời điểm UTC, thời lượng và các đặc trưng đã trích xuất liên quan. Tên trường, ngưỡng nhận diện và thời gian lưu trữ chính thức sẽ được xác định khi triển khai. Thiết kế DynamoDB đề xuất dùng `event_id` làm khóa chính duy nhất và chỉ mục gồm phạm vi thiết bị/người dùng được cấp quyền kết hợp thời điểm để truy vấn lịch sử; cần kiểm chứng thiết kế chỉ mục và quyền truy cập theo nhu cầu truy vấn thực tế.

Writer cần từ chối payload không hợp lệ, xác minh danh tính và phạm vi thiết bị được phép, không lưu hình ảnh hoặc video gốc. Reader cần giới hạn dữ liệu theo phạm vi truy cập của người gọi. JWT, thông tin xác thực và payload nhạy cảm không được xuất hiện trong mã nguồn hay logs. S3 sẽ chặn truy cập công khai và chỉ cấp quyền origin cần thiết cho CloudFront OAC. Metadata sự kiện vẫn có thể là dữ liệu nhạy cảm, vì vậy cần kiểm soát quyền truy cập và thời gian lưu trữ.

### 4. Triển khai kỹ thuật

#### Các giai đoạn triển khai

1. **Edge AI và trích xuất đặc trưng:** cấu hình webcam, OpenCV và MediaPipe; tính EAR, MAR, Head Pose; đánh giá Gaze như một đặc trưng thử nghiệm.
2. **Phân tích theo thời gian và độ tin cậy:** phân tích dữ liệu CSV, hiệu chỉnh luật và thời lượng sự kiện, xây dựng bộ đệm cục bộ có lưu bền vững và cơ chế retry giới hạn với thời gian chờ tăng dần; giữ nguyên event ID khi gửi lại.
3. **Backend serverless:** cấu hình DynamoDB, Lambda Writer/Reader, API Gateway routes, kiểm tra request và ghi có điều kiện; xác minh quyền gửi sự kiện và truy vấn lịch sử.
4. **Dashboard và bảo mật:** cấu hình Cognito, JWT Authorizer và kiểm tra quyền ở backend; lưu frontend tĩnh trên S3 private, phân phối qua CloudFront OAC; cấu hình IAM và giám sát.
5. **Kiểm thử và Workshop:** kiểm tra nhận diện, đồng bộ, gửi trùng, phân quyền và tích hợp cloud; dự toán chi phí, chuẩn bị minh chứng demo và tài liệu dọn dẹp tài nguyên.

#### Yêu cầu kỹ thuật

| Lớp hệ thống | Yêu cầu |
| --- | --- |
| Thiết bị biên | Máy tính có webcam, Python, OpenCV, MediaPipe và bộ nhớ cục bộ |
| Phân tích | EAR, MAR, Head Pose, Gaze thử nghiệm, đánh giá CSV và luật theo thời gian |
| Cloud | Tài khoản AWS, HTTP API, Lambda, DynamoDB, Cognito, S3 và CloudFront |
| Vận hành | Quyền IAM, CloudWatch logs và alarms, CloudTrail và Budgets |
| Kiểm thử | Điều kiện ánh sáng và góc đầu có kiểm soát, mẫu sự kiện gắn nhãn, mất mạng, request trùng và request không được cấp quyền |

#### Tiến độ hiện tại

Theo phần tự đánh giá dự án, pipeline Edge sử dụng webcam và các đặc trưng khuôn mặt đã được xây dựng. Gaze vẫn ở mức thử nghiệm và Temporal Analysis dựa trên luật cần hoàn thiện thêm. Kiến trúc AWS đã được thiết kế; chưa xác nhận việc triển khai cloud đầu cuối và chức năng dashboard. Local Event Buffer, Retry, đánh giá độ chính xác và kiểm thử khôi phục kết nối là các phần cần tiếp tục thực hiện. Đây là các mục tiêu triển khai, không phải kết quả kiểm thử đã hoàn thành.

### 5. Lộ trình & Mốc triển khai

Lịch dưới đây là kế hoạch đề xuất, nối tiếp ngày tháng trong Worklog hiện có và mục tiêu nộp cuối tuần 10. Chỉ ghi nhận công việc hoàn thành khi đã thực hiện trên thực tế.

| Tuần | Thời gian (2026) | Công việc và mốc dự kiến |
| --- | --- | --- |
| 1–5 | 05/10 – 06/11 | Học AWS nền tảng, networking, compute, storage, serverless và AI/ML |
| 6 | 09/11 – 13/11 | Phân tích bài toán, xác định phạm vi, nghiên cứu Edge AI và thiết kế kiến trúc |
| 7 | 16/11 – 20/11 | Phát triển trích xuất đặc trưng khuôn mặt; đánh giá EAR, MAR, Head Pose và Gaze thử nghiệm |
| 8 | 23/11 – 27/11 | Hoàn thiện Temporal Analysis, bộ đệm và Retry; triển khai backend tiếp nhận sự kiện |
| 9 | 30/11 – 04/12 | Triển khai truy vấn lịch sử có phân quyền, dashboard, IAM và giám sát |
| 10 | 07/12 – 11/12 | Kiểm thử nguyên mẫu, hoàn thiện dự toán, viết Workshop, chuẩn bị demo và nộp báo cáo |

Mục tiêu nộp: **11/12/2026**, phụ thuộc vào việc FCAJ xác nhận lịch nộp và thời lượng thực tập theo yêu cầu.

### 6. Ước tính ngân sách

#### Giả định lập dự toán

Các giả định sau dành cho nguyên mẫu quy mô nhỏ, không phải mức sử dụng đã đo: một thiết bị Edge, tối đa năm người dùng thử, 10.000 request gửi sự kiện và 10.000 request truy vấn lịch sử mỗi tháng, 20.000 lượt gọi Lambda với bộ nhớ 128 MB và thời gian trung bình 200 ms, 1 GB dữ liệu DynamoDB gồm chỉ mục, 0,1 GB lưu trữ S3, 10.000 HTTPS request CloudFront, 1 GB truyền tải frontend, 0,5 GB log CloudWatch và hai cảnh báo tiêu chuẩn. Các lần retry, thao tác chỉ mục và truy vấn phân trang bổ sung phải được tính khi lập dự toán thực tế.

#### Ngân sách hạ tầng

| Dịch vụ / Nhóm chi phí | Mức sử dụng dự kiến hàng tháng | Ngân sách phân bổ đề xuất (USD/tháng) |
| --- | --- | ---: |
| API Gateway và Lambda | 20.000 API request và lượt gọi Lambda | 0,50 |
| DynamoDB | Đọc/ghi on-demand và 1 GB dữ liệu gồm chỉ mục | 1,00 |
| Cognito | Tối đa năm người dùng thử; xác nhận gói dịch vụ | 0,50 |
| S3 và CloudFront | Lưu trữ tĩnh, request và 1 GB truyền tải | 1,00 |
| CloudWatch | Logs, lưu trữ và hai cảnh báo | 1,00 |
| CloudTrail, Budgets và chi phí khác | Kiểm toán tài khoản cơ bản và thông báo ngân sách | 0,50 |
| Dự phòng | Request hoặc truyền tải phát sinh | 0,50 |
| **Tổng ngân sách lập kế hoạch** | | **5,00** |

Đây là mức phân bổ ngân sách đề xuất, **không phải báo giá từ AWS Pricing Calculator hoặc hóa đơn thực tế**. Nếu duy trì cùng ngân sách trong 12 tháng, mức dự trù là 60 USD; Workshop chỉ triển khai nguyên mẫu trong thời gian ngắn. Chi phí phần cứng phát sinh giả định bằng 0 nếu tận dụng máy tính và webcam hiện có. Chưa bao gồm WAF trả phí, phí SMS/MFA, sao lưu tùy chọn, thuế và lưu lượng bổ sung; nếu sử dụng phải tính riêng.

Trước khi nộp, chọn AWS Region thực tế, xác nhận gói dịch vụ và mức sử dụng, tạo dự toán trên [AWS Pricing Calculator](https://calculator.aws/) rồi bổ sung link chia sẻ hoặc PDF xuất từ dự toán. Tham khảo [bảng giá AWS](https://aws.amazon.com/pricing/) để xác minh đơn giá và điều kiện Free Tier. Thiết lập cảnh báo ngân sách (ví dụ 50%, 80% và 100% của ngân sách đề xuất 5 USD), giới hạn thời gian lưu logs và xóa tài nguyên không còn dùng. Cảnh báo AWS Budgets không tự động chặn chi phí phát sinh.

### 7. Đánh giá rủi ro

#### Ma trận rủi ro và biện pháp giảm thiểu

| Rủi ro | Khả năng xảy ra | Mức ảnh hưởng | Biện pháp đề xuất |
| --- | --- | --- | --- |
| Ánh sáng yếu, che khuất mặt hoặc góc đầu bất thường | Trung bình | Cao | Kiểm thử trong điều kiện có kiểm soát; đánh dấu khi theo dõi khuôn mặt không đáng tin cậy |
| Nhận diện nhầm hoặc bỏ sót | Trung bình | Cao | Hiệu chỉnh ngưỡng, sử dụng luật theo thời gian và đo trên mẫu gắn nhãn |
| Kết nối mạng không ổn định | Trung bình | Trung bình | Lưu đệm cục bộ bền vững và retry với thời gian chờ tăng dần |
| Gửi trùng sự kiện | Trung bình | Trung bình | Giữ event ID ổn định và ghi có điều kiện vào DynamoDB |
| Truy cập trái phép hoặc lộ dữ liệu nhạy cảm | Trung bình | Cao | Xác thực nguồn gửi, JWT, kiểm tra quyền sở hữu, IAM, S3 private và tối giản metadata |
| Chi phí AWS ngoài dự kiến | Thấp–Trung bình | Trung bình | Giám sát mức sử dụng, cảnh báo ngân sách, giới hạn lưu trữ và cleanup |
| Chậm tiến độ hoặc tích hợp | Trung bình | Trung bình | Ưu tiên nhận diện Edge, tiếp nhận dữ liệu, lịch sử có phân quyền và demo; hoãn tính năng tùy chọn |

#### Kế hoạch dự phòng

Khi mất mạng, nhận diện tại Edge cần tiếp tục hoạt động và metadata chưa đồng bộ cần được lưu đệm cho đến khi kết nối khôi phục. Nếu một đặc trưng chưa đáng tin cậy, ghi rõ giới hạn và chỉ sử dụng ở mức thử nghiệm. Nếu thành phần cloud tùy chọn không hoàn thành đúng tiến độ, ghi rõ trạng thái và demo luồng chức năng cốt lõi đã được kiểm chứng. Thực hiện kiểm thử trong môi trường an toàn, khi phương tiện đứng yên; không cố tình gây buồn ngủ hoặc mất tập trung khi đang lái xe.

### 8. Kết quả kỳ vọng

#### Sản phẩm kỹ thuật và đánh giá

- Nguyên mẫu Computer Vision chạy cục bộ, trích xuất đặc trưng khuôn mặt và phát hiện sự kiện ứng viên dựa trên luật theo thời gian.
- Luồng đồng bộ chỉ gửi metadata, hỗ trợ lưu đệm cục bộ, retry và xử lý sự kiện trùng.
- Dashboard AWS có xác thực, hiển thị lịch sử sự kiện thông qua backend serverless.
- Tài liệu kiểm thử chuyển động khuôn mặt có kiểm soát, điều kiện ánh sáng/góc đầu, khôi phục mạng, gửi trùng và phân quyền. Báo cáo precision/recall hoặc số trường hợp nhận diện nhầm và bỏ sót nếu đủ dữ liệu gắn nhãn; không khẳng định độ chính xác khi chưa đo.
- Sơ đồ kiến trúc, hướng dẫn Workshop song ngữ, dự toán chi phí, minh chứng giám sát, demo và hướng dẫn dọn dẹp tài nguyên.

#### Giá trị dài hạn

Dự án tạo nền tảng tái sử dụng cho nghiên cứu Edge AI, cải tiến phân tích theo thời gian và phát triển ứng dụng hướng sự kiện có khả năng mở rộng. Gaze tiếp tục là tính năng thử nghiệm cho đến khi được kiểm chứng. Nguyên mẫu hướng đến hỗ trợ nhận thức và nghiên cứu, không thay thế thực hành lái xe an toàn hoặc sản phẩm an toàn ô tô đã được kiểm định.
