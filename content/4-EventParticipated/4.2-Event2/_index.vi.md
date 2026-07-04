---
title: "Event 2"
date: 2026-05-09
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---
# BÀI THU HOẠCH: AUTOMATED PROMPT ENGINEERING - ENHANCING LLM OUTPUT QUALITY

### Mục Đích Của Sự Kiện

* Chia sẻ nghệ thuật giao tiếp với AI (The Art of Communicating with AI) và các phương pháp tối ưu chất lượng đầu ra của các Mô hình Ngôn ngữ Lớn (LLM).
* Chỉ ra tầm quan trọng của việc thiết kế câu lệnh (Prompt Engineering) và tác hại của việc đặt câu lệnh sai cách.
* Cung cấp các cấu trúc, nguyên tắc và kỹ thuật từ cơ bản đến nâng cao để tương tác hiệu quả với LLM.
* Giới thiệu giải pháp "Proptimizer" - tự động hóa việc tối ưu prompt cùng kiến trúc hệ thống Serverless trên đám mây AWS.

### Danh Sách Diễn Giả

* **Nguyễn Tuấn Thịnh** - DevOps/Cloud Engineer, thành viên dự án First Cloud AI Journey.

### Nội Dung Nổi Bật

#### 1. Ảnh hưởng tiêu cực của câu lệnh (Prompt) kém chất lượng
* **Kết quả chung chung:** Sử dụng câu lệnh chung chung (Generic prompt) sẽ chỉ nhận về câu trả lời chung chung từ AI.
* **Tốn kém chi phí:** Sự lãng phí token do viết prompt dài dòng, thiếu tối ưu sẽ làm tăng chi phí vận hành.
* **Thiếu tính nhất quán:** Các chỉ dẫn mơ hồ khiến AI trả về kết quả không đồng nhất giữa các lần chạy.
* **Giảm năng suất:** Việc giao tiếp kém hiệu quả với AI gây lãng phí thời gian sửa đổi câu trả lời.

#### 2. Cấu trúc của một câu lệnh chuẩn (Great Prompt)
Một câu lệnh chất lượng cần bao gồm các thành phần cốt lõi sau:
* **Role:** Vai trò AI cần đóng vai (ví dụ: Chuyên gia tư vấn nghề nghiệp).
* **Instruction:** Nhiệm vụ cụ thể cần AI thực hiện.
* **Context:** Thông tin ngữ cảnh nền tảng.
* **Input Data:** Dữ liệu đầu vào cần xử lý.
* **Output Format:** Định dạng, cấu trúc và văn phong đầu ra.
* **Examples:** Các ví dụ mẫu (kỹ thuật few-shot) để AI định hình kết quả.
* **Constraints/Guidelines:** Các giới hạn (độ dài, những từ cần tránh, việc tập trung...).

#### 3. Kinh tế học Token (Token Economics)
* **Khái niệm Token:** LLM xử lý văn bản dựa trên token (đơn vị nhỏ hơn từ). Số lượng token tiêu thụ khác nhau tùy theo ngôn ngữ (tiếng Việt thường tốn nhiều token hơn tiếng Anh).
* **Chênh lệch chi phí:** Chi phí cho token đầu vào (Input) rẻ hơn nhiều so với token đầu ra (Output). (Ví dụ: token đầu vào khoảng \$5/1 triệu tokens, trong khi token đầu ra lên tới \$25/1 triệu tokens).

#### 4. Các kỹ thuật Prompt nâng cao (Advanced Techniques)
* **Chain-of-Thought (CoT):** Hướng dẫn AI suy luận và giải thích từng bước logic trước khi đưa ra kết quả.
* **Self-Consistency:** Kết hợp CoT để AI chạy nhiều luồng suy nghĩ độc lập và chọn ra đáp án phổ biến, hợp lý nhất.
* **Tree-of-Thoughts (ToT):** Suy luận dưới dạng cây quyết định để đánh giá nhiều hướng đi tối ưu.
* Các kỹ thuật **Retrieval-Augmented Generation (RAG)** và **Role Prompting**.

#### 5. Kiến trúc hệ thống tối ưu hóa (Proptimizer Architecture)
Giải pháp Proptimizer là một tiện ích mở rộng trình duyệt giúp tối ưu hóa prompt, được xây dựng 100% Serverless trên AWS với chi phí hạ tầng cực kỳ tối ưu:
* **Frontend & Phân phối:** Sử dụng AWS CloudFront (CDN) kết hợp Amazon S3 để lưu trữ và phân phối nội dung tĩnh.
* **Xác thực & API:** Amazon Cognito quản lý định danh người dùng; Amazon API Gateway điều phối traffic và AWS Lambda xử lý các tác vụ logic mà không cần duy trì máy chủ.
* **Tích hợp AI & Dữ liệu:** Kết nối với các mô hình AI (Claude, GPT) qua Amazon Bedrock, đồng thời lưu trữ lịch sử prompt tốc độ cao bằng Amazon DynamoDB.
* **Giám sát:** Quản lý log và hiệu năng bằng Amazon CloudWatch.

### Những Gì Học Được

#### Tư Duy Thiết Kế
* **Tập trung vào chỉ dẫn tích cực:** Cần mô tả cụ thể những việc **NÊN LÀM (DOs)** thay vì chỉ cấm đoán các việc KHÔNG NÊN (DON'Ts).
* **Tối ưu hóa cấu trúc:** Sử dụng các dấu phân cách (Delimiters) để phân tách rõ ràng các phần của prompt và chia nhỏ văn bản đầu vào dài để AI dễ tiếp nhận.
* **Chống bịa đặt (Hallucination):** Cho phép AI trả lời "Tôi không biết" nếu không tìm thấy dữ liệu chính xác, và tránh giao các phép tính toán học phức tạp trực tiếp cho LLM thuần túy.

#### Kiến Trúc Kỹ Thuật
* Hiểu sâu cơ chế tích hợp công nghệ hạ tầng AWS với ứng dụng Generative AI.
* Biết cách sử dụng **Amazon Bedrock** làm cổng kết nối API bảo mật tới nhiều mô hình ngôn ngữ lớn khác nhau.
* Cách thiết kế cơ sở dữ liệu **NoSQL DynamoDB** tối ưu cho các truy vấn lưu trữ lịch sử prompt với độ trễ phần nghìn giây.

#### Ứng Dụng Vào Công Việc
* **Áp dụng cấu trúc Prompt 7 thành phần** vào các hoạt động tương tác hàng ngày với AI để tối đa hóa độ chính xác.
* **Tối ưu chi phí token:** Tinh gọn các câu chỉ thị, loại bỏ từ thừa để giảm lượng token tiêu thụ.
* **Ứng dụng Advanced Prompting:** Áp dụng Chain-of-Thought (CoT) vào các tác vụ cần phân tích logic và review mã nguồn.
* **Thiết kế ứng dụng Serverless:** Lên ý tưởng phát triển các tool hỗ trợ nội bộ dựa trên kiến trúc Serverless (S3, Lambda, Bedrock).

### Trải nghiệm trong event

#### Học hỏi từ chuyên gia
* Bài thuyết trình mang lại góc nhìn kết hợp thực tế giữa lập trình ứng dụng (engineering) và giao tiếp với mô hình AI (prompting), giúp hiểu rõ quy trình phát triển sản phẩm tích hợp GenAI.

#### Trải nghiệm trực quan
* Hình dung rõ nét cơ chế suy luận của các kỹ thuật nâng cao thông qua sơ đồ trực quan của CoT và ToT.
* Tiếp thu luồng kiến trúc thực tế của giải pháp Proptimizer trên môi trường AWS.

#### Bài học rút ra
* Kỹ năng Prompt Engineering là chìa khóa mở ra tiềm năng thực tế của AI; tối ưu hóa câu lệnh giúp tối đa hóa hiệu suất làm việc.
* Kiến trúc Serverless trên AWS là lựa chọn hoàn hảo cho các dự án AI nhỏ và vừa nhờ chi phí khởi tạo thấp (\$0) và khả năng tự động mở rộng linh hoạt.

> Tổng thể, sự kiện giúp tôi nâng cao đáng kể kỹ năng thiết kế câu lệnh và mở rộng tư duy xây dựng các sản phẩm AI hiệu quả trên nền tảng đám mây.
