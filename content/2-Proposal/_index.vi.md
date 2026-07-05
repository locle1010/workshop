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

Triển khai toàn bộ hệ thống lên AWS với kiến trúc Serverless và cơ chế bảo mật tối ưu:

| Thành phần | Giải pháp AWS |
|------------|---------------|
| Xác thực người dùng | **Amazon Cognito** — Hosted UI + JWT Token |
| Backend API | **AWS Lambda** + **API Gateway** (Node.js/Express) |
| Cơ sở dữ liệu | **Amazon DynamoDB** — Single-Table Design |
| Lưu trữ ảnh | **Amazon S3** (Upload Bucket) |
| Hosting Frontend | **Amazon S3** + **Amazon CloudFront** (CDN) |
| Tên miền & SSL | **AWS Route 53** + **AWS ACM** (tùy chọn) |
| Phân quyền tối giản | **AWS IAM** — Vai trò deployer & service-linked role |
| Quản lý khóa bí mật | **AWS KMS** + **AWS Secrets Manager** — Mã hóa GEMINI_API_KEY |
| Giám sát bảo mật | **Amazon GuardDuty** + **AWS Config** — Phát hiện hành vi đáng ngờ |
| Tối ưu chi phí S3 | **S3 Lifecycle Rules** — Tự động lưu trữ Glacier Flexible Retrieval |
| Bảo mật Web | **AWS WAF** — Bộ lọc ứng dụng Web bảo vệ CloudFront |
| Tự động hóa CI/CD | **AWS CodePipeline** + **AWS CodeBuild** — Luồng triển khai tự động từ GitHub |
| Sao lưu cơ sở dữ liệu | **AWS Backup** — Kế hoạch sao lưu DynamoDB tự động hàng ngày |
| Giám sát hệ thống | **Amazon CloudWatch** — Log & Alarm cảnh báo lỗi backend |

---

### 3. Kiến trúc hệ thống

Hệ thống AI Assistant sử dụng kiến trúc Serverless tích hợp đầy đủ 17 dịch vụ AWS sau:

| Dịch vụ | Vai trò |
|---------|---------|
| **Amazon Cognito** | Xác thực và phân quyền người dùng |
| **AWS Lambda** | Chạy Backend API không cần server |
| **Amazon API Gateway** | HTTP API endpoint kết nối Lambda |
| **Amazon DynamoDB** | Database NoSQL lưu dữ liệu |
| **Amazon S3** | Lưu file Frontend (Static Web) và ảnh upload |
| **Amazon CloudFront** | CDN phân phối Frontend toàn cầu |
| **AWS Route 53** | Quản lý DNS và tên miền |
| **AWS ACM** | SSL/TLS Certificate cho HTTPS |
| **AWS IAM** | Quản lý quyền và vai trò truy cập tài nguyên bảo mật |
| **AWS KMS** | Quản lý khóa mã hóa dữ liệu nhạy cảm |
| **AWS Secrets Manager** | Lưu trữ và bảo vệ Google Gemini API Key |
| **Amazon CloudWatch** | Thu thập log Lambda và cấu hình Alarm cảnh báo lỗi |
| **Amazon GuardDuty** | Giám sát mối đe dọa bảo mật liên tục trên tài khoản |
| **AWS Config** | Đánh giá và ghi nhận cấu hình tài nguyên theo chuẩn |
| **AWS WAF** | Tường lửa ứng dụng web bảo vệ CloudFront khỏi tấn công |
| **AWS Backup** | Tự động sao lưu và bảo vệ dữ liệu DynamoDB hàng ngày |
| **AWS CodePipeline** | Tự động luồng CI/CD triển khai ứng dụng |
| **AWS CodeBuild** | Xây dựng và deploy code backend/frontend tự động |

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
+ Tích hợp mã hóa **AWS KMS** và **AWS Secrets Manager** bảo vệ khóa API
+ Cấu hình **Serverless Framework** để triển khai Lambda và API Gateway

#### Giai đoạn 3: Phát triển Frontend & Desktop App

+ Xây dựng Web App bằng React (Vite) với Cognito Hosted UI
+ Phát triển các tính năng: chat AI, quản lý hồ sơ, upload ảnh
+ Cấu hình Unity Desktop App kết nối với Backend API và Cognito
+ Đồng bộ dữ liệu hai chiều giữa Desktop App và Web App

#### Giai đoạn 4: Triển khai, Bảo mật & Tự động hóa

+ Deploy Backend lên AWS Lambda qua Serverless Framework
+ Deploy Frontend lên S3 + CloudFront
+ Cấu hình tên miền tùy chỉnh và SSL certificate (Route 53 & ACM)
+ Thiết lập **AWS CodePipeline** và **AWS CodeBuild** để build/deploy tự động khi push code lên GitHub `main` branch
+ Triển khai cấu hình bảo mật với **Amazon GuardDuty**, **AWS Config**, và **AWS WAF**
+ Kích hoạt quy trình sao lưu **AWS Backup** cho DynamoDB và **CloudWatch Alarm** để gửi cảnh báo lỗi

---

### 5. Ước tính chi phí

> Chi phí ước tính chi tiết được xuất từ công cụ **AWS Pricing Calculator** dựa trên mức sử dụng thử nghiệm (mã cấu hình: `c964d0cc-491c-4141-a922-fa56a58fae21`).

| Dịch vụ | Chi tiết cấu hình ước tính | Chi phí ước tính (USD/tháng) |
|---------|----------------------------|----------------------------|
| **AWS WAF** | 1 WebACL ($5.00) + 1 Rule ($1.00) + 1,000 requests | $6.00 |
| **Amazon CloudFront** | 20 GB Outbound Data Transfer + 1,000 HTTPS Requests | $2.65 |
| **Amazon CloudWatch** | 5 Custom Metrics ($1.50) + 2 Alarms ($0.20) + 0.15 GB Log storage | $1.70 |
| **AWS CodePipeline** | 1 Active Pipeline | $1.00 |
| **AWS Backup** | Sao lưu bảng DynamoDB (~7 GB-month Warm Storage) | $0.79 |
| **Amazon Route 53** | 1 Hosted Zone ($0.50) + 1,000 DNS Queries | $0.50 |
| **AWS Secrets Manager** | 1 Secret ($0.40) + 1,000 API Requests | $0.41 |
| **AWS Config** | 50 Configuration Items Recorded + 10 Rule Evaluations | $0.16 |
| **Amazon S3** | 5 GB-month Standard Storage + 1,000 Tier 1 Requests | $0.13 |
| **AWS CodeBuild** | 10 phút build (Linux ARM) | $0.02 |
| **Amazon Cognito** | Phí theo dõi active user (1 MAU) | $0.01 |
| **Amazon GuardDuty** | Phân tích 1,000 S3 + CloudTrail log events | $0.01 |
| **Amazon API Gateway** | 1,000 Requests REST/HTTP API | $0.01 |
| **AWS Lambda** | 1,000 Requests (ARM64, 50 GB-seconds) | $0.00 (Dưới Free Tier) |
| **AWS KMS** | 1,000 Requests giải mã khóa mật mã | $0.00 (Dưới Free Tier) |
| **Amazon DynamoDB** | 10 GB Storage + 1,000 WCU + 500 RCU | $0.00 (Dưới Free Tier) |

**Tổng chi phí dự kiến:** **~$13.39 USD/tháng**

---

### 6. Đánh giá rủi ro

| Rủi ro | Mức độ | Biện pháp xử lý |
|--------|--------|-----------------|
| Luồng xác thực Cognito phức tạp (OAuth 2.0, callback URL, token) | Cao | Nghiên cứu kỹ tài liệu AWS Cognito; kiểm thử từng bước riêng lẻ |
| Đồng bộ dữ liệu không nhất quán giữa Desktop App và Web | Cao | Thiết kế API rõ ràng; dùng DynamoDB làm nguồn dữ liệu duy nhất |
| Khóa API Gemini bị lộ khi đẩy code lên repository công khai | Cao | Sử dụng AWS Secrets Manager kết hợp AWS KMS để lưu trữ và mã hóa khóa API ở runtime |
| Lỗi cấu hình CI/CD làm gián đoạn luồng triển khai tự động | Trung bình | Kiểm tra kỹ tệp buildspec và phân quyền IAM phù hợp cho CodeBuild |
| Chi phí phát sinh ngoài dự tính khi traffic tăng đột biến hoặc quên tắt tài nguyên | Trung bình | Bật AWS Budget Alert; theo dõi CloudWatch Alarms; thực hiện dọn dẹp ở Bước 5.9 |

---

### 7. Kết quả kỳ vọng

+ Hệ thống **AI Assistant** hoàn chỉnh chạy trên AWS Cloud với kiến trúc Serverless
+ **Web App** truy cập được từ mọi thiết bị qua CloudFront URL bảo mật bởi AWS WAF
+ **Desktop App (Unity)** đồng bộ dữ liệu lên Cloud theo thời gian thực
+ Người dùng có thể **đăng ký, đăng nhập** an toàn qua Cognito
+ Quy trình **CI/CD tự động** deploy code lập tức khi push lên GitHub
+ Các giải pháp **sao lưu, bảo mật nền tảng và quản lý secrets** được triển khai toàn diện

---

### 8. Mã nguồn và link demo

*   **Mã nguồn dự án (GitHub):** [https://github.com/LeQu0cAnh/PetWebAWSProject](https://github.com/LeQu0cAnh/PetWebAWSProject)
*   **Trang tài liệu Workshop (GitHub Pages):** [https://locle1010.github.io/workshop/](https://locle1010.github.io/workshop/)
*   **Link Demo ứng dụng (Web):** [https://aa.locle1010.dpdns.org](https://aa.locle1010.dpdns.org)