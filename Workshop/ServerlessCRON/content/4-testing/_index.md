---
title : "Testing Scheduled Events"
date  : "`r Sys.Date()`"
weight: 4
chapter: false
pre   : " <b> 4. </b> "
---

### Overview

In this chapter you will **verify that your EventBridge rule is correctly invoking your Lambda function**.  
You will watch the first invocation in near‑real time, inspect the log output in **CloudWatch Logs**, and confirm that the schedule continues to run automatically.

---

### Objectives

- Confirm that the EventBridge rule is **enabled** and showing the correct upcoming trigger times  
- Observe at least **one live invocation** of the Lambda function  
- Inspect the **log stream** in CloudWatch to validate input and output  
- View basic **metrics** (Invocations & Duration) for additional confidence  

---

### 1. Confirm the rule’s next‑run time

1. Open **Amazon EventBridge -> Rules**  
2. Click on **`ScheduledLambdaTrigger`**  
3. In the **Schedule** section verify the **Next 10 trigger dates** list  
   (Make sure the times match your expected local time / UTC offset.)
4. Click **Edit**
![rule‑detail](/images/4.testing/testing-1.png)

> **Tip** – If you don’t want to wait until the next hour/day, temporarily edit the CRON expression to a time a few minutes ahead, _Save_, then revert after testing.
![rule‑detail](/images/4.testing/testing-2.png)  
Explanation:
`cron(0/3 * * * ? *)` = Run every 3 minute every day

---

### 2. Wait for the first scheduled invocation

- Go to **Lambda -> Monitor -> Recent invocations** tab  
- Refresh after the expected trigger time; you should see **1 new invocation**

![lambda‑invocation](/images/4.testing/testing-3.png)

---

### 3. Inspect logs in CloudWatch

1. In the Lambda console, choose **Monitor -> View logs in CloudWatch**  
2. Click the **latest Log stream** (timestamp matches your trigger time)  
3. Verify you see the two log lines you coded earlier:
![lambda‑invocation](/images/4.testing/testing-4.png)

---

### 4. Validate metrics

- Still in the **Monitor** tab, go to **CloudWatch metrics**  
- Confirm **Invocations = 1** and **Errors = 0**  
- If you allowed the rule to continue running, you’ll see the graph update at each interval

![metrics](/images/4.testing/testing-5.png)

---

### 5. (Optional) Manual re‑test

If you need an immediate re‑run without changing the schedule:

You can test your Lambda by [go to this section](/2-preparation/2.2-preparelambda/#5-test-the-function).


---

### ✅ Result

You have now proven that:

- **EventBridge** is firing on schedule  
- **Lambda** executes successfully with the correct role  
- **CloudWatch Logs & Metrics** capture all execution details  

In the next chapter we will add **monitoring best‑practices**—alerts, log retention, and custom metrics—to keep your serverless CRON jobs healthy in production.