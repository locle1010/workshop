---
title: "Worklog Tuần 3"
date: 2026-04-26
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---
### Mục tiêu tuần 3:

* Cấu hình tự động hóa sao lưu dữ liệu với AWS Backup và đồng bộ hóa local storage lên Amazon S3.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Nghiên cứu tài liệu tổng quan dịch vụ AWS Backup, cơ chế thiết lập Backup Plans và Vaults lưu trữ tập trung. | 04/05/2026 | 04/05/2026 | <https://000013.awsstudygroup.com> |
| 3 | Thiết lập backup plan tự động hóa định kỳ cho các tài nguyên EBS, RDS, DynamoDB và thực hiện test tính năng phục hồi dữ liệu. | 05/05/2026 | 05/05/2026 | <https://000013.awsstudygroup.com/3-createbackupplan/> |
| 4 | Nghiên cứu dịch vụ Amazon S3: Đối tượng lưu trữ, phân cấp Storage Classes và định cấu hình Lifecycle Policies. | 06/05/2026 | 06/05/2026 | <https://000057.awsstudygroup.com> |
| 5 | Triển khai hạ tầng AWS Storage Gateway (File Gateway) tại môi trường local để tạo cầu nối dữ liệu. | 07/05/2026 | 07/05/2026 | <https://000024.awsstudygroup.com> |
| 6 | Setup File Shares và cấu hình kết nối đồng bộ hóa tự động dữ liệu từ các máy trạm On-premise lên S3 bucket. | 08/05/2026 | 08/05/2026 | <https://000024.awsstudygroup.com/2-useawsstoragegw/2.2-createfileshare/> |

### Kết quả đạt được tuần 3:

* **Kết quả chung:** Vận hành hệ thống backup tự động hóa đa tài nguyên an toàn và đồng bộ hóa thành công kho lưu trữ cục bộ lên Cloud S3.
* **Đánh giá tuần:** Hoàn thành tốt nhiệm vụ. Nắm vững tư duy giải pháp hybrid storage và cơ chế sao lưu phục hồi thảm họa.
* **Chi tiết kết quả thực hiện:**
  * **Ngày 04/05/2026:** Nắm vững quy trình quản lý vòng đời sao lưu tập trung.
  * **Ngày 05/05/2026:** Hệ thống tự động backup hoạt động tốt, khôi phục dữ liệu (Restore) thành công không lỗi.
  * **Ngày 06/05/2026:** Tối ưu hóa được chi phí lưu trữ dài hạn dựa trên tần suất truy cập.
  * **Ngày 07/05/2026:** File Gateway kết nối thành công tới điểm cuối Cloud.
  * **Ngày 08/05/2026:** Hệ thống local tự động đồng bộ dữ liệu an toàn lên các S3 bucket.
