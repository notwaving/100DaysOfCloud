# Hoist: Terraform Tutorial Pt 2

## Introduction

Now that I've gone through the introductary tutorials in the official Terraform documentation, it's time to jump back in to class with Mastermndio!

## Prerequisite

- GitHub (or similar) account
- Comfortable on the command line
- AWS knowledge
- AWS account and IAM role with appropriate permissions

## Use Case

- Create and manage configuration files that describe to Terraform the resourses needed to run your apps

## Cloud Research

We're going to create a new repo in GitHub, then create a Terraform configuration file to define our resources in.

## Try yourself

### Step 1 - Create repo on GitHub and pull to remote

- Create a new repository
- `git clone` to your chosen repository on your machine

### Step 2 - Create a `*.tf` file to add config for AWS provider

- For this tutorial I created `infrastructure.tf` in the project repo
- Set up the provider as AWS, as in the [docs](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- `terraform init` to install dependencies
- `terraform fmt` and `terraform validate` to check formatting and if configuration is valid code
- Commit these changes to the GitHub repo

### Step 3 — Provision an S3 bucket

Rather than doing this in the AWS console, we're going to do it in terraform!

- Search docs, [this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket) is what I found.

- Starting with the "Private bucket with tags" code example:

  - modify (acl to "public-read")
  - the second parameter in "resource" is the name Terraform refers to the bucket as - in this case, "website_bucket"
  - add tags if you want, but that's optional
  - Check formatting and validation as before
  - Run with `terraform apply`, and check AWS S3 console to see if it works!
  - If you run `terraform apply` again without any changes, note how Terraform states `Refreshing state...` and displays how it didn't add, hcange or destroy resources.

![apply twice](/Journey/060/apply-twice.png)

### Step 4 — Ensure the bucket is configured to serve websites

- In the official docs the next code block is related to static hosting. Integrate this into what you've already got in the `infrastructure.tf` file.
- Formate and validate (no file called `policy.json` exists)
- Find an example of an S3 static website hosting policy, paste and modify in a new file called `policy.json`
- Run `terraform apply` and check proposed changes
- Drag and drop website files to S3 bucket in AWS console

### Step 5 - Configure 2 EC2 instances

Our requirements:

- Name: hoist-internal-development
- Size: t2.micro
- OS: ubuntu 18.04
- We need to log in so make sure it uses the IAM key pair

- Check `aws_usage` in Terraform docs, copy and paste what you need into your already existing code
- As for the parameters, rename the 'name', select the free `t2.micro` for 'instance type', and for "ami", go through EC2 in the console as if you were setting up an EC2 instance, in order to see what the AMI codes are...

![ami](/Journey/060/ami.png)

- In the EC2 console, create a key pair, download the `.pem` file to the root of this project's repo and refer to it like this: `"key_name = "<key name>"`
- Check everything as before, then `apply`
- But we need to create _two_ instances! Add `count = 2` to the EC2 resource code block

```bash
# aws_instance.developmentServer[0] will be created
  + resource "aws_instance" "developmentServer" {
      + ami                          = "ami-0ac73f33a1888c64a"
      + arn                          = (known after apply)
      + associate_public_ip_address  = (known after apply)

...

# aws_instance.developmentServer[1] will be created
  + resource "aws_instance" "developmentServer" {
      + ami                          = "ami-0ac73f33a1888c64a"
      + arn                          = (known after apply)
      + associate_public_ip_address  = (known after apply)

etc
```

![ec2](/Journey/060/ec2.png)

### Step 6 - Destroy resourses

- Run `terraform destroy`

## ☁️ Cloud Outcome

With a bit of docs and copying and pasting, you can easily provision, update, and destroy infrastructure!

## Next Steps

Do part 3 of the course!

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1349109423457906689?s=20)
