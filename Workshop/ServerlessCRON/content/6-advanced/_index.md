---
title : "Advanced Scheduling and Filtering"
date  : "`r Sys.Date()`"
weight: 6
chapter: false
pre   : " <b> 6. </b> "
---

### Overview

Now that you have a basic rule triggering your Lambda on a fixed schedule, this chapter explores **advanced EventBridge capabilities**:

1. Fine‑grained CRON expressions (weekdays only, last day of month, ranges)  
2. **Rate** versus **CRON** patterns—when to use each  
3. **Event pattern filtering** to invoke targets only when specific conditions match  
4. **Input transformers** for custom payloads  
5. Sending the same event to **multiple targets** (Lambda, Step Functions, SNS, etc.)

---

### Objectives

- Build complex schedules (e.g., “every last weekday of the month at 17:30”)  
- Add filters so that only certain events reach your Lambda  
- Transform the event body before it hits your function  
- Attach more than one target to a single rule  

---

### 1. Fine‑grained CRON Expressions
A fine‑grained CRON expression is a precise time-based schedule that allows you to define highly specific trigger times using the extended CRON syntax supported by AWS EventBridge. They reduce complexity, minimize unnecessary invocations, and ensure your jobs align with real-world schedules.  
It lets you:
- Target specific days, hours, or even combinations like “first Monday” or “last day of month”.
- Define business-hour windows (e.g., every 10 minutes from 8am to 6pm).
- Schedule non-trivial patterns such as weekdays only, quarterly runs, or end-of-month logic.

| Use‑case                              | Expression Example | Meaning | Potential Use|
|---------------------------------------|--------------------|---------|--------------|
| Weekdays at 09:00 (UTC)               | `cron(0 9 ? * MON-FRI *)` | Mon -> Fri, 09:00 | Send a daily standup email to the team every weekday morning.|
| Last calendar day of month, 23:45     | `cron(45 23 L * ? *)`     | 31 st / 30 th / 28 th / 29 th |Automatically generate and email monthly billing reports.|
| Every 5 minutes during business hours | `cron(0/5 8-17 ? * MON-FRI *)` | 08:00–17:55, Mon–Fri |Poll a database for updates or sync CRM data throughout the workday.|
| First Monday of every month, 06:00    | `cron(0 6 ? * 2#1 *)` | `2#1` = first Monday |Kick off the monthly system maintenance checklist or audits.|

> **Tip** – `L`, `W`, and `#` are advanced symbols:  
>  • `L` = last day,  
>  • `W` = nearest weekday,  
>  • `2#1` = Day‑of‑week #nth (Monday #1 = first Monday).

To check how to create rule for EventBridge by [go to this section](/3-eventbridge/#1-Navigate-to-Amazon-EventBridge-Console).

---

### 2. Rate–Based Schedules
A rate-based rule is a rule that defines a schedule that triggers an event at a fixed interval, such as every 5 minutes, every hour, or every day.
You should use Rate-based rules when you want to trigger an action at consistent, fixed intervals without worrying about complex scheduling syntax.

Use **rate()** for simple intervals: 

| Rate expression          | Behaviour                        |
|--------------------------|----------------------------------|
| `rate(5 minutes)`        | Every 5 minutes continuously     |
| `rate(1 hour)`           | Every hour                       |
| `rate(1 day)`            | Every 24 h (same time each day)  |

**Best practice:** choose **rate()** for “every _N_ minutes/hours/days”, choose **cron()** when you need calendar‑aware schedules (weekdays, specific month/day, etc.).
- When create new rule, choose the same as when create cron expression.
![advanced-1](https://dangdung177.github.io/Internship/images/6.advanced/advanced-1.png)
- In **Define Schedule** section, choose the right option of **Schedule pattern**
- Choose Rate Expression as you see fit.
![advanced-2](https://dangdung177.github.io/Internship/images/6.advanced/advanced-2.png)
- Then continue create rule as you [create a cron rule](/3-eventbridge/#4-Select-Target).
---
### 3. Adding Event Pattern Filters

Sometimes you publish different kinds of events to the default bus and only want **some** of them to reach a target. Then you will need event pattern.
An event pattern defines the data EventBridge uses to determine whether to send the event to the target. If the event pattern matches the event, EventBridge sends the event to the target.

1. In the rule builder, choose **Event pattern** instead of **Schedule**.  
![advanced-2](https://dangdung177.github.io/Internship/images/6.advanced/advanced-3.png)
2. Event source: choose **AWS events or event source** (or **Custom pattern** if using your own bus).
3. Define JSON filter, e.g.:

```json
{
  "source": ["custom.billing"],
  "detail-type": ["MonthlyInvoiceReady"],
  "detail": {
    "amount": [ { "numeric": [">=", 1000] } ]
  }
}
```
This triggers only when:
- The payload’s source is custom.billing, 
- Detail-type matches
- Amount ≥ 1000.
4. Continue -> select the Lambda, SNS, etc. target -> **Create rule**

### 4. Using an Input Transformer
An Input Transformer is a feature that allows you to customize the data sent to a target when a rule is triggered. It enables you to reshape or filter the event data before it's passed to the target, making it easier to work with specific data points or format the data in a way that's compatible with the target service.
1. Edit or create a rule.
2. In the Targets section, after choosing your Lambda, expand Additional settings.
![advanced-2](https://dangdung177.github.io/Internship/images/6.advanced/advanced-4.png)
3. Choose **Configure target input** and then **Input transformer** then **Configure input transformer** 
You can transform the incoming event before it arrives at your Lambda:
```json
{
  "InputPathsMap": {
    "user": "$.detail.userId",
    "total": "$.detail.amount"
  },
}
{
   "InputTemplate": "{ 
      \"userId\": <user>, 
      \"invoiceTotal\": <total> 
   }"
}
```
![advanced-2](https://dangdung177.github.io/Internship/images/6.advanced/advanced-5.png)
Original Event:
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
Result delivered to Lambda:
```json
{
  "userId": "abc123",
  "invoiceTotal": 1520.50
}
```
This keeps your function lightweight—no need to parse the full EventBridge envelope.


### 5. Multiple Targets
In EventBridge, a single rule can have up to five targets. This allows a rule to trigger multiple actions or services when an event matches its pattern, enabling a fan-out pattern where a single event can initiate several different processes concurrently.
Example:
- Lambda -> Update a database record with the new instance state.
- SNS -> Publish a message to an SNS topic.
- Cloudwatch -> Send the event details to CloudWatch Logs.

When create or edit a new rule:   
- Go to the “Add target” section
- Add your Lambda function
![advanced-2](https://dangdung177.github.io/Internship/images/6.advanced/advanced-target1.png)
- Click “Add another target”
- Choose SNS topic
![advanced-2](https://dangdung177.github.io/Internship/images/6.advanced/advanced-target2.png)
- Configure both as needed
- Click Create rule or Update rule

### ✅ 6. **Result**  
You can now:
- Craft sophisticated CRON or rate schedules
- Filter events so only relevant data triggers your workflows
- Pre‑shape event payloads with input transformers
- Fan‑out a single rule to multiple downstream services

In the final chapter, you’ll clean up resources to avoid ongoing costs and keep your AWS account tidy.
