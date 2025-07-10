---
title : "Chuẩn bị code hàm Lambda"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 2.2 </b> "
---
### Tạo Hàm Lambda

Trong bước này, bạn sẽ tạo một **hàm AWS Lambda cơ bản** được kích hoạt theo lịch bởi **Amazon EventBridge**. Đây sẽ là thành phần chính cho CRON job không máy chủ của bạn.

Chúng ta sẽ sử dụng một ví dụ đơn giản ghi lại thông báo vào **Amazon CloudWatch Logs** mỗi khi hàm được chạy.

---

### 1. Truy cập [AWS Lambda Console](https://console.aws.amazon.com/lambda/)

- Tìm kiếm và mở dịch vụ **Lambda**
![lambda-create](/images/2.prerequisite/Lambda-1.png)
- Nhấn vào **Create function**
![lambda-create](/images/2.prerequisite/Lambda-2.png)

---

### 2. Cấu hình hàm Lambda

- Chọn **Author from scratch**
- Đặt tên hàm: `ScheduledLoggerFunction`
- Runtime: `Python 3.13`
![lambda-config](/images/2.prerequisite/Lambda-3.png)
- Phần **Permissions**:
  - Chọn **Use an existing role**
  - Chọn role bạn đã tạo trước đó: `Cron-Lambda-Executor`
- Nhấn **Create function**
![lambda-config](/images/2.prerequisite/Lambda-4.png)

---

### 3. Thêm code ví dụ

Sau khi tạo xong function, cuộn xuống phần **Code** và thay thế mã mặc định bằng đoạn mã sau:

```python
import json  # Dùng để định dạng dữ liệu sự kiện dưới dạng JSON dễ đọc

def lambda_handler(event, context):
    print("✅ Hàm Lambda đã được thực thi theo lịch!")  # Log xác nhận
    print("Chi tiết sự kiện:", json.dumps(event, indent=2))  # In dữ liệu sự kiện từ EventBridge

    return {
        'statusCode': 200,
        'body': json.dumps('Lambda đã chạy thành công')  # Trả về thông báo thành công
    }
```
### 4. Triển khai hàm
Nhấn Deploy(Ctrl+Shift+U) để lưu và áp dụng code bạn vừa thêm.
![lambda-config](/images/2.prerequisite/Lambda-5.png)
Hàm Lambda của bạn đã sẵn sàng. Tiếp theo, chúng ta sẽ kiểm tra xem nó có hoạt động đúng hay không.

### 5. Kiểm tra hàm
- Nhấn nút Test(Ctrl+Shift+I). Nếu chưa có sự kiện test, màn hình sẽ hiển thị **Create new test event**.
- Đặt tên Event: `ScheduledTest`.
- Template: Giữ nguyên `Hello World` hoặc chọn khác tùy ý.
- Nhấn **Save**.
![lambda-config](/images/2.prerequisite/Lambda-6.png)
- Nhấn Test lại một lần nữa để chạy function.
Bạn sẽ thấy kết quả hiển thị trong phần Execution results
![lambda-config](/images/2.prerequisite/Lambda-7.png)

### 6. Kiểm tra Log Events trong CloudWatch

- Chuyển sang tab **Monitor**.
- Nhấn  **View CloudWatch logs**.
![lambda-config](/images/2.prerequisite/Lambda-8.png)
- Tìm **Log streams** mới nhất và nhấn vào nó.
![lambda-config](/images/2.prerequisite/Lambda-9.png)
- Bạn sẽ thấy chi tiết của **Log events**
![lambda-config](/images/2.prerequisite/Lambda-10.png)

Hàm Lambda của bạn đã sẵn sàng. Trong bước tiếp theo, chúng ta sẽ tạo một luật EventBridge để kích hoạt hàm này theo lịch định sẵn.