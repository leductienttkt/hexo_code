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

|Đặc điểm|	Amazon EBS-Backed|	Amazon Instance Store-Backed|
| -------- | -------- | -------- |
|Thời gian Boot	|Thường ít hơn 1 phút|	Thường ít hơn 5 phút|
|Giới hạn dữ liệu|	16TiB	|10TiB|
|Upgrade Instance|	Có thể thay đổi instance type, kernel, ram... khi instance stop|	Không thể thay đổi thuộc tính của instance|
|Phí tổn	|Tính phí cho phần sử dụng instance, sử dụng Amazon EBS volume và lưu trữ AMI	|Tính phí cho phần sử dụng instance và lưu AMI trên S3|
|Tạo AMI	|Chỉ cần sử dụng 1 câu command	|Yêu cầu cài đặt và sử dụng AMI tools|
|Stop State	|Instance có thể đưa về stop state	|Instance không thể đưa về stop state|

### Pricing

* **On-Demand**  –  bạn chỉ trả tiền cho dung lượng điện toán theo giờ hoặc theo giây, tùy theo phiên bản bạn chạy. Không cần cam kết lâu dài hoặc thanh toán trước. Bạn có thể tăng hoặc giảm dung lượng điện toán tùy theo nhu cầu của ứng dụng và chỉ phải trả tiền theo mức giờ được quy định cho phiên bản bạn sử dụng.

   On-Demand được đề xuất sử dụng cho:
    * Người dùng muốn sử dụng Amazon EC2 với mức chi phí thấp và sự linh hoạt mà không phải thanh toán trước hay cam kết lâu dài
    * Ứng dụng có khối lượng công việc ngắn hạn, mức độ sử dụng thay đổi hoặc không thể dự đoán trước và không thể gián đoạn
    * Ứng dụng được phát triển hoặc chạy thử trên Amazon EC2 lần đầu tiên
    
* **Reserved** – Reserved đưa ra mức chiết khấu đáng kể (lên đến 75%) so với giá On-Demand.

    Phiên bản dự trữ được đề xuất sử dụng cho:

    * Ứng dụng có trạng thái ổn định
    * Ứng dụng có thể cần dung lượng dự trữ
    * Khách hàng có thể cam kết sử dụng EC2 trong thời hạn từ 1 đến 3 năm để giảm tổng chi phí điện toán
 
* **Spot** – cho phép bạn yêu cầu dung lượng điện toán Amazon EC2 dự phòng với mức giá thấp hơn tối đa 90% so với giá On-Demand.

   Spot được đề xuất sử dụng cho:

    * Ứng dụng có thời gian bắt đầu và kết thúc linh hoạt
    * Ứng dụng chỉ khả thi ớ mức giá điện toán cực kỳ thấp
    * Người dùng có nhu cầu điện toán cấp thiết cho lượng lớn dung lượng bổ sung

* **Dedicated Hosts** – Máy chủ dành riêng là máy chủ EC2 vật lý dành riêng để bạn sử dụng. Máy chủ dành riêng có thể giúp bạn giảm chi phí bằng cách cho phép bạn sử dụng các giấy phép phần mềm gắn liền với máy chủ hiện có, bao gồm Windows Server, SQL Server và SUSE Linux Enterprise Server (tùy theo thời hạn giấy phép)
    * Có thể mua On-Demand (giờ).
    * Có thể mua dưới dạng Reserved với mức giá giảm lên đến 70% so với giá On-Demand.
* **Dedicated Instances** –  trả theo giờ cho các instance chạy trên single-tenant hardware.
* Có phí chuyển dữ liệu khi sao chép AMI từ vùng này sang vùng khác
* Giá EBS khác với giá instance. 
* AWS áp đặt một khoản phí nhỏ hàng giờ nếu Elastic IP address không được liên kết với một instance hoặc nếu nó được liên kết với một instance đang dừng.
* Bạn phải trả phí cho bất kỳ Elastic IP addresses bổ sung nào được liên kết với một instance.

### Security

* Sử dụng **IAM** để kiểm soát quyền truy cập vào các instances.
    * IAM policies
    * IAM roles
* Hạn chế truy cập bằng cách chỉ cho phép các máy chủ hoặc mạng đáng tin cậy truy cập vào các cổng trên instance.
* **Security group** hoạt động như một tường lửa ảo kiểm soát lưu lượng cho một hoặc nhiều instances.
    * Tạo các security groups khác nhau cho các instance có yêu cầu bảo mật khác nhau.

    *  Có thể thêm các quy tắc cho mỗi security gruop cho phép lưu lượng truy cập vào hoặc ra từ các instance được liên kết của nó.
    * Có thể chỉnh sửa các rules của security group bất cứ lúc nào.
    * Những rules mới sẽ được tự động áp dụng cho tất các instances đang liên kết với security group.
    * Tổng hợp tất cả các rules từ tất cả các security groups được liên kết với một instance để quyết định có cho phép lưu lượng truy cập hay không.
    * Mặc định security cho phép tất cả outbound traffic.
    * Security group rules luôn được cho phép, bạn không thể tạo ra các rules từ chối truy cập.
    * Security groups are stateful
* Nếu bạn không chỉ định security group khi khởi chạy instance, instance sẽ được tự động liên kết với **default security group** cho VPC, với các rules sau:
    * Cho phép tất cả inbound traffic từ nhưng instances khác liên kết với default security group
    * Cho phép tất cả outbound traffic từ instance.
    
* Vô hiệu hóa thông tin đăng nhập dựa trên mật khẩu cho các instance được khởi chạy từ AMI của bạn, vì mật khẩu có thể bị bẻ khóa hoặc tìm thấy.

### Networking

* Elastic IP address là một static IPv4 address được thiết kế cho dynamic cloud computing. 

* Bạn cần liên kết một Elastic IP address với instance của bạn để có thể kết nối với internet
* Một Elastic IP address chỉ sử dụng trong một region.
* Mặc định, tất cả tài khoản AWS bị hạn chế 5 Elastic IP addresses trong mỗi region.
* Mặc định EC2 instances đi kèm với private IP.
* Mỗi instance trong một VPC đều có một network interface mặc định, được gọi là **primary network interface (eth0)**, bạn không thể tách primary network interface khỏi instance.
* Bạn có thể tạo và đính kèm bổ sung các network interfaces. Số lượng network interfaces tối đa phù thuộc vào loại instance.
* Bạn có thể đính kèm một network interface cho một instance trong một subnet khác, miễn là nó cùng AZ
* Network interface mặc định terminated khi instance terminated
* Scale với EC2 Scaling Groups và phân phối lưu lượng giữa các instances sử dụng Elastic Load Balancer.

### Monitoring

* Các mục EC2 để giám sát:
    * CPU utilization, Network utilization, Disk performance, Disk Reads/Writes sử dụng **EC2 metrics**
    * Memory utilization, disk swap utilization, disk space utilization, page file utilization, log collection sử dụng **monitoring agent/CloudWatch Logs**
* Các công cụ giám sát tự động bao gồm:
    * **System Status Checks** – giám sát các hệ thống AWS cần có để sử dụng instance của bạn để đảm bảo chúng hoạt động tốt. Các kiểm tra này phát hiện các vấn đề với instance của bạn yêu cầu sự tham gia của AWS để sửa chữa.

    * **Instance Status Checks** – giám sát phần mềm và cấu hình mạng của instance. Những kiểm tra này phát hiện các vấn đề đòi hỏi sự tham gia của bạn để sửa chữa.
    * **Amazon CloudWatch Alarms** – giám sát một số liệu trong một khoảng thời gian bạn chỉ định và thực hiện một hoặc nhiều hành động dựa trên giá trị của số liệu liên quan đến một ngưỡng nhất định trong một số khoảng thời gian.
    * **Amazon CloudWatch Events** – tự động hóa các dịch vụ AWS của bạn và tự động trả lời các sự kiện hệ thống.
    * **Amazon CloudWatch Logs** – giám sát, lưu trữ và truy cập các tệp logs của bạn từ các instance Amazon EC2, AWS CloudTrail hoặc các nguồn khác.
* Giám sát EC2 instances với **CloudWatch**. Mặc định, EC2 gửi dữ liệu đến CloudWatch theo chu kì 5 phút.

### Instance Metadata and User Data

* **Instance metadata** là dữ liệu về instance của bạn, có thể sử dụng để cấu hình hoặc quản lý instance đang chạy.

* **Instance metadata** và **user data** không được bảo vệ bằng các phương pháp mã hóa.
* Có thể truyền 2 loại user data cho EC2: shell scripts và cloud-init directives.
* User data giới hạn tới 16 KB.
* Nếu bạn dừng một instance, chỉnh sửa user data của nó rồi khởi động lại, user data được thay đổi sẽ không thực thi khi bạn khởi động instance

### Placement Groups

* Bạn có thể khởi chạy hoặc bắt đầu các instances trong một placement group.
    * **Cluster** – phân cụm các instances thành một nhóm có độ trễ thấp trong một Availability Zone.

    * **Partition** – phân tán các instances của bạn trên các phân vùng logic sao cho các nhóm instances trong một phân vùng không chia sẻ phần cứng bên dưới với các nhóm instances trong các phân vùng khác nhau.
    * **Spread** – tách các instances trên phần cứng cơ bản. Đề xuất cho các ứng dụng có một số lượng nhỏ các instance quan trọng cần được tách biệt với nhau.
* Rules:
    * Tên bạn chỉ định cho một placement group phải là duy nhất trong tài khoản AWS của trong một region.

    * Bạn không thể hợp nhất các placement groups.
    * Một instance chỉ có thể khởi chạy trong một placement group tại một thời điểm.
    
### Storage

![](/images/EC2-storage.jpg)

* **EBS - Elastic Block Store**
    * Cung cấp **block-level** storage volumes lâu dài, bạn có thể đính kèm vào một instance.
    * Sử dụng như một thiết bị lưu trữ chính cho dữ liệu yêu cầu cập nhật thường xuyên và chi tiết.
    * Để giữ một bản sao lưu dữ liệu của bạn, hãy tạo một snapshot của một EBS, được lưu trữ trong S3. Bạn có thể tạo một EBS từ snapshot và đính kèm nó vào một instance khác.
    
* **Instance Store**
    * Cung cấp block-level storage tạm thời cho các instances.
    * Dữ liệu trên instance store volume chỉ tồn tại trong suốt vòng đời của instance được liên kết, nếu bạn dừng hay chấm dứt một instance, tất cả dữ liệu trên instance store volume sẽ bị mất.

* **EFS - Elastic File System**
    * Cung cấp file storage có thể mở rộng để sử dụng với Amazon EC2. Bạn có thể tạo một hệ thống tệp EFS và định cấu hình instance của mình để gắn hệ thống tệp.
    * You can use an EFS file system as a common data source for workloads and applications running on multiple instances.
    * Bạn có thể sử dụng hệ thống tệp EFS làm nguồn dữ liệu chung cho workloads và ứng dụng chạy trên nhiều instance.
    
* **S3**
    * Cung cấp quyền truy cập vào cơ sở hạ tầng lưu trữ dữ liệu đáng tin cậy và giá rẻ.
    * Lưu trữ EBS snapshots và instance store-backed AMIs.

### Resources and Tagging

* EC2 resources bao gồm images, instances, volumes, và snapshots. Khi bạn tạo một resource, AWS gán cho resource một ID duy nhất

* Some resources can be used in all regions (global), and some resources are specific to the region or Availability Zone in which they reside.
* Một vài resources có thể sử dụng trên tất cả regions, một vài resources chỉ sử dụng trong region hoặc AZ của nó.
* Bạn có thể tùy ý gán metadata của riêng mình cho từng tài nguyên bằng các thẻ, bao gồm một khóa và một giá trị tùy chọn



-----
Sources:

https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/

https://aws.amazon.com/ec2/features/

https://aws.amazon.com/ec2/pricing/

https://aws.amazon.com/ec2/faqs/





