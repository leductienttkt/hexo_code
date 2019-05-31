---
title: AWS Well-Architected Framework – Disaster Recovery
categories:
 - Overview
---

* **RTO** là thời gian cần thiết sau khi bị gián đoạn để khôi phục.
* **RPO** là lượng mất dữ liệu chấp nhận được đo theo thời gian.
* Disaster Recovery With AWS

   -  **Backup and Restore** – lưu trữ dữ liệu sao lưu trên S3 và khôi phục dữ liệu nhanh chóng và đáng tin cậy.

      ![](/images/Disaster-Recovery-1.jpg)

   - **Pilot Light** for Quick Recovery into AWS – thời gian phục hồi nhanh hơn so với **Backup and Restore** vì các phần cốt lõi của hệ thống đã chạy và liên tục được cập nhật.

      ![](/images/Disaster-Recovery-2.jpg)
      ![](/images/Disaster-Recovery-3.jpg)

   - **Warm Standby Solution** – phiên bản thu nhỏ của một môi trường đầy đủ chức năng luôn chạy trong cloud.

      ![](/images/Disaster-Recovery-4.jpg)
      ![](/images/Disaster-Recovery-5.jpg)

   - **Multi-Site Solution** – chạy cơ sở hạ tầng của bạn trên một site khác.

      ![](/images/Disaster-Recovery-6.jpg)
      ![](/images/Disaster-Recovery-7.jpg)

   - AWS Production to an AWS DR Solution Using Multiple AWS Regions – tận dụng lợi thế của AWS multiple availability zones.

* Services

   - **S3** nơi để sao lưu dữ liệu, có thể nhanh chóng để thực hiện khôi phục.
   - **Import/Export** để chuyển các tập dữ liệu rất lớn bằng cách vận chuyển các thiết bị lưu trữ trực tiếp đến AWS.
   - **Glacier** để lưu trữ dữ liệu dài hạn trong đó thời gian truy xuất có thể cho phép trong vài giờ.
   - **Storage Gateway** sao chép snapshots của dữ liệu của bạn sang S3 để sao lưu. Bạn có thể tạo local volumes hoặc EBS từ các snapshots này.
   - Các máy chủ được cấu hình sẵn được đóng gói dưới dạng **Amazon Machine Images (AMIs)**.
   - **Elastic Load Balancing (ELB)** để phân phối lưu lượng cho nhiều instances.
   - **Route 53** để định tuyến lưu lượng đến các trang web khác nhau cung cấp cùng một ứng dụng hoặc dịch vụ.
   - **Elastic IP address** cho static IP addresses.
   - **Virtual Private Cloud (Amazon VPC)** để cung cấp một section riêng, tách biệt của  AWS cloud.
   - **Direct Connect** cho kết nối mạng chuyên dụng từ cơ sở của bạn đến AWS.
   - **Relational Database Service (RDS)** cho các cơ sở dữ liệu quan hệ trong đám mây.
   - **DynamoDB** quản lý các cơ sở dữ liệu NoQuery để lưu trữ và truy xuất bất kỳ lượng dữ liệu nào và phục vụ bất kỳ mức lưu lượng yêu cầu nào.
   - **Redshift** cho dịch vụ kho dữ liệu quy mô petabyte phân tích tất cả dữ liệu của bạn bằng các công cụ  thông minh hiện có.
   - **CloudFormation** để tạo ra các tài nguyên AWS liên quan và cung cấp chúng theo cách có trật tự và có thể dự đoán được.
   - **Elastic Beanstalk** là một dịch vụ để triển khai  các ứng dụng và dịch vụ web.
   - **OpsWorks** là một dịch vụ quản lý ứng dụng để triển khai và vận hành các ứng dụng thuộc mọi loại hình và kích cỡ.



-----
Source:
https://media.amazonwebservices.com/AWS_Disaster_Recovery.pdf


