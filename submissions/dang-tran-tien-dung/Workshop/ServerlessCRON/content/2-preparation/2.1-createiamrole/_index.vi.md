---
title : "Tạo IAM Role cho Lambda"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

### Tổng quan

Trong phần này, bạn sẽ tạo một **IAM Role cho AWS Lambda** với các quyền cần thiết để thực thi mã và ghi log vào **Amazon CloudWatch**. Role tuân theo nguyên tắc bảo mật của AWS bằng cách chỉ cấp các quyền tối thiểu cần thiết.

{{% notice note %}}
**Lưu ý bảo mật:** IAM Role cho phép hàm Lambda tương tác an toàn với các dịch vụ AWS khác thông qua **thông tin xác thực tạm thời**, thay vì nhúng khóa truy cập dài hạn.
{{% /notice %}}

---

### Các bước thực hiện

#### 1. Mở IAM Console
- Đăng nhập **AWS Management Console**
- Tìm và mở dịch vụ **IAM**
![IAM](/images/2.prerequisite/IAM-1.png)

#### 2. Tạo Role mới
- Ở thanh điều hướng bên trái, chọn **Roles**
- Nhấn **Create role**
![IAM-CreateRole](/images/2.prerequisite/IAM-2.png)
![IAM](/images/2.prerequisite/IAM-3.png)

#### 3. Chọn loại thực thể tin cậy
- Trong **Trusted entity type**, chọn **AWS service**
- Chọn **Lambda** làm trường hợp sử dụng
- Nhấn **Next**
![IAM](/images/2.prerequisite/IAM-4.png)

#### 4. Gán quyền
- Trong ô tìm kiếm, nhập: `AWSLambdaBasicExecutionRole`
- Chọn chính sách được quản lý **AWSLambdaBasicExecutionRole**  
  - Cho phép hàm Lambda ghi log vào **Amazon CloudWatch Logs**
- (Tùy chọn) Nếu Lambda cần truy cập S3, DynamoDB, SNS,… hãy gán thêm quyền tại đây
- Nhấn **Next**
![IAM](/images/2.prerequisite/IAM-5.png)

{{% notice info %}}
Bạn có thể gán thêm chính sách sau này nếu hàm Lambda cần truy cập các dịch vụ AWS khác.
{{% /notice %}}

#### 5. Cấu hình chi tiết Role
- Đặt tên cho Role, ví dụ: `Cron-Lambda-Executor`
- (Tùy chọn) Thêm mô tả, chẳng hạn: _"Cho phép Lambda gọi các dịch vụ AWS thay bạn."_
- Xem lại trust policy và quyền
- Nhấn **Create role**
![IAM](/images/2.prerequisite/IAM-6.png)
![IAM](/images/2.prerequisite/IAM-7.png)

🔐 *Quan trọng:* Tên Role phải duy nhất và **không phân biệt chữ hoa – chữ thường**; ví dụ `cron-lambda-executor` và `Cron-Lambda-Executor` được xem là trùng.

---

### ✅ Xác nhận tạo Role
- Bạn sẽ thấy thông báo tạo thành công
![IAM](/images/2.prerequisite/IAM-8.png)
- Mở trang chi tiết của Role vừa tạo
- Ghi lại **Role ARN** — dùng khi tạo hoặc cập nhật hàm Lambda
- Đảm bảo **AWSLambdaBasicExecutionRole** đã được gán
![IAM](/images/2.prerequisite/IAM-9.png)

---

IAM Role của bạn đã sẵn sàng để được gán cho hàm Lambda nhằm chạy các CRON job theo lịch với EventBridge.
