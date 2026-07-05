---
title: "Chuẩn bị môi trường"
date: 2026-07-04
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

#### Tổng quan

Trước khi bắt đầu triển khai, bạn cần cài đặt các công cụ cần thiết và cấu hình quyền truy cập AWS.

---

#### Yêu cầu phần mềm

Bạn cần cài đặt đầy đủ 3 công cụ sau theo thứ tự:

| # | Phần mềm | Phiên bản |
|---|----------|-----------|
| 1 | **Git** | 2.x trở lên |
| 2 | **Node.js** | 2x.x |
| 3 | **AWS CLI** | 2.x |

---

#### Cài đặt Git

Tải installer tại: [https://git-scm.com/downloads](https://git-scm.com/downloads)

+ **Windows:** Tải file `.exe` → chạy installer → giữ nguyên mặc định → Next đến Finish
+ **macOS:** Chạy `brew install git` (nếu có Homebrew) hoặc tải `.dmg` từ trang trên
+ **Linux (Ubuntu/Debian):** `sudo apt-get install git -y`

Kiểm tra:
```bash
git --version
# Kết quả mong đợi: git version 2.x.x
```

---

#### Cài đặt Node.js 2x.x

Tải installer tại: [https://nodejs.org/en/download](https://nodejs.org/en/download)

+ **Windows:** Tải file `.msi` → chạy installer → Next đến Finish (tích chọn "Add to PATH")
+ **macOS:** Tải file `.pkg` → chạy installer
+ **Linux:**
```bash
curl -fsSL https://deb.nodesource.com/setup_2x.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Kiểm tra:
```bash
node --version
# Kết quả mong đợi: v2x.x.x
npm --version
# Kết quả mong đợi: 1x.x.x
```

---

#### Cài đặt AWS CLI v2

**Windows:**
1. Tải file MSI installer tại: [https://awscli.amazonaws.com/AWSCLIV2.msi](https://awscli.amazonaws.com/AWSCLIV2.msi)
2. Chạy file `.msi` vừa tải về
3. Nhấn **Next** → **Next** → **Install** → **Finish**
4. Mở lại **Command Prompt** hoặc **PowerShell** (cần mở lại để nhận PATH mới)

**macOS:**
```bash
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
```

Hoặc dùng Homebrew:
```bash
brew install awscli
```

**Linux (Ubuntu/Debian):**
```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

**Kiểm tra cài đặt thành công:**
```bash
aws --version
# Kết quả mong đợi: aws-cli/2.x.x Python/3.x.x ...
```

> Tài liệu chính thức: [https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

---

#### Kiểm tra tất cả công cụ

Sau khi cài xong cả 3, chạy lệnh kiểm tra tổng hợp:
```bash
git --version
node --version
npm --version
aws --version
```

![Kiểm tra phiên bản các công cụ](/images/5-Workshop/5.2-Prerequisite/5.2.1.png)

---

#### Tạo IAM User và cấu hình AWS CLI

> **Quan trọng:** AWS CLI không bao giờ được dùng credentials của Root Account. Bạn phải tạo IAM User riêng trong AWS Console trước, sau đó mới dùng key của IAM User đó để đăng nhập CLI.

> **Yêu cầu:** Cần có sẵn tài khoản AWS. Nếu chưa có, đăng ký miễn phí tại [https://aws.amazon.com/free](https://aws.amazon.com/free)

---

##### Bước 1: Đăng nhập AWS Console bằng Root Account

Truy cập [https://console.aws.amazon.com](https://console.aws.amazon.com) và đăng nhập bằng email + mật khẩu Root Account.

> Đây là lần duy nhất bạn dùng Root Account. Sau khi tạo xong IAM User, hãy đăng xuất khỏi Root.

---

##### Bước 2: Tạo IAM User cho Deploy

**Bước 2.1:** Trong Console, tìm và mở dịch vụ **IAM**

> Gõ "IAM" vào thanh tìm kiếm ở trên cùng rồi chọn kết quả đầu tiên

> Hoặc truy cập thẳng: [https://console.aws.amazon.com/iam/home#/users](https://console.aws.amazon.com/iam/home#/users)

![Tìm kiếm IAM trong Console](/images/5-Workshop/5.2-Prerequisite/5.2.2.png)

![Trang IAM Console](/images/5-Workshop/5.2-Prerequisite/5.2.3.png)

**Bước 2.2:** Trong menu bên trái, chọn **Users** → nhấn **Create user**

![Danh sách Users](/images/5-Workshop/5.2-Prerequisite/5.2.4.png)

![Nút Create user](/images/5-Workshop/5.2-Prerequisite/5.2.5.png)

**Bước 2.3:** Điền thông tin user:
- **User name:** `ai-assistant-deployer`
- Nhấn **Next**

![Điền tên user](/images/5-Workshop/5.2-Prerequisite/5.2.6.png)

**Bước 2.4:** Thiết lập quyền — chọn **Attach policies directly**

Trong ô tìm kiếm, gõ `AdministratorAccess`, tích chọn policy đó rồi nhấn **Next**

![Chọn policy AdministratorAccess](/images/5-Workshop/5.2-Prerequisite/5.2.7.png)

**Bước 2.5:** Xem lại thông tin → nhấn **Create user**

![Review và tạo user](/images/5-Workshop/5.2-Prerequisite/5.2.8.png)

---

##### Bước 3: Tạo Access Key cho IAM User vừa tạo

**Bước 3.1:** Sau khi tạo xong, nhấn vào tên user `ai-assistant-deployer` để vào trang chi tiết

![Chi tiết IAM User](/images/5-Workshop/5.2-Prerequisite/5.2.9.png)

**Bước 3.2:** Chọn tab **Security credentials**

![Tab Security credentials](/images/5-Workshop/5.2-Prerequisite/5.2.10.png)

**Bước 3.3:** Cuộn xuống mục **Access keys** → nhấn **Create access key**

![Mục Access keys](/images/5-Workshop/5.2-Prerequisite/5.2.11.png)

**Bước 3.4:** Chọn use case **Command Line Interface (CLI)** → tích xác nhận ở dưới → nhấn **Next** → nhấn **Create access key**

![Chọn CLI use case](/images/5-Workshop/5.2-Prerequisite/5.2.12.png)

![Xác nhận và tạo access key](/images/5-Workshop/5.2-Prerequisite/5.2.13.png)

**Bước 3.5:** Màn hình hiển thị credentials — **lưu lại ngay:**
- `Access Key ID` (dạng: `AKIAIOSFODNN7EXAMPLE`)
- `Secret Access Key` (dạng: `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY`)

Hoặc nhấn **Download .csv file** để tải về file chứa cả hai giá trị.

> Secret Access Key **chỉ hiển thị một lần duy nhất**. Nếu bỏ lỡ, phải xóa key cũ và tạo lại.

![Credentials được tạo thành công](/images/5-Workshop/5.2-Prerequisite/5.2.14.png)

---

##### Bước 4: Cấu hình AWS CLI với IAM User

Mở Terminal (PowerShell trên Windows, Terminal trên macOS/Linux) và chạy:

```bash
aws configure
```

Điền lần lượt theo từng dòng:

```
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: ap-southeast-1
Default output format [None]: json
```

> Thông tin này được lưu tại `~/.aws/credentials` và `~/.aws/config` trên máy bạn.

![Cấu hình AWS CLI](/images/5-Workshop/5.2-Prerequisite/5.2.15.png)

---

##### Bước 5: Xác minh đăng nhập thành công

```bash
aws sts get-caller-identity
```

Kết quả mong đợi — phần `Arn` phải chứa tên IAM User, **không phải root**:

```json
{
    "UserId": "AIDAXXXXXXXXXXXXXXXXX",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/ai-assistant-deployer"
}
```

Nếu thấy `user/ai-assistant-deployer` trong Arn → cấu hình đúng, sẵn sàng deploy.

![Xác minh đăng nhập thành công](/images/5-Workshop/5.2-Prerequisite/5.2.16.png)

---

#### Clone repository

```bash
git clone https://github.com/LeQu0cAnh/PetWebAWSProject.git
cd PetWebAWSProject
```

![Clone repository](/images/5-Workshop/5.2-Prerequisite/5.2.17.png)

---

#### Cài đặt Serverless Framework

Serverless Framework giúp deploy Lambda và API Gateway chỉ với một lệnh duy nhất.

```bash
npm install -g serverless@3
serverless --version
```

![Cài đặt Serverless Framework thành công](/images/5-Workshop/5.2-Prerequisite/5.2.18.png)

> Trên macOS/Linux nếu gặp lỗi quyền: `sudo npm install -g serverless@3`

---

#### Bảo mật nền tảng (Amazon GuardDuty & AWS Config)

Để đảm bảo tài khoản AWS được bảo mật và giám sát liên tục trước các hành vi bất thường, hãy kích hoạt **Amazon GuardDuty** và **AWS Config**.

##### 1. Kích hoạt Amazon GuardDuty (Phát hiện mối đe dọa)

Amazon GuardDuty liên tục giám sát các hành vi đáng ngờ (ví dụ: đăng nhập lạ, tấn công mạng, đào tiền ảo trái phép) dựa trên log của tài khoản.

**CLI:**
```bash
aws guardduty create-detector --enable --region ap-southeast-1
```
![Kích hoạt GuardDuty qua CLI](/images/5-Workshop/5.2-Prerequisite/5.2.19.png)


**Console:**
1. Tìm kiếm và mở dịch vụ **GuardDuty** trong AWS Web Console.
2. Nhấn nút **Get started** -> chọn **Enable GuardDuty**.

![Kích hoạt GuardDuty qua Console Step 1](/images/5-Workshop/5.2-Prerequisite/5.2.20.png)
![Kích hoạt GuardDuty qua Console Step 2](/images/5-Workshop/5.2-Prerequisite/5.2.21.png)

##### 2. Thiết lập AWS Config (Giám sát cấu hình tài nguyên)

AWS Config theo dõi toàn bộ lịch sử thay đổi cấu hình tài nguyên AWS và so sánh với các chính sách bảo mật.

**Console:**
1. Tìm kiếm dịch vụ **AWSConfig** -> chọn **1-click setup**.
![Thiết lập AWS Config Step 1](/images/5-Workshop/5.2-Prerequisite/5.2.22.png)
2. Nhấn **Confirm** để hoàn tất.
![Thiết lập AWS Config Step 2](/images/5-Workshop/5.2-Prerequisite/5.2.23.png)

---

Sau khi hoàn thành bước này, bạn đã sẵn sàng để bắt đầu thiết lập các dịch vụ AWS.
