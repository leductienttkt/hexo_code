---
title: AWS Well-Architected Framework – Design Principles
categories:
 - Overview
---
### 1. Scalability

* Scaling Horizontally – sự gia tăng số lượng tài nguyên
* Scaling Vertically –  sự gia tăng các thông số kỹ thuật của một tài nguyên riêng lẻ.

### 2. Disposable Resources Instead of Fixed Servers

* Instantiating Compute Resources – tự động thiết lập tài nguyên mới cùng với cấu hình và code của chúng
* Infrastructure as Code – AWS assets có thể lập trình. Bạn có thể áp dụng các kỹ thuật, kinh nghiệm và công cụ từ phát triển phần mềm để làm cho toàn bộ cơ sở hạ tầng của bạn có thể sử dụng lại, có thể bảo trì, mở rộng và kiểm tra được.

### 3. Automation

* Serverless Management and Deployment – Tự động hóa việc triển khai mã của bạn. AWS xử lý các nhiệm vụ quản lý cho bạn.
* Infrastructure Management and Deployment – AWS tự động xử lý các chi tiết, như cung cấp tài nguyên, cân bằng tải, tự động mở rộng và giám sát, do đó bạn có thể tập trung vào triển khai tài nguyên.
* Alarms and Events – Các dịch vụ AWS sẽ liên tục theo dõi tài nguyên của bạn và bắt đầu các sự kiện khi các số liệu hoặc điều kiện nhất định được đáp ứng.

### 4. Loose Coupling

* Well-Defined Interfaces – giảm sự phụ thuộc lẫn nhau trong một hệ thống bằng cách cho phép các thành phần khác nhau chỉ tương tác với nhau thông qua các công nghệ cụ thể, như API RESTful.
* Service Discovery – các ứng dụng được triển khai như một tập hợp các dịch vụ nhỏ hơn sẽ có thể được sử dụng mà không cần biết trước về các chi tiết cấu trúc mạng của chúng. Ngoài việc che giấu sự phức tạp, điều này cũng cho phép các chi tiết cơ sở hạ tầng thay đổi bất cứ lúc nào.
* Asynchronous Integration – các thành phần tương tác không cần phản hồi ngay lập tức.
* Distributed Systems Best Practices

### 5. Services, Not Servers

* Managed Services – cung cấp services mà nhà phát triển có thể sử dụng  cho các ứng dụng của họ, chẳng hạn như cdatabases, machine learning, analytics, queuing, search, email, notifications, v.v.
* Serverless Architectures – cho phép bạn xây dựng cả các dịch vụ theo hướng sự kiện và đồng bộ mà không cần quản lý cơ sở hạ tầng máy chủ, điều này có thể làm giảm độ phức tạp hoạt động của các ứng dụng đang chạy.

### 6. Databases

* Chọn công nghệ cơ sở dữ liệu phù hợp cho từng công việc.
* Relational Databases cung cấp một ngôn ngữ truy vấn mạnh mẽ, khả năng lập chỉ mục linh hoạt, kiểm soát toàn vẹn mạnh mẽ và khả năng kết hợp dữ liệu từ nhiều bảng một cách nhanh chóng và hiệu quả.
* NoSQL Databases trao đổi một số khả năng truy vấn và giao dịch của cơ sở dữ liệu quan hệ để có mô hình dữ liệu linh hoạt hơn, quy mô theo chiều ngang. Nó sử dụng nhiều mô hình dữ liệu, bao gồm biểu đồ, cặp giá trị khóa và tài liệu JSON và được công nhận rộng rãi để dễ phát triển, hiệu suất có thể mở rộng, tính sẵn sàng cao và khả năng phục hồi.
* Data Warehouses là một loại cơ sở dữ liệu quan hệ chuyên biệt, được tối ưu hóa để phân tích và báo cáo một lượng lớn dữ liệu.
* Graph Databases sử dụng cấu trúc biểu đồ cho các truy vấn.

### 7. Managing Increasing Volumes of Data

* Data Lake – một cách tiếp cận kiến trúc cho phép bạn lưu trữ một lượng lớn dữ liệu ở một vị trí trung tâm để nó có sẵn để được phân loại, xử lý, phân tích và tiêu thụ bởi các nhóm khác nhau trong tổ chức của bạn.

### 8. Removing Single Points of Failure

* Introducing Redundancy
   - Standby redundancy – khi tài nguyên bị lỗi, chức năng được phục hồi trên tài nguyên thứ cấp với quá trình chuyển đổi dự phòng. Việc chuyển đổi dự phòng thường yêu cầu một thời gian trước khi hoàn thành và trong giai đoạn này, tài nguyên vẫn không có sẵn. Điều này thường được sử dụng cho các thành phần trạng thái như cơ sở dữ liệu quan hệ.
   - Active redundancy – các yêu cầu được phân phối cho nhiều tài nguyên tính toán dự phòng. Khi một trong số chúng thất bại, phần còn lại sẽ xử lí một phần khối lượng công việc lớn hơn.

* Detect Failure – sử dụng health checks và collect logs
* Durable Data Storage
   - Synchronous replication – chỉ thừa nhận một giao dịch sau khi nó được lưu trữ lâu dài trong cả bộ lưu trữ chính và bản sao của nó. Đó là lý tưởng để bảo vệ tính toàn vẹn của dữ liệu khỏi sự cố của bộ lưu trữ chính.
   - Asynchronous replication – tách bộ lưu trữ chính khỏi các bản sao của nó. Điều này có nghĩa là những thay đổi trên bộ lưu trữ chính không được phản ánh ngay lập tức trên bản sao của nó.
   - Quorum-based replication – kết hợp sao chép đồng bộ và không đồng bộ bằng cách xác định số lượng nút tối thiểu phải tham gia vào thao tác ghi thành công.

* Automated Multi-Data Center Resilience –  sử dụng AWS Regions và Availability Zones (Multi-AZ Principle).
* Fault Isolation and Traditional Horizontal Scaling 

### 9. Optimize for Cost

* Right Sizing – AWS cung cấp một loạt các loại tài nguyên và cấu hình cho nhiều trường hợp sử dụng.
* Elasticity – tiết kiệm tiền với AWS bằng cách tận dụng độ co giãn của nền tảng.
* Take Advantage of the Variety of Purchasing Options.

### 10. Caching

* Application Data Caching – lưu trữ và truy xuất thông tin nhanh, được quản lý từ bộ nhớ cache .
* Edge Caching – phục vụ nội dung theo cơ sở hạ tầng gần hơn với người xem, giúp giảm độ trễ và mang lại tốc độ truyền dữ liệu cao.

### 11. Security

* Use AWS Features for Defense in Depth – bảo mật nhiều cấp độ cơ sở hạ tầng của bạn từ mạng xuống đến ứng dụng và cơ sở dữ liệu.
* Share Security Responsibility with AWS – AWS xử lý bảo mật của đám mây trong khi khách hàng xử lý bảo mật trong đám mây.
* Reduce Privileged Access – thực hiện nguyên tắc kiểm soát đặc quyền tối thiểu.
* Security as Code
* Real-Time Auditing – thực hiện giám sát liên tục và tự động hóa các điều khiển trên AWS để giảm thiểu rủi ro bảo mật


-----
Source:
https://d0.awsstatic.com/whitepapers/AWS_Cloud_Best_Practices.pdf

