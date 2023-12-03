# Week 0 — Billing and Architecture

## Required Homework Tasks

### Install and Verify AWS CLI

I had initially installed AWS CLI locally before the bootcamp.
I installed it following the instructions from [AWS CLI Install Documentation](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

I run the `aws --version` command to verify it being installed successfully.
![Proof of Working AWS CLI](assets/aws-cli-confirmation.png)

#### Create a Budget

I created a budget for $1 because i obviosly cannot afford any kind of spend.

I used the aws cli in doing this: [AWS CLI documentation for budget creation](https://docs.aws.amazon.com/cli/latest/reference/budgets/create-budget.html#examples)

I followed the followig steps;

1. I created a [budget.json](aws/json/budget.json) file that describes the budget
1. I added a [notifications-with-subscribers.json](aws/json/notifications-with-subscribers.json) that describes the notification subcribers

In the terminal I run the command below to create the buget while referencing the files above.

Note: I got my account ID using the aws cli command

```
aws sts get-caller-identity --query Account --output text
```

Better to store it in env, so I did:

```
export ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
```

Then i can just reference it as $ACCOUNT_ID

```
aws budgets create-budget \
    --account-id $ACCOUNT_ID \
    --budget file://aws/json/budget.json \
    --notifications-with-subscribers file://aws/json/notifications-with-subscribers.json
```
