![placeholder image](/Journey/067/automate-all-the-things.png)

# Automating Terraform With GitHub Actions (I am a Monster)

## Introduction

I've seen an opportunity to automate something (I'll automate anything, me) with two of my favourite tools, Terraform and GitHub Actions! We're going to need to set up a Terraform Cloud account too...

## Prerequisite

- GitHub account
- AWS account and IAM credentials
- Some experience with GitHub Actions - you don't want to blindly import _all_ of the template provided, just bits of it

## Use Case

- Today I used Terraform to provision a single RDS database instance to host our group project's MySQL database on the cloud (AWS RDS). I was able to output the endpoint after this instance was created, and pass it on to the database admin. The group requested I push this up onto GitHub, but the only files I was happy to put up there were `main.tf` and `variables.tf`. While safe, it's not much use if a team member needed to pull the code down...
- tl;dr - Collaboration!

## Cloud Research

- We're going to work our way through [this guide](https://learn.hashicorp.com/tutorials/terraform/github-actions)
- First we need to setup a Terraform Cloud account, as the GitHub Action we're creating will connect to it to plan and apply our configuration. Our state and sensitive info will sit there too. We need to add our AWS credentials, and also generate an API token.
- The wall I hit was not realising I needed to put the database name and password variables in the `Terraform Variables` section of `Variables` and NOT in `Environment Variables` (which also contain AWS access keys)

![where to put variables](/Journey/067/variable-shenanigans.png)

- I'm not going to go through it step by step, as the HashiCorp's documentation is as solid as ever.

## ☁️ Cloud Outcome

The realisation that I'll automate absolutely _anything_

## Next Steps

See where the the group want to go with deploying the Lambda function. I don't mind either way whether they want to use Terraform or Serverless!

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1351622243525292035?s=20)
