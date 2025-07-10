---
title : "Xây dựng Serverless CRON Jobs Với EventBridge Và Lambda"
date : "`r Sys.Date()`"
weight : 1
chapter : false
---

# Xây dựng Serverless CRON Jobs với EventBridge và Lambda

### Tổng quan
Trong bài lab này, bạn sẽ học cách tự động hóa các tác vụ theo lịch trình bằng cách sử dụng Amazon EventBridge và AWS Lambda. CRON jobs rất hữu ích để thực hiện các tác vụ lặp lại như dọn dẹp dữ liệu, tạo báo cáo, hoặc giám sát hệ thống.

Chúng ta sẽ sử dụng Amazon EventBridge để định nghĩa biểu thức CRON và kích hoạt các hàm Lambda mà không cần quản lý máy chủ. Điều này giúp bạn triển khai giải pháp tự động hóa có khả năng mở rộng, tiết kiệm chi phí và hoàn toàn không máy chủ.

![ServerlessCRON](https://dangdung177.github.io/Internship/images/cron-architecture-vi.png) 

### Nội dung
 1. [Giới Thiệu](1-introduce/)
 2. [Chuẩn Bị](2-preparation/)
 3. [Thiết Lập Quy Tắc cho EventBridge](3-eventbridge/)
 4. [Kiểm Tra Lịch Trình](4-testing/)
 5. [Giám Sát với CloudWatch Logs](5-cloudwatch/)
 6. [Lập Lịch nâng cao và Bộ lọc](6-advanced/)
 7. [Dọn Dẹp Tài Nguyên](7-cleanup/)
