---
title: "Custom Domain (Tùy chọn)"
date: 2026-07-04
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

#### Tổng quan

> **Bước này là tùy chọn.** Bỏ qua nếu bạn không có tên miền riêng.

Trong bước này, bạn sẽ gắn tên miền tùy chỉnh (ví dụ: `pet.yourdomain.com`) vào CloudFront Distribution và cấp SSL certificate miễn phí thông qua **AWS Certificate Manager (ACM)**.

---

### Yêu cầu

+ Bạn đã có tên miền (mua từ Route 53 hoặc nhà cung cấp khác)
+ Name Servers của tên miền đã trỏ về **AWS Route 53**

Kiểm tra Hosted Zone đang quản lý:
```bash
aws route53 list-hosted-zones-by-name \
  --dns-name "yourdomain.com" \
  --query "HostedZones[0].{Name:Name,Id:Id}" \
  --output table
```

![Kiểm tra Hosted Zone](/images/5-Workshop/5.6-Domain/5.6.1.png)

---

### Tạo SSL Certificate (ACM)

> **Bắt buộc:** CloudFront chỉ chấp nhận Certificate được tạo tại Region **`us-east-1` (N. Virginia)**.

```bash
aws acm request-certificate \
  --domain-name "yourdomain.com" \
  --subject-alternative-names "*.yourdomain.com" \
  --validation-method DNS \
  --region us-east-1 \
  --query "CertificateArn" \
  --output text
```

 **Lưu lại `CertificateArn`**

![Tạo SSL Certificate](/images/5-Workshop/5.6-Domain/5.6.2.png)

Lấy DNS validation record:
```bash
aws acm describe-certificate \
  --certificate-arn <CertificateArn> \
  --region us-east-1 \
  --query "Certificate.DomainValidationOptions[0].ResourceRecord"
```

Output có dạng:
```json
{
    "Name": "_xxxx.yourdomain.com.",
    "Type": "CNAME",
    "Value": "_yyyy.acm-validations.aws."
}
```

![Lấy DNS validation record](/images/5-Workshop/5.6-Domain/5.6.3.png)

---

### Xác thực Certificate qua DNS

#### Dùng Route 53

**Console:** ACM → Chọn certificate → Nhấn nút **Create records in Route 53** → Create records

![Chọn certificate trong ACM](/images/5-Workshop/5.6-Domain/5.6.4.png)

![Tạo DNS records trong Route 53](/images/5-Workshop/5.6-Domain/5.6.5.png)

![Xác nhận tạo records](/images/5-Workshop/5.6-Domain/5.6.6.png)

![Records đã được tạo thành công](/images/5-Workshop/5.6-Domain/5.6.7.png)

Chờ 2-5 phút cho status chuyển thành **Issued**

![Certificate đã được xác thực (Issued)](/images/5-Workshop/5.6-Domain/5.6.8.png)

---

### Gắn Certificate vào CloudFront

**Console:** CloudFront → Distribution → **Add domains**

![Mở Distribution để thêm domain](/images/5-Workshop/5.6-Domain/5.6.9.png)

![Màn hình Add domains](/images/5-Workshop/5.6-Domain/5.6.10.png)

+ **Configure domains:** Thêm `yourdomain.com` và `www.yourdomain.com`

![Cấu hình tên miền](/images/5-Workshop/5.6-Domain/5.6.11.png)

+ **Get TLS certificate:** Chọn certificate ACM vừa tạo (chỉ hiển thị certificate ở us-east-1)

![Chọn TLS certificate](/images/5-Workshop/5.6-Domain/5.6.12.png)

+ Nhấn **Add domains**

![Xác nhận thêm domain](/images/5-Workshop/5.6-Domain/5.6.13.png)

+ Chờ CloudFront cập nhật xong chọn **Route domains to CloudFront**

![Route domains to CloudFront](/images/5-Workshop/5.6-Domain/5.6.14.png)

+ Chọn **Set up routing automatically**

![Cấu hình routing tự động](/images/5-Workshop/5.6-Domain/5.6.15.png)

---

### Cập nhật CORS Backend lần cuối

Sau khi domain đã hoạt động, cập nhật `backend/.env.production`:
```env
FRONTEND_URL=https://yourdomain.com
```

Re-deploy:
```bash
cd backend
npx serverless deploy --stage prod
```

---

### Kiểm tra kết quả

Sau ~5-10 phút cho DNS propagate, truy cập `https://yourdomain.com` để kiểm tra.

![Trang web chạy trên tên miền tùy chỉnh](/images/5-Workshop/5.6-Domain/5.6.16.png)
