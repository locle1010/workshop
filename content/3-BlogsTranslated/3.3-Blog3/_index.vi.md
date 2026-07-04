---
title: "Blog 3"
date: 2026-04-26
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---
# Tạo Model 3D từ việc dùng AI để chuyển đổi hình 2D sang 3D Assets trên AWS

> **Bài viết gốc:** [AWS Study Group - FCAJ](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2190041575094136)

![Tạo Model 3D bằng AI](/images/3-BlogsTranslated/Blog3_photo.jpg)

## 1. Tổng Quan Kiến Trúc Đề Xuất

Để đảm bảo sự cân bằng giữa hiệu suất xử lý đồ họa (GPU) và chi phí, workflow của chúng ta sẽ được chia thành hai giai đoạn tính toán chính, kết nối với nhau qua kho lưu trữ Amazon S3:

*   **Amazon S3 (Kho lưu trữ trung tâm):** Nơi chứa các bản vẽ concept 2D ban đầu và lưu trữ các file định dạng `.glb` (3D objects) được tạo ra ở mỗi bước.
*   **Bước 1 - Tạo Mesh (Amazon EC2 g4dn.2xlarge):** Sử dụng dòng máy ảo `g4dn` được trang bị GPU Nvidia. Chúng ta sẽ kéo ảnh từ S3 xuống, dùng mô hình **TripoSG** để tính toán và sinh ra phần khối geometry (mesh) cơ bản. File sau khi tạo sẽ được đẩy ngược lên S3.
*   **Bước 2 - Tạo và Dán Texture (Amazon EC2 g6e.2xlarge):** Giai đoạn multi-view texturing đòi hỏi lượng vRAM cực lớn. Tại đây, mô hình **MV-Adapter** sẽ dùng ảnh gốc 2D làm reference, phủ texture đa góc độ lên model vừa được tạo ra ở Bước 1.

## 2. Triển Khai Thực Tế & Tối Ưu Chi Phí

Việc đầu tiên là thiết lập hạ tầng mạng và chọn đúng Amazon Machine Image (AMI). Các lập trình viên nên dùng các AMI được cài sẵn môi trường Deep Learning (CUDA, PyTorch) để tiết kiệm thời gian cấu hình môi trường.

> [!TIP]
> **Pro-tip Tối Ưu Chi Phí:** Các dòng EC2 trang bị GPU như `g4dn` hay `g6e` có giá thuê theo giờ không hề rẻ. Nếu bạn là sinh viên IT hoặc lập trình viên mới, hãy tích cực tham gia các sự kiện "Platform First Cloud AI Journey Bootcamp" hoặc các workshop của AWS Việt Nam. Việc hoàn thành task để nhận về 100 AWS credits sẽ giúp bạn có dư dả ngân sách để thoải mái test các instance này trong giai đoạn R&D mà không lo "cháy túi".

### Khởi chạy môi trường và xử lý Lưới (Mesh)

Khi sử dụng MV-Adapter để phủ texture, có một vấn đề kỹ thuật thường gặp: các mesh sinh ra bởi AI (từ Tripo) đôi khi bị lỗi "non-manifold" (các mặt phẳng giao nhau không hợp lệ hoặc lưới bị hở).

Để quy trình không bị crash, chúng ta cần một đoạn script can thiệp để vá lỗi lưới ngay trên EC2 trước khi chạy texturing:

```bash
python fix_manifold.py inputs/raw_model.glb inputs/manifold_model.glb
```

Sau đó, tiến hành chạy lệnh tạo và phủ texture:

```bash
python -m scripts.texture_i2tex --image inputs/concept.jpeg --mesh inputs/manifold_model.glb --save_dir outputs --remove_bg
```

## 3. Đưa AI Asset Vào Game Engine (Mảnh Gem Cuối Cùng)

Nhiều bài hướng dẫn thường dừng lại ở việc tạo ra file `.glb` và coi đó là thành quả. Nhưng đối với một Game Developer, đó mới chỉ là nguyên liệu thô. File sinh ra từ AI có số lượng đa giác (polygon count) thường không được kiểm soát tốt, và đương nhiên là... chưa có xương (rig).

Chúng ta cần sử dụng các công cụ phổ biến như **Blender** để tối ưu hóa model và gắn xương (có thể dùng plugin Blender **"Rigify"**). Ngoài ra, ta có thể đưa model lên trang web **Mixamo** để tự động gắn xương và áp dụng các chuyển động (animation) sẵn có trên nền tảng này vào nhân vật.

## 4. Tổng Kết

Việc sử dụng các mô hình AI mã nguồn mở như TripoSG và MV-Adapter trên AWS EC2 giúp bạn làm chủ hoàn toàn quá trình sinh 3D asset mà không bị giới hạn bởi các API bên ngoài. Dù sản phẩm từ AI hiện tại chưa thể thay thế hoàn toàn bàn tay của một 3D Artist chuyên nghiệp, nhưng nó là một công cụ đắc lực trong giai đoạn Prototyping. Tự động hóa được khâu này, lập trình viên sẽ tiết kiệm được vô số thời gian để tập trung vào phần quan trọng nhất: thiết kế gameplay và viết script logic cho game.

**Nguồn tham khảo:** <https://aws.amazon.com/blogs/gametech/open-source-3d-game-asset-generation-using-aws>
