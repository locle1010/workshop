---
title: "Worklog Tuần 4"
date: 2026-04-26
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---
### Mục tiêu tuần 4:

* Vận hành máy chủ ảo Amazon EC2, thiết lập tường lửa Security Group và triển khai Web Server.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Nghiên cứu dịch vụ Amazon EC2, tìm hiểu quy chuẩn phân loại Instance Types và cách lựa chọn AMI tối ưu. | 11/05/2026 | 11/05/2026 | <https://000004.awsstudygroup.com> |
| 3 | Thực hành quy trình khởi chạy các máy chủ EC2 hoạt động trên nền hệ điều hành Ubuntu và Amazon Linux 2. | 12/05/2026 | 12/05/2026 | <https://000004.awsstudygroup.com/4-launchlinuxinstance/> |
| 4 | Tạo cặp khóa xác thực Key Pairs bảo mật cao và cấu hình tường lửa Security Group kiểm soát traffic Inbound/Outbound. | 13/05/2026 | 13/05/2026 | <https://000004.awsstudygroup.com/5-amazonec2basic/5.3-createcustomami/> |
| 5 | Sử dụng giao thức SSH từ máy cá nhân truy cập từ xa vào hệ điều hành máy chủ ảo để kiểm tra tính ổn định. | 14/05/2026 | 14/05/2026 | <https://000004.awsstudygroup.com/5-amazonec2basic/5.7-ubuntu/> |
| 6 | Triển khai cài đặt và cấu hình phần mềm Web Server (Nginx) trên instance, chạy thử nghiệm hiển thị nội dung web công khai. | 15/05/2026 | 15/05/2026 | <https://000004.awsstudygroup.com/6-awsfcjmanagement-linux/> |

### Kết quả đạt được tuần 4:

* **Kết quả chung:** Khởi chạy thành công cụm máy chủ ảo EC2, hoàn thiện thắt chặt cấu hình bảo mật và deploy ổn định dịch vụ Web Server.
* **Đánh giá tuần:** Đạt yêu cầu đề ra. Hiểu rõ quy trình thao tác cấu hình hệ điều hành Linux và quản lý port an toàn bảo mật trên EC2.
* **Chi tiết kết quả thực hiện:**
  * **Ngày 11/05/2026:** Hiểu cách chọn cấu hình phần cứng ảo hóa phù hợp bài toán chi phí.
  * **Ngày 12/05/2026:** Các instance máy chủ được kích hoạt trạng thái running bình thường.
  * **Ngày 13/05/2026:** Chặn hoàn toàn các kết nối lạ trái phép, chỉ mở port dịch vụ quy định.
  * **Ngày 14/05/2026:** Quyền làm chủ root server thông suốt qua SSH key pair.
  * **Ngày 15/05/2026:** Trang web demo chạy mượt mà trực tuyến qua địa chỉ IP công cộng.
