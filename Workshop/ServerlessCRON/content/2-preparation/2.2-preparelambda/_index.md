---
title : "Prepare Lambda Function Code"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---

### Create a Lambda Function

In this step, we will create a basic **AWS Lambda function** that will be triggered on a schedule by **Amazon EventBridge**. This function will serve as the core of your serverless CRON job.

We will use a simple example that logs a message to **Amazon CloudWatch Logs** when the function runs.

---

### 1. Go to the [AWS Lambda Console](https://console.aws.amazon.com/lambda/)
- Search for and open the **Lambda** service
![lambda-create](/images/2.prerequisite/Lambda-1.png)
- Click **Create function**
![lambda-create](/images/2.prerequisite/Lambda-2.png)
---

### 2. Configure the function

- Choose **Author from scratch**
- Function name: `ScheduledLoggerFunction`
- Runtime: `Python 3.13`
![lambda-config](/images/2.prerequisite/Lambda-3.png)
- Permissions:
  - Choose **Use an existing role**
  - Select the role you created earlier: `Cron-Lambda-Executor`
- Click **Create function**
![lambda-config](/images/2.prerequisite/Lambda-4.png)

---

### 3. Add sample code

Once your function is created, scroll down to the **Code** section and replace the default code with the following:

```python

import json  # Used to format event data into readable JSON

def lambda_handler(event, context):
    print("✅ Lambda function executed on schedule!")  # Confirmation log
    print("Event detail:", json.dumps(event, indent=2))  # Print event payload (from EventBridge)

    return {
        'statusCode': 200,
        'body': json.dumps('Lambda executed successfully')  # Return success message
    }

```
### 4. Deploy the function
- Click Deploy(Ctrl+Shift+U) to save and apply your code changes.
![lambda-config](/images/2.prerequisite/Lambda-5.png)
Your Lambda function is now ready. Now we will test the if the function run correctly.

### 5. Test the function
- Click the Test button(Ctrl+Shift+I) to test the code, if you don't already have a test function, it will open the **Create new test event** for you.
- Event name: `ScheduledTest`.
- Template: Keep `Hello World` or choose other as you see fit.
- Click **Save**.
![lambda-config](/images/2.prerequisite/Lambda-6.png)
- Click the **Test** button again to run the function.
You will see the output in the `Execution results`
![lambda-config](/images/2.prerequisite/Lambda-7.png)

### 6. Check Log Events in CloudWatch
- Change to tab **Monitor**.
- Click **View CloudWatch logs**.
![lambda-config](/images/2.prerequisite/Lambda-8.png)
- Find the nearest **Log streams** and click into it.
![lambda-config](/images/2.prerequisite/Lambda-9.png)
- You will see the detail of **Log events**
![lambda-config](/images/2.prerequisite/Lambda-10.png)

Your Lambda function is now ready. In the next step, we will create an EventBridge rule to trigger this function on a schedule.
