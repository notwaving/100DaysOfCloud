# RDS and Terraform

## Introduction

✍️ (Why) Explain in one or two sentences why you choose to do this project or cloud topic for your day's study.

## Prerequisite

- Familiar with AWS
- Confidence with the command line

## Use Case

- 🖼️ (Show-Me) Create an graphic or diagram that illustrate the use-case of how this knowledge could be applied to real-world project
- ✍️ (Show-Me) Explain in one or two sentences the use case

## Cloud Research

- ✍️ Document your trial and errors. Share what you tried to learn and understand about the cloud topic or while completing micro-project.
- 🖼️ Show as many screenshot as possible so others can experience in your cloud research.

## Try yourself

✍️ Add a mini tutorial to encourage the reader to get started learning something new about the cloud.

### Step 1 — Create `main.tf` and `variables.tf`

- Code is already in course repo, and forked to your organisation so copy and configure that!

### Step 2 — Terraform commands

`terraform init` to sort out the repo
`format` and `validate`
`plan`
`apply`

### Step 3 — Set up instance

You're prompted to enter:

- name of your database instance! `my-example-instance`
  `name` - (Optional) The name of the database to create when the DB instance is created. If this parameter is not specified, no database is created in the DB instance. Note that this does not apply for Oracle or SQL Server engines. See the AWS documentation for more details on what applies for those engines.
- password `I wish I was in Dixie`
  `password` - (Required unless a snapshot_identifier or replicate_source_db is provided) Password for the master DB user. Note that this may show up in logs, and it will be stored in the state file.
- instance identifier `my-test-database`
  `identifier` as defined in Terraform:
  (Optional, Forces new resource) The name of the RDS instance, if omitted, Terraform will assign a random, unique identifier. Required if restore_to_point_in_time is specified.

NB - prompted in same way for db instance and instance identifier - double check class video for disambiguation

Is this only stored on your machine?

### Step 4 - Execute

- Double check the execution plan
- Make a note of your vars and password

## ☁️ Cloud Outcome

✍️ (Result) Describe your personal outcome, and lessons learned.

## Next Steps

✍️ Describe what you think you think you want to do next.

## Social Proof

✍️ Show that you shared your process on Twitter or LinkedIn

[link](link)
