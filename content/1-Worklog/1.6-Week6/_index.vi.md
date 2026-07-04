---
title: "Worklog Tuần 6"
date: 2026-04-26
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---
### Mục tiêu tuần 6:

* Triển khai hệ thống cơ sở dữ liệu Managed DB (RDS/Aurora), kho phân tích Redshift và giải pháp bộ nhớ đệm Cache.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Đánh giá lại các mô hình thiết kế DB quan hệ và nghiên cứu dịch vụ quản trị Managed Database đám mây. | 25/05/2026 | 25/05/2026 | <https://000005.awsstudygroup.com> |
| 3 | Làm lab cấu hình chuyên sâu Amazon RDS và AWS Aurora, thiết lập cụm Multi-AZ nâng cao tính sẵn sàng cao. | 26/05/2026 | 26/05/2026 | <https://000005.awsstudygroup.com/4-create-rds/> |
| 4 | Cấu hình tính năng sao lưu tự động (Automated Backups) cho DB cluster và cài đặt ngưỡng cảnh báo qua CloudWatch. | 27/05/2026 | 27/05/2026 | <https://000008.awsstudygroup.com> |
| 5 | Nghiên cứu kiến trúc kho lưu trữ dữ liệu Amazon Redshift đáp ứng các bài toán phân tích tổng hợp dữ liệu lớn (Big Data). | 28/05/2026 | 28/05/2026 | <https://ap-southeast-1.console.aws.amazon.com/redshiftv2> |
| 6 | Tìm hiểu giải pháp in-memory caching bằng Amazon ElastiCache (Redis) tăng tốc phản hồi, giảm tải cho DB lõi. | 29/05/2026 | 29/05/2026 | <https://000061.awsstudygroup.com> |

### Kết quả đạt được tuần 6:

* **Kết quả chung:** Triển khai thành công hệ quản trị database chịu lỗi cao trên RDS/Aurora và làm chủ giải pháp tối ưu hiệu năng ElastiCache.
* **Đánh giá tuần:** Hoàn thành tốt. Đã nắm vững sự khác biệt giữa DB truyền thống và Managed DB trên Cloud, cấu hình thành công Multi-AZ.
* **Chi tiết kết quả thực hiện:**
  * **Ngày 25/05/2026:** Xác định được kiến trúc DB tối ưu chịu tải tốt trên Cloud.
  * **Ngày 26/05/2026:** Database cluster vận hành ổn định, có năng lực tự động failover chịu lỗi.
  * **Ngày 27/05/2026:** Mọi biến động tài nguyên DB được giám sát real-time chặt chẽ.
  * **Ngày 28/05/2026:** Nắm vững cơ chế Columnar Storage phục vụ truy vấn báo cáo lớn.
  * **Ngày 29/05/2026:** Thiết lập thành công tầng đệm cache giúp tối ưu tốc độ phản hồi ứng dụng.
