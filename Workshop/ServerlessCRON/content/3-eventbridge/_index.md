---
title : "Setting up EventBridge Rule"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

### Overview

In this chapter, you will create a **scheduled rule in Amazon EventBridge** using a CRON expression to trigger your **Lambda function** at a specific time or interval.

Amazon EventBridge supports **serverless scheduling** that eliminates the need for traditional cron servers. This rule will serve as the trigger mechanism for your CRON jobs.

---

### Objectives

- Define a CRON schedule using **EventBridge Rule**
- Trigger the Lambda function created earlier
- Verify correct association and behavior

---

### 1. Navigate to Amazon EventBridge Console

- Open the [Amazon EventBridge Console](https://console.aws.amazon.com/events/)
- From the left menu, choose **Rules**
- Click on **Create rule**

![eventbridge1](https://dangdung177.github.io/Internship/images/3.eventbridge/eventbridge-1.png)
![eventbridge2](https://dangdung177.github.io/Internship/images/3.eventbridge/eventbridge-2.png)

---

### 2. Configure Rule Basics

- Name: `ScheduledLambdaTrigger`
- Description: _Trigger Lambda function based on schedule_
- For **Rule type**, select: `Schedule`
- Choose **Enable the rule on the selected event bus**
- Click **Continue to create rule**

![eventbridge3](https://dangdung177.github.io/Internship/images/3.eventbridge/eventbridge-3.png)

---

### 3. Define Schedule Pattern

- Choose **Recurring schedule**
- In the **Schedule pattern**, select **CRON-based schedule expression**
- Example:  cron(0 10 * * ? *)
-> This runs every day at 10:00 AM GMT+7 (Because the time in Vietnam is 7 hours faster than GMT so we reduce Hours by 7)
- Click **Next**

![eventbridge3](https://dangdung177.github.io/Internship/images/3.eventbridge/eventbridge-4.png)

---

### 4. Select Target

- Target type: **AWS service**
- Select **Lambda function**
- Function name: `ScheduledLoggerFunction` (or your function name)
- For **Execution Role**, choose **Create a new role for this specific resource** or **Use existing role** if prompted

> Make sure the Lambda's IAM Role has the appropriate permissions.

- Click **Next**

![eventbridge4](https://dangdung177.github.io/Internship/images/3.eventbridge/eventbridge-5.png)
---

### 5. Review and Create Rule

Review all settings:
- Rule Detail
- Build schedule
- Build Target(s)
- Configure tag(s)
- Click **Create rule**
![eventbridge4](https://dangdung177.github.io/Internship/images/3.eventbridge/eventbridge-6.png)

---

### Result

Your EventBridge rule is now active and will **automatically invoke your Lambda function** on the schedule you defined.

You can:
- Monitor invocation via **CloudWatch Logs**
- Change the CRON expression at any time
- Add additional targets if needed

---

In the next chapter, we will test your CRON job by manually invoking it and observing logs and execution results.
