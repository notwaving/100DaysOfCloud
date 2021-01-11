# 🌍 HashiCorp Terraform AWS Tutorials Pt 2

## Introduction

We're picking up from where we left off yesterday with HashiCorp's Terraform tutorials for AWS.

## Prerequisite

Yesterday's work (day 58)

## Use Case

- Use environment/input variables instead of hard-coding access keys, AMIs, etc
- Extract specific variables to use in scripts, etc
- Terraform Cloud allows teams to collaborate on state

## Cloud Research

Today we're learning about

- Input variables
- Querying data with output variables
- Storing remote state, aka Terraform Cloud

## Try yourself

### Input Variables

- As an example we'll extract our region as a variable. Create a file, `variables.tf` (N.B. file name can be anything, it just has to end with `.tf`)

- In that file:

```go
variable region {
  default = "us-west-2"
}
```

- Back in `example.tf`, in `provider`, change the "region" value to "var.region"

- Run `terraform apply` to see that it runs correctly

- N.B. You can also do this on the command line with this syntax:

```bash
terraform apply -var="region=us-west-2"
```

...but remember doing so this way will not save the var permanently.

- To persist variable values, create a file and assign variables within this file. Create a file named `terraform.tfvars` with the following contents:

```go
region = "us-west-2"
```

Terraform automatically loads all files in the current directory with the exact name of `terraform.tfvars` or any variation of `*.auto.tfvars`. If the file is named something else, you can use the -var-file flag to specify a file name. These files use the same syntax as Terraform configuration files (HCL). And like Terraform configuration files, these files can also be JSON.

We don't recommend saving usernames and passwords to version control. You can create a local file with a name like `secret.tfvars` and use `-var-file` flag to load it.

You can use multiple `-var-file` arguments in a single command, with some checked in to version control and others not checked in.

```bash
$ terraform apply \
  -var-file="secret.tfvars" \
  -var-file="production.tfvars"
```

- If no value is assigned to a variable via any of these methods and the variable has a default key in its declaration, that value will be used for the variable.

- Strings and numbers are the most commonly used variables, but lists (arrays) and maps (hashtables or dictionaries) can also be used. The syntax for each is in the [documentation](https://learn.hashicorp.com/tutorials/terraform/aws-variables?in=terraform/aws-get-started).

### Query Data with Output Variables

When building potentially complex infrastructure, Terraform stores hundreds or thousands of attribute values for all your resources. But as a user of Terraform, you may only be interested in a few values of importance, such as a load balancer IP, VPN address, etc.

Outputs are a way to tell Terraform what data is important. This data is outputted when apply is called, and can be queried using the terraform output command.

- Let's define an output to show us the IP address of the elastic IP address created. In any `*.tf` file:

```go
output "ip" {
  value = aws_eip_ip.public_ip
}
```

This defines an output variable named "ip". The value field specifies what the value will be, and almost always contains one or more interpolations, since the output data is typically dynamic. In this case, we're outputting the public_ip attribute of the elastic IP address.

Multiple output blocks can be defined to specify multiple output variables.

- Run `terraform apply` to populate the output

![public ip](/Journey/059/public_ip.png)

- You can also query the outputs after apply-time with `terraform output ip`. This command is useful for scripts to extract outputs

### Store Remote State

Terraform Cloud allows teams to access Terraform configuration files in order to collaborate on infrastructure. It allows you to keep state secure and encrypted. Terraform Cloud also securely stores variables, including API tokens and access keys. It provides a safe, stable environment for long-running Terraform processes.

The tutorial then sets up an account in Terraform Cloud. I will wait until this comes up on the course, so I get the setup correct the first time. [This is the tutorial, for future reference](https://learn.hashicorp.com/tutorials/terraform/aws-remote?in=terraform/aws-get-started)

Although Terraform Cloud can act as a standard remote backend to support Terraform runs on local machines, it works even better as a remote run environment. It supports two main workflows for performing Terraform runs:

1. A VCS-driven workflow, in which it automatically queues plans whenever changes are committed to your configuration's VCS repo.
2. An API-driven workflow, in which a CI pipeline or other automated tool can upload configurations directly.

The second could be really useful for the group project, as we're setting up a pipeline with CircleCI.

## ☁️ Cloud Outcome

Most of this was really clear, but sensitive information (like access keys and API tokens) can also be stored in Terraform Cloud.

Terraform Cloud sounds like an IaC version of GitHub.

## Next Steps

Pick up where I left off with Mastermnd's Terraform seminars, as planned.

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1348602456591245318?s=20)
