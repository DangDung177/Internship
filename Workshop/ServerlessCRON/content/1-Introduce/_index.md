---
title : "Introduction"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "   
---
### What is CRON Jobs?
**CRON jobs** are time-based schedule jobs commonly used in system administration and automation. The word "CRON" derived from the Greek word chronos, meaning **time**.
Normally, CRON jobs would be defined in a **crontab** file and subsequently executed by a local scheduler on the server.
**CRON job** is the term used to name jobs that are scheduled to run periodically at: 
- Fixed time (e.g., 09:30 AM)
- Fixed interval (e.g., every 15 minutes)
- Specific days of the week or month (e.g., every Monday)
![CRONJobs](https://https://dangdung177.github.io/Internship/images/cron-table.png) 
The following special characters are supported in AWS EventBridge:

- **(*)**: All value (every day, every hour,... ).
- **(?)**: No specific value (used in day-of-month or day-of-week, **never both!!**).
- **(,)**: Separates multiple values ( 1,4 means Monday and Thursday).
- **(-)**: Range of values (e.g., 2-5 = Tuesday to Friday).
- **(/)**: Increments (0/15 = every 15 mins starting at minute 0).
- **(L)**: Specifies the last day of the month or week.
- **(W)**: Specifies a nearest weekday(Monday to Friday) to a given day (15W, 3W, ...).
    - If it is a weekday, it runs on that day
    - If it’s a weekend(Saturday and Sunday), it shifts to the nearest weekday(Friday or Monday)

CRON syntax work like this:  
```cron(Minutes Hours Day-of-month Month Day-of-week Year)```

Examples:  
`cron(0 10 * * ? *)` = Run at 10:00 AM (UTC+0) every day
![CRONJobs](/images/cron-breakdown.png) 

### Why Serverless CRON Jobs?
With  **AWS EventBridge** and **AWS Lambda**, we can replace our CRON settings with a serverless solution that is:

- **Fully managed by AWS**: Allow developers to focus on other task.

- **Cost-efficient**: Only pay for the compute time your Lambda function consumes when it runs instead of maintaining a constantly running server for scheduled tasks.

- **Highly scalable**: Serverless platform handles scaling automatically with demand.

- **Easier to integrate**: Serverless platforms are usually very easy to integrate with other AWS services such as S3, DynamoDB, SNS, ...

This approach allow we to define jobs using **CRON expressions in EventBridge** and perform them via **Lambda functions** - all without provisioning any infrastructure.

### Amazon EventBridge
![AmazonEventBridge](/images/eventbridge.png) 
**Amazon EventBridge** is a serverless event bus service that uses events to connect different components of your applications, helping you build scalable event-driven applications.
EventBridge support **CRON expressions**, letting you run scheduled tasks (CRON jobs) without worrying about the infrastructure.

### Amazon Lambda
![AmazonLambda](/images/lambda.png) 
**Amazon Lambda** is a compute service that lets you run code without operational overhead of provisioning servers. It automatically scales, and manages all the infrastructure required to run your code in response to any kind of events - including those from EventBridge rules.

### Amazon CloudWatch 
![AmazonCloudWatch](/images/cloudwatch.png) 
**Amazon CloudWatch** is a monitoring service for AWS resources and your applications in real time. It allows you to collect logs and metrics and set alarms. CloudWatch is essential to tracking, diagnosing, and understandings application health and resource use.

---

Using EventBridge and Lambda, you can automate tasks such as:

- Daily or weekly reports.

- Cleaning up stale data.

- Scheduled database backups or sync jobs.

- Sending alerts or notifications.

- System health checks or audits.

---

In this lab, you'll walk through building a complete serverless CRON job pipeline, from scheduling, to execution, monitoring, and alerting — without provisioning or managing a single server.

