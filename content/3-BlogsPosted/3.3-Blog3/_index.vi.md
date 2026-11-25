---
title: "Blog 3"
date: 2026-11-24
weight: 3
chapter: false
pre: "<b> 3.3. </b>"
---

# AMAZON EVENTBRIDGE SCHEDULER – DỊCH VỤ NHỎ NHƯNG MẠNH MẼ CHO CÁC DỰ ÁN AWS

Trong quá trình phát triển ứng dụng, nhiều tác vụ không cần được thực thi ngay lập tức mà cần chạy tại một thời điểm cụ thể hoặc lặp lại theo chu kỳ. Ví dụ như gửi thông báo nhắc lịch hẹn, cập nhật dữ liệu mỗi 30 phút, xóa các bản ghi đã hết hạn hoặc kiểm tra trạng thái hệ thống vào cuối ngày.

Nếu sử dụng một EC2 instance để xử lý những tác vụ này, chúng ta phải duy trì máy chủ, cấu hình môi trường và tự quản lý cơ chế lập lịch. Điều này có thể gây lãng phí tài nguyên khi một tác vụ chỉ mất vài giây để hoàn thành.

Để giải quyết vấn đề này, Amazon Web Services cung cấp **Amazon EventBridge Scheduler** – một dịch vụ lập lịch serverless giúp tự động kích hoạt các dịch vụ AWS tại những thời điểm được định sẵn mà không cần duy trì máy chủ chuyên dụng.

Trong bài viết này, chúng ta sẽ tìm hiểu cách EventBridge Scheduler hoạt động, các kiểu lập lịch, những tính năng nổi bật và cách sử dụng dịch vụ để tự động gọi AWS Lambda trong một dự án thực tế.

### 1. Amazon EventBridge Scheduler là gì?

Amazon EventBridge Scheduler là dịch vụ lập lịch được AWS quản lý, cho phép người dùng tạo và quản lý các tác vụ chạy một lần tại một thời điểm cụ thể hoặc lặp lại theo lịch đã định nghĩa.

Thay vì viết chương trình liên tục kiểm tra thời gian và gọi API, chúng ta chỉ cần tạo một Schedule, xác định thời điểm thực thi và lựa chọn dịch vụ đích (Target). EventBridge Scheduler sẽ tự động gửi yêu cầu đến dịch vụ đó khi lịch đã cấu hình được kích hoạt.

Một số dịch vụ AWS có thể được gọi thông qua EventBridge Scheduler bao gồm:

- **AWS Lambda:** Thực thi các đoạn code xử lý dữ liệu hoặc logic nghiệp vụ.
- **Amazon ECS / AWS Fargate:** Khởi chạy container để thực hiện các tác vụ cần nhiều tài nguyên tính toán hơn.
- **AWS Step Functions:** Kích hoạt một quy trình gồm nhiều bước xử lý.
- **Amazon SNS:** Gửi thông báo đến các subscriber.
- **Amazon SQS:** Đưa message vào hàng đợi để hệ thống xử lý sau.
- **Amazon EventBridge Event Bus:** Phát sự kiện để các thành phần khác trong hệ thống tiếp nhận.

Với Universal Target, EventBridge Scheduler có thể gọi hơn 6.000 API operations thuộc hơn 270 dịch vụ AWS. Điều này giúp Scheduler tích hợp linh hoạt với nhiều thành phần khác nhau trong kiến trúc Cloud.

![Kiến trúc Amazon EventBridge Scheduler](/fcj-workshop-template/images/eventbridge-scheduler-architecture.png)

*Hình 1 - Minh họa cách Amazon EventBridge Scheduler tạo lịch và kích hoạt các dịch vụ đích trên AWS.*

### 2. Các kiểu lập lịch trong EventBridge Scheduler

Một trong những tính năng hữu ích nhất của EventBridge Scheduler là khả năng hỗ trợ nhiều kiểu lập lịch khác nhau, cho phép lập trình viên lựa chọn phương án phù hợp nhất với yêu cầu của ứng dụng.

**2.1. One-time Schedule – Lập lịch một lần**

One-time Schedule kích hoạt tác vụ duy nhất một lần tại ngày và giờ được chỉ định.

Ví dụ:

- Gửi thông báo nhắc lịch hẹn cho người dùng.
- Tự động gửi thông báo sau 24 giờ kể từ khi người dùng đăng ký.
- Kích hoạt tác vụ vào ngày hết hạn của đơn hàng.

Kiểu lập lịch này phù hợp với các tác vụ chỉ cần chạy một lần thay vì lặp lại định kỳ.

**2.2. Rate-based Schedule – Lập lịch theo chu kỳ**

Rate-based Schedule thực thi tác vụ lặp lại theo những khoảng thời gian cố định.

Ví dụ:

```text
rate(30 minutes)
```

Biểu thức này yêu cầu EventBridge Scheduler gọi dịch vụ đích mỗi 30 phút.

Các trường hợp sử dụng phổ biến bao gồm:

- Cập nhật dữ liệu ứng dụng định kỳ.
- Kiểm tra trạng thái hệ thống.
- Tổng hợp dữ liệu theo chu kỳ.
- Tự động đồng bộ thông tin giữa các dịch vụ.

**2.3. Cron-based Schedule – Lập lịch bằng biểu thức Cron**

Cron-based Schedule sử dụng biểu thức Cron để xác định thời điểm thực thi dựa trên phút, giờ, ngày hoặc thứ trong tuần.

Ví dụ:

```text
cron(0 8 ? * MON *)
```

Biểu thức này tương ứng với 8 giờ sáng thứ Hai hằng tuần theo múi giờ được cấu hình cho Schedule.

Một tính năng hữu ích khác là hỗ trợ múi giờ địa phương. Ví dụ:

```text
Asia/Ho_Chi_Minh
```

Nhờ đó, lập trình viên có thể cấu hình lịch theo giờ Việt Nam mà không phải tự chuyển đổi từng thời điểm thực thi sang UTC.

### 3. Các tính năng nổi bật của EventBridge Scheduler

EventBridge Scheduler không chỉ là một công cụ hẹn giờ. Dịch vụ còn cung cấp các cơ chế tích hợp sẵn nhằm nâng cao độ tin cậy của hệ thống và giảm công việc vận hành.

**3.1. Không cần duy trì máy chủ (Serverless)**

Đây là một trong những ưu điểm rõ rệt nhất khi sử dụng EventBridge Scheduler.

Hãy hình dung một ứng dụng cần thực thi tác vụ trong vài giây mỗi 30 phút. Nếu sử dụng EC2, instance có thể vẫn phải chạy liên tục mặc dù phần lớn thời gian không xử lý công việc nào.

Với EventBridge Scheduler, AWS quản lý cơ sở hạ tầng lập lịch bên dưới. Lập trình viên chỉ cần cấu hình Schedule và Target mà không phải vận hành máy chủ chuyên dụng.

Điều này giúp:

- Giảm công việc quản trị máy chủ.
- Loại bỏ nhu cầu viết chương trình liên tục kiểm tra thời gian.
- Giảm tài nguyên hạ tầng nhàn rỗi.
- Mở rộng dễ dàng hơn khi số lượng lịch tăng lên.

**3.2. Retry Policy – Tự động thử lại khi gọi dịch vụ đích thất bại**

Trong các ứng dụng thực tế, dịch vụ đích có thể tạm thời không khả dụng, bị giới hạn tốc độ yêu cầu (throttling) hoặc không thể xử lý yêu cầu.

EventBridge Scheduler hỗ trợ Retry Policy để tự động thử lại các lượt gọi dịch vụ đích thất bại.

Một số giới hạn cấu hình quan trọng bao gồm:

- Thời gian lưu giữ tối đa của sự kiện chưa được xử lý có thể cấu hình lên đến 24 giờ.
- Số lần thử lại tối đa có thể cấu hình lên đến 185 lần.
- Các lần thử lại sử dụng exponential backoff để tăng dần thời gian chờ giữa các lần thử.

Cơ chế này giúp hệ thống có thêm cơ hội hoàn thành tác vụ khi dịch vụ đích gặp sự cố tạm thời.

**3.3. Dead-letter Queue (DLQ)**

Nếu Scheduler vẫn không thể chuyển sự kiện đến dịch vụ đích sau các lần thử lại đã cấu hình, lập trình viên có thể thiết lập một hàng đợi Amazon SQS làm Dead-letter Queue.

Các sự kiện không thể được chuyển thành công sẽ được gửi đến DLQ để tiếp tục điều tra và xử lý.

Ví dụ, tác vụ gửi thông báo có thể thất bại do Lambda Function tạm thời không khả dụng. Thay vì mất dấu sự kiện, thông tin của sự kiện có thể được lưu trong DLQ để nhóm phát triển tìm hiểu nguyên nhân và thực hiện biện pháp xử lý phù hợp.

**3.4. Flexible Time Window – Cửa sổ thời gian linh hoạt**

Hãy hình dung hàng nghìn lịch được cấu hình chạy vào lúc 0 giờ. Nếu tất cả yêu cầu được gửi đến hệ thống đích cùng một thời điểm, khối lượng công việc của hệ thống có thể tăng đột ngột.

Flexible Time Window cho phép AWS phân bổ các lượt gọi trong một khoảng thời gian đã cấu hình.

Ví dụ, nếu chọn cửa sổ thời gian 5 phút, Scheduler có thể gọi dịch vụ đích trong vòng 5 phút sau thời điểm đã lập lịch.

Tính năng này phù hợp với những tác vụ không yêu cầu thực thi tại một thời điểm tuyệt đối chính xác. Nếu tác vụ cần chạy sát với thời điểm đã lập lịch hơn, có thể tắt Flexible Time Window.

**3.5. Hỗ trợ múi giờ và quản lý tập trung**

EventBridge Scheduler cho phép quản lý các lịch thông qua AWS Management Console, AWS CLI và các SDK.

Người dùng cũng có thể lựa chọn múi giờ địa phương khi tạo lịch, chẳng hạn như `Asia/Ho_Chi_Minh`.

Đối với các dự án có nhiều tác vụ tự động, Schedule Groups có thể được sử dụng để tổ chức các lịch theo mục đích:

- `development-schedules`
- `production-schedules`
- `notification-schedules`

Cách tổ chức này giúp việc quản lý và giám sát các lịch trở nên dễ dàng hơn khi dự án phát triển.

### 4. Ví dụ thực tế: Tự động gọi AWS Lambda mỗi 30 phút

Giả sử dự án của chúng ta có tính năng thu thập dữ liệu định kỳ. Mỗi 30 phút, hệ thống cần gọi một Lambda Function để lấy dữ liệu mới và cập nhật cơ sở dữ liệu.

Có hai cách tiếp cận:

**4.1. Cách tiếp cận truyền thống: Sử dụng EC2 và Crontab**

Nếu sử dụng EC2, chúng ta cần thực hiện các công việc sau:

1. Khởi chạy EC2 instance và cấu hình hệ điều hành.
2. Cài đặt môi trường cần thiết, chẳng hạn như Python, Node.js hoặc AWS CLI.
3. Viết script để gọi Lambda Function.
4. Cấu hình `crontab` để thực thi script mỗi 30 phút.
5. Quản lý log, khắc phục sự cố và duy trì máy chủ.
6. Gắn IAM Role cho EC2 để cấp quyền gọi Lambda.

Cách tiếp cận này có thể hoạt động, nhưng yêu cầu duy trì máy chủ liên tục ngay cả khi tác vụ thực tế chỉ mất vài giây.

**4.2. Sử dụng EventBridge Scheduler**

Với EventBridge Scheduler, quy trình trở nên đơn giản hơn nhiều và có thể cấu hình trực tiếp thông qua AWS Management Console.

**Bước 1: Tạo Schedule**

Mở AWS Management Console, truy cập Amazon EventBridge và chọn Scheduler để tạo một Schedule mới.

Chọn Recurring schedule, sau đó chọn Rate-based schedule và nhập:

```text
rate(30 minutes)
```

Nếu tác vụ cần chạy theo giờ Việt Nam, chọn `Asia/Ho_Chi_Minh` trong phần cấu hình Schedule.

**Bước 2: Lựa chọn Target**

Chọn AWS Lambda làm dịch vụ đích và chỉ định Lambda Function cần được gọi.

**Bước 3: Cấu hình Execution Role**

Tạo hoặc lựa chọn một IAM Execution Role cho phép EventBridge Scheduler gọi Lambda Function.

AWS có thể hỗ trợ tạo Role trong quá trình cấu hình. Tuy nhiên, lập trình viên nên xem xét các quyền được cấp và đảm bảo Role chỉ có những quyền cần thiết.

**Bước 4: Cấu hình cơ chế xử lý lỗi**

Tùy thuộc vào yêu cầu của dự án, lập trình viên có thể cấu hình:

- Retry Policy để tự động thử lại các lượt gọi dịch vụ đích thất bại.
- Amazon SQS DLQ để lưu các sự kiện không thể được chuyển thành công.
- Flexible Time Window khi tác vụ không yêu cầu thực thi tại thời điểm lập lịch chính xác.

Sau khi hoàn tất cấu hình, EventBridge Scheduler sẽ tự động gọi Lambda theo lịch đã định nghĩa mà không cần một EC2 instance chuyên dụng.

**4.3. Giám sát quá trình thực thi**

Để xác minh hệ thống có hoạt động đúng hay không, lập trình viên có thể sử dụng:

- **Amazon CloudWatch Metrics:** Theo dõi số lượt gọi, số lượt gọi thất bại và các chỉ số khác liên quan đến Scheduler.
- **AWS Lambda Monitor:** Kiểm tra số lượt gọi, thời gian thực thi và các lỗi Lambda.
- **CloudWatch Logs:** Kiểm tra log được tạo bởi Lambda Function và cơ chế ghi log của Scheduler nếu đã được cấu hình.
- **Amazon SQS DLQ:** Kiểm tra những sự kiện không thể được chuyển thành công đến dịch vụ đích.

Việc giám sát thường xuyên giúp phát hiện sớm sự cố và đảm bảo các tác vụ tự động hoạt động như mong đợi.

### 5. Những lưu ý quan trọng

**5.1. Chi phí và Free Tier**

Theo trang thông tin giá của AWS, EventBridge Scheduler cung cấp tối đa 14 triệu lượt gọi miễn phí mỗi tháng. Sau khi vượt mức sử dụng miễn phí, chi phí được tính dựa trên số lượt gọi và AWS Region đang sử dụng.

Đối với các bài thực hành của sinh viên, dự án học tập và ứng dụng có số lượng tác vụ lập lịch tương đối nhỏ, hạn mức miễn phí này rất hữu ích cho việc thử nghiệm.

Tuy nhiên, lập trình viên cần lưu ý rằng các dịch vụ đích như Lambda, ECS, SQS và CloudWatch có thể phát sinh chi phí riêng. Vì vậy, không nên mặc định toàn bộ quy trình tự động hóa là hoàn toàn miễn phí.

**5.2. So sánh EventBridge Scheduler và EventBridge Rules**

Trước đây, nhiều lập trình viên sử dụng EventBridge Rules để tạo các tác vụ lập lịch. Tuy nhiên, hai tính năng này phục vụ những mục đích khác nhau:

- **EventBridge Scheduler:** Được thiết kế cho các tác vụ lập lịch độc lập. Hỗ trợ One-time, Rate và Cron Schedule, cùng với Retry Policy, DLQ và Flexible Time Window.
- **EventBridge Rules:** Chủ yếu được thiết kế để lọc sự kiện trên Event Bus và định tuyến các sự kiện phù hợp đến dịch vụ đích dựa trên Event Patterns. Rules cũng hỗ trợ Scheduled Rules.

AWS khuyến nghị sử dụng EventBridge Scheduler cho các nhu cầu lập lịch mới vì dịch vụ cung cấp nhiều tính năng lập lịch chuyên biệt hơn và khả năng mở rộng.

**5.3. Các phương pháp tốt nhất về bảo mật và vận hành**

- Chỉ cấp cho IAM Execution Role những quyền tối thiểu cần thiết.
- Tránh tắt Retry Policy và DLQ khi chưa cân nhắc mức độ quan trọng của tác vụ được lập lịch.
- Giám sát CloudWatch Metrics để phát hiện các lượt gọi thất bại.
- Xóa hoặc cấu hình tự động xóa các One-time Schedule đã hoàn thành nếu không còn cần sử dụng.
- Xem xét chi phí của cả Scheduler và những dịch vụ đích mà Scheduler gọi.

### 6. Kết luận

Amazon EventBridge Scheduler là một dịch vụ nhỏ giải quyết một vấn đề rất phổ biến trong phát triển ứng dụng: tự động thực thi các tác vụ theo lịch mà không cần duy trì máy chủ chuyên dụng.

Với khả năng tích hợp nhiều dịch vụ AWS, các kiểu lập lịch khác nhau, Retry Policy, Dead-letter Queue và Flexible Time Window, Scheduler giúp đơn giản hóa việc triển khai các quy trình tự động hóa.

Đối với các dự án sinh viên và ứng dụng serverless, đây là một dịch vụ đáng để tìm hiểu và áp dụng. Thay vì xây dựng một hệ thống lập lịch từ đầu, lập trình viên có thể sử dụng dịch vụ do AWS quản lý để giảm thời gian phát triển và tập trung nhiều hơn vào logic cốt lõi của ứng dụng.

Cảm ơn các bạn đã đọc bài viết! Nếu bạn đã sử dụng EventBridge Scheduler trong các dự án của mình hoặc có kinh nghiệm tối ưu hóa tác vụ lập lịch trên AWS, hãy chia sẻ suy nghĩ và những kinh nghiệm thực tế của bạn!

### 7. Tài liệu tham khảo

1. **Tài liệu AWS – Amazon EventBridge Scheduler là gì?**
   https://docs.aws.amazon.com/scheduler/latest/UserGuide/what-is-scheduler.html

2. **Tài liệu AWS – Các kiểu lập lịch trong EventBridge Scheduler**
   https://docs.aws.amazon.com/scheduler/latest/UserGuide/schedule-types.html

3. **Tài liệu AWS – Bắt đầu với EventBridge Scheduler**
   https://docs.aws.amazon.com/scheduler/latest/UserGuide/getting-started.html

4. **Tài liệu AWS – Cấu hình Flexible Time Window**
   https://docs.aws.amazon.com/scheduler/latest/UserGuide/managing-schedule-flexible-time-windows.html

5. **Tài liệu AWS – Quản lý Schedule**
   https://docs.aws.amazon.com/scheduler/latest/UserGuide/managing-schedule.html

6. **Bảng giá AWS – Amazon EventBridge**
   https://aws.amazon.com/eventbridge/pricing/
