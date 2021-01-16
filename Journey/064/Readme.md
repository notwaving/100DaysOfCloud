# Hoist: Terraform Tutorial Pt 5

## Introduction

We're nearly there! Going in deep on further Terraform features

## Prerequisite

- Understand everything in prior Hoist classes
- Docker, if following the additional tutorial

## Use Case

- You want to secure your configuration secrets
- You want to loop through a list to produce multiple resources of the same type, but make each unique in some way
- You want to parse information or make decisions based on info coming through as variables (either input or output)
- You're importing legacy Terraform config

## Cloud Research

- Secrets management
- Functions and dynamic templates
- Importing infrastructure

### Secrets Management

Secrets are any sensistive data that Terraform needs access to in order to deploy and configure infrastructure properly. These include things such as usernames, passwords and API keys.

- Don't store secrets in version control - or anywhere - in plain text
- Keep Terraform state safe

There are three main methods for handling secrets in Terraform. In order of effectiveness:

- Environment Variables (like `TF_VAR`s) - local only
  These aren't managed by Terraform, so could be confusing to any collaborators (because they can't access them)
- Encrypted files (i.e. security by obscurity). You can push these to source control, as no one has the keys. Except someone has to manage the keys...
  - KMS
  - PGP
- Secret Managers (overkill for small and personal projects)
  - AWS Secrets Manager - $0.40 per secret per month!!!!
  - Vault - also not free

AWS also offers KMS

### Functions and dynamic templates

Because Terraform is written in Go, "the only loops available are for loops"! Remember how you wanted to create multiple EC2 instances, and used the `count` parameter? It created those instances with a [for loop](https://blog.gruntwork.io/terraform-tips-tricks-loops-if-statements-and-gotchas-f739bbae55f9).

There are also [built in functions](https://www.terraform.io/docs/configuration/functions.html)

`terraform console` takes you to the interpreter, aka Terraform console, where you can test functions

- `max(1, 67, 7)` returns 67
- `split("", "hello")` returns `"h", "e", "l", "l", "o"`
  - `split(".", "www.academy.mastermnd.io")[2]` returns `mastermnd`

### Importing Infrastructure

Sometimes, things already exist. Which you're normally going to be provisioning new infrastructure, sometimes you'll need to handle old stuff.

We can import existing infrastructure.

- a single resource at a time
- doesn't import configuration, which means it alters the normal Terraform workflow
  - `apply`, then write config

## Try yourself

We're going to import infrastructure, using Docker! [Documentation](https://learn.hashicorp.com/tutorials/terraform/state-import)

Follow the tutorial in the link.

## ☁️ Cloud Outcome

Not sure I'd be regularly using much of this, especially importing. Learnt about the existence and power of functions. Discovering the different secrets management options was really useful too.

## Next Steps

Last part tomorrow!

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1350528148836937728?s=20)
