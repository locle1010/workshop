---
title: "Cài đặt Unity App"
date: 2026-07-04
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

#### Tổng quan

Desktop App là ứng dụng Unity chạy trên máy tính người dùng, cho phép:

+ Chat với AI Assistant qua khung chat trên màn hình
+ Đồng bộ dữ liệu lên Cloud (AWS Lambda + DynamoDB)
+ Kết nối xác thực người dùng qua Cognito Hosted UI

Trong bước này, bạn sẽ cài đặt Unity Hub, tải đúng Unity Editor phiên bản chính xác của dự án, mở project và cập nhật các thông số kết nối API.

---

#### Cài đặt Unity Hub

Unity Hub là công cụ quản lý các phiên bản Unity Editor và dự án.

**Tải Unity Hub tại:** [https://unity.com/download](https://unity.com/download)

---

#### Đăng nhập Unity Hub

Sau khi mở Unity Hub, bạn cần đăng nhập tài khoản Unity ID:

1. Nhấn **Sign in** ở góc phải trên
2. Đăng nhập bằng Unity ID (đăng ký miễn phí tại [https://id.unity.com](https://id.unity.com) nếu chưa có)
3. Sau khi đăng nhập thành công, quay lại Unity Hub

> Phần mềm cho phép cá nhân dùng **miễn phí (Personal plan)** — đủ để mở và chạy dự án này.

---

#### Mở dự án trong Unity Editor

**Bước 1:** Trong Unity Hub, chuyển sang tab **Projects**

**Bước 2:** Nhấn **Add** → Chọn **Add project from disk...**

![Thêm project từ disk](/images/5-Workshop/5.7-UnityApp/5.7.1.png)

**Bước 3:** Di chuyển đến thư mục chứa dự án:
```
<thư-mục-dự-án>/app/
```
Chọn thư mục `app` rồi nhấn **Open**

**Bước 4:** Unity Hub có thể hỏi về phiên bản Editor. Chọn **2022.3.62f2** từ dropdown (nếu chưa cài thì tải và cài đặt phiên bản này)

**Bước 5:** Nhấn vào project để mở

![Mở project trong Unity Hub](/images/5-Workshop/5.7-UnityApp/5.7.2.png)

---

#### Cập nhật API URL trong Unity Inspector

Sau khi dự án mở và compile xong, bạn cần cập nhật URL các inspector sau.

---

##### Script 1: ZME_CloudSync (Đồng bộ Cloud)

Script này xử lý đăng nhập Cognito và đồng bộ dữ liệu lên backend.

**Bước 1:** Trong tab **Hierarchy** (góc trái màn hình), tìm và chọn GameObject có tên **AWS_Manager**

**Bước 2:** Chuyển sang tab **Inspector** (góc phải màn hình)

**Bước 3:** Tìm section **"Backend API"** và điền các trường:

| Trường trong Inspector | Giá trị cần điền |
|------------------------|-----------------|
| **Backend Api Url** | `https://<API_ID>.execute-api.ap-southeast-1.amazonaws.com/api/pet/profile` |
| **Cognito Domain** | `https://ai-assistant-auth.auth.ap-southeast-1.amazoncognito.com` |
| **Client Id** | `<ClientId lấy từ Bước 5.3>` |

> Thay `<API_ID>` bằng ID endpoint lấy từ kết quả `serverless deploy` ở Bước 5.4.

**Bước 4:** Lưu Scene bằng **Ctrl+S** (Windows) or **Cmd+S** (macOS)

![Cập nhật API URL trong Unity Inspector](/images/5-Workshop/5.7-UnityApp/5.7.3.png)

---

#### Build ứng dụng thành file chạy độc lập

Nếu muốn đóng gói thành file `.exe` để chạy mà không cần Unity Editor:

1. Vào **File** > **Build Settings**

![Mở Build Settings](/images/5-Workshop/5.7-UnityApp/5.7.4.png)

2. Chọn Platform: **PC, Mac and Linux Standalone**
3. Nhấn **Switch Platform**
4. **Target Platform:** Windows
5. Nhấn **Build** và chọn thư mục xuất

![Cấu hình Build và xuất file](/images/5-Workshop/5.7-UnityApp/5.7.5.png)

6. Chạy file `.exe` vừa tạo

![Ứng dụng Unity chạy thành công](/images/5-Workshop/5.7-UnityApp/5.7.6.png)
