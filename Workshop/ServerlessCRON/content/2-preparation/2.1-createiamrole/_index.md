---
title : "Create IAM Role for Lambda"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

### Overview

In this section, you will create an **IAM Role for AWS Lambda** with the required permissions to execute code and write logs to **Amazon CloudWatch**. This role follows AWS security best practices by granting only the minimal permissions needed.

{{% notice note %}}
**Security Note:** IAM Roles allow Lambda functions to securely interact with other AWS services using **temporary credentials** without embedding access keys.
{{% /notice %}}

---

### Implementation Steps

#### 1. Open the IAM Console
- Log in to the **AWS Management Console**
- Search for and open the **IAM** service
![IAM](https://dangdung177.github.io/Internship/images/2.prerequisite/IAM-1.png)

#### 2. Create a new role:
- In the left navigation pane, click **Roles**
- Click the **Create role** button
![IAM-CreateRole](https://dangdung177.github.io/Internship/images/2.prerequisite/IAM-2.png)
![IAM](https://dangdung177.github.io/Internship/images/2.prerequisite/IAM-3.png)
#### 3. Select the trusted entity type:
- Under **Trusted entity type**, choose **AWS service**
- Select **Lambda** as the use case
- Click **Next**
![IAM](https://dangdung177.github.io/Internship/images/2.prerequisite/IAM-4.png)
#### 4. Assign permissions:
- In the permissions search bar, type: `AWSLambdaBasicExecutionRole`
- Select the managed policy named **AWSLambdaBasicExecutionRole**
  - This allows the Lambda function to write logs to **Amazon CloudWatch Logs**
- (Optional) If your Lambda will interact with services like S3, DynamoDB, or SNS, attach additional permissions here
- Click **Next**
![IAM](https://dangdung177.github.io/Internship/images/2.prerequisite/IAM-5.png)

{{% notice info %}}
You can attach more policies later if your Lambda needs to access other AWS services.
{{% /notice %}}

#### 5. Configure role details:
- Enter a role name such as: `Cron-Lambda-Executor`
- (Optional) Add a description like: _"Allows Lambda functions to call AWS services on your behalf."_
- Review the trust policy and attached permissions
- Click **Create role**
![IAM](https://dangdung177.github.io/Internship/images/2.prerequisite/IAM-6.png)
![IAM](https://dangdung177.github.io/Internship/images/2.prerequisite/IAM-7.png)

🔐 *Important:* Role names must be unique and are not case-sensitive. For example, `cron-lambda-executor` and `Cron-Lambda-Executor` are treated the same.

---

### ✅ Confirm role creation:
- You should see a success message
![IAM](https://dangdung177.github.io/Internship/images/2.prerequisite/IAM-8.png)
- Go to the newly created role’s summary page
- Note the **Role ARN** — you will use this when creating or updating your Lambda function
- Verify that **AWSLambdaBasicExecutionRole** is attached
![IAM](https://dangdung177.github.io/Internship/images/2.prerequisite/IAM-9.png)

---

Your IAM Role is now ready to be used by a Lambda function to run scheduled CRON jobs with EventBridge.
