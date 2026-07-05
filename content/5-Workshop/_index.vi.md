---
title: "AI Assistant Workshop"
date: 2026-07-04
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Triển khai AI Assistant lên AWS Cloud

#### Tổng quan

**AI Assistant** là ứng dụng trợ lý ảo tích hợp AI. Workshop này hướng dẫn bạn từng bước triển khai hệ thống lên AWS theo kiến trúc **Serverless** — không cần quản lý server, tự mở rộng quy mô và tối ưu chi phí.

Toàn bộ các bước trong workshop này ưu tiên sử dụng **AWS CLI**. Trong trường hợp bất khả kháng (cấu hình phức tạp, lỗi CLI), sẽ có hướng dẫn thay thế qua **AWS Web Console**.

#### Nội dung

1. [Giới thiệu & Kiến trúc](5.1-Introduction/)
2. [Chuẩn bị môi trường & Bảo mật nền tảng](5.2-Prerequisite/)
3. [Thiết lập Cognito, DynamoDB & AWS Backup](5.3-Database/)
4. [Thiết lập KMS, Secrets Manager, S3 & Deploy Backend](5.4-Backend/)
5. [Deploy Frontend lên CloudFront & cấu hình AWS WAF](5.5-Frontend/)
6. [Cấu hình Custom Domain (Tùy chọn)](5.6-Domain/)
7. [Thiết lập CI/CD tự động](5.7-CICD/)
8. [Cài đặt Unity App](5.8-UnityApp/)
9. [Dọn dẹp tài nguyên](5.9-Cleanup/)