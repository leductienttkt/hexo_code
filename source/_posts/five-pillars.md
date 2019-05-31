---
title: AWS Well-Architected Framework – Five Pillars
categories:
 - Overview
---

![](/images/AWS-Well-Architected-Framework-Five-Pillars.jpg)

### 1. Operational Excellence

* Khả năng chạy và giám sát các hệ thống để cung cấp giá trị kinh doanh và liên tục cải thiện các quy trình và thủ tục hỗ trợ.

* Có 3 lĩnh vực và công cụ thực hành tốt nhất cho operational excellence:
   1. Prepare – AWS Config
   2. Operate – Amazon CloudWatch
   3. Evolve – Amazon Elasticsearch Service

* Key AWS service: **AWS CloudFormation** 
    
### 2. Security

* Khả năng bảo vệ thông tin, hệ thống và tài sản trong khi cung cấp giá trị kinh doanh thông qua các đánh giá rủi ro và chiến lược giảm thiểu.
* Có 5 lĩnh vực và công cụ thực hành tốt nhất để bảo mật:

   1. Identity and Access Management – IAM, Multi-Factor Authentication, AWS Organizations
   2. Detective Controls – AWS CloudTrail, AWS Config, Amazon GuardDuty
   3. Infrastructure Protection – Amazon VPC, Amazon CloudFront with AWS Shield, AWS WAF
   4. Data Protection – ELB, Amazon Elastic Block Store (Amazon EBS), Amazon S3, and Amazon Relational Database Service (Amazon RDS) encryption, Amazon Macie, AWS Key Management Service (AWS KMS)
   5. Incident Response – IAM, Amazon CloudWatch Events

* Key AWS service: **AWS Identity and Access Management (IAM)**

### 3. Reliability

* Khả năng của một hệ thống phục hồi từ sự gián đoạn cơ sở hạ tầng hoặc dịch vụ, tự động có được tài nguyên để đáp ứng nhu cầu và giảm thiểu sự gián đoạn như cấu hình sai hoặc các sự cố mạng tạm thời.
* Có 3 lĩnh vực và công cụ thực hành tốt nhất cho reliability:

   1. Foundations – IAM, Amazon VPC, AWS Trusted Advisor, AWS Shield
   2. Change Management – AWS CloudTrail, AWS Config, Auto Scaling, Amazon CloudWatch
   3. Failure Management – AWS CloudFormation, Amazon S3, AWS KMS, Amazon Glacier

* Key AWS service: **Amazon CloudWatch**

### 4. Performance Efficiency

* Khả năng sử dụng tài nguyên điện toán hiệu quả để đáp ứng các yêu cầu hệ thống và duy trì hiệu quả đó khi nhu cầu thay đổi và công nghệ phát triển. 
* Có 4 lĩnh vực và công cụ thực hành tốt nhất cho performance efficiency:
   1. Selection – Auto Scaling for Compute, Amazon EBS and S3 for Storage, Amazon RDS and DynamoDB for Database, Route53, VPC, and AWS Direct Connect for Network
   2. Review – AWS Blog and What’s New section of the website
   3. Monitoring –  Amazon CloudWatch
   4. Tradeoffs – Amazon Elasticache, Amazon CloudFront, AWS Snowball, Amazon RDS read replicas.
  
* Key AWS service: **Amazon CloudWatch**

### 5. Cost Optimization

* Khả năng tránh hoặc loại bỏ chi phí không cần thiết hoặc tài nguyên dưới mức tối ưu.
*  Có 4 lĩnh vực và công cụ thực hành tốt nhất cho cost optimization:
   1. Cost-Effective Resources – Cost Explorer, Amazon CloudWatch and Trusted Advisor, Amazon Aurora for RDS, AWS Direct Connect with Amazon CloudFront
   2. Matching supply and demand – Auto Scaling
   3. Expenditure Awareness –  AWS Cost Explorer, AWS Budgets
   4. Optimizing Over Time – AWS News Blog and the What’s New section on the AWS website, AWS Trusted Advisor
* Key AWS service: **Cost Explorer**


-----

Source:
https://d1.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf

