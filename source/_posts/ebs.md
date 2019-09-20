---
title: Amazon EBS - Amazon Elastic Block Store
categories:
 - Storage Services
---
* **Block level storage** để sử dụng với các phiên bản EC2.
* Rất thích hợp để sử dụng làm bộ lưu trữ chính cho các hệ thống tệp, cơ sở dữ liệu hoặc cho bất kỳ ứng dụng nào.
* EBS mới nhận được hiệu suất tối đa của chúng ngay khi chúng có thể dùng và không yêu cầu khởi tạo. Tuy nhiên, các khối lưu trữ trên các ổ đĩa được khôi phục từ snapshots phải được khởi tạo (lấy từ Amazon S3 và được ghi vào ổ đĩa) trước khi bạn có thể truy cập.
* Bảo vệ dữ liệu khi terminated instance được tắt theo mặc định, bạn phải kích hoạt thủ công.
* Theo mặc định, bạn có thể có tối đa 5.000 EBS volumes.
* Theo mặc định, bạn có thể có tới 10.000 snapshots .

### Features

* Các loại tùy chọn lưu trữ khác nhau: General Purpose SSD (gp2), Provisioned IOPS SSD (io1), Throughput Optimized HDD (st1), Cold HDD (sc1) volumes có dung lượng lên tới 16 TB.
* Bạn có thể mount nhiều volumes trên cùng 1 instance, nhưng mỗi volume chỉ được gắn vào 1 instance cùng thời điểm
* Bạn có thể tạo một hệ thống tệp trên các ổ đĩa này hoặc sử dụng chúng theo cách mà bạn sử dụng một ổ cứng.
* Bạn có thể tạo các khối EBS của mình dưới dạng các khối được mã hóa,.
* Bạn có thể tạo các snapshots theo thời gian của EBS volumes, và sẽ được lưu trữ trên Amazon S3, Tương tự AMIs, snapshots có thể sao chép giữa cái AWS regions.
* Volumes được tạo trong 1 AZ cụ thể và có thể được gắn vào bất kì instances nào trong cùng AZ. Để sử dụng volume ở một AZ khác, bạn có thể tạo một snapshot và khôi phục snapshot đó thành một volume mới trong AZ đó.
* Bạn có thể sao chép các snapshots sang các khu vực khác và sau đó khôi phục chúng sang các volume mới ở đó, giúp dễ dàng tận dụng nhiều vùng AWS để mở rộng địa lý, di chuyển trung tâm dữ liệu và khắc phục thảm họa..
* Các số liệu hiệu suất, chẳng hạn như băng thông, thông lượng, độ trễ và độ dài hàng đợi trung bình do Amazon CloudWatch cung cấp, cho phép bạn theo dõi hiệu suất của volumes của mình để đảm bảo rằng bạn đang cung cấp đủ hiệu suất cho các ứng dụng của mình mà không phải trả tiền cho các tài nguyên mà bạn không nhu cầu.
* Nếu EBS volume là root device của instance , bạn phải dừng instance đó trước khi bạn có thể tách volume.
* Bạn có thể sử dụng AWS Backup, một dịch vụ sao lưu tự động và tập trung, để bảo vệ EBS volumes và các tài nguyên AWS khác của bạn. AWS Backup được tích hợp với Amazon DynamoDB, Amazon EBS, Amazon RDS, Amazon EFS và AWS Storage Gateway để cung cấp cho bạn một giải pháp sao lưu AWS được quản lý hoàn toàn.
* Với AWS Backup, bạn có thể định cấu hình sao lưu cho EBS volumes, tự động lập lịch sao lưu, đặt chính sách duy trì và theo dõi hoạt động sao lưu và khôi phục.

### Types of EBS Volumes

* General Purpose SSD (gp2)
    * Cung cấp hiệu suất cơ bản là 3 IOPS / GiB, với khả năng lên tới 3.000 IOPS trong thời gian dài.
    * Hỗ trợ lên tới 10.000 IOPS và 160MB/s thông lượng.
* Provisioned IOPS SSD (io1)
    * Được thiết kế cho khối lượng công việc chuyên sâu I/O, đặc biệt là khối lượng công việc cơ sở dữ liệu, nhạy cảm với hiệu suất lưu trữ và tính nhất quán.
    * Cho phép bạn chỉ định tốc độ IOPS phù hợp khi bạn tạo volume.
* Throughput Optimized HDD (st1)
    * Lưu trữ chi phí thấp tập trung vào thông lượng hơn là IOPS.
    * Throughput lên đến 500 MiB/s.
* Cold HDD (sc1)
    * Lưu trữ chi phí thấp tập trung vào thông lượng hơn là IOPS.
    * Throughput lên đến 250 MiB/s.



|Volume Type | General Purpose SSD (gp2) | Provisioned IOPS SSD (io1) |Throughput Optimized HDD (st1) |Cold HDD (sc1)|
| -------- | -------- | -------- | -------- |-------- |
| Use Cases     | Được khuyến nghị cho hầu hết công việc, system boot volumes, máy tính để bàn ảo, ứng dụng tương tác độ trễ thấp, môi trường development và test... |  Các ứng dụng kinh doanh quan trọng đòi hỏi hiệu suất IOPS bền vững, cơ sở dữ liệu lớn.... | Công việc -streaming đòi hỏi thông lượng nhanh, nhất quán ở mức giá thấp, big data, khoogn thể là boot volume.. |Dữ liệu được truy cập không thường xuyên, ưu tiên chi phí, không thể là boot volume... |
| Volume Size     | 1 GiB – 16 TiB     |  4 GiB – 16 TiB | 500 GiB – 16 TiB |500 GiB – 16 TiB |
| Max IOPS/Volume     | 10,000     |  32,000 | 500 |250 |
| Max Throughput/Volume     | 160 MiB/s     |  500 MiB/s | 500 MiB/s |250 MiB/s |
|Max IOPS/Instance     | 80,000     |  80,000 | 80,000 |80,000 |
| Max Throughput/Instance     | 1,750 MiB/s     |  1,750 MiB/s | 1,750 MiB/s |1,750 MiB/s |
| Thuộc tính ưu tiên   | IOPS     |  IOPS | MiB/s |MiB/s |

![](/images/EBS-1.jpg)

### Encryption

* Dữ liệu được lưu trữ  trên một ổ đĩa được mã hóa, disk I/O và snapshots được tạo từ nó đều được mã hóa.
* Đồng thời cung cấp mã hóa cho dữ liệu truyền từ EC2 sang EBS do mã hóa xảy ra trên các máy chủ lưu trữ các phiên bản EC2.
* Các loại dữ liệu sau đây được mã hóa:
    * Dữ liệu bên trong volume
    * Tất cả dữ liệu di chuyển giữa volume và instance
    * Tất cả  snapshots được tạo từ volume
    * Tất cả volumes được tạo từ những snapshots đó
* Sử dụng AWS Key Management Service (AWS KMS) khi tạo các volumes được mã hóa và bất kỳ snapshots được tạo từ các volume được mã hóa của bạn.
* Volumes được khôi phục từ snapshots được mã hóa được tự động mã hóa.
* Mã hóa EBS chỉ có sẵn trên các loại instances nhất định.
* Không có cách trực tiếp để mã hóa một volume không được mã hóa hiện có hoặc để loại bỏ mã hóa khỏi một volume được mã hóa. Tuy nhiên, bạn có thể di chuyển dữ liệu giữa các volume được mã hóa và không được mã hóa.

### Monitoring

* Cloudwatch Monitoring 2 loại: Giám sát cơ bản và chi tiết
*  Danh sách các trạng thái bao gồm:
    * Ok – volume bình thường
    * Warning – volume xuống cấp
    * Impaired – volume bị đình trệ
    * Insufficient-data –  không đủ dữ kiện
* Các sự kiện volumes bao gồm thời gian bắt đầu cho biết thời gian xảy ra sự kiện và thời lượng cho biết thời gian I/O của volume bị tắt. Thời gian kết thúc được thêm vào sự kiện khi I/O cho âm lượng được bật.
* Các sự kiện volume:
    * Awaiting Action: Enable IO
    * IO Enabled
    * IO Auto-Enabled
    * Normal
    * Degraded
    * Severely Degraded
    * Stalled

### Modifying the Size, IOPS, or Type of an EBS Volume on Linux

* Nếu EBS volume thế hệ hiện tại của bạn được gắn với loại instance EC2 thế hệ hiện tại, bạn có thể tăng kích thước của nó, thay đổi loại volume hoặc (đối với volume io1) mà không cần tách rời nó.
* EBS hiện hỗ trợ kích thước volume tối đa là 16 TiB.
* Hai lược đồ phân vùng thường được sử dụng trên các hệ thống Linux và Windows: master boot record (MBR) và GUID partition table (GPT).
* Một EBS volume được sửa đổi đi qua một chuỗi các trạng thái. Đầu tiên vào trạng thái Sửa đổi, sau đó là trạng thái Tối ưu hóa và cuối cùng là trạng thái Hoàn thành.
* Bạn có thể mở rộng một phân vùng đến một kích thước mới. Mở rộng bằng cách sử dụng parted hoặc gdisk.
* Giảm kích thước của EBS volume không được hỗ trợ.

### EBS Snapshots

* Sao lưu dữ liệu trên EBS volume của bạn lên S3 bằng cách tạo snapshots theo thời gian.
* Snapshots là bản sao lưu gia tăng, có nghĩa là chỉ các volume trên thiết bị đã thay đổi sau khi snapshot gần đây nhất của bạn được lưu. Điều này giảm thiểu thời gian cần thiết để tạo snapshot và tiết kiệm chi phí lưu trữ bằng cách không sao chép dữ liệu.
* Khi bạn xóa một snapshot, chỉ có dữ liệu duy nhất cho snapshot đó bị xóa.

![](/images/EBS-2.jpg)

* Bạn có thể chia sẻ snapshot trên các tài khoản AWS bằng cách sửa đổi quyền truy cập của nó.
* Bạn có thể tạo các bản sao của snapshots của riêng bạn cũng như các snapshots đã được chia sẻ với bạn.
* snapshot  bị giới hạn ở Region nơi nó được tạo.
* EBS snapshots hổ trợ EBS encryption.
* Bạn có thể xóa một snapshot của root device của EBS volume được sử dụng bởi một AMI đã đăng ký. Trước tiên, bạn phải hủy đăng ký AMI trước khi bạn có thể xóa snapshot.
* Tags do người dùng chỉ định không được sao chép từ snapshot nguồn sang snapshot mới.
* Để chia sẻ snapshot với Region khác, hãy sao chép snapshot vào Region đó.

### Amazon EBS–Optimized Instances

* Cung cấp hiệu suất tốt nhất cho EBS volume của bạn bằng cách giảm thiểu sự tranh chấp giữa I/ OEBS và lưu lượng truy cập khác từ instance của bạn.
* Các phiên bản được tối ưu hóa của EBS cung cấp băng thông chuyên dụng từ 425 Mbps đến 14.000 Mbps cho EBS.
* Đối với các loại instance được EBS tối ưu hóa theo mặc định, không cần bật tối ưu hóa EBS và không có hiệu lực nếu bạn tắt tối ưu hóa EBS.

### Pricing

* Bạn bị tính phí tính theo tổng số lượng lưu trữ tính bằng GB mỗi tháng cho đến khi bạn giải phóng bộ nhớ.
* Với Provisioned IOPS SSD (io1) bạn cũng bị tính phí bằng bởi IOPS mỗi tháng.
* Sau khi bạn tách volume, bạn vẫn bị tính phí lưu trữ volume miễn là dung lượng lưu trữ vượt quá giới hạn của AWS Free Tier. Bạn phải xóa volume để tránh phát sinh thêm phí.
* Lưu trữ snapshot dựa trên dung lượng mà dữ liệu của bạn tiêu thụ trong Amazon S3.
* Sao chép snapshot sang Region mới sẽ phải chịu chi phí lưu trữ mới.
* Khi bạn bật tối ưu hóa EBS cho một phiên bản không được tối ưu hóa EBS theo mặc định, bạn phải trả thêm một khoản phí thấp, hàng giờ cho dung lượng chuyên dụng.

### Limits of EBS Per Region

|Resource | Default Limit |
| -------- | -------- |
| Số lượng EBS snapshots     | 100,000     |
| Số lượng snapshots đồng thời được phép cho 1 volume duy nhất     | 5 cho io1, gp2, magnetic; 1 cho st1, sc1     |
| Yêu cầu sao chép đồng thời vào một region    | 5     |
| Tổng dung lượng lưu trữ của  General Purpose SSD (gp2)   | 300 TiB     |
| Tổng dung lượng lưu trữ của Provisioned IOPS SSD (io1)     | 300 TiB     |
| Tổng dung lượng lưu trữ của Throughput Optimized HDD (st1)    | 300 TiB     |
| Tổng dung lượng lưu trữ của Cold HDD (sc1)   | 300 TiB     |
| Tổng dung lượng lưu trữ của Magnetic volumes     | 300 TiB     |
| Tổng số IOPS được cung cấp     | 300000     |
