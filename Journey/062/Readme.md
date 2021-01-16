# Creating an AWS RDS Instance with Terraform

## Introduction

Today's subject reflects the work needed to be done on my current group project. The task is to configure some [pre-written Terraform config](https://github.com/techreturners/yrtt_terraform_rds) to provision an AWS RDS instance, a Security Group (for permissions), and a VPC!

## Prerequisite

- Familiarity with AWS
- Confidence with the command line
- Familiarity with Terraform
- An IAM profile to allow you to connect to your AWS account

## Use Case

- In our group project for Tech Returners' Your Return to Tech, we're moving on to the backend. We're at the stage where we want to connect a MySQL database to the cloud, and the curriculum suggests we do this by provisioning an RDS instance with AWS.

## Cloud Research

- I had to go away and do most of the Hoist Terraform course to truly understand what was going on, e.g. include defaults in the variable definitions to prevent being prompted to provide these in the terminal; outputs, etc.
- Modules. If the Terraform infrastructure gets larger I intend to introduce them

## Try yourself

Let's do this thing!

### Step 1 — Create `main.tf` and `variables.tf`

- Code is already in course repo, and forked to your organisation so copy and configure that. Remember to add default values to variables.tf, otherwise you'll be prompted for these parameters in the console, and (at this point, at least )I don't know where they're stored
- I also created a `terraform.tfvars` file to put the real values in. Any params stored here will override what's in `variables.tf`, so we need to decide on them as a team, store them here, and rememeber NOT TO COMMIT THIS TO SOURCE CONTROL!

### Step 2 — Terraform commands

`terraform init` to sort out the repo
`fmt` and `validate`
`plan`
`apply`

### Step 3 — Set up instance

Our vars, as defined in `variables.tf`

- "database_name" `name` - (Optional) The name of the database to create when the DB instance is created. If this parameter is not specified, no database is created in the DB instance. Note that this does not apply for Oracle or SQL Server engines. See the AWS documentation for more details on what applies for those engines.

- "database_username" `username` - (Required unless a snapshot_identifier or replicate_source_db is provided) Username for the master DB user.

- "database_password" `password` - (Required unless a snapshot_identifier or replicate_source_db is provided) Password for the master DB user. Note that this may show up in logs, and it will be stored in the state file.

- "rds_instance_identifier" `identifier` (Optional, Forces new resource) The name of the RDS instance, if omitted, Terraform will assign a random, unique identifier. Required if restore_to_point_in_time is specified.

Ready to be populated with the group!

![tfvars](/Journey/062/group-vars.png)

### Step 4 - Output

![available data](/Journey/062/rds-endpoint.png)

I want the endpoint of the RDS instance returned because that's what the database admin needs to connect her database to. At the very end of `main.tf` add:

![endpoint code](/Journey/062/endpoint-code.png)

### Step 5 - Execute

- Go through the commands
- Double check the execution plan
- Execute

![rds endpoint](/Journey/062/rds-endpoint.png)

Here's the endpoint, ready to be passed on to the database admin!

## ☁️ Cloud Outcome

- I learned that I needed to learn Terraform at a deeper level. This has allowed me to not understand what I'm doing, but to put it together much more quickly
- I've been introduced to the very good documentation - an absolute must when reading/writing Terraform infrastructure. All your options are covered here.

## Next Steps

- Depending on how complicated this Terraform infrastructure gets, I may want to
  - create modules for resources
  - if looking to share Terraform files with the team, use state locking and hosting on DynamoDB (and S3)

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1349842799106158597?s=20)
