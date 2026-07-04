---
title: "Worklog Tuần 11"
date: 2026-04-26
weight: 2
chapter: false
pre: " <b> 1.11. </b> "
---
### Mục tiêu tuần 11:

* Triển khai ứng dụng Frontend tĩnh lên Amazon S3, kích hoạt mạng phân phối CDN CloudFront và định tuyến Route 53.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Đóng gói mã nguồn Frontend thành bộ resource tĩnh; Khởi tạo Amazon S3 Bucket và kích hoạt Static Website Hosting. | 29/06/2026 | 29/06/2026 |  |
| 3 | Đẩy toàn bộ source code frontend lên S3, phân định Bucket Policy cấp quyền truy cập đọc (Read) hợp lý cho client công cộng. | 30/06/2026 | 30/06/2026 |  |
| 4 | Cấu hình AWS CloudFront Distribution, trỏ nguồn (Origin) về S3 Bucket đóng vai trò CDN tăng tốc độ tải trang toàn cầu. | 01/07/2026 | 01/07/2026 |  |
| 5 | Yêu cầu chứng chỉ SSL/TLS miễn phí thông qua AWS Certificate Manager (ACM) và cấu hình mã hóa bảo mật HTTPS. | 02/07/2026 | 02/07/2026 |  |
| 6 | Cấu hình hệ thống Route 53 quản lý DNS, tạo Alias Records định tuyến tên miền riêng chính thức trỏ về mạng CloudFront. | 03/07/2026 | 03/07/2026 |  |

### Kết quả đạt được tuần 11:

* **Kết quả chung:** Hoàn thiện deploy toàn vẹn Frontend lên bộ đôi S3/CloudFront, kích hoạt HTTPS bảo mật và cấu hình thông suốt DNS qua Route 53.
* **Đánh giá tuần:** Hoàn thành tốt mục tiêu. Website chạy hoàn chỉnh public, nắm vững kiến thức tối ưu hóa CDN và định tuyến domain thực tế.
* **Chi tiết kết quả thực hiện:**
  * **Ngày 29/06/2026:** Frontend được lưu trữ tối ưu hóa chi phí vận hành trên S3.
  * **Ngày 30/06/2026:** Giao diện website hiển thị thành công qua đường dẫn mặc định S3.
  * **Ngày 01/07/2026:** Mạng lưới CDN hoạt động ổn định, độ trễ tải trang giảm thiểu tối đa.
  * **Ngày 02/07/2026:** Trang web hiển thị ổ khóa xanh bảo mật an toàn thông tin đường truyền.
  * **Ngày 03/07/2026:** Người dùng truy cập được ứng dụng hoàn chỉnh qua Tên miền riêng dễ nhớ.
