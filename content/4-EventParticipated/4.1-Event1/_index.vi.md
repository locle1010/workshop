---
title: "Event 1"
date: 2026-05-23
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---
# BÀI THU HOẠCH: TỰ ĐỘNG HÓA CÔNG VIỆC VỚI TRỢ LÝ AI AMAZON Q VÀ MCP

### I. Mục tiêu & Định hướng Sự kiện

*   **Giới thiệu giải pháp**: Phổ biến về trợ lý AI **Amazon Q** - công cụ đắc lực do AWS phát triển nhằm hỗ trợ tối đa cho nhóm người dùng cuối (end-user).
*   **Tối ưu hiệu suất**: Đưa ra lời giải cho bài toán tiết kiệm thời gian, tự động hóa quy trình báo cáo và vận hành doanh nghiệp.
*   **Làm rõ kỹ thuật**: Phân tích sâu cơ chế hoạt động của mô hình Agent, đồng thời giới thiệu giao thức Model Context Protocol (MCP) giúp AI kết nối trực tiếp với ứng dụng ngoại vi.
*   **Định hình tư duy**: Khơi dậy tinh thần "product-driven" cho các lập trình viên, hướng tới việc xây dựng các giải pháp giải quyết trực tiếp nhu cầu khách hàng.

### II. Chuyên gia chia sẻ

*   **Diễn giả**: **Hải An** (Cloud Consultant tại C Pacific Việt Nam).

### III. Điểm nhấn nội dung chuyên môn

#### 1. Nguyên lý thiết kế lấy người dùng làm trung tâm (User-centric)
*   Công nghệ chỉ đóng vai trò là phương tiện hỗ trợ; giá trị cốt lõi của một sản phẩm nằm ở khả năng giải quyết triệt để nỗi đau (pain point) của người dùng.
*   Việc ứng dụng AI để tự động hóa khâu xử lý dữ liệu và tạo báo cáo tuần giúp giảm tải công việc cực kỳ hiệu quả cho đội ngũ quản lý.

#### 2. Khả năng tích hợp của Amazon Q
*   Hệ thống Agent của AWS được thiết kế để liên kết sâu rộng với các nền tảng doanh nghiệp quen thuộc từ **Microsoft** (Word, Teams, Outlook, PowerPoint) cho đến **Google** (Gmail, Calendar).

#### 3. Bản chất của Agent và Giao thức MCP
*   Các mô hình LLM thông minh nhưng bị giới hạn ở khả năng thực thi tác vụ trực tiếp ngoài đời thực (như gửi email, đặt lịch).
*   Giao thức **MCP hoạt động như chiếc cầu nối kỹ thuật**, cho phép AI tương tác và truy xuất dữ liệu trực tiếp từ các hệ thống thứ ba như Jira, Confluence hay Gmail thông qua các hàm hành động (Actions).

#### 4. Thực chứng qua Demo thực tế
*   **Trực quan hóa dữ liệu tự động**: Người dùng chỉ cần tải tệp dữ liệu thô (Excel) lên để Amazon Q tự động xử lý và tạo biểu đồ trực quan mà không cần viết lệnh phân tích phức tạp.
*   **Tự động hóa sau cuộc họp**: AI ghi âm, chuyển đổi giọng nói thành văn bản, tóm tắt nội dung cuộc họp và tự động gửi email phân công công việc tiếp theo đến từng người tham gia thông qua các kết nối MCP.

#### 5. Cơ chế Bảo mật
*   Tuân thủ nghiêm ngặt mô hình Trách nhiệm Chia sẻ (Shared Responsibility Model) của AWS: AWS đảm bảo an toàn cho hạ tầng đám mây và các mô hình nền tảng, còn khách hàng quản lý và phân quyền đối với dữ liệu của mình.

### IV. Kiến thức tiếp thu và Khả năng ứng dụng

#### Tư duy phát triển
*   Sản phẩm công nghệ phải bắt nguồn từ những nhu cầu thực tế và cấp thiết của người dùng.
*   Cần chuyển dịch tư duy thiết kế AI từ mô hình chat hỏi-đáp thông thường sang các Agent tự hành có tính thực thi cao.

#### Kiến thức hạ tầng
*   Nắm vững công thức: **Agent = LLM + Các Action/Function thực thi trên môi trường máy chủ (MCP)**.
*   Hiểu rõ luồng xử lý tích hợp giữa AI và API bên ngoài để xây dựng các kịch bản tự động hóa đầu cuối.

#### Định hướng công việc
*   **Tối ưu hiệu suất cá nhân**: Sử dụng Amazon Q để nhanh chóng phân tích các số liệu thô và xuất báo cáo trực quan mà không cần setup phức tạp.
*   **Xây dựng giải pháp tự động hóa**: Nghiên cứu xây dựng các MCP Server tùy biến để liên kết AI với các công cụ quản lý dự án nội bộ (Jira, Teams), tự động hóa việc theo dõi công việc sau các buổi họp.

### V. Góc nhìn thực tế tại sự kiện

#### Học hỏi từ diễn giả thực chiến
*   Phần chia sẻ đầy nhiệt huyết của anh Hải An đã truyền cảm hứng mạnh mẽ về tinh thần làm việc nhóm và sự tự tin khi giải quyết các bài toán kỹ thuật phức tạp của khách hàng.

#### Trải nghiệm kỹ thuật thực tế
*   Quan sát trực tiếp quá trình hệ thống dịch các yêu cầu tự nhiên thành cấu trúc prompt cụ thể giúp AI thực thi đúng nhiệm vụ.
*   Nhìn nhận trực quan sức mạnh của MCP khi kết nối và tương tác tức thì với các ứng dụng ngoại vi.

#### Ứng dụng công nghệ mới
*   Hiểu rõ cách thức hoạt động của Amazon Q, từ đó mở rộng tư duy thiết kế các máy chủ MCP tùy chỉnh nhằm mở rộng tính năng của AI theo đặc thù của từng dự án.

> Nhìn chung, sự kiện mang lại giá trị lớn cả về mặt kiến thức kỹ thuật lẫn tư duy thiết kế hệ thống, giúp tôi định hình rõ ràng hơn hướng phát triển các giải pháp tự động hóa thông minh trong tương lai.

![Hình ảnh tham gia sự kiện](/images/4-EventParticipated/event1_picture.jpg)
