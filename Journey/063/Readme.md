# New post title here

## Introduction

Day 3 done, time for day 4!

## Prerequisite

- The previous episodes of the course

## Use Case

- 🖼️ (Show-Me) Create an graphic or diagram that illustrate the use-case of how this knowledge could be applied to real-world project
- ✍️ (Show-Me) Explain in one or two sentences the use case

## Cloud Research

- Managing state
- Terraform backends
- State locking

### Terraform Backends

You don't want to put your Terraform state on GitHub when collaborating. In the same way that you don't just pull the last React build, instead running `npm install` when working on the app locally.

Benefits of backends:

- Working on a team. Backends store their state remotely and protect that stae with lock to prevent that corruption. Some backends such as Terraform Cloud even do version control
- Keeping sensitve info off disk. State is retrieved from backends on demand and only stored in memory. If you're using a backend such as Amazon S3, the only location the state is ever persisted in is S3
- Remote operations. For larger infrastructures or certain changes, `terraform apply` can take a long, long time. Some backends support remote operations which enable the operation to exist remotely. You can then turn off your computer and your operation will still complete. Paired with remote state storage and locking above, this also helps in team envs.

Backends are completely optional. If you're an individual, you can likely get away with never using backends. N.B. It's not good practice to only host data on a local machine. What if something happens and you need to get your code onto a different machine?

### States and Locks

State keeps track of your current condition of your infrastructure. Pretty important that we ensure that this is correct.

If supported, our backend can lock state so that we don't run into any conflicts (e.g. in the same way a database does)

- `terraform state` prints all subcommands.
- `terraform state list` lists resources in the state.

## Try yourself

✍️ Add a mini tutorial to encourage the reader to get started learning something new about the cloud.

### Step 1 — Create `.tf` and `.tfvars` file for modules

- Inside `devInstances` module, create the varibles in .tf, define them in `.tfvars`, then remember to import those variables in the main module file, e.g. "count = var.server_count"

- Do the same for the S3 bucket

### Step 2 — Hook up Terraform backend with S3, using a Dynamo DB table to lock the state

![Terraform, DynamoDB, S3](/Journey/063/s3.png)

Grab the example config, and place it in the `main.tf` file

```hcl
# Set up Terraform backend
terraform {
  backend "s3" {
    bucket = "terraform-dynamodb-bucket"
    key    = "terraform/state.tfstate"
    region = "eu-west-2"
  }
}
```

- `terraform init` to update everything

![success!](/Journey/063/backend_bucket.png)

State is no longer managed locally!

`terraform apply` to upload your first state file!

Go to S3 to check the bucket and the state therein.

### Step 3 — State locking with DynamoDB

- We need to create a DynamoDB table, so go to it via the AWS console

![dynamo1](/Journey/063/dynamo1.png)

Then add to backend config in the `main` file
![dynamo2](/Journey/063/dynamo2.png)

Check it out!

![table](/Journey/063/ooo.png)

## ☁️ Cloud Outcome

- State is very importand and you need it for Terraform to manage your infrastructure in the way you want to manage it
- Terraform is usually used in distributed teams. State allows us to track and understand the changes to our infra, as well as get our infra from the current state into the state described in the code
- Managing state in a location accessible by everybody (i.e. not locally) like S3 - for the backend
- You can also lock that state. This ensures the state maintains its integrity - only one person can make changes at a time

## Next Steps

Part 5

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1350196667216252937?s=20)
