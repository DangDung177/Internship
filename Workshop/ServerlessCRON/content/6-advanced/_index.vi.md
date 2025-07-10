---
title : "Lập Lịch nâng cao và Bộ lọc"
date  : "`r Sys.Date()`"
weight: 6
chapter: false
pre   : " <b> 6. </b> "
---

### Tổng quan

Giờ đây bạn đã có một rule cơ bản kích hoạt Lambda theo lịch cố định, chương này sẽ khám phá các **tính năng nâng cao của EventBridge**:

1. Biểu thức CRON chi tiết (chỉ ngày trong tuần, ngày cuối tháng, dải giờ)  
2. So sánh giữa **Rate** và **CRON** — khi nào nên dùng cái nào  
3. **Lọc mẫu sự kiện** để chỉ kích hoạt khi khớp điều kiện cụ thể  
4. **Input transformer** để tùy chỉnh payload  
5. Gửi cùng một sự kiện đến **nhiều target** (Lambda, Step Functions, SNS, v.v.)

---

### Mục tiêu

- Tạo lịch nâng cao (ví dụ: “ngày làm việc cuối cùng của tháng lúc 17:30”)  
- Thêm bộ lọc để chỉ một số sự kiện đi vào Lambda  
- Biến đổi nội dung sự kiện trước khi đến function  
- Gắn nhiều target cho cùng một rule  

---

### 1. Biểu thức CRON chi tiết

Biểu thức CRON chi tiết là lịch dựa trên thời gian cho phép xác định thời điểm kích hoạt cụ thể bằng cú pháp mở rộng của AWS EventBridge. Nó giúp:

- Nhắm đến các ngày/giờ cụ thể hoặc tổ hợp như “Thứ hai đầu tiên” hoặc “ngày cuối tháng”  
- Định nghĩa khung giờ làm việc (ví dụ: mỗi 10 phút từ 8h đến 18h)  
- Lên lịch các mẫu không đơn giản như chỉ ngày trong tuần, theo quý, hoặc cuối tháng

| Tình huống                               | Ví dụ biểu thức           | Ý nghĩa                   | Ứng dụng thực tế                                               |
|------------------------------------------|----------------------------|---------------------------|----------------------------------------------------------------|
| Ngày trong tuần lúc 09:00 (UTC)          | `cron(0 9 ? * MON-FRI *)`  | Thứ 2 -> Thứ 6, 09:00      | Gửi email điểm danh hàng ngày vào sáng các ngày làm việc       |
| Ngày cuối tháng, lúc 23:45               | `cron(45 23 L * ? *)`      | 31/30/28/29 tùy tháng     | Tự động tạo và gửi báo cáo thanh toán hàng tháng qua email     |
| Mỗi 5 phút trong giờ hành chính          | `cron(0/5 8-17 ? * MON-FRI *)` | 08:00–17:55, Thứ 2–6  | Đồng bộ dữ liệu CRM hoặc kiểm tra DB trong giờ làm             |
| Thứ Hai đầu tiên mỗi tháng, 06:00        | `cron(0 6 ? * 2#1 *)`      | `2#1` = Thứ Hai đầu tiên  | Bắt đầu checklist bảo trì hệ thống hàng tháng                  |

> **Tips** – `L`, `W` và `#` là các ký hiệu nâng cao:  
>  • `L` = ngày cuối tháng,  
>  • `W` = ngày làm việc gần nhất,  
>  • `2#1` = Thứ Hai đầu tiên của tháng

Xem cách tạo rule cho EventBridge tại [mục này](/3-eventbridge/#1-Navigate-to-Amazon-EventBridge-Console).

---

### 2. Lịch theo khoảng thời gian (Rate-Based)

Rule kiểu rate định nghĩa lịch kích hoạt đều đặn theo khoảng thời gian như mỗi 5 phút, mỗi giờ, hoặc mỗi ngày.  
Bạn nên dùng rate khi cần tác vụ chạy định kỳ mà không cần lịch theo ngày cụ thể.

Dùng **rate()** cho các khoảng thời gian đơn giản:

| Biểu thức Rate         | Hành vi                                |
|------------------------|----------------------------------------|
| `rate(5 minutes)`      | Mỗi 5 phút                              |
| `rate(1 hour)`         | Mỗi giờ                                 |
| `rate(1 day)`          | Mỗi 24 giờ (cùng thời điểm mỗi ngày)    |

**Gợi ý:** dùng **rate()** khi cần “mỗi _N_ phút/giờ/ngày”, dùng **cron()** khi cần lịch có nhận thức theo ngày tháng.

- Khi tạo rule mới, chọn tương tự như cron  
![advanced-1](https://dangdung177.github.io/Internship/images/6.advanced/advanced-1.png)  
- Trong phần **Define Schedule**, chọn **Schedule pattern** phù hợp  
- Chọn biểu thức Rate bạn muốn dùng  
![advanced-2](https://dangdung177.github.io/Internship/images/6.advanced/advanced-2.png)  
- Tiếp tục tạo rule như [tạo cron rule](/3-eventbridge/#4-Select-Target)

---

### 3. Thêm bộ lọc mẫu sự kiện (Event Pattern)

Khi bạn gửi nhiều loại sự kiện đến EventBridge nhưng chỉ muốn một vài trong số đó đi đến target, bạn có thể dùng bộ lọc event pattern.

Event pattern là điều kiện để quyết định có gửi event đến target hay không.

1. Trong builder, chọn **Event pattern** thay vì **Schedule**  
![advanced-2](https://dangdung177.github.io/Internship/images/6.advanced/advanced-3.png)  
2. Event source: chọn **AWS events** hoặc **Custom pattern**  
3. Định nghĩa bộ lọc JSON, ví dụ:

```json
{
  "source": ["custom.billing"],
  "detail-type": ["MonthlyInvoiceReady"],
  "detail": {
    "amount": [ { "numeric": [">=", 1000] } ]
  }
}
```
Rule này chỉ kích hoạt khi:
- Source là "custom.billing"
- Detail-type đúng
- Amount >= 1000

4. Tiếp tục -> chọn Lambda, SNS, v.v. -> **Create rule**

### 4. Sử dụng Input Transformer
Input Transformer là một tính năng cho phép bạn tùy chỉnh dữ liệu được gửi đến target khi một rule được kích hoạt. Nó giúp bạn định dạng lại hoặc lọc dữ liệu sự kiện trước khi truyền đến target, giúp dễ dàng làm việc với các trường dữ liệu cụ thể hoặc định dạng dữ liệu sao cho phù hợp với dịch vụ nhận.
1. Tạo hoặc sửa rule
2. Trong phần Targets, mở Additional settings
![advanced-2](https://dangdung177.github.io/Internship/images/6.advanced/advanced-4.png)
3. Chọn **Configure target input** -> **Input transformer** -> **Configure input transformer**  
Ví dụ:

```json
{
  "InputPathsMap": {
    "user": "$.detail.userId",
    "total": "$.detail.amount"
  }
}

{
  "InputTemplate": "{ 
    \"userId\": <user>, 
    \"invoiceTotal\": <total> 
  }"
}
```
![advanced-2](https://dangdung177.github.io/Internship/images/6.advanced/advanced-5.png)

Event gốc:
```json
{
  "source": "custom.billing",
  "detail-type": "MonthlyInvoiceReady",
  "detail": {
    "userId": "abc123",
    "amount": 1520.50,
    "plan": "Pro",
    "region": "ap-southeast-1"
  }
}
```
Kết quả gửi đến Lambda:
```json
{
  "userId": "abc123",
  "invoiceTotal": 1520.50
}
```
Điều này giúp giữ hàm Lambda nhẹ dữ liệu, không cần xử lý cả cấu trúc EventBridge đầy đủ.

### 5. Nhiều Target
EventBridge cho phép mỗi rule có tối đa 5 targets. Điều này cho phép kích hoạt nhiều hành động hoặc dịch vụ khác nhau khi một sự kiện khớp với pattern của nó, tạo ra mô hình phân tán rất hữu ích để phát tán một sự kiện đến nhiều dịch vụ cùng lúc.
Ví dụ:
- Lambda -> ghi log hoặc cập nhật DB
- SNS -> thông báo cho bộ phận tài chính
- CloudWatch -> lưu log sự kiện

Khi tạo hoặc sửa rule:
- Vào phần Add target -> chọn Lambda
![advanced-2](https://dangdung177.github.io/Internship/images/6.advanced/advanced-target1.png)
- Nhấn Add another target
- Chọn SNS topic
![advanced-2](https://dangdung177.github.io/Internship/images/6.advanced/advanced-target2.png)
- Cấu hình từng target theo cần thiết
- Nhấn Create rule hoặc Update rule
### ✅ 6. Kết quả đạt được
Giờ đây bạn đã có thể:
- Lập lịch nâng cao với CRON hoặc rate
- Lọc sự kiện chỉ lấy cái bạn cần xử lý
- Biến đổi payload bằng input transformer
- Phân tán sự kiện đến nhiều dịch vụ downstream

Trong chương cuối, chúng ta sẽ dọn dẹp tài nguyên để tránh chi phí phát sinh không cần thiết.