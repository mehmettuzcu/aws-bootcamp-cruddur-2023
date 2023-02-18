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

## Create a new User and Generate AWS Credentials i
* Go to (IAM Users Console] create aim user
* Enable console access for the user ;
* Create anew Adwin Group and apply AdwinistratorAccess }
* Create the user and go find and click into the user i
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

### Check that the AWS CLI is working and you are the expected user i

```bash
aws --cli-auto-prompt
aws sts get-caller-identity
```

You should see something like this:
```bash

```

