# 🌍 HashiCorp Terraform AWS Tutorials Pt 1

## Introduction

Following on from yesterday's class, today we're working through [HashiCorp's Terraform (with AWS) tutorials](https://learn.hashicorp.com/collections/terraform/aws-get-started)! It's really well documented so I'll only note what stood out to me

## Prerequisite

- Docker
- AWS account
- AWS CLI
- AWS credentials configured locally

## Use Case

- Terraform is a very popular IaC tool and as it is cloud provider agnostic, it's likely to be used in a DevOps/Cloud job, which is why I'm spending longer on the fundamentals.

## Cloud Research

- Introduction to IaS with Terraform
- Quick start tutorial
- Build infrastructure with AWS - this creates an EC2 instance
- Change infrastructure with AWS
- Destroy infrastruction

## Try yourself

Notes on each mini tutorial below

### Introduction to IaS with Terraform

- Now that workforces are becoming increasingly distributed and remote it's easier to make the case for cloud computing. An alternative to physically configured infrastructure is IaC. You create a server in the cloud with a simple declarative config language, rather than clicking through an interactive configuration tool.

- IaS is the process of managing a file or files, rather than manually configuring resources in a user interface. At a high level, Terraform allows operators to use HCL to author files containing definitions of their desired resources on almost any provider (e.g. AWS, GCP, Docker, GitHub...) and automates the creation of those resources at the time of apply.

- In Terraform, HashiCorp created a declarative language (HCL) that consumes provider API for resource provisioning. By defining specific resources, Terraform manages those resources with provider plugins. Once you run `terraform apply`, real-world infrastructure is created.

- Advantages of IaC:
  - Easiliy repeatable
  - Easily readable
  - Operational certainty with `terraform plan`
  - Standardised environment builds
  - Quickly provisioned development envs
  - Disaster recovery

#### Workflows

Simple workflows for deployment closely follow these steps:

1. **Scope** - confirm which resources need to be created for a given project

2. **Author** - create the config file in HCL based on the scoped parameters

3. **Initialise** - run `terraform init` in the project repo with the config files. This will download the correct provider plug-ins for the project

4. Plan & Apply - run `terraform plan` to verify creation process, then `terraform apply` to create real resourses as well as state file that complares future changes in your config files to what actually exists in your deployment environment.

#### Advantages of Terraform

- Platform agnostic
- State management
  State is the source of truth by which config changes are measured. If a change is made or a resource is appended to a configuaration, Terraform compares those changes with the state file to determine what changes result in a new resource or resource modifications
- Operator confidence
  The workflow built into Terraform aims to instill confidence in users by promoting easily repeatable operations and a planning phase to allow users to ensure actions taken will not cause disruption in their environment. On `terraform apply`, the user is prompted to review the proposed changes and must affirm the changes, or else Terraform will not apply the proposed plan.

### Quick Start tutorial

Prequisites are Docker and Terraform

Now we're going to provision an NGINX server in record time using Docker and Terraform

- `open -a docker` opens Docker Desktop

- Create a directory for this tutorial with `mkdir terraform-docker-demo && cd $_`

- In your code editor, paste the provided Terraform config into a file named `main.tf`

- Initialise the project (downloads a plugin that allows Terraform to interact with Docker) with `terraform init`

- Provision the NGINX server container with `terraform apply`. When Terraform asks you to confirm type `yes` and press `ENTER`

- Verify the existence of the NGINX container by visiting http://localhost:8000/...

![succcess!](/Journey/058/success_NGINX.png)

...or running `docker ps` to list the container

![docker_ps](/Journey/058/docker_ps.png)

- To stop the container, run `terraform destroy`

You've now provisioned and destroyed an NGINX webserver with Terraform

### Build Infrastructure With AWS

This does the same as before, but with an EC2 instance on AWS.

#### Write configuration

- Create a directory for this config and change into it:
  `mkdir learn-terraform-aws-instance && cd $_`

- Create a file for the config code with `touch example.tf`

- Paste the code given in the tutorial into this new file. Terraform loads all files in the working directory that end in `.tf`

![config code](/Journey/058/example_tf_code.png)

We'll now explain in more detail what's in each code block:

##### Terraform block

The `terraform {}` block is required so that Terraform knows which provider to download from the Terraform Registry. In the config above, the AWS provider's source is defined as `hashicorp/aws` - shorthand for `registry.terraform.io/providers/hashicorp/aws`

![hashicorp/aws on terraform registry](/Journey/058/hashicorp:aws.png)

The `version` argument is optional, but recommended. If a version is not specified Terraform downloads the most recent provider during initialisation.

##### Providers

The `provider` block configures the named provider, in our case AWS, which is responsible for creating and managing resources. A provider is a plugin that Terraform uses to translate the API interactions with the service. It is responsible for understanding API interactions and exposing resources. Because Terraform can interact with any API, you can represent almost any infrastructure type as a resource.

The `profile` attribute in your provider block refers Terraform to the AWS credentials stored in your AWS Config file. Never hard-code credentials into `*.tf` config files. We're explicitly defining the default AWS config profile here to show how Terraform should access sensitve credentials. Multiple provider blocks can exist if a Terraform configuration managed resources from different providers. You can even use multiple providers together. For example you could pass the ID of an AWS instance to a monitoring resource from DataDog.

##### Resources

The `resource` block defines a piece of infrastructure. A resource might be a physical component such as an EC2 instance, or it can be a logical resource such as a Heroku application.

The resource block has two strings before the block: the resource type and the resource name. In the example, the resource type is `aws_instance` and the name is `example`. The prefix of the type maps to the provider. In our case "aws_instance" automatically tells Terraform that it is managed by the "aws" provider.

The arguments for the resource are within the resource block. The arguments could be things like machine sizes, disk image names, or VPC IDs. Our [providers reference](https://www.terraform.io/docs/providers/index.html) documents the required and optional arguments for each resource provider. For your EC2 instance, you specified an AMI for Ubuntu, and requested a `t2.micro` instance so you qualify under the free tier.

##### Initialise the directory

As we did in the last tutorial, initialise the configuration with `terraform init`

Terraform downloads the aws provider and installs it in a hidden subdirectory of the current working directory. The output shows which version of the plugin was installed.

##### Format and validate the configuration

The `terraform fmt` command automatically updates configurations in the current directory for easy readability and consistency.

If you are copying configuration snippets or just want to make sure your configuration is syntactically valid and internally consistent, the built in `terraform validate` command will check and report errors within modules, attribute names, and value types.

If your configuration is valid, Terraform will return a success message.

##### Create infrastructure

As in the last tutorial, run `terraform apply`

![ec2 created](/Journey/058/ec2.png)

You've now created infrastructure using Terraform! Visit the EC2 console to see the created EC2 instance. Make sure you're looking at the same region that was configured in the provider configuration...

![AWS EC2](/Journey/058/AWS-EC2.png)

##### Inspect state

When you applied your configuration, Terraform wrote data into a file called `terraform.tfstate`. This file now contains the IDs and properties of the resources Terraform created so that it can manage or destroy those resources going forward.

You must save your state file securely and distribute it only to trusted team members who need to manage your infrastructure. In production, we recommend storing your state remotely. Remote stage storage enables collaboration using Terraform but is beyond the scope of this tutorial.

Inspect the current state using `terraform show`.

When Terraform created this EC2 instance, it also gathered a lot of information about it. These values can be referenced to configure other resources or outputs, which we discuss more later on in these tutorials.

##### Manually managing state

Terraform has a built in command called `terraform state` for advanced state management. For example, if you have a long state file, you may want a list of the resources in state, which you can get by using the `list` subcommand, i.e. `terraform state list`. Similar to the Linux command?

##### A gotcha I found:

- AMIs are region-specific, so unless you're prepared to find one specific to your region of choice, don't change the region

### Change Infrastructure

We're going to modify the resource created in the last tutorial to see hown Terraform handles change

##### Configuration

Update the `ami` of the instance. Change the `aws_instance.example` resource under the provider block in `example.tf` by replacing the current AMI ID with a new one (ID provided in tutorial).

##### Apply changes

After changing the config, run `terraform apply` again to see how Terraform applies this change to the existing resources

![output](/Journey/058/ami-change.png)

The prefix `-/+` means that Terraform will destroy and recreate the resource, rather than updating it in-place. While some attributes can be updated in-place (which are shown with the `~` prefix), changing the AMI for an EC2 instance requires recreating it. Terraform handles these details for you, and the execution plan makes it clear what Terraform will do.

Additionally, the execution plan shows that the AMI change is what required your resource to be replaced. Using this information, you can adjust your changes to possibly avoid destroy/create updates if they are not acceptable in some situations.

Answer `yes` to execute the planned steps. As indicated by the execution plan, Terraform first destroyed the existing instance and then created a new one in its place.

![execution plan](/Journey/058/execution-plan.png)

You can use `terraform show` again to see the new values associated with this instance.

### Destroy Infrastructure

Destroying your infrastructure is a rare event in production environments. But if you're using Terraform to spin up multiple environments such as development, test, QA environments, then destroying is a useful action.

##### Destroy

The `terraform destroy` command terminates resources defined in your Terraform configuration. This command is the reverse of `terraform apply` in that it terminates all the resources specified by the configuration. It does not destroy resources running elsewhere that are not described in the current configuration.

As with apply, Terraform shows its execution plan and waits for approval before making any changes.

Just like with `apply`, Terraform determines the order in which things must be destroyed. In this case there was only one resource, so no ordering was necessary. In more complicated cases with multiple resources, Terraform will destroy them in a suitable order to respect dependencies.

## ☁️ Cloud Outcome

We learnt how to design, build, provision and destroy infrastructure created with Terraform, using AWS as our cloud provider

## Next Steps

Complete the tutorials on input / output variables, and store remote state.

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1348300692491010056?s=20)
