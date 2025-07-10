---
title : "Giám sát với CloudWatch Logs"
date  : "`r Sys.Date()`"
weight: 5
chapter: false
pre   : " <b> 5. </b> "
---

### Tổng quan

Trong chương này, bạn sẽ học cách **giám sát việc thực thi hàm Lambda** của mình bằng cách sử dụng **Amazon CloudWatch Logs** và **CloudWatch Metrics**. Các công cụ này cho phép bạn theo dõi các tác vụ CRON đã lên lịch và xử lý lỗi hoặc hành vi bất thường.

CloudWatch được tích hợp tự động với Lambda khi bạn cấp quyền `AWSLambdaBasicExecutionRole`.

---

### Mục tiêu

- Khám phá log được tạo bởi Lambda  
- Hiểu cấu trúc của log stream và log event  
- Xem các chỉ số gọi hàm (số lần, thời lượng, tỷ lệ lỗi)  
- Thiết lập công cụ giám sát cơ bản để sẵn sàng cho môi trường sản xuất  

---

### 1. Truy cập CloudWatch Logs

1. Truy cập [Amazon CloudWatch Console](https://console.aws.amazon.com/cloudwatch/)  
2. Trong thanh điều hướng bên trái, nhấn **Logs -> Log groups**  
3. Tìm nhóm log của Lambda (ví dụ: `/aws/lambda/ScheduledLoggerFunction`)  
4. Nhấn vào **Log group** để xem các stream  
5. Nhấn vào **log stream mới nhất** để xem chi tiết log

![cloudwatch-logs](https://dangdung177.github.io/Internship/images/5.cloudwatch/cloudwatch-1.png)

---

### 2. Hiểu cấu trúc của log

Mỗi lần Lambda được gọi sẽ tạo ra một **log stream** chứa:

- Dòng **START** (thời gian và request ID)  
- Lệnh **PRINT** từ code của bạn (ví dụ: "✅ Lambda function executed")  
- Dòng **END** và **REPORT** thể hiện thời lượng chạy, bộ nhớ đã dùng, và thông tin tính phí  

Log này giúp bạn xác minh rằng:

- Hàm được gọi đúng thời điểm  
- Đầu vào và đầu ra đúng như mong đợi  
- Không có lỗi hoặc timeout xảy ra  

![cloudwatch-stream](https://dangdung177.github.io/Internship/images/5.cloudwatch/cloudwatch-2.png)

---

### 3. Kiểm tra CloudWatch Metrics

1. Chuyển sang tab **Monitor** trong giao diện Lambda  
2. Xem các chỉ số quan trọng như:  
   - **Invocations**: số lần hàm được gọi  
   - **Duration**: thời lượng trung bình và tối đa  
   - **Errors**: số lần lỗi  
   - **Throttles**: Lambda bị giới hạn do vượt quota  
3. Bạn có thể tùy chỉnh biểu đồ và thời gian hiển thị  
![cloudwatch-metrics](https://dangdung177.github.io/Internship/images/5.cloudwatch/cloudwatch-4.png)

> ✅ Các chỉ số này giúp bạn nhanh chóng phát hiện các vấn đề như: thời lượng tăng cao, lịch chạy bị bỏ lỡ hoặc tỷ lệ lỗi tăng.

![cloudwatch-metrics](https://dangdung177.github.io/Internship/images/5.cloudwatch/cloudwatch-3.png)

---

### 4. Thiết lập cảnh báo (Tùy chọn)

Bạn có thể thiết lập cảnh báo CloudWatch để nhận thông báo khi có sự cố:

- Truy cập **CloudWatch -> Alarms -> Create Alarm**  
![cloudwatch-metrics](https://dangdung177.github.io/Internship/images/5.cloudwatch/cloudwatch-5.png)
- Chọn metric của Lambda bạn muốn giám sát (ví dụ: Errors > 0)  
![cloudwatch-metrics](https://dangdung177.github.io/Internship/images/5.cloudwatch/cloudwatch-6.png)
- Trong ví dụ này, chọn **Errors**
![cloudwatch-metrics](https://dangdung177.github.io/Internship/images/5.cloudwatch/cloudwatch-7.png)
- Định nghĩa điều kiện kích hoạt cảnh báo  
![cloudwatch-metrics](https://dangdung177.github.io/Internship/images/5.cloudwatch/cloudwatch-8.png)
- Sau đó chọn hành động thông báo. Trong ví dụ này, chúng ta sẽ chọn gửi email qua SNS.
   - Đảm bảo bạn đã xác nhận đăng ký chủ đề SNS. Thực hiện bằng cách vào **Amazon SNS** -> **Subscriptions** -> **Request Confirmation** -> Kiểm tra hộp thư **Gmail** của bạn (email có thể nằm trong **thùng rác**) -> **Confirm Subscription**
   ![cloudwatch-metrics](https://dangdung177.github.io/Internship/images/5.cloudwatch/cloudwatch-gmail.png)

![cloudwatch-metrics](https://dangdung177.github.io/Internship/images/5.cloudwatch/cloudwatch-9.png)
- Bạn cũng có thể chọn hành động khác như Lambda, Auto Scaling, EC2, Systems Manager hoặc Investigation Action  
![cloudwatch-metrics](https://dangdung177.github.io/Internship/images/5.cloudwatch/cloudwatch-10.png)
- Đặt tên cho cảnh báo và nhấn **Next**
![cloudwatch-metrics](https://dangdung177.github.io/Internship/images/5.cloudwatch/cloudwatch-11.png)
- Kiểm tra lại toàn bộ cấu hình và nhấn **Create alarm**
![cloudwatch-metrics](https://dangdung177.github.io/Internship/images/5.cloudwatch/cloudwatch-12.png)

> Đây là cách thực hành tốt nhất để đảm bảo CRON job của bạn sẵn sàng hoạt động ổn định trong môi trường sản xuất.

---

### ✅ Kết quả

Bạn đã học cách:

- Giám sát Lambda với CloudWatch Logs  
- Đọc log để xử lý lỗi  
- Dùng metric để đánh giá hiệu suất  
- (Tùy chọn) Thiết lập cảnh báo sớm để phát hiện lỗi  

Trong chương tiếp theo, bạn sẽ khám phá các tính năng nâng cao của EventBridge như lọc sự kiện, payload tùy chỉnh và biểu thức CRON linh hoạt hơn.
