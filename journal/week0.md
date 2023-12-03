# Week 0 — Billing and Architecture

## Required Homework Tasks

### Install and Verify AWS CLI

I had initially installed AWS CLI locally before the bootcamp.
I installed it following the instructions from [AWS CLI Install Documentation](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

I run the `aws --version` command to verify it being installed successfully.
![Proof of Working AWS CLI](assets/aws-cli-confirmation.png)

### Create a Budget

I created a budget for $1 because i obviosly cannot afford any kind of spend.

I used the aws cli in doing this: [AWS CLI documentation for budget creation](https://docs.aws.amazon.com/cli/latest/reference/budgets/create-budget.html#examples)

I followed the followig steps;

1. I created a [budget.json](aws/json/budget.json) file that describes the budget
1. I added a [notifications-with-subscribers.json](aws/json/notifications-with-subscribers.json) that describes the notification subcribers

In the terminal I run the command below to create the buget while referencing the files above.

Note: I got my account ID using the aws cli command

```sh
aws sts get-caller-identity --query Account --output text
```

### Set Env Vars

Better to store it in env, so I did:

```sh
export ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
```

<!--
We will set these credentials for the current bash terminal
```
export AWS_ACCESS_KEY_ID=""
export AWS_SECRET_ACCESS_KEY=""
export AWS_DEFAULT_REGION=us-east-1
``` -->

Then i can just reference it as $ACCOUNT_ID

```sh
aws budgets create-budget \
    --account-id $ACCOUNT_ID \
    --budget file://aws/json/budget.json \
    --notifications-with-subscribers file://aws/json/notifications-with-subscribers.json
```

I run the command and went to the management console to check the bugdget
![Proof of Working Budget](assets/budget-confirmation-screen.png)

Now next step is to configure sns for alarms to work

### Create SNS Topic

- We need an SNS topic before we create an alarm.
- The SNS topic is what will delivery us an alert when we get overbilled
- [aws sns create-topic](https://docs.aws.amazon.com/cli/latest/reference/sns/create-topic.html)

We'll create a SNS Topic

```sh
aws sns create-topic --name billing-alarm
```

which will return a TopicARN

We'll create a subscription supply the TopicARN and our Email

```sh
aws sns subscribe \
    --topic-arn TopicARN \
    --protocol email \
    --notification-endpoint your@email.com
```

Check your email and confirm the subscription

#### Create Alarm

- [aws cloudwatch put-metric-alarm](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/put-metric-alarm.html)
- [Create an Alarm via AWS CLI](https://aws.amazon.com/premiumsupport/knowledge-center/cloudwatch-estimatedcharges-alarm/)
- We need to update the configuration json script with the TopicARN we generated earlier
- We are just a json file because --metrics is is required for expressions and so its easier to us a JSON file.

```sh
aws cloudwatch put-metric-alarm --cli-input-json file://aws/json/alarm-config.json
```
