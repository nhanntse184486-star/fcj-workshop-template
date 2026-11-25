---
title: "Worklog Tuần 6"
date: 2026-10-26
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Phân tích dấu hiệu buồn ngủ, mất tập trung và phạm vi hệ thống.
* Thiết kế pipeline Webcam, OpenCV, MediaPipe và Temporal Analysis.
* Xác định yêu cầu xác thực, quyền riêng tư, giám sát và đồng bộ sự kiện.

### Nội dung công việc theo tuần:

| STT | Công việc | Tài liệu tham khảo |
| --- | --- | --- |
| 1 | Phân tích dấu hiệu buồn ngủ, mất tập trung và phạm vi hệ thống. | Proposal – Driver Drowsiness & Distraction Detection |
| 2 | Lựa chọn xử lý video tại Edge, chỉ gửi metadata lên cloud. | Proposal – Driver Drowsiness & Distraction Detection |
| 3 | Thiết kế pipeline Webcam, OpenCV, MediaPipe và Temporal Analysis. | Proposal – Driver Drowsiness & Distraction Detection |
| 4 | Thiết kế HTTP API, Lambda Writer/Reader, DynamoDB và Dashboard. | <https://docs.aws.amazon.com/apigateway/> |
| 5 | Xác định yêu cầu xác thực, quyền riêng tư, giám sát và đồng bộ sự kiện. | <https://docs.aws.amazon.com/wellarchitected/> |

### Kết quả đạt được:

* Xác định phạm vi hệ thống phát hiện buồn ngủ và mất tập trung dựa trên các đặc trưng khuôn mặt.
* Xây dựng kiến trúc Edge AI kết hợp AWS Cloud, trong đó hình ảnh được xử lý tại thiết bị và chỉ metadata sự kiện được truyền lên cloud.
* Xác định luồng dữ liệu, các dịch vụ AWS và yêu cầu về xác thực, quyền riêng tư, giám sát và đồng bộ sự kiện.
