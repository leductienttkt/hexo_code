---
title: Amazon Elastic Compute Cloud ( Amazon EC2 )
categories:
 - Compute Services
---
**Amazon Elastic Compute Cloud (Amazon EC2)** cung cấp khả năng tính toán có thể mở rộng trong AWS.

Sử dụng Amazon EC2 giúp loại bỏ nhu cầu đầu tư vào phần cứng, vì vậy bạn có thể phát triển và triển khai các ứng dụng nhanh hơn.

Bạn có thể sử dụng Amazon EC2 để khởi chạy nhiều hoặc ít máy chủ ảo mà bạn cần, chỉ định cấu hình bảo mật, kết nối mạng và quản lý lưu trữ.

Amazon EC2 cho phép bạn điều chỉnh quy mô lên hoặc xuống để xử lý các thay đổi về yêu cầu.

### Features

* Môi trường máy chủ được gọi là **instances**.
* Gói hệ điều hành và những cài đặt bổ sung trong một mẫu có thể sử dụng lại được gọi là **Amazon Machine Images.**
* Các cấu hình khác nhau của CPU, bộ nhớ, lưu trữ và dung lượng mạng cho các instances của bạn, được gọi là các instance types.

    * **t-type** và **m-type** cho các mục đích chung.
    * **c-type** tối ưu hóa xử lí tính toán
    * **r-type**, **x-type** và **z-type** tối ưu hóa bộ nhớ
    * **d-type**, **h-type** và **i-type** tối ưu hóa lưu trữ
    * **f-type**, **g-type** và **p-type** tăng tốc độ tính toán

* Bảo mật thông tin đăng nhập instances bằng việc sử dụng **key pairs** (AWS lưu trữ public key và bạn lưu trữ private key ở nơi an toàn)
* Dung lượng lưu trữ cho dữ liệu tạm thời bị xóa khi bạn dừng hoặc chấm dứt instance của bạn, được gọi là **instance store volumes.**
* Lưu trữ lâu dài dữ liệu của bạn bằng **Amazon Elastic Block Store (Amazon EBS)** - (Amazon EBS volumes)
* Nhiều vị trí thực tế cho tài nguyên của bạn, như instance và Amazon EBS volumes, được gọi là  **Regions** and **Availability Zones**
* Tường lửa cho phép bạn chỉ định các giao thức, cổng và dải IP nguồn có thể tiếp cận các instances của bạn bằng cách sử dụng các **security groups**
* Static IPv4 addresses for dynamic cloud computing, known as Elastic IP addresses (see aws networking and content delivery).
* Địa chỉ IPv4 tĩnh cho điện toán đám mây động, được gọi là  **Elastic IP addresses**
* Tạo và gán metadatas (tags) cho các tài nguyên EC2
* Có thể tùy ý kết nối với mạng riêng của mình (**virtual private clouds - VPC**)
* Thêm một tập lệnh sẽ được chạy khi khởi động instance được gọi là **user-data**.

### Instance states

* **Start** –  instance chạy bình thường, chi phí sẽ được tính liên tục khi instance chạy.
* **Stop** –đơn giản là tắt instance. Có thể khởi động lại nó bất cứ lúc nào. Tất cả EBS volumes vẫn được đính kèm, tuy nhiên dữ liệu trong **instance store volumes** sẽ bị xóa. Bạn sẽ không phải trả tiền khi instance đã stop. Bạn có thể đính kèm hoặc tách các EBS volumes. Bạn cũng có thể tạo AMI từ instance, thay đổi kernel, RAM disk, và instance type khi ở trạng thái này.
* **Terminate** – tắt và xóa instance. Bạn không thể khởi động lại instance khi đã terminate. Mặc định **root device volume** sẽ bị xóa, nhưng những EBS volumes đính kèm vẫn giữ nguyên. Dữ liệu trong **instance store volumes** bị xóa.
* Để ngăn chặn việc vô tình terminate instance, vô hiệu hóa instance termination.

### Root Device Volumes

* Root device volume chưa image dùng để khởi động instance.
* Instance Store-backed Instances
    * Mọi dữ liệu trên instance store volumes sẽ bị xóa khi instance bị terminated hoặc nếu nó không thành công 
    * Bạn cũng nên sao lưu dữ liệu quan trọng từ instance store volumes của bạn một cách thường xuyên.
    
* Amazon EBS-backed Instances
    * An Amazon EBS-backed instance có thể dừng lại và sau đó được khởi động lại mà không ảnh hưởng đến dữ liệu được lưu trữ trong các volumes đính kèm.
    * Khi ở trạng thái dừng, bạn có thể sửa đổi các thuộc tính của instance, thay đổi kích thước của nó hoặc cập nhật kernel mà nó đang sử dụng hoặc bạn có thể đính kèm root volume của mình vào một instance đang chạy khác để gỡ lỗi hoặc cho bất kỳ mục đích nào khác.
    * Mặc định, **root device volume** cho AMI backed by Amazon EBS bị xóa khi instance terminates.
    * Previously, to launch an encrypted EBS-backed EC2 instance from an unencrypted AMI, you would first need to create an encrypted copy of the AMI and use that to launch the EC2 instance. Now, you can launch encrypted EBS-backed EC2 instances from unencrypted AMIs directly.
    * Trước đây, để khởi chạy một instance EC2 được mã hóa EBS từ một AMI không được mã hóa, trước tiên bạn cần tạo một bản sao được mã hóa của AMI và sử dụng nó để khởi động instance EC2. Bây giờ, bạn có thể khởi chạy trực tiếp các instances EBS-backed EC2 được mã hóa từ các AMI không được mã hóa.

### AMI

* Bao gồm:
    * Template cho **root volume** cho instance (OS, application server, và applications)
    * Launch permissions kiểm soát tài khoản AWS nào có thể sử dụng AMI để khởi chạy instance
    * Block device mapping chỉ định các ổ đĩa để đính kèm vào instance khi nó khởi chạy

    ![](/images/AMI.jpg)
    
*  **Backed by Amazon EBS** – **root device** cho một instance được khởi chạy từ AMI là một **Amazon EBS volume**. AMIs backed by Amazon EBS snapshots có thể sử dụng mã hóa EBS.
* **Backed by Instance Store** –**root device** cho một instance được khởi chạy từ AMI là một **instance store volume** được tạo từ template lưu trữ trong S3.
* Bạn có thể sao chép AMI sang các regions khác nhau.

