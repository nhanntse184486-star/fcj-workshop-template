---
title: "Worklog Tuần 7"
date: 2026-11-02
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* Chuẩn bị Python, webcam và OpenCV để xử lý khung hình.
* Xây dựng đặc trưng EAR và MAR cho nhắm mắt và ngáp.
* Phân tích đặc trưng theo thời gian, sử dụng CSV và quy tắc nhận diện.

### Nội dung công việc theo tuần:

| STT | Công việc | Tài liệu tham khảo |
| --- | --- | --- |
| 1 | Chuẩn bị Python, webcam và OpenCV để xử lý khung hình. | <https://docs.opencv.org/4.x/> |
| 2 | Tích hợp MediaPipe Face Mesh và trích xuất landmarks. | <https://ai.google.dev/edge/mediapipe/solutions/vision/face_landmarker> |
| 3 | Xây dựng đặc trưng EAR và MAR cho nhắm mắt và ngáp. | Proposal – Driver Drowsiness & Distraction Detection |
| 4 | Xử lý Head Pose và khảo sát Gaze Direction. | Proposal – Driver Drowsiness & Distraction Detection |
| 5 | Phân tích đặc trưng theo thời gian, sử dụng CSV và quy tắc nhận diện. | Proposal – Driver Drowsiness & Distraction Detection |

### Kết quả đạt được:

* Xây dựng pipeline xử lý hình ảnh từ webcam bằng OpenCV và MediaPipe Face Mesh với 478 điểm đặc trưng khuôn mặt.
* Trích xuất EAR, MAR và Head Pose phục vụ phân tích trạng thái tài xế; nghiên cứu Gaze Direction như một đặc trưng thử nghiệm.
* Xây dựng hướng tiếp cận phân tích theo thời gian dựa trên dữ liệu CSV và các quy tắc nhận diện dấu hiệu bất thường.
