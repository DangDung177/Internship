---
title : "Chuẩn Bị"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

{{% notice info %}}
Để thực hiện bài lab này, bạn cần có quyền tạo và quản lý Lambda functions, EventBridge rules và CloudWatch Logs trên tài khoản AWS của mình.
{{% /notice %}}

Trong bài lab này, bạn sẽ xây dựng một hệ thống CRON job không máy chủ sử dụng **Amazon EventBridge** và **AWS Lambda**, cùng với **Amazon CloudWatch** để giám sát. Trước khi đi vào triển khai, chúng ta sẽ chuẩn bị đầy đủ các quyền và tài nguyên cần thiết.

Bạn cũng sẽ chuẩn bị sẵn một hàm Lambda đơn giản để sử dụng cho việc chạy định kỳ.

### Yêu cầu:
- Một tài khoản AWS với quyền truy cập:
  - AWS Lambda
  - Amazon EventBridge
  - Amazon CloudWatch Logs
  - IAM (để tạo role)
- Có thể truy cập **AWS Management Console** hoặc sử dụng **AWS CLI** (tùy chọn)

### Trong phần này, chúng ta sẽ:
- Tạo một IAM Role với quyền thực thi Lambda và ghi log
- Chuẩn bị mã nguồn cho một Lambda function mẫu (Node.js hoặc Python)
- (Tùy chọn) Thiết lập payload test hoặc cấu trúc log cơ bản

### Nội dung
  - [Tạo IAM Role cho Lambda](2.1-createiamrole/)
  - [Chuẩn bị code hàm Lambda](2.2-preparelambda/)
