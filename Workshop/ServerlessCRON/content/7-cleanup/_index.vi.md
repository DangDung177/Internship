---
title : "Dọn Dẹp Tài Nguyên"
date  : "`r Sys.Date()`"
weight: 7
chapter: false
pre   : " <b> 7. </b> "
---

### Tổng quan
Để tránh các khoản chi phí không cần thiết và giữ cho môi trường AWS của bạn gọn gàng, điều quan trọng là phải xóa những tài nguyên đã được tạo cho mục đích thử nghiệm hoặc trình diễn. Trong chương cuối cùng này, bạn sẽ thực hiện dọn dẹp các quy tắc EventBridge, hàm Lambda, chủ đề SNS và nhật ký CloudWatch được tạo trong suốt workshop này.

### 1. Xóa các Rule trên EventBridge
Truy cập [Amazon EventBridge Dashboard](https://console.aws.amazon.com/events/home)
- Nhấp **Rules**.
- Chọn Rule cần xóa.
- Nhấp **Delete**.
- Xác nhận xóa.
![Clean](/images/7.clean/clean-2.png)

### 2. Xóa IAM
Truy cập [IAM service management console](https://console.aws.amazon.com/iamv2/home#)
Nhấp **Roles**.
Trong ô tìm kiếm, nhập **Lambda**.
Chọn **Roles** mà bạn đã tạo.
Nhấp **Delete** và xác nhận để xóa role.
![Clean](/images/7.clean/clean-1.png)

### 3. Xóa Lambda
Truy cập [Lambda management console](https://console.aws.amazon.com/lambda/home#)
- Nhấp **Functions**.
- Trong danh sách **Functions**, tìm các hàm Lambda được sử dụng trong workshop.
- Nhấp để chọn các hàm cần xóa.
- Với mỗi hàm, chọn **Actions** -> **Delete** và xác nhận xóa.
![Clean](/images/7.clean/clean-3.png)

### 4. Xóa SNS Topics và Subscriptions
Truy cập [Amazon SNS service management console](https://console.aws.amazon.com/sns/v3/home).
- Chuyển đến Topics và xóa topic bạn đã tạo (ví dụ: "Default_CloudWatch_Alarms_Topic").
![Clean](/images/7.clean/clean-4.png)
- Chuyển đến Subscriptions, hủy đăng ký hoặc xóa các subscription đang hoạt động.
![Clean](/images/7.clean/clean-5.png)

### 5. Xóa CloudWatch Log Groups và Alarms
Truy cập [Cloudwatch service management console](https://console.aws.amazon.com/cloudwatch/home)
   - Nhấp **All alarms**.
   - Chọn tất cả alarm được tạo cho workshop.
   - Nhấp **Actions** -> **Delete**.
   - Vào **Log groups**.
   - Chọn tất cả log group được tạo cho workshop.
   - Nhấp **Actions** -> **Delete log groups**.
   ![Clean](/images/7.clean/clean-6.png)