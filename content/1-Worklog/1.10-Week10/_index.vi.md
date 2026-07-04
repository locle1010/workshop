---
title: "Worklog Tuần 10"
date: 2026-04-26
weight: 2
chapter: false
pre: " <b> 1.10. </b> "
---
### Mục tiêu tuần 10:

* Triển khai cụm mã nguồn Backend lên AWS, cấu hình kết nối an toàn với cơ sở dữ liệu NoSQL DynamoDB.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Kiểm tra chuẩn hóa mã nguồn Backend local, dọn dẹp biến môi trường và đóng gói bộ cài đặt ứng dụng. | 22/06/2026 | 22/06/2026 |  |
| 3 | Khởi tạo môi trường DB NoSQL trên cloud, tạo các bảng dữ liệu (Tables) và cấu hình Partition/Sort Key trên Amazon DynamoDB. | 23/06/2026 | 23/06/2026 |  |
| 4 | Thiết lập IAM Roles và Security Policies phân quyền hạn tối thiểu (Least Privilege) cho dịch vụ Backend tương tác DB. | 24/06/2026 | 24/06/2026 |  |
| 5 | Tiến hành deploy cấu hình đưa dịch vụ mã nguồn Backend lên chạy trực tuyến thành công trên nền tảng AWS Cloud. | 25/06/2026 | 25/06/2026 |  |
| 6 | Sử dụng công cụ Postman thực hiện chạy bộ test kiểm thử API Endpoint, xác thực dữ liệu đọc ghi vào DynamoDB mượt mà. | 26/06/2026 | 26/06/2026 |  |

### Kết quả đạt được tuần 10:

* **Kết quả chung:** Đưa thành công dịch vụ xử lý Backend lên môi trường trực tuyến AWS và cấu hình kết nối an toàn tuyệt đối tới Amazon DynamoDB.
* **Đánh giá tuần:** Hoàn thành xuất sắc mục tiêu tuần. Phân hệ Backend deploy mượt mà, kiểm soát tốt bảo mật kết nối IAM tới Database NoSQL.
* **Chi tiết kết quả thực hiện:**
  * **Ngày 22/06/2026:** Mã nguồn backend tối ưu hóa cấu trúc, không có lỗi runtime local.
  * **Ngày 23/06/2026:** Cơ sở dữ liệu DynamoDB được cấu hình đúng chuẩn schema thiết kế.
  * **Ngày 24/06/2026:** Loại bỏ hoàn toàn nguy cơ lộ thông tin quản trị (Credential) kết nối DB.
  * **Ngày 25/06/2026:** Backend dịch vụ hoạt động trực tuyến ổn định trên môi trường đám mây.
  * **Ngày 26/06/2026:** 100% các API Endpoint phản hồi chính xác cấu trúc dữ liệu JSON yêu cầu.
