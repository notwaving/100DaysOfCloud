# Hoist: Terraform Tutorial Pt 1

## Introduction

Terraform is on my Tech Returners course, and I've just found [this](https://youtu.be/82gMxNob3Uc) introduction to it via Aaron Brooks of Mastermnd!

"Terraform" - to change the environment of a planet so that it is more like another planet; in sci-fi this usually means making it habitable for humans - like Earth.

"Hashicorp Terraform is a tool for building, changing and versioning infrastructure safely and efficiently. It can manage existing and popular service providers as well as custom in-house solutions."

We're using it in a cloud space, but it operates on other levels too. The reason why Terraform is so popular is because it's so flexible in terms of what it can be integrated with, i.e. it's cloud provider agnostic.

## Prerequisite

This shouldn't be hard if you've had prior experience with Infrastructure as Code... Did Aaron just say Terraform was like AWS CloudFormation?

## Use Case

DevOps, SREs, Cloud, Systems and Software Engineers frequently use Terraform in their jobs.

## Cloud Research

- Infrastructure as Code
- Pillars of IaS
- Build time vs runtime config

### Infrastructure as Code

This is much bigger than Terraform - IaC is driving much of the automation you can do today.

- IaC is the process of managing and provisioning data centres through machine-readable definition files, rather than physical hardware configuration or interactive configuration tools. Is this like the CloudFormation template we made with SAM? Or the `main.yml` config file required by GitHub Actions?

#### Why IaC?

- Creates consistent and reusable ways to deploy and manage infrastructure assets and configuration. Human error is a bit problem!

- Help prevent configuration drift (i.e. between different servers in a cluster)

- Free documentation (like a manifest/inventory)

- Speed - no manual setting or maintennance

- Automated infrastructure

### Pillars of IaC (not official, but it helps)

- Infra Provisioning
- Server/software configuration management (e.g. Ansible, Chef, Puppet).
  They manage the software on the infrastructure assets above - like servers. People generally provision the infrastructure first, and _then_ take the config management tool, and use that to apply the software or changes to the infrastructure you made. They're both independent of each other.
- Scripting
  Some scripting is a form of IaC. Scripts can be used to configure state, the way that something exists

### Build time vs run time config

Aaron prefers build-time config over runtime config. Run time config is adding server config management after the server has been built? Hashicorp's Packer can add all the config management at build time. Cuts down on deployment time? In the comments: "Build time is the best. If you are trying to scale quickly, you want an image that already has everything ready to go. Runtime - you're at the mercy of your upstream not having problems or in the event you didn't pin to specific versions, you can potentially be inconsistent, which is bad when scaling out to many machines.

### Hashicorp Configuration Language (HCL)

Terraform uses its own doman specific language to declare infrastructure assets and configurations. In terms of writing Terraform configuration, it looks a bit like JSON or YAML, but you can use expressions, loops etc too.

## ☁️ Cloud Outcome

- This was a nicely paced, well-explained introduction to Terraform and it's place in the DevOps tooling pantheon.
- Terraform is written in Go, compiles with Linux

## Next Steps

- Install Terraform
- Go through the official [Terraform tutorials for AWS](https://learn.hashicorp.com/collections/terraform/aws-get-started).

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1347993093161803780?s=20)
