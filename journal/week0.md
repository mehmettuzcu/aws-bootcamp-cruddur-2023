# Week 0 — Billing and Architecture

## Getting the AWS CLI Working

We'll be using the AWS CLI often in this bootcamp, so we'll proceed to installing this account.

## Install AWS CLI

* We are going to installthe AWS CLI when our Gitpod enviroment lanuches.
* We are are going to set AWS CLI to use partial autoprompt mode to make it easier to debug CLI commands. 
The bash commands we are using are the same as the [AWS CLI Install i
Instructions (https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

Update our _gitpod.yal to include the following task.

```bash
tasks:
  - name: aws-cli
    env:
      AWS_CLI_AUTO_PROMPT: on-partial
    init: |
      cd /workspace
      curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
      unzip awscliv2.zip
      sudo ./aws/install
      cd $THEIA_WORKSPACE_ROOT
```
We''l also run these commands indivually to perform the install manually

## Create a new User and Generate AWS Credentials
* Go to (IAM Users Console] create aim user
* Enable console access for the user
* Create anew Adwin Group and apply AdwinistratorAccess }
* Create the user and go find and click into the user
* Clickon Security Credentials and Create Access Key }
* Choose AWS CLI Access }
* Download the CSV with the credentials

### Set Env Vars
We willset these credentials for the current bash terminal

```bash
export AWS_ACCESS_KEY_ID=""
export AWS_SECRET_ACCESS_KEY=""
export AWS_DEFAULT_REGION=""
```

We'll tell Gitpod to remember these credentials if we relaunch our workspaces

```bash
gp env AWS_ACCESS_KEY_ID=""
gp env AWS_SECRET_ACCESS_KEY=""
gp env AWS_DEFAULT_REGION=""
```

### Check that the AWS CLI is working and you are the expected user

```bash
aws --cli-auto-prompt
aws sts get-caller-identity
```

You should see something like this:
```bash

```

# Enable Billing
We need to turn on Billing Alerts to recieve alerts...
* Inyour Root Account go to the Billing Page
* Under Billing Preferences Choose Receive Billing Alerts
* Save Preferences

## Creating a Billing Alarm
### Create SNS Topic
* We need an SN topic before we create an alarm.
* The SN topic is what will delivery us an alert when we get overbilled
* aws sns create-topic
We'll create a SNS Topic &
```bash
aws sns create-topic --name billing-alarm
```
which will return a TopicARN !
We'll create a subscription supply the TopicARN and our Email

```bash
aws sns subscribe \
    --topic-arn="arn:aws:sns:eu-west-1:424818714147:billing-alarm" \
    --protocol=email \
    --notification-endpoint=mehmettuzcu22@gmail.com

```

Check your email and confirm the subscription

## Create Alarm

* aws cloudwatch put-metric-alarm
* [Create an Alarm via AWS CLI](https://aws.amazon.com/premiumsupport/knowledge-center/cloudwatch-estimatedcharges-alarm/)
* We need to update the configuration json script with the TopicARN we generated earlier
* We are just a json file because --metrics isis required for expressions and so its easier to us a JSON file.
```bash
aws cloudwatch put-metric-alarm --cli-input-json file://aws/json/alarm_config.json

```

# Create an AWS Budget

aws budgets create-budget

Get your AWS Account ID
```bash
aws sts get-caller-identity —-query Account --output text

```

* Supply your AWS Account ID
* Update the json files
* This is another case with AWS CLI it just much easier to json files due to lots of nested json


```bash
export AWS_ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
```

```bash
aws budgets create-budget \
    --account-id $AWS_ACCOUNT_ID \
    --budget file://aws/json/budget.json \
    --notifications-with-subscribers file://aws/json/notifications-with-subscribers.json
```