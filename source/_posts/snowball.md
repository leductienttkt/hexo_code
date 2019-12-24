---
title: AWS Snowball
categories:
 - Storage Services
---

* Chuyển lượng lớn dữ liệu vào và ra khỏi AWS bằng các thiết bị lưu trữ vật lý, không dùng Internet.
* Ở các US regions, Snowballs có hai kích cỡ: 50TB và 80TB. Tất cả các khu vực khác chỉ có 80TB Snowballs.
* Bạn sở hữu máy trong 10 ngày miễn phí để thực hiện chuyển dữ liệu của mình.
*  Features:
    *  Import và export dữ liệu giữa các vị trí lưu trữ dữ liệu tại chỗ của bạn và S3.
    *  Mã hóa, bảo vệ dữ liệu của bạn trong quá trình vận chuyển vật lý.
 * Snowball vs Snowball Edge

    |Use Case | Snowball | Snowball Edge |
    | -------- | -------- | -------- |
    | Import data to S3     | Yes     | Yes     |
    | Export data from S3     | Yes     | Yes     |
    | Durable local storage    |      | Yes     |
    | Local compute with AWS Lambda     |      | Yes     |
    | Amazon EC2 compute instances     |      | Yes     |
    | Use in a cluster of devices     |      | Yes     |
    |Use with AWS IoT Greengrass (IoT)     |      | Yes     |
    | Transfer files through NFS with a GUI     |      | Yes     |

    |Storage capacity (usable capacity) | Snowball | Snowball Edge |
    | -------- | -------- | -------- |
    |50 TB (42 TB) – US regions only     | Yes     |      |
    | 80 TB (72 TB)     | Yes     |      |
    | 100 TB (83 TB)    |      | Yes     |
    |100 TB Clustered (45 TB per node)     |      | Yes     |

    | Snowball Tools | Snowball Edge Tools |
    | -------- | -------- |
    | Snowball client with Snowball     | 	Snowball client with Snowball Edge     |
    | Amazon S3 Adapter for Snowball with Snowball    | 	Amazon S3 Adapter for Snowball with Snowball Edge    |
    |     | 	File interface with Snowball Edge     |
    |     | 	AWS IoT Greengrass console with Snowball Edge    |

*  Vì mục đích bảo mật, việc truyền dữ liệu phải được hoàn thành trong vòng 90 ngày kể từ khi Snowball được chuẩn bị.


### Snowball Jobs

* Import into S3
    * Dữ liệu của bạnđược sao chép  vào một Snowball duy nhất và sau đó được chuyển vào S3.
    * Snowballs và các import jobs có mối quan hệ one-to-one, có nghĩa là mỗi job có chính xác một Snowball được liên kết với nó. Nếu bạn cần thêm Snowballs, bạn có thể tạo mới các jobs hoặc sao chép các jobs hiện có.
    * Mỗi tệp hoặc đối tượng được import phải có kích thước nhỏ hơn hoặc bằng 5 TB.
* Export from S3
    * Sao chép dữ liệu trong S3 vào các Snowballs, sau đó chuyển Snowball đến địa điểm lưu trữ dữ liệu của bạn.
    * Khi tạo một export job, nó chia thành các job nhỏ, mỗi job có kích thước không quá 80TB, và mỗi job có chính xác một Snowball được liên kết với nó.

### Security

* Tất cả dữ liệu được chuyển đến Snowball có hai lớp mã hóa
    * Một lớp mã hóa được áp dụng trong bộ nhớ của máy trạm cục bộ của bạn. Mã hóa này sử dụng các khóa 256 bit AES GCM và các khóa được chuyển đổi cho mỗi 60 GB dữ liệu được truyền.
    * Mã hóa SSL là lớp mã hóa thứ hai cho tất cả dữ liệu đi vào hoặc ra khỏi một Snowball tiêu chuẩn.
* Snowball sử dụng mã hóa phía máy chủ (SSE) để bảo vệ dữ liệu.
* Snowball jobs phải được xác thực. Bạn làm điều này bằng cách tạo và quản lý người dùng IAM trong tài khoản của bạn.

### Data Validation

* Khi bạn sao chép tệp từ nguồn dữ liệu cục bộ bằng  Snowball client or the Amazon S3 Adapter for Snowball vào Snowball, một checksum được tạo. Các checksums được sử dụng để tự động xác thực dữ liệu khi nó được chuyển.

### Pricing

* Bạn phải trả phí dịch vụ cho mỗi job chuyển dữ liệu (cộng với phí vận chuyển).
* Mỗi job bao gồm việc sử dụng thiết bị Snowball miễn phí trong 10 ngày sử dụng tại chỗ và có một khoản phí nhỏ cho những ngày thêm.
* Ngày vận chuyển, bao gồm cả ngày thiết bị được nhận và ngày được chuyển trở lại AWS, không được tính vào 10 ngày miễn phí.

### Limits

* Vì mục đích bảo mật, việc truyền dữ liệu phải được hoàn thành trong vòng 90 ngày kể từ khi Snowball được chuẩn bị.
* Giới hạn dịch vụ mặc định cho số lượng Snowball bạn có thể có cùng một lúc là 1.
* Các tập tin phải ở trạng thái tĩnh trong khi được sao chép.
