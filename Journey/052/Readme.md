# Introduction to AWS SAM CLI Pt 2

## Introduction

Didn't quite finish the AWS' hello world project, so doing that today.

## Use Case

- Testing an app locally using the AWS SAM CLI and Docker.
- Deleting the AWS resources you no longer need

## Cloud Research

- When you host the API locally, remember to run the curl command in a separate tab

- Invoking the Lambda function directly is faster is done in a single command

### Testing: Option 1 - Host API locally

- `sam local start-api` starts your app locally

![tab 1](/Journey/052/tab1.png)

- It can take a while for the Docker image to load. After it's loaded, you can use curl to send a request to your application that's running on your local host. Run this in a new tab:
  `curl http://127.0.0.1:3000/hello`

![tab 2](/Journey/052/tab2.png)

- The start-api command starts up a local endpoint that replicates your REST API endpoint. It downloads an execution container that you can run your function in locally. The end result is the same output that we saw when we called the function in the AWS Cloud.

### Testing: Option 2 - Invoke Lambda function directly

- `sam local invoke "HelloWorldFunction" -e events/event.json`

![lambda result](/Journey/052/lambda.png)

- The invoke command directly invokes your Lambda functions, and can pass input event payloads that you provide. With this command, you pass the event payload in the file event.json that the sample application provides.

This was almost instant and ran with only one step, so 🤷🏽‍♀️.

### 🧹 Cleaning up

The tutorial is over, so now we need to delete the AWS CloudFormation stack we created.

#### Option 1 - Delete from the AWS console

- Sign into the AWS Management Console
- Open the

#### Option 2 - Delete from the AWS command line

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1341894855509889024?s=20)
