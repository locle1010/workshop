---
title: "Bản đề xuất"
date: 2026-07-04
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# AI Assistant — Triển khai ứng dụng AI lên AWS Cloud
## Giải pháp AWS Serverless cho trợ lý ảo tích hợp AI

---

### 1. Tóm tắt

**AI Assistant** là ứng dụng trợ lý ảo kết hợp Desktop App (Unity) và Web App (React), cho phép người dùng chat với AI (Google Gemini), quản lý hồ sơ và đồng bộ dữ liệu giữa các nền tảng.

Dự án này triển khai toàn bộ hệ thống lên **AWS Cloud** theo kiến trúc **Serverless** hiện đại — không cần quản lý server, tự động mở rộng quy mô và chỉ trả tiền theo lượng sử dụng thực tế.

---

### 2. Vấn đề & Giải pháp

#### Vấn đề hiện tại

Ứng dụng AI Assistant hiện tại chỉ chạy cục bộ trên máy tính (Unity Desktop App), chưa có:
+ Backend API để xử lý logic và lưu trữ dữ liệu tập trung
+ Hệ thống xác thực người dùng an toàn
+ Khả năng đồng bộ dữ liệu giữa Desktop App và Web

#### Giải pháp

Triển khai toàn bộ hệ thống lên AWS với kiến trúc Serverless:

| Thành phần | Giải pháp AWS |
|------------|---------------|
| Xác thực người dùng | **Amazon Cognito** — Hosted UI + JWT Token |
| Backend API | **AWS Lambda** + **API Gateway** (Node.js/Express) |
| Cơ sở dữ liệu | **Amazon DynamoDB** — Single-Table Design |
| Lưu trữ ảnh | **Amazon S3** (Upload Bucket) |
| Hosting Frontend | **Amazon S3** + **Amazon CloudFront** (CDN) |
| Tên miền & SSL | **AWS Route 53** + **AWS ACM** (tùy chọn) |

---

### 3. Kiến trúc hệ thống

Hệ thống AI Assistant sử dụng kiến trúc Serverless với các dịch vụ AWS sau:

| Dịch vụ | Vai trò |
|---------|---------|
| **Amazon Cognito** | Xác thực và phân quyền người dùng |
| **AWS Lambda** | Chạy Backend API không cần server |
| **Amazon API Gateway** | HTTP API endpoint kết nối Lambda |
| **Amazon DynamoDB** | Database NoSQL lưu dữ liệu |
| **Amazon S3** | Lưu file Frontend và ảnh upload |
| **Amazon CloudFront** | CDN phân phối Frontend toàn cầu |
| **AWS Route 53** | Quản lý DNS và tên miền |
| **AWS ACM** | SSL/TLS Certificate cho HTTPS |

![Kiến trúc hệ thống AI Assistant](/images/2-Proposal/architec.jpg)

---

### 4. Kế hoạch triển khai

#### Giai đoạn 1: Nghiên cứu & Thiết kế

+ Khảo sát các dịch vụ AWS phù hợp với yêu cầu dự án (Cognito, Lambda, DynamoDB, S3, CloudFront)
+ Thiết kế kiến trúc hệ thống Serverless tổng thể
+ Xác định Single-Table Design cho DynamoDB, định nghĩa `PK`/`SK` cho từng entity
+ Thiết kế luồng xác thực (Cognito Hosted UI → JWT → API Gateway)
+ Thiết kế REST API Backend (endpoint, request/response structure)

#### Giai đoạn 2: Phát triển Backend

+ Xây dựng Backend API bằng Node.js/Express chạy trên AWS Lambda
+ Tích hợp **Amazon Cognito** để xác thực và phân quyền người dùng
+ Tích hợp **Amazon DynamoDB** để lưu trữ dữ liệu người dùng và trợ lý ảo
+ Tích hợp **Amazon S3** cho tính năng upload ảnh
+ Tích hợp **Google Gemini API** cho tính năng chat AI
+ Cấu hình **Serverless Framework** để triển khai Lambda và API Gateway

#### Giai đoạn 3: Phát triển Frontend & Desktop App

+ Xây dựng Web App bằng React (Vite) với Cognito Hosted UI
+ Phát triển các tính năng: chat AI, quản lý hồ sơ, upload ảnh
+ Cấu hình Unity Desktop App kết nối với Backend API và Cognito
+ Đồng bộ dữ liệu hai chiều giữa Desktop App và Web App

#### Giai đoạn 4: Triển khai & Kiểm thử

+ Deploy Backend lên AWS Lambda qua Serverless Framework
+ Deploy Frontend lên S3 + CloudFront
+ Kiểm thử luồng xác thực end-to-end (đăng ký, đăng nhập, token refresh)
+ Kiểm thử tính năng chat AI, đồng bộ dữ liệu giữa hai nền tảng
+ Cấu hình tên miền tùy chỉnh và SSL certificate (nếu có)

---

### 5. Ước tính chi phí

> Chi phí ước tính dựa trên AWS Pricing Calculator với mức sử dụng thấp (workshop/cá nhân).

| Dịch vụ | Chi phí ước tính |
|---------|-----------------|
| **AWS Lambda** | $0.00/tháng (< 1M request/tháng miễn phí) |
| **API Gateway** | ~$0.01/tháng (1,000 request) |
| **Amazon DynamoDB** | $0.00/tháng (On-demand, dưới free tier) |
| **Amazon S3** | ~$0.02/tháng (lưu trữ nhỏ) |
| **Amazon CloudFront** | $0.00/tháng (1 TB đầu tiên miễn phí) |
| **Amazon Cognito** | $0.00/tháng (50,000 MAU đầu miễn phí) |

**Tổng:** ~$0.03–0.05 USD/tháng *(gần như miễn phí với lưu lượng nhỏ)*


---

### 6. Đánh giá rủi ro

| Rủi ro | Mức độ | Biện pháp xử lý |
|--------|--------|-----------------|
| Luồng xác thực Cognito phức tạp (OAuth 2.0, callback URL, token) | Cao | Nghiên cứu kỹ tài liệu AWS Cognito; kiểm thử từng bước riêng lẻ |
| Đồng bộ dữ liệu không nhất quán giữa Desktop App và Web | Cao | Thiết kế API rõ ràng; dùng DynamoDB làm nguồn dữ liệu duy nhất |
| Tích hợp Gemini API gặp lỗi rate limit hoặc thay đổi response format | Trung bình | Xử lý lỗi tốt ở phía Backend; có fallback response |
| Unity WebGL/REST không tương thích với CORS của API Gateway | Trung bình | Cấu hình CORS đúng từ đầu; kiểm thử sớm trước khi phát triển sâu |
| Thiết kế DynamoDB không tối ưu gây khó mở rộng sau này | Trung bình | Phân tích kỹ access pattern trước khi tạo bảng |
| Chi phí phát sinh ngoài dự tính khi traffic tăng đột biến | Thấp | Bật AWS Budget Alert; theo dõi CloudWatch Metrics thường xuyên |

---

### 7. Kết quả kỳ vọng

+ Hệ thống **AI Assistant** hoàn chỉnh chạy trên AWS Cloud với kiến trúc Serverless
+ **Web App** truy cập được từ mọi thiết bị qua CloudFront URL
+ **Desktop App (Unity)** đồng bộ dữ liệu lên Cloud theo thời gian thực
+ Người dùng có thể **đăng ký, đăng nhập** an toàn qua Cognito Hosted UI
+ Chi phí vận hành **gần như bằng 0** với lưu lượng sử dụng cá nhân
+ Nền tảng có thể **mở rộng** dễ dàng khi số lượng người dùng tăng

### 8. Mã nguồn và link demo

*   **Mã nguồn dự án (GitHub):** [https://github.com/LeQu0cAnh/PetWebAWSProject](https://github.com/LeQu0cAnh/PetWebAWSProject)
*   **Trang tài liệu Workshop (GitHub Pages):** [https://locle1010.github.io/workshop/](https://locle1010.github.io/workshop/)
*   **Link Demo ứng dụng (Web):** [https://aa.locle1010.dpdns.org](https://aa.locle1010.dpdns.org)