---
title : "Clean Up Resources"
date  : "`r Sys.Date()`"
weight: 7
chapter: false
pre   : " <b> 7. </b> "
---

### Overview
To avoid unnecessary charges and keep your AWS environment clean, it’s important to remove resources that were created for testing or demonstration purposes. In this final chapter, you'll walk through cleaning up EventBridge rules, Lambda functions, SNS topics, and CloudWatch logs created during this workshop.

### 1. Delete EventBridge Rules
Go to [Amazon EventBridge Dashboard](https://console.aws.amazon.com/events/home)
- Click **Rules**.
- Select Rules Instance.
- Click **Delete**.
- Confirm deletion.
![Clean](https://dangdung177.github.io/Internship/images/7.clean/clean-2.png)

### 2. Delete IAM 
Go to [IAM service management console](https://console.aws.amazon.com/iamv2/home#)
- Click **Roles**.
- In the search box, enter **Lambda**.
- Select **Role** you have created.
- Click **Delete**, and confirm deletion to delete the role.
![Clean](https://dangdung177.github.io/Internship/images/7.clean/clean-1.png)

### 3. Delete Lambda
Go to [Lambda management console](https://console.aws.amazon.com/lambda/home#)
- Click **Functions**.
- In the **Functions** list, find the Lambda functions used in this workshop.
- Click to select functions you want to delete.
- Select each one and choose **Actions** -> **Delete** and confirm deletion to delete the functions.
![Clean](https://dangdung177.github.io/Internship/images/7.clean/clean-3.png)

### 4. Delete SNS Topics and Subscriptions
Access [Amazon SNS service management console](https://console.aws.amazon.com/sns/v3/home).
- Navigate to Topics and delete any topic you created (e.g., "Default_CloudWatch_Alarms_Topic").
![Clean](https://dangdung177.github.io/Internship/images/7.clean/clean-4.png)
- Go to Subscriptions, unsubscribe or delete any active subscriptions
![Clean](https://dangdung177.github.io/Internship/images/7.clean/clean-5.png)

### 5. Delete CloudWatch Log Groups and Alarms
Go to [Cloudwatch service management console](https://console.aws.amazon.com/cloudwatch/home)
   1. Click **All alarms**.
   2. Choose all alarms created for this workshop.
   3. Click **Actions** -> **Delete**.
   4. Go to **Log groups**
   5. Choose all groups created for this workshop
   6. Click **Actions** -> **Delete log groups**.
   ![Clean](https://dangdung177.github.io/Internship/images/7.clean/clean-6.png)