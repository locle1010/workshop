---
title: "Giới thiệu"
date: 2026-07-04
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

#### Giới thiệu về AI Assistant Workshop

Trong workshop này, bạn sẽ triển khai toàn bộ hệ thống **AI Assistant** lên AWS Cloud theo kiến trúc **Serverless** hiện đại — không cần quản lý server, tự động mở rộng quy mô và chỉ trả tiền theo lượng sử dụng thực tế.

#### Về dự án AI Assistant

**AI Assistant** là ứng dụng tích hợp AI với các tính năng:

+  Chat với AI (Google Gemini)
+  Đồng bộ dữ liệu giữa Desktop App (Unity) và Web
+  Lưu trữ dữ liệu trên AWS

#### Kiến trúc hệ thống

Hệ thống AI Assistant sử dụng các dịch vụ AWS sau:

| Dịch vụ | Vai trò |
|---------|---------|
| **Amazon Cognito** | Xác thực và phân quyền người dùng |
| **AWS Lambda** | Chạy Backend API (Node.js/Express) không cần server |
| **API Gateway** | HTTP API endpoint kết nối với Lambda |
| **Amazon DynamoDB** | Database NoSQL lưu trữ dữ liệu |
| **Amazon S3** | Lưu trữ file tĩnh Frontend và ảnh upload |
| **Amazon CloudFront** | CDN phân phối Frontend toàn cầu |
| **AWS Route 53** | Quản lý DNS và tên miền |
| **AWS ACM** | SSL/TLS Certificate cho HTTPS |

#### Tổng quan về workshop

Trong workshop này, bạn sẽ đi qua các bước:

1. **Chuẩn bị môi trường** — Cài đặt Git, Node.js, AWS CLI và Serverless Framework
2. **Thiết lập Cognito** — Tạo User Pool để xác thực người dùng
3. **Thiết lập DynamoDB** — Tạo bảng database
4. **Thiết lập S3 Upload** — Cấu hình bucket lưu trữ ảnh
5. **Deploy Backend** — Triển khai API lên AWS Lambda qua Serverless Framework
6. **Deploy Frontend** — Build React app và host trên S3 + CloudFront
7. **Cấu hình Domain** — Gắn tên miền và SSL certificate (tùy chọn)
8. **Dọn dẹp** — Xóa tài nguyên để tránh phát sinh chi phí

> **Lưu ý:** Workshop ưu tiên dùng **AWS CLI** cho mọi bước. Trong trường hợp bất khả kháng (config phức tạp, lỗi CLI), sẽ có hướng dẫn thay thế qua **AWS Console**.

![Kiến trúc hệ thống AI Assistant](/images/5-Workshop/5.1-Introduction/architec.jpg)
