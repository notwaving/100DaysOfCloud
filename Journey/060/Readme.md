# Hoist: Terraform Tutorial Pt 2

## Introduction

Now that I've gone through the introductary tutorials in the official Terraform documentation, it's time to jump back in to class with Mastermindio!

## Prerequisite

- GitHub (or similar) account
- Comfortable on the command line
- AWS account and IAM role with appropriate permissions

## Use Case

- 🖼️ (Show-Me) Create an graphic or diagram that illustrate the use-case of how this knowledge could be applied to real-world project
- ✍️ (Show-Me) Explain in one or two sentences the use case

## Cloud Research

- ✍️ Document your trial and errors. Share what you tried to learn and understand about the cloud topic or while completing micro-project.
- 🖼️ Show as many screenshot as possible so others can experience in your cloud research.

## Try yourself

✍️ Add a mini tutorial to encourage the reader to get started learning something new about the cloud.

### Step 1 - Create repo on Github and pull to remote

- Create a new repository
- `git clone` to your chosen repository on your machine

### Step 2 - Create a `*.tf` file to add config for AWS provider

- For this tutorial I created `infrastructure.tf` in the project repo
- Set up the provider as AWS, as in the [docs](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- `terraform init` to install dependencies
- `terraform fmt` and `terraform validate` to check formatting and if configuration is valid code
- Commit these changes to the GitHub repo

### Step 3 — Provision an S3 bucket to serve a blog site

Rather than doing this in the AWS console, we're going to do it in terraform!

- Search docs, [this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket) is what I found.

- Starting with the "Private bucket with tags" code example:
  - modify (acl to "public-read")
  - the second parameter in "resource" is the name Terraform refers to the bucket as - in this case, "website_bucket"
  - Check formatting and validation as before
  - Run with `terraform apply`, and check AWS S3 console to see if it works!

## ☁️ Cloud Outcome

✍️ (Result) Describe your personal outcome, and lessons learned.

## Next Steps

✍️ Describe what you think you think you want to do next.

## Social Proof

✍️ Show that you shared your process on Twitter or LinkedIn

[Twitter](link)
