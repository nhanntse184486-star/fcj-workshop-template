---
title: "Sự kiện 2 – Cloud Mastery 1"
date: 2026-11-14
weight: 2
chapter: false
draft: false
pre: "<b> 4.2. </b>"
---

# Cloud Mastery 1

## 1. Thông tin sự kiện

| Thông tin | Chi tiết |
|---|---|
| Tên sự kiện | Cloud Mastery 1 |
| Ngày tổ chức | Thứ Bảy, 14/11/2026 |
| Địa điểm | Tầng 26, Tòa nhà Bitexco, TP. Hồ Chí Minh, Việt Nam |
| Hình thức | Chia sẻ kiến thức trực tiếp |
| Vai trò | Người tham dự |

## 2. Mục tiêu sự kiện

Cloud Mastery 1 mang đến cơ hội tiếp cận kiến thức điện toán đám mây thông qua các ví dụ thực tế về phát triển ứng dụng AI. Chương trình giúp sinh viên hiểu cách lựa chọn dịch vụ AWS, cân nhắc các yêu cầu vận hành và nhận diện những yếu tố tạo nên một hệ thống đáng tin cậy. Đây cũng là dịp kết nối với những người cùng quan tâm đến Cloud Computing và AI.

## 3. Nội dung và hoạt động nổi bật

### 3.1. Vai trò của Cloud trong ứng dụng AI

Phần trình bày **The Role of Cloud** sử dụng ví dụ chuỗi nhà hàng để minh họa cách hạ tầng đám mây hỗ trợ ứng dụng AI. Bốn khía cạnh được nhấn mạnh trong slide gồm:

| Khía cạnh | Ý nghĩa |
|---|---|
| Security – Bảo mật | Bảo vệ dữ liệu, kiểm soát quyền truy cập và giảm rủi ro trong quá trình vận hành. |
| Scalability – Khả năng mở rộng | Điều chỉnh tài nguyên theo nhu cầu xử lý và lưu lượng thực tế. |
| Reliability – Độ tin cậy | Hướng đến khả năng duy trì hoạt động khi một thành phần gặp sự cố. |
| Cost – Chi phí | Sử dụng tài nguyên theo nhu cầu, hạn chế lãng phí và theo dõi chi phí. |

Các yếu tố này cần được xem xét đồng thời khi xây dựng ứng dụng, thay vì chỉ chú trọng vào độ chính xác của mô hình AI.

### 3.2. AWS Toolbox – Các dịch vụ hỗ trợ phát triển AI

| Dịch vụ | Vai trò được giới thiệu |
|---|---|
| AWS Lambda | Thực thi mã theo sự kiện mà không cần tự quản lý máy chủ. |
| Amazon Bedrock | Truy cập các foundation model thông qua API để xây dựng ứng dụng AI. |
| Amazon Bedrock AgentCore | Cung cấp các năng lực vận hành AI agent, bao gồm môi trường thực thi, bộ nhớ và quản lý danh tính. |

Cách phân chia vai trò giữa các dịch vụ cho thấy một ứng dụng AI có thể kết hợp nhiều thành phần chuyên biệt, thay vì đặt toàn bộ trách nhiệm xử lý vào một mô hình duy nhất.

### 3.3. Use AI Wisely – Sử dụng AI hợp lý

Phần chia sẻ sử dụng hình ảnh bếp trưởng điều phối các bộ phận trong nhà hàng để mô tả kiến trúc AI. Một mô hình lớn có thể đảm nhiệm việc điều phối, trong khi các mô hình nhỏ hơn thực hiện những nhiệm vụ chuyên biệt. Hai nguyên tắc nổi bật là lựa chọn mô hình phù hợp với từng nhiệm vụ và tận dụng bộ nhớ đệm đối với những câu hỏi lặp lại khi điều kiện cho phép.

Cách tiếp cận này giúp cân nhắc chất lượng đầu ra, độ trễ và chi phí thay vì mặc định sử dụng mô hình lớn nhất cho mọi yêu cầu.

### 3.4. Tình huống thực tế: Cox Automotive

Ví dụ Cox Automotive đặt ra bài toán xây dựng giải pháp AI tự động hóa mà người dùng có thể tin tưởng. Slide giới thiệu hướng chuẩn hóa nền tảng trước khi mở rộng triển khai các agent:

| Thành phần | Vai trò trong tình huống được trình bày |
|---|---|
| AgentCore Runtime | Môi trường thực thi thống nhất, hỗ trợ triển khai, giám sát và xử lý lỗi. |
| AgentCore Identity | Phân quyền chi tiết theo vai trò của từng agent. |
| Amazon Bedrock Guardrails | Kiểm soát đầu vào và đầu ra nhằm giảm rủi ro từ nội dung không phù hợp. |
| AgentCore Memory | Duy trì ngữ cảnh qua các phiên tương tác. |
| AgentCore Observability | Ghi nhận tương tác và hỗ trợ phân tích lỗi trong quy trình nhiều bước. |

Điểm đáng chú ý là AI agent không chỉ cần năng lực xử lý thông tin mà còn cần nền tảng quản trị, bảo mật và giám sát phù hợp.

## 4. Kiến thức và bài học rút ra

- Thiết kế ứng dụng AI cần xuất phát từ yêu cầu thực tế của người dùng và điều kiện vận hành.
- Bảo mật, khả năng mở rộng, độ tin cậy và chi phí là những tiêu chí cần cân bằng khi lựa chọn kiến trúc Cloud.
- Lựa chọn mô hình theo độ phức tạp của nhiệm vụ giúp tránh sử dụng tài nguyên không cần thiết.
- Phân quyền, kiểm soát dữ liệu và khả năng quan sát hệ thống có vai trò quan trọng trong việc xây dựng ứng dụng AI đáng tin cậy.
- Những ví dụ thực tế tại sự kiện giúp kết nối kiến thức lý thuyết với các quyết định thiết kế phần mềm.

## 5. Liên hệ với dự án thực tập

Đối với đề tài **Driver Drowsiness & Distraction Detection**, nội dung sự kiện gợi mở các hướng vận dụng sau:

| Bài học | Liên hệ với dự án |
|---|---|
| Lựa chọn phương thức xử lý phù hợp | Ưu tiên xử lý hình ảnh tại thiết bị bằng OpenCV và MediaPipe; chỉ truyền metadata sự kiện cần thiết lên Cloud. |
| Kiến trúc theo sự kiện | Sử dụng API Gateway và Lambda để tiếp nhận, xử lý các sự kiện phát hiện buồn ngủ hoặc mất tập trung. |
| Độ tin cậy | Chú trọng cơ chế bộ đệm, retry và định danh sự kiện để hạn chế mất hoặc ghi trùng dữ liệu. |
| Bảo mật | Giới hạn quyền IAM, xác thực người dùng Dashboard bằng Cognito và bảo vệ dữ liệu hình ảnh cá nhân. |
| Chi phí và giám sát | Ưu tiên dịch vụ serverless phù hợp với nhu cầu, theo dõi lỗi và chỉ số vận hành bằng CloudWatch. |

Các nội dung về Bedrock và AgentCore mở rộng góc nhìn về phát triển AI trên AWS, nhưng không phải là thành phần bắt buộc của hệ thống phát hiện buồn ngủ hiện tại.

## 6. Tổng kết

Cloud Mastery 1 là cơ hội để tôi tiếp cận cách AWS hỗ trợ xây dựng và vận hành ứng dụng AI từ góc nhìn kiến trúc. Việc tham dự cùng bạn học giúp tăng cơ hội trao đổi về công nghệ và định hướng phát triển kỹ năng. Bài học nổi bật của sự kiện là lựa chọn công nghệ theo mục tiêu của hệ thống, kết hợp hiệu quả xử lý với bảo mật, độ tin cậy và chi phí vận hành.

## 7. Hình ảnh tham gia sự kiện

![Ảnh check-in cùng bạn tại Cloud Mastery 1](/fcj-workshop-template/images/4-EventParticipated/Event2/check-in.png)

*Hình 1. Ảnh check-in cùng bạn tại Cloud Mastery 1.*

![Phần trình bày The Role of Cloud: bảo mật, khả năng mở rộng, độ tin cậy và chi phí](/fcj-workshop-template/images/4-EventParticipated/Event2/role-of-cloud.png)

*Hình 2. Phần trình bày The Role of Cloud: bảo mật, khả năng mở rộng, độ tin cậy và chi phí.*

![Slide Use AI Wisely: lựa chọn mô hình phù hợp với nhiệm vụ](/fcj-workshop-template/images/4-EventParticipated/Event2/use-ai-wisely.png)

*Hình 3. Slide Use AI Wisely: lựa chọn mô hình phù hợp với nhiệm vụ.*

![Slide AWS Toolbox: Lambda, Bedrock và AgentCore](/fcj-workshop-template/images/4-EventParticipated/Event2/aws-toolbox.png)

*Hình 4. Slide AWS Toolbox: Lambda, Bedrock và AgentCore.*

![Tình huống thực tế Cox Automotive và bài toán xây dựng AI đáng tin cậy](/fcj-workshop-template/images/4-EventParticipated/Event2/cox-automotive.png)

*Hình 5. Tình huống thực tế Cox Automotive và bài toán xây dựng AI đáng tin cậy.*

![Cách Cox Automotive chuẩn hóa nền tảng vận hành AI agent với AgentCore](/fcj-workshop-template/images/4-EventParticipated/Event2/agentcore.png)

*Hình 6. Cách Cox Automotive chuẩn hóa nền tảng vận hành AI agent với AgentCore.*

