![SAM](/Journey/051/sam-cli.jpg)

# Introduction to AWS SAM CLI Pt 1

## Introduction

Amazon's Serverless Application Model (SAM) is a framework for deploying serverless apps in AWS. The SAM CLI is a utility for building and testing serverless apps locally, and inside a Lambda-like Docker container. It also packages and deploys them. SAM is also a template language, and is part of AWS CloudFormation.

I imagine this is going to be quite the learning curve, so today I'm looking at [this blog](https://alexharv074.github.io/2019/03/02/introduction-to-sam-part-i-using-the-sam-cli.html), and, configuring SAM on my machine and [AWS' hello world tutorial](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-getting-started-hello-world.html).

## Prerequisite

- Familiarity with AWS services
- Working knowledge of the terminal
- Docker installed on machine

## Use Case

- As part of the Cloud Resume Challenge, I'm required to configure backend API resources (i.e. DynamoDB, Lambda function, and API Gateway) with SAM CLI, and not the AWS console.
- Single-deployment configuration. With this, everything can be organised and operated on a single stack - great for integrating with a CI/CD pipeline. I've already identified compatable GitHub Actions
- Local debugging and testing, allowing you to troubleshoot issues that you might otherwise identify only _after_ deploying to the cloud

## Cloud Research

- Official GitHub [documentation](https://github.com/aws/aws-sam-cli) in the README is a good introduction.
- Integrates well with IDEs, like VS Code, IntelliJ, Pycharm, etc
- Because SAM is an extension of CloudFormation, you get all the deployment capabilities of CloudFormation. You can definie resourse by using CloudFormation in your SAM template, and the full suite of resources that come with CloudFormation.
- Links to lots of documentation in the blog mentioned in the introduction, above.

## Try yourself

Getting started with SAM CLI: installation and Hello World app

### Step 1 — Install and configure SAM CLI

- Follow the [guide](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html) for your OS. As a Mac user I've already got an AWS account, IAM with admin permissions, Docker and Homebrew installed on my machine.
- All that's left to install is the SAM CLI, and that's done via Hombrew with `brew tap aws/tap` and `brew install aws-sam-cli`.

### Step 2 — Build a `Hello World` application

- Implements a basic API backend, consisting of an AWS API Gateway endpoint and an AWS Lambda function. When you send a GET request to the API Gateway endpoint, the Lambda function is invoked. The function returns a `hello world` message.

![architecture](/Journey/051/architecture.png)

- You have the option to choice a Lambda deployment package type, either zip or image. For the purposes of this tutorial, choose zip. If you choose image, you need to use AWS Elastic Container Registry(ECR) to deploy it.

- Using [official docs](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-getting-started-hello-world.html), follow the guide!

- TIP: If `sam build` fails, a quick and dirty solution is `sam build --use-container`

![success!](/Journey/051/success.png)

Think my handle is confusing enough?

### Step 3 — Deploy

- Run `sam deploy --guided` and follow the on-screen prompts

- This will produce a API Gateway endpoint (as well as an IAM role and a Lambda function). Use `curl` to send a request to your application using that endpoint's URL, e.g:

`curl https://<restapiid>.execute-api.us-east-1.amazonaws.com/Prod/hello/`

## ☁️ Cloud Outcome

![yay!](/Journey/051/more-success.png)

## Next Steps

Work my way through the blog - it's nicely laid out. Otherwise, I can try testing this app locally too.

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1341093773489614849?s=20)
