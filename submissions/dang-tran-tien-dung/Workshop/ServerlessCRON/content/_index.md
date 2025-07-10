---
title : "Building Serverless CRON Jobs with EventBridge and Lambda"
date : "`r Sys.Date()`"
weight : 1
chapter : false
---

# Building Serverless CRON Jobs with EventBridge and Lambda

### Overall
In this lab, you’ll learn how to automate tasks on a schedule using AWS EventBridge and AWS Lambda. CRON jobs are useful for performing repetitive tasks like data cleanup, report generation, or system monitoring. 

We’ll use Amazon EventBridge to define CRON expressions and trigger Lambda functions without needing to manage servers. This allows for a fully serverless, scalable, and cost-effective solution for time-based automation.

![ServerlessCRON](/images/cron-architecture-en.png) 

### Content
 1. [Introduction](1-introduce/)
 2. [Preparation](2-preparation/)
 3. [Setting up EventBridge Rule](3-eventbridge/)
 4. [Testing Scheduled Events](4-testing/)
 5. [Monitoring with CloudWatch Logs](5-cloudwatch/)
 6. [Advanced Scheduling and Filtering](6-advanced/)
 7. [Clean up Resources](7-cleanup/)
