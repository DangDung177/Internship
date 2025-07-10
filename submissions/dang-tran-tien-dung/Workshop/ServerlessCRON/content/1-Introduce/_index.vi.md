---
title : "Giới Thiệu"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "   
---

### CRON Jobs là gì?

**CRON jobs** là các tác vụ được lập lịch theo thời gian, thường được sử dụng trong quản trị hệ thống và tự động hóa. Từ "CRON" bắt nguồn từ tiếng Hy Lạp *chronos*, có nghĩa là **thời gian**.

Thông thường, các CRON job sẽ được định nghĩa trong tệp **crontab** và sau đó được thực thi bởi bộ lập lịch cục bộ trên máy chủ.

**CRON job** là thuật ngữ dùng để chỉ các tác vụ được lên lịch chạy định kỳ tại:

- Thời điểm cố định (ví dụ: 09:30 sáng)

- Khoảng thời gian cố định (ví dụ: mỗi 15 phút)

- Ngày cụ thể trong tuần hoặc trong tháng (ví dụ: mỗi thứ Hai)

![CRONJobs](/images/cron-table.png) 
Các ký tự đặc biệt sau được hỗ trợ trong AWS EventBridge:

- **(*)**: Tất cả giá trị (mỗi ngày, mỗi giờ,...).
- **(?)**: Không có giá trị cụ thể (chỉ được sử dụng ở trường ngày trong tháng hoặc ngày trong tuần, không được dùng cả hai!!).
- **(,)**: Phân tách nhiều giá trị (ví dụ: 1,4 nghĩa là T2 và T5).
- **(-)**: Khoảng giá trị (ví dụ: 2-5 tương đương T3-T6).
- **(/)**: Tăng theo bội số (ví dụ: 0/15 = cứ mỗi 15 phút bắt đầu từ phút thứ 0).
- **(L)**: Chỉ định ngày cuối cùng của tháng hoặc tuần.
- **(W)**: Chỉ định ngày trong tuần gần nhất (T2-T6) với một ngày cụ thể trong tháng (15W, 3W, ...).
    - Nếu ngày đó là ngày trong tuần, nó sẽ chạy vào đúng ngày đó.
    - Nếu rơi vào cuối tuần (Thứ Bảy hoặc Chủ Nhật), nó sẽ dịch chuyển đến ngày trong tuần gần nhất (Thứ Sáu hoặc Thứ Hai).

Cú pháp CRON hoạt động như sau:  
`cron(Phút Giờ Ngày-trong-tháng Tháng Ngày-trong-tuần Năm)`  
Ví dụ:  
`cron(0 10 * * ? *)` = Chạy vào lúc 10:00 sáng (UTC+0) mỗi ngày
![CRONJobs](/images/cron-breakdown-vi.png) 

---

### Vì sao nên sử dụng serverless CRON Jobs?

Với **AWS EventBridge** và **AWS Lambda**, chúng ta có thể thay thế cấu hình CRON truyền thống bằng một giải pháp **serverless** với nhiều lợi ích:

- **Được AWS quản lý hoàn toàn**: Giúp nhà phát triển tập trung vào các công việc khác thay vì lo về hạ tầng.

- **Tiết kiệm chi phí**: Chỉ trả tiền cho thời gian thực thi của Lambda khi nó chạy, thay vì duy trì một máy chủ hoạt động liên tục để chạy tác vụ theo lịch.

- **Khả năng mở rộng cao**: Nền tảng serverless tự động mở rộng theo nhu cầu mà không cần can thiệp thủ công.

- **Dễ tích hợp hơn**: Nền tảng serverless thường rất dễ tích hợp với các dịch vụ khác của AWS như S3, DynamoDB, SNS,...

Với cách tiếp cận này, chúng ta có thể định nghĩa các tác vụ bằng **biểu thức CRON trong EventBridge** và thực thi chúng qua **hàm Lambda** — hoàn toàn không cần triển khai bất kỳ cơ sở hạ tầng nào.

---

### Amazon EventBridge

![AmazonEventBridge](/images/eventbridge.png)  
**Amazon EventBridge** là một dịch vụ serverless của AWS, EventBridge giúp kết nối các thành phần khác nhau của ứng dụng thông qua các sự kiện, hỗ trợ chúng ta xây dựng các ứng dụng theo hướng sự kiện có khả năng mở rộng.
EventBridge hỗ trợ **biểu thức CRON**, cho phép chúng ta chạy các tác vụ theo lịch (CRON jobs) mà không cần lo lắng về cơ sở hạ tầng.

---

### AWS Lambda

![AmazonLambda](/images/lambda.png)  
**AWS Lambda** là một dịch vụ Cloud serverless giúp chúng ta chạy mã nguồn mà không cần gánh nặng vận hành hoặc cấp phát máy chủ. Lambda tự động mở rộng và quản lý toàn bộ hạ tầng cần thiết để chạy mã của chúng ta khi có sự kiện — bao gồm cả các sự kiện từ EventBridge.

---

### Amazon CloudWatch

![AmazonCloudWatch](/images/cloudwatch.png)  
**Amazon CloudWatch** là dịch vụ giám sát các tài nguyên AWS và ứng dụng của chúng ta trong thời gian thực. Dịch vụ này cho phép chúng ta thu thập log, theo dõi chỉ số và thiết lập cảnh báo. CloudWatch là công cụ thiết yếu giúp theo dõi, chẩn đoán và hiểu rõ tình trạng hoạt động của ứng dụng và việc sử dụng tài nguyên.

---

Với EventBridge và Lambda, chúng ta có thể tự động hóa các tác vụ như:

- Tạo báo cáo hàng ngày hoặc hàng tuần.

- Dọn dẹp dữ liệu cũ không còn sử dụng.

- Lên lịch sao lưu cơ sở dữ liệu hoặc đồng bộ định kỳ.

- Gửi thông báo hoặc cảnh báo.

- Kiểm tra sức khỏe hệ thống hoặc đánh giá định kỳ.

---

Trong bài lab này, chúng ta sẽ từng bước xây dựng một pipeline CRON job serverless hoàn chỉnh, từ lập lịch, thực thi, giám sát đến cảnh báo — mà không cần triển khai hoặc quản lý bất kỳ máy chủ nào.
