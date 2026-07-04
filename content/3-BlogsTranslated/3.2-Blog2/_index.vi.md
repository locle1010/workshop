---
title: "Blog 2"
date: 2026-04-26
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---
# Kết hợp Amazon Cognito và Amazon Verified Permissions để triển khai Fine-grained Access Control

> **Bài viết gốc:** [AWS Study Group - FCJ](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2185975575500736)

![Amazon Cognito & Amazon Verified Permissions](/images/3-BlogsTranslated/Blog2_photo.jpg)

Chào anh em AWS Study Group VN!

Trong quá trình tìm hiểu về bảo mật cho các ứng dụng B2C, mình có đọc được một bài viết khá hay từ AWS Security Blog về cách kết hợp Amazon Cognito và Amazon Verified Permissions để triển khai cơ chế phân quyền chi tiết (fine-grained access control) cho ứng dụng.

Thông thường khi xây dựng ứng dụng, chúng ta thường phải giải quyết hai bài toán riêng biệt:
*   **Authentication (Xác thực):** Người dùng là ai?
*   **Authorization (Phân quyền):** Người dùng được phép làm gì?

Amazon Cognito từ lâu đã là dịch vụ quen thuộc để quản lý đăng ký, đăng nhập và xác thực người dùng. Tuy nhiên, khi hệ thống ngày càng có nhiều vai trò người dùng, nhiều loại tài nguyên và các quy tắc truy cập phức tạp hơn, việc xử lý toàn bộ logic phân quyền ngay trong source code có thể khiến ứng dụng khó bảo trì và khó mở rộng về sau.

Đó là lý do AWS giới thiệu **Amazon Verified Permissions** - dịch vụ quản lý quyền truy cập tập trung dựa trên ngôn ngữ chính sách Cedar. Thay vì viết các điều kiện phân quyền rải rác trong code, chúng ta có thể định nghĩa các policy riêng biệt và để Verified Permissions chịu trách nhiệm đánh giá quyền truy cập.

Trong ví dụ được AWS trình bày, Cognito đảm nhiệm việc xác thực người dùng và phát hành JWT Token. Sau khi người dùng đăng nhập thành công, ứng dụng sẽ gửi thông tin người dùng cùng yêu cầu truy cập tới Amazon Verified Permissions. Dịch vụ này sẽ dựa trên các Cedar Policy để quyết định người dùng có được phép thực hiện hành động đó hay không.

### Một số mô hình phân quyền phổ biến được giới thiệu:
*   **Resource Ownership:** Người dùng chỉ được thao tác trên dữ liệu do mình sở hữu.
*   **Role-Based Access Control (RBAC):** Phân quyền dựa trên vai trò như Student, Faculty, Admin,...
*   **Hierarchical Permissions:** Kế thừa quyền theo cấp bậc tổ chức.
*   **Administrative Override:** Quản trị viên có quyền ghi đè các quy tắc thông thường.
*   **Explicit Deny:** Các chính sách từ chối luôn được ưu tiên cao nhất.

Điểm mình thấy khá thú vị là AWS minh họa bằng một hệ thống học thuật với các vai trò như sinh viên, trợ giảng, giảng viên, trưởng khoa và quản trị viên. Mỗi vai trò có phạm vi truy cập khác nhau nhưng toàn bộ logic phân quyền đều được quản lý tập trung thông qua Verified Permissions thay vì hard-code trực tiếp trong ứng dụng.

Sau khi đọc bài viết, mình thấy điểm đáng chú ý nhất là cách AWS tách riêng phần xác thực và phân quyền thành hai lớp độc lập. Với những hệ thống có nhiều nhóm người dùng hoặc nhiều quy tắc truy cập khác nhau, cách tiếp cận này giúp việc quản lý quyền trở nên rõ ràng và linh hoạt hơn rất nhiều.

### Một số lợi ích nổi bật có thể kể đến như:
*   Tách biệt hoàn toàn Authentication và Authorization.
*   Giảm đáng kể lượng code phân quyền trong ứng dụng.
*   Dễ dàng thay đổi quy tắc truy cập mà không cần sửa logic nghiệp vụ.
*   Tăng khả năng kiểm toán và quản trị quyền truy cập.
*   Phù hợp với các hệ thống SaaS hoặc B2C có số lượng người dùng lớn và nhiều cấp quyền phức tạp.

Theo mình, Amazon Cognito và Amazon Verified Permissions đang bổ sung cho nhau khá tốt trong việc giải quyết bài toán xác thực và phân quyền. Đây là một hướng tiếp cận đáng tham khảo khi xây dựng các ứng dụng B2C cần kiểm soát quyền truy cập chi tiết, đồng thời vẫn đảm bảo khả năng mở rộng khi hệ thống phát triển trong tương lai.

**Nguồn tham khảo:** <https://aws.amazon.com/blogs/security/building-secure-b2c-applications-with-fine-grained-access-control-using-amazon-cognito-and-amazon-verified-permissions/>
