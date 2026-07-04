---
title: "Event 3"
date: 2026-06-27
weight: 1
chapter: false
pre: " <b> 4.3. </b> "
---
# BÀI THU HOẠCH: XÂY DỰNG TRỢ LÝ GIỌNG NÓI AI (VOICE AGENTS) QUY MÔ LỚN

### Mục Đích Của Sự Kiện

* Giới thiệu tổng quan về cơ chế hoạt động của Voice AI và cách tích hợp giao tiếp bằng giọng nói vào hệ thống trí tuệ nhân tạo.
* Phân tích các giới hạn của AI giọng nói hiện tại đối với tiếng Việt và giải quyết bài toán xử lý ngôn ngữ trong môi trường doanh nghiệp (đặc biệt là khối tài chính - ngân hàng).
* Khám phá các tiêu chuẩn kiến trúc để đưa một Voice Agent từ môi trường thử nghiệm (POC) lên môi trường thực tế (Production).
* Truyền cảm hứng cho việc phát triển các trải nghiệm tương tác giữa người và máy tính một cách tự nhiên, bảo mật và tin cậy.

### Danh Sách Diễn Giả

* **Hiếu Nghị**
* **Kiệt**
* **Trung Đỗ**

### Nội Dung Nổi Bật

#### 1. Lựa chọn kiến trúc AI phù hợp với ngôn ngữ đặc thù
* Các mô hình Speech-to-Speech hiện tại đa phần chỉ tối ưu hóa tốt cho tiếng Anh. Tiếng Việt là một ngôn ngữ ít tài nguyên (low-resource language), nên để vận hành hiệu quả trong doanh nghiệp, kiến trúc hệ thống thường được tách rời thành 3 khối: **Speech-to-Text (STT) -> LLM xử lý văn bản -> Text-to-Speech (TTS)**.
* Kiến trúc phân tách này giúp doanh nghiệp kiểm soát chặt chẽ câu trả lời của AI (giảm thiểu tình trạng ảo giác) và dễ dàng thực thi các tác vụ gọi hàm phức tạp (Tool calling) như tự động khóa thẻ ngân hàng hoặc tra cứu số dư tài khoản.

#### 2. Xử lý ngữ cảnh và văn hóa giao tiếp (Context & Culture)
* **Xưng hô chuẩn xác:** Hệ thống cần có khả năng nhận diện giới tính và độ tuổi từ giọng nói để xưng hô (anh/chị) một cách chính xác và lịch sự.
* **Xử lý ngắt lời:** AI phải được huấn luyện để phân biệt được khi nào người dùng đang tạm nghỉ để suy nghĩ (ví dụ: ngắt quãng khi đọc số điện thoại) và khi nào đã thực sự nói xong, tránh tình trạng "cướp lời" khách hàng.
* **Nhận diện giọng vùng miền (Accent):** Dữ liệu huấn luyện STT cần tích hợp từ 10% đến 20% giọng địa phương để đảm bảo độ chính xác khi nhận dạng. Tuy nhiên, giọng phản hồi của AI (TTS) vẫn nên giữ giọng chuẩn phổ thông để duy trì tính chuyên nghiệp.

#### 3. Tối ưu hóa tốc độ phản hồi và đưa lên Production
* **Độ trễ (Latency):** Đây là yếu tố quyết định thành bại của Voice AI. Toàn bộ luồng xử lý từ STT, LLM đến TTS phải được thực thi dưới dạng truyền phát (streaming) liên tục thay vì xử lý tuần tự từng khối lớn.
* **Cơ chế Human-in-the-loop:** Hệ thống cần tự động phát hiện các tình huống AI vượt quá giới hạn xử lý (ví dụ: khách hàng tỏ thái độ cáu gắt) để nhanh chóng chuyển tiếp cuộc gọi mượt mà đến điện thoại viên con người.

### Những Gì Học Được

#### Tư Duy Thiết Kế
* Sản phẩm AI tốt không chỉ nằm ở công nghệ cốt lõi mà phải tập trung vào trải nghiệm người dùng, thấu hiểu hành vi và văn hóa giao tiếp tự nhiên của con người.
* Khi thiết kế hệ thống tự động hóa, luôn cần tích hợp cơ chế dự phòng chuyển giao cho con người (Human-in-the-loop) tại các điểm chạm dịch vụ quan trọng.

#### Kiến Trúc Kỹ Thuật
* Nắm vững ưu nhược điểm của mô hình phân tách 3 thành phần (STT - LLM - TTS) khi giải quyết bài toán ngôn ngữ phức tạp.
* Hiểu rõ vai trò bắt buộc của cơ chế streaming và khả năng gọi hàm (Tool calling) để xây dựng một Agent chủ động tương tác thời gian thực.

#### Ứng Dụng Vào Công Việc
* **Nâng cấp trải nghiệm ứng dụng:** Vận dụng tư duy chia tách kiến trúc STT-LLM-TTS để nghiên cứu tích hợp trợ lý điều khiển bằng giọng nói vào các ứng dụng di động đa nền tảng, hỗ trợ tương tác rảnh tay cho người dùng.
* **Tối ưu hóa hạ tầng:** Ứng dụng nguyên lý xử lý streaming và tối ưu hóa độ trễ vào việc phát triển hệ thống trên môi trường AWS nhằm xây dựng các luồng tự động hóa tốc độ cao và chịu tải tốt.

### Trải nghiệm trong event

#### Học hỏi từ chuyên gia thực chiến
* Các chia sẻ thực tế mang lại góc nhìn sâu sắc về khoảng cách từ một bản demo AI đơn giản đến một hệ thống Voice Agent vận hành quy mô lớn phục vụ hàng triệu khách hàng trong ngành ngân hàng.

#### Trải nghiệm kỹ thuật thực tế
* Trực tiếp quan sát live demo mượt mà của hệ thống Voice Agent và cách các kỹ sư giải quyết các bài toán hóc búa về xử lý âm thanh tiếng Việt.
* Mở rộng hiểu biết về sức mạnh của Generative AI khi kết hợp gọi hàm (Tool calling) để tự động hóa các quy trình nghiệp vụ phức tạp.

### Bài học rút ra
* Kiến trúc phân tách (STT - LLM - TTS) kết hợp khả năng gọi hàm (Tool Calling) là giải pháp tối ưu để giải quyết rào cản ngôn ngữ tiếng Việt và nâng cao độ chính xác cho AI.
* Trải nghiệm giao tiếp thấu hiểu ngữ cảnh và cơ chế dự phòng chuyển giao cho con người là yếu tố sống còn để đưa Voice AI vào vận hành thực tế thành công.

> Tổng thể, sự kiện mang đến góc nhìn toàn diện về công nghệ Voice AI, giúp tôi học hỏi thêm nhiều kiến thức thực tế để áp dụng vào các dự án hiện tại và tương lai.
