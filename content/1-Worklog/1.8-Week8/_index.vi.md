---
title: "Worklog Tuần 8"
date: 2026-11-09
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

* Hoàn thiện luồng phân tích trạng thái và định dạng metadata sự kiện.
* Tích hợp HTTP API với Lambda Event Writer để tiếp nhận metadata.
* Tích hợp Lambda Event Reader để truy vấn lịch sử sự kiện.

### Nội dung công việc theo tuần:

| STT | Công việc | Tài liệu tham khảo |
| --- | --- | --- |
| 1 | Hoàn thiện luồng phân tích trạng thái và định dạng metadata sự kiện. | Proposal – Driver Drowsiness & Distraction Detection |
| 2 | Xử lý event_id, Local Event Buffer và cơ chế retry. | Proposal – Driver Drowsiness & Distraction Detection |
| 3 | Tích hợp HTTP API với Lambda Event Writer để tiếp nhận metadata. | <https://docs.aws.amazon.com/lambda/> |
| 4 | Tích hợp DynamoDB và cơ chế Conditional PutItem chống ghi trùng. | <https://docs.aws.amazon.com/amazondynamodb/> |
| 5 | Tích hợp Lambda Event Reader để truy vấn lịch sử sự kiện. | <https://docs.aws.amazon.com/amazondynamodb/> |

### Kết quả đạt được:

* Hoàn thành tích hợp giữa pipeline Edge AI và backend AWS để truyền và xử lý metadata sự kiện.
* Kiến trúc backend sử dụng API Gateway HTTP API, Lambda Event Writer, Lambda Event Reader và DynamoDB để tiếp nhận, lưu trữ và truy vấn lịch sử sự kiện.
* Thiết kế cơ chế định danh sự kiện, ghi có điều kiện và đồng bộ dữ liệu nhằm hỗ trợ xử lý sự kiện trùng và gián đoạn kết nối.
