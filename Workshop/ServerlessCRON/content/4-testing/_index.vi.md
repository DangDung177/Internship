---
title : "Kiểm tra sự kiện đã lên lịch"
date  : "`r Sys.Date()`"
weight: 4
chapter: false
pre   : " <b> 4. </b> "
---

### Tổng quan

Trong chương này, bạn sẽ **xác minh rằng EventBridge rule đã được cấu hình đúng để kích hoạt Lambda function** của bạn.  
Bạn sẽ theo dõi lần gọi đầu tiên gần như theo thời gian thực, kiểm tra đầu ra trong **CloudWatch Logs**, và xác nhận rằng lịch trình chạy tự động đúng như mong đợi.

---

### Mục tiêu

- Xác nhận rằng rule của EventBridge đã được **kích hoạt** và hiển thị đúng thời gian chạy tiếp theo  
- Quan sát ít nhất **1 lần thực thi trực tiếp** của Lambda function  
- Kiểm tra **log stream** trong CloudWatch để xác nhận đầu vào và đầu ra  
- Xem các **metric cơ bản** (Số lần gọi & Thời gian thực thi) để tăng thêm sự tin tưởng  

---

### 1. Xác nhận thời gian chạy tiếp theo của rule

1. Mở **Amazon EventBridge -> Rules**  
2. Nhấn vào **`ScheduledLambdaTrigger`**  
3. Trong phần **Schedule**, kiểm tra danh sách **Next 10 trigger dates**  
   _(Đảm bảo thời gian hiển thị khớp với múi giờ địa phương của bạn hoặc UTC offset)_  
4. Nhấn **Edit**
![rule‑detail](https://dangdung177.github.io/Internship/images/4.testing/testing-1.png)

> **Tips** – Nếu bạn không muốn chờ đến đúng giờ, hãy tạm thời chỉnh biểu thức CRON thành vài phút phía trước, nhấn _Save_, rồi chỉnh lại sau khi test.
![rule‑detail](https://dangdung177.github.io/Internship/images/4.testing/testing-2.png)  
Giải thích:  
`cron(0/3 * * * ? *)` = Chạy mỗi 3 phút mỗi ngày

---

### 2. Chờ lần gọi đầu tiên theo lịch

- Vào **Lambda -> Monitor -> tab Recent invocations**  
- Nhấn refresh sau thời gian mong đợi; bạn sẽ thấy **1 lần thực thi mới**

![lambda‑invocation](https://dangdung177.github.io/Internship/images/4.testing/testing-3.png)

---

### 3. Kiểm tra log trong CloudWatch

1. Trong Lambda console, chọn **Monitor -> View logs in CloudWatch**  
2. Nhấn vào **log stream mới nhất** (thời gian khớp với thời gian gọi)  
3. Xác nhận bạn thấy hai dòng log mà bạn đã lập trình trước đó:
![lambda‑invocation](https://dangdung177.github.io/Internship/images/4.testing/testing-4.png)

---

### 4. Xác minh metrics

- Vẫn ở tab **Monitor**, chuyển sang **CloudWatch metrics**  
- Xác nhận **Invocations = 1** và **Errors = 0**  
- Nếu bạn để rule tiếp tục chạy, biểu đồ sẽ cập nhật mỗi chu kỳ

![metrics](https://dangdung177.github.io/Internship/images/4.testing/testing-5.png)

---

### 5. (Tuỳ chọn) Kiểm tra lại thủ công

Nếu bạn cần chạy lại ngay lập tức mà không thay đổi lịch:

Bạn có thể test Lambda bằng cách [chuyển đến phần này](/2-preparation/2.2-preparelambda/#5-test-the-function).

---

### ✅ Kết quả

Bạn đã xác nhận rằng:

- **EventBridge** kích hoạt theo đúng lịch  
- **Lambda** thực thi thành công với đúng vai trò IAM  
- **CloudWatch Logs & Metrics** lưu lại đầy đủ thông tin thực thi  

Trong chương tiếp theo, chúng ta sẽ thêm các **kỹ thuật giám sát nâng cao** — cảnh báo, giữ log, và metric tuỳ chỉnh — để đảm bảo CRON job serverless của bạn hoạt động ổn định trong môi trường sản xuất.
