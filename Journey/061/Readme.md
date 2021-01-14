# Hoist: Terraform Tutorial Pt 3

## Introduction

Pushing on with Mastermndio's free Terrform course!

## Prerequisite

- It would help if you did the previous classes

## Use Case

- Input variables can be applied across multiple environments/workspaces

## Cloud Research

- Input Variables
- Modules
- Output Variables
- Practice!

### Input Variables

HCL allows you to use variables for making dynamic configurations

These variables can be used to specify values for parameters and args that may change, depending on how you're using the config. They can be placed in files with these formats:

- variables.tf
- terraform.tfvars
- terraform.tfvars.json
  Then you reference the variables contained within, in your main config file

Environment variables are prefixed with `TF_VAR_`

We declare variables like a resource in our configuration

## Try yourself

We're going to have a play with variables and how they integrate with the Terraform.

### Step 1 — Declare an Input Variable

The relevant [documentation](https://www.terraform.io/docs/configuration/variables.html)

HCL is stongly typed, so you need to remember to declare this too, e.g.:

```hcl
variable "image_id" {
  type = string
}
```

You can also set a default:

```hcl
variable "availability_zone_names" {
  type    = list(string)
  default = ["us-west-1a"]
}
```

We're going to set a variable in the main `infrastructure.tf` file, and call it with `var.<variableName>`

- In this case, it's `ami = var.machine_image`

![input var](/Journey/061/basic-input-var.png)

If nothing is passed to the machine_image variable, the default gets passed instead.

You can also add custom validation (syntax in documentation linked above), e.g. checking an AMI image by counting the chars in the passed variable, checking it begins with "ami"...

### Step 2 - Move the variable into its own file

- Create `variables.tf` in the root and link from there

![variable.tf](/Journey/061/var1.png)
![instance.tf](/Journey/061/var2.png)

### Step 3 — Create a `.tfvars` file

- Assign variables here:

```hcl
machine_image = "ami-xyz"
instance_size = "t3.small"
server_num = 5
```

Any variables in here will override anything else. You can only _assign_ variables, not declare them.

- Run `terraform plan` to see which vars make it to the execution plan (_spoiler: it's the variables in the `.tfvars` file_)!

`variables.tf` is where the variables are created, and `terraform.tfvars` is where you assign them.

> A .tfvars file is used to assign values to variables that have already been declared in .tf files, not to declare new variable. To declare variable "test", place this block in one of your .tf files, such as variables.tf

> To nest a value for this variable in terraform.tfvars, use the definition syntax instead:
> `test = <value>`

### Modules - Reusable IaC

A module is a container for multiple resources that are used together. Every Terraform config - when you run `plan` or `apply` - form the root module, which consists of the resources defined in the .tf files in the main working directory.

You can grab modules from the [Terraform Registry](https://registry.terraform.io/).

> Modules are self-contined packages of Terraform configurations that are managed as a group.

#### Creating modules (one way to do it)

1. Put a .tf file in a folder, in the root directory.
2. Call the module from the root module

This is like using a function/method/class in software development. Whenever you make changes, you can do it in the module (not the root)

### Turn resources into modules

- Make a folder and file called `blog/blog.tf`
- Copy and paste your resources (S3 bucket, in our case) from `infrastructure.tf` into the blog file
- import the module:

```hcl
module "blog" {
  source = "./blog"
}
```

- Run `terraform apply` to see the module's resources now included in the execution plan.

- Create a module for the EC2 instances, then import the module

At this point the input variables stopped working on the EC2 instance, and I reverted back to hard-coding the values (as Aaron did in his tutorial)

UPDATE: Scoping applies to modules, and vars would need to be moved to their respective module folders.

### Output Variables

Used to pass info between modules, and for debugging.

e.g. (known after apply) messages - once known, we can get the info held and pass them to modules as variables

They only output after `apply`, not after a `plan`.

![output result](/Journey/061/output-result.png)

If you now run `terraform output` the variable and value will be returned to the terminal.

Let's find out what that public ip address is!

In the relevant module (in this case, the one for the EC2 instance, `devInstances.tf`). We have to go there because that's where the resource we want the output for is being created:

```hcl
output "test_var" {
  value = aws_instance.developmentServer[*].private_ip
}
```

In the root (i.e. `infrastructure.tf`) add this to the file:

```hcl
output "real_test" {
  value = module.devInstances.test_var
}
```

![output ip](//Journey/061/output-ip.png)

## ☁️ Cloud Outcome

- Lots of practice writing variables and learning the order of precedence they're applied in: `.tfvar` > `.tf` > `default`
- We learned about modules - isolating the code from the root file, how to create and call them
- Output variables - how to extract values and pass them around, how they only get applied after changes have taken effect, for debugging
- Terraform's Registry to find already existing modules.

## Next Steps

Part 4, of course!

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1349381862775545857?s=20)
