---
title: "Event 2"
date: 2026-05-09
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---
# BÀI THU HOẠCH: AUTOMATED PROMPT ENGINEERING - ENHANCING LLM OUTPUT QUALITY

### 1. Buổi chia sẻ tập trung vào điều gì?

*   **Làm chủ kỹ thuật giao tiếp**: Khai phá nghệ thuật ra lệnh cho AI (Prompting) và những giải pháp cải thiện chất lượng dữ liệu phản hồi từ các mô hình LLM.
*   **Phân tích lỗi sai**: Chỉ rõ tầm quan trọng của việc xây dựng câu lệnh và những tác động không tốt khi thiết lập prompt sơ sài, thiếu khoa học.
*   **Trang bị kỹ năng**: Cung cấp bộ khung cấu trúc và kỹ thuật viết prompt từ cơ bản đến nâng cao để tối ưu hóa tương tác với AI.
*   **Giới thiệu ứng dụng**: Giới thiệu công cụ tự động hóa tối ưu prompt mang tên "Proptimizer" cùng thiết kế hệ thống Serverless trên cloud AWS.

### 2. Diễn giả đồng hành

*   **Người chia sẻ**: **Nguyễn Tuấn Thịnh** (DevOps/Cloud Engineer thuộc dự án First Cloud AI Journey).

### 3. Tóm tắt các chủ đề cốt lõi

#### A. Những tổn thất khi sử dụng Prompt kém chất lượng
*   **Nội dung hời hợt**: Gửi đi câu lệnh chung chung (Generic prompt) sẽ chỉ nhận lại câu trả lời nông và thiếu giá trị thực tiễn.
*   **Lãng phí tài chính**: Sử dụng prompt dài dòng, lặp ý gây hao phí token không cần thiết, làm tăng hóa đơn chi phí API.
*   **Kết quả bất ổn định**: Prompt thiếu rõ ràng khiến AI phản hồi không đồng nhất trong những lần chạy khác nhau.
*   **Tổn hao thời gian**: Mất nhiều thời gian để chỉnh sửa thủ công và yêu cầu AI sinh lại kết quả, làm giảm đáng kể hiệu suất làm việc.

#### B. Cấu trúc chuẩn hóa của một câu lệnh (Great Prompt)
Một prompt chất lượng cao cần được tổ chức chặt chẽ theo bộ khung gồm:
1.  **Role (Vai trò)**: Thiết lập tư cách cho AI (ví dụ: Chuyên gia phân tích bảo mật).
2.  **Instruction (Chỉ thị)**: Mô tả chính xác việc AI cần thực hiện.
3.  **Context (Ngữ cảnh)**: Thông tin nền giúp AI hiểu sâu hơn về bối cảnh tác vụ.
4.  **Input Data (Dữ liệu đầu vào)**: Nội dung thô cần được xử lý.
5.  **Output Format (Định dạng đầu ra)**: Yêu cầu về cấu trúc trình bày (JSON, Markdown, Bảng...).
6.  **Examples (Ví dụ mẫu)**: Đưa ra các mẫu few-shot để AI học theo phong cách mong muốn.
7.  **Constraints (Ràng buộc)**: Các quy tắc cần tuân thủ (ví dụ: độ dài tối đa, những điều không được phép làm).

#### C. Bài toán kinh tế Token (Token Economics)
*   **Định nghĩa**: LLM không xử lý ký tự trực tiếp mà chia nhỏ văn bản thành các token. Đáng chú ý, các ngôn ngữ có dấu như tiếng Việt sẽ tốn nhiều token hơn đáng kể so với tiếng Anh.
*   **Cơ cấu chi phí**: Chi phí xử lý token đầu vào (Input) thường rẻ hơn rất nhiều so với token đầu ra (Output), nên việc tối ưu hóa nội dung trả về là cực kỳ cần thiết để kiểm soát chi phí.

#### D. Các phương pháp Prompting chuyên sâu
*   **Chain-of-Thought (CoT)**: Kích thích AI giải thích tiến trình suy luận logic từng bước trước khi đưa ra kết quả cuối cùng.
*   **Self-Consistency**: Cho LLM chạy nhiều luồng suy nghĩ độc lập song song để so sánh và lựa chọn kết quả có độ đồng thuận cao nhất.
*   **Tree-of-Thoughts (ToT)**: Xây dựng cấu trúc lập luận dạng cây để đánh giá và lựa chọn các nhánh phân tích tối ưu.
*   Ứng dụng kỹ thuật **RAG** (Retrieval-Augmented Generation) và **Role Prompting**.

#### E. Thiết kế kiến trúc giải pháp Proptimizer
Proptimizer là tiện ích mở rộng trên trình duyệt giúp tối ưu hóa prompt tự động, sử dụng toàn bộ mô hình Serverless trên AWS để tiết kiệm tối đa chi phí vận hành:
*   **Phân phối tĩnh**: Sử dụng Amazon S3 lưu trữ Frontend kết hợp Amazon CloudFront làm CDN phân phối toàn cầu.
*   **Điều phối API**: Dùng Amazon Cognito để quản lý người dùng, Amazon API Gateway định tuyến và AWS Lambda chạy xử lý backend không cần duy trì máy chủ.
*   **Xử lý AI & Lưu trữ**: Gọi các mô hình LLM (Claude, GPT) thông qua Amazon Bedrock, lưu trữ lịch sử prompt truy vấn nhanh với DynamoDB.
*   **Theo dõi hệ thống**: Sử dụng Amazon CloudWatch để ghi nhận log và giám sát hiệu năng.

### 4. Đúc kết bài học & Hướng triển khai thực tế

#### Tư duy thiết kế câu lệnh
*   Nên tập trung vào các chỉ thị mang tính khẳng định (DOs) để hướng dẫn AI làm gì, thay vì chỉ sử dụng các câu phủ định (DON'Ts) chung chung.
*   Tối ưu hóa cấu trúc prompt bằng cách sử dụng các ký tự phân tách rõ ràng (như `---`, `###`) và chia nhỏ các tài liệu đầu vào quá dài.
*   Hạn chế ảo giác (Hallucination) bằng cách cho phép AI báo cáo "Tôi không biết" nếu thông tin bị thiếu hụt.

#### Kỹ thuật hệ thống đám mây
*   Hiểu rõ phương pháp tích hợp hạ tầng AWS Serverless với các mô hình Generative AI.
*   Vận dụng **Amazon Bedrock** làm cổng kết nối tập trung, an toàn để quản lý API đến nhiều nhà cung cấp mô hình khác nhau.
*   Thiết kế bảng **DynamoDB** tối ưu cho các truy vấn lịch sử prompt với độ trễ siêu thấp dưới 10ms.

#### Kế hoạch hành động tại nơi làm việc
*   **Áp dụng cấu trúc 7 thành phần** vào công việc tương tác hàng ngày với AI để có câu trả lời chất lượng nhất.
*   **Giảm chi phí vận hành**: Tinh giản tối đa câu chữ của prompt để tiết kiệm lượng token tiêu thụ.
*   **Ứng dụng kỹ thuật CoT** trong việc phân tích mã nguồn và kiểm tra logic code.
*   **Thiết kế ứng dụng nội bộ**: Lên kế hoạch xây dựng các tool tự động hóa nội bộ trên nền tảng Serverless (S3, Lambda, Bedrock).

### 5. Cảm nhận và Trải nghiệm thực tế

#### Góc nhìn từ kỹ sư chia sẻ
*   Bài nói chuyện mang lại cái nhìn toàn diện khi kết hợp khéo léo giữa tư duy lập trình hệ thống (Engineering) và nghệ thuật giao tiếp với mô hình AI (Prompting), giúp tôi hiểu rõ quy trình phát triển một sản phẩm GenAI thực tế.

#### Trải nghiệm trực quan
*   Hình dung rõ nét cách hoạt động của CoT và ToT thông qua các sơ đồ tư duy trực quan.
*   Nắm bắt được sơ đồ kiến trúc thực tế của giải pháp Proptimizer chạy trên nền tảng đám mây AWS.

#### Bài học tâm đắc
*   Prompt Engineering không đơn thuần là đặt câu hỏi, mà là chiếc chìa khóa tối ưu hóa sức mạnh của AI trong công việc.
*   Mô hình Serverless trên AWS là lựa chọn cực kỳ lý tưởng cho các giải pháp tích hợp AI quy mô nhỏ nhờ chi phí ban đầu gần như bằng 0 và khả năng tự động mở rộng mượt mà.

> Sự kiện đã mang lại cho tôi những kiến thức vô cùng bổ ích, giúp nâng cao kỹ năng thiết kế câu lệnh và mở ra tư duy xây dựng các sản phẩm AI hiệu quả trên cloud.
