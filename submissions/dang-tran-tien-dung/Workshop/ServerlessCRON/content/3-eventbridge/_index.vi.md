---
title : "Thiết lập quy tắc EventBridge"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

### Tổng quan

Trong chương này, bạn sẽ tạo một **quy tắc định kỳ trong Amazon EventBridge** sử dụng biểu thức CRON để kích hoạt **hàm Lambda** của bạn vào thời điểm hoặc khoảng thời gian cụ thể.

Amazon EventBridge hỗ trợ **lên lịch serverless**, loại bỏ nhu cầu sử dụng các máy chủ cron truyền thống. Quy tắc này sẽ đóng vai trò là cơ chế kích hoạt cho các CRON job của bạn.

---

### Mục tiêu

- Định nghĩa lịch CRON sử dụng **EventBridge Rule**
- Kích hoạt hàm Lambda đã tạo trước đó
- Xác minh việc liên kết và hoạt động đúng

---

### 1. Điều hướng đến Amazon EventBridge Console

- Mở [Amazon EventBridge Console](https://console.aws.amazon.com/events/)
- Từ menu bên trái, chọn **Rules**
- Nhấn vào **Create rule**

![eventbridge1](/images/3.eventbridge/eventbridge-1.png)
![eventbridge2](/images/3.eventbridge/eventbridge-2.png)

---

### 2. Cấu hình cơ bản cho Quy Tắc

- Tên: `ScheduledLambdaTrigger`
- Mô tả: _Trigger Lambda function based on schedule_  
- Với **Rule type**, chọn: `Schedule`
- Chọn **Enable the rule on the selected event bus**
- Nhấn **Continue to create rule**

![eventbridge3](/images/3.eventbridge/eventbridge-3.png)

---

### 3. Định nghĩa lịch biểu (Schedule Pattern)

- Chọn **Recurring schedule**
- Trong **Schedule pattern**, chọn **CRON-based schedule expression**
- Ví dụ:  cron(0 10 * * ? *)  
-> Lệnh này sẽ chạy hàng ngày lúc 10:00 sáng theo giờ GMT+7 (Vì giờ Việt Nam nhanh hơn GMT 7 tiếng nên cần giảm Giờ 7 tiếng)
- Nhấn **Next**

![eventbridge3](/images/3.eventbridge/eventbridge-4.png)

---

### 4. Chọn mục tiêu (Target)

- Kiểu mục tiêu: **AWS service**
- Chọn **Lambda function**
- Tên hàm: `ScheduledLoggerFunction` (hoặc tên hàm của bạn)
- Với **Execution Role**, chọn **Create a new role for this specific resource** hoặc **Use existing role** nếu được yêu cầu

> Đảm bảo IAM Role của Lambda có quyền phù hợp.

- Nhấn **Next**

![eventbridge4](/images/3.eventbridge/eventbridge-5.png)

---

### 5. Kiểm tra và tạo quy tắc

Xem lại tất cả các cấu hình:
- Chi tiết quy tắc
- Lịch đã xây dựng
- Mục tiêu đã định nghĩa
- Gắn thẻ (nếu cần)
- Nhấn **Create rule**

![eventbridge4](/images/3.eventbridge/eventbridge-6.png)

---

### Kết quả

Quy tắc EventBridge của chúng ta đã hoạt động và sẽ **tự động gọi Lambda function** theo lịch bạn đã định nghĩa.

Bạn có thể:
- Theo dõi lần gọi hàm qua **CloudWatch Logs**
- Thay đổi biểu thức CRON bất cứ lúc nào
- Thêm nhiều mục tiêu nếu cần

---

Trong chương tiếp theo, chúng ta sẽ kiểm tra CRON job bằng cách kích hoạt thủ công và quan sát log và kết quả thực thi.
