---
title: "Event 1"
date: 2026-05-23
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---
# BÀI THU HOẠCH: TỰ ĐỘNG HÓA CÔNG VIỆC VỚI TRỢ LÝ AI AMAZON Q VÀ MCP

### Mục Đích Của Sự Kiện

* Giới thiệu **Amazon Q**, một trợ lý AI dành cho người dùng cuối (end-user) do AWS phát triển.
* Giải quyết bài toán tối ưu hóa thời gian trong quá trình vận hành và lập báo cáo của doanh nghiệp.
* Làm rõ cơ chế hoạt động của Agent và cách sử dụng giao thức MCP (Model Context Protocol) để giúp AI tương tác trực tiếp với các ứng dụng bên ngoài.
* Truyền cảm hứng cho các lập trình viên về tư duy phát triển sản phẩm thực chiến, hướng tới việc giải quyết vấn đề thực tế của khách hàng.

### Danh Sách Diễn Giả

* **Hải An** - Cloud Consultant tại C Pacific Việt Nam.

### Nội Dung Nổi Bật

#### 1. Triết lý lấy người dùng làm trung tâm (User-centric)
* Trình độ kỹ thuật chỉ là công cụ; yếu tố cốt lõi để tạo ra sản phẩm thành công là phải giải quyết triệt để vấn đề thực tế của người dùng.
* Ứng dụng AI giúp tự động hóa quá trình tổng hợp dữ liệu và lập báo cáo hàng tuần, giúp tiết kiệm thời gian đáng kể cho các cấp quản lý.

#### 2. Hệ sinh thái tích hợp của Amazon Q
* AWS xây dựng nền tảng Agent tích hợp sâu với các hệ sinh thái doanh nghiệp phổ biến như **Microsoft** (Word, Outlook, Teams, PowerPoint) và **Google** (Gmail, Calendar).

#### 3. Khái niệm Agent và MCP (Model Context Protocol)
* Các mô hình ngôn ngữ lớn (LLM) tuy thông minh nhưng bản thân chúng không thể tự thực thi hành động (ví dụ: tự đặt lịch hẹn hay gửi email).
* Để AI tương tác được với thế giới thực, hệ thống cần các hàm thực thi (Action/Function). Giao thức **MCP đóng vai trò như "những cánh tay nối dài"** kết nối AI với các nguồn dữ liệu và công cụ như Gmail, Jira, hay Confluence.

#### 4. Tự động hóa qua các luồng Demo thực tế
* **Tạo Dashboard phân tích:** Người dùng không cần kiến thức phân tích dữ liệu chuyên sâu (BI) vẫn có thể tải một file dữ liệu thô (Excel) lên hệ thống để Amazon Q tự động phân tích và trực quan hóa thành biểu đồ.
* **Tóm tắt cuộc họp:** AI tự động chuyển đổi giọng nói thành văn bản, tóm tắt các quyết định quan trọng trong cuộc họp và kích hoạt MCP để tự động gửi email báo cáo các bước tiếp theo (next steps) cho người tham gia.

#### 5. Tuân thủ Bảo mật
* Nền tảng hoạt động dựa trên Mô hình Trách nhiệm Chia sẻ (Shared Responsibility Model) của AWS: AWS chịu trách nhiệm bảo mật hạ tầng và các mô hình nền tảng, trong khi người dùng kiểm soát dữ liệu và phân quyền ứng dụng của mình.

### Những Gì Học Được

#### Tư Duy Thiết Kế
* Sản phẩm công nghệ phải bắt đầu từ nhu cầu thiết thực và gần gũi của người dùng.
* Thiết kế hệ thống AI cần vượt qua giới hạn của mô hình "hỏi - đáp" thuần túy, hướng tới xây dựng các "Agent" tự hành có khả năng mang lại giá trị vận hành trực tiếp.

#### Kiến Trúc Kỹ Thuật
* Nắm vững kiến thức cốt lõi: **Agent = LLM + Các dịch vụ tính toán (Action/Function/MCP)**.
* Hiểu cách thức Amazon Q kết hợp khả năng xử lý ngôn ngữ tự nhiên của AI với các API của bên thứ ba để tạo ra một luồng tự động hóa hoàn chỉnh.

#### Ứng Dụng Vào Công Việc
* **Nâng cao hiệu suất cá nhân:** Sử dụng Amazon Q để xử lý nhanh các tác vụ phân tích dữ liệu thô thành báo cáo trực quan mà không cần tốn thời gian thiết lập các công cụ phức tạp.
* **Tự động hóa quy trình quản trị:** Nghiên cứu phát triển các module MCP nội bộ để tích hợp trợ lý AI với các công cụ quản lý công việc (như Jira, Microsoft Teams) nhằm tự động hóa luồng theo dõi công việc và gửi nhắc nhở sau họp.

### Trải nghiệm trong event

#### Học hỏi từ các diễn giả thực chiến
* Bài chia sẻ của diễn giả Hải An mang tới nguồn cảm hứng lớn. Anh nhấn mạnh rằng sự khác biệt tạo nên thành công nằm ở sự tự tin và tinh thần phối hợp đồng đội để giải quyết bài toán của khách hàng.

#### Trải nghiệm kỹ thuật thực tế
* Được trực tiếp quan sát cách hệ thống phân rã yêu cầu tự nhiên thành một "system prompt" cấu trúc hóa để AI hiểu chính xác nhiệm vụ.
* Trực quan hóa sức mạnh của giao thức MCP trong việc kết nối và tương tác thời gian thực với các ứng dụng bên ngoài.

#### Ứng dụng công cụ hiện đại
* Tìm hiểu sâu về nền tảng Amazon Q và mở ra tư duy tự xây dựng các MCP server tùy biến để mở rộng khả năng của AI theo nhu cầu cụ thể của dự án.

> Tổng thể, sự kiện không chỉ cung cấp kiến thức kỹ thuật hữu ích mà còn giúp tôi thay đổi tư duy về thiết kế ứng dụng, hiện đại hóa hệ thống và cộng tác hiệu quả hơn trong đội ngũ.
