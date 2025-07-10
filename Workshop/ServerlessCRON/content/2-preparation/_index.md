---
title : "Preparation"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

{{% notice info %}}
To complete this lab, you will need permission to create and manage Lambda functions, EventBridge rules, and CloudWatch Logs.
{{% /notice %}}

This lab will walk you through building a serverless CRON job using **Amazon EventBridge** and **AWS Lambda**, with **Amazon CloudWatch** used for monitoring. Before diving into implementation, we’ll make sure you have all necessary IAM roles and access in place.

You’ll also prepare a simple Lambda function to be used for scheduled execution.

### What You'll Need:
- An AWS account with full access to:
  - Lambda
  - EventBridge
  - CloudWatch Logs
  - IAM (to create roles)
- AWS Management Console access or AWS CLI (optional)

### In this section, we will:
- Create an IAM Role with permissions for Lambda and logging
- Prepare a sample Lambda function (Node.js or Python)
- Optionally set up a test payload or logging structure

### Content
  - [Create IAM Role for Lambda](2.1-createiamrole/)
  - [Prepare Lambda Function Code](2.2-preparelambda/)
