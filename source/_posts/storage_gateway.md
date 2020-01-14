---
title: AWS Storage Gateway
categories:
 - Storage Services
---

* Dịch vụ cho phép lưu trữ kết hợp giữa các môi trường tại chỗ và AWS Cloud.
* Nó tích hợp các ứng dụng và quy trình làm việc tại chỗ với dịch vụ lưu trữ đám mây thông qua các giao thức lưu trữ tiêu chuẩn.
* Dịch vụ lưu trữ các tệp dưới dạng đối tượng S3 gốc, lưu trữ trong Amazon Glacier và lưu trữ EBS Snapshots được tạo bởi  Volume Gateway với Amazon EBS.

### Storage Solutions

**1. File Gateway**

* Phần mềm, hoặc gateway, được triển khai vào môi trường tại chỗ của bạn dưới dạng một máy ảo chạy trên trình ảo hóa VMware ESXi hoặc Microsoft Hyper-V.
* File gateway hổ trợ:
    * S3 Standard
    * S3 Standard – Infrequent Access
    * S3 One Zone – IA
* Với File gateway, bạn có thể làm:
    * Bạn có thể lưu trữ và truy xuất tệp trực tiếp bằng giao thức NFS phiên bản 3 hoặc 4.1.
    * Bạn có thể lưu trữ và truy xuất tệp trực tiếp bằng giao thức hệ thống tệp SMB phiên bản 2 và 3.
    * Bạn có thể truy cập dữ liệu của mình trực tiếp trong S3 từ bất kỳ ứng dụng hoặc dịch vụ AWS Cloud nào.
    * Bạn có thể quản lý dữ liệu S3 của mình bằng các chính sách vòng đời, sao chép cross-region và phiên bản.
* File Gateway hiện hỗ trợ  Amazon S3 Object Lock, cho phép các hệ thống dựa trên write-once-read-many (WORM) để lưu trữ và truy cập các đối tượng trong Amazon S3.
* Bất kỳ sửa đổi nào, chẳng hạn như chỉnh sửa tập tin, xóa hoặc đổi tên từ các máy khách cổng NFS hoặc SMB của gateway được lưu trữ dưới dạng các phiên bản mới của đối tượng, mà không ghi đè hoặc xóa các phiên bản trước đó.

![](/images/AWSStoragegateway-1.jpg)

**2. Volume Gateway**

* **Cached volumes** – bạn lưu trữ dữ liệu của mình trong S3 và giữ lại một bản sao của các tập hợp dữ liệu thường xuyên truy cập cục bộ. Các khối được lưu trong bộ nhớ cache có thể có kích thước từ 1 GiB đến 32 TiB và phải được làm tròn đến GiB gần nhất. Mỗi gateway được định cấu hình cho các cached volumes có thể hỗ trợ tối đa 32 volumes.

![](/images/AWSStoragegateway-2.jpg)

* **Stored volumes**  - nếu bạn cần truy cập độ trễ thấp vào toàn bộ tập dữ liệu của mình, trước tiên hãy cấu hình gateway tại chỗ của bạn để lưu trữ tất cả dữ liệu của bạn cục bộ. Sau đó, sao lưu không đồng bộ các snapshots theo thời gian của dữ liệu này sang S3. Khối lượng được lưu trữ có thể có kích thước từ 1 GiB đến 16 TiB và phải được làm tròn đến GiB gần nhất. Mỗi gateway được cấu hình cho các stored volumes có thể hỗ trợ tối đa 32 volumes.

![](/images/AWSStoragegateway-3.jpg)

**3. Tape Gateway**  - lưu trữ dữ liệu sao lưu trong Amazon Glacier.

### File Gateway File Share
* Bạn có thể tạo NFS hoặc SMB file shares bằng Bảng điều khiển quản lý AWS hoặc API dịch vụ.
* Sau khi file gateway của bạn được kích hoạt và chạy, bạn có thể thêm file shares bổ sung và cấp quyền truy cập vào S3 buckets.
* Bạn có thể sử dụngfile shares để truy cập các objects trong S3 bucket thuộc tài khoản AWS khác.
* Dịch vụ AWS Storage Gateway đã thêm Access Control Lists (ACL) vào Server Message Block (SMB) trên File Gateway, giúp thực thi các tiêu chuẩn bảo mật dữ liệu khi sử dụng gateway để lưu trữ và truy cập dữ liệu trong S3.

### Security
* Bạn có thể sử dụng AWS KMS để mã hóa dữ liệu.
* Xác thực và kiểm soát truy cập với IAM.

### Pricing
* Bạn bị tính phí dựa trên loại và dung lượng lưu trữ bạn sử dụng, yêu cầu bạn thực hiện và lượng dữ liệu được chuyển ra khỏi AWS.
* Bạn chỉ bị tính phí cho lượng dữ liệu bạn ghi vào tape từ Tape Gateway, không phải dung lượng tape.
