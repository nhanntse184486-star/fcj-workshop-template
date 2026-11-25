---
title: "Blog 3"
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

## Amazon EventBridge Scheduler: Tự động hóa tác vụ định kỳ với AWS Lambda

### 1. Tại sao cần dịch vụ lập lịch?

Một ứng dụng thường có tác vụ định kỳ như tổng hợp báo cáo, kiểm tra dữ liệu hoặc gửi thông báo. Duy trì một máy chủ chỉ để chờ đến thời điểm chạy tác vụ tạo thêm công việc vận hành. EventBridge Scheduler cung cấp cách lập lịch được AWS quản lý và kích hoạt target đã cấu hình.

### 2. Các kiểu lập lịch

| Kiểu | Ví dụ | Mục đích |
|---|---|---|
| Rate-based | `rate(30 minutes)` | Lặp theo khoảng thời gian |
| Cron-based | `cron(0 8 * * ? *)` | Chạy lúc 08:00 mỗi ngày theo múi giờ được cấu hình |
| One-time | `at(2026-12-01T08:00:00)` | Chạy một lần tại thời điểm xác định |

Scheduler hỗ trợ múi giờ cho lịch cron và lịch một lần. Độ chính xác của lịch là ở mức phút, không thích hợp với yêu cầu điều khiển thời gian thực theo giây.

### 3. Các thành phần và độ tin cậy

Luồng tham khảo: `Schedule → Execution role → Lambda → CloudWatch Logs`.

Execution role cho phép Scheduler gọi target cụ thể. Retry Policy quy định số lần thử lại và thời gian tối đa giữ sự kiện chưa gửi thành công. Dead-letter queue (DLQ) bằng Amazon SQS có thể lưu sự kiện không gửi được sau khi hết điều kiện retry. Flexible Time Window hữu ích khi tác vụ có thể chạy trong một khoảng thời gian thay vì đúng một phút cố định.

Scheduler có mô hình giao ít nhất một lần, vì vậy tác vụ xử lý cần có tính idempotent nếu việc thực thi trùng có thể gây tác dụng phụ.

### 4. Thực hành: gọi Lambda mỗi 30 phút

Tạo một Lambda với Python runtime được hỗ trợ, sử dụng handler đơn giản:

```python
import json
from datetime import datetime, timezone

def lambda_handler(event, context):
    result = {
        "task": "scheduled-check",
        "timestamp": datetime.now(timezone.utc).isoformat()
    }
    print(json.dumps(result))
    return result
```

1. Trong Lambda Console, tạo hàm `scheduled-check`, cấp quyền ghi CloudWatch Logs.
2. Mở EventBridge Scheduler, chọn Create schedule và đặt tên `check-every-30-minutes`.
3. Chọn lịch lặp Rate-based, nhập `rate(30 minutes)` và đặt Flexible time window thành Off nếu cần lịch cố định.
4. Chọn AWS Lambda làm target, chọn hàm vừa tạo và cấu hình execution role có quyền `lambda:InvokeFunction` đối với hàm đó.
5. Cấu hình retry phù hợp, có thể thêm SQS DLQ, bật schedule và lưu.
6. Kiểm tra log của Lambda trong CloudWatch sau lần kích hoạt. Khi kết thúc thử nghiệm, xóa schedule, hàm và các tài nguyên riêng của bài lab nếu không còn sử dụng.

### 5. Các tình huống kiểm thử

| Tình huống | Kết quả mong đợi |
|---|---|
| Lịch được bật và quyền hợp lệ | Target nhận yêu cầu theo lịch |
| Thiếu quyền gọi Lambda | Scheduler ghi nhận lỗi gửi yêu cầu |
| Hết điều kiện retry và đã cấu hình DLQ | Sự kiện gửi thất bại được đưa vào DLQ nếu quyền hợp lệ |
| Lambda nhận sự kiện trùng | Logic nghiệp vụ không tạo tác dụng phụ trùng lặp |

Lỗi phát sinh bên trong hàm Lambda sau khi target đã chấp nhận yêu cầu bất đồng bộ cần được theo dõi và xử lý riêng ở tầng Lambda.

### 6. Liên hệ với đề tài thực tập

Scheduler có thể hỗ trợ tổng hợp số lượng sự kiện buồn ngủ và mất tập trung theo ngày hoặc tạo báo cáo định kỳ từ metadata DynamoDB. Đây là một khả năng mở rộng cho hệ thống Driver Drowsiness & Distraction Detection, không thay thế cơ chế cảnh báo trực tiếp tại Edge và không phải thành phần bắt buộc trong kiến trúc hiện tại.

### 7. Kết luận

EventBridge Scheduler phù hợp với các tác vụ nền có lịch chạy rõ ràng. Việc giới hạn quyền, xử lý trùng lặp, giám sát lỗi và xóa lịch không còn sử dụng giúp hệ thống dễ vận hành hơn.

### Tài liệu tham khảo

- [EventBridge Scheduler](https://docs.aws.amazon.com/scheduler/latest/UserGuide/what-is-scheduler.html)
- [Schedule types](https://docs.aws.amazon.com/scheduler/latest/UserGuide/schedule-types.html)
- [AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)
- [Amazon SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html)
