---
title: Amazon Glacier
categories:
 - Storage Services
---

* Giải pháp lưu trữ dài hạn được tối ưu hóa cho dữ liệu được sử dụng không thường xuyên.
* Glacier là một dịch vụ web dựa trên REST.
* Bạn có thể lưu trữ số lượng dữ liệu không giới hạn.
* Bạn không thể chỉ định Glacier là lớp lưu trữ tại thời điểm bạn tạo một đối tượng.
* Nó được thiết kế để cung cấp độ bền trung bình hàng năm là 99.999999999% cho một kho lưu trữ.
* Glacier lưu trữ đồng bộ dữ liệu của bạn trên nhiều AZ trước khi xác nhận tải lên thành công.
* Để ngăn ngừa hư hỏng các gói dữ liệu qua, Glacier tải lên checksum trong quá trình tải lên dữ liệu. Nó so sánh checksum nhận được với checksum của dữ liệu nhận được và xác thực tính xác thực của dữ liệu với checksum trong quá trình truy xuất dữ liệu.
* Glacier hoạt động cùng với các Amazon S3 lifecycle rules để giúp bạn tự động lưu trữ dữ liệu S3 và giảm chi phí lưu trữ chung. Dữ liệu lưu trữ được yêu cầu được sao chép vào S3 One Zone-IA.

### Data Model

* Vault
    * Một container để lưu trữ tài liệu.
    * Mỗi vault có một địa chỉ duy nhất có dạng: `https://[region-specific-endpoint]/[account-id]/vaults/[vaultname]`
    * Bạn có thể lưu trữ không giới hạn số lượng tài liệu trong một vault.
    * Vault hoạt động trên Region cụ thể.

* Archive
    * Có thể là bất kỳ dữ liệu nào như ảnh, video hoặc tài liệu và là đơn vị lưu trữ cơ bản trong Glacier.
    * Mỗi archive có một địa chỉ duy nhất với hình thức:  `https://[region-specific-endpoint]/[account-id]/vaults/[vault-name]/archives/[archive-id]`

* Job
    * Bạn có thể thực hiện một truy vấn chọn trên archive, truy xuất archive hoặc lấy archive của vault. Glacier Selec t chạy truy vấn tại chỗ và ghi kết quả đầu ra vào Amazon S3.
    * Jobs select, truy xuất archive và kiểm kê vault được liên kết với một vault. Một vault có thể có nhiều jobs chạy tại bất kỳ thời điểm nào.

* Notification Configuration
    * Vì các jobs cần có thời gian để chạy, Glacier hỗ trợ cơ chế thông báo để thông báo cho bạn khi job hoàn thành.

### Glacier Operations

* Lấy một archive (hoạt động không đồng bộ)
* Lấy một vault (list of archives)  (hoạt động không đồng bộ)
* Tạo và xóa vaults
* Nhận mô tả vault cho một vault cụ thể hoặc cho tất cả các vaults trong một region
* Đặt, truy xuất và xóa cấu hình thông báo trên vault
* Tải lên và xóa archives. Bạn không thể cập nhật một archive hiện có.
* Glacier jobs - chọn, truy xuất archive, truy xuất inventory.

### Vaults

* Vault hoạt động trên region cụ thể
* Vault names phải là duy nhất trong một tài khoản và khu vực nơi vault được tạo.
* Bạn chỉ có thể xóa một vault nếu không có archives trong vault kể từ lần kiểm kê cuối cùng mà Glacier đã tính toán và không có ghi vào vault kể từ lần kiểm kê cuối cùng.
* Bạn có thể truy xuất thông tin vault như ngày tạo, số lượng archives và tổng kích thước của tất cả các archives
* Glacier duy trì một vault inventory của tất cả các tài liệu lưu trữ trong mỗi vault của bạn để khắc phục thảm họa. Glacier cập nhật vault inventory khoảng một lần một ngày. Tải xuống vault inventory là một hoạt động không đồng bộ.
* Bạn có thể gán metadata của riêng mình cho kho Glacier dưới dạng tags. Tags là một cặp khóa-giá trị mà bạn xác định cho một vault.

### Archives

* Glacier hỗ trợ các hoạt động lưu trữ cơ bản sau: tải lên, tải xuống và xóa. Tải xuống một kho lưu trữ là một hoạt động không đồng bộ.
* Bạn có thể tải lên một kho lưu trữ trong một lần hoặc tải nó lên thành từng phần.
* Sử dụng API tải lên nhiều phần, bạn có thể tải lên kho lưu trữ lớn, lên tới khoảng 10.000 x 4 GB.
* Bạn không thể tải tài liệu lưu trữ lên Glacier bằng cách sử dụng bảng điều khiển quản lý. Sử dụng AWS CLI hoặc viết mã để thực hiện các yêu cầu, bằng cách sử dụng REST API trực tiếp hoặc bằng cách sử dụng SDK AWS.
* Bạn không thể xóa kho lưu trữ bằng bảng điều khiển quản lý Amazon S3 Glacier (Glacier). Glacier cung cấp một lệnh gọi API mà bạn có thể sử dụng để xóa một kho lưu trữ tại một thời điểm.
* Sau khi bạn tải lên một kho lưu trữ, bạn không thể cập nhật nội dung hoặc mô tả của nó. Cách duy nhất bạn có thể cập nhật nội dung lưu trữ hoặc mô tả của nó là bằng cách xóa kho lưu trữ và tải lên một kho lưu trữ khác.
* Glacier không hỗ trợ bất kỳ metadata bổ sung nào cho tài liệu lưu trữ.

### Glacier Select

* Bạn có thể thực hiện các hoạt động lọc bằng cách sử dụng các câu lệnh SQL đơn giản trực tiếp trên dữ liệu của mình trong Glacier.
* Bạn có thể chạy truy vấn và phân tích tùy chỉnh trên dữ liệu của mình được lưu trữ trong Glacier mà không phải khôi phục dữ liệu của bạn về S3.
* Khi bạn thực hiện các truy vấn select, Glacier cung cấp ba tầng truy cập dữ liệu:
    * Expedited – dữ liệu được truy cập thường được cung cấp trong vòng 1  đến 5 phút.
    * Standard – dữ liệu được truy cập thường được cung cấp trong vòng 3 đến 5 giờ.
    * Bulk – dữ liệu được truy cập thường được cung cấp trong vòng 5 đến 12 giờ.

### Glacier Data Retrieval Policies

* Đặt giới hạn truy xuất dữ liệu và quản lý các hoạt động truy xuất dữ liệu trên tài khoản AWS của bạn ở từng khu vực.
* Ba loại chính sách:
    * Free Tier Only – bạn có thể giữ các truy xuất của mình trong phạm vi miễn phí hàng ngày và không phải chịu bất kỳ chi phí truy xuất dữ liệu nào.
    * Max Retrieval Rate – đảm bảo rằng tốc độ truy xuất cao nhất từ tất cả các retrieval jobs trên tài khoản của bạn trong một khu vực không vượt quá giới hạn byte mỗi giờ bạn đặt.
    * Không giới hạn truy xuất

### Security

* Glacier mã hóa dữ liệu của bạn theo mặc định và hỗ trợ truyền dữ liệu an toàn với SSL.
* Dữ liệu được lưu trữ trong Amazon Glacier là bất biến, có nghĩa là sau khi lưu trữ được tạo, nó không thể được cập nhật.
* Truy cập vào Glacier yêu cầu thông tin đăng nhập mà AWS có thể sử dụng để xác thực các yêu cầu của bạn. Những thông tin đăng nhập phải có quyền truy cập vào kho Glacier hoặc S3.
* Glacier yêu cầu tất cả các yêu cầu được ký để bảo vệ xác thực. Để ký một yêu cầu, bạn tính toán chữ ký số bằng hàm băm mật mã trả về giá trị băm mà bạn đưa vào yêu cầu làm chữ ký của mình.
* Glacier chỉ hỗ trợ các chính sách ở cấp vault.
* Khi có hoạt động xảy ra trong Glacier, hoạt động đó được ghi lại trong sự kiện CloudTrail cùng với các sự kiện dịch vụ AWS khác trong Event History.

### Pricing

* Bạn bị tính phí trên mỗi GB mỗi tháng lưu trữ.
* Bạn phải trả phí cho các hoạt động truy xuất, chẳng hạn như yêu cầu truy xuất và lượng dữ liệu được truy xuất tùy thuộc vào cấp truy cập dữ liệu - Expedited, Standard, or Bulk.
* Yêu cầu tải lên được tính phí.
* Bạn bị tính phí cho dữ liệu được chuyển ra khỏi Glacier.
* Giá cho Glacier Select dựa trên tổng số lượng dữ liệu được quét, lượng dữ liệu được trả về và số lượng yêu cầu được bắt đầu.
* Có tính phí nếu bạn xóa dữ liệu trong vòng 90 ngày.

### Limits

* Trong một tài khoản AWS, bạn có thể có tới 1000 vaults.
